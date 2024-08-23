# REST Architectural Constraints
* REST does not enforce any rule regarding how it should be implemented at a lower level, it just put high-level 
  constraints on the system allowing any implementation that follows these constraints to be considered RESTful.

## The six guiding constraints are:


## Uniform Interface
The uniform interface constraint defines the interface between clients and servers. It simplifies and decouples the architecture, which enables each part to evolve independently.

* **Resource Identification:** Each resource is identified by a unique URI.
  * **Example:** https://api.example.com/customers/1
  * **Note:** This ensures that each resource can be accessed via a unique path, making it easy to locate and manipulate.

* **Resource Manipulation through Representations:** When a client holds a representation of a resource, including any metadata, it has enough information to modify or delete the resource on the server.
  * **Example:** Using HTTP methods like GET, POST, PUT, DELETE.
  * **Note:** This allows clients to interact with resources in a standardized way, regardless of how the resource is implemented on the server.

* **Self-descriptive Messages:** Each message includes enough information to describe how to process the message.
  * **Example:** Headers like Content-Type, Content-Length, Cache-Control.
  * **Note:** This ensures that each request and response can be understood in isolation, without relying on out-of-band information.

* **Hypermedia as the Engine of Application State (HATEOAS):** Clients interact with the server entirely through hypermedia provided dynamically by the server.
  * **Example:** The server provides links to related resources, such as {"href": "/customers/1/orders", "rel": "orders", "type": "application/json"}.
  * **Note:** This allows clients to navigate the API dynamically, following links provided by the server to discover and interact with resources.

* **Resource Size and Linking:** Any single resource should not be too large and contain each and everything in its representation. Whenever relevant, a resource should contain links (HATEOAS) pointing to relative URIs to fetch related information.
  * **Example:** A customer resource might link to their orders rather than embedding all order details: {"href": "/customers/1/orders", "rel": "orders"}.
  * **Note:** This promotes efficiency and modularity, ensuring that responses are concise and that related data can be fetched as needed.




## Client-Server
The separation of concerns between the client and the server. The client is responsible for the user interface and the server is responsible for the data storage.

* **Client:** The client is responsible for the user interface and user experience. It initiates requests to the server, processes the responses, and renders the user interface.
  * **Example:** A web browser rendering a webpage by making AJAX requests to a server.
  * **Note:** This ensures that the client handles presentation logic while the server handles business logic and data storage.

* **Server:** The server is responsible for storing and managing the data, processing requests from clients, and generating responses.
  * **Example:** A database server managing user data and responding to client queries.
  * **Note:** This centralizes data management, making it easier to maintain and secure.

* **Separation of Concerns:** By separating the user interface concerns from the data storage concerns, we improve the portability of the user interface across multiple platforms and improve scalability by simplifying the server components.
  * **Example:** Developing a single API that serves both a web application and a mobile app.
  * **Note:** This modular approach allows each component to be developed and updated independently.

* **Scalability:** By separating the concerns, we can scale the client and server components independently.
  * **Example:** Adding more server instances to handle increased load without changing the client.
  * **Note:** This allows each part of the system to scale according to its needs, improving overall efficiency.

* **Portability:** By separating the concerns, we can develop the client and server components independently.
  * **Example:** Developing a mobile client that interacts with the same server as a web client.
  * **Note:** This allows the same backend to serve different frontends, enhancing reuse and reducing redundancy.

* **Reliability:** By separating the concerns, we can improve the reliability of the system.
  * **Example:** Updating the server without affecting the client.
  * **Note:** This reduces the risk of introducing errors into the system when updates are made.

* **Security:** By separating the concerns, we can improve the security of the system.
  * **Example:** Restricting server access to only authorized clients.
  * **Note:** This ensures that sensitive data and business logic are protected on the server side.

* **Performance:** By separating the concerns, we can improve the performance of the system.
  * **Example:** Caching responses on the client to reduce the number of requests to the server.
  * **Note:** This reduces latency and improves the user experience by minimizing server load.

* **Flexibility:** By separating the concerns, we can improve the flexibility of the system.
  * **Example:** Changing the server implementation without affecting the client.
  * **Note:** This allows for easier updates and modifications to the backend without requiring changes to the frontend.

* **Maintainability:** By separating the concerns, we can improve the maintainability of the system.
  * **Example:** Updating the server implementation without affecting the client.
  * **Note:** This modularity simplifies troubleshooting and enhances the ability to make incremental improvements.

* **Testability:** By separating the concerns, we can improve the testability of the system.
  * **Example:** Testing the client and server components independently.
  * **Note:** This allows for more targeted and efficient testing processes.

* **Extensibility:** By separating the concerns, we can improve the extensibility of the system.
  * **Example:** Adding new features to the client and server components independently.
  * **Note:** This promotes the ability to expand the system with minimal disruption.

* **Interoperability:** By separating the concerns, we can improve the interoperability of the system.
  * **Example:** Developing the client and server components using different technologies.
  * **Note:** This enables the integration of diverse technologies, enhancing the versatility of the system.

This constraint essentially means that client applications and server applications MUST be able to evolve separately without any dependency on each other. A client should know only resource URIs, and that’s all. Today, this is standard practice in web development, so nothing fancy is required from your side. Keep it simple.

Servers and clients may also be replaced and developed independently, as long as the interface between them is not altered.



## Stateless
Each request from a client must contain all the information required by the server to fulfill the request. The server should not store any client state between requests.

* **Stateless Communication:** Each request from a client to a server must contain all the information necessary to understand the request. The server cannot rely on any previous requests from the client to understand the current request.
  * **Example:** An API request includes all necessary authentication tokens and parameters.
  * **Note:** This ensures that each request is self-contained and can be processed independently.

* **Stateless Servers:** Servers should not store any client state between requests. Each request should be independent and self-contained. This allows servers to be more scalable and reliable.
  * **Example:** A RESTful API server does not keep session information between requests.
  * **Note:** This enhances server scalability and simplifies load balancing.

* **Stateless Clients:** Clients should not store any server state between requests. Each request should be independent and self-contained. This allows clients to be more flexible and portable.
  * **Example:** A web client includes all necessary data in each request to the server.
  * **Note:** This allows clients to interact with different servers without dependency on stored state.

* **Stateless Interactions:** Interactions between clients and servers should be stateless. Each request should be independent and self-contained. This allows interactions to be more reliable and secure.
  * **Example:** Each HTTP request from a web application to a server includes all necessary information for processing.
  * **Note:** This improves the reliability and security of communications.

* **Stateless Systems:** Systems should be designed to be stateless. Each component should be independent and self-contained. This allows systems to be more scalable and maintainable.
  * **Example:** Microservices architecture where each service is stateless and independent.
  * **Note:** This promotes scalability and ease of maintenance.

* **Stateless Protocols:** Protocols should be designed to be stateless. Each message should be independent and self-contained. This allows protocols to be more flexible and extensible.
  * **Example:** HTTP is a stateless protocol where each request from client to server is independent.
  * **Note:** This flexibility enables diverse implementations and use cases.

* **Stateless Applications:** Applications should be designed to be stateless. Each operation should be independent and self-contained. This allows applications to be more reliable and secure.
  * **Example:** A cloud-based application that does not rely on server-side sessions.
  * **Note:** This design enhances reliability and security.

* **Stateless Architectures:** Architectures should be designed to be stateless. Each component should be independent and self-contained. This allows architectures to be more scalable and maintainable.
  * **Example:** A serverless architecture where each function is stateless.
  * **Note:** This promotes scalability and reduces complexity.

* **Stateless Designs:** Designs should be stateless. Each element should be independent and self-contained. This allows designs to be more flexible and extensible.
  * **Example:** A design pattern where each module operates independently.
  * **Note:** This enhances the flexibility and extensibility of the system.

* **Stateless Implementations:** Implementations should be stateless. Each module should be independent and self-contained. This allows implementations to be more reliable and secure.
  * **Example:** Stateless microservices that handle requests independently.
  * **Note:** This ensures that implementations are reliable and secure.

If the client application needs to be a stateful application for the end-user, where the user logs in once and does other authorized operations after that, then each request from the client should contain all the information necessary to service the request – including authentication and authorization details.

No client context shall be stored on the server between requests. The client is responsible for managing the state of the application.




## Cacheable
The server must indicate to the client if requests can be cached or not. If a response is cacheable, the client can reuse the response data for equivalent requests in the future.

* **Cacheable Responses:** Servers should indicate whether responses can be cached or not. If a response is cacheable, the client can reuse the response data for equivalent requests in the future.
  * **Example:** A server response includes `Cache-Control: public, max-age=3600` indicating the response can be cached for 1 hour.
  * **Note:** This improves efficiency by reducing the need to fetch the same data repeatedly.

* **Cache-Control:** Servers should use the `Cache-Control` header to indicate whether responses can be cached or not. The `Cache-Control` header can specify caching directives like `public`, `private`, `no-cache`, `no-store`, `max-age`, etc.
  * **Example:** `Cache-Control: no-store` indicates that the response should not be cached.
  * **Note:** This header provides fine-grained control over how responses are cached.

* **Expires:** Servers can use the `Expires` header to indicate when a response expires. The `Expires` header specifies a date and time after which the response is considered stale.
  * **Example:** `Expires: Wed, 21 Oct 2024 07:28:00 GMT` specifies the exact date and time after which the response should not be considered fresh.
  * **Note:** This helps clients determine when to revalidate or refresh the cached response.

* **ETag:** Servers can use the `ETag` header to provide a unique identifier for a response. The `ETag` header can be used by clients to validate cached responses.
  * **Example:** `ETag: "abc123"` allows the client to check if the cached version matches the current version on the server.
  * **Note:** This ensures that clients only use fresh and up-to-date data.

* **Last-Modified:** Servers can use the `Last-Modified` header to indicate when a response was last modified. The `Last-Modified` header can be used by clients to validate cached responses.
  * **Example:** `Last-Modified: Wed, 21 Oct 2024 07:28:00 GMT` allows the client to check if the cached response is still valid based on the last modification date.
  * **Note:** This provides an additional way to validate cached data.

* **Vary:** Servers can use the `Vary` header to indicate which request headers should be used to determine if a cached response can be reused. The `Vary` header can prevent clients from reusing cached responses that are not suitable for the current request.
  * **Example:** `Vary: Accept-Encoding` tells the client to cache different versions of the response based on the `Accept-Encoding` request header.
  * **Note:** This ensures that cached responses are appropriately matched to requests with different headers.

* **Cache-Control Directives:** Servers can use the `Cache-Control` header to specify caching directives like `public`, `private`, `no-cache`, `no-store`, `max-age`, etc. The `Cache-Control` header can control how responses are cached by clients and intermediaries.
  * **Example:** `Cache-Control: private, max-age=600` indicates that the response is for a single user and can be cached for 10 minutes.
  * **Note:** This allows for precise control over the caching behavior of responses.

Roy Fielding got inspiration from HTTP, so it reflected in this constraint. Make all client-server interactions stateless. The server will not store anything about the latest HTTP request the client made. It will treat every request as new. No session, no history. In today’s world, the caching of data and responses is of utmost importance wherever they are applicable/possible. The webpage you are reading here is also a cached version of the HTML page. Caching brings performance improvement for the client side and better scope for scalability for a server because the load has been reduced.

In REST, caching shall be applied to resources when applicable, and then these resources MUST declare themselves cacheable. Caching can be implemented on the server or client side.

Well-managed caching partially or completely eliminates some client-server interactions, further improving scalability and performance.


## Layered System
A client cannot ordinarily tell whether it is connected directly to the end server, or to an intermediary along the way. Intermediary servers can improve system scalability by enabling load balancing and shared caches.

* **Layered Architecture:** The layered architecture constraint allows an architecture to be composed of multiple layers. Each layer can only interact with the adjacent layers. This allows the architecture to be more scalable and maintainable.
  * **Example:** A web application where the presentation layer interacts with the business logic layer, which in turn interacts with the data access layer.
  * **Note:** This separation of concerns enhances modularity and maintainability.

* **Layered Systems:** Systems should be designed to be layered. Each layer should only interact with the adjacent layers. This allows systems to be more scalable and maintainable.
  * **Example:** A distributed system where requests pass through a load balancer, then an application server, and finally a database server.
  * **Note:** This design improves scalability and fault tolerance.

* **Layered Designs:** Designs should be layered. Each layer should only interact with the adjacent layers. This allows designs to be more flexible and extensible.
  * **Example:** An application design where the user interface layer interacts with the service layer, which interacts with the repository layer.
  * **Note:** This approach makes it easier to modify or replace individual layers without affecting the entire system.

* **Layered Implementations:** Implementations should be layered. Each layer should only interact with the adjacent layers. This allows implementations to be more reliable and secure.
  * **Example:** Implementing a microservices architecture where each service is a layer that communicates with other services through well-defined APIs.
  * **Note:** This enhances security and reliability by isolating different concerns into separate layers.

REST allows you to use a layered system architecture where you deploy the APIs on server A, store data on server B, and authenticate requests on server C, for example. A client cannot ordinarily tell whether it is connected directly to the end server or an intermediary along the way.


## Code on Demand (optional)
Servers can temporarily extend or customize the functionality of a client by transferring logic to the client. This constraint is optional and is not commonly used.

* **Code on Demand:** Servers can transfer executable code to clients. This allows servers to temporarily extend or customize the functionality of clients. This constraint is optional and is not commonly used.
  * **Example:** A server sends JavaScript code to a web client to render a dynamic UI widget.
  * **Note:** This can enhance the client's functionality without requiring a full update or new release.

Well, this constraint is optional. Most of the time, you will be sending the static representations of resources in the form of XML or JSON. But when you need to, you are free to return executable code to support a part of your application, e.g., clients may call your API to get a UI widget rendering code. It is permitted.

All the above constraints help you build a truly RESTful API, and you should follow them. Still, at times, you may find yourself violating one or two constraints. Do not worry; you are still making a RESTful API – but not “truly RESTful.”

Notice that all the above constraints are most closely related to WWW (the web). Using RESTful APIs, you can do the same thing with your web services that you do to web pages.

### Resources
* [REST Architectural Constraints](https://restfulapi.net/rest-architectural-constraints/)