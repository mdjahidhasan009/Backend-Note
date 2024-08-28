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