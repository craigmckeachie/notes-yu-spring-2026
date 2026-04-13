# Week 1

## Create a New Project

1. In the welcome window, choose "New Project" near the top
2. In the new project window, choose the following options:
   - Name: enter the name of your project
     > the name should be kebab-case (lowercase with dashes between words) Ex shopping-cart
   - Location: click the folder icon and navigate to your Pluralsight folder and choose it
   - Check the box: Create Git Repository (this will create a local git repository on your computer)
   - Build System: Maven
   - JDK: (first time) Click in the box to open the drop down > Choose Download JDK > Version 17 > Vendor Amazon Correto 17 the click Select
   - JDK: (every subsequent time) Choose Amazon Correto 17
   - Uncheck: Add sample code
   - Expand "Advanced Settings"
     - GroupId: com.pluralsight
3. Click the "Create" button
4. Got to the `\src\main\java` directory right-click and choose **Create Package**
5. Name the package `com.pluralsight`
6. Right-click on the package you just created and choose **New** > **Java class**
   - Be careful not to add the .java file extension, it will be added for you

## Commit Your Code

1. Click the "-o-" (commit) icon in the left menu
2. Click the checkbox next to changes to choose all files
3. Click in the large textbox on the bottom left
4. Enter a commit message, for example "Initial commit"
   > Write messages as if you are giving the codebase an instruction
   > "If applied, this commit will..."
5. Click the "Commit" button if this is the first time your are committing for the project
6. Click the "Commit & Push" button if this is a subsequent commit (2nd time +)
   > Note that if you commit and then need to push you can go to the Git Menu and choose> Push


## Share on Github

1. In the file menu at the top, Choose Git > Github > Share project on Github
2. In the window, uncheck "private" (we want our repositories to be public)




## Naming Conventions

- project-name (shopping-app) lowercase with dashes between words (kebab-case)
- package.name (com.pluralsight) (reverse dns naming)
- ClassName (ShoppingList) PascalCasing
- commit message (initial commit) lowercase
- repository-name (shopping-app) lowercase with dashes between words (kebab-case)
- localVariable (item) camelCasing
