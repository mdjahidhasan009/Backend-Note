# Partial Response:

Partial response is a method that lets API users choose what information they want in the response. For example, imagine
you have an online store with a **website** and a **mobile app**. On the website, you want to get details like 
**"product name"**, **"price"**, **"photo"**, **"category"**, and **"stock"** from the catalog service. But on the 
mobile app, you only need **"product name"**, **"photo"**, and **"price"**. By using Partial response, API users can
request only the specific data they require.

To request a partial response that includes only the **"product name"**, **"photo"**, and **"price"** fields, the client 
would make a request to the server like this:

```bash
GET /api/v1/products?fields=name,photo,price
```

The server would then respond with a representation of the products containing only the specified fields:

```json
{
    "data": [
        {
            "id": "23634674",
            "name": "Product Name",
            "photo": "https://store.com/photo1.jpg",
            "price": 55
        },
        {
            "id": "2363856679",
            "name": "Product Name",
            "photo": "https://store.com/photo2.jpg",
            "price": 66
        }
    ]
}
```

## The benefits of partial response:

- **Reduced Bandwidth Usage:** By fetching only the necessary fields, partial response reduces the amount of data 
  transferred between the client and server. This can significantly decrease bandwidth usage, especially for large 
  resources with many fields.
- **Improved Performance:** With reduced data transfer, partial response can lead to faster response times and improved 
  performance, particularly for clients with limited network bandwidth or slower connections.
- **Optimized Client-Side Processing:** Clients often only need a subset of fields from a resource for their specific 
  use case. By requesting only the required fields, clients can optimize their processing and memory usage, leading to
  more efficient client-side operations.
- **Simplified API Consumption:** Partial response simplifies API consumption by allowing clients to specify exactly 
  which fields they need, rather than filtering out unnecessary fields on the client side. This results in cleaner and
  more concise code on the client side.
- **Reduced Over-fetching:** Over-fetching occurs when an API sends more data than necessary, leading to wasted 
  bandwidth and increased processing time on both the client and server. Partial response helps mitigate over-fetching 
  by allowing clients to retrieve only the required fields, thus reducing unnecessary data transfer.

# Query Parameter

Query parameters are a fundamental aspect of HTTP requests used to specify additional information for a resource 
retrieval or manipulation operation. They are appended to the URL of the request and consist of a key-value pair
separated by an equal sign (=) and delimited by an ampersand (&) if multiple parameters are present.

Query parameters serve various purposes in RESTful architecture, including:
- **Filtering:** Clients can use query parameters to filter resources based on specific criteria. For example, in an
  e-commerce API, clients might want to retrieve only products of a certain category or within a particular price range.
  <br/><br/>
  **Example:** `/products?priceMin=600&priceMax=2000`
- **Sorting:** Query parameters allow clients to specify the order in which resources should be returned. This is useful
  when clients want to retrieve resources in a particular order, such as alphabetical or by date.
- **Pagination:** When dealing with large datasets, pagination is essential to retrieve data in manageable chunks. Query 
  parameters like limit and offset or page enable clients to request a specific subset of results.
  <br/><br/>
  **Example:** `/products?limit=10&page=1`
- **Projection:** Clients can use query parameters to specify which fields of a resource should be included or excluded 
  in the response. This is helpful when clients only need a subset of fields for their use case, reducing bandwidth 
  usage and improving performance.
  <br/><br/>
  **Example:** `/products?fields=name,price,photo`
- **Search:** Query parameters can be used to perform full-text search or keyword-based search within resources. Clients
  can specify search terms as query parameters to retrieve relevant results from the API.
  <br/><br/>
  **Example:** `/products?search=iphone`

Let's consider an example of a `/products` endpoint in an e-commerce API that supports various query parameters for 
filtering, sorting, pagination, and projection:

```http
GET /products?priceMin=600&priceMax=2000&sort=price&order=asc&page=2&limit=10&fields=name,photo,price
```
- The base URI is `/products`, representing the collection of products.
- The query parameters are used to filter the products based on certain criteria.
- The `sort=price` parameter indicates that the products should be sorted based on their price.
- The `order=asc` parameter specifies that the sorting should be in ascending order. You can use `desc` for descending 
  order.
- The `page=2` parameter indicates that the results should be paginated, and the response should include the products
  from the second page.
- The `limit=10` parameter specifies that each page should contain a maximum of 10 products.
```
{
  "status": 200,
  "message": "Success",
  "data": [
    {
      "id": "23634674",
      "name": "Product Name",
      "photo": "https://store.com/photo1.jpg",
      "price": 55
    },
    {
      "id": "2363856679",
      "name": "Product Name",
      "photo": "https://store.com/photo2.jpg",
      "price": 66
    }
  ],
  "pagination": {
    "page": 2,
    "limit": 10,
    "totalPages": 5,
    "totalItems": 50,
    "links": {
      "self": "/products?page=1",
      "next": "/products?page=2"
    }
  }
}
```

# REST API Errors Response

Handling errors in a RESTful API involves following certain principles to ensure consistency, clarity, and ease of use
for clients. Here are some best practices for handling errors in a RESTful architecture:

- **Use Standard HTTP Status Codes**: HTTP status codes provide standardized responses that convey the outcome of a
  request. Use appropriate status codes to indicate the success or failure of a request. Some common HTTP status codes
  for error handling include:
    - `400 Bad Request`: The request could not be understood or processed due to client error (e.g., malformed request
      syntax).
    - `401 Unauthorized`: The client must authenticate itself to access the resource.
    - `403 Forbidden`: The client is not allowed to access the requested resource, even with authentication.
    - `404 Not Found`: The requested resource could not be found.
    - `422 Unprocessable Entity`: The request was well-formed but could not be processed due to semantic errors (e.g.,
      validation errors, Business Logic Errors).
    - `500 Internal Server Error`: An unexpected error occurred on the server while processing the request.
- **Provide Descriptive Error Messages**: Along with HTTP status codes, include descriptive error messages in the
  response body to provide additional context about the error. This helps clients understand the nature of the problem
  and take appropriate action. Use clear and human-readable error messages that provide guidance on how to resolve the
  issue.
- **Follow a Consistent Error Format**: Define a consistent format for error responses across your API. This might
  include standardizing the structure of error objects, such as including fields like `error_code`, `message`, and
  details. Consistency makes it easier for clients to parse and handle errors programmatically.
- **Include Error Details**: In addition to a general error message, provide specific details about the cause of the
  error whenever possible. This might include information about invalid input parameters, missing required fields, or
  constraints that were violated. Including error details helps developers diagnose and troubleshoot issues more
  effectively. We can use hint and trace_id to provide more information about the error.
- **Handle Uncaught Exceptions Gracefully**: Ensure that uncaught exceptions or errors on the server side are handled
  gracefully to prevent them from leaking sensitive information or causing instability. Use try-catch blocks or
  exception-handling mechanisms to intercept and handle errors, returning appropriate HTTP status codes and error
  messages to clients.
- **Offer Guidance for Recovery:** When returning error responses, include guidance or suggestions for how clients can 
  recover from the error and proceed with their intended operation. This might involve recommending alternative actions,
  providing links to relevant documentation, or suggesting retry strategies.
  <br/><br/>
  Let's say we have a simple RESTful API for managing products. We'll focus on the scenario where a client attempts to
  retrieve information about a product, but the requested product does not exist. In this case, we'll return a 404 Not
  Found status code along with an error message indicating that the product could not be found.
  <br/><br/>
  **Request:**

  ```http
    GET /api/v1/products/123  
  ```
    
  ```json
  {
  "traceId": "236534346",
  "errors": [
    {
      "code": 404,
      "message": "Product not found",
      "details": "The requested product with ID '123' does not exist.",
      "hints": "Please check the product is available with id '123'"
    }
  ]}
  ```
- The response body includes an error object with the following fields:
  - `code`: The HTTP status code indicating the type of error (`404` in this case).
  - `message`: A human-readable error message describing the problem (`Product not found`).
  - `hints`: Additional details providing context about the error (`The requested Product with ID '123' does not exist`).
  - `Trace Id`: Trace ID helps the API provider company easily identify the root cause of the error.
- If the success scenario occurs, it should also provide the required data and follow the HATEOAS principle. 
  - Can provide the next link, previous link, self link, etc. in the response.
  - Can provide the pagination information in the response.
  - Can provide hints in the response to help the client understand the response, or deprecation information, etc.
    - Can provide the trace_id in the response to help the client provide the trace_id to the technical support team to 
      get more information about the error or issue.
  ```json
  {
    "message": "Product Retrieval Successful",
    "info": "The URL /api/v1/products has been deprecated and we have now shifted it to /api/v2/products. If you want to learn more about this update, please visit our developer portal at http://developer.stackshop.com.",
    "data": [
      {
        "id": "5c661a94-5d64-4446-837c-76f4b65f506e",
        "name": "Smartphone",
        "price": 49.99,
        "links": {
          "self": "/products/5c661a94-5d64-4446-837c-76f4b65f506e",
          "add-to-cart": "/cart/add"
        }
      },
      {
        "id": "29201b9d-e577-4a7c-a034-18d601f253b3",
        "name": "Smartphone",
        "price": 49.99,
        "links": {
          "self": "/products/29201b9d-e577-4a7c-a034-18d601f253b3",
          "add-to-cart": "/cart/add"
        }
      },
      {
        "id": "a7690e4b-18a4-4b84-9874-23c76ca9ad12",
        "name": "Smartphone",
        "price": 49.99,
        "links": {
          "self": "/products/a7690e4b-18a4-4b84-9874-23c76ca9ad12",
          "add-to-cart": "/cart/add"
        }
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 5,
      "totalPages": 11,
      "totalItems": 54,
      "links": {
        "self": "/products?page=2&limit=5",
        "first": "/products?page=1&limit=5",
        "prev": "/products?page=1&limit=5",
        "next": "/products?page=3&limit=5"
      }
    }
  }
  ```
  ```json
  {
    "message": "Product Created Successfully",
    "data": {
      "id": "ed662673-f73b-4a1d-90a5-0193df0451da",
      "sku": "500",
      "name": "Hat",
      "description": "Description of the product",
      "price": 356,
      "status": "PUBLISHED",
      "createdAt": "2024-03-30T17:23:34.615Z",
      "updatedAt": "2024-03-30T17:23:34.615Z",
      "links": {
        "self": "/products",
        "get": "/products/ed662673-f73b-4a1d-90a5-0193df0451da",
        "update": "/products/ed662673-f73b-4a1d-90a5-0193df0451da",
        "delete": "/products/ed662673-f73b-4a1d-90a5-0193df0451da"
      }
    },
    "trace_id": "1711819414612"
  }
  ```
  ```json
  {
    "message": "Product Retrieval Successful",
    "data": {
      "id": "ed662673-f73b-4a1d-90a5-0193df0451da",
      "name": "Hat",
      "description": "Description of the product",
      "price": 356,
      "status": "PUBLISHED",
      "links": {
        "self": "/products/ed662673-f73b-4a1d-90a5-0193df0451da",
        "add-to-cart": "/cart/add"
     }
   },
   "trace_id": "1711819541117"
  }
  ```
  ```json
  {
    "message": "Product Deleted Successful",
    "trace_id": "23463426346"
  }
  ```

## Error Object Example
### 400 Bad Request
```json
{
  "message": "Validation Error",
  "errors": [
    "\"sku\" is required"
  ],
  "hints": "Please provide all the required fields. If the problem is not resolved, please feel free to contact our technical team with the trace_id.",
  "trace_id": "1710188356390"
}
```

### 401 Unauthorized
```json
{
  "message": "Access Denied",
  "errors": [],
  "hints": "Please send the access token in the header as a Bearer Token. If the problem is not resolved, please feel free to contact our technical team.",
  "trace_id": "23463426346"
}
```

### 500 Internal Server Error
```json
{
  "message": "Internal Server Error",
  "errors": [],
  "hints": "This is a server-related issue, please feel free to contact our technical team.",
  "trace_id": "23463426346"
}
```

If we use sentry, datadog, telemetry, we can send the `trace_id` to the client. So, the client can provide the `trace_id`
to the technical support team to get more information about the error. 

Hint is also helpful for the client to understand the error and take action.   

# HTTP Cache-Control
## Cacheability

Responses from the server should be explicitly labeled as cacheable or non-cacheable.

The `Cache-Control` header is used to specify caching directives that control how responses are cached and served.
```http
-H : Cache-Control, "public, max-age=5"
```

## Directives

- **Max-Age**: Specifies the maximum time (in seconds) that a response can be cached by the client or intermediary 
  caches.  
  For example, `Cache-Control: max-age=3600` indicates that the response can be cached for up to one hour.

- **S-Max-Age**: Similar to max-age, but applies only to shared caches (e.g., proxies). It overrides the max-age
  directive for shared caches.  
  For example, `Cache-Control: s-maxage=3600` specifies that shared caches can cache the response for one hour.

- **No-Cache**: Indicates that a response can be cached by the client or intermediary caches, but must be revalidated
  with the server before each use. It does not prevent caching but requires validation of the cached response's freshness.  
  For example, `Cache-Control: no-cache`.

- **No-Store**: Specifies that a response should not be stored in any cache, including browser caches and intermediary
  caches. It instructs clients to fetch the response from the server for each request.  
  For example, `Cache-Control: no-store`.  
  Sensitive data should not be stored anywhere (e.g., Banking data, Medical record).


## Public

Specifies that a response can be cached by any cache, including both private (client-side) caches and shared caches 
(e.g., proxies). Sensitive data should not be cached.

For example, `Cache-Control: public`.
```
-H : Cache-Control, "public", "max-age=5"
```

## Private

Indicates that a response can be cached by the client’s browser but not by shared caches (e.g., proxies). It is
typically used for responses intended for a specific user or client.

For example, `Cache-Control: private`.
```
-H : Cache-Control, "private", "max-age=5"
```

### Example
```js
```javascript
const getAllProductsController = async (req, res, next) => {
    const response = {
        pagination: {
            page,
            limit,
            totalPage,
            totalItems: items.length,
            links: {
                self: `/products?page=${page}&limit=${limit}`,
                first: `/products?page=1&limit=${limit}`,
                last: `/products?page=${totalPage}&limit=${limit}`,
                prev: page > 1 ? `/products?page=${page - 1}&limit=${limit}` : null,
                next: page < totalPage ? `/products?page=${page + 1}&limit=${limit}` : null,
            },
        },
        data: items,
    };

    console.log("Call from GET /products");

    // Set Cache-Control header
    res.header("Cache-Control", "public, max-age=60");

    res.status(200).json(response);
    } catch (err) {
    const error = CustomError.serverError(err);
    next(error);
    }
};

module.exports = getAllProductsController;
```

We define a `Cache-Control` header with the value `public, max-age=60` here. This indicates that the response can be
cached by any cache (public) and that the maximum age of the cached response is 60 seconds. The client or intermediary
caches can store the response for up to 60 seconds before revalidating it with the server.
``js
// Set Cache-Control header
res.header("Cache-Control", "public, max-age=60");
``

When the API is called for the first time, the data is fetched directly from the API. Subsequent calls within a set time
frame (defined by the cache duration) will fetch the data from the browser's cache instead of making a new request to 
the server. This behavior can be verified by checking the headers of the API call.

In the first API call, the `console.log("Call from GET /products");` statement will be logged in the server terminal, 
indicating that the request was processed by the server.

For any subsequent requests within the cache duration (e.g., one minute), the data will be served from the browser's 
cache, and the log statement will not appear in the server terminal.

### Verification via Headers In the Network Tab of the Browser
- **First Request**:
  - Status Code: `200 OK`
  - Data fetched directly from the API.
  - The `console.log("Call from GET /products");` will be logged.

- **Subsequent Requests** (within the cache duration):
  - Status Code: `200 OK (from disk cache)`
  - Data fetched from the browser's cache.
  - No `console.log` message will be logged on the server terminal.

## ETag Headers

The ETag (Entity Tag) header is an HTTP response header that provides a mechanism for web servers to assign a unique
identifier to a specific version of a resource. ETags are used for cache validation and conditional requests, allowing
clients to determine whether their cached representation of a resource is still valid without having to download the
entire resource again from the server.

The primary purpose of the ETag header is to provide a lightweight and efficient way to validate cached responses and 
reduce unnecessary data transfer.

- **ETag Generation**: When a client makes a request for a resource, the server generates an ETag value for the current 
  version of the resource. This value is typically a hash of the resource, such as its content, metadata, or other
  characteristics.
  - **Resource**: `/product/123`

- **Inclusion in Response**: The server includes the ETag value in the response headers using the `ETag` header.
  - **For example**: `ETag: "abcdef123456"`

- **Storage by Client**: The client saves the response along with the ETag value for future reference. If the client
  needs to make subsequent requests for the same resource, it can include the ETag value in the `If-None-Match` header 
  of the request.

- **Conditional Requests**: When the client makes a subsequent request for the resource, it includes the stored ETag 
  value in the `If-None-Match` header of the request.
  - **For example**: `If-None-Match: "abcdef123456"`

- **Validation by Server**: Upon receiving the conditional request, the server compares the ETag value provided by the 
  client with the current ETag value of the resource. If the ETag values match, it indicates that the cached
  representation is still valid, and the server responds with a `304 Not Modified` status code, indicating that the
  client should use its cached copy. If the ETag values do not match, the server responds with the full resource content,
  along with a new ETag value for the updated version.

### ETag Process Explanation

1. **Initial Request**:
  - The client sends a request to the server to retrieve a resource (e.g., `/products/123`).
  - The server generates an ETag (a unique identifier for the version of the resource) and includes it in the response 
    headers.
  - The server responds with a `200 OK` status, including the requested resource data in the body and the ETag in the 
    header.

2. **Client Stores ETag**:
  - The client stores the received resource data along with the ETag for future reference.

3. **Subsequent Request**:
  - The client makes a subsequent request to the server for the same resource, this time including the stored ETag in 
    the `If-None-Match` header of the request.
  - The server checks if the ETag provided by the client matches the current ETag of the resource.

4. **Conditional Response**:
  - If the ETag matches, indicating that the resource has not been modified since the last request, the server responds
    with a `304 Not Modified` status, instructing the client to use the cached data.
  - If the ETag does not match, the server generates a new ETag, sends the updated resource data, and includes the new
    ETag in the response header.

This process helps in reducing unnecessary data transfer and optimizing client-server communication by leveraging cache
validation with ETags.

## Example Etag Implementation
### Server Code
```js
const crypto = require("crypto");

const generateEtag = (data) => {
  const hash = crypto.createHash("sha1");
  hash.update(JSON.stringify(data));
  return hash.digest("hex");
};

module.exports = generateEtag;
```

```js
const getProductController = async (req, res, next) => {
  const invalidFields = fields?.reduce((acc, field) => {
    acc.push(`${field} is not a valid field`);
    return acc;
  }, []);

  if (invalidFields?.length) {
    const error = CustomError.badRequest({
      message: "Invalid fields",
      errors: invalidFields,
      hints: "Please provide valid fields from [${allowedFields.join(",")}]",
    });
    return next(error);
  }

  let product = await getProductService(id);

  if (!product) {
    const error = CustomError.notFound({
      message: "Product not found",
      errors: ["The product with the provided id does not exist"],
      hints: "Please provide a valid product id",
    });
    return next(error);
  }

  // This block of code is filtering the product object based on the fields requested in the query parameters.
  if (fields) {
    const filteredProduct = fields.reduce(
      (acc, field) => {
        acc[field] = product[field];
        return acc;
      },
      { id: product.id }
    );
    product = filteredProduct;
  }

  // Generate ETag
  const ETag = generateEtag(product);

  // Check if the ETag matches the one in the request headers
  if (req.headers["if-none-match"] === ETag) {
    console.log("[match etag]", ETag, req.headers["if-none-match"]);
    res.status(304).send("Not Modified");
    return;
  }

  // Add Cache-Control
  res.setHeader("ETag", ETag);

  console.log(`call from /products${id}`);

  // Prepare response
  res.status(200).json({
    message: "Product Retrieval Successful",
    data: {
      ...product,
      links: {
        self: `/products/${product.id}`,
        "add-to-cart": `/cart/add`,
      },
    },
    trace_id: req.headers["x-trace-id"],
  });
};

module.exports = getProductController;
```
#### Notes:

- It looks like the server is ultimately fetching the data from the database, but there could be a Redis or Memcache 
  layer, so it would fetch from that.
- Using caching reduces bandwidth and latency. For example, if one of our blogs goes viral, it could be hit millions of
  times in a minute, so this will save a lot of bandwidth, which means less cost.
- In this example, we generate an ETag hash using every data field of the product. Alternatively, we could use only the
  `updatedAt` field for validation.

#### If the Server Does Not Support ETag

```js
app.use(
  cors({
    origin: ["http://127.0.0.1:5500"],
    credentials: true,
    exposedHeaders: ["x-trace-id", "x-correlation-id", "ETag", "if-none-match"],
    methods: ["GET", "HEAD", "PUT", "PATCH", "POST", "DELETE"],
    preflightContinue: false,
  })
);
```
We have to check if the server supports ETag. Sometimes, the server does not serve ETag headers by default. In such
cases, we may need to specify the ETag inside CORS settings, as shown in the example above, to ensure the necessary 
headers are exposed. Additionally, if the server lacks ETag support, we might need to implement custom middleware to add
ETag functionality, enabling efficient caching and validation mechanisms

### Frontend Code

```html
<html lang="en">
  <body>
    <script>
      const urlParams = new URLSearchParams(window.location.search);
      const params = Object.fromEntries(urlParams.entries());
      const productId = params.id;

      const getSingleProduct = async (productId) => {
        const etag = sessionStorage.getItem("etag");

        const headers = {
          ...(etag && { "If-None-Match": etag }),
        };

        const response = await fetch(`http://localhost:4000/api/v1/products/${productId}`, {
          headers: headers,
        });

        if (response.status === 304) {
          const data = JSON.parse(sessionStorage.getItem("product"));
          console.log("data", data);
          renderDOM({
            name: data.name,
            price: data.price,
            description: data.description,
            status: data.status,
          });
          console.log("Not Modified");
          return;
        }

        if (response.status === 200) {
          const data = await response.json();
          sessionStorage.setItem("etag", response.headers.get("ETag"));
          sessionStorage.setItem("product", JSON.stringify(data.data));
          renderDOM({
            name: data.data.name,
            price: data.data.price,
            description: data.data.description,
            status: data.data.status,
          });
          console.log("Modified");
        }
      };

      const renderDOM = (data) => {
        product_details.innerHTML = `<h2>${data.name}</h2>
          <p>Price: ${data.price}</p>
          <p>Description: ${data.description}</p>
          <span>Status: ${data.status}</span>`;
      };

      getSingleProduct(productId);
    </script>
  </body>
</html>
```
### Note
When the browser makes the first request to fetch the product data, it will receive a response with a status code 
**200** along with the data. The data will be stored in the `sessionStorage`, including the `ETag` value from the 
response headers.

On subsequent requests for the same product, the browser will include the stored `ETag` in the request headers. If the 
product data has not been modified on the server, the response will have a status code **304** ("Not Modified"), 
indicating that the cached data is still valid. In this case, no data will be sent in the response, and the browser will
use the cached data stored in `sessionStorage` to render the product details.

# REST API Versioning
API versioning is the practice of managing and maintaining different versions of an API to accommodate changes and
updates over time while ensuring backward compatibility and minimizing disruptions for existing clients. It involves 
assigning a version identifier to each version of the API, allowing clients to specify which version they intend to use.

## Benefits of API Versioning
- **Backward Compatibility**: API versioning allows for the introduction of changes and updates while ensuring that 
  existing clients continue to function without disruption.
- **Incremental Updates**: By versioning the API, developers can make incremental updates and improvements without
  affecting the entire API surface.
- **Flexibility**: API versioning provides flexibility for clients to choose which version of the API they want to use, 
  allowing them to adopt new features at their own pace.
- **Maintainability**: Versioning makes it easier to maintain and support multiple versions of the API simultaneously,
  catering to different client needs and requirements.
- **Documentation and Communication**: Versioning helps in clearly documenting and communicating changes to the API, 
  making it easier for developers to understand and adapt to the changes.


## Breaking Change

A breaking change is a modification to a software component that disrupts or invalidates existing functionality or
behavior. It may cause existing client applications or integrations to fail or behave differently than expected. 
Breaking changes require clients to make updates or modifications to accommodate the changes in order to maintain 
compatibility with the updated version of the software.

### Example of Breaking Change

- **Changing the URI structure of an existing API endpoint:**
  - Before: `GET /api/v1/product/{id}`
  - After: `GET /api/v1/products/{id}`
  - This change could break existing client implementations that rely on the old URI structure, causing requests to fail
    with 404 Not Found errors.
- **Changing the data type or format of a response field.**
- **Modifying the behavior of existing API methods or endpoints in a way that affects client applications.**
  - Example: `PUT /products` to `PATCH /products`


### Versioning for Breaking Change
- **URL Based:**
  - Old Version: `/api/v1/products`
  - New Version: `/api/v2/products`
- **Header Based:** (It's a good idea to use header-based versioning if your software requires ongoing breaking changes 
  or you follow semantic versioning (Minor, Major))
  - Example:
    ```http
    GET /api/products
    Headers:
    X-API-Version: 2
    ```


## Non-Breaking Change
A non-breaking change is a modification to a software component that does not affect existing functionality or behavior.
Non-breaking changes are backward-compatible, meaning they can be introduced without requiring modifications to existing
client applications or integrations. Non-breaking changes may include enhancements, optimizations, or additions to 
existing functionality that do not impact the existing behavior of the software.

### Example Of Non-Breaking Change

- Adding new API endpoints
- Optimizing performance or improving error handling without changing existing behavior
- Adding a new field to the product representation

#### Before:
```json
{
  "name": "Smartphone"
}
```
#### After
```json
{
  "id": 123,
  "name": "Smartphone",
  "color": "gray"
}
```


## Best Practices Of Handling API Changes

**Versioning Strategy**:
- Implement a versioning strategy that clearly distinguishes between different versions of the API (e.g., URI versioning,
  header versioning) to allow clients to specify the desired version explicitly.

**API Documentation**:
- Maintain comprehensive and up-to-date API documentation that clearly outlines the API endpoints, request and response.
  formats, supported versions, and any changes introduced in each version.
- Provide examples, usage guidelines, and migration steps to assist API consumers.

**Backward Compatibility**:
- Strive to maintain backward compatibility with existing clients whenever possible.
- Avoid introducing breaking changes that may disrupt existing functionality or require clients to make significant 
  modifications to their integrations.

**Graceful Deprecation**:
- When deprecating endpoints or features, implement a graceful deprecation process by providing deprecation warnings, 
  informative error messages, and migration guides to help clients transition to alternative solutions or newer versions
  of the API.

**Monitoring and Feedback**:
- Monitor API usage patterns, error rates, and client feedback to identify any issues or concerns related to API changes.
- Collect feedback from API consumers through surveys, support channels, and developer feedback channels to understand 
  their experiences and address any challenges they face.


# Source
* [REST API Design Workshop](https://www.youtube.com/watch?v=orn9N90dcVY&list=PL_XxuZqN0xVAWGDKIzcn6NWikVkljJQZc&index=5)