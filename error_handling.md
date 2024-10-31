# Application Stability

- **Crash Prevention**: Unhandled errors can crash your Node.js application entirely. Well-structured error handling 
  with mechanisms like try-catch blocks and error handling middleware prevents unexpected crashes, keeping your 
  application running even when issues arise.
- **Controlled Failure**: Graceful recovery strategies (e.g., retrying failed operations, logging the error, and 
  notifying external systems) prevent a single error from causing cascading failures throughout your application.
- **Resiliency & Uptime**: A robust error handling system makes your application more resilient in the face of
  unexpected issues and contributes to increased uptime—a critical aspect of production applications.

# User Experience
- **Informative Error Messages**: Instead of cryptic error messages or the application breaking suddenly, customize your
  error responses to provide context-aware messages to the user. This guides them on potential corrective actions or  
  reassures them that the problem is being looked into.
- **State Preservation**: In some cases, you might be able to recover gracefully without interrupting the user's 
  workflow. Proper error handling helps in designing smooth recovery experiences.
- **Reduced Frustration**: Well-handled errors with helpful guidance translate to a less frustrating experience for your
  users, improving their overall satisfaction with your application.


# Debugging Efficiency
- **Detailed Error Logging**: Structured error logs with timestamps, stack traces, contextual information (like user ID,
  request data) are invaluable for quickly pinpointing the root cause of problems.
- **Understanding Error Patterns**: Centralized error handling and logging allow you to track error trends over time. 
  This can reveal issues you might not be aware of otherwise, enabling proactive fixes.
- **Faster Troubleshooting**: The right error information available at your fingertips streamlines the debugging 
  processes, reducing the time it takes to identify and resolve problems.


# Async-Await for Async Error Handling
- **Improves Code Readability**: Helps avoid the complexity of nested callbacks ("callback hell") within asynchronous
  operations.
- **Maintains Structure**: Ensures cleaner, more structured error handling using familiar control flow patterns like 
  `try...catch` for async-await.
- **Promotes Consistency**: Using promises provides a standardized way to represent the outcome of asynchronous tasks, 
  simplifying error handling.

```javascript
// Example using async-await
async function fetchData() {
    try {
        const response = await fetch('https://api.example.com/data');
        const data = await response.json();
        return data;
    } catch (error) {
        console.error('Error fetching data:', error);
        // Handle the error appropriately (throw, log, etc.)
    }
}

// Example using promise chaining
fetch('https://api.example.com/data')
    .then((response) => response.json())
    .then((data) => console.log(data))
    .catch((error) => console.error('Error fetching data:', error));
```

## Extend the Built-in Error Object
- **Provides Richer Context**: Create custom error classes to include additional properties that help quickly identify 
  the error cause (e.g., HTTP status codes, invalid fields, etc.).
- **Improves Error Classification**: Differentiate between various types of errors for specific handling and logging
  (e.g., ValidationError, DatabaseError).
- **Allows Customization**: Offers flexibility to tailor error messages, stack traces, and properties based on project
  requirements.

```js
class ValidationError extends Error {
  constructor(message, field) {
    super(message);
    this.status = 400; // HTTP status code
    this.field = field;
  }
}

function validateUser(user) {
  if (!user.email) {
    throw new ValidationError('Email is required', 'email');
  }
  // Further validations
}
```

# Distinguish Operational vs. Programmer Errors

- **Enhances Recovery Strategies**: Helps shape how errors are handled. Operational errors can require retry mechanisms 
  or user-friendly messages, while programmer errors necessitate code fixes.
- **Identifies Problem Areas**: Knowing the error source aids in debugging. It clearly indicates whether the error is
  expected (e.g., network issues) or caused by a defect in the code.
- **Prioritizes Problem Solving**: Assists in effectively triaging and prioritizing issues during development and in
  production environments.

### Example Code
```js
// Operational error
async function connectToDatabase() {
  try {
    const client = await MongoClient.connect('mongodb://localhost:27017');
    // Use the client
  } catch (error) {
    console.error('Error connecting to database:', error);
    // Retry or provide user-friendly message
  }
}

// Programmer error
function calculateArea(width, height) {
  // Missing height parameter check
  const area = width * undefined; // This will result in NaN (Not a Number)
  return area;
}
```

# Handle Errors Centrally, Not Within a Middleware
- **Promotes Consistency**: A centralized error handler ensures a consistent response format across the application
  (e.g., consistent error structures and HTTP status codes).
- **Reduces Code Duplication**: Prevents redundant error handling logic in each middleware function.
- **Simplifies Maintenance**: Changes to error handling mechanisms can be easily made in a single location.

```js
// Express error handling
app.use((err, req, res, next) => {
  console.error(err.stack);
  next(err); // Pass error to central handler
});

// Central error handler
app.use((err, req, res, next) => {
  const statusCode = err.status || 500;
  res.status(statusCode).send({ message: err.message });
});
```


# Document API Errors Using OpenAPI or GraphQL
- **Streamlines Client Integration**: Clearly define potential error responses for API developers, reducing guesswork
  and improving integration efficiency.
- **Helps Client-Side Error Handling**: Enables clients to gracefully handle expected errors with appropriate actions.
- **Enhances API Reliability**: Improves overall API predictability and robustness.


# Exit the Process Gracefully
- **Prevents Resource Leaks**: Unrecoverable errors (e.g., out of memory) can cause a program to hang. Graceful exits 
  clean up resources and log error details.
- **Enables Automated Restarts**: Facilitates automated process restarts using process managers (e.g., PM2) for 
  increased application resilience.
- **Allows External Monitoring**: Provides a signal to external monitoring tools to notify developers of critical 
  issues.

```javascript
process.on('uncaughtException', (error) => {
  console.error('Uncaught Exception:', error);
  // Log error, potentially notify monitoring system
  process.exit(1);
});
```

## Use a Mature Logger to Increase Errors Visibility
- **Structured Error Reporting**: Mature loggers (e.g., Winston, Bunyan) allow adding metadata (timestamps, error 
  levels) for efficient analysis.
- **Flexible Outputs**: Send logs to various destinations (console, files, external services) for analysis and 
  retention.
- **Debugging & Auditing**: Essential for investigating errors in production environments and tracing error patterns.

```javascript
const winston = require('winston');
const logger = winston.createLogger({
    level: 'error',
    format: winston.format.simple(),
    transports: [new winston.transports.Console()],
});

logger.error('An error occurred:', { additionalInfo: 'Some context' });
```

# Test Error Flows Using Test Framework
- **Verifies Error Handling Logic**: Confirms error handlers behave as expected under various failure scenarios.
- **Prevents Regressions**: Ensures code changes don't introduce new error handling bugs.
- **Simulates Real-world Failure**: Tests how the application withstands errors from network requests, database issues,
  and more.

```javascript
const invalidParameters = [
  ['object1._id'],
  ['object1._id', 'object2._id', null],
  ['object1._id', 'object2._id', [{ id: 'object3._id' }]],
  ['object1._id', 'object2._id', [{ _id: 'object3._id', newAttribute: true }]]
];

invalidParameters.forEach((params) => {
  expect(() => callStubbed(user, 'updateStudent', ...params)).to.throw(
    /Match error/
  );
});
```

# Discover Errors and Downtime Using APM Products
- **Proactive Monitoring**: Detects errors and performance issues in real-time within production environments.
- **Analyzes Error Patterns**: APM tools (e.g., New Relic, Datadog) collect and aggregate error data for pattern 
  identification.
- **Provides Alerts**: Sends customizable notifications based on error thresholds and other metrics.

# Catch Unhandled Promise Rejections
- **Prevents Silent Failures**: Unhandled promise rejections can cause a process to crash unexpectedly.
- **Maintains Stability**: Ensures unexpected errors are logged and dealt with, improving application resilience.

```javascript
process.on('unhandledRejection', (reason, promise) => {
  console.error('Unhandled Promise Rejection:', reason);
  // Handle the error appropriately
});
```


# Validate Arguments Using a Dedicated Library
- **Early Bug Detection**: Prevents cascading errors by validating inputs at function boundaries.
- **Increases Predictability**: Ensures functions work with valid data, reducing unexpected outcomes.
- **Streamlines Error Handling**: Provides descriptive validation errors.

```javascript
// Define a schema for user input
const userSchema = Joi.object({
  username: Joi.string().alphanum().min(3).max(30).required(),
  password: Joi.string().pattern(new RegExp('^[a-zA-Z0-9]{3,30}$')).required(),
  email: Joi.string()
    .email({ minDomainSegments: 2, tlds: { allow: ['com', 'net'] } })
    .required(),
});

function createUser(userData) {
  // Validate against the schema
  const { error, value } = userSchema.validate(userData);

  if (error) {
    throw new Error(
      `Validation error: ${error.details.map((x) => x.message).join(', ')}`
    );
  }

  // If validation passes, proceed with creating the user.
}
```

# Subscribe to Event Emitters' 'error' Event

- **Centralized Error Handling**: Handle errors consistently in one place, preventing them from going unnoticed and potentially crashing your application.
- **Improved Debugging**: Logged errors with context (e.g., the EventEmitter that emitted the error) provide valuable clues during debugging.
- **Proactive Monitoring**: Catch errors early on before they escalate into bigger problems or impact your users.

```javascript
const http = require('http');
const server = http.createServer((req, res) => {
  // ... request handling logic
});

server.on('error', (error) => {
  console.error('Server error:', error);
  // Additional error handling logic here (e.g., restarting the server)
});

server.listen(3000);
```

# Resources
* [Advanced Node.JS](https://interactivecares.com/courseDetails/246?title=Advanced_Node.JS)