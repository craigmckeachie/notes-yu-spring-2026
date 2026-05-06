# Week 5


## Module 1 Objects

### Specialization

- What does it mean to specialize?

### 2 Types responsibilities

- What are the 2 types of responsibilities an object can have?
  - Know, Brain, Store, Nouns (facts, data)  = fields, getters and setters 
  - Do, Muscle, Perform, Verbs (actions) = methods


- Person
  - Know
    - name
    - age
    - eye color
    - blood type
  - Do
    - breathe
    - blink
    - read
    - run
    - eat
    - combHair
    - read

### Exercise 1 Objects

- Guest
- Room
- Employee
  - Clerk
- Receipt
- Restaurant
- Meal
- Department
  - FrontOffice
  - Housekeeping
- Request
- RoomKey?
- Phone?



### Exercise 2 Responsibilities

- Guest
  - Know (fields/getters)
    - Credit Card
    - Drivers License
    - check-in date
    - checkout date
    - Assigned Room (Room)
  - Do (methods)
    - check in 
- Reservation
- Drivers License (Identification)
- Credit Card
  - Know
    - number
    - name
    - CVC code
    - expiration date
- Room
  - Know
    - number
  - Do
- Employee (Clerk)
  - Know
  - Do
- Receipt
  - Know
  - Do
- Restaurant
  - Know
  - Do
- Meal
  - Know
  - Do
- Department (Front Office, Housekeeping, Maintenance)
  - Know
  - Do
- Request
  - Know
  - Do

## Module 2 Java Classes: A Review

### Terminology

− encapsulation
  - class with private fields (attributes)
  - housing
  - data holder
- class
  - blueprint, cookie cutter
− object
  - building/house, cookie
- constructor
  - builder or construction men
  - method used to do create an object instance from the class 
  - commonly used to initialize attributes
- instantiate an object
  - create an object instance from the class (new ClassName())
− call a method
- run a method
  - take an action
  - done by typing the name of the method followed by parenthesis
- overload a method
   - same method name, different parameters


### Methods
- What does high cohesion mean?
  - related information and actions
  - connected
  - organize related things
- Three types of methods
  - constructors
  - getters and setters
  - other methods
- 
### Method Overloading

- How do you determine a method signature?
  - name of each method + type of each parameter
- Why Overload?
  - single method name can handle many scenarios
- Why Overload Constructors
  - many people, many different situations

## Module 3: Unit Testing





##  The Method-Centric Style (Roy Osherove)
Common in large enterprise codebases, this style keeps tests organized by the method they are testing. This is helpful for developers who want to see all tests for a specific function grouped together in their IDE.

*   **Pattern:** `MethodName_StateUnderTest_ExpectedBehavior`

| **Example** | **Pros/Cons** |
| :--- | :--- |
| `calculateTax_InvalidIncome_ThrowsException` | **Pro:** Very easy to find tests for a specific method. |
| `withdraw_InsufficientFunds_ReturnsError` | **Con:** If you rename the method, you have to rename the test. |
---


- "MethodName_StateUnderTest_ExpectedBehavior"
  - IsAvailable_UnoccupiedClean_ReturnsTrue
  - IsAvailable_OccupiedDirty_ReturnsFalse

### Quiz
1. What are the 3 types of testing in the testing pyramid? 
   - Which type of tests should be done more? Why? 
   - Which type of tests should you be doing less? Why?
2. What is the name of testing framework we will use to test our code in class? 
   - What version of JUnit will we use?
3. In what folder in a Maven project do you write your tests?
4. If you want to generate a test you right click on what to create a test?
   - a) the test folder
   - b) the package folder you created inside the test folder
   - c) Code-> New Test
   - d) you right click in the file you are writing the test for and choose Generate > Test
5. How do you add JUnit to your IntelliJ project?
6. What are the three main sections of a test?
7. What are some common patterns for naming unit tests in Java?

