# Java Package Organization

---

```text id="t9q1bm"
com.companyname.budgettracker/
│
├── App.java
│
├── model/
│   ├── Transaction.java
│   ├── Deposit.java
│   └── Payment.java
│
├── data/
│   └── TransactionRepository.java
│
└── ui/
    └── UserInterface.java
```

---

## App.java (Entry Point)

```java id="a1p9kk"
package com.companyname.budgettracker;

public class App {

    public static void main(String[] args) {
        System.out.println("Budget Tracker Started");
    }
}
```

---

## model / Transaction.java (Abstract Base Class)

```java id="m8x2zz"
package com.companyname.budgettracker.model;

public abstract class Transaction {

    private String description;
    private double amount;

    public Transaction(String description, double amount) {
        this.description = description;
        this.amount = amount;
    }

    public String getDescription() {
        return description;
    }

    public double getAmount() {
        return amount;
    }
}
```

---

## model / Deposit.java (Positive Value Rule)

```java id="d1q7aa"
package com.companyname.budgettracker.model;

public class Deposit extends Transaction {

    public Deposit(String description, double amount) {
        super(description, Math.abs(amount));
    }
}
```

---

## model / Payment.java (Negative Value Rule)

```java id="p3k9mm"
package com.companyname.budgettracker.model;

public class Payment extends Transaction {

    public Payment(String description, double amount) {
        super(description, -Math.abs(amount));
    }
}
```

---

## data / TransactionRepository.java

```java id="r6t2qq"
package com.companyname.budgettracker.data;

public class TransactionRepository {
    // Will eventually handle storing and retrieving transactions
}
```

---

## ui / UserInterface.java

```java id="u7n4pp"
package com.companyname.budgettracker.ui;

public class UserInterface {

    public void displayWelcome() {
        System.out.println("Welcome to Budget Tracker");
    }
}
```

---

# Why This Structure Works

This design is intentionally small, but it already follows a clean separation of concerns pattern that scales well.

---

## 1. Root Level: App.java (Single Entry Point)

Keeping `App.java` at the root package:

* Makes startup obvious and easy to find
* Avoids burying the entry point in subpackages
* Keeps the simplest possible launch path for beginners

Its only job is to start the application and wire things together.

---

## 2. model Package (Core Domain Logic)

This is the most important part of the design.

### Why it exists:

It represents the “real world concepts” of the application.

### Why Transaction is abstract:

* A generic transaction should never exist alone
* Every transaction is either a `Deposit` or a `Payment`
* It enforces correct design at compile time

### Why subclasses exist:

* `Deposit` guarantees **positive values**
* `Payment` guarantees **negative values**

This prevents a whole category of bugs where sign logic leaks into UI or data layers.

So instead of this happening everywhere:

```java
amount = -amount; // scattered logic (bad)
```

The rule is centralized:

```java
new Payment("Rent", 1200.00); // always safe
```

---

## 3. data Package (Persistence Boundary)

This layer is responsible for:

* storing transactions (later file, database, memory list, etc.)
* retrieving transactions for the UI or business logic

### Why isolate it:

It prevents your UI and model from being tied to *how data is stored*.

So later you can change:

* in-memory list → file storage
* file storage → database

without touching your model or UI.

---

## 4. ui Package (User Interaction Layer)

This is where all console interaction lives.

### Responsibilities:

* printing menus
* reading user input
* calling repository/model logic

### Why keep it separate:

So your business logic is not polluted with:

```java
System.out.println(...)
Scanner input = new Scanner(System.in);
```

This separation makes the system easier to test and extend.

---

## Overall Design Philosophy

This structure follows a simplified layered architecture:

### Model (core rules)

→ “What is a transaction?”

### Data (storage)

→ “Where do transactions go?”

### UI (interaction)

→ “How does a user use the system?”

### App (composition root)

→ “How does everything start?”

---


