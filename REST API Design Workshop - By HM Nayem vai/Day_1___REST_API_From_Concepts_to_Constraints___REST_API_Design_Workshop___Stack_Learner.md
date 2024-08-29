# Project Proposal

**Project Name**: ECommerce Solution  
**Mission**: Create a digital presence of our company

## Problem Statement
The Z company sells shoes offline. Now they want to create an online store and build a D2C brand. They need web and 
mobile applications for the end user, an admin dashboard, and CMS for administration purposes.

## Functional Requirements
- **Product Catalog**: The product catalog feature will enable users to browse through a comprehensive list of items 
  available for purchase within the system.
- **Multiple Categories**: The multiple categories functionality will organize products into distinct categories and 
  subcategories based on their attributes, characteristics, or types.
- **Instant Search**: Instant search functionality will provide users with real-time search results as they type their 
  query into the search bar. It will quickly retrieve relevant products based on their names, descriptions, or 
  attributes, displaying matching results dynamically.
- **Shopping Cart**: The shopping cart feature will allow users to add selected items to their virtual cart for
  subsequent purchase. Users can review the contents of their cart, modify quantities, or remove items as needed before
  proceeding to checkout.
- **Digital Payments**: Digital payments functionality will enable users to complete transactions securely and
  conveniently using electronic payment methods. It will support various payment options such as credit cards, debit 
  cards, mobile wallets, or online payment gateways.

## Deliverables
A Scalable REST API


# API
The API is a contract between the client and the server. It defines how the client can interact with the server. The API
is a set of rules that the client must follow to communicate with the server. The API specifies the endpoints, methods,
parameters, and responses that the client can use to interact with the server. The API is a bridge between the 
client(consumer) and the server(provider), allowing them to communicate with each other. Sometime, we use that API to
communicate between two different services and clients.

- Provider: The server that provides the API.
- Consumer: The client that consumes the API.

# API Architectural Styles

# REST
**REST** stands for **Representational State Transfer** and **API** stands for **Application Programming Interface**.
It's an architectural style for networked applications, defining principles for resource identification, addressing, and
data exchange between clients and servers via HTTP.

## Representational State

In the context of REST Architecture, a resource is any data object or entity that can be accessed or manipulated through
the REST API. And "Representational State" means it represents the current state of your Resources. Now this
representation can be **JSON**, **XML**, or any other format.

```json
{
  "id": 123,
  "title": "To Kill a Mockingbird",
  "author": "Harper Lee",
  "genre": "Fiction",
  "price": 10.99,
  "availability": true
}
```

REST APIs have six architectural constraints those are:
## Client-Server
The system should be divided into client and server components that are independent of each other. This separation 
allows for improved scalability and enables the client and server to evolve independently.

## Stateless
Each request from a client to the server must contain all the information necessary to understand and fulfill the 
request. The server should not maintain any client state between requests. If you manage user sessions in a RESTful API,
you would essentially be introducing statefulness into your architecture, which goes against the statelessness 
constraint of REST. This constraint simplifies server design, improves reliability, and supports better scalability.

Imagine an Admin who wants to create a new product. They use a `/products` protected endpoint and send the needed 
product details. If we control who can access this endpoint based on user sessions and only allow the admin, it goes
against the principles of RESTfulness.

* Product Details Sent:
  * product_name
  * price
  * sku
  * ...
  * token
* Endpoint:
  * `/products`f
* Server Consideration:
  * Avoid user sessions for determining access, as it goes against RESTfulness principles.

## Cacheable
Responses from the server should be explicitly labeled as cacheable or non-cacheable. Caching can improve performance 
and reduce latency by allowing clients to reuse previously fetched responses.

The `Cache-Control` header is used to specify caching directives that control how responses are cached and served. 
Directives include:

- **public**: Indicating that the response can be cached by any cache.
- **private**: Indicating that the response is intended for a single user and should not be cached by shared caches.
- **max-age**: Indicating the maximum time a response can be cached.
- **no-cache**: Indicating that the response should not be served from the cache without validation.

#### Example Usage:
Client requests `/products`:
```http
-H : Cache-Control, "public, max-age=5"
```

### Cache-Control Example Scenarios

#### Scenario 1: Private Cache
- **Client** sends a request through the **Gateway** to the **Server**.
- **Server** responds with:
  ```http
  -H : Cache-Control, "private, max-age=5"
  ```
- The response is intended for a single user and should not be stored in a shared cache.

#### Scenario 2: Public Cache
- **Client** sends a request through the Gateway to the Server.
- **Server** responds with:
  ```
  -H : Cache-Control, "public, max-age=5"
  ```
- The response can be stored in a shared cache and reused by multiple clients.


## Uniform Interface
The interface between the client and server should be uniform and consistent. This constraint is further divided into 
four sub-constraints:

### Identification of Resources

Each resource in the system should be uniquely identifiable using a URI (Uniform Resource Identifier). Each resource in
the system should be uniquely identifiable using a URI (Uniform Resource Identifier).

**Example:** In a product catalog application, each product is a resource that can be uniquely identified by a URI. For
instance:
- **URI for a specific product:** `/products/123`
- **URI for all products:** `/products`

### Manipulation of Resources through Representations
Clients manipulate resources through representations, such as JSON or XML, rather than directly accessing the resource
itself. This enables decoupling between clients and servers.

#### Example
When a client wants to add a new product to the catalog, it sends a POST request with a representation of the new 
product in JSON format. Similarly, when retrieving a product, the server responds with a representation of the product
in JSON or XML format.

```http
POST /products
Content-Type: application/json

{
  "name": "Product Name",
  "description": "Description of the product.",
  "price": 49.99,
  "sku": "PRODUCT-05"
}
```

### Self-Descriptive Messages
Each message exchanged between client and server should include metadata that describes how to process the message. This
metadata can include HTTP headers, status codes, and media types.

#### Example
When a client requests a product, the server responds with metadata such as the HTTP status code, content type, and any
caching directives.

**Response for retrieving a product:**

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 123,
  "name": "Product Name",
  "description": "Description of the product.",
  "price": 49.99,
  "sku": "PRODUCT-05"
}
```

### Hypermedia as the Engine of Application State (HATEOAS)
The server should include hyperlinks in responses to guide the client through the application's state transitions. This 
constraint enables discoverability and allows clients to navigate the API dynamically.
#### Example
When a client retrieves a product, the server includes hyperlinks to related resources, such as links to update or
delete the product, and links to navigate to other parts of the API.

**Response for retrieving a product:**

```json
{
  "id": 123,
  "name": "Product Name",
  "description": "Description of the product.",
  "price": 49.99,
  "sku": "PRODUCT-05",
  "links": {
    "self": "/products/123",
    "addtocart": "/addtocart"
  }
}
```

## Layered System
The architecture should be composed of multiple layers, with each layer having a specific responsibility. This 
constraint promotes modularity, encapsulation, and scalability by allowing intermediaries such as proxies and gateways 
to be inserted between clients and servers.

### Benefit:
- A layered system makes the architecture simpler by hiding the complicated parts.
- The architecture can change as needs change.
- Clients don't have to worry about how requests are spread out.


## Code on Demand (Optional)


# SOAP

# Webhook

# GraphQL

# gRPC


# What is API Design?

# Why Designing API is Important?

### Interoperability
Well-designed APIs define clear contracts and standards for data exchange, enabling interoperability between diverse
platforms, programming languages, and technologies.

### Abstraction
APIs abstract the underlying complexities of software components, exposing only the necessary functionality and data to 
consumers.

### Reusability
Developers can leverage existing APIs to access common functionalities such as authentication, data storage, or payment
processing, rather than reinventing the wheel.

### Adaptability
Well-designed APIs are flexible and adaptable, allowing them to evolve over time in response to changing requirements, 
technologies, and business needs.

### Developer Experience
Well-designed APIs are intuitive, consistent, and well-documented, making it easier for developers to understand, 
integrate, and use the provided functionality.

### Security & Reliability
APIs should incorporate security best practices. Well-designed APIs also handle errors gracefully and provide
informative error messages.

## API Management
We can [oocommerce](https://woocommerce.github.io/woocommerce-rest-api-docs/) rest api documentation how they manage
their API for each module like products, orders, customers, etc. they have separate endpoints. And they have documentation
for each endpoint. They have a versioning system for their API. They have rate limiting for their API etc.

[Github](https://docs.github.com/en/rest?apiVersion=2022-11-28) also has a well-managed API. 

We should add expected 
* response 
  * status code
  * example response
  * response schema
  * response headers
    * pagination
    * rate limiting
    * token
* request body
* available methods
* api version

etc. in our API documentation.

# API First World
API-first, also called the API-first approach, prioritizes APIs at the beginning of the software development process, 
positioning APIs as the building blocks of software. API-first organizations develop APIs before writing other code,
instead of treating them as afterthoughts. This lets teams construct applications with internal and external services
that are delivered through APIs.

# API Stackholders
## API Producer

## API Consumer
### Different Types of API Consumers
#### Public
Public APIs are accessible to external developers and the general public. Public APIs are typically well-documented, 
have usage limits or pricing tiers.

#### Private
Private APIs are designed for internal use within an organization and are not exposed to external developers or users.

#### Partner
Partner APIs are specifically designed for use by trusted external partners, collaborators, or affiliates.

# API Lifecycle Overview

Open API Specification (OAS) is a standard for defining RESTful APIs. It provides a way to describe the API's endpoints,
parameters, responses, and other details in a machine-readable format. OAS is often used to generate API documentation,
client SDKs, and server stubs automatically. We can produce postman test runner, collection runner, mock server, etc. 
from OAS file easily.

## Producer Lifecycle
#### Design
Planning and designing the API.

#### Develop
Writing the code and building the API.

#### Test
Testing the API to ensure it works as expected.

#### Secure
Ensuring the API is secure and protected.

#### Deploy
Deploying the API to make it available for use.

#### Observe
Monitoring the API in production to ensure it operates smoothly.

#### Distribute
Making the API available to consumers.

#### Define
Defining the API's specifications and endpoints.

## Consumer Lifecycle
#### Discover
Finding and identifying useful APIs.

#### Observe
Monitoring the usage and performance of the API.

#### Deploy
Integrating the API into the consumer's system.

#### Test
Testing the API integration to ensure it works properly.

#### Integrate
Incorporating the API into existing workflows or applications.

#### Evaluate
Assessing the performance and value of the API.

# API Design 
API design is the process of making intentional decisions about how an API will expose data and functionality to its 
consumers. A successful API design describes the API endpoints, methods, and resources in a standardized specification 
format.

## Process:

**Step 1**: Determine what the API is intended to do  
**Step 2**: Define the API contract with a specification  
**Step 3**: Validate your assumptions with mocks and tests  
**Step 4**: Document the API


# A Good Looking API
## Swagger
We can define API's before writing code using Swagger. Any team can access that and do there work. 

Let's say we have product tag/category and it has multiple api's.
In each api we can define
* One line details of that API
* Sections
  * Parameters
    * Query Parameters
    * Path Parameters
    * We can mark required parameters
    * We can define default values, data types, etc.
  * Responses
    * We can define responses, schema, headers for different status codes
  * Request Body
    * We can define request body, schema, etc.
    * We can define required fields, data types of fields, etc.
* We can define security requirements
  * Authentication, Authorization, etc.


# REST API Maturity 
## REST API Richardson Maturity Model
### Level 0: The Swamp of POX(Not a RESTful)
POX (Plain Old XML) is an architectural style for designing APIs where XML (eXtensible Markup Language) is primarily
used as both the data format and the means of communication between clients and servers. POX predates more modern
RESTful approaches and is often associated with RPC (Remote Procedure Call)-style architectures.

Here are some key characteristics of POX APIs along with examples:
- **Use of XML for Communication**: In POX APIs, XML is used as the sole format for representing data exchanged between
  clients and servers. This means that requests sent by clients and responses returned by servers are typically XML
  documents.

  ```xml
  <request>
    <operation>get</operation>
    <resource>user</resource>
    <id>123</id>
  </request>
  ```
- **Use of HTTP as Transport**: While POX APIs primarily rely on XML for data exchange, they typically use HTTP as the
  underlying transport protocol. However, the usage of HTTP may not adhere to the principles of REST, and HTTP methods
  might not be utilized properly.
- **RPC-like Interaction**: POX APIs often exhibit RPC-like interaction patterns, where clients invoke remote procedures
  or methods exposed by the server. This can lead to tight coupling between clients and servers, as clients need to have
  prior knowledge of the API methods and parameters. <br/><br/>

  **Example RPC-like Request:**
  ```xml
  <methodCall>
    <methodName>getUser</methodName>
    <params>
      <param>
        <value><string>123</string></value>
      </param>
    </params>
  </methodCall>
  ```

### Level 1: Resources(Not a RESTful)
At Level 1 of the Richardson Maturity Model, the focus is on recognizing resources as fundamental entities within the 
API design. This level represents a transition from RPC-style architectures to a more resource-centric approach. Here 
are the key details of Level 1:
- **Resource Identification**:
  - APIs at this level begin to identify resources as the key entities that can be accessed and manipulated.
  - Each resource is typically represented by a unique URI (Uniform Resource Identifier) within the API.
- **Single Endpoint**:
  - Despite the recognition of resources, APIs at this level often have a single endpoint handling all requests.
  - This means that all interactions with different resources are typically routed through a single URI, resulting in 
    limited URI space and potential for ambiguity.
- **HTTP Methods**:
  - While resources are identified, there's often a lack of adherence to appropriate HTTP methods.
  - APIs may primarily rely on the HTTP GET method for all interactions, regardless of the operation being performed 
    (e.g., retrieving, updating, deleting).
- **Example**: Let's consider an example of a simple Level 1 API for managing users. In this API:
  - The API exposes a single endpoint: `/users`.
  - The `/users` endpoint is responsible for handling all operations related to users.
  - Regardless of the operation being performed (e.g., retrieving user information, updating user details), the API only
    uses HTTP GET requests.
  ```http
  GET /users           # Retrieves a list of all users
  GET /users/{id}      # Retrieves information about a specific user
  GET /users/{id}/profile  # Retrieves the profile of a specific user
  ```

### Level 2: HTTP Verbs(Partially RESTful)
Level 2 of the Richardson Maturity Model focuses on the proper use of HTTP verbs (methods) to perform CRUD (Create, 
Read, Update, Delete) operations on resources. At this level, APIs fully embrace the standard HTTP methods and use them 
in a consistent and meaningful way.

Here's an explanation of Level 2 along with examples:
Each HTTP method has a specific purpose in RESTful APIs, and Level 2 emphasizes using them appropriately:

- **GET**: Used to retrieve resource representations.
- **POST**: Used to create new resources.
- **PUT**: Used to update existing resources with a full representation.
- **PATCH**: Used to update existing resources with partial representations.
- **DELETE**: Used to remove resources.

```http
GET    http://example.com/images         # retrieves a list of images
POST   http://example.com/images         # uploads a new image, will return the image id

GET    http://example.com/images/{image-id}  # retrieves the specific image
PUT    http://example.com/images/{image-id}  # updates the specific image
DELETE http://example.com/images/{image-id}  # deletes the specific image
```

- **CRUD Operations**:
  - **Create**: Use POST to create a new resource
  - **Read**: Use GET to retrieve a resource's representation
  - **Update**: Use PUT or PATCH to update an existing resource
  - **Delete**: Use DELETE to remove a resource

  ```http
  POST /books
  Body: { "title": "RESTful API Design", "author": "Leonard Richardson" }
  
  GET /books/123
  
  PUT /books/123
  Body: { "title": "RESTful API Design: 2nd Edition" }
  
  DELETE /books/123
  ```

### Level 3: Hypermedia Controls(Fully RESTful)
Level 3 of the Richardson Maturity Model focuses on the use of Hypermedia Controls, also known as HATEOAS (Hypermedia
as the Engine of Application State). In this level, APIs provide hyperlinks within responses that guide clients on how
to interact with the API dynamically, without requiring prior knowledge of resource URIs.

- **Dynamic Navigation**: APIs at Level 3 include hyperlinks in responses that allow clients to navigate the API 
  dynamically. These hyperlinks provide entry points to related resources and actions that clients can take. For example,
  a response for a blog post might include hyperlinks to view comments, edit the post, or delete it.
- **Discoverability**: Hypermedia Controls enable clients to discover available actions and resources within the API
  dynamically. Clients no longer need to rely on hard-coded URIs or documentation to interact with the API. Instead,
  they can follow hyperlinks embedded in responses to discover and access related resources. For instance, a response 
  for a user profile might include hyperlinks to view the user's posts, and followers, or update their profile
  information.
- **State Transitions**: Hypermedia Controls define the available state transitions or actions that clients can perform
  on resources. By following hyperlinks, clients can trigger state transitions within the application. For example, a
  response for an order might include hyperlinks to cancel the order, view order details, or proceed to checkout.
  <br/>
  #### Example
  - Suppose you have a RESTful API for an online bookstore.
  - When a client requests information about a specific book, the API responds with a representation of the book
    resource, along with hyperlinks to related resources or actions.
  - The response might look like this (in JSON format):

  ```json
  {
    "title": "To Kill a Mockingbird",
    "author": "Harper Lee",
    "genre": "Fiction",
    "price": 10.99,
    "links": {
      "self": "/books/123",
      "addtocart": "/addtocart"
    }
  }
  ```

# Source
* [REST API Design Workshop](https://www.youtube.com/watch?v=EbHf2aCuPVM&list=PL_XxuZqN0xVAWGDKIzcn6NWikVkljJQZc&index=2)

