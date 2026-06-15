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