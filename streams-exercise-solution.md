

## Idea 1: The E-Commerce Store (`Product`)

Instead of searching for people by last name, students search for products by brand or category. Instead of averaging ages, they sum the total value of stock or count the items.

* **The Object:** `Product` (Properties: `String name`, `String category`, `double price`)
* **The Stream Conversions:**
* `getPeopleByLastName` $\rightarrow$ `getProductsByCategory` (using `.filter()`)
* `getAverageAge` $\rightarrow$ `getTotalInventoryValue` (using `.mapToDouble(Product::getPrice).sum()`)
* `getAges` $\rightarrow$ `getProductPrices` (using `.map()`)
* `Collections.max()` / `min()` $\rightarrow$ Finding the most and least expensive items.



> **Why it works:** Students intuitively understand online shopping. Summing up a shopping cart or inventory value is a real-world scenario they see every day.






## Code Example: The E-Commerce Implementation

To make it as easy as possible for you to swap out, here is how the rewritten code looks using **Idea 1 (Products)**, complete with your commented-out loops so students can compare the imperative vs. declarative approaches.

### The Product Class

```java
package com.pluralsight;

public class Product {
    private String name;
    private String category;
    private double price;

    public Product(String name, String category, double price) {
        this.name = name;
        this.category = category;
        this.price = price;
    }

    public String getName() { return name; }
    public String getCategory() { return category; }
    public double getPrice() { return price; }

    @Override
    public String toString() {
        return "Product{name='" + name + "', category='" + category + "', price=$" + price + "}";
    }
}

```

### The Main Program

```java
package com.pluralsight;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Program {
    public static void main(String[] args) {
        List<Product> inventory = getInventory();

        // 1. Filter by category
        String searchCategory = "Electronics";
        List<Product> electronics = getProductsByCategory(inventory, searchCategory);
        System.out.println("--- Electronics ---");
        printProducts(electronics);

        // 2. Sum the total cost of all products
        double totalValue = getTotalInventoryValue(inventory);
        System.out.println("\nTotal Inventory Value: $" + totalValue);

        // 3. Count how many items belong to a specific category
        long electronicsCount = countProductsInCategory(inventory, "Electronics");
        System.out.println("Number of Electronic items: " + electronicsCount);

        // 4. Map to get just the prices
        List<Double> prices = getPrices(inventory);

        // 5. Min / Max
        double mostExpensive = Collections.max(prices);
        double cheapest = Collections.min(prices);
        System.out.println("Most Expensive: $" + mostExpensive);
        System.out.println("Cheapest: $" + cheapest);
    }

    private static List<Product> getProductsByCategory(List<Product> products, String category) {
        // Old way:
        // List<Product> match = new ArrayList<>();
        // for(Product p : products) { if(p.getCategory().equals(category)) match.add(p); }
        // return match;

        return products.stream()
                .filter(p -> p.getCategory().equalsIgnoreCase(category))
                .toList();
    }

    private static double getTotalInventoryValue(List<Product> products) {
        // Old way:
        // double total = 0;
        // for(Product p : products) { total += p.getPrice(); }
        // return total;

        return products.stream()
                .mapToDouble(Product::getPrice)
                .sum();
    }

    private static long countProductsInCategory(List<Product> products, String category) {
        return products.stream()
                .filter(p -> p.getCategory().equalsIgnoreCase(category))
                .count(); 
    }

    private static List<Double> getPrices(List<Product> products) {
        return products.stream()
                .map(Product::getPrice)
                .toList();
    }

    private static void printProducts(List<Product> products) {
        products.stream().forEach(System.out::println);
    }

    private static List<Product> getInventory() {
        List<Product> products = new ArrayList<>();
        products.add(new Product("Laptop", "Electronics", 999.99));
        products.add(new Product("Smartphone", "Electronics", 699.99));
        products.add(new Product("Headphones", "Electronics", 149.99));
        products.add(new Product("Coffee Maker", "Home", 89.99));
        products.add(new Product("Blender", "Home", 45.50));
        products.add(new Product("Desk Chair", "Furniture", 199.00));
        products.add(new Product("T-Shirt", "Clothing", 19.99));
        products.add(new Product("Jeans", "Clothing", 49.99));
        return products;
    }
}

```