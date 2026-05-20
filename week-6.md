# Week 6

- moved all quizzes to [week-6-quizzes](./week-6-quizzes.md)

- [protected access modifier explained](https://gemini.google.com/app/3d5320da2fb2d605)
- [using Git on the command-line](https://codewithcraig.netlify.app/git/git-command-line/)
  

## Constructors and Inheritance

  - Java will do things for you with parameterless constructors
  - generate a parameterless constructor for you automatically 
    - as long as you haven't created one with parameters
  - call the super class parameterless constructor from any constructor you create in a child class
  - BUT, when you create a constructor with parameters in a child class you usually don't want to call the parameterless constructor in the super class...you want to call a constructor in the super class with parameters so it can store them for you
    - you want to leave it up to the super class to do stuff for you 