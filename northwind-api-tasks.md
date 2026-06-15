# Lab: Building the Northwind Products API

You have an empty Spring Boot project called **northwind-api** and access to the **northwind** MySQL database. Your job is to build a full REST API for the `Products` table — model, repository, service, and controller — following the same patterns used in the **conferenceapp** reference project.

> **How to use the reference app:** Open conferenceapp alongside your project. Every step below has a direct parallel in that codebase. Find it, understand it, then write your own version for Products.

---

## Before You Start

Take five minutes to explore the northwind database and understand the `Products` table:

```sql
USE northwind;
DESCRIBE Products;
SELECT * FROM Products LIMIT 5;
```

Note the column names and data types — you'll need them when building your model.

---

## Task 1 — Configure the Database Connection

**File:** `src/main/resources/application.properties`

The conferenceapp connects to a database called `sessions`. You need to connect to `northwind` instead. Find the datasource properties in the reference app and add equivalent ones to your project, pointing at the correct database name.

**You're done when:** Your application starts without a connection error.

---

## Task 2 — Create the Model

**File:** `src/main/java/com/yearup/northwindapi/model/Product.java`

The `Session` class in the reference app maps to the `sessions` table using JPA annotations. Create a `Product` class that maps to the `Products` table in northwind.

Things to figure out from the reference:
- What annotation marks this class as a JPA entity?
- What annotation tells JPA which table name to use?
- What annotation marks the primary key field?
- What annotation handles auto-generated IDs?

Map every column from the `Products` table as a field. Add getters and setters for all fields.

**You're done when:** The class compiles and the field names match the database columns.

---

## Task 3 — Create the Repository

**File:** `src/main/java/com/yearup/northwindapi/repository/ProductRepository.java`

The `SessionRepository` in the reference app is a simple interface that gives you database operations for free. Create a `ProductRepository` the same way.

Once the basic interface is in place, add **two** custom finder methods:
1. Find products by category ID
2. Find products whose name contains a search string (case-insensitive)

Look at how `findByTrackIgnoreCase` and `findBySpeakerContainingIgnoreCase` are written in the reference repository — Spring Data JPA derives the SQL from the method name automatically. Use the same naming pattern for your fields.

**You're done when:** The interface compiles. You don't need to write any SQL.

---

## Task 4 — Create the Service

**File:** `src/main/java/com/yearup/northwindapi/service/ProductService.java`

The `SessionService` sits between the controller and the repository. It's injected with the repository via the constructor (not a field annotation — notice how the reference does it).

Implement these methods:
- Get all products
- Get a single product by ID (returns `Optional<Product>`)
- Create a new product
- Delete a product by ID
- Get products by category ID
- Search products by name

Each method should delegate to the repository. The service itself should not contain any SQL or business logic beyond calling the right repository method.

**You're done when:** All six methods are implemented and the class compiles cleanly.

---

## Task 5 — Create the Controller

**File:** `src/main/java/com/yearup/northwindapi/controller/ProductController.java`

The `SessionController` in the reference app exposes the service as a REST API. Your controller should follow the same structure.

Map these endpoints:

| Method | Path | What it does |
|--------|------|--------------|
| GET | `/api/products` | Return all products |
| GET | `/api/products/{id}` | Return one product, or 404 if not found |
| GET | `/api/products/category/{categoryId}` | Return products in a category |
| GET | `/api/products/search/{name}` | Search products by name |
| POST | `/api/products` | Create a new product |
| DELETE | `/api/products/{id}` | Delete a product |

Pay attention to how the reference controller:
- Annotates the class vs. individual methods
- Uses `ResponseEntity` for endpoints that might return a 404
- Reads path variables from the URL
- Reads the request body for POST

**You're done when:** All six endpoints are defined and the class compiles.

---

## Task 6 — Test Your Endpoints

Run the application and test each endpoint. You can use a browser for GET requests and Insomnia (or `curl`) for POST and DELETE.

Suggested test order:
1. `GET /api/products` — should return all products from northwind
2. `GET /api/products/1` — should return the product with ID 1
3. `GET /api/products/99999` — should return a 404, not an error
4. `GET /api/products/category/1` — should return products in category 1
5. `GET /api/products/search/chai` — should find products with "chai" in the name
6. `POST /api/products` with a JSON body — should insert a new product
7. `DELETE /api/products/{id}` for the ID you just created — should remove it

**You're done when:** All seven tests produce the expected results.

---

## Checklist

- [ ] `application.properties` points to the `northwind` database
- [ ] `Product` model maps all columns with correct JPA annotations
- [ ] `ProductRepository` extends `JpaRepository` and has two custom finders
- [ ] `ProductService` has all six methods using constructor injection
- [ ] `ProductController` has all six endpoints with correct HTTP methods and paths
- [ ] Application starts without errors
- [ ] All endpoints return correct data when tested
