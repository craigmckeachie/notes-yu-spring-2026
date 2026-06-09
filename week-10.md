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



---

