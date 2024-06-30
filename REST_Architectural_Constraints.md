# REST Architectural Constraints
* REST does not enforce any rule regarding how it should be implemented at a lower level, it just put high-level 
  constraints on the system allowing any implementation that follows these constraints to be considered RESTful.

## The six guiding constraints are:

### Uniform Interface 
The uniform interface constraint defines the interface between clients and servers. It simplifies and decouples the 
architecture, which enables each part to evolve independently.
* **Resource Identification:** Each resource is identified by a unique URI like `https://api.example.com/customers/1`.
* **Resource Manipulation through Representations:** When a client holds a representation of a resource, including any 
  metadata, it has enough information to modify or delete the resource on the server like `GET`, `POST`, `PUT`, `DELETE`.
* **Self-descriptive Messages:** Each message includes enough information to describe how to process the message like
  `Content-Type`, `Content-Length`, `Cache-Control`, etc.
* **Hypermedia as the Engine of Application State (HATEOAS):** Clients interact with the server entirely through 
  hypermedia provided dynamically by the server. The server provides the client with links to the resources it needs to 
  interact with like `href`, `rel`, `type`, etc.
* Any single resource should not be too large and contain each and everything in its representation. Whenever relevant, 
  a resource should contain links (HATEOAS) pointing to relative URIs to fetch related information.


### Client-Server
The separation of concerns between the client and the server. The client is responsible for the user interface and the 
server is responsible for the data storage.
### Stateless 
Each request from a client must contain all the information required by the server to fulfill the request. The server 
should not store any client state between requests.

### Cacheable
The server must indicate to the client if requests can be cached or not. If a response is cacheable, the client can reuse
the response data for equivalent requests in the future.

### Layered System
A client cannot ordinarily tell whether it is connected directly to the end server, or to an intermediary along the way. 
Intermediary servers can improve system scalability by enabling load balancing and shared caches.

### Code on Demand (optional)
Servers can temporarily extend or customize the functionality of a client by transferring logic to the client. This 
constraint is optional and is not commonly used.

### Resources
* [REST Architectural Constraints](https://restfulapi.net/rest-architectural-constraints/)