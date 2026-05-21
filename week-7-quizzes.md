# Week 7 Quizzes

### Collections 

#### Questions
1. Describe a collection using one word?
2. What are two most common types of collections we have been using in class? Are there other types of collections?
3. Name some common scenarios of why you would loop through a collection.
   - find one
   - find many
   - aggregate (count, sum)
   - transform (change the shape)
   - do something for each
   - sort
4. What are the two basic states of data?
   - data at rest
   - data in transit
5. What does Java call or what are the objects Java uses to represent
   - data at rest
   - data in transit
6. What is the difference between a function and a method?
7. Is a lambda similar to a method? How are they alike?
8.  What do you pass as an argument into the parameter of a collection's forEach method?
9.  You get a stream of data by calling the "______" method on a "____"
10. The collect method takes the data in transit in a _____ and lays it to rest in a ______.
11. Match these
   - find one
   - find many
   - aggregate (count, sum)
   - transform (change the shape)
   - do something for each
   - sort
   -  "to one of these methods on a stream"
   - count()
   - forEach()
   - sorted()
   - filter()
   - map()
   - reduce()
  

#### Questions and Answers

1. Describe a collection using one word?
   - list
2. What are two most common types of collections we have been using in class? Are there other types of collections?
   - Array, ArrayList
   - Yes
3. Name some common scenarios of why you would loop through a collection.
   - find one
   - find many
   - aggregate (count, sum)
   - transform (change the shape)
   - do something for each
   - sort
4. What are the two basic states of data?
   - data at rest
   - data in transit
5. What does Java call or what are the objects Java uses to represent
   - data at rest (Collection)
   - data in transit (Stream)
6. What is the difference between a function and a method?
   - Answer: 
     - Method
         - a type of function
         - has to belong to a class
      - Function
        - a code that does something
        - don't have to belong to a class
7. Is a function or method like a lambda? How?
   - They have parameters, body, and potentially can return something
8.  What do you pass as an argument into the parameter of a collection's forEach method?
   - a lambda
9.  You get a stream of data by calling the "______" method on a "____"
    - Answer: Stream, Collection
10. The collect method takes the data in transit in a _____ and lays it to rest in a ______. 
    - Answer: Stream, Collection
11. Match these
   - find one
   - find many
   - aggregate (count, sum)
   - transform (change the shape)
   - do something for each
   - sort
   -  "to one of these methods on a stream"
   - count()
   - forEach()
   - sorted()
   - filter()
   - map()
   - reduce()
 - Answer: 
    - Count, Reduce -- Aggregate
    - Filter -- Find many
    - forEach -- do something for each
    - Sorted  -- Sort
    - Map -- Transform



## Sorting Quiz

- https://howtodoinjava.com/java8/stream-sorted-method/

1. What is the method that you can call on stream to sort it?
2. There are two overloads to the sorted method, what does each take as parameter(s).
3. What is a Comparator?
   - functional interface
   - defines custom sorting logic


1. What is the method that you can call on stream to sort it?
   - `sorted`
2. There are two overloads to the sorted method, what does each take as parameter(s).
   - no parameters (parameterless)
   - one parameter a Comparator
3. What is a Comparator?
   - functional interface
   - defines custom sorting logic
<!-- https://gemini.google.com/share/3a7d068066c5) -->


## Interfaces Quiz
1. All methods on an interface must be _____?
2. Can an interface have field/data members?
3. A concrete class inherits (extends) a base or abstract class, you ____ an interface?
4. A concrete class that extends an abstract class MUST override all ____ methods.
5. A concrete class that implements an interface MUST override ____ methods.
6. A concrete class can inherit (extend) multiple classes: true or false?
7. A concrete class can implement multiple interfaces: true or false?
8. Concrete classes can be instantiated: true or false?
9. Abstract classes can be instantiated: true or false?
10. Interfaces can be instantiated: true or false?
11. If a class says it implements an interface, but the compiler
doesn't find an implementation for the interface methods, it does what?



## Packages Quiz

1. A package is a _____?
2. Package should always be _____cased?
3. Packages are used to:
   1. Avoid ____ conflicts with classes?
   2. Organizing ____ classes?
4. A package name must match the ____ structure?
5. How do you use a class defined in a different package (folder)?
6. What is the difference between "a package statement at the top of a .java file" and "an import statement at the top of a .java file"
7. What the 3 most common packages we will create to hold our code for our in class projects.
8. Where does the App or Program class go?
9. What goes in the model package?
10. What goes in the data package?
11. What goes in the ui package?


1. A package is a _____?
   - folder or directory
2. Package should always be _____cased?
   - lower
3. Packages are used to:
   1. Avoid [name] conflicts with classes?
   2. Organizing [related] classes?
4. A package name must match the [folder or directory] structure?
5. How do you use a class defined in a different package (folder)?
   - import the package
6. What is the difference between "a package statement at the top of a .java file" and "an import statement at the top of a .java file"
    - package = where is the current class
    - import = make stuff in other folders available
7. What the 3 most common packages we will create to hold our code for our in class projects.
   - ui, model, data
8. Where does the App or Program class go?
   - at the root of the project in the root package
9.  What goes in the model package?
10. What goes in the data package?
11. What goes in the ui package?