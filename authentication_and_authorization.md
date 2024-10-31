# Authentication
* Robust password storage using bcrypt(or similar algorithms) with a unique salt for hashing.
* Secure password reset mechanism with a time-limited token.
* Use JWT(JSON Web Tokens) for session management.

# Authorization
* Role-based access control(RBAC) to restrict access to certain parts of the application.
* Implement fine-grained access control to restrict access to specific resources.

## Strategies for Authorization

- **Role-Based Access Control (RBAC)**: Assigns roles to users with defined permissions.
- **Attribute-Based Access Control (ABAC)**: Permissions based on user attributes, resource attributes, and environmental factors.
- **Custom logic**: Authorization rules implemented directly in middleware.
- **Policy-Based Access Control (PBAC)**: Uses policies to determine access rights.
- **Claims-Based Access Control**: Uses claims in tokens to determine access rights.
- **Context-Based Access Control**: Access based on the context of the request.

## Secure Cookies
* use httpOnly and secure flags for cookies to prevent XSS attacks and to enforce HTTPS.
* Short session expiration time to reduce the risk of session hijacking.
* Use SameSite attribute to prevent CSRF attacks.
* Consider a dedicated library for session management like express-session.


# Cookies

### Do Cookies Violate RESTfulness of an API?

Using cookies does not inherently violate the principles of RESTfulness, but it depends on how they are used.

### Key Points:

1. **Statelessness**:
   One of the key principles of RESTful APIs is that they should be stateless. This means that each request from a
   client to the server must contain all the information the server needs to fulfill the request. The server should not
   rely on information stored on the server between requests, such as session data.

2. **Cookies and Statelessness**:
    - **Stateful Cookies**: If cookies are used to maintain session state on the server (e.g., with session IDs), this
      can violate the statelessness constraint of REST. This is because the server needs to keep track of session data
      between requests, making the interaction stateful.
    - **Stateless Cookies**: If the cookie is used purely as a token (e.g., a JWT) that contains all the necessary
      information for authentication, and no state is stored on the server between requests, this does not violate REST
      principles. The server just verifies the token and processes the request accordingly, maintaining the
      statelessness of the interaction.

3. **Use of Cookies in RESTful APIs**:
    - **Acceptable Use**: Using cookies as a means to pass stateless authentication tokens (like JWT) is acceptable in
      RESTful APIs. The cookie is simply a transport mechanism for the token.
    - **Problematic Use**: Relying on server-side sessions that are managed via cookies (e.g., traditional session
      cookies) can make the API less RESTful, as it introduces statefulness.


# JSON Web Tokens (JWT) / Tokens

### Web Applications:
- **Cookies for Authentication:** Web applications often use cookies for authentication. When a user logs in, the server
  creates a session and sends a session cookie to the browser. This cookie is automatically included in subsequent
  requests by the browser, allowing the server to recognize and authenticate the user.
- **Stateful vs. Stateless:** Cookies can be used in a stateful session-based authentication system, where the server
  keeps track of active sessions. Alternatively, cookies can store JWT tokens for stateless authentication, where the
  server doesn’t need to maintain session state.

### Mobile Applications:
- **Token-Based Authentication:** Mobile applications typically use a token-based system, such as JWT (JSON Web Token)
  or OAuth 2.0 tokens. Upon logging in, the server issues a token that the mobile app stores locally (e.g., in secure
  storage like Keychain for iOS or EncryptedSharedPreferences for Android).
- **Token Storage:** The token is sent with each request to the server via the `Authorization` header (e.g.,
  `Authorization: Bearer <token>`). The server then verifies the token to authenticate the user.

### Using Both Cookies and Tokens:
- **Web Apps:** Cookies are commonly used because they are automatically handled by the browser, simplifying session
  management. They can also store tokens (JWT) if a token-based system is desired.
- **Mobile Apps:** Use tokens stored in secure storage instead of cookies because mobile platforms do not natively
  support cookies in the same way browsers do.
- **API Design:** If your API needs to serve both web and mobile clients, it should be designed to support both cookies
  and tokens. For instance:
    - **Web Clients:** The API can authenticate using cookies.
    - **Mobile Clients:** The API authenticates using tokens.


# Strategies for Authorization

- **Role-Based Access Control (RBAC)**: Assigns roles to users with defined permissions.
- **Attribute-Based Access Control (ABAC)**: Permissions based on user attributes, resource attributes, and environmental factors.
- **Custom logic**: Authorization rules implemented directly in middleware.
- **Policy-Based Access Control (PBAC)**: Uses policies to determine access rights.
- **Claims-Based Access Control**: Uses claims in tokens to determine access rights.
- **Context-Based Access Control**: Access based on the context of the request.


# Resources
* [REST API Design Workshop](https://www.youtube.com/playlist?list=PL_XxuZqN0xVAWGDKIzcn6NWikVkljJQZc)
* [Advanced Node.JS](https://interactivecares.com/courseDetails/246?title=Advanced_Node.JS)