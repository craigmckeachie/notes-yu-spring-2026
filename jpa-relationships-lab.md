# JPA Relationships
## Associations, JPQL, and the N+1 Problem
*Northwind API — Lab Extension*

---

## 1. The Problem: Two Tables, One Screen

Your Northwind Products API currently returns JSON like this:

```json
{
  "productId": 1,
  "productName": "Chai",
  "categoryId": 1,
  "unitPrice": 18.00
}
```

The category ID is there, but not the category name. If you want to display "Beverages" instead of "1" on a product list screen, you need to reach into the `Categories` table. The question is: how do you do that cleanly in Spring Boot without writing raw SQL and without creating performance problems?

This lab introduces **JPA associations** — the mechanism that lets you navigate from one entity to a related entity — and explains how to use them correctly.

---

## 2. Associations (Navigable Properties)

In Java/JPA, an **association** (sometimes called a navigable property or relationship property) is a field on an entity class that holds a reference to another entity, instead of just a primitive value like an int or a String.

Right now your `Product` class has:

```java
private Integer categoryId;   // just a number
```

An association would replace or accompany that with:

```java
private Category category;    // the actual Category object
```

With the association in place you can write `product.getCategory().getCategoryName()` directly in your code, without writing any additional queries.

### 2.1 The Category Entity

Before you can add a relationship to `Product`, you need a `Category` entity that maps to the `Categories` table:

```java
@Entity
@Table(name = "Categories")
public class Category {

    @Id
    @Column(name = "CategoryID")
    private Integer categoryId;

    @Column(name = "CategoryName")
    private String categoryName;

    @Column(name = "Description")
    private String description;

    // getters and setters
}
```

### 2.2 Adding the Association to Product

Add these two things to your `Product` class:

```java
@ManyToOne(fetch = FetchType.LAZY)
@JoinColumn(name = "CategoryID", insertable = false, updatable = false)
private Category category;

// getter and setter
```

There are four things happening here:

| Annotation | What it does |
|---|---|
| `@ManyToOne` | Declares the relationship type. Many products can belong to one category. |
| `fetch = FetchType.LAZY` | Tells JPA not to load the Category automatically — only load it when `getCategory()` is actually called. |
| `@JoinColumn(name = "CategoryID")` | Tells JPA which column in the Products table is the foreign key for this join. |
| `insertable = false, updatable = false` | Prevents a conflict — `CategoryID` is already mapped as a plain integer field. Without this, Hibernate would complain that two fields manage the same column. |

---

## 3. Reading Relationship Annotations: "Me to Them"

JPA has four relationship annotations: `@OneToOne`, `@OneToMany`, `@ManyToOne`, and `@ManyToMany`. The two words in each annotation always describe the relationship from the perspective of the entity you are currently in, reading left-to-right as **"me to them"**.

To figure out which annotation to use, ask two questions:

- How many of **me** can relate to one of them?
- How many of **them** can relate to one of me?

### 3.1 Product → Category

Standing inside the `Product` entity:

- Can one product belong to multiple categories? **No** — one product has one category.
- Can one category have multiple products? **Yes** — Beverages contains Chai, Chang, and many others.

So from `Product`'s perspective: **many** (products) relate to **one** (category). That's `@ManyToOne`.

### 3.2 Category → Product (the other side)

Standing inside the `Category` entity:

- Can one category relate to multiple products? **Yes.**
- Can one product belong to multiple categories? **No.**

So from `Category`'s perspective: **one** (category) relates to **many** (products). That's `@OneToMany`.

> **Key insight:** `@ManyToOne` and `@OneToMany` are always two sides of the same relationship — the words always mirror each other. If one side is `@ManyToOne` the other side must be `@OneToMany`. If you ever find yourself writing `@ManyToOne` on both sides, something is wrong.

Note that you do not have to map both sides. In this lab you only need to navigate from `Product` to `Category`. You don't need to add a `List<Product>` on `Category` unless you actually need to navigate in that direction.

---

## 4. Lazy vs Eager Loading

When JPA loads a `Product` from the database, should it also immediately load the associated `Category`? That's the choice between lazy and eager loading.

| | Lazy (default for `@ManyToOne`) | Eager |
|---|---|---|
| **When category loads** | Only when you call `getCategory()` | Automatically every time a Product loads |
| **Best for** | Screens that don't always need the category | Sounds convenient — but see the N+1 warning below |

The recommendation for most production code is: **default to lazy for everything**, and load eagerly only when you explicitly need it using a fetch join (covered in Section 6).

---

## 5. The N+1 Problem

The N+1 problem is one of the most common performance mistakes in JPA applications. It happens when you load a list of entities and then access a lazy relationship on each one, causing one extra database query per entity.

### 5.1 How it happens

Imagine this service method that loads all products and prints the category name of each one:

```java
public List<Product> getAllProducts() {
    List<Product> products = productRepository.findAll();  // Query 1: loads all 77 products

    for (Product p : products) {
        System.out.println(p.getCategory().getCategoryName());  // Query 2, 3, 4 ... 78
    }

    return products;
}
```

The call to `p.getCategory()` triggers a lazy load — but it's inside a loop. Northwind has 77 products, so this code fires **78 database queries**: one to get all products, then one per product to get its category. On a larger dataset this becomes catastrophic.

**N+1 formula:**
- 1 query to load N entities
- \+ N queries to load the relationship on each entity
- = N+1 total queries

For 77 products: 1 + 77 = **78 queries** instead of 1.

### 5.2 Why eager loading doesn't fix it

You might think switching to `FetchType.EAGER` would help — it does not. Eager loading fires a separate query for every entity loaded, which produces the same N+1 situation. It just moves the problem from when you call `getCategory()` to when Hibernate loads each `Product`.

The real fix is to tell the database to do the join in a single query using JPQL.

---

## 6. JPQL and the Fetch Join

### 6.1 What is JPQL?

JPQL stands for **Java Persistence Query Language**. It is the query language defined by the **JPA specification** — a Java EE / Jakarta EE standard that defines how Java applications should interact with relational databases.

To understand where JPQL fits, here is the full stack:

```
Your Code
    ↓
Spring Data JPA       (Spring's convenience layer — JpaRepository, @Query)
    ↓
JPA Specification     (the standard / contract — defines JPQL)
    ↓
Hibernate             (the actual implementation that runs the queries)
    ↓
JDBC
    ↓
MySQL
```

**JPA** is the specification — a set of interfaces and rules with no executable code of its own.

**Hibernate** is the most popular implementation of the JPA spec. When your Spring Boot app runs, Hibernate is doing the actual work: translating JPQL into SQL, managing database sessions, and populating your entity objects.

**Spring Data JPA** sits on top and gives you the repository pattern — `JpaRepository`, derived query methods like `findByCategoryId`, and the `@Query` annotation where you write JPQL.

**JPQL vs SQL:**

```
SQL operates on tables and columns:
  SELECT * FROM Products p JOIN Categories c ON p.CategoryID = c.CategoryID

JPQL operates on entities and fields:
  SELECT p FROM Product p JOIN p.category
```

In JPQL you reference the Java entity name (`Product`, not `Products`) and the Java field name (`p.category`, not `CategoryID`). Hibernate translates this into the correct SQL using your `@Entity` and `@JoinColumn` annotations.

### 6.2 Regular JOIN vs JOIN FETCH

JPQL has two forms of join, and understanding the difference is the key to solving the N+1 problem.

| | `JOIN` | `JOIN FETCH` |
|---|---|---|
| **What it does** | Performs a SQL join so you can filter or sort by the related table's columns — but does NOT load the related entity into memory. | Performs the SQL join AND tells Hibernate to populate the relationship field on the entity. Calling `getCategory()` afterward reads from memory — no extra query. |
| **Example** | `SELECT p FROM Product p JOIN p.category WHERE p.categoryId = 1` | `SELECT p FROM Product p JOIN FETCH p.category` |

### 6.3 LEFT JOIN FETCH

A regular `JOIN FETCH` only returns products that have a matching category. If any product has a null `categoryId`, it would be silently excluded. `LEFT JOIN FETCH` keeps all products and simply leaves `category` as `null` for any product without one:

```java
SELECT p FROM Product p LEFT JOIN FETCH p.category
```

As a general rule, prefer `LEFT JOIN FETCH` to avoid silently dropping data.

### 6.4 The Complete Solution

Here is the full, correct pattern for loading products with their categories in a single query.

**Repository — fetch join query:**

```java
@Query("SELECT p FROM Product p LEFT JOIN FETCH p.category")
List<Product> findAllWithCategory();
```

**Service — calls the fetch join method:**

```java
public List<Product> getAllProducts() {
    return productRepository.findAllWithCategory();
}
```

**What happens in the database — one SQL query, one round trip:**

```sql
SELECT p.ProductID, p.ProductName, p.UnitPrice, ...,
       c.CategoryID, c.CategoryName, c.Description
FROM Products p
LEFT JOIN Categories c ON p.CategoryID = c.CategoryID
```

Hibernate loads all products and all their categories in one query. Each `Product` object comes back with its `category` field already populated — calling `getCategory().getCategoryName()` reads from memory and triggers no additional queries.

Because `Category` is now part of the `Product` object, Spring will automatically include it in the JSON response:

```json
{
  "productId": 1,
  "productName": "Chai",
  "categoryId": 1,
  "category": {
    "categoryId": 1,
    "categoryName": "Beverages",
    "description": "Soft drinks, coffees, teas, beers, and ales"
  },
  "unitPrice": 18.00
}
```

---

## 7. Exercise

Complete the tasks in order — each one builds on the previous. Make sure your northwind-api runs and `GET /api/products` returns data before you begin.

### Task 1 — Create the Category entity

Create `Category.java` in the model package. It should map to the `Categories` table.

Run `DESCRIBE Categories;` in MySQL to see the columns. Map at least `CategoryID`, `CategoryName`, and `Description`. Follow the same `@Entity`, `@Table`, `@Id`, and `@Column` pattern as `Product.java`.

**Done when:** `Category.java` compiles and the application still starts without errors.

---

### Task 2 — Add the association to Product

Add a `category` field to `Product.java` with the correct JPA annotations. Refer to Section 2.2 for the exact annotations and what each one does. Add a getter and setter.

**Done when:** `Product.java` compiles and `GET /api/products` still returns data.

---

### Task 3 — Add the fetch join query to the repository

Add a new method to `ProductRepository.java` using `@Query` with `LEFT JOIN FETCH` to load products and their categories in a single query. Refer to Section 6.4 for the syntax.

**Done when:** The interface compiles.

---

### Task 4 — Update the service method

Update the `getAllProducts()` method in `ProductService.java` to call your new fetch join repository method instead of `findAll()`.

**Done when:** `ProductService.java` compiles.

---

### Task 5 — Test the endpoint

Call `GET /api/products` and check the response. Each product should now include a nested `category` object containing the `categoryName`.

**Done when:** The JSON response includes a `category` object on each product with a readable `categoryName`.

---

### Task 6 — Observe the query count

Make sure `spring.jpa.show-sql=true` is in your `application.properties`. Call `GET /api/products` and watch the console. Count the SQL statements.

- One SELECT with a JOIN — your fetch join is working correctly.
- Many SELECT statements — the N+1 problem is occurring. Revisit your repository query and service method.

> **Discussion question:** What would happen if you switched back to `findAll()` in your service but left the `category` association on `Product`? How many queries would you see in the console for 77 products, and why?

---

### Checklist

- [ ] `Category.java` exists in the model package with correct JPA annotations
- [ ] `Product.java` has a `category` field with `@ManyToOne`, `@JoinColumn`, and `FetchType.LAZY`
- [ ] `ProductRepository` has a `findAllWithCategory()` method using `LEFT JOIN FETCH`
- [ ] `ProductService.getAllProducts()` calls `findAllWithCategory()` instead of `findAll()`
- [ ] `GET /api/products` returns products with a nested `category` object in the JSON
- [ ] The console shows exactly one SQL query when the endpoint is called
