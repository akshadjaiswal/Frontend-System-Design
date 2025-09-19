
# Complete REST API Guide

## Table of Contents

1.  [Introduction](#introduction)
2.  [System Architecture Evolution](#system-architecture-evolution)
    -   [One-Tier Architecture](#one-tier-architecture)
    -   [Two-Tier Architecture](#two-tier-architecture)
    -   [Three-Tier Architecture](#three-tier-architecture)
3.  [What is REST API?](#what-is-rest-api)
    -   [Understanding REST](#understanding-rest)
    -   [Understanding API](#understanding-api)
    -   [The Role of HTTP](#the-role-of-http)
4.  [Benefits of REST API](#benefits-of-rest-api)
5.  [Building Blocks of REST](#building-blocks-of-rest)
    -   [HTTP Request Structure](#http-request-structure)
    -   [HTTP Response Structure](#http-response-structure)
6.  [URL Components](#url-components)
7.  [HTTP Methods](#http-methods)
8.  [HTTP Headers](#http-headers)
9.  [Status Codes](#status-codes)

## Introduction

REST (Representational State Transfer) API is one of the most popular architectural styles for designing web services that enable communication between different applications over the internet. REST APIs have become the backbone of modern web development, enabling seamless integration between diverse systems regardless of the programming languages or platforms they use.

This comprehensive guide covers everything from fundamental concepts to practical implementation, providing you with the knowledge needed to understand, design, and build robust REST APIs.

## System Architecture Evolution

To understand why REST APIs are essential, we need to examine how web application architectures have evolved to address scalability, maintainability, and flexibility challenges.

### One-Tier Architecture

**Structure:**

```
┌─────────────────────────────────┐
│        Single Server            │
├─────────────────────────────────┤
│  Frontend (User Interface)      │
│  Backend (Business Logic)       │
│  Database (Data Storage)        │
└─────────────────────────────────┘

```

In one-tier architecture, all components of an application run on a single machine or system. This includes the user interface, business logic, and database operations all bundled together.

**Characteristics:**

-   Everything operates within one codebase
-   Single technology stack for the entire application
-   Direct database access from the user interface
-   All processing happens on one server

**Real-World Example:** Think of a traditional desktop application like an older version of Microsoft Access where the database, forms, and business logic all exist in one file on one computer.

**Problems with One-Tier Architecture:**

-   **Scalability Nightmare**: Cannot scale individual components independently
-   **Technology Lock-in**: Entire application must use the same programming language and framework
-   **Maintenance Hell**: Changes to UI affect business logic and vice versa
-   **Single Point of Failure**: Server failure means complete application downtime
-   **Limited Concurrent Users**: Cannot handle high user loads effectively
-   **Deployment Complexity**: Must deploy entire application even for small changes

### Two-Tier Architecture

**Structure:**

```
┌─────────────────┐    Network    ┌─────────────────────────┐
│     Client      │ ◄──────────► │       Server            │
│   (Frontend)    │   Connection  │ (Backend + Database)    │
├─────────────────┤               ├─────────────────────────┤
│ • User Interface│               │ • Business Logic        │
│ • Presentation  │               │ • Data Processing       │
│ • Input Validation│             │ • Database Operations   │
└─────────────────┘               └─────────────────────────┘

```

Two-tier architecture, also known as client-server architecture, separates the presentation layer (client) from the business logic and data layer (server).

**Key Improvements:**

-   **Separation of Concerns**: Clear distinction between what users see and how data is processed
-   **Technology Flexibility**: Frontend can use different technology than backend
-   **Better Resource Utilization**: Can optimize client and server hardware separately
-   **Improved Security**: Business logic hidden from client-side code

**Communication Flow:**

1.  Client sends request to server
2.  Server processes request using business logic
3.  Server queries database if needed
4.  Server sends response back to client
5.  Client updates user interface

**Real-World Example:** A web application where React.js frontend communicates with a Node.js backend that contains both API endpoints and database operations.

**Limitations:**

-   Business logic and data access still tightly coupled
-   Database operations mixed with application logic
-   Difficult to scale database operations independently
-   Limited ability to reuse business logic across different applications

### Three-Tier Architecture

**Structure:**

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  Presentation   │    │   Application   │    │      Data       │
│     Tier        │    │      Tier       │    │      Tier       │
├─────────────────┤    ├─────────────────┤    ├─────────────────┤
│ • User Interface│    │ • Business Logic│    │ • Database      │
│ • Client Apps   │◄──►│ • API Endpoints │◄──►│ • File Systems  │
│ • Web Browsers  │    │ • Authentication│    │ • Data Storage  │
│ • Mobile Apps   │    │ • Validation    │    │ • Data Retrieval│
└─────────────────┘    └─────────────────┘    └─────────────────┘

```

Three-tier architecture further separates concerns by creating distinct layers for presentation, business logic, and data management.

**Layer Responsibilities:**

**Presentation Tier (Client):**

-   Handles user interface and user experience
-   Manages user input and displays results
-   Can be web browsers, mobile apps, or desktop applications
-   Communicates with application tier through APIs

**Application Tier (Server/Backend):**

-   Contains business logic and rules
-   Processes requests from presentation tier
-   Manages authentication and authorization
-   Validates data and enforces business constraints
-   Coordinates communication with data tier

**Data Tier (Database):**

-   Manages data storage and retrieval
-   Handles database operations (CRUD)
-   Ensures data integrity and consistency
-   Can include multiple databases or storage systems

**Advantages:**

-   **Independent Scaling**: Each tier can be scaled separately based on demand
-   **Technology Diversity**: Each tier can use the most appropriate technology
-   **Maintainability**: Changes in one tier don't necessarily affect others
-   **Reusability**: Business logic can be reused across different presentation layers
-   **Security**: Better separation of sensitive operations and data

**Real-World Example:** An e-commerce platform with:

-   **Presentation**: React web app, iOS app, Android app
-   **Application**: Express.js API server handling orders, payments, inventory
-   **Data**: PostgreSQL database storing products, users, orders

## What is REST API?

REST API combines two fundamental concepts: REST (architectural style) and API (communication interface). Understanding both components is crucial for mastering REST API development.

### Understanding REST

REST stands for **Representational State Transfer**. It's an architectural style that defines a set of constraints and principles for creating web services.

**Core Concept:** REST is based on the idea that web resources (data, services, documents) should be:

-   **Identifiable**: Each resource has a unique identifier (URL)
-   **Manipulable**: Resources can be created, read, updated, and deleted
-   **Self-descriptive**: Messages contain enough information to process them
-   **Stateless**: Each request contains all necessary information

**Key Principles of REST:**

**1. Client-Server Architecture**

-   Clear separation between client (consumer) and server (provider)
-   Client handles user interface concerns
-   Server handles data storage and business logic
-   Both can evolve independently

**2. Statelessness**

-   Server doesn't store client context between requests
-   Each request must contain all information needed to process it
-   No server-side sessions or stored client state
-   Improves scalability and reliability

**3. Cacheability**

-   Responses should be cacheable when appropriate
-   Reduces client-server interactions
-   Improves performance and scalability
-   Servers can indicate whether responses are cacheable

**4. Uniform Interface**

-   Consistent way to interact with resources
-   Standard HTTP methods (GET, POST, PUT, DELETE)
-   Standard status codes and headers
-   Predictable URL patterns

**5. Layered System**

-   Architecture can be composed of hierarchical layers
-   Each layer only knows about immediate layers
-   Allows for intermediaries like proxies, gateways, load balancers

### Understanding API

API stands for **Application Programming Interface**. It's a set of rules and protocols that allows different software applications to communicate with each other.

**What APIs Enable:**

-   **Inter-application Communication**: Different programs can share data and functionality
-   **Service Integration**: Combine services from multiple providers
-   **Platform Independence**: Applications built on different technologies can interact
-   **Modular Development**: Build applications using existing services and components

**API Components:**

-   **Endpoints**: Specific URLs where API can be accessed
-   **Methods**: Actions that can be performed (GET, POST, PUT, DELETE)
-   **Parameters**: Data sent with requests
-   **Responses**: Data returned from API calls
-   **Authentication**: Security mechanisms to control access

### The Role of HTTP

HTTP (Hypertext Transfer Protocol) serves as the foundation for REST APIs, providing the communication protocol between clients and servers.

**HTTP as the Foundation:** REST APIs leverage HTTP's built-in features:

-   **Methods**: GET, POST, PUT, DELETE map to CRUD operations
-   **Status Codes**: Standardized response codes (200, 404, 500, etc.)
-   **Headers**: Metadata about requests and responses
-   **URLs**: Resource identification and location

**Request-Response Cycle:**

```
Client                          Server
  │                               │
  │ ──── HTTP Request ────────► │
  │                               │
  │ ◄─── HTTP Response ──────── │
  │                               │

```

**HTTP Methods in REST Context:**

-   **GET**: Retrieve data (Read operation)
-   **POST**: Create new resources (Create operation)
-   **PUT**: Update existing resources (Update operation)
-   **DELETE**: Remove resources (Delete operation)
-   **PATCH**: Partial updates to resources

## Benefits of REST API

REST APIs have become the dominant choice for web service architecture due to their numerous advantages that address modern development challenges.

### 1. Simplicity and Ease of Use

**Developer-Friendly Design:** REST APIs use familiar HTTP concepts that developers already understand. The learning curve is minimal because it builds on existing web technologies.

**Standard HTTP Methods:**

```javascript
// Simple and intuitive operations
GET /api/users          // Get all users
GET /api/users/123      // Get specific user
POST /api/users         // Create new user
PUT /api/users/123      // Update user
DELETE /api/users/123   // Delete user

```

**Easy Integration:** Most programming languages have built-in or readily available HTTP clients:

-   **JavaScript**: fetch() API, axios, request libraries
-   **Python**: requests library, urllib
-   **Java**: HttpClient, OkHttp, RestTemplate
-   **PHP**: cURL, Guzzle
-   **C#**: HttpClient, RestSharp

### 2. Statelessness

**No Server-Side State Management:** Each request is independent and contains all necessary information. The server doesn't need to remember previous interactions with clients.

**Benefits of Statelessness:**

-   **Scalability**: Servers can handle requests without maintaining session state
-   **Reliability**: No lost sessions or state corruption
-   **Load Balancing**: Requests can be distributed across multiple servers
-   **Fault Tolerance**: Server failures don't affect client state

**Example:**

```javascript
// Each request is complete and independent
// Request 1
GET /api/orders/123
Authorization: Bearer abc123
Accept: application/json

// Request 2 (no dependency on Request 1)
POST /api/orders
Authorization: Bearer abc123
Content-Type: application/json
Body: { "product_id": 456, "quantity": 2 }

```

### 3. Scalability

**Horizontal Scaling:** Stateless nature allows adding more server instances to handle increased load.

**Vertical Scaling:** Individual components can be optimized and upgraded independently.

**Caching Support:** Built-in HTTP caching mechanisms reduce server load and improve response times.

**Load Distribution:**

```
        Load Balancer
           /    |    \
    Server 1  Server 2  Server 3
        \       |       /
         \      |      /
          Database Cluster

```

### 4. Flexibility with Data Formats

REST APIs support multiple data formats, allowing clients to choose the most appropriate format for their needs.

**Supported Formats:**

-   **JSON**: Most popular, lightweight, JavaScript-friendly
-   **XML**: Structured, good for complex data, enterprise systems
-   **HTML**: Direct browser consumption
-   **Plain Text**: Simple data exchange
-   **Binary**: Files, images, documents

**Content Negotiation:**

```http
# Client requests JSON
GET /api/products
Accept: application/json

# Server responds with JSON
Content-Type: application/json
{"id": 1, "name": "Product A"}

# Client requests XML
GET /api/products
Accept: application/xml

# Server responds with XML
Content-Type: application/xml
<product><id>1</id><name>Product A</name></product>

```

### 5. Uniform Interface

REST provides a consistent, predictable interface across all resources and operations.

**Standard URL Patterns:**

```
GET    /api/resources           # List all resources
GET    /api/resources/123       # Get specific resource
POST   /api/resources           # Create new resource
PUT    /api/resources/123       # Update specific resource
DELETE /api/resources/123       # Delete specific resource

```

**Consistent Response Structure:**

```json
{
  "data": { /* actual resource data */ },
  "status": "success",
  "message": "Resource retrieved successfully",
  "timestamp": "2023-10-15T10:30:00Z"
}

```

### 6. Caching Support

HTTP caching mechanisms work seamlessly with REST APIs, providing significant performance benefits.

**Cache Types:**

-   **Browser Cache**: Client-side caching for web applications
-   **Proxy Cache**: Intermediate caching layers
-   **Server Cache**: Application-level caching
-   **CDN Cache**: Geographically distributed caching

**Cache Control Headers:**

```http
# Server response with caching directives
Cache-Control: max-age=3600, public
ETag: "123456789"
Last-Modified: Wed, 15 Oct 2023 10:00:00 GMT

# Client conditional request
GET /api/products/123
If-None-Match: "123456789"
If-Modified-Since: Wed, 15 Oct 2023 10:00:00 GMT

# Server response (if not modified)
HTTP/1.1 304 Not Modified

```

### 7. Separation of Concerns

REST APIs enable clear separation between client (frontend) and server (backend) responsibilities.

**Frontend Responsibilities:**

-   User interface and user experience
-   Data presentation and visualization
-   Client-side validation and interaction
-   State management for UI components

**Backend Responsibilities:**

-   Business logic implementation
-   Data validation and processing
-   Authentication and authorization
-   Database operations and data persistence

**Benefits:**

-   **Independent Development**: Frontend and backend teams can work simultaneously
-   **Technology Choice**: Each side can use optimal technologies
-   **Specialized Teams**: Developers can focus on their expertise areas
-   **Easier Testing**: Components can be tested independently

### 8. Interoperability

REST APIs enable communication between systems built with different technologies, programming languages, and platforms.

**Language Agnostic:**

-   Python backend can serve React frontend
-   Java API can be consumed by iOS Swift application
-   .NET server can communicate with Android Java client
-   PHP API can serve Angular TypeScript application

**Platform Independence:**

-   Web applications
-   Mobile applications (iOS, Android)
-   Desktop applications
-   IoT devices
-   Server-to-server communication

### 9. Testing and Debugging

REST APIs are inherently testable due to their stateless nature and standard HTTP protocols.

**Testing Tools:**

-   **Postman**: GUI-based API testing and documentation
-   **cURL**: Command-line HTTP client
-   **Insomnia**: API client and testing tool
-   **Newman**: Command-line Postman collection runner

**Easy Debugging:**

```bash
# Simple cURL request for testing
curl -X GET "https://api.example.com/users/123" \
     -H "Authorization: Bearer token123" \
     -H "Accept: application/json"

# Response includes all necessary debugging information
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: max-age=300
{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com"
}

```

### 10. Security

REST APIs can leverage existing HTTP security mechanisms and can be enhanced with additional security layers.

**Built-in Security Features:**

-   **HTTPS**: Encrypted communication
-   **Authentication Headers**: Bearer tokens, API keys
-   **CORS**: Cross-origin resource sharing control
-   **Content Security Policy**: Protection against XSS attacks

**Additional Security Measures:**

-   **OAuth 2.0**: Industry-standard authorization framework
-   **JWT Tokens**: Secure information transmission
-   **Rate Limiting**: Protection against abuse
-   **Input Validation**: Data sanitization and validation

## Building Blocks of REST

Understanding the fundamental components of REST communication is essential for building and consuming REST APIs effectively. Every REST interaction consists of structured requests and responses that follow HTTP conventions.

### HTTP Request Structure

Every HTTP request sent to a REST API consists of three main components that work together to convey the client's intent to the server.

**Complete Request Structure:**

```
┌─────────────────────────────────────┐
│           REQUEST LINE              │
├─────────────────────────────────────┤
│           HEADERS                   │
│         (Key: Value pairs)          │
├─────────────────────────────────────┤
│             BODY                    │
│        (Optional data)              │
└─────────────────────────────────────┘

```

**1. Request Line** The request line contains the most essential information about what the client wants to do.

Components:

-   **HTTP Method**: The action to perform (GET, POST, PUT, DELETE, etc.)
-   **Request URI**: The path to the resource
-   **HTTP Version**: The protocol version being used

Example:

```
GET /api/users/123 HTTP/1.1
POST /api/products HTTP/1.1
PUT /api/orders/456 HTTP/1.1
DELETE /api/comments/789 HTTP/1.1

```

**2. Request Headers** Headers provide metadata about the request, helping the server understand how to process it properly.

**Common Headers:**

**Authorization:**

```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=
Authorization: API-Key abc123def456

```

**Content-Type (for requests with body):**

```
Content-Type: application/json
Content-Type: application/xml
Content-Type: multipart/form-data
Content-Type: text/plain

```

**Accept (preferred response format):**

```
Accept: application/json
Accept: application/xml
Accept: text/html
Accept: */*

```

**User-Agent (client information):**

```
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36
User-Agent: MyApp/1.0 (iOS 15.0; iPhone13,2)
User-Agent: PostmanRuntime/7.29.2

```

**Other Important Headers:**

```
Host: api.example.com
Cache-Control: no-cache
If-None-Match: "123456789"
If-Modified-Since: Wed, 15 Oct 2023 10:00:00 GMT
X-Custom-Header: custom-value

```

**3. Request Body** The body contains data sent to the server, typically used with POST, PUT, and PATCH requests.

**JSON Body Example:**

```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "age": 30,
  "address": {
    "street": "123 Main St",
    "city": "New York",
    "zipCode": "10001"
  }
}

```

**Form Data Body Example:**

```
name=John+Doe&email=john%40example.com&age=30

```

**Complete Request Example:**

```
POST /api/users HTTP/1.1
Host: api.example.com
Authorization: Bearer abc123def456
Content-Type: application/json
Accept: application/json
User-Agent: MyApp/1.0
Content-Length: 156

{
  "name": "John Doe",
  "email": "john@example.com",
  "department": "Engineering",
  "role": "Developer"
}

```

### HTTP Response Structure

HTTP responses follow a similar structure to requests but contain different types of information focused on the server's response to the client's request.

**Complete Response Structure:**

```
┌─────────────────────────────────────┐
│         STATUS LINE                 │
├─────────────────────────────────────┤
│          HEADERS                    │
│       (Server metadata)             │
├─────────────────────────────────────┤
│            BODY                     │
│       (Response data)               │
└─────────────────────────────────────┘

```

**1. Status Line** The status line provides immediate feedback about the result of the request.

Components:

-   **HTTP Version**: Protocol version used
-   **Status Code**: Numeric code indicating result
-   **Reason Phrase**: Human-readable status description

Examples:

```
HTTP/1.1 200 OK
HTTP/1.1 201 Created
HTTP/1.1 400 Bad Request
HTTP/1.1 401 Unauthorized
HTTP/1.1 404 Not Found
HTTP/1.1 500 Internal Server Error

```

**2. Response Headers** Response headers provide metadata about the server's response and instructions for the client.

**Common Response Headers:**

**Content Information:**

```
Content-Type: application/json; charset=utf-8
Content-Length: 1234
Content-Encoding: gzip

```

**Caching Directives:**

```
Cache-Control: max-age=3600, public
ETag: "123456789abcdef"
Last-Modified: Wed, 15 Oct 2023 10:30:00 GMT
Expires: Thu, 16 Oct 2023 10:30:00 GMT

```

**Security Headers:**

```
Access-Control-Allow-Origin: https://myapp.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Authorization, Content-Type
X-Content-Type-Options: nosniff
X-Frame-Options: DENY

```

**Server Information:**

```
Server: nginx/1.18.0
Date: Wed, 15 Oct 2023 10:30:00 GMT
X-Rate-Limit-Remaining: 99
X-Rate-Limit-Reset: 1634294400

```

**3. Response Body** The response body contains the actual data requested by the client or information about the operation's result.

**Successful Data Response:**

```json
{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com",
  "created_at": "2023-10-15T10:30:00Z",
  "updated_at": "2023-10-15T10:30:00Z",
  "profile": {
    "bio": "Software Developer",
    "location": "New York",
    "website": "https://johndoe.dev"
  }
}

```

**Error Response:**

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid email format",
    "details": [
      {
        "field": "email",
        "message": "Must be a valid email address"
      }
    ]
  },
  "timestamp": "2023-10-15T10:30:00Z",
  "path": "/api/users"
}

```

**Collection Response:**

```json
{
  "data": [
    {
      "id": 1,
      "name": "Product A",
      "price": 99.99
    },
    {
      "id": 2,
      "name": "Product B",
      "price": 149.99
    }
  ],
  "pagination": {
    "current_page": 1,
    "total_pages": 10,
    "total_items": 100,
    "items_per_page": 10
  }
}

```

**Complete Response Example:**

```
HTTP/1.1 201 Created
Content-Type: application/json; charset=utf-8
Content-Length: 198
Cache-Control: no-cache
Location: /api/users/124
Date: Wed, 15 Oct 2023 10:30:00 GMT
Server: Express/4.18.0

{
  "id": 124,
  "name": "John Doe",
  "email": "john@example.com",
  "created_at": "2023-10-15T10:30:00Z",
  "message": "User created successfully"
}

```

## URL Components

Understanding URL structure is crucial for designing and consuming REST APIs effectively. URLs serve as the addresses for resources in your API, and their structure directly impacts the usability and intuitiveness of your API.

### Complete URL Structure

A complete URL consists of several components that work together to identify and locate resources:

```
https://api.example.com:443/v1/users/123?sort=name&limit=10#profile

┌──────┬─────────────────┬────┬──────────────┬─────────────────┬─────────┐
│Schema│      Host       │Port│     Path     │  Query String   │Fragment │
└──────┴─────────────────┴────┴──────────────┴─────────────────┴─────────┘

```

### 1. Schema (Protocol)

The schema defines the protocol used for communication between client and server.

**HTTP vs HTTPS:**

-   **HTTP (Hypertext Transfer Protocol)**
    
    -   Default port: 80
    -   Unencrypted communication
    -   Suitable for development and non-sensitive data
    -   Example: `http://api.example.com`
-   **HTTPS (HTTP Secure)**
    
    -   Default port: 443
    -   Encrypted communication using SSL/TLS
    -   Required for production APIs handling sensitive data
    -   Example: `https://api.example.com`

**Other Protocols in API Context:**

-   **WebSocket**: `ws://` or `wss://` for real-time communication
-   **FTP**: `ftp://` for file transfer (rarely used in REST APIs)

### 2. Host

The host identifies the server where the API is located and consists of multiple parts:

**Host Structure:**

```
subdomain.domain.top-level-domain
   │        │         │
   │        │         └─ .com, .org, .net, .io, .dev
   │        └─────────── example, google, github
   └──────────────────── api, www, cdn, admin

```

**Examples:**

-   `api.example.com` - API subdomain
-   `v1.api.example.com` - Version-specific subdomain
-   `users-api.example.com` - Service-specific subdomain
-   `example.com` - Root domain
-   `localhost` - Local development

**Development vs Production Hosts:**

```javascript
// Development
const DEV_API_BASE = 'http://localhost:3000/api';

// Staging
const STAGING_API_BASE = 'https://staging-api.example.com';

// Production
const PROD_API_BASE = 'https://api.example.com';

```

### 3. Port

The port specifies which port on the server to connect to. While often omitted (using default ports), it's crucial for development and custom configurations.

**Common Ports:**

-   **80**: Default HTTP port
-   **443**: Default HTTPS port
-   **3000, 8000, 8080**: Common development ports
-   **5000**: Often used for Node.js applications
-   **8443**: Alternative HTTPS port

**When to Include Ports:**

```javascript
// Development - port usually required
http://localhost:3000/api/users

// Production with default ports - port omitted
https://api.example.com/users

// Production with custom port - port required
https://api.example.com:8443/users

```

### 4. Path

The path identifies the specific resource or endpoint within the API. Well-designed paths are intuitive and follow consistent patterns.

**Path Design Principles:**

**Resource-Based Paths:**

```
/users              # Collection of users
/users/123          # Specific user with ID 123
/users/123/posts    # Posts belonging to user 123
/users/123/posts/456 # Specific post 456 by user 123

```

**Hierarchical Structure:**

```
/api/v1/organizations/456/departments/789/employees/123
└─┘ └─┘ └──────────────┘ └─────────────┘ └────────────┘
API Ver  Organization    Department      Employee

```

**Static vs Dynamic Paths:**

**Static Paths** (Fixed resource locations):

```
/api/health         # Health check endpoint
/api/version        # API version information
/api/documentation  # API documentation

```

**Dynamic Paths** (Variable resource identifiers):

```
/api/users/:userId           # :userId is a parameter
/api/products/:productId     # :productId is a parameter
/api/orders/:orderId/items   # Nested resource structure

```

**Path Parameters in Different Frameworks:**

**Express.js:**

```javascript
app.get('/users/:userId', (req, res) => {
  const userId = req.params.userId;
  // Handle request for specific user
});

app.get('/products/:category/:productId', (req, res) => {
  const { category, productId } = req.params;
  // Handle request for product in specific category
});

```

**Real-World Path Examples:**

**E-commerce API:**

```
GET /api/products                    # List all products
GET /api/products/electronics        # Products in electronics category
GET /api/products/123                # Specific product
GET /api/products/123/reviews        # Reviews for product 123
GET /api/products/123/reviews/456    # Specific review

```

**Social Media API:**

```
GET /api/users/johndoe              # User profile
GET /api/users/johndoe/posts        # User's posts
GET /api/users/johndoe/followers    # User's followers
GET /api/posts/123/comments         # Comments on post
GET /api/posts/123/likes            # Likes on post

```

### 5. Query String

Query strings provide additional parameters to modify the request or filter results. They start with `?` and use `&` to separate multiple parameters.

**Query String Structure:**

```
?parameter1=value1&parameter2=value2&parameter3=value3

```

**Common Use Cases:**

**Filtering:**

```
/api/products?category=electronics&price_min=100&price_max=500
/api/users?status=active&role=admin
/api/orders?date_from=2023-01-01&date_to=2023-12-31

```

**Sorting:**

```
/api/products?sort=price_asc        # Sort by price ascending
/api/products?sort=price_desc       # Sort by price descending
/api/users?sort=created_at_desc     # Sort by creation date
/api/products?sort=name,price_asc   # Multiple sort criteria

```

**Pagination:**

```
/api/products?page=2&limit=20       # Page-based pagination
/api/products?offset=40&limit=20    # Offset-based pagination
/api/users?cursor=abc123&limit=10   # Cursor-based pagination

```

**Field Selection:**

```
/api/users?fields=id,name,email     # Only return specific fields
/api/products?include=reviews,tags   # Include related data
/api/orders?exclude=internal_notes   # Exclude sensitive fields

```

**Search:**

```
/api/products?search=laptop         # Text search
/api/users?q=john&fields=name,email # Query with field specification
/api/articles?title_contains=rest   # Partial matching

```

**Complex Query Examples:**

```javascript
// E-commerce product search
/api/products?category=electronics&brand=apple&price_min=500&price_max=2000&sort=price_asc&page=1&limit=20

// User management with filtering
/api/users?role=admin,user&status=active&created_after=2023-01-01&fields=id,name,email,role&sort=created_at_desc

// Analytics data with date range and grouping
/api/analytics/sales?date_from=2023-01-01&date_to=2023-12-31&group_by=month&include_tax=true&currency=USD

```

**URL Encoding:** Special characters in query strings must be properly encoded:

```
Original: /api/search?q=hello world&tags=rest api
Encoded:  /api/search?q=hello%20world&tags=rest%20api

Original: /api/users?email=john@example.com
Encoded:  /api/users?email=john%40example.com

```

**Common Encoding:**

-   Space: `%20` or `+`
-   @: `%40`
-   &: `%26`
-   =: `%3D`
-   ?: `%3F`
-   #: `%23`

### 6. Fragment (Hash)

Fragments are rarely used in REST API URLs but can be helpful for client-side navigation or referencing specific parts of a response.

**Fragment Examples:**

```
/api/documentation#authentication    # Jump to authentication section
/api/users/123#profile              # Focus on profile section

```

**Note:** Fragments are not sent to the server and are handled entirely by the client.

## HTTP Methods

HTTP methods define the type of operation to be performed on a resource. RESTful APIs use HTTP methods to create a uniform interface that maps naturally to CRUD (Create, Read, Update, Delete) operations.

### Primary HTTP Methods

### 1. GET - Retrieve Data

The GET method is used to retrieve data from the server. It should never modify server state and is considered "safe" and "idempotent."

**Characteristics:**

-   **Safe**: Does not modify server state
-   **Idempotent**: Multiple identical requests have the same effect
-   **Cacheable**: Responses can be cached by browsers and proxies
-   **No request body**: Data sent only via URL parameters

**Use Cases:**

```javascript
// Retrieve all resources
GET /api/users                    // Get all users
GET /api/products                 // Get all products

// Retrieve specific resource
GET /api/users/123                // Get user with ID 123
GET /api/products/456             // Get product with ID 456

// Retrieve related resources
GET /api/users/123/orders         // Get orders for user 123
GET /api/products/456/reviews     // Get reviews for product 456

// Search and filter
GET /api/products?category=electronics&price_min=100
GET /api/users?role=admin&status=active

```

**Implementation Example:**

```javascript
// Express.js GET handler
app.get('/api/users/:userId', async (req, res) => {
  try {
    const userId = req.params.userId;
    const user = await User.findById(userId);
    
    if (!user) {
      return res.status(404).json({
        error: 'User not found',
        code: 'USER_NOT_FOUND'
      });
    }
    
    res.status(200).json({
      data: user,
      status: 'success'
    });
  } catch (error) {
    res.status(500).json({
      error: 'Internal server error',
      code: 'SERVER_ERROR'
    });
  }
});

```

### 2. POST - Create New Resources

The POST method creates new resources on the server. It's not idempotent, meaning multiple identical requests may have different effects.

**Characteristics:**

-   **Not safe**: Modifies server state
-   **Not idempotent**: Multiple requests may create multiple resources
-   **Can have request body**: Data sent in request body
-   **Not cacheable**: Responses typically not cached

**Use Cases:**

```javascript
// Create new resources
POST /api/users                   // Create new user
POST /api/products                // Create new product
POST /api/orders                  // Create new order

// Create related resources
POST /api/users/123/orders        // Create order for user 123
POST /api/products/456/reviews    // Create review for product 456

// Non-CRUD operations
POST /api/auth/login              // User authentication
POST /api/password/reset          // Password reset
POST /api/emails/send             // Send email

```

**Request/Response Example:**

```javascript
// Request
POST /api/users
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com",
  "role": "user"
}

// Successful Response
HTTP/1.1 201 Created
Location: /api/users/124
Content-Type: application/json

{
  "id": 124,
  "name": "John Doe",
  "email": "john@example.com",
  "role": "user",
  "created_at": "2023-10-15T10:30:00Z",
  "status": "success",
  "message": "User created successfully"
}

```

**Implementation Example:**

```javascript
app.post('/api/users', async (req, res) => {
  try {
    const { name, email, role } = req.body;
    
    // Validation
    if (!name || !email) {
      return res.status(400).json({
        error: 'Name and email are required',
        code: 'VALIDATION_ERROR'
      });
    }
    
    // Check if user exists
    const existingUser = await User.findByEmail(email);
    if (existingUser) {
      return res.status(409).json({
        error: 'User with this email already exists',
        code: 'USER_EXISTS'
      });
    }
    
    // Create user
    const newUser = await User.create({ name, email, role });
    
    res.status(201).json({
      data: newUser,
      status: 'success',
      message: 'User created successfully'
    });
  } catch (error) {
    res.status(500).json({
      error: 'Internal server error',
      code: 'SERVER_ERROR'
    });
  }
});

```

### 3. PUT - Update/Replace Resources

The PUT method updates an existing resource or creates it if it doesn't exist. It's idempotent and typically replaces the entire resource.

**Characteristics:**

-   **Not safe**: Modifies server state
-   **Idempotent**: Multiple identical requests have the same effect
-   **Complete replacement**: Usually replaces entire resource
-   **Can create**: May create resource if it doesn't exist

**Use Cases:**

```javascript
// Update existing resources
PUT /api/users/123                // Update user 123
PUT /api/products/456             // Update product 456

// Replace entire resource
PUT /api/users/123/profile        // Replace user profile
PUT /api/settings/theme           // Replace theme settings

```

**Request/Response Example:**

```javascript
// Request - Complete resource replacement
PUT /api/users/123
Content-Type: application/json

{
  "name": "John Smith",
  "email": "johnsmith@example.com",
  "role": "admin",
  "department": "Engineering"
}

// Successful Response
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 123,
  "name": "John Smith",
  "email": "johnsmith@example.com",
  "role": "admin",
  "department": "Engineering",
  "updated_at": "2023-10-15T10:30:00Z",
  "status": "success",
  "message": "User updated successfully"
}

```

### 4. PATCH - Partial Updates

The PATCH method applies partial modifications to a resource. Unlike PUT, it only updates specified fields.

**Characteristics:**

-   **Not safe**: Modifies server state
-   **Not necessarily idempotent**: Depends on implementation
-   **Partial update**: Only modifies specified fields
-   **More efficient**: Sends only changed data

**Use Cases:**

```javascript
// Partial updates
PATCH /api/users/123              // Update specific user fields
PATCH /api/products/456/price     // Update only product price
PATCH /api/orders/789/status      // Update order status

```

**Request/Response Example:**

```javascript
// Request - Partial update
PATCH /api/users/123
Content-Type: application/json

{
  "email": "newemail@example.com",
  "department": "Marketing"
}

// Successful Response
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 123,
  "name": "John Smith",           // Unchanged
  "email": "newemail@example.com", // Updated
  "role": "admin",                // Unchanged
  "department": "Marketing",       // Updated
  "updated_at": "2023-10-15T10:30:00Z",
  "status": "success",
  "message": "User updated successfully"
}

```

### 5. DELETE - Remove Resources

The DELETE method removes resources from the server. It's idempotent because deleting the same resource multiple times has the same effect.

**Characteristics:**

-   **Not safe**: Modifies server state
-   **Idempotent**: Multiple deletions have the same effect
-   **Permanent action**: Usually irreversible (unless soft delete)
-   **No request body**: Typically no data in request body

**Use Cases:**

```javascript
// Delete specific resources
DELETE /api/users/123             // Delete user 123
DELETE /api/products/456          // Delete product 456
DELETE /api/orders/789            // Delete order 789

// Delete related resources
DELETE /api/users/123/sessions    // Delete all user sessions
DELETE /api/products/456/reviews/789 // Delete specific review

```

**Response Examples:**

```javascript
// Successful deletion with content
HTTP/1.1 200 OK
Content-Type: application/json

{
  "status": "success",
  "message": "User deleted successfully",
  "deleted_id": 123
}

// Successful deletion without content
HTTP/1.1 204 No Content

// Resource not found
HTTP/1.1 404 Not Found
Content-Type: application/json

{
  "error": "User not found",
  "code": "USER_NOT_FOUND"
}

```

### Additional HTTP Methods

### 6. HEAD - Metadata Only

HEAD method is identical to GET but returns only headers, not the response body. Useful for checking resource existence or metadata.

**Use Cases:**

```javascript
HEAD /api/users/123               // Check if user exists
HEAD /api/files/large-video.mp4   // Check file size before download

```

### 7. OPTIONS - Available Methods

OPTIONS method returns information about the communication options available for a resource or server.

**Use Cases:**

```javascript
OPTIONS /api/users                // Get allowed methods
OPTIONS *                         // Get server capabilities

// Response
HTTP/1.1 200 OK
Allow: GET, POST, PUT, PATCH, DELETE
Access-Control-Allow-Methods: GET, POST, PUT, PATCH, DELETE
Access-Control-Allow-Headers: Authorization, Content-Type

```

## HTTP Headers

HTTP headers carry metadata about requests and responses, providing essential information for proper communication between clients and servers. Understanding headers is crucial for building secure, efficient, and well-behaved REST APIs.

### Request Headers

Request headers provide information about the client, the requested resource, and the expected response format.

### Authentication Headers

**Authorization Header:** The most critical header for secured APIs, containing credentials or tokens.

```javascript
// Bearer Token (JWT, OAuth)
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

// Basic Authentication
Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=

// API Key
Authorization: API-Key abc123def456789
X-API-Key: abc123def456789

// Custom Authentication
X-Auth-Token: user123:session456

```

**Implementation Example:**

```javascript
// Express.js middleware to handle authentication
const authenticateToken = (req, res, next) => {
  const authHeader = req.headers['authorization'];
  const token = authHeader && authHeader.split(' ')[1];
  
  if (!token) {
    return res.status(401).json({
      error: 'Access token required',
      code: 'MISSING_TOKEN'
    });
  }
  
  jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
    if (err) {
      return res.status(403).json({
        error: 'Invalid or expired token',
        code: 'INVALID_TOKEN'
      });
    }
    req.user = user;
    next();
  });
};

// Usage
app.get('/api/protected', authenticateToken, (req, res) => {
  res.json({ message: 'Access granted', user: req.user });
});

```

### Content Headers

**Content-Type:** Specifies the format of data being sent in the request body.

```javascript
// JSON data
Content-Type: application/json

// Form data
Content-Type: application/x-www-form-urlencoded

// File upload
Content-Type: multipart/form-data

// XML data
Content-Type: application/xml

// Plain text
Content-Type: text/plain

// Binary data
Content-Type: application/octet-stream

```

**Content-Length:** Indicates the size of the request body in bytes.

```javascript
Content-Length: 1234

```

**Content-Encoding:** Specifies any encoding applied to the request body.

```javascript
Content-Encoding: gzip
Content-Encoding: deflate
Content-Encoding: br (Brotli)

```

### Accept Headers

**Accept:** Specifies the response formats the client can handle.

```javascript
// Prefer JSON
Accept: application/json

// Multiple formats with preferences
Accept: application/json, application/xml;q=0.8, text/plain;q=0.5

// Any format
Accept: */*

// Specific version
Accept: application/vnd.api+json;version=1

```

**Accept-Language:** Indicates preferred languages for the response.

```javascript
Accept-Language: en-US,en;q=0.9,es;q=0.8
Accept-Language: fr-FR,fr;q=0.9,en;q=0.8

```

**Accept-Encoding:** Specifies compression algorithms the client supports.

```javascript
Accept-Encoding: gzip, deflate, br
Accept-Encoding: gzip;q=1.0, deflate;q=0.6, *;q=0.1

```

### Caching Headers

**Cache-Control:** Controls caching behavior for the request.

```javascript
// Don't use cached version
Cache-Control: no-cache

// Don't store response
Cache-Control: no-store

// Revalidate before using cache
Cache-Control: must-revalidate

```

**If-None-Match:** Conditional request based on ETag.

```javascript
If-None-Match: "123456789"
If-None-Match: "abc123", "def456"

```

**If-Modified-Since:** Conditional request based on modification date.

```javascript
If-Modified-Since: Wed, 15 Oct 2023 10:00:00 GMT

```

### Custom and Utility Headers

**User-Agent:** Identifies the client application.

```javascript
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36
User-Agent: MyMobileApp/1.2.3 (iOS 15.0; iPhone13,2)
User-Agent: PostmanRuntime/7.29.2

```

**Referer:** URL of the page that made the request.

```javascript
Referer: https://myapp.com/dashboard

```

**X-Forwarded-For:** Original client IP when using proxies.

```javascript
X-Forwarded-For: 203.0.113.195, 198.51.100.178

```

**Custom Headers:** Application-specific headers (prefix with X- for non-standard headers).

```javascript
X-Request-ID: abc123-def456-ghi789
X-Client-Version: 2.1.0
X-Device-Type: mobile
X-Correlation-ID: request-123456789

```

### Response Headers

Response headers provide information about the server, the response data, and instructions for the client.

### Status and Server Headers

**Server:** Information about the server software.

```javascript
Server: nginx/1.18.0 (Ubuntu)
Server: Apache/2.4.41
Server: Express.js/4.18.0

```

**Date:** When the response was generated.

```javascript
Date: Wed, 15 Oct 2023 10:30:00 GMT

```

**Content-Type:** Format of the response data.

```javascript
Content-Type: application/json; charset=utf-8
Content-Type: text/html; charset=utf-8
Content-Type: image/png
Content-Type: application/pdf

```

**Content-Length:** Size of the response body.

```javascript
Content-Length: 2048

```

### Caching Response Headers

**Cache-Control:** Caching directives for clients and proxies.

```javascript
// Public cache, valid for 1 hour
Cache-Control: public, max-age=3600

// Private cache, no intermediate caching
Cache-Control: private, max-age=300

// Don't cache at all
Cache-Control: no-cache, no-store, must-revalidate

// Cache but revalidate
Cache-Control: max-age=0, must-revalidate

```

**ETag:** Entity tag for cache validation.

```javascript
ETag: "123456789abcdef"
ETag: W/"weak-etag-value"

```

**Last-Modified:** When the resource was last modified.

```javascript
Last-Modified: Wed, 15 Oct 2023 09:00:00 GMT

```

**Expires:** Absolute expiration time.

```javascript
Expires: Thu, 16 Oct 2023 10:30:00 GMT

```

### Security Headers

**Access-Control-Allow-Origin:** CORS policy for cross-origin requests.

```javascript
Access-Control-Allow-Origin: https://myapp.com
Access-Control-Allow-Origin: *

```

**Access-Control-Allow-Methods:** Allowed HTTP methods for CORS.

```javascript
Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS

```

**Access-Control-Allow-Headers:** Allowed headers for CORS.

```javascript
Access-Control-Allow-Headers: Authorization, Content-Type, X-Requested-With

```

**Security-focused Headers:**

```javascript
// Prevent XSS attacks
X-XSS-Protection: 1; mode=block

// Prevent MIME sniffing
X-Content-Type-Options: nosniff

// Control framing
X-Frame-Options: DENY
X-Frame-Options: SAMEORIGIN

// HTTPS enforcement
Strict-Transport-Security: max-age=31536000; includeSubDomains

// Content Security Policy
Content-Security-Policy: default-src 'self'; script-src 'self' 'unsafe-inline'

```

### Location and Redirection Headers

**Location:** URL of a newly created resource or redirect destination.

```javascript
// After POST request creating new resource
Location: /api/users/124

// Redirect response
Location: https://api.example.com/v2/users

```

### Rate Limiting Headers

**Rate Limit Information:**

```javascript
X-Rate-Limit-Limit: 1000        // Total requests allowed
X-Rate-Limit-Remaining: 999     // Requests remaining
X-Rate-Limit-Reset: 1634294400  // When limit resets (Unix timestamp)
X-Rate-Limit-Retry-After: 60    // Seconds until retry allowed

```

## Status Codes

HTTP status codes provide standardized responses that indicate the result of a client's request. Understanding status codes is essential for building APIs that communicate clearly with clients about success, failure, and various edge cases.

### Status Code Categories

HTTP status codes are grouped into five categories based on their first digit:

```
1xx - Informational responses
2xx - Success responses  
3xx - Redirection responses
4xx - Client error responses
5xx - Server error responses

```

### 1xx Informational Responses

These codes indicate that the request has been received and the process is continuing.

**100 Continue:** Server has received request headers and client should continue with request body.

```javascript
// Rarely used in REST APIs
HTTP/1.1 100 Continue

```

**101 Switching Protocols:** Server is switching protocols as requested by the client.

```javascript
// WebSocket upgrade
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade

```

### 2xx Success Responses

These codes indicate that the request was successfully received, understood, and processed.

**200 OK:** Request succeeded. Most common success response.

```javascript
// GET request successful
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com"
}

// PUT/PATCH request successful
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 123,
  "name": "John Smith",
  "updated_at": "2023-10-15T10:30:00Z",
  "message": "User updated successfully"
}

```

**201 Created:** Request succeeded and a new resource was created.

```javascript
// POST request creating new resource
HTTP/1.1 201 Created
Location: /api/users/124
Content-Type: application/json

{
  "id": 124,
  "name": "Jane Doe",
  "email": "jane@example.com",
  "created_at": "2023-10-15T10:30:00Z"
}

```

**202 Accepted:** Request accepted for processing, but processing not completed.

```javascript
// Asynchronous processing
HTTP/1.1 202 Accepted
Content-Type: application/json

{
  "job_id": "job_123456789",
  "status": "processing",
  "message": "Request accepted for processing",
  "status_url": "/api/jobs/job_123456789"
}

```

**204 No Content:** Request succeeded but no content to return.

```javascript
// DELETE request successful
HTTP/1.1 204 No Content

// PUT request with no response body
HTTP/1.1 204 No Content

```

### 3xx Redirection Responses

These codes indicate that further action must be taken to complete the request.

**301 Moved Permanently:** Resource has permanently moved to a new URL.

```javascript
HTTP/1.1 301 Moved Permanently
Location: https://api.example.com/v2/users/123
Content-Type: application/json

{
  "message": "This endpoint has moved permanently",
  "new_url": "https://api.example.com/v2/users/123"
}

```

**302 Found (Temporary Redirect):** Resource temporarily moved to different URL.

```javascript
HTTP/1.1 302 Found
Location: https://api-backup.example.com/users/123

```

**304 Not Modified:** Resource not modified since last request (used with caching).

```javascript
// Client sends conditional request
GET /api/users/123
If-None-Match: "123456789"

// Server response if not modified
HTTP/1.1 304 Not Modified
ETag: "123456789"
Cache-Control: max-age=3600

```

### 4xx Client Error Responses

These codes indicate that the client made an error in the request.

**400 Bad Request:** Server cannot understand the request due to client error.

```javascript
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "error": "Invalid JSON format",
  "code": "INVALID_JSON",
  "details": "Expected closing bracket at position 45"
}

// Validation errors
{
  "error": "Validation failed",
  "code": "VALIDATION_ERROR",
  "details": [
    {
      "field": "email",
      "message": "Invalid email format"
    },
    {
      "field": "age",
      "message": "Age must be between 18 and 120"
    }
  ]
}

```

**401 Unauthorized:** Authentication is required and has failed or not been provided.

```javascript
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer realm="api"
Content-Type: application/json

{
  "error": "Authentication required",
  "code": "UNAUTHORIZED",
  "message": "Valid access token required"
}

// Expired token
{
  "error": "Token expired",
  "code": "TOKEN_EXPIRED",
  "message": "Access token has expired"
}

```

**403 Forbidden:** Server understands request but refuses to authorize it.

```javascript
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
  "error": "Insufficient permissions",
  "code": "FORBIDDEN",
  "message": "Admin role required to access this resource"
}

// Resource ownership
{
  "error": "Access denied",
  "code": "ACCESS_DENIED",
  "message": "You can only access your own resources"
}

```

**404 Not Found:** Requested resource could not be found.

```javascript
HTTP/1.1 404 Not Found
Content-Type: application/json

{
  "error": "Resource not found",
  "code": "NOT_FOUND",
  "message": "User with ID 123 does not exist"
}

// Endpoint not found
{
  "error": "Endpoint not found",
  "code": "ENDPOINT_NOT_FOUND",
  "message": "The requested endpoint /api/v1/invalid does not exist"
}

```

**405 Method Not Allowed:** HTTP method not supported for the requested resource.

```javascript
HTTP/1.1 405 Method Not Allowed
Allow: GET, POST, PUT
Content-Type: application/json

{
  "error": "Method not allowed",
  "code": "METHOD_NOT_ALLOWED",
  "message": "DELETE method not supported for this resource",
  "allowed_methods": ["GET", "POST", "PUT"]
}

```

**409 Conflict:** Request conflicts with current state of the resource.

```javascript
HTTP/1.1 409 Conflict
Content-Type: application/json

{
  "error": "Resource conflict",
  "code": "CONFLICT",
  "message": "User with email john@example.com already exists"
}

// Concurrent modification
{
  "error": "Concurrent modification",
  "code": "CONFLICT",
  "message": "Resource was modified by another request",
  "current_version": 5,
  "provided_version": 3
}

```

**422 Unprocessable Entity:** Request is well-formed but contains semantic errors.

```javascript
HTTP/1.1 422 Unprocessable Entity
Content-Type: application/json

{
  "error": "Unprocessable entity",
  "code": "UNPROCESSABLE_ENTITY",
  "message": "Age cannot be negative",
  "details": [
    {
      "field": "age",
      "value": -5,
      "message": "Must be a positive integer"
    }
  ]
}

```

**429 Too Many Requests:** Client has sent too many requests in a given timeframe.

```javascript
HTTP/1.1 429 Too Many Requests
Retry-After: 60
X-Rate-Limit-Limit: 100
X-Rate-Limit-Remaining: 0
X-Rate-Limit-Reset: 1634294460
Content-Type: application/json

{
  "error": "Rate limit exceeded",
  "code": "RATE_LIMIT_EXCEEDED",
  "message": "Too many requests. Try again in 60 seconds",
  "retry_after": 60
}

```

### 5xx Server Error Responses

These codes indicate that the server failed to fulfill a valid request.

**500 Internal Server Error:** Generic server error when an unexpected condition was encountered.

```javascript
HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{
  "error": "Internal server error",
  "code": "INTERNAL_ERROR",
  "message": "An unexpected error occurred. Please try again later",
  "request_id": "req_123456789"
}

```

**502 Bad Gateway:** Server received an invalid response from an upstream server.

```javascript
HTTP/1.1 502 Bad Gateway
Content-Type: application/json

{
  "error": "Bad gateway",
  "code": "BAD_GATEWAY",
  "message": "Unable to connect to database service"
}

```

**503 Service Unavailable:** Server is temporarily unable to handle the request.

```javascript
HTTP/1.1 503 Service Unavailable
Retry-After: 120
Content-Type: application/json

{
  "error": "Service unavailable",
  "code": "SERVICE_UNAVAILABLE",
  "message": "Service temporarily unavailable due to maintenance",
  "retry_after": 120
}

```

**504 Gateway Timeout:** Server didn't receive timely response from upstream server.

```javascript
HTTP/1.1 504 Gateway Timeout
Content-Type: application/json

{
  "error": "Gateway timeout",
  "code": "GATEWAY_TIMEOUT",
  "message": "Request to external service timed out"
}

```

### Status Code Selection Guidelines

**Choosing the Right Status Code:**

**For Successful Operations:**

-   **200 OK**: General success, data returned
-   **201 Created**: New resource created successfully
-   **202 Accepted**: Request accepted, processing asynchronously
-   **204 No Content**: Success, no data to return

**For Client Errors:**

-   **400 Bad Request**: Malformed request, invalid JSON, missing required fields
-   **401 Unauthorized**: Authentication missing or invalid
-   **403 Forbidden**: Authenticated but not authorized
-   **404 Not Found**: Resource doesn't exist
-   **409 Conflict**: Resource already exists, concurrent modification
-   **422 Unprocessable Entity**: Valid format but business logic errors
-   **429 Too Many Requests**: Rate limiting

**For Server Errors:**

-   **500 Internal Server Error**: Unexpected server-side errors
-   **502 Bad Gateway**: Problems with upstream services
-   **503 Service Unavailable**: Temporary service outage
-   **504 Gateway Timeout**: Upstream service timeout

