# Week 6

# Questions

## Inheritance Quiz

1. What does D.R.Y. mean?
2. Why do we need inheritance? Do all programming languages have inheritance?
3. What is the problem with putting everything in one class? 
4. Why do they call it inheritance?
5. What do we call the class we are inheriting from?
6. What do we call the class that is inheriting from another class?
7. Last week we learned about a has-a relationship. What is the inheritance relationship called?
8. What is the keyword that indicates one class inherits from another?
9. What gets inherited? 
    - data fields/members (attributes)?  
    - constructors?
    - getters and setters?
    - other methods?
10. What does the access modifier protected mean?






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



## Polymorphism Quiz
1. Define polymorphism.
2. Child class
    - ____ functionality of parent (add behavior/methods)
    - _____ functionality of parent (polymorphism)
3. Overriding Methods means in 6 words or less?
4. Overriding vs Overloading in 8 words or less each
   - Overloading = Same method name + Different parameters.
- Overriding = Same method name + Same parameters + Inheritance.
5. Do you need the @Override annotation to override a method?
6. What does the @Override annotation do? In 6 words or less?
   - method must override super class method
7. What is a reference variable?
8. Can a super/base/parent class be put in variable typed as a subclass/derived/child?
9. Can a subclass/derived/child class be put in variable typed as a super/base/parent?
10. What does the instanceof reserved keyword return?
11. Give me an example using 
    1.  instanceof in an if and then have a cast inside the loop
    2.  instanceof Dog d



## CLI Quiz

1. How many command shells does a Windows machine have (assume Git has been installed)? What is each called? Which one should you use in this class?
2. What is another name for a folder?
3. What is a file system?
4. `pwd` is an abbreviation for?
5. `ls` is an abbreviation for?
   1. what are some common 
6. `pwd` and `ls` are called?
7. In `ls -a` what is the `-a` called?
8. `cd` is an abbreviation for?
9. What is the command to go up one directory level?
10. What is the command to go to the users home directory?
    - where is the home directory on a Windows computer
    - where is the home director on a Mac computer
11. What is the command to create a folder?
12. What is the command to create a file?
13. What is the command to delete a file or directory?



====================================
# Answers

## Inheritance Quiz

1. What does D.R.Y. mean?
   - Don't repeat yourself
2. Why do we need inheritance? 
   - So that we don't repeat ourself
3. Do all programming languages have inheritance?
   - no
4. What is the problem with putting everything in one class? 
   - hard to read, debug, maintain
5. Why do they call it inheritance?
   - because it works like heredity
6. What do we call the class we are inheriting from?
   - super class, parent class, base class
7. What do we call the class that is inheriting from another class?
    - subclass, child class, derived class
8. Last week we learned about a has-a relationship. What is the inheritance relationship called?
  - is-a
9.  What is the keyword that indicates one class inherits from another?
10. What gets inherited? 
    - data fields/members (attributes)?  Y
    - constructors? NO
    - getters and setters? Y
    - other methods? Y
11. What does the access modifier protected mean? 
    - can see in child class (YES)
      - AND
    - can see in package (YES)



- Then Animal - Dog or Cat example

----



## Working with Constructors Quiz

1. Child class constructors automatically do what?
   - call the super class parameterless constructor
2. How would I call the super class parameterless constructor from a child class constructor?
   ```Java
    super();
   ```

>   
- Java will do things for you with parameterless constructors
  - generate a parameterless constructor for you automatically 
    - as long as you haven't created one with parameters
  - call the super class parameterless constructor from any constructor you create in a child class
  - BUT, when you create a constructor with parameters in a child class you usually don't want to call the parameterless constructor in the super class...you want to call a constructor in the super class with parameters so it can store them for you
    - you want to leave it up to the super class to do stuff for you 



## Polymorphism Quiz
1. Define polymorphism.
   - many forms
2. Child class
    - ____ functionality of parent (add behavior/methods)
    - _____ functionality of parent (polymorphism)
3. Overriding Methods means in 6 words or less?
   - Same method name + Same parameters + Inheritance.
4. Overriding vs Overloading in 8 words or less each
   - Overloading = Same method name + Different parameters.
   - Overriding = Same method name + Same parameters + Inheritance.
1. Do you need the @Override annotation to override a method?
   - no
2. What does the @Override annotation do? In 6 words or less?
   - declares that the method must override super class method
     - if not then you'll get a compiler error
3. Can a super/base/parent class be directly put in variable typed as a subclass/derived/child?
   - no or yes if you downcast it
4. Can a subclass/derived/child class be put in variable typed as a super/base/parent?
   - yes
5.  What does the `instanceof` reserved keyword return?
   - boolean
6.  Give me an example using `instanceof` in an if and then have a cast inside the loop
   



## CLI

1. How many command shells does a Windows machine have (assume Git has been installed)? What is each called? Which one should you use in this class?
   - 3
   - Command Prompt, Powershell, Git Bash
2. What is another name for a folder?
   - directory
3. What is a file system?
   - the root directory with all the subdirectories and files underneath it
4. `pwd` is an abbreviation for?
   - print working directory
5. `ls` is an abbreviation for?
   - list
6. `pwd` and `ls` are called?
   - commands
   - a command is like a method or function
7. In `ls -a` what is the `-a` called?
   - option, flag, argument, switch
   - a flag is like an argument passed into a parameter of a method or function 
8. `cd` is an abbreviation for?
   - change directory
9.  What is the command to go up one directory level?
    - `cd ..`
10. What is the command to go to the users home directory?
    - `cd ~` or just `cd`
    - where is the home directory on a Windows computer
      - `c:\users\username`
    - where is the home director on a Mac computer
      - `\users\username`
11. What is the command to create a folder?
    - `mkdir`
12. What is the command to create a file?
    - `touch filename`
13. What is the command to delete a file or directory?
    - `rm directory` or `rm filename`


## Abstraction Quiz

1. What two things can be abstract?
2. An abstract class cannot be used to...?
3. You know you could use an abstract class when someone asks this question?
4. Where do you put the reserved keyword `abstract` to make a class abstract?
5. Is it common to inherit from an abstract class? How many classes can you inherit from an abstract class?
6. Can you create a collection (for example an ArrayList or Array) and give it a generic data type of an abstract class
7. What is the difference between a method signature and it's implementation. Give some examples from your Github repositories.
8. Abstract methods must be declared in an ____ class.
9. Abstract methods must be _____ in a child/sub/derived class.
10. Give me one or more tips given in the workbook for using AI to learn Java?



## Branching and Merging with Git Quiz

1. Describe a practical example of branching where your production code has an issue that needs to urgently be fixed but you are halfway through working on a new feature on your computer?
2. What is the command to create a branch in git (describe all 3 parts of it)?
3. The reference "head"  refers to the branch you are ____ __?
4. Does the branch command in git put you in the new branch by default?
5. What is the command to create a branch and checkout the branch (move into it) using one command with options/flags? Bonus: Is there another way to do the same thing?
6. What is the command to view all branches?
7. What is the command and argument to delete a local branch?
8. The `git merge <branch-name-1>` command merges into `brach-name-1` or does it merge `branch-name-1` into the current branch (is it the source or the destination)?
9. A merge conflict will always occur when two branches make edits to the same file?
10. A merge conflict will always occur when two branches make edits to the same line in the same file?
11. How do you tell git that you've resolved a merge conflict? What do you need to do?
12. What command and flag will get you out of a merge that is non-trivial so you can reset and try a different approach or try again?