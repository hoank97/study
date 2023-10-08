# Difference between Interceptor with Middleware

![](https://i.stack.imgur.com/2lFhd.jpg)

In NestJs, Interceptor and Middleware are two important concepts used to handle HTTP requests before they reach their destination. However, they have difference purpose and usage patterns, such as:
1. **`Middleware`**:
- Middlewares functions are executed before an HTTP request reaches the controller or handler.
- Middlewares can be used to inspect and process request before they reach the ore processing layer.
- Middlewares have the capability to intervene in the request and response handling without altering the main processing logic of the application.
- Examples: user authentication, logging, access control checks,etc
2. **`Interceptor`**:
- Interceptors are also intermediary classes, but they are typically used to modify or inspect data after it has been processed by a controller or a handler.
- Interceptors can intervene and modify the response data before it is returned to the client.
- Interceptors are often used for tasks such as transforming response data into a specific format, handling errors, and adding additional data to the response