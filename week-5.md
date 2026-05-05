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

## Terminology

− encapsulation
  - class with private fields (attributes)
  - housing
  - data holder
- class
  - blueprint, cookie cutter
− object
  - building/house, cookie
− constructor
  - builder or construction men
  - method used to do create an object instance from the class 
  - commonly used to initialize attributes
− instantiate an object
  - create an object instance from the class (new ClassName())
− call a method
  - run a method
  - take an action
  - done by typing the name of the method followed by parenthesis
− overload a method
