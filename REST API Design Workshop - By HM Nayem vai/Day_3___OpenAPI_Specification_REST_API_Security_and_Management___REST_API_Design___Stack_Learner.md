# REST API Specification

An API is a way for applications to interact with data and systems. Instead of humans using it directly, applications
such as **web or mobile applications, as well as REST Clients like Postman or Curl**, use an API to communicate with a 
backend system. By understanding this definition, we can see that every API has a provider and a consumer. The "REST API
Specification" ensures that there is a clear and well-defined contract between these two parties. In the "REST API
Specification," we follow two types of contracts:

- **Contract-last**, also known as the **Code-first** approach.
- **Contract-first**, also known as the **Design-first** approach.

## Contract-last or Code-first Approach

In the contract-last approach, also known as the **code-first approach**, the API design process starts with the 
implementation of the backend logic or services. Once the backend implementation is complete, developers generate the
API specification or contract based on the implemented functionality. The API contract is then used to document the API 
endpoints, request and response formats, and other details.

### Specifications:
- **Implementation-driven**: The API design is driven by the implementation details of the backend services or
  functionality.
- **Code-centric**: Developers focus on writing code to implement the desired functionality, and the API contract is 
  derived from the implemented code.
- **Iterative**: Changes to the API design may require modifications to the underlying codebase, leading to an iterative
  development process.
- **Less focus on API design upfront**: API design decisions are deferred until after the backend implementation is 
  complete, potentially leading to increased complexity in adapting the API as it evolves.

## Contract-first or Design-first Approach

In the API-first approach, the **API design process begins with defining the API contract or specification upfront**,
before any implementation work is done. The API contract outlines the endpoints, request and response formats, data
models, and other details of the API. Once the API contract is defined and agreed upon, developers can then proceed to 
implement the backend logic or services based on the specified contract.

### Specifications:
- **Design-driven**: Focus on defining a clear and consistent API contract upfront.
- **Specification-centric**: Developers create the API contract as a separate artifact, typically using formats such as
  OpenAPI before writing code.
- **Contract-driven development**: Developers build the backend implementation based on the agreed-upon API contract.
- **Early validation and feedback**: By defining the API contract upfront, teams can validate the design, solicit 
  feedback, and make adjustments before the implementation begins.

In this lecture, we'll use the **OpenAPI Specification** to write our API specification.

* **Analysis**
* **Specifications**
* **Mocking & Validation**
* **Requirements**

# OpenAPI Specification
The OpenAPI Specification (OAS), formerly known as Swagger Specification, is an open standard for defining and 
documenting RESTful APIs. It provides a standardized format for describing the **structure, endpoints, request and 
response formats, parameters, authentication methods**, and other details of an API. The OpenAPI Specification is 
typically written in JSON or YAML format and serves as a contract that defines how clients can interact with the API.

## Key Features
### OPEN API Specification
- **API Documentation**: The OpenAPI Specification serves as a comprehensive documentation for the API, allowing
  developers to understand the functionality, endpoints, and data formats supported by the API.
- **Interoperability**: The standardized format of the OpenAPI Specification enables interoperability between different
  tools, frameworks, and platforms. It allows developers to generate client libraries, server stubs, and other artifacts 
  automatically from the API definition.
- **Code Generation**: Developers can use the OpenAPI Specification to generate client SDKs, server stubs, and API 
  documentation automatically using various code generation tools and libraries.
- **Validation and Testing**: The OpenAPI Specification can be used to validate API requests and responses, ensuring 
  that they conform to the specified contract. It also facilitates automated testing of API endpoints and scenarios.
- **Tooling Ecosystem**: There is a rich ecosystem of tools and utilities built around the OpenAPI Specification, 
  including editors, validators, code generators, and documentation generators. These tools streamline API development, 
  testing, and documentation processes.
- **Versioning and Evolution**: The OpenAPI Specification supports the versioning and evolution of APIs by allowing 
  developers to define multiple versions of the API and document changes between versions. This helps maintain backward
  compatibility and communicate changes effectively to API consumers.

**We can use SwaggerHub, online Swagger editor, VSCode plugin, and JSDoc to generate Swagger documentation.**

We will use YAML and [OpenAPI specification](https://spec.openapis.org/oas/v3.1.0.html)

Those all property needed for swagger
```
openapi: 3.0.0  # required: version number of the specification that is being used
info: {}  # type: InfoObject - metadata about the API
servers: []  # type: Server Object - an array of server objects
tags: []  # type: Tag Object - an array of tags used by the document with additional meta data
security: []  # type: Security Requirement Object - an array of supported authorization techniques
paths: {}  # type: Path Object - required: the available paths and operations for the API
components: {}  # type: the container of various reusable schemas for the document
```

## Example YML for swagger
```yaml
openapi: 3.0.0
info: 
  title: 'My API Design'
  description: 'This is the API design for my personal blogging application. I am designing it from scratch.'
  termsOfService: https://example.com/terms # if available
  contact: 
    name: HM Nayem
    url: https://example.com/contact
    email: support@example.com
  license:
    name: Apache 2.0
    url: https://www.apache.org/licenses/LICENSE-2.0.html
  version: 1.0.0
servers:
  - url: https://api.example.com/v1
    description: production server
  - url: http://localhost:4000/v1
    description: Dev
paths: 
  /products:
    get: 
      tags: 
        - Products
      description: 'Fetch all products including search, filter, and pagination'
      parameters: 
        - name: search
          in: query
          required: true
          schema: 
            type: string
          example: keyboard
        - name: page
          in: query
          required: true
          schema: 
            type: integer
          example: 1
        - name: limit
          in: query
          required: true
          schema: 
            type: integer
          example: 100
      responses: 
        '200': 
          description: 'A list of products will be returned'
          content: 
            application/json: 
              schema: 
                type: object
                properties: 
                  name: 
                    type: string
                    example: 'Keyboard'
                  price: 
                    type: integer
                    example: 100
            application/xml: 
              schema: 
                type: object
                properties: 
                  name: 
                    type: string
                    example: 'Keyboard'
                  price: 
                    type: integer
                    example: 100
        '401':
          description: 'Unauthorized'
          content: 
            application/json: 
              schema: 
                type: object
                properties: 
                  message: 
                    type: string
                    example: 'Unauthorized'
    post: 
      tags: 
        - Products
      description: 'Create a new product'
      requestBody: 
        required: true
        content: 
          application/json: 
            schema: 
              type: object
              properties: 
                name: 
                  type: string
                  example: 'Mouse'
                price: 
                  type: integer
                  example: 100
      responses: 
        '201': 
          description: 'A list of products will be returned'
          content: 
            application/json: 
              schema: 
                type: object
                properties: 
                  id:
                    type: string
                    example: 'asaddas'
                  name: 
                    type: string
                    example: 'Keyboard'
                  price: 
                    type: integer
                    example: 100
            application/xml: 
              schema: 
                type: object
                properties: 
                  name: 
                    type: string
                    example: 'Keyboard'
                  price: 
                    type: integer
                    example: 100
        '401':
          description: 'Unauthorized'
          content: 
            application/json: 
              schema: 
                type: object
                properties: 
                  message: 
                    type: string
                    example: 'Unauthorized'
```
But in this case we are copying and pasting the 401 in everywhere we can make a component and can reuse that.

First make a component for security
```yaml
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
```
And add this to the post
```yaml
post:
  security:
    - bearerAuth: []
  tags:
    - Products
  description: Create a new product
```
After adding this we will see the lock icon.
<br/><br/>
Now add production schema at the component
```yaml
components:
  securitySchemes:
    bearerAuth: []
  schemas:
    Product:
      type: object
      properties:
        id:
          type: string
          example: 'asdasddas'
        name:
          type: string
          example: "Keyboard"
        price:
          type: integer
          example: 100
```

Now we can use this schema at <br/>
`POST` product endpoint's `201` status code `$ref: '#/components/schemas/Product'`
```yaml
post:
  security:
    - bearerAuth: []
  tags:
    - Products
  description: Create a new product
  requestBody:
    required: true
  responses:
    '201':
      description: 'A list of product will be returned'
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Product'
        application/xml:
          schema:
            type: object
            properties:
              name:
                type: string
                example: "Keyboard"
              price:
                type: integer
                example: 100
```
`GET` product endpoint's `200` status code 
```yaml
responses:
  '200':
    description: 'A list of product will be returned'
    content:
      application/json:
        schema:
          type: array
          items:
            $ref: '#/components/schemas/Product'
```

### Add another component `401`
```yaml
  responses:
    '401':
      description: 'Unauthorized'
      content:
        application/json:
          schema:
            type: object
            properties:
              message:
                type: string
                example: 'Unauthorized'
              status:
                type: integer
                example: 401
              traceId:
                type: string
                example: 'asdasdsadas'
```
Now we can use that `POST` request `401`
```yaml
'401':
    $ref: '#/components/responses/401'
```

```yaml
tags:
  - Products
description: Create a new product
requestBody:
  content:
    application/json:
      schema:
        $ref: '#/components/schemas/Product'
    application/xml:
      schema:
        type: object
        properties:
          name:
            type: string
            example: 'Keyboard'
          price:
            type: integer
            example: 100
responses:
  '201':
    description: 'A list of product will be returned'
    content:
      application/json:
        schema:
          type: array
          items:
            $ref: '#/components/schemas/Product'
      application/xml:
        schema:
          type: object
          properties:
            name:
              type: string
              example: 'Keyboard'
            price:
              type: integer
              example: 100
  '401':
    $ref: '#/components/responses/401'

```

Also, can add those at `GET` request
```yaml
  '401':
    $ref: '#/components/responses/401'
```

```yaml
tags:
  - Products
description: Fetch all products including search, filter, and pagination
parameters:
  - name: search
    in: query
    required: true
    schema:
      type: string
      example: keyboard
  - name: page
    in: query
    required: true
    schema:
      type: integer
      example: 1
  - name: limit
    in: query
    required: true
    schema:
      type: integer
      example: 100
responses:
  '200':
    description: 'A list of product will be returned'
    content:
      application/json:
        schema:
          type: array
          items:
            $ref: '#/components/schemas/Product'
      application/xml:
        schema:
          $ref: '#/components/schemas/Product'
  '401':
    $ref: '#/components/responses/401'

```

### Complete example of YAML
#### API Documentation in OpenAPI 3.0

This YAML file provides a specification for a Product API using the OpenAPI 3.0 standard. It defines various endpoints
for managing products, including operations to create, read, update, and delete (CRUD) products. Key components include:

- **Info Section**: Contains metadata about the API, such as title, description, version, and contact information.
- **Servers Section**: Lists the servers where the API is hosted.
- **Paths Section**: Specifies the available endpoints (`/products`, `/products/{id}`) and the HTTP methods (GET, POST, 
  PUT, DELETE) that can be used, along with their parameters, request bodies, and responses.
- **Components Section**: Defines reusable objects such as security schemes, schemas (e.g., `Product`), and response
  templates (e.g., `Unauthorized`, `BadRequest`).

This structure helps in ensuring that the API follows a consistent and standardized format, making it easier for
developers to understand and use the API effectively.


```yaml
openapi: 3.0.0
info:
  title: Product API
  description: CRUD operations for managing products
  version: 1.0.0
  termsOfService: https://example.com/terms-of-service
  contact:
    name: Developer Name
    url: https://example.com/contact
    email: developer@example.com
  license:
    name: Apache 2.0
    url: https://www.apache.org/licenses/LICENSE-2.0.html
servers:
  - url: http://localhost:8000
paths:
  /products:
    get:
      tags: ['Product']
      summary: Get all products
      parameters:
        - in: query
          name: page
          required: true
          schema:
            type: integer
            example: 1
        - in: query
          name: limit
          required: true
          schema:
            type: integer
            example: 10
        - in: query
          name: search
          description: Search by product name
          schema:
            type: string
      responses:
        '200':
          description: A list of products retrieved successfully.
          content:
            application/json:
              schema:
                type: object
                properties:
                  pagination:
                    type: object
                    properties:
                      totalItems:
                        type: integer
                        example: 50
                      totalPages:
                        type: integer
                        example: 5
                      currentPage:
                        type: integer
                        example: 1
                  products:
                    type: array
                    items:
                      $ref: '#/components/schemas/Product'
                  links:
                    type: object
                    properties:
                      self:
                        type: string
                        example: "http://localhost:8000/products?page=1&limit=10"
                      next:
                        type: string
                        example: "http://localhost:8000/products?page=2&limit=10"
                      prev:
                        type: string
                        example: null
        '403':
          $ref: '#/components/responses/Forbidden'
        '500':
          $ref: '#/components/responses/InternalServerError'
    post:
      security:
        - bearerAuth: []
      tags: ['Product']
      summary: Create a new product
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Product'
      responses:
        '201':
          description: Product created successfully.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/StructuredResponse'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '400':
          $ref: '#/components/responses/BadRequest'
        '500':
          $ref: '#/components/responses/InternalServerError'
  /products/{id}:
    get:
      tags: ['Product']
      summary: Get a product by ID
      parameters:
        - name: id
          in: path
          required: true
          description: ID of the product to retrieve
          schema:
            type: integer
      responses:
        '200':
          description: Product retrieved successfully.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Product'
        '404':
          $ref: '#/components/responses/NotFound'
        '500':
          $ref: '#/components/responses/InternalServerError'
    put:
      tags: ['Product']
      security:
        - bearerAuth: []
      summary: Update an existing product
      parameters:
        - name: id
          in: path
          required: true
          description: ID of the product to update
          schema:
            type: integer
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Product'
      responses:
        '200':
          description: Product updated successfully.
        '401':
          $ref: '#/components/responses/Unauthorized'
        '400':
          $ref: '#/components/responses/BadRequest'
        '404':
          $ref: '#/components/responses/NotFound'
        '500':
          $ref: '#/components/responses/InternalServerError'
    delete:
      security:
        - bearerAuth: []
      tags: ['Product']
      summary: Delete an existing product
      parameters:
        - name: id
          in: path
          required: true
          description: ID of the product to delete
          schema:
            type: integer
      responses:
        '200':
          description: Product deleted successfully.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/StructuredResponse'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '404':
          $ref: '#/components/responses/NotFound'
        '500':
          $ref: '#/components/responses/InternalServerError'
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
  schemas:
    Product:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        description:
          type: string
        price:
          type: number
        quantity:
          type: integer
      required:
        - name
        - price
    StructuredResponse:
      type: object
      properties:
        success:
          type: boolean
        message:
          type: string
        data:
          oneOf:
            - $ref: '#/components/schemas/Product'
            - type: array
              items:
                $ref: '#/components/schemas/Product'
  responses:
    Unauthorized:
      description: Unauthorized
      content:
        application/json:
          schema:
            type: object
            properties:
              status:
                type: number
                example: 401
              message:
                type: string
                example: 'Unauthorized: Please authenticate to access this resource.'
              links:
                type: string
                example: 'https://example.com/api/v1/auth/login'
    BadRequest:
      description: Bad Request
      content:
        application/json:
          schema:
            type: object
            properties:
              status:
                type: number
                example: 400
              message:
                type: string
                example: 'Bad Request: The request parameters are invalid.'
    NotFound:
      description: Not Found
      content:
        application/json:
          schema:
            type: object
            properties:
              status:
                type: number
                example: 404
              message:
                type: string
                example: 'Not Found: The requested resource was not found.'
    InternalServerError:
      description: Internal Server Error
      content:
        application/json:
          schema:
            type: object
            properties:
              status:
                type: number
                example: 500
              message:
                type: string
                example: 'Internal Server Error: Please try again later.'
    Forbidden:
      description: Forbidden
      content:
        application/json:
          schema:
            type: object
            properties:
              status:
                type: number
                example: 403
              message:
                type: string
```

If we give this yaml to any API management tools it can generate collection, test, mocking for us. Using swagger we can
generate client from this.

# REST API Security
## What is API Security?

API security is the practice of **preventing and mitigating attacks** that originate at the API level, and it is a 
crucial pillar of any organization's overall security strategy.

APIs not only enable users to interact with applications, but also facilitate communication between their **underlying
internal services**—many of which transmit or store sensitive data. An insecure API can therefore provide an entry point 
for attackers and seriously compromise an application’s security posture.

## Threats & Vulnerabilities
### Poor Security Hygiene
This is the most basic category of security vulnerabilities, but it is also one of the most important. Failure to 
maintain good security hygiene can lead to major breaches that could have easily been prevented. For instance, teams
should ensure that their tokens and API keys are not hard-coded or included in their API client libraries' **source 
code, Git history, or documentation**. They should also use Transport Layer Security (TLS) to encrypt all API
communications, whether the API is delivered as an HTTP-based REST API or a Protobuf-based gRPC API.

### Authentication and Authorization Vulnerabilities
APIs often give users different permissions depending on their role. Authentication and authorization issues therefore
pose a significant security threat because they enable users to perform actions and access data that should be 
restricted.

There are many ways for these issues to enter an API. For instance, organizations might fail to perform method-level
authorization checks to confirm that the object being read or written to is within the user's authorization scope. 
Additionally, if authorization is scoped by API endpoints but individual objects have more granular permissions, there
is a risk that improperly-checked permissions could result in access violations (this is related to the following
vulnerability: lack of read and write granularity). Teams might also fail to use cryptographic methods, such as public
key encryption, in user authentication workflows, which introduces the risk of spoofing.


### Lack of Read and Write Granularity
Data objects have many properties—some of which are sensitive, and some of which are not. For example, an instance of a 
User object may have a username property (which is not sensitive) and a password property (which is sensitive). 
Sometimes, developers fail to protect sensitive properties on the server side and instead rely on clients to filter
them out, which leaves them accessible to malicious parties. This vulnerability is known as excessive data exposure.

Similarly, users should not be able to update internal properties (such as user.id) and permission-related properties
(such as user.is_admin). Unfortunately, developers sometimes introduce a mass assignment vulnerability by programming
an API to simultaneously assign every piece of data it receives from a client. Instead, developers should implement
logic that checks each property and only assigns a new value to properties that can be updated by the user in question.

### Failure to implement quotas and throttling
Quotas and throttling are intended to limit the number of requests to a particular service in order to conserve 
computational resources and ensure high availability. APIs that don’t implement quotas or throttling can leave 
applications vulnerable to brute force attacks, in which an attacker systematically guesses a password in order to gain
access, and Denial of Service (DoS) attacks, in which an attacker floods an API with traffic in order to exhaust the 
resources of a downstream service and take it offline.

### Improperly set or missing HTTP headers
When a web client sends an initial request to an API, the response should have appropriate HTTP headers, including 
security headers. Security headers give the browser instructions for safely interacting with the API. For instance,
HTTP Strict Transport Security (HSTS) headers can be used to enforce valid certificates and TLS communication, and 
Access-Control and Cross-Origin Resource Policy (CORP) headers can prevent a valid client from being misused to perform 
API requests on behalf of potentially malicious third parties.

Additionally, improperly set headers, such as the Cache-Control and Vary headers, can result in private data leaking to 
other API clients if proxies or load balancers are used in front of the web server.

### Failure to perform input validation, sanitization, and encoding at the method level
API injection occurs when an attacker sends malicious code through an API, often within a request's parameters or body. 
This malicious code might be executed on the server side (for example, in a SQL query or during the deserialization of
data), or it may be executed on the client side if it gets embedded in HTML content and it was not properly escaped
beforehand. This malicious code might then change permissions or database records that should be protected, or even 
result in arbitrary remote code execution (RCE).

It's therefore crucial for developers to leverage Web Application Firewalls (WAFs) and perform input validation, 
sanitization, and encoding at the method level, which will prevent any malformed or unsafe data from entering the 
workflow. Libraries like OWASP's ESAPI can help reduce the occurrence of this vulnerability.

### How to Secure REST API?
- **Authentication**
- **Access Control / Authorization**
- **Input Validation & Sanitization**
- **Rate Limiting**
- **Security Headers**
- **Logging & Continuous Monitoring**

# Authentication
## REST API Authentication
Authentication is the process of verifying the identity of clients accessing the API. It ensures that only authorized 
users or applications can access protected resources. Common authentication mechanisms for REST APIs include:
- **Basic Authentication**: Username & password.
- **JSON Web Tokens (JWT)**: Tokens containing digitally signed claims that can be used to authenticate and authorize 
  clients.
- **API keys**: Unique identifiers issued to clients for authentication purposes.
- **OAuth 2.0**: A framework for delegated authorization, allowing clients to obtain access tokens to access protected
  resources on behalf of users.

## Basic Authentication/Basic Token Authentication
Basic Authentication is a simple method for enforcing access controls to web resources. It's based on the HTTP protocol
and involves sending a username and password with each request, encoded in Base64 format. Here's how it works:
- **Client Request**: When a client (such as a web browser or API client) makes a request to a server that requires 
  authentication, it includes the username and password in the HTTP headers using the Authorization header. The 
  credentials are encoded in Base64 format, separated by a colon (`username:password`).
- **Server Verification**: The server decodes the Base64-encoded credentials and verifies them against a user database 
  or directory service. If the credentials are valid, the server grants access to the requested resource; otherwise, it
  returns a 401 Unauthorized response.
- **Authentication Challenges**: If the server returns a 401 Unauthorized response, it may include a WWW-Authenticate 
  header in the response to indicate the authentication scheme required and any additional parameters. The client can
  then retry the request with the appropriate credentials.

### Advantages of Basic Authentication

- **Simplicity**: Basic Authentication is straightforward to implement and understand, requiring minimal configuration
  and setup on both the client and server sides.
- **Compatibility**: Basic Authentication is supported by most web browsers and HTTP client libraries, making it widely
  compatible with various client and server technologies.
- **Statelessness**: Basic Authentication is stateless, meaning that each request contains all the necessary 
  authentication information. There's no need for server-side session management, which simplifies scalability and load
  balancing.

### Disadvantages of Basic Authentication

- **Weak Security**: Basic Authentication sends credentials in plaintext, encoded with Base64, over the network. While 
  Base64 encoding provides minimal obfuscation, it does not encrypt the credentials, making them vulnerable to 
  interception and exploitation by attackers.
- **No Built-in Encryption**: Basic Authentication does not provide built-in encryption or protection against 
  eavesdropping, man-in-the-middle attacks, or replay attacks. As a result, it's not suitable for securing sensitive or 
  confidential information without additional encryption mechanisms (e.g., SSL/TLS).
- **Credential Exposure**: Because credentials are sent with every request, they are exposed to potential interception 
  or capture by network sniffers, compromising the security of the authentication process.

## JSON Web Token (JWT)

JWT stands for JSON Web Token. It's an RFC 7519 standard for securely transmitting JSON information between parties. 
Used in web apps for authentication and authorization, including securing REST APIs.

**Structure:** JWTs have three parts: header, payload, and signature. Each part is base64url encoded and combined. The
header specifies token type and signing algorithm. Payload holds user/entity claims. Signature, generated using a secret
or key pair, verifies token integrity.

- **Authentication:** When a user logs in or authenticates, the server issues a JWT containing information about the 
  user (e.g., user ID, roles, permissions). The JWT is then sent to the client, typically in the form of an HTTP header 
  (e.g., Authorization header) or a cookie.
- **Authorization:** For REST API requests, clients include the JWT in headers for authentication. Servers verify JWTs
  by checking signatures and claims to authenticate and authorize users for requested resources.
- **Statelessness:** JWTs are stateless, meaning that all the necessary information for authentication and authorization
  is contained within the token itself. This eliminates the need for the server to maintain a session state, making JWTs
  suitable for use in stateless architectures such as RESTful APIs.

### Advantages
- **Statelessness:** Since JWTs contain all the necessary information, REST APIs can be designed to be stateless, 
  improving scalability and reducing server-side storage requirements.
- **Interoperability:** JWTs are standardized and widely supported, making them interoperable across different platforms,
  frameworks, and programming languages.
- **Security:** JWTs can be digitally signed and encrypted, providing integrity and confidentiality for the transmitted 
  data. They can also be validated cryptographically, ensuring that only trusted tokens are accepted.
- **Performance:** JWTs are compact and can be transmitted efficiently over HTTP headers, reducing network overhead and
  latency.

### Disadvantages

- **Token Size:** JWTs can become large if they contain a lot of claims or attributes, which may increase network 
  bandwidth usage and payload size.
- **Token Lifetime:** JWTs have a fixed lifetime, and once issued, they cannot be revoked or invalidated. This can pose 
  security risks if a token is compromised or stolen.
- **Complexity:** Implementing JWT-based authentication and authorization requires careful consideration of security 
  best practices, including key management, token validation, and handling of expired or invalid tokens.

## API Key

An API key is a unique identifier or token that is used to authenticate and authorize clients accessing a REST API. It
serves as a simple and lightweight mechanism for controlling access to API resources by identifying and tracking the 
usage of individual clients or applications.

- **Generation**: API keys are typically generated by the API provider and issued to clients or applications that need
  to access the API. Each API key is a randomly generated string of characters that uniquely identifies the client or
  application.
- **Inclusion in Requests**: Clients include their API key in the requests they make to the API, typically as a query
  parameter, header, or part of the request payload. The API key acts as a credential that the server can use to 
  authenticate and authorize the client.
- **Validation**: Upon receiving a request, the API server validates the API key provided by the client to ensure that
  it is valid and authorized to access the requested resource. This may involve checking the API key against a database
  or whitelist of allowed keys maintained by the server.
- **Access Control**: Once the API key is validated, the server can enforce access control policies to determine whether
  the client is authorized to access the requested resource. This may include restricting access based on the client’s 
  identity, permissions, or usage limits associated with the API key.
- **Monitoring and Rate Limiting**: API keys can be used to track and monitor the usage of individual clients or 
  applications accessing the API. This allows the API provider to enforce rate limits, quotas, and usage policies to 
  prevent abuse and ensure fair access to API resources for all clients.

### Advantages

- **Simplicity**: API keys are easy to generate, distribute, and manage, making them a simple and lightweight 
  authentication mechanism for securing APIs.
- **Scalability**: API keys scale well with the number of clients or applications accessing the API, as each client can
  be issued a unique key that can be tracked and managed independently.
- **Granular Control**: API keys allow for granular control over access to API resources, as access can be restricted or
  revoked on a per-key basis.
- **Low Overhead**: API keys have minimal overhead in terms of network bandwidth and processing resources, as they are 
  typically included in the request headers or URL query parameters.

### Disadvantages

- **Limited Security**: API keys provide only basic authentication and authorization, and they may not be suitable for 
  securing sensitive or critical API endpoints or data.
- **Key Management**: Managing and securing API keys can be challenging, particularly in distributed or multi-client
  environments, as keys may need to be rotated, revoked, or updated periodically.
- **Lack of User Identity**: API keys are tied to clients or applications rather than individual users, making it 
  difficult to enforce user-level access control or track user activity within the API.

## OAuth 2.0

OAuth 2.0 is an authorization framework that enables secure access to resources on behalf of a user without sharing 
their credentials. It allows users to grant third-party applications limited access to their resources (such as profile
information, photos, or documents) hosted on a server, without revealing their passwords. OAuth 2.0 is widely used in
modern web and mobile applications to enable secure authentication and authorization flows.

### OAuth 2.0 Grant Types

In OAuth 2.0, grants are the set of steps a Client has to perform to get resource access authorization. The authorization
framework provides several grant types to address different scenarios:

- Authorization Code
- Implicit Grant
- Authorization Code Grant with Proof Key for Code Exchanges (PKCE)
- Resource Owner Credentials
- Client Credentials
- Device Authorization Flow
- Refresh Token Grant

#### Advantages

- **User Privacy**: OAuth 2.0 allows users to grant limited access to their resources without sharing their credentials
  with third-party applications, enhancing user privacy and security.
- **Scalability**: OAuth 2.0 is scalable and supports various grant types and authentication scenarios, making it 
  suitable for a wide range of use cases, including web, mobile, and IoT applications.
- **Flexibility**: OAuth 2.0 provides flexibility in terms of authentication and authorization mechanisms, allowing 
  developers to choose the appropriate grant type and authentication flow based on their application's requirements.
- **Standardization**: OAuth 2.0 is a widely adopted and standardized protocol, with extensive support in libraries, 
  frameworks, and platforms, making it easier to implement and integrate into existing systems.

[Link to RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749)

#### Disadvantages

- **Complexity**: OAuth 2.0 can be complex to implement and understand, particularly for developers new to
  authentication and authorization concepts. The various grant types, flows, and security considerations require careful
  consideration and implementation.
- **Token Management**: Managing access tokens, refresh tokens, and their lifecycle can be challenging, particularly in 
  distributed or multi-client environments. Developers must implement proper token storage, validation, and revocation 
  mechanisms to ensure security and compliance.
- **Security Risks**: Improperly implemented OAuth 2.0 flows or misconfigurations can introduce security risks such as 
  token leakage, authorization code interception, or token replay attacks. Developers must follow security best practices
  and guidelines to mitigate these risks.


# Authentication Best Practise
## Max Retry & Jail in Login

**Max Retry:** The "Max Retry" feature limits the number of login attempts that a user can make within a specified time
period. After a certain number of failed login attempts, the user is locked out of their account for a specified period
of time, typically several minutes or hours. This helps to prevent brute-force attacks, where an attacker attempts to 
guess a user's password by making repeated login attempts. By limiting the number of attempts, the system can slow down
or prevent such attacks.

**Jail:** The "jail" feature involves blocking IP addresses or user accounts that have exceeded the maximum number of 
failed login attempts within a certain time period. The blocked IP addresses or user accounts are prevented from 
attempting further logins for a specified period of time, typically several minutes or hours. This helps to prevent 
brute-force attacks, and also provides a mechanism to prevent malicious users from repeatedly attempting to access an 
account or system.

## Sensitive Data Encryption
Encryption is a process of converting plain text data into a ciphertext that can only be deciphered by someone who has
the decryption key. This makes it difficult for attackers to access sensitive data, even if they are able to intercept
it or gain unauthorized access to it.

To encrypt sensitive data, you can use encryption algorithms such as AES or RSA, along with a strong key management 
system to ensure that keys are securely stored and managed. Additionally, you can implement other security measures 
such as access controls, firewalls, and intrusion detection systems to further protect sensitive data.

**Note:** Avoid using Basic Authentication

# Access Control
## Access Control Best Practices
- **Implement RBAC:** Role Based Access Control
- **Throttle Requests:** Limiting requests through throttling is important to prevent DDoS attacks and brute force 
  attacks. DDoS attacks overwhelm the server with too many requests, while brute force attacks try to guess user 
  credentials through multiple login attempts.
- **Use HTTPS:** Ensure that your API server uses HTTPS instead of HTTP. HTTPS is a secure protocol that encrypts data
  in transit, making it difficult for attackers to intercept and read sensitive information. To implement HTTPS, you
  need to obtain an SSL/TLS certificate and configure your server to use HTTPS.
- **Use UUID over incremental Id**

# Input Data Validation
- Use proper HTTP methods
- Validate content-type on request header
- Validate and sanitize request body
- Avoid sending sensitive data in the query parameters
- Use only server-side encryption

# Security Headers
- **Content-Security-Policy**: A powerful allow-list of what can happen on your page which mitigates many attacks
- **Cross-Origin-Opener-Policy**: Helps process-isolate your page
- **Cross-Origin-Resource-Policy**: Blocks others from loading your resources cross-origin
- **Origin-Agent-Cluster**: Changes process isolation to be origin-based
- **Referrer-Policy**: Controls the Referrer header
- **Strict-Transport-Security**: Tells browsers to prefer HTTPS
- **X-Content-Type-Options**: Avoids MIME sniffing
- **X-DNS-Prefetch-Control**: Controls DNS prefetching
- **X-Download-Options**: Forces downloads to be saved (Internet Explorer only)
- **X-Frame-Options**: Legacy header that mitigates clickjacking attacks
- **X-Permitted-Cross-Domain-Policies**: Controls cross-domain behavior for Adobe products, like Acrobat
- **X-Powered-By**: Info about the web server. Remove because it could be used in simple attacks
- **X-XSS-Protection**: Legacy header that tries to mitigate XSS attacks, but makes things worse, so Helmet disables it

# API Management
- **API Design**: API management starts with designing APIs that meet the needs of consumers while adhering to best
  practices, standards, and design principles. This involves defining API specifications, endpoints, data formats, 
  authentication mechanisms, and documentation.
- **API Development**: After designing APIs, the next step is to develop them according to the specified design. API 
  management platforms often provide tools and frameworks to simplify API development, automate code generation, and 
  ensure consistency and quality.
- **API Deployment**: Once APIs are developed, they need to be deployed in a reliable and scalable environment. API 
  management solutions offer deployment options for hosting APIs on-premises, in the cloud, or in hybrid environments,
  ensuring high availability and performance.
- **API Security**: API management involves implementing robust security measures to protect APIs from unauthorized 
  access, data breaches, and cyber threats. This includes authentication, authorization, encryption, and other security
  mechanisms to ensure that only authorized users and applications can access protected resources.
- **API Monitoring and Analytics**: API management platforms provide monitoring and analytics tools to track API usage, 
  performance, availability, and other key metrics. This allows organizations to gain insights into how APIs are being 
  used, identify issues or bottlenecks, and optimize API performance and scalability.
- **API Documentation and Developer Portals**: API management solutions include developer portals and documentation 
  tools to facilitate API discovery, onboarding, and usage. This includes providing comprehensive documentation, code 
  samples, tutorials, and sandbox environments to help developers understand and integrate with APIs more effectively.
- **API Lifecycle Management**: API management involves managing the entire lifecycle of APIs, from design and 
  development to retirement. This includes versioning, deprecation, backward compatibility, and migration strategies to
  ensure seamless evolution of APIs over time.
- **API Monetization and Billing**: For organizations offering APIs as products or services, API management includes 
  capabilities for monetization, billing, and revenue generation. This may involve usage-based pricing models, 
  subscription plans, and billing integrations to monetize API usage effectively.

## API Management Platform

- **API portal**: Providers access, control, testing, and documentation.
- **API lifecycle manager**: Controls the journey from design through retirement.
- **API policy manager**: Controls the policies that help guarantee the security of the API.
- **API analytics**: Reveals metrics that can be used to make informed business decisions.
- **API gateway**: Connects API providers and consumers.

**API management** is the organized control of APIs throughout their lifecycle, including design, deployment, security,
monitoring, and monetization.

We have [kong](https://konghq.com/) which we can use as API gateway if we use enterprise version we can get all the
features for API management. Also have [apigee](https://cloud.google.com/apigee) from Google, [apiman](https://www.apiman.io/)
provide API gateway, developer portal etc, also azure, aws has there api. We have many api manage tools they manage
gateway to governace, security, rate limiting, monitoring, analytics, developer portal, etc.

Insomnia, postman, swaggerhub, api dog are some of the tools for API documentation.

## Agent and Proxy-based API Management

There are two popular API management models available:

- **Agent-Based**
- **Proxy-Based**

### Agent-Based
In agent-based API management, lightweight agents or modules are deployed alongside the applications or services that
expose APIs. These agents intercept and control API traffic at the application level, allowing for fine-grained 
management and enforcement of policies.

Key characteristics of agent-based API management include:
- **In-process Integration**: Agents are integrated directly into the application code or runtime environment, allowing
  them to intercept API calls within the application process. This enables deep visibility and control over API traffic, 
  including access control, security enforcement, and traffic management.
- **Low Overhead**: Agents typically have minimal overhead, as they are tightly integrated with the application and 
  operate within the same process. This makes them suitable for environments where performance and resource utilization
  are critical considerations.
- **Granular Control**: Agent-based API management provides granular control over API traffic, allowing for fine-grained 
  policy enforcement based on factors such as user identity, API key, rate limits, and content types.
- **Dynamic Policy Enforcement**: Agents can dynamically apply policies and configurations based on runtime conditions, 
  such as traffic patterns, resource availability, and security threats. This enables adaptive and responsive API 
  management in dynamic environments.
- **Application-level Insights**: Since agents operate within the application context, they have access to rich 
  application-level insights and telemetry data. This allows for comprehensive monitoring, logging, and analysis of API
  traffic and application behavior.

### Proxy-Based API Management
In proxy-based API management, a dedicated proxy server or gateway sits between API consumers and providers, acting as 
an intermediary for all API traffic. The proxy server intercepts and forwards API requests and responses, applying 
policy enforcement and management functions.

#### Key characteristics of proxy-based API management include:
- **Separation of Concerns**: Proxy servers provide a centralized point of control and enforcement for API management 
  functions, separate from the application logic. This allows for the decoupling of concerns and simplifies the
  management, configuration, and scaling of API infrastructure.
- **Scalability and Load Balancing**: Proxy-based API management solutions can scale horizontally by deploying multiple
  instances of the proxy server and load-balancing traffic across them. This enables high availability, fault tolerance, 
  and scalability for API traffic.
- **Protocol Agnosticism**: Proxy servers can handle API traffic across various protocols and communication channels, 
  including HTTP, HTTPS, WebSockets, and TCP/IP. This flexibility makes them suitable for managing diverse API 
  ecosystems and integration scenarios.
- **Security Gateway**: Proxy servers often include built-in security features such as authentication, authorization,
  encryption, and threat protection. They act as a security gateway for API traffic, protecting against common security
  threats and vulnerabilities.
- **Policy Enforcement**: Proxy-based API management solutions enable centralized policy enforcement and management, 
  allowing administrators to define and enforce access control, rate limiting, caching, and transformation policies
  across all APIs.


## API Developer Portals

An API developer portal, also known as an API portal or developer hub, is a centralized online platform that serves as a 
gateway for developers to discover, explore, and consume APIs (Application Programming Interfaces). It provides a range 
of resources and tools to facilitate API adoption and integration. The main features of an API developer portal include:

- **API Documentation**: Detailed documentation describing the functionality, endpoints, parameters, request and response 
  formats, authentication methods, and usage examples of the APIs available on the platform.
- **Interactive API Explorer**: A tool that allows developers to interactively explore and test APIs directly within the
  portal. It typically provides a user-friendly interface for making API requests, viewing responses, and experimenting 
  with different parameters.
- **Developer Onboarding**: Registration and authentication mechanisms for developers to sign up and obtain access to the 
  APIs. This may include account creation, API key generation, and agreement to terms of service.
- **API Key Management**: Tools for managing API keys, authentication tokens, and access permissions. Developers can 
  obtain and manage their API keys through the portal, and administrators can control access levels and usage quotas.
- **Community Engagement**: Forums, discussion boards, chat channels, and support resources for developers to ask 
  questions, share knowledge, collaborate, and provide feedback. This fosters a sense of community and encourages 
  collaboration among developers.
- **Tutorials and Guides**: Educational resources, tutorials, guides, and best practices to help developers get started 
  with using the APIs effectively. This may include step-by-step tutorials, code samples, and use case examples.


# Source
* [Day 3: OpenAPI Specification, REST API Security, and Management | REST API Design | Stack Learner](https://www.youtube.com/watch?v=c_VvDiFgwhk&list=PL_XxuZqN0xVAWGDKIzcn6NWikVkljJQZc&index=8)
