# Lab: Northwind Orders API

You have already built a full REST API for the `Products` table. Now you will do the same for the `Orders` table, and extend it with a customer association so you can display the customer's company name alongside each order.

Use your Products code as your reference throughout. Every pattern you need already exists there.

---

## Before You Start

Spend a few minutes exploring the relevant tables in the northwind database:

```sql
USE northwind;
DESCRIBE Orders;
DESCRIBE Customers;
SELECT * FROM Orders LIMIT 5;
SELECT * FROM Customers LIMIT 5;
```

Note the column names, data types, and the foreign key that links `Orders` to `Customers`. You will need all of this when building your model.

---

## Task 1 — Create the Model

**File:** `src/main/java/com/yearup/northwindapi/model/Order.java`

Map every column from the `Orders` table as a field. Pay attention to the data types — dates in the Northwind database should use `LocalDate` in Java.

> **Note:** `Order` is a reserved word in SQL and can conflict in some JPA configurations. Name your class `Order` but use `@Table(name = "Orders")` to be explicit, as you did with `Product`.

**Done when:** The class compiles with all columns mapped.

---

## Task 2 — Create the Repository

**File:** `src/main/java/com/yearup/northwindapi/repository/OrderRepository.java`

Create the repository interface the same way you did for `ProductRepository`. Once the basic interface is in place, add two custom finder methods:

1. Find orders by customer ID
2. Find orders by employee ID

**Done when:** The interface compiles. No SQL required.

---

## Task 3 — Create the Service

**File:** `src/main/java/com/yearup/northwindapi/service/OrderService.java`

Use constructor injection as you did in `ProductService`. Implement these methods:

- Get all orders
- Get a single order by ID
- Create a new order
- Update an existing order
- Delete an order by ID
- Get orders by customer ID
- Get orders by employee ID

**Done when:** All seven methods are implemented and the class compiles.

---

## Task 4 — Create the Controller

**File:** `src/main/java/com/yearup/northwindapi/controller/OrderController.java`

Map these endpoints following the same patterns as `ProductController`:

| Method | Path | What it does |
|--------|------|--------------|
| GET | `/api/orders` | Return all orders |
| GET | `/api/orders/{id}` | Return one order, or 404 if not found |
| GET | `/api/orders/customer/{customerId}` | Return orders for a customer |
| GET | `/api/orders/employee/{employeeId}` | Return orders for an employee |
| POST | `/api/orders` | Create a new order |
| PUT | `/api/orders/{id}` | Update an existing order |
| DELETE | `/api/orders/{id}` | Delete an order |

**Done when:** All seven endpoints are defined and the class compiles.

---

## Task 5 — Test Your Endpoints

Run the application and test each endpoint using Insomnia or a browser for GET requests.

Suggested test order:

1. `GET /api/orders` — should return all orders
2. `GET /api/orders/1` — should return the order with ID 1
3. `GET /api/orders/99999` — should return a 404, not an error
4. `GET /api/orders/customer/ALFKI` — should return orders for that customer
5. `GET /api/orders/employee/1` — should return orders for employee 1
6. `POST /api/orders` with a JSON body — should create a new order
7. `PUT /api/orders/{id}` for the order you just created — should update it
8. `DELETE /api/orders/{id}` for the same order — should remove it

**Done when:** All eight tests produce the expected results.

---

## Task 6 — Create the Customer Entity

**File:** `src/main/java/com/yearup/northwindapi/model/Customer.java`

Create a `Customer` entity that maps to the `Customers` table. Map at least:

- `CustomerID`
- `CompanyName`
- `ContactName`
- `City`
- `Country`

> **Note:** The `CustomerID` column in Northwind is a `CHAR(5)`, not an integer. The correct Java type for this field is `String`.

**Done when:** `Customer.java` compiles and the application still starts without errors.

---

## Task 7 — Add the Association to Order

Add a navigable `customer` property to your `Order` class so you can call `order.getCustomer().getCompanyName()` without writing any additional queries.

Refer to the JPA Relationships lab for the exact annotations needed and what each one does. Think carefully about:

- Which annotation describes the relationship from `Order`'s perspective
- Which column is the foreign key
- What fetch type to use and why

**Done when:** `Order.java` compiles and `GET /api/orders` still returns data.

---

## Task 8 — Add the Fetch Join Query to the Repository

Add a new method to `OrderRepository` that loads orders and their associated customers in a single query, with no N+1 problem.

Refer to the JPA Relationships lab for the fetch join syntax. Use `LEFT JOIN FETCH` so orders without a customer are not silently excluded.

**Done when:** The interface compiles.

---

## Task 9 — Update the Service

Update `getAllOrders()` in `OrderService` to call your new fetch join method instead of `findAll()`.

**Done when:** `OrderService.java` compiles.

---

## Task 10 — Test the Association

Call `GET /api/orders` and check the response. Each order should now include a nested `customer` object containing `companyName` and other customer fields.

Then check the console. Make sure `spring.jpa.show-sql=true` is in your `application.properties` and confirm that only one SQL statement is fired — a single SELECT with a JOIN.

**Done when:** The JSON includes a nested `customer` object and the console shows exactly one query.

---

## Checklist

- [ ] `Order.java` maps all columns from the `Orders` table with correct JPA annotations
- [ ] `OrderRepository` extends `JpaRepository` and has two custom finder methods
- [ ] `OrderService` has all seven methods using constructor injection
- [ ] `OrderController` has all seven endpoints with correct HTTP methods and paths
- [ ] All eight endpoint tests pass
- [ ] `Customer.java` exists in the model package with correct JPA annotations
- [ ] `Order.java` has a `customer` association with the correct relationship annotation, join column, and fetch type
- [ ] `OrderRepository` has a `findAllWithCustomer()` method using `LEFT JOIN FETCH`
- [ ] `OrderService.getAllOrders()` calls the fetch join method instead of `findAll()`
- [ ] `GET /api/orders` returns orders with a nested `customer` object in the JSON
- [ ] The console shows exactly one SQL query when `GET /api/orders` is called
