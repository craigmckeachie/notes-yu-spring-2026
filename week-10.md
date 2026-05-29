# Workbook 8a: Getting to Know Spring Boot

## Steps to Setup Demo Projects
- create `pluralsight/workbook-8/demo` directory
- unzip `demo-frontend` and `demo-backend` inside `demo`


## Steps to Setup Backend
- in workbench run `demo-backend/setup.sql`  (this step creates the database `internships` but not the tables or inserting data into tables)
  - you will need to click the refresh icon in the schema view to see the database
- in IntelliJ open the `demo-backend` project
- in IntelliJ open `demo-backend/src/main/resources/application.properties`
  - set password `spring.datasource.password=yourpassword`  to mysql root password for your computer
- in IntelliJ in the `demo-backend` run the `main` method in the application class (this step will create two tables)
- in workbench verify two tables were created in the `internships` database
- in workbench modify `demo-backend/setup.sql` .. uncomment the insert statements to seed data into the tables
- in workbench run `demo-backend/setup.sql` (this inserts data into the two tables)
- in a web browser visit http://localhost:8080/api/internships and verify data is returned from the API

