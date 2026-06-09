# Lab Exercise: Building the `InterviewController` from the Ground Up

### Objective

In this exercise, you will complete a REST controller for managing job interview schedules within the internship platform. You will use Spring Boot annotations to map incoming HTTP requests to specific Java methods, handle path variables, extract query parameters, and read request bodies.

### Background & Scenario

You are expanding the backend application to support interview tracking. You are given a partially complete `InterviewController`. Currently, it’s just a standard Java class with a hard-coded `List<Interview>` serving as an in-memory data store. Your job is to transform this class into a fully functioning REST API by adding the correct Spring Web annotations.

---

## Phase 1: The Starter Code

Create the following two classes within your existing project structure under the `model` and `controller` packages.

### 1. The Model Class (`model/Interview.java`)

```java
package com.pluralsight.demo.internship.model;

public class Interview {
    private Long id;
    private String candidateName;
    private String position;
    private String status; // e.g., "Scheduled", "Completed", "Canceled"

    // Constructors
    public Interview() {}

    public Interview(Long id, String candidateName, String position, String status) {
        this.id = id;
        this.candidateName = candidateName;
        this.position = position;
        this.status = status;
    }

    // Getters and Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public String getCandidateName() { return candidateName; }
    public void setCandidateName(String candidateName) { this.candidateName = candidateName; }
    public String getPosition() { return position; }
    public void setPosition(String position) { this.position = position; }
    public String getStatus() { return status; }
    public void setStatus(String status) { this.status = status; }
}

```

### 2. The Starter Controller (`controller/InterviewController.java`)

```java
package com.pluralsight.demo.internship.controller;

import com.pluralsight.demo.internship.model.Interview;
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;

// TODO: Task 1 - Make this class a REST Controller and map it to "/api/interviews"
public class InterviewController {

    private final List<Interview> interviews = new ArrayList<>();

    // Constructor seeds some initial mock data
    public InterviewController() {
        interviews.add(new Interview(1L, "Alice Smith", "Java Developer Intern", "Scheduled"));
        interviews.add(new Interview(2L, "Bob Jones", "React Front-End Intern", "Completed"));
        interviews.add(new Interview(3L, "Charlie Brown", "DevOps Intern", "Scheduled"));
    }

    // TODO: Task 2 - Map this method to GET requests at "/api/interviews"
    // BONUS Task 6 - Modify this method to filter by an optional "status" query parameter
    public List<Interview> getAllInterviews() {
        return interviews;
    }

    // TODO: Task 3 - Map this method to GET requests for a specific ID (e.g., /api/interviews/1)
    public Interview getInterviewById(Long id) {
        return interviews.stream()
                .filter(i -> i.getId().equals(id))
                .findFirst()
                .orElse(null);
    }

    // TODO: Task 4 - Map this method to POST requests to schedule a new interview
    public Interview scheduleInterview(Interview newInterview) {
        // Simple ID generation logic for our in-memory list
        Long nextId = interviews.stream().mapToLong(Interview::getId).max().orElse(0L) + 1;
        newInterview.setId(nextId);
        interviews.add(newInterview);
        return newInterview;
    }

    // TODO: Task 5 - Map this method to DELETE requests for a specific ID
    public String cancelInterview(Long id) {
        boolean removed = interviews.removeIf(i -> i.getId().equals(id));
        return removed ? "Interview canceled and removed successfully." : "Interview not found.";
    }
}

```

---

## Phase 2: Your Tasks

Your job is to add the appropriate Spring Web annotations to the starter code to achieve the following functionality:

1. **Task 1: Controller Setup** Turn the `InterviewController` into a JSON-rendering REST controller. Configure it so that every route handled by this class automatically begins with the base path `/api/interviews`.
* *Hint: Look into `@RestController` and `@RequestMapping`.*


2. **Task 2: Fetching All Interviews** Expose `getAllInterviews()` to handle standard HTTP `GET` requests at the base path.
* *Hint: Look into `@GetMapping`.*


3. **Task 3: Fetching a Single Interview by ID** Expose `getInterviewById()` to handle `GET` requests where the ID is part of the URL path (e.g., `/api/interviews/2`). Bind that URL token directly into the method's `id` parameter.
* *Hint: You will need a placeholder in your mapping path and the `@PathVariable` annotation.*


4. **Task 4: Scheduling a New Interview** Expose `scheduleInterview()` to handle HTTP `POST` requests. Ensure Spring intercepts the incoming JSON request payload and deserializes it directly into the `newInterview` object.
* *Hint: Look into `@PostMapping` and `@RequestBody`.*


5. **Task 5: Canceling an Interview** Expose `cancelInterview()` to handle HTTP `DELETE` requests targeting a specific interview ID via the path.
* *Hint: Combine `@DeleteMapping` and `@PathVariable`.*


6. **Bonus Task 6: Filtering via Query Parameters** Go back and modify `getAllInterviews()`. Introduce an optional string query parameter named `status` (e.g., `/api/interviews?status=Scheduled`). Update the method logic to return only interviews matching that status if it is provided, or all interviews if it is omitted.
* *Hint: Use the `@RequestParam` annotation and set its `required` attribute to `false`.*



---

## Reference Checklist

| Annotation | Purpose | Target Level |
| --- | --- | --- |
| **`@RestController`** | Marks the class as an API controller and serializes return values to JSON. | Class |
| **`@RequestMapping`** | Defines the base URL path for the entire controller. | Class |
| **`@GetMapping`** | Maps HTTP GET requests to a specific method. | Method |
| **`@PostMapping`** | Maps HTTP POST requests to a specific method. | Method |
| **`@DeleteMapping`** | Maps HTTP DELETE requests to a specific method. | Method |
| **`@PathVariable`** | Extracts values directly from the URL path pattern. | Parameter |
| **`@RequestParam`** | Extracts optional or required key-value pairs from the URL query string. | Parameter |
| **`@RequestBody`** | Binds the incoming HTTP request body JSON to a Java object. | Parameter |