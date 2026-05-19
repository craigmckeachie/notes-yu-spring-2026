Here is a complete, ready-to-distribute lab exercise for your students based on **Idea 1 (The E-Commerce Store)**.

It provides the complete `Product` class, a `Program` class pre-populated with inventory data, and stubbed-out methods. Each method contains explicit, step-by-step instructions in the comments directing them to first consider (or implement) the traditional `for-loop` approach, followed by the modern Java `Stream` approach.

---

### File 1: `Product.java`

Provide this file to your students as-is.

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

    public String getName() {
        return name;
    }

    public String getCategory() {
        return category;
    }

    public double getPrice() {
        return price;
    }

    @Override
    public String toString() {
        return "Product{name='" + name + "', category='" + category + "', price=$" + price + "}";
    }
}

```

---

### File 2: `Program.java` (The Student Exercise)

This is the starter file you will give to your students. It contains the data setup and the empty methods waiting for their code.

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
        System.out.println("--- " + searchCategory + " Items ---");
        List<Product> electronics = getProductsByCategory(inventory, searchCategory);
        printProducts(electronics);

        // 2. Sum the total cost of all products
        double totalValue = getTotalInventoryValue(inventory);
        System.out.println("\nTotal Inventory Value: $" + totalValue);

        // 3. Count how many items belong to a specific category
        String countCategory = "Clothing";
        long clothingCount = countProductsInCategory(inventory, countCategory);
        System.out.println("Number of " + countCategory + " items: " + clothingCount);

        // 4. Map to get just the prices
        List<Double> prices = getPrices(inventory);

        // 5. Display the most and least expensive items using Collections min/max
        if (!prices.isEmpty()) {
            double mostExpensive = Collections.max(prices);
            double cheapest = Collections.min(prices);
            System.out.println("\nMost Expensive Item: $" + mostExpensive);
            System.out.println("Cheapest Item: $" + cheapest);
        }
    }

    /**
     * TASK 1: Filter products by their category.
     */
    private static List<Product> getProductsByCategory(List<Product> products, String category) {
        // STEP A (Traditional Loop): 
        // Think about how you would do this with a standard for-each loop.
        // You would create a new empty List<Product>, loop through 'products',
        // use an if-statement to check if p.getCategory().equalsIgnoreCase(category),
        // add matching items to your list, and return it.

        // STEP B (Streams):
        // Comment out your loop if you wrote one, and implement it using Streams.
        // 1. Open a stream: products.stream()
        // 2. Use .filter() to check if the product category matches the input category.
        // 3. Use .toList() to collect the results into a list and return it.
        
        return null; // Replace this line
    }

    /**
     * TASK 2: Calculate the sum of all product prices in the inventory.
     */
    private static double getTotalInventoryValue(List<Product> products) {
        // STEP A (Traditional Loop):
        // Think about how you would do this with a standard loop.
        // You would declare a double total = 0;, loop through all products,
        // add each product's price to the total, and return the total.

        // STEP B (Streams):
        // Convert the logic to a Stream pipeline.
        // 1. Open a stream: products.stream()
        // 2. Map the products to their prices using .mapToDouble(Product::getPrice)
        // 3. Chain the .sum() terminal operation to get the total and return it.

        return 0.0; // Replace this line
    }

    /**
     * TASK 3: Count how many products match a given category.
     */
    private static long countProductsInCategory(List<Product> products, String category) {
        // STEP A (Traditional Loop):
        // Think about how you would do this with a loop.
        // You would declare an int count = 0;, loop through the products, 
        // increment count if the category matches, and return it.

        // STEP B (Streams):
        // Convert the logic to a Stream pipeline.
        // 1. Open a stream: products.stream()
        // 2. Filter out products that don't match the category.
        // 3. Use the .count() terminal operation to return the total count as a long.

        return 0; // Replace this line
    }

    /**
     * TASK 4: Transform a list of Products into a list of just their Prices (Doubles).
     */
    private static List<Double> getPrices(List<Product> products) {
        // STEP A (Traditional Loop):
        // You would create a List<Double> prices = new ArrayList<>();
        // Loop through 'products', get each price, add it to the list, and return it.

        // STEP B (Streams):
        // 1. Open a stream: products.stream()
        // 2. Use .map() to transform each Product object into its price (Product::getPrice).
        // 3. Use .toList() to collect the prices into a list and return it.

        return null; // Replace this line
    }

    /**
     * TASK 5: Print out each product in the list.
     */
    private static void printProducts(List<Product> products) {
        // STEP A (Traditional Loop):
        // You would write a for-each loop and call System.out.println(product);

        // STEP B (Streams):
        // Use a stream pipeline and the .forEach() terminal operation 
        // combined with a method reference (System.out::println) to print them all.
        
    }

    // Helper method to populate our test database
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
        products.add(new Product("Socks", "Clothing", 9.99));
        return products;
    }
}

```