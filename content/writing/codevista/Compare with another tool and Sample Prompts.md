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

Here are more than **100 sample prompts** for CodeVista, categorized into various use cases based on the features and capabilities outlined in the uploaded document. These prompts are designed to help developers maximize their use of CodeVista.

---

### **1. Code Explanation Prompts**
1. Explain the purpose of @function:getUserData.
2. What does this code do? @selected_code.
3. Explain the logic of the @class:OrderProcessor class.
4. Explain the difference between `let` and `var` in JavaScript.
5. Explain why this SQL query is slow: @selected_code.
6. Explain the algorithm used in @function:sortArray.
7. What is the time complexity of this function? @selected_code.
8. Explain the purpose of @current_file.
9. Explain the usage of recursion in @function:calculateFactorial.
10. What does this regular expression do? `/[a-z]{3,}/`.

---

### **2. Code Fixing Prompts**
1. Fix this code: @selected_code.
2. Fix the syntax error in @function:fetchData.
3. Fix the performance bottleneck in @class:DataProcessor.
4. Fix the null pointer exception in @selected_code.
5. Fix this error: "TypeError: undefined is not a function" in @current_file.
6. Fix the security vulnerability in @function:processPayment.
7. Fix the memory leak in @function:handleLargeData.
8. Fix the deprecated API usage in @selected_code.
9. Fix the SQL injection vulnerability in @function:getUserInput.
10. Fix the infinite loop in @selected_code.

---

### **3. Code Autocompletion Prompts**
1. Autocomplete this function: @selected_code.
2. Suggest a completion for @function:calculateTax.
3. Complete this SQL query: `SELECT * FROM users WHERE`.
4. Complete the React component for a login form.
5. Complete the missing logic in @function:processOrder.
6. Suggest autocompletion for this Python script: @current_file.
7. Complete the function to fetch data from an API.
8. Provide a completion for this HTML form: `<form>`.
9. Autocomplete the CSS for a responsive grid layout.
10. Suggest autocompletion for the @class:UserController class.

---

### **4. Unit Testing Prompts**
1. Generate unit tests for @function:validateUserInput.
2. Write Jest tests for @class:OrderProcessor.
3. Generate test cases for edge scenarios in @function:calculateDiscount.
4. Write a unit test for this code: @selected_code.
5. Create a test suite for @current_file.
6. Generate test cases for input validation in @function:processData.
7. Write a test for the @function:fetchUserData function with mock API calls.
8. Generate unit tests for @class:PaymentService.
9. Write test cases for this function: @selected_code.
10. Generate test cases for invalid inputs in @function:calculateFactorial.

---

### **5. Refactoring Prompts**
1. Refactor this code to use async/await: @selected_code.
2. Refactor @function:processData to improve readability.
3. Refactor @class:UserManager to follow the SOLID principles.
4. Refactor this legacy code to use ES6+ syntax: @current_file.
5. Refactor @function:fetchOrders to reduce time complexity.
6. Refactor the nested if-else blocks in @selected_code.
7. Refactor @function:calculateTotal to use a functional programming approach.
8. Refactor this code to use dependency injection: @selected_code.
9. Refactor @class:OrderService to split responsibilities into smaller classes.
10. Refactor this code to improve maintainability: @current_file.

---

### **6. Performance Optimization Prompts**
1. Optimize the performance of @function:fetchData.
2. Suggest caching strategies for @function:getUserDetails.
3. Optimize this SQL query: @selected_code.
4. Improve the performance of @class:DataProcessor.
5. Optimize the API response time of @function:getOrders.
6. Reduce the time complexity of @function:sortArray.
7. Optimize @function:processLargeDataset for memory usage.
8. Suggest performance improvements for @class:OrderManager.
9. Optimize the database schema in @current_file.
10. Improve the load time of this web page: @selected_code.

---

### **7. Security Enhancement Prompts**
1. Add input validation to @function:processUserInput.
2. Check for SQL injection vulnerabilities in @selected_code.
3. Implement CSRF protection in @current_file.
4. Add encryption for sensitive data in @class:UserService.
5. Suggest security improvements for @function:loginUser.
6. Check for XSS vulnerabilities in @function:renderHTML.
7. Add JWT validation to @function:verifyToken.
8. Enhance the security of @class:PaymentGateway.
9. Add rate limiting to @function:handleRequests.
10. Check for potential vulnerabilities in @current_file.

---

### **8. Documentation Prompts**
1. Generate JSDoc comments for @function:processOrder.
2. Add docstrings to all functions in @current_file.
3. Create API documentation for @class:OrderService.
4. Write a README file for this project.
5. Generate comments explaining @function:calculateTax.
6. Document the parameters and return values of @function:fetchUserData.
7. Create a UML diagram for @class:UserManager.
8. Write a high-level overview of the system architecture in @current_file.
9. Generate inline comments for @selected_code.
10. Document the dependencies and setup instructions for this project.

---

### **9. Architecture Design Prompts**
1. Suggest a microservices architecture for this project.
2. Design a database schema for an e-commerce platform.
3. Create an event-driven architecture for @class:OrderProcessor.
4. Suggest design patterns for building a scalable API.
5. Propose a CQRS pattern implementation for this system.
6. Design a caching strategy for @function:getFrequentlyAccessedData.
7. Create a high-level architecture diagram for @current_file.
8. Suggest improvements to the current monolithic architecture.
9. Propose a message queue system for asynchronous processing.
10. Design a multi-tenant database schema for this SaaS platform.

---

### **10. Frontend Development Prompts**
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

### **11. Backend Development Prompts**
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

### **12. DevOps Prompts**
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

### **13. Miscellaneous Prompts**
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

### **14. Advanced Prompts**
1. Generate a function to perform sentiment analysis using Hugging Face.
2. Write a script to scrape and analyze stock market data.
3. Create a function to cluster data using K-means.
4. Generate a script to forecast time series data.
5. Write a Python function to clean a dataset.
6. Generate a SQL query to analyze sales data.
7. Write a function to preprocess text data for NLP tasks.
8. Create a script to train a machine learning model using scikit-learn.
9. Write a function to visualize data using Matplotlib.
10. Generate a function to calculate the correlation matrix of a dataset.

---

### **15. Additional Prompts**
These include variations of the above prompts tailored for specific programming languages, frameworks, or tools. Examples:
1. Refactor this Python script to improve readability.
2. Generate a test case for this Java code: @selected_code.
3. Write a function to implement OAuth2 authentication in @current_file.
4. Create a CI/CD pipeline for deploying a React app.
5. Suggest improvements to the GraphQL schema in @current_file.

Let me know if you'd like more prompts in a specific category or tailored for a particular use case!