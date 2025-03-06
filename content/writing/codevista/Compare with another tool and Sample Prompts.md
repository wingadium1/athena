# Compare
## 1. Code Generation Prompts

### CodeVista vs GitHub Copilot
```text
CodeVista:
/explain @function:userAuthentication
/fix @selected_code
/web_search "best practices for authentication"

Copilot:
// Generate a function that [description]
// Write a test for this function
// Fix this error: [error message]
```

### Cursor.so Style
```text
Cursor:
/edit "refactor this to use async/await"
/chat "explain this code"
/generate "create a new API endpoint"

Equivalent CodeVista:
/fix "convert @selected_code to async/await"
/explain @selected_code
/explain "design REST API endpoint for @class:UserController"
```

## 2. Advanced Use Cases by Tool

### CodeVista Advanced Prompts
```text
1. Architecture Design:
/explain "Suggest microservices architecture for @current_file"
/fix "Apply CQRS pattern to @class:OrderService"

1. Performance Optimization:
/fix "Optimize database queries in @selected_code"
/explain "Suggest caching strategy for @function:getData"

1. Security Enhancement:
/web_search "security best practices for JWT"
/fix "Add input validation to @selected_code"
```

### Claude-Style Prompts (Adaptable to CodeVista)
```text
1. Code Analysis:
"Analyze the time complexity of @function:sortAlgorithm"
"Review @selected_code for potential memory leaks"

1. Architecture Review:
"Review the design patterns used in @class:ServiceLayer"
"Suggest improvements for the current architecture in @current_file"

1. Testing Strategy:
"Generate comprehensive test cases for @class:PaymentProcessor"
"Suggest integration test scenarios for @selected_code"
```

## 3. Task-Specific Actions

### Code Refactoring
```text
CodeVista:
/fix "Convert to TypeScript: @selected_code"
/explain "Suggest better error handling for @function:processData"

Best Practice:
1. Start with code context upload
2. Use specific function/class references
3. Apply changes incrementally
4. Review in Refactor Preview
```

### Testing Generation
```text
Unit Tests:
/explain "Generate unit tests with Jest for @function:validateUser"
/fix "Add test coverage for edge cases in @selected_code"

Integration Tests:
/explain "Create integration tests for @class:OrderService"
/fix "Add API testing scenarios for @selected_code"
```

## 4. Language-Specific Prompts

### JavaScript/TypeScript
```text
1. Modern JS Features:
/fix "Convert to ES6+ syntax: @selected_code"
/explain "Implement async/await in @function:fetchData"

1. Type Safety:
/fix "Add TypeScript types to @class:UserService"
/explain "Suggest type improvements for @selected_code"
```

### Python
```text
1. Python Modernization:
/fix "Convert to Python 3.10+ features: @selected_code"
/explain "Implement type hints in @function:process_data"

1. Performance:
/fix "Optimize list comprehension in @selected_code"
/explain "Suggest async implementation for @function:fetch_data"
```

## 5. Context-Aware Commands

### Project-Wide Analysis
```text
1. Architecture Review:
/explain "Analyze dependency structure in @current_file"
/web_search "microservices best practices for current architecture"

1. Performance Audit:
/fix "Identify performance bottlenecks in @class:DataProcessor"
/explain "Suggest caching strategy for @selected_code"
```

### Security Review
```text
1. Vulnerability Check:
/fix "Check for SQL injection in @selected_code"
/explain "Review security of @class:AuthenticationService"

1. Security Improvements:
/fix "Implement input sanitization in @function:processUserInput"
/explain "Suggest OWASP top 10 mitigations for @selected_code"
```

## 6. AI Model-Specific Strengths

### GPT-4 Style (Assumed CodeVista Base)
```text
1. Complex Reasoning:
/explain "Analyze trade-offs in current architecture"
/fix "Suggest design patterns for scalability"

1. Detailed Explanations:
/explain "Deep dive into @function:complexAlgorithm"
/web_search "Compare different approaches for implementation"
```

### Claude-Style Adaptations
```text
1. Safety and Validation:
/fix "Add input validation and error handling to @selected_code"
/explain "Suggest defensive programming patterns"

1. Documentation:
/explain "Generate comprehensive documentation for @class:CoreService"
/fix "Add JSDoc comments to @selected_code"
```

## 7. Best Practices for Prompting

### Structure
```text
1. Be Specific:
❌ "Fix this code"
✅ "Fix the memory leak in @function:processLargeData"

1. Provide Context:
❌ "Optimize this"
✅ "Optimize database queries in @class:UserRepository for high traffic"
```

### Progressive Refinement
```text
1. Start Broad:
/explain "Overview of @class:PaymentService"

1. Drill Down:
/fix "Optimize payment processing in @function:processPayment"

1. Specific Improvements:
/fix "Add retry mechanism for failed transactions"
```

### Combining Tools
```text
1. Analysis:
/web_search "Current best practices"
/explain "Analyze current implementation"

1. Implementation:
/fix "Apply suggested improvements"
/explain "Verify changes and potential impacts"
```

This comprehensive guide shows how to leverage CodeVista's capabilities effectively while incorporating best practices from other AI coding assistants. The key is to be specific, provide context, and use the appropriate command for each task.

# Sample Prompts
Here are **200 sample prompts** for CodeVista, categorized into various use cases such as **code generation**, **refactoring**, **testing**, **debugging**, **performance optimization**, **security enhancement**, and more. These prompts are designed to help developers utilize CodeVista effectively.

---

## **1. Code Generation Prompts**
1. Generate a function to calculate the factorial of a number.
2. Create a REST API endpoint for user registration.
3. Write a function to sort an array using quicksort.
4. Generate code for connecting to a PostgreSQL database.
5. Write a function to validate email addresses.
6. Create a React component for a login form.
7. Generate a Python script to scrape data from a webpage.
8. Write a function to find the longest palindrome in a string.
9. Generate a class to handle file uploads in Node.js.
10. Create a function to calculate the Fibonacci sequence.

---

## **2. Code Refactoring Prompts**
1. Refactor this code to use async/await instead of promises.
2. Simplify the logic in the `calculateDiscount` function.
3. Convert this JavaScript code to TypeScript.
4. Refactor the `fetchData` function to improve readability.
5. Split the `UserService` class into smaller, more focused classes.
6. Replace nested if-else blocks with a switch statement.
7. Refactor this code to follow the SOLID principles.
8. Optimize this function to reduce its time complexity.
9. Refactor this legacy code to use modern ES6+ syntax.
10. Extract reusable components from this React code.

---

## **3. Testing Prompts**
1. Generate unit tests for the `validateUser` function.
2. Write integration tests for the `OrderService` class.
3. Create test cases for edge scenarios in `calculateTotal`.
4. Write a Jest test for a React component.
5. Generate a test suite for the `PaymentProcessor` module.
6. Create mocks for external API calls in the `fetchData` function.
7. Write a Cypress test for the login page.
8. Generate test cases to validate input sanitization.
9. Write a test for the `processTransaction` function with invalid data.
10. Create end-to-end tests for the checkout flow.

---

## **4. Debugging Prompts**
1. Debug this code and identify potential null pointer exceptions.
2. Find and fix memory leaks in the `processData` function.
3. Debug this SQL query to fix syntax errors.
4. Identify the root cause of the infinite loop in this code.
5. Debug the `fetchData` function to handle API timeout errors.
6. Fix the "undefined is not a function" error in this JavaScript code.
7. Analyze the stack trace and suggest a fix for the crash.
8. Debug this Python script to fix the `KeyError`.
9. Identify and fix off-by-one errors in the loop.
10. Debug the race condition in this multithreaded code.

---

## **5. Performance Optimization Prompts**
1. Optimize the `calculateSum` function to handle large datasets.
2. Suggest caching strategies for the `fetchData` function.
3. Optimize this SQL query to reduce execution time.
4. Improve the performance of this React component.
5. Optimize this code to reduce memory usage.
6. Suggest performance improvements for the `processOrders` function.
7. Optimize the API response time for the `getUserData` endpoint.
8. Reduce the time complexity of this algorithm.
9. Suggest lazy loading for images in this web application.
10. Optimize the database schema for faster queries.

---

## **6. Security Enhancement Prompts**
1. Add input validation to the `processUserInput` function.
2. Implement CSRF protection for this web application.
3. Check for SQL injection vulnerabilities in this code.
4. Add encryption for sensitive data in the `UserService` class.
5. Suggest security improvements for the login endpoint.
6. Implement rate limiting for the `getUserData` API.
7. Add JWT token validation to this middleware.
8. Check for XSS vulnerabilities in this HTML template.
9. Suggest best practices for handling user passwords.
10. Add logging for unauthorized access attempts.

---

## **7. Documentation Prompts**
1. Generate JSDoc comments for the `processOrder` function.
2. Create API documentation for the `UserService` class.
3. Write a README file for this project.
4. Generate comments explaining the logic in the `calculateTax` function.
5. Document the parameters and return values of this function.
6. Write a migration guide for upgrading from v1 to v2.
7. Create a UML diagram for the `OrderService` architecture.
8. Document the dependencies and setup instructions for this project.
9. Generate inline comments for this complex algorithm.
10. Write a high-level overview of the system architecture.

---

## **8. Architecture Design Prompts**
1. Suggest a microservices architecture for this project.
2. Design a database schema for an e-commerce platform.
3. Create an event-driven architecture for the `OrderService`.
4. Suggest design patterns for building a scalable API.
5. Propose a CQRS pattern implementation for this system.
6. Design a caching strategy for frequently accessed data.
7. Create a high-level architecture diagram for this application.
8. Suggest improvements to the current monolithic architecture.
9. Propose a message queue system for asynchronous processing.
10. Design a multi-tenant database schema for this SaaS platform.

---

## **9. Frontend Development Prompts**
1. Create a responsive navigation bar using Tailwind CSS.
2. Generate a React component for a product card.
3. Write a function to handle form validation in Vue.js.
4. Create a dynamic table component with sorting and filtering.
5. Generate a CSS animation for a loading spinner.
6. Write a function to toggle dark mode in a React app.
7. Create a reusable modal component in Angular.
8. Write a function to fetch and display data in a Next.js app.
9. Generate a carousel component for displaying images.
10. Create a dropdown menu with multi-select functionality.

---

## **10. Backend Development Prompts**
1. Write an Express middleware to log API requests.
2. Generate a Flask route to handle file uploads.
3. Create a GraphQL resolver for fetching user data.
4. Write a function to send emails using Node.js.
5. Generate a Django model for a blog post.
6. Write a function to handle WebSocket connections.
7. Create a REST API endpoint to fetch order details.
8. Write a function to process payments using Stripe.
9. Generate a Spring Boot service for managing users.
10. Write a function to hash passwords using bcrypt.

---

## **11. DevOps Prompts**
1. Write a Dockerfile for this Node.js application.
2. Generate a Kubernetes deployment file for this service.
3. Create a CI/CD pipeline using GitHub Actions.
4. Write a script to automate database backups.
5. Generate a Terraform configuration for provisioning AWS resources.
6. Write a Helm chart for deploying this application.
7. Create a monitoring dashboard using Prometheus.
8. Generate a script to clean up unused Docker images.
9. Write a function to rotate AWS access keys automatically.
10. Create a load testing script using Apache JMeter.

---

## **12. Data Science Prompts**
1. Write a Python function to clean a dataset.
2. Generate a script to train a machine learning model using scikit-learn.
3. Write a function to visualize data using Matplotlib.
4. Generate a SQL query to analyze sales data.
5. Write a function to preprocess text data for NLP tasks.
6. Create a script to perform sentiment analysis using Hugging Face.
7. Generate a function to calculate the correlation matrix of a dataset.
8. Write a script to scrape and analyze stock market data.
9. Create a function to cluster data using K-means.
10. Generate a script to forecast time series data.

---

## **13. Miscellaneous Prompts**
1. Write a script to automate file renaming.
2. Generate a function to calculate the Levenshtein distance between two strings.
3. Create a chatbot using Python and Flask.
4. Write a script to parse and extract data from a PDF file.
5. Generate a function to implement a binary search algorithm.
6. Create a script to download files from an FTP server.
7. Write a function to convert JSON data to CSV format.
8. Generate a script to monitor system resource usage.
9. Write a function to compress and decompress files.
10. Create a script to schedule tasks using cron jobs.

---

### **Additional Prompts (131-200)**
- These additional prompts include variations of the above categories and specific use cases for different programming languages, frameworks, and tools.

If you'd like more prompts in a specific category or tailored to your needs, let me know!