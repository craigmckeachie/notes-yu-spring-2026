# Workbook 8a: Getting to Know Spring Boot


# Exercise 1 Steps

## Steps to Setup Demo Projects
- create `pluralsight/workbook-8/demo` directory
- unzip `demo-frontend` and `demo-backend` inside `demo`
  > when you unzip it is very possible you will get a duplicate directory `demo-frontend\demo-frontend\src` and `demo-backend\demo-backend\src`...remove the duplicate directory before proceeding (Cutting the inner directory Ctrl+x and and moving into the demo directory and pasting is an easy way to fix this)
  Or you could just be careful when unzipping to not unzip into `demo\demo-backend` but just `demo`


## Steps to Setup Backend
- in workbench, run `demo-backend/setup.sql`  (this step creates the database `internships` but not the tables or inserting data into tables)
  - you will need to click the refresh icon in the schema view to see the database
- in IntelliJ, open the `demo-backend` project
- in IntelliJ, open `demo-backend/src/main/resources/application.properties`
  - set password `spring.datasource.password=yourpassword`  to mysql root password for your computer
- in IntelliJ, in the `demo-backend` run the `main` method in the application class (this step will create two tables)
- in workbench, verify two tables were created in the `internships` database
- in a web browser, visit http://localhost:8080/api/internships and it should return an empty array `[]` because there is no data in the database tables at this point
- in workbench, modify `demo-backend/setup.sql` .. uncomment the insert statements to seed data into the tables
- in workbench, run `demo-backend/setup.sql` (this inserts data into the two tables)
- in a web browser, visit http://localhost:8080/api/internships and verify data is returned from the API

---
- [Exercise: Changing an endpoint: Step 2 page 62 disclaimer](https://gemini.google.com/share/70c8b1e8d67a)
- 



---
Here is a quick TLDR and a couple of non-technical analogies you can use to explain this easily.

---


# REST

- [REST Cheatsheet](https://codewithcraig.netlify.app/reference/rest/)


# Spring Annotations

## The TLDR

Spring annotations are like **smart sticky notes** you slap onto regular Java code.

* Without them, your code is just a private library of Java classes.
* With them, Spring reads the sticky notes at startup and automatically wires your code to the internet, translates incoming JSON data into Java objects, and turns your return values back into JSON. It handles all the messy plumbing so you don't have to write hundreds of lines of servlet code.

---

## Analogy 1: The Drive-Thru Restaurant (Process-Oriented)

Think of your Spring Web application as a fast-food drive-thru.

* **`@RestController` (The Restaurant Sign & Menu):** This tells the outside world, *"Hey, this building is an active drive-thru, not a private house. Come here to get data."*
* **`@GetMapping` / `@PostMapping` (The Intercom/Drive-Thru Lanes):** These are the specific windows or lanes. A `@GetMapping("/burgers")` is the specific lane where you order burgers. If you try to order a burger at the car wash lane, it won't work.
* **`@RequestBody` (The Order Box/Baggage):** When a customer hands a raw, messy bag of ingredients or a custom order sheet over to the cashier, that’s the raw HTTP request. Spring acting as the cashier automatically unpacks it and places it neatly on the kitchen counter in a format the chef (your Java method) understands.
* **`@ResponseBody` (The To-Go Bag):** When the chef finishes cooking a meal (a Java Object), Spring automatically wraps it in a neat, standardized "To-Go Bag" (JSON) and hands it back out the window to the customer.

---

## Analogy 2: The International Shipping Port (Data-Oriented)

If you want to focus on how data changes shape:

Imagine your Java application is an island country that *only* speaks Java, but it wants to trade with the global internet, which *only* ships goods in standardized shipping containers (JSON).

* **`@RestController`** is the **designated shipping port**. It authorizes the island to do international business.
* **`@RequestBody`** is the **Unloading Crane**. It takes a standard container (JSON) off the ship, cracks it open, and unpacks the goods into local island trucks (Java POJOs) so your local workers (methods) can use them.
* **`@ResponseBody`** is the **Loading Crane**. When your workers finish manufacturing a product (a Java return object), the crane automatically packs it into a standard shipping container (JSON) so the foreign ship can carry it away.

**The takeaway:** The annotations are instructions for Spring to act as the ultimate customs agent, translator, and traffic controller.
