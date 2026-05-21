# Capstone 2: The DELI-cious Strategy Guide  

This guide is designed to help you plan, organize, and complete your DELI-cious capstone project using the same skills and patterns from Workbook 5 and Workbook 6.

> Objective: Build a Java CLI app that lets a user place a sandwich order, add drinks and chips, and generate a receipt.

> Note: These examples are meant to inspire and guide you — they are not intended as complete solutions. Use them as a starting point, but make sure your final project reflects your own ideas and effort. Some concepts in this document may be simplified or not fully fleshed out.

## 📋 Table of Contents

- 🧱 [Project Setup](#project-setup)
- 🧠 [Potential Classes](#potential-classes)
- ⚙️ [Core Functionality](#core-functionality)
- 🧾 [Required Screens & Menus](#required-screens-and-menus)
- 💲 [Sandwich Shop Pricing Guide](#sandwich-shop-pricing-guide)
- 🧪 [Example Pseudo Code for `Sandwich` Class](#example-pseudo-code-for-sandwich-class)
- 🧪 [Example Pseudo Code for `Order` Class](#example-pseudo-code-for-order-class)
- 🧪 [Example Pseudo Code for `ReceiptWriter`](#example-pseudo-code-for-receiptwriter)
- 🧪 [Example Pseudo Code for `UserInterface`](#example-pseudo-code-for-userinterface)
- 🔍 [Example Test Scenarios (manual or automated)](#example-test-scenarios-manual-andor-automed)
- 📦 [User Story Suggestions – DELI-cious Capstone](#user-stories-sugestions--deli-cious-capstone)
- 🤖 [Example AI Prompts for this Project](#-example-ai-prompts-for-this-project)

---

## Project Setup

- Setup a project board on on GitHub [more info](#user-stories-sugestions--deli-cious-capstone)
- Create your project using intelliJ in your `~/pluralsight/capstones` directory (e.g., `DELI-cious`)
- Setup the base project structure including packages and Application class with `main` method
- Initialize your repo and make your first commit
- Create a GitHub repo with same name as your capstone with NO readme and NO .gitignore
- Follow the last set of instructions to link your local repo to the remote repo


**Here is a potential package structure**

```
com.pluralsight              // Main program
com.pluralsight.ui           // UserInterface
com.pluralsight.models       // Order, Sandwich, Drink, Chips, Topping
com.pluralsight.util         // ReceiptWriter
```

---

## Potential Classes

| Class          | Purpose                              | Workbook Reference             |
|----------------|--------------------------------------|--------------------------------|
| `Order`        | Holds sandwiches, drinks, chips      | Workbook 6 - Portfolio         |
| `Sandwich`     | Bread, size, meat, toppings, sauces, total price   | Workbook 5 - House/Vehicle     |
| `Topping`      | meat, cheese, regular, sauce, etc        | Workbook 5 - Jewelry/Gold      |
| `Drink`,`Chips` | Name, size, price                   | Workbook 5 - BankAccount       |
| `ReceiptWriter`| Writes receipt using BufferedWriter  | Workbook 5 - DealershipFileManager |
| `UserInterface`| Menu prompts and input               | All CLI projects from Workbook 5 and Capstone 1 |

---

## Core Functionality

- Customers can create **custom sandwich orders**
- Sandwich sizes: `4"`, `8"`, `12"`
- Bread choices: `white`, `wheat`, `rye`, `wrap`
- Toppings:
  - **Regular toppings** (included): lettuce, onions, etc.
  - **Premium toppings** (meats, cheeses): cost varies by size
- Sandwiches can be toasted
- Customers can **add multiple sandwiches** to an order
- Optional additions: **Drinks** and **Chips**
- Display full order details and total price before confirmation
- Save each completed order to a **receipt file** in the `receipts` folder
  - File name format: `yyyyMMdd-HHmmss.txt`

---

## Required Screens and Menus

### 🏠 Home Screen

- `1) New Order`
- `0) Exit`

### 🧺 Order Screen

- `1) Add Sandwich`
- `2) Add Drink`
- `3) Add Chips`
- `4) Checkout`
- `0) Cancel Order`

### 🥖 Add Sandwich Flow

- Prompt for:
  - Bread type
  - Size
  - Toppings (user should be able to add extras of each)
    - Meats
    - Cheeses
    - Regular toppings
    - Sauces
  - Toasted or not

### 🥤 Add Drink

- Select drink size and flavor

### 🍟 Add Chips

- Select chip type

### ✅ Checkout

- Display full order summary and price
- Options:
  - `Confirm` → generate receipt file
  - `Cancel` → discard order

---


## Screen & Menu Examples


- [Screen & Menu Example Code](https://github.com/craigmckeachie/wb3-examples/blob/main/src/main/java/com/pluralsight/ScreenExample.java)


```text
[LEVEL 1: PARENT SCREEN]
   │
   ├── (Command: 1) ──► [LEVEL 2: CHILD SCREEN A] ───┐
   │                       │                         │ (Deep Path)
   │                       ├── (Command: 1) ──► [LEVEL 3: GRANDCHILD]
   │                       │                       └─ (Command: B) ─► Back to A
   │                       └── (Command: R) ──► Back to Parent
   │
   ├── (Command: 2) ──► [LEVEL 2: CHILD SCREEN B] ───┐
   │                       │                         │ (Shallow Path)
   │                       └── (Command: R) ──► Back to Parent
   │
   └── (Command: X) ──► [Exit Application]
```

### Naming

The **Feature-Based Naming Convention** is a strategy where method names are derived from the functional "domain" or "task" the user is currently interacting with. Instead of using generic terms like `Screen1` or `SubMenuA`, we use names that describe the application's actual features.

By incorporating the **Parent-Child-Grandchild** hierarchy into this convention, the code becomes a self-documenting map of the application's architecture.

#### The Anatomy of the Convention

1.  **Level 1 (The Root):** Use `runMainScreen()`. This is the "Parent" and acts as the entry point for the entire application.
2.  **Level 2 (The Domain):** Use `run[FeatureName]Screen()`. These are the "Children." They represent major sections of the app (e.g., `runCatalogScreen`).
3.  **Level 3 (The Specific Task):** Use `run[SubFeatureName]Screen()`. These are the "Grandchildren." They represent a deep dive into a specific functional task (e.g., `runBookSearchScreen`).

---

#### Updated Feature-Based Architecture Diagram

This diagram shows the final structure we built: two child features, where only one branches into a grandchild task.

- [Library App Screen & Menu Example Code](https://github.com/craigmckeachie/wb3-examples/blob/main/src/main/java/com/pluralsight/LibraryApp.java)

```text
[CLASS: LibraryApp]
   │
   └── runMainScreen()  (PARENT)
       │
       ├── (1) ──► runCatalogScreen() (CHILD A - Deep Path)
       │              │
       │              ├── (1) ──► runBookSearchScreen() (GRANDCHILD)
       │              │              └─ (B) ─► Back to Catalog
       │              │
       │              ├── (A) ──► [Direct Action: Add Book]
       │              └── (R) ──► [Return to Main Menu]
       │
       ├── (2) ──► runMemberScreen()  (CHILD B - Shallow Path)
       │              │
       │              ├── (A) ──► [Direct Action: Register Member]
       │              └── (R) ──► [Return to Main Menu]
       │
       └── (X) ──► [Exit Application]
```

---

#### Why This Works for Students

* **Logical Traceability:** When a student sees an error in a method called `runBookSearchScreen`, they immediately know it belongs to the Catalog feature.
* **Contextual Actions:** It allows for "Actions" (like `Add Book`) to exist at any level. You don't have to go to a Grandchild screen to do work; the Child screen can handle logic *and* navigation.
* **Stack Discipline:** It reinforces how the **Call Stack** works. Each method call is a new layer. To get back to the Parent, you must finish (exit) the Child.



#### Reference Code Summary

In the code, this convention looks like this:

* **Parent:** `runMainScreen()` handles the high-level switch.
* **Child:** `runCatalogScreen()` handles catalog logic and calls the search screen.
* **Grandchild:** `runBookSearchScreen()` handles the specific search task and provides a way back.

This pattern ensures that as the application grows from 3 screens to 30, the naming remains predictable and the hierarchy remains easy to follow.


## Sandwich Shop Pricing Guide

### 🥪 Sandwich Base Prices

| Size | Price |
|------|-------|
| 4"   | $5.50 |
| 8"   | $7.00 |
| 12"  | $8.50 |

---

### 🥩 Meats (Premium Toppings)

| Type        | 4"   | 8"   | 12"  |
|-------------|------|------|------|
| Steak       | $1.00 | $2.00 | $3.00 |
| Ham         | $1.00 | $2.00 | $3.00 |
| Salami      | $1.00 | $2.00 | $3.00 |
| Roast Beef  | $1.00 | $2.00 | $3.00 |
| Chicken     | $1.00 | $2.00 | $3.00 |
| Bacon       | $1.00 | $2.00 | $3.00 |
| **Extra Meat** | $0.50 | $1.00 | $1.50 |

---

### 🧀 Cheeses (Premium Toppings)

| Type        | 4"   | 8"   | 12"  |
|-------------|------|------|------|
| American    | $0.75 | $1.50 | $2.25 |
| Provolone   | $0.75 | $1.50 | $2.25 |
| Cheddar     | $0.75 | $1.50 | $2.25 |
| Swiss       | $0.75 | $1.50 | $2.25 |
| **Extra Cheese** | $0.30 | $0.60 | $0.90 |

---

### 🥬 Regular Toppings (Included)

- Lettuce
- Peppers
- Onions
- Tomatoes
- Jalapeños
- Cucumbers
- Pickles
- Guacamole
- Mushrooms

---

### 🧂 Sauces (Included)

- Mayo
- Mustard
- Ketchup
- Ranch
- Thousand Island
- Vinaigrette

---

### 🥣 Sides (Included)

- Au Jus
- Sauce

---

### 🥤 Drinks

| Size   | Price |
|--------|-------|
| Small  | $2.00 |
| Medium | $2.50 |
| Large  | $3.00 |

---

### 🍟 Chips

- $1.50

---

## Example Pseudo Code for `Sandwich` Class

```java
public class Sandwich {
    // store bread type (white, wheat, rye, wrap)
    // store sandwich size (4, 8, or 12 inches)
    // store if sandwich is toasted
    // store meats, cheeses, toppings, sauces
    // keep track of total price

    // constructor: takes bread type, size, toasted
    //   - initialize properties
    //   - set base price based on size

    // addMeat method:
    //   - add meat(s)
    //   - what is the price based on size and whether it's extra?

    // addCheese method:
    //   - add cheese(s)
    //   - add price depending on size and whether it's extra

    // addTopping method:
    //   - add topping(s)
    //   - no charge

    // addSauce method:
    //   - add sauce(s)
    //   - no charge

    // getPrice method:
    //   - return current total price

    // getSummary method:
    //   - return a string with all sandwich details and price
}
```

---

## Example Pseudo Code for `Order` Class

```java
public class Order {
    // create List for order items (sandwiches, drinks, and chips)
    // store total price

    // constructor:
    //   - initialize the class propeties

    // addSandwich method:
    //   - add sandwich to list

    // addDrink method:
    //   - add drink to list

    // addChips method:
    //   - add chips to list

    // getTotal method:
    //   - return total price of order

    // getSandwiches, getDrinks, getChips:
    //   - return the orders items

    // getOrderSummary method:
    //   - return formatted string of all items and total
}
```

---

## Example Pseudo Code for `ReceiptWriter`

```java
public class ReceiptWriter {
    public static void saveReceipt(Order order) {
        // generate filename using current date/time (yyyyMMdd-HHmmss.txt)
        // create a FileWriter and wrap in BufferedWriter (src/main/resources/receipts)

        // loop through all order items
        //   - write the items to the to reciept
        
        //write total cost

        // close BufferedWriter
        // handle IOException with error message
    }

    private static String generateTimestamp() {
        // Create timestamp string useing a formatter
        // return that string
    }
}
```

> Date formatting examples
```java
// Format 1: 09/05/2021        
DateTimeFormatter formatter1 = DateTimeFormatter.ofPattern("MM/dd/yyyy");

// Format 2: 2021-09-05
DateTimeFormatter formatter2 = DateTimeFormatter.ofPattern("yyyy-MM-dd");

// Format 3: September 5, 2021
DateTimeFormatter formatter3 = DateTimeFormatter.ofPattern("MMMM d, yyyy");

// Format 4: Sunday, Sep 5, 2021  10:02
DateTimeFormatter formatter4 = DateTimeFormatter.ofPattern("EEEE, MMM d, yyyy  HH:mm");

// Format 5: 2023-09-06 12:42:20
DateTimeFormatter formatter5 = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");

// Current date and time
LocalDateTime now = LocalDateTime.now();
System.out.println( now.format(formatter1));

```

## Example Pseudo Code for `UserInterface` 

```java
package com.pluralsight;

import java.util.Scanner;

public class UserInterface {
    // declare a Scanner object for reading input from the console

    // constructor:

    // showHomeScreen:
    //   - print app title and welcome message
    //   - print:
    //       1) New Order
    //       0) Exit
    //   - prompt user for choice
    //   - return user input

    // showOrderMenu:
    //   - print:
    //       1) Add Sandwich
    //       2) Add Drink
    //       3) Add Chips
    //       4) Checkout
    //       0) Cancel Order
    //   - prompt user for choice
    //   - return user input

    // promptForBreadType:
    //   - print list of bread types
    //   - ask user to type one in (e.g., "white")
    //   - return as user input

    // promptForSize:
    //   - ask user: "What size? (4, 8, or 12 inches)"
    //   - return user input

    // promptForToasted:
    //   - ask: "Would you like it toasted? (yes or no)"
    //   - return user input

    // promptForMeats:
    //   - possible loop (one or multiple meats?):
    //       - ask user to type of meat (or 'done' to finish)
    //       - ask if they want Extra meat? (yes or no)"
    //   - return list of meats and extras

    // and so on and so on........

}

```

---

## Example Test Scenarios (manual and/or automed)

| Scenario | What to Test |
|----------|--------------|
| **Basic sandwich** | Build a 4" sandwich with one meat and one cheese. Verify total = base + topping prices. |
| **Extra meat and cheese** | Add bacon (extra) and cheddar (extra) to an 8" sandwich. Confirm extra pricing is applied. |
| **Regular toppings only** | Add lettuce, onions, tomato, pickles. Confirm no additional charge. |
| **Toasted option** | Build a sandwich and mark it as toasted. Verify it shows up in the as toasted. |
| **Multiple sandwiches** | Add 2 or more sandwiches to one order. Confirm all are listed with correct prices. |
| **Add chips and drink** | Add chips and a medium drink. Verify total includes both. |
| **Cancel order** | Begin an order but choose Cancel. Confirm nothing is saved and app returns to main menu. |
| **Checkout and receipt** | Complete a full order and confirm a receipt file is created in the `receipts` folder. |
| **Receipt content** | Open the receipt file. Verify that all items, sizes, and prices match the screen summary. |
| **Invalid input handling** | Enter bad inputs (letters for size, negative values). App should reject and re-prompt. |
| **'Done' input ends loops** | When adding toppings, entering 'done' ends the input loop and moves on. |
| **Price formatting** | Check that prices show with 2 decimal places (e.g., `$8.50`). |


---

## User Stories Sugestions – DELI-cious Capstone

> Workbook 3 - 2 - Agile Software Development v6.0Y covers setting up a project board

---
```
Title: Set up project structure
Description: As a developer, I want to create the base folder and class structure so I can start coding my deli app.
- [ ] Create main package `com.pluralsight`
- [ ] Create sub-packages for `models`, `ui`, and `util`
- [ ] Create Application class with `main`method
```
```
Title: Initialize local repo and connect to GitHub
Description: As a developer, I want to link my local project to a GitHub repository so my work is backed up and shareable.
- [ ] Run `git init`
- [ ] Make initial commit
- [ ] create repo remote repo on GitHub with NO Readme.md and NO .gitignore
- [ ] Link local and remote repo
```
```
Title: Build Sandwich class
Description: As a customer, I want to be able to build a sandwich by choosing bread, size, and toppings so I can customize my meal.
- [ ] Define bread, size, and toasted options
- [ ] Add methods for meats, cheeses, and regular toppings
- [ ] Include price calculation logic
```
```
Title: Create Order class
Description: As a customer, I want to add multiple sandwiches, drinks, and chips to my order so I can place a full meal order.
- [ ] Create `ArrayList` for sandwiches
- [ ] Create `ArrayList` for drinks and chips
- [ ] Create `getTotal()` method
```
```
Title: Implement ReceiptWriter
Description: As a customer, I want to receive a receipt when I check out so I can confirm my order and total.
- [ ] Format receipt using `BufferedWriter`
- [ ] Use timestamp as filename
- [ ] Save file to `src/main/resources/receipts` folder
```
```
Title: Show Home Screen
Description: As a user, I want a main menu to start a new order or exit the app.
- [ ] Print welcome message
- [ ] Handle input for "1) New Order" and "0) Exit"
```
```
Title: Show Order Menu
Description: As a user, I want a menu where I can add items to my order or check out.
- [ ] Add options: Sandwich, Drink, Chips, Checkout, Cancel
- [ ] Handle input and route to correct prompt
```
```
Title: Add Signature Sandwich Option (Bonus)
Description: As a customer, I want to choose a pre-made sandwich like a BLT so I don't have to build it from scratch.
- [ ] Create classes like `BLTSandwich`
- [ ] Add them to order with set ingredients
- [ ] Calculate fixed price
```

---

## 🤖 Example AI Prompts for this project

### 🧱 Project Design & Planning

- “Can you explain how to break a project like a sandwich ordering app into classes and responsibilities?”
- “Why might it be useful to separate logic between a `main` method and a `UserInterface` class?”
- “What are common mistakes students make when planning CLI menu apps?”

---

### 🍞 Sandwich-Building Logic

- “How do you handle the logic of charging extra for meats and cheeses?”
- “What are some design choices to avoid when building a class that calculates price?”

---

### 🔁 Input & Menu Flow

- “What’s a good strategy for letting a user add as many toppings as they want using Scanner?”
- “Why does `nextLine()` sometimes behave strangely after `nextInt()` in Java?”
- “How should I organize my menus to keep my `main` method clean and readable?”

---

### 💾 File Writing (BufferedWriter)

- “Why might my receipt file be created but empty in Java?”
- “What’s a common mistake when using `BufferedWriter` to write multiple lines?”
- “Why does it matter where I call `writer.close()` in Java file writing?”

---

### 🧩 OOP Concepts & Stretch Goals

- “What’s the difference between an interface and an abstract class, and when would I use one in this app?”
- “If I wanted to sort all my order items by price, what design decisions would help make that easier?”

---

### 🧪 Debugging

- “Why isn’t my sandwich price calculating correctly even though I’m adding values?”
- “What can cause a loop to run forever when asking the user for input?”
- “Why am I getting `NullPointerException` when working with lists?”

---

### 💬 Presentation Prep & Reflection

- “How can I explain the most important design decisions I made in this project?”
- “What’s a good way to talk about what I struggled with and how I solved it?”