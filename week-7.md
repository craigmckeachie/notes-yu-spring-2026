# Week 7

# Interfaces

- [Concrete vs Abstract vs Interface](https://github.com/craigmckeachie/Week6-Fall2025/blob/main/notes/inheritance-abstract-interfaces.md#-summary-table)
<!-- - [Fixed Assets Class Diagram](https://drive.google.com/file/d/1z5-C-2e7fXfd4ZaMA2yy31p9cUIcLSsX/view?usp=sharing) -->
<!-- - [Fixed Assets Code](https://github.com/miaraylight/finance-portfolio/tree/main/src/main/java/com/pluralsight) -->

# Lambdas

## 1. Traditional method version

```java
public class HelloWorld {

    public void sayHello() {
        System.out.println("Hello, World!");
    }
}
```

```java
public class Program {

    public static void main(String[] args) {

        HelloWorld hw = new HelloWorld();
        hw.sayHello();
    }
}
```

---

## 2. Lambda version

```java
public class Program {

    public static void main(String[] args) {

        Runnable hello = () -> System.out.println("Hello, World!");

        hello.run();
    }
}
```

## 1. Traditional method version

```java
public class HelloWorld {

    public void sayHello(String name) {
        System.out.println("Hello, " + name + "!");
    }
}
```

```java
public class Program {

    public static void main(String[] args) {

        HelloWorld hw = new HelloWorld();
        hw.sayHello("Craig");
    }
}
```

---

## 2. Lambda version

```java
import java.util.function.Consumer;

public class Program {

    public static void main(String[] args) {

        Consumer<String> hello =
            name -> System.out.println("Hello, " + name + "!");

        hello.accept("Craig");
    }
}
```

---

## Comparison

Traditional method:

```java
hw.sayHello("Craig");
```

Lambda version:

```java
hello.accept("Craig");
```

The lambda receives the parameter (`name`) and performs the behavior inline.



# Streams

- [Stream Examples: People Exercise](https://github.com/craigmckeachie/Streams/blob/main/src/main/java/com/pluralsight/Program.java)
- [Streams API benefits and drawbacks](https://chatgpt.com/share/690cb14e-3658-8000-a8b2-aa176deafff5)
- [When can I use a method reference (::)](https://chatgpt.com/c/690bc7b2-f0fc-832a-9da3-53744654a7c7)
- [Performance differences: Streams API vs Loops](https://chatgpt.com/c/690ba2e8-442c-832d-8174-56a9b95c58f2)

### The Stream Operations Cheat Sheet

* **Filtering and Finding:** Filter — Find many
* **Transforming Data:** Map — Transform
* **Sorting and Ordering:** Sorted — Sort
* **Aggregating and Reducing:** Count, Reduce — Aggregate
* **Consuming and Presenting:** forEach — Do something for each

---

### 5 Ways to Transform Your Data with Streams

* **The Sieve:** Filter — Find many
* **The Converter:** Map — Transform
* **The Organizer:** Sorted — Sort
* **The Accumulator:** Count, Reduce — Aggregate
* **The End of the Line:** forEach — Do something for each

# Packages

- [Java package organization](./java-package-organization.md)


# Sorting
- [Comparable vs Comparator and Sorting](https://chatgpt.com/share/6a0b34ac-0ec4-83ea-93d2-e083aa4535dc)