# Building a REST API with Spring Boot
## Layers, Annotations, and How It All Fits Together
*Northwind API — Introduction*

---

## 1. The Big Picture

When you build a REST API in Spring Boot, you are not writing one big program. You are writing four distinct layers, each with a single responsibility, that work together to handle a request.

Here is what happens when a client calls `GET /api/products/1`:

```
REQUEST                                      RESPONSE

Client                              →   JSON sent back to client
  ↓                                             ↑
Controller  — reads the URL, calls service      |  serializes result to JSON
  ↓                                             ↑
Service     — calls the repository              |  returns data to controller
  ↓                                             ↑
Repository  — queries the database              |  returns data to service
  ↓                                             ↑
Database (MySQL)  ——— executes query, returns rows ———
```

Each layer only talks to the one directly below it. The controller never touches the database. The repository never knows anything about HTTP. This separation keeps the code organized, testable, and easy to change.

In this lab you will build all four layers for the `Products` table, one at a time, bottom to top.

### Before you write any code — application.properties

Before the application can do anything useful it needs to know how to connect to the database. Spring Boot reads all configuration from a single file:

**`src/main/resources/application.properties`**

This is the first file you will configure in the lab. At minimum it needs these properties:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/northwind
spring.datasource.username=root
spring.datasource.password=yourpassword
```

Two additional properties are important to set from the start:

```properties
# Prevents JPA from trying to create or modify your tables on startup
spring.jpa.hibernate.ddl-auto=none

# Prints the SQL Hibernate generates to the console — very useful for debugging
spring.jpa.show-sql=true
```

`ddl-auto=none` is particularly important. Without it, JPA may attempt to modify your database schema when the application starts. You will see references to these properties again as you work through each layer.

---

## 2. The Model — Mapping a Table to a Java Class

Before you can read data from the database, you need to tell JPA what the table looks like. You do this by creating a **model class** — a plain Java class annotated so that JPA knows how to map it to a database table.

This kind of class is called an **entity**.

```java
@Entity
@Table(name = "Products")
public class Product {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "ProductID")
    private Integer productId;

    @Column(name = "ProductName")
    private String productName;

    @Column(name = "UnitPrice")
    private BigDecimal unitPrice;

    // getters and setters
}
```

### What each annotation does

| Annotation | Where it goes | What it does |
|---|---|---|
| `@Entity` | Class | Tells JPA this class maps to a database table |
| `@Table(name = "...")` | Class | Specifies the exact table name in the database |
| `@Id` | Field | Marks this field as the primary key |
| `@GeneratedValue(strategy = GenerationType.IDENTITY)` | Field | Tells JPA the database generates the ID automatically (AUTO_INCREMENT) |
| `@Column(name = "...")` | Field | Maps the field to a specific column name in the table |

### Why @Column is needed here

By default, Spring Boot converts camelCase Java field names to snake_case column names — so `productName` would look for a column called `product_name`. The Northwind database uses PascalCase column names like `ProductName`, so you need `@Column(name = "ProductName")` to tell JPA the exact column name to use.

> If your application starts but every query fails with an "unknown column" error, this is almost always the cause. Check your `spring.jpa.show-sql=true` output — if the generated SQL uses snake_case column names that don't exist in your table, you are missing `@Column` annotations.

### Java types and database types

The Java type of each field should match the database column type:

| MySQL type | Java type |
|---|---|
| `INT` | `Integer` |
| `VARCHAR`, `CHAR`, `TEXT` | `String` |
| `DECIMAL`, `MONEY` | `BigDecimal` |
| `DATE` | `LocalDate` |
| `DATETIME`, `TIMESTAMP` | `LocalDateTime` |
| `BIT(1)`, `TINYINT(1)` | `Boolean` |

Every field needs a getter and a setter. Spring uses them to serialize the object to JSON when sending a response.

---

## 3. The Repository — Free Database Operations

Once you have a model, you need a way to talk to the database. In Spring Data JPA, you get this almost for free by creating an **interface** that extends `JpaRepository`.

```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Integer> {
}
```

The two type parameters tell Spring what this repository manages:
- `Product` — the entity class
- `Integer` — the type of the primary key

Just by extending `JpaRepository`, you immediately have access to methods like:

| Method | What it does |
|---|---|
| `findAll()` | Returns all rows |
| `findById(id)` | Returns one row by primary key, wrapped in `Optional` |
| `save(entity)` | Inserts or updates a row — used for both POST and PUT |
| `deleteById(id)` | Deletes a row by primary key |
| `existsById(id)` | Returns true if a row with that ID exists |

You do not write any SQL for these. Spring generates the queries automatically. With `spring.jpa.show-sql=true` in your `application.properties` you can see exactly what SQL gets generated for each call.

### Derived query methods

Spring Data JPA can also generate queries from method names. You declare the method signature and Spring figures out the SQL.

```java
List<Product> findByCategoryId(Integer categoryId);

List<Product> findByProductNameContainingIgnoreCase(String name);
```

The method name is parsed word by word:

- `findBy` — start a SELECT WHERE
- `CategoryId` — the field to filter on (must match the Java field name exactly)
- `Containing` — use a `LIKE '%value%'` match
- `IgnoreCase` — make the comparison case-insensitive

You never write the SQL — Spring reads the method name and generates it. The method just needs to exist in the interface.

---

## 4. The Service — Business Logic and the Constructor Pattern

The service sits between the controller and the repository. Its job is to contain business logic and coordinate calls to the repository. Even when there is no complex logic, the service layer is still important because it keeps the controller thin and makes the code easier to test and change later.

```java
@Service
public class ProductService {

    private final ProductRepository productRepository;

    public ProductService(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    public List<Product> getAllProducts() {
        return productRepository.findAll();
    }

    public Optional<Product> getProductById(Integer id) {
        return productRepository.findById(id);
    }

    public Product createProduct(Product product) {
        return productRepository.save(product);
    }

    public ResponseEntity<Product> updateProduct(Integer id, Product product) {
        if (!productRepository.existsById(id)) {
            return ResponseEntity.notFound().build();
        }
        product.setProductId(id);
        Product updated = productRepository.save(product);
        return ResponseEntity.ok(updated);
    }

    public void deleteProduct(Integer id) {
        productRepository.deleteById(id);
    }
}
```

### @Service

This annotation tells Spring that this class is a service component. Spring will create one instance of it and make it available for injection elsewhere.

### Constructor injection

Notice that the repository is not assigned with `@Autowired` on a field. Instead, it is passed in through the constructor. This is called **constructor injection** and it is the preferred approach in modern Spring Boot because:

- The dependency is declared as `final` — it cannot be accidentally reassigned
- The class is easier to test — you can pass a mock repository in a unit test without involving Spring at all
- The dependencies are explicit — you can see exactly what the class needs just by reading the constructor

Spring sees a constructor that takes a `ProductRepository` and automatically provides one — you do not need to annotate the constructor with anything.

### Optional

`findById` returns `Optional<Product>` rather than `Product` directly. An `Optional` is a container that either holds a value or is empty — it forces the caller to handle the case where the product does not exist, rather than returning `null` and risking a `NullPointerException`. The controller will use this to decide whether to send back the product or a 404 response.

### How save() handles both insert and update

Notice that `createProduct` and `updateProduct` both call `productRepository.save()`. JPA uses the ID to decide what to do:

- If the entity has no ID (null) — JPA runs an `INSERT`
- If the entity has an ID that exists in the database — JPA runs an `UPDATE`

In `updateProduct`, the ID from the URL path is set on the product object with `product.setProductId(id)` before calling `save()`, which ensures JPA updates the correct row rather than inserting a new one. The `existsById` check beforehand ensures a 404 is returned if the ID doesn't exist, rather than silently inserting a new row.

---

## 5. The Controller — Handling HTTP Requests

The controller is the entry point for HTTP requests. It receives a request, calls the service, and sends back a response.

```java
@RestController
@RequestMapping("/api/products")
public class ProductController {

    private final ProductService productService;

    public ProductController(ProductService productService) {
        this.productService = productService;
    }

    @GetMapping
    public List<Product> getAllProducts() {
        return productService.getAllProducts();
    }

    @GetMapping("/{id}")
    public ResponseEntity<Product> getProductById(@PathVariable Integer id) {
        return productService.getProductById(id)
                .map(ResponseEntity::ok)
                .orElse(ResponseEntity.notFound().build());
    }

    @GetMapping("/search")
    public List<Product> searchProducts(@RequestParam String name) {
        return productService.searchProductsByName(name);
    }

    @PostMapping
    public Product createProduct(@RequestBody Product product) {
        return productService.createProduct(product);
    }

    @PutMapping("/{id}")
    public ResponseEntity<Product> updateProduct(@PathVariable Integer id,
                                                 @RequestBody Product product) {
        return productService.updateProduct(id, product);
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteProduct(@PathVariable Integer id) {
        productService.deleteProduct(id);
        return ResponseEntity.noContent().build();
    }
}
```

### The class-level annotations

`@RestController` combines two things: it marks the class as a Spring component and tells Spring to serialize every method's return value directly to JSON in the response body. Without this, you would have to annotate every method individually.

`@RequestMapping("/api/products")` sets the base path for every endpoint in this controller. All the method-level paths are appended to this.

### Mapping annotations

Each method is mapped to an HTTP method using an annotation:

| Annotation | HTTP method | Typical use |
|---|---|---|
| `@GetMapping` | GET | Read data |
| `@PostMapping` | POST | Create new data |
| `@PutMapping` | PUT | Replace existing data |
| `@DeleteMapping` | DELETE | Remove data |

Each mapping can optionally include a path: `@GetMapping("/{id}")` maps to `GET /api/products/{id}`.

### Reading data from the request — @PathVariable vs @RequestParam

Both annotations read input from the URL, but from different parts of it.

**`@PathVariable`** reads a value embedded in the URL path itself. The placeholder in the path and the parameter name must match:

```java
@GetMapping("/{id}")
public ResponseEntity<Product> getProductById(@PathVariable Integer id) { ... }
```

Called with: `GET /api/products/1` — Spring extracts `1` from the path and passes it as `id`.

**`@RequestParam`** reads a value from the query string — the part of the URL after the `?`:

```java
@GetMapping("/search")
public List<Product> searchProducts(@RequestParam String name) { ... }
```

Called with: `GET /api/products/search?name=chai` — Spring extracts `chai` from the query string and passes it as `name`.

### When to use which

The choice is about what the value represents:

| Use `@PathVariable` when... | Use `@RequestParam` when... |
|---|---|
| The value **identifies a specific resource** | The value **filters or modifies a collection** |
| `GET /api/products/1` — fetching product 1 | `GET /api/products/search?name=chai` — searching |
| `PUT /api/products/1` — updating product 1 | `GET /api/products?discontinued=true` — filtering |
| There is exactly one logical value | There may be multiple optional parameters |

A useful rule of thumb: if removing the value from the URL would make the path meaningless or point to a different resource, it belongs in the path. If it is optional context that narrows down a result set, it belongs in the query string.

`@RequestParam` can also be made optional with a default value:

```java
@GetMapping("/search")
public List<Product> searchProducts(@RequestParam(defaultValue = "") String name) { ... }
```

This allows `GET /api/products/search` to work even without a query string, returning all products instead of throwing a missing parameter error.

### @RequestBody

Used on POST and PUT endpoints to read the JSON body of the request and deserialize it into a Java object:

```java
@PutMapping("/{id}")
public ResponseEntity<Product> updateProduct(@PathVariable Integer id,
                                             @RequestBody Product product) { ... }
```

Spring reads the JSON sent in the request body and maps it to a `Product` object automatically, using the field names and getters/setters you defined on the model. Notice that `@PutMapping` uses both `@PathVariable` to identify which product to update and `@RequestBody` to receive the updated data — this is the standard pattern for update endpoints.

### ResponseEntity

Some endpoints need to control the HTTP status code as well as the body. `ResponseEntity` lets you do this.

For an endpoint that might not find what it's looking for:

```java
return productService.getProductById(id)
        .map(ResponseEntity::ok)                     // found → 200 OK with the product as the body
        .orElse(ResponseEntity.notFound().build());  // not found → 404 with no body
```

For an endpoint that returns nothing:

```java
return ResponseEntity.noContent().build();  // 204 No Content
```

Common status codes you will use:

| Status | Meaning | When to use |
|---|---|---|
| `200 OK` | Success with a body | Successful GET, successful POST, successful PUT |
| `204 No Content` | Success with no body | Successful DELETE |
| `404 Not Found` | Resource does not exist | GET, PUT, or DELETE for an ID that doesn't exist |

---

## 6. How Spring Wires It Together

You never call `new ProductService()` or `new ProductController()`. Spring creates all of these objects and connects them automatically. This is called **dependency injection**.

When the application starts, Spring scans the project for classes annotated with `@RestController`, `@Service`, `@Repository`, and `@Component`. It creates one instance of each and satisfies their constructor parameters by looking at what other instances it has created.

This means:
- Spring creates a `ProductRepository` (from the `@Repository` interface)
- Spring creates a `ProductService` and passes the repository into its constructor
- Spring creates a `ProductController` and passes the service into its constructor

You configure this once by creating the classes with the right annotations. Spring handles the rest.

---

## 7. Putting It Together — Requests End to End

### GET request — reading a single product

Here is what happens when a client sends `GET /api/products/1`:

```
1.  Spring receives the HTTP request
2.  It matches the path to @GetMapping("/{id}") on ProductController
3.  It extracts "1" from the path and passes it as id=1 (@PathVariable)
4.  It calls ProductController.getProductById(1)
5.  The controller calls productService.getProductById(1)
6.  The service calls productRepository.findById(1)
7.  JPA generates a SELECT statement and runs it against MySQL
8.  MySQL finds the row and returns it
9.  JPA maps the row to a Product object and wraps it in Optional.of(product)
10. The Optional travels back up through service → controller
11. The controller calls .map(ResponseEntity::ok) — the Optional has a value,
    so this produces a 200 response with the product as the body
12. Spring serializes the Product to JSON and sends it in the response

If no row exists for id=1:
9b.  JPA returns Optional.empty()
11b. The controller calls .orElse(ResponseEntity.notFound().build())
     — producing a 404 response with no body
```

### PUT request — updating a product

Here is what happens when a client sends `PUT /api/products/1` with a JSON body:

```
1.  Spring receives the HTTP request
2.  It matches the path to @PutMapping("/{id}") on ProductController
3.  It extracts "1" from the path and passes it as id=1 (@PathVariable)
4.  It deserializes the JSON body into a Product object (@RequestBody)
5.  It calls ProductController.updateProduct(1, product)
6.  The controller calls productService.updateProduct(1, product)
7.  The service calls productRepository.existsById(1)
8.  JPA runs a SELECT to check whether product 1 exists

If the product does not exist:
9b.  The service returns ResponseEntity.notFound().build() — a 404 with no body

If the product exists:
9.  The service calls product.setProductId(1) to ensure the correct row is targeted
10. The service calls productRepository.save(product)
11. JPA sees that the ID exists and generates an UPDATE statement
12. MySQL updates the row
13. The updated Product travels back up through service → controller
14. Spring serializes the updated Product to JSON and sends it with status 200
```

### POST request — creating a product

Here is what happens when a client sends `POST /api/products` with a JSON body:

```
1.  Spring receives the HTTP request
2.  It matches the path to @PostMapping on ProductController
3.  It deserializes the JSON body into a Product object (@RequestBody)
4.  It calls ProductController.createProduct(product)
5.  The controller calls productService.createProduct(product)
6.  The service calls productRepository.save(product)
7.  JPA sees no ID on the product and generates an INSERT statement
8.  MySQL inserts the row and returns the generated ID
9.  The saved Product (now with its new ID populated) travels back up
    through service → controller
10. Spring serializes the Product to JSON and sends it with status 200
```

Every lab task you complete adds one layer to this chain. When all five tasks are done, the full chain is in place and all of these requests can travel all the way through.

---

## Quick Reference — Annotations at a Glance

| Annotation | Package | Used on | Purpose |
|---|---|---|---|
| `@Entity` | JPA | Class | Marks a class as a database entity |
| `@Table` | JPA | Class | Specifies the table name |
| `@Id` | JPA | Field | Primary key |
| `@GeneratedValue` | JPA | Field | Auto-generated primary key |
| `@Column` | JPA | Field | Maps to a specific column name |
| `@Repository` | Spring | Interface | Marks as a repository component |
| `@Service` | Spring | Class | Marks as a service component |
| `@RestController` | Spring | Class | Marks as a REST controller; returns JSON |
| `@RequestMapping` | Spring | Class | Sets the base URL path |
| `@GetMapping` | Spring | Method | Maps GET requests |
| `@PostMapping` | Spring | Method | Maps POST requests |
| `@PutMapping` | Spring | Method | Maps PUT requests |
| `@DeleteMapping` | Spring | Method | Maps DELETE requests |
| `@PathVariable` | Spring | Parameter | Reads a value from the URL path |
| `@RequestParam` | Spring | Parameter | Reads a value from the query string |
| `@RequestBody` | Spring | Parameter | Reads and deserializes the request body |
