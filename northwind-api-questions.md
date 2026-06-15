Q: **Why do I need the @Column(name = "UnitsInStock") annotation...because the casing is different in the DB. Isn't sql case insensitive?**

Good question. SQL itself is case-insensitive for column names, and by default Hibernate is too — it lowercases everything before sending the query. So `unitsInStock` would become `unitsinstock` in the SQL, and MySQL would match it fine.

The real issue is the **camelCase-to-snake_case conversion** that Spring Boot enables by default. Hibernate's `SpringPhysicalNamingStrategy` automatically converts camelCase field names to snake_case column names:

- `unitsInStock` → `units_in_stock`
- `productName` → `product_name`
- `categoryId` → `category_id`

The Northwind database uses `UnitsInStock`, `ProductName`, `CategoryID` — no underscores. So without `@Column`, Hibernate looks for `units_in_stock` and finds nothing.

**You have two options:**

Option 1 — Keep `@Column` on the fields where the DB name doesn't match snake_case (what we did). Fine for a small model.

Option 2 — Switch to Hibernate's physical naming strategy in `application.properties`, which does no conversion at all and trusts your field names to match:

```properties
spring.jpa.hibernate.naming.physical-strategy=org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
```

With that setting, Hibernate sends the field name exactly as written. Since SQL is case-insensitive, `productName` matches `ProductName` in MySQL and you can drop all the `@Column` annotations.

For a teaching context, Option 1 is actually better to leave in — it makes the DB-to-Java mapping explicit and gives students something concrete to read. But Option 2 is worth knowing about.


---
**Q: Isn't an integer in a mysql database a different size than integer in Java. Are we sure Integer is correct? It shouldn't be long for the Id columns?**

Good instinct to question it, but in this case `Integer` is actually correct. Here's why:

**MySQL `INT`** is a 32-bit signed integer, range roughly -2.1 billion to +2.1 billion. **Java `Integer`** is also 32-bit signed, same range. They match exactly.

**Java `Long`** is 64-bit, which maps to MySQL's **`BIGINT`**. That would technically work (Hibernate will widen it), but it's semantically wrong — you're saying the column can hold 64-bit values when it can't.

The Northwind schema defines:
```sql
ProductID INT NOT NULL AUTO_INCREMENT
```

So `Integer` is the precise match. You'd use `Long` if the column were `BIGINT`, which is common in large-scale production systems where you might exhaust the 2 billion row limit — but Northwind's `INT` columns don't need it.

The conferenceapp actually uses `Long` for its `id` field, which is a very common habit — many developers just default to `Long` for all IDs. It works, but it's not strictly accurate unless the DB column is `BIGINT`. For a teaching codebase where you want the Java types to honestly reflect the database schema, `Integer` for `INT` is the right call.


---

