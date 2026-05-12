# Week 6

## Quiz

1. What does D.R.Y. mean?
2. Why do we need inheritance? Do all programming languages have inheritance?
3. What is the problem with putting everything in one class? 
4. Why do they call it inheritance?
5. What do we call the class we are inheriting from?
6. What do we call the class that is inheriting from another class?
7. Last week we learned about a has-a relationship. What is the inheritance relationship called?

1. What is the keyword that indicates one class inherits from another?
2. What gets inherited? 
    - data fields/members (attributes)?  
    - constructors?
    - getters and setters?
    - other methods?
3. What does the access modifier protected mean? How often should you use it on a data member? What is an alternative way to get to the data?


- Then Animal - Dog or Cat example



## Exercise 0


Create the Person and Student classes as shown below.


| Parent Class (General) | Shared Attribute | Child Class (Specific) | Unique Attribute |
|---|---|---|---|
| Animal | name | Cat | breed |
| Person | name | Student | studentID |

## Working with Constructors Quiz

1. Child class constructors automatically do what?
   - call the super class parameterless constructor
2. How would I call the super class parameterless constructor from a child class constructor?
   ```Java
    super();
   ```

- Java will do things for you with parameterless constructors
  - generate a parameterless constructor for you automatically 
    - as long as you haven't created one with parameters
  - call the super class parameterless constructor from any constructor you create in a child class
  - BUT, when you create a constructor with parameters in a child class you usually don't want to call the parameterless constructor in the super class...you want to call a constructor in the super class with parameters so it can store them for you
    - you want to leave it up to the super class to do stuff for you 







