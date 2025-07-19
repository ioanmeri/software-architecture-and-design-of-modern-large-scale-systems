# Section 4: API Design

- [Introduction to API Design for Software Architects](#introduction-to-api-design-for-software-architects)
- [RPC](#rpc)
- [REST API](#rest-api)

---

## Introduction to API Design for Software Architects

### Introduction to API Design

**What is an API?**
- After capturing all functional requirements, we can think of our system as a _black box_
- The _black box_ has
  - Behavior
  - Well-defined interface
- That _interface_ is a contract between
  - Engineers who implement the system
  - Client applications who use the system
- Since this _interface_ is called by other applications, it is referred to as an **Application Programming Interface** or **API**
- In a large-scale system, API is called by other applications remotely through the network
- The applications calling our API may be
  - _Frontend clients_ like mobile applications / web browsers
  - _Backend systems_ that belong to other companies
  - _Internal systems_ within our organization
- Each component of our system will have its own API

---

### Categories of API

APIs are classified into three groups
- Public APIs
- Private / Internal APIs
- Partner APIs
 
**1. Public APIs**
  - Exposed to the general public
  - Any developer can use/call them from their application
  - Good practise
    - Requiring the users to _register_ with use before allowing to send requests and use the system
  - This allows
    - Control over _who_ uses the system externally
    - Control over _how_ they use the system
    - Better security
    - To blacklist users breaking rules

**2. Private APIs**
- Exposed only internally within the company
- They allow other teams / parts of the organization to
  - Take advantage of the system
  - Provide bigger value for the company
  - Not expose the system directly outside the organization

**3. Partner APIs**
- Similar to Public APIs
- Exposed only to companies/users having business relationship with us
- The business relationship is in the form of
  - Customer Agreement after buying our product
  - Subscribing to our service

---

### Benefits of a well designed API

1. Client who uses it can immediately and easily enhance their business by using our system
2. They need not know anything about our system's internal design / implementation
3. Once we define and expose our API, _clients can integrate with us_ wihout waiting for full implementation of our system
4. API makes it easier to design and architect the internal structure of our system
5. It defines the endpoints to the different routes that the user can take to use our system

---

### API best practices and patterns

**1. Complete Encaplusation**

- _Complete Encapsulation_ of the Internal design and implementation
- Abstracting it away from a developer wanting to use our system
- if client wanting to use our API
  - Requires any information about how it is implemented internally
  - Needs to know our business logic to use it
  - Then, the whole purpose of the API is defeated
- API should be _completely decoupled_ from our internal design and implementation
- We can _change the design_ later without breaking the contract with our clients

---

**2. Easy to Use**
- Easy to Use
- Easy to understand
- Imposssible to misuse
- The ways to make an API simple can be
  - Only one way to get certain data / perform a task
  - Descriptive names for actions and resources
  - Exposing only the information and actions that users need
  - Keeping things consistent all across our API

---

**3. Keeping the Operations Idempotent**

> An operation doesn't have any additional effect on the result if it is performed more than once


**Idemponent Operations - Example**

- Updating the user's address to a new address is an Idempotent Operation
- The result is the same regardless of performing it any number of times

**Non-Idempotent Operations - Example**
- Incrementing a user's balance by a hundred dollars is **not** an Idempotent Operation
- The result will be different depending on the number of times we perform it

- Idempotent Operations are preferred for our API as they are going to be used through the network
- if the client application sends us a message
  - The message can be lost
  - The response to that message may be lost
  - The message wasn't received because a critical component in our system went down
- Because of _network decoupling_, the client application has no idea which scenario actually happened
- if our operation is idempotent, they can simply _resend the same message_ again without any consequences

---

**4. API Pagination**

- Important when a response from our system to the client request contains a very large payload or dataset
- Without pagination most client applications will
  - Not be able to handle big responses
  - Result in a poor user experience
- Imagine what would happen if
  - After opening your email account, you see all the emails you ever received instead of the latest emails
  - After searching for an item on online store / search engine, you see too many items that matched your query
- The client application / web browser is unlikely to handle so much data
- It would take an unreasonable time to show all those results
- Pagination allows the client
  - To request only a small segment of the response
  - Specify the maximum size of each response from our system
  - Specify an offset within the overall dataset
- To receive the next segment we increment the offset

---

**5. Asynchronous Operations**

- Some operations need one big result at the end
- Nothing meaningful can be provided before the entire operarion finishes
- Examples of such operations can be
  - Running a big report requiring our system to talk to many databases
  - Big data analysis that scans a lot of records / log files
  - Compression of large video files
- If the operation takes a long time, the client application has to wait for the result
- The pattern used for these situations is an _Asynchronous API_
- A client applications _receives a response immediately_ without having to wait for the final result
- That response includes some kind of identifier that allows
  - To track the progress and status of the operation
  - Receive the final result

---

**6. Versioning our API**

- Best API design allows us to make _changes to the internal design and implementation_ without changing the API
- In practice, we may need to make _non-backward compatible API changes_
- If we explicitly version the APIs we can
  - Maintain two versions of the API at the same time
  - Deprecate the older one gradually

---

**Defining our API**

- We can define our API in any way as long as we adhere to these best practices
- A few types of APIs have became more standard in the industry

---

### Summary

- API are a _contract that we define for other applications_ so they can use our system without having to know anything about its _internal design and implementation_
- The three categories of APIs are
  - Public APIs
  - Private APIs
  - Partner APIs
- Best practises and patterns for designing a good API are
  - Complete Encapsulation
  - Ease of use
  - Making our operations idempotent
  - API Pagination
  - Asynchronous Operations
  - API versioning

---

## RPC

### How RPC works

**Remote Procedure Call (RPC)**

Client Application ➡️ Remote Procedure Call ➡️ Server Application  ➡️ Subroutine ➡️ Response

---

**Unique Features of RPC**

- The remote method invocation looks like calling a normal local method in terms of the developer code
- This is referred to as _location transparency_
- To the developer of the client application a method executed _locally_ or _remotely_ looks the same
- RPC frameworks support _multiple_ programming languages
- Applications written in different programming languages can talk to each other using RPC

---

**How RPC Works**

Client ➡️ Interface Description Language ➡️ Server

```
debicAccount(UserInfo userInfo, int32 amount) -> Response

UserInfo {
  String name, lastName;
  String creditCardNumber;
  int32 securityCode;
  ...
}


Response {
  bool success;
  String errorMessage;
}

```

➡️ RPC Code Generation Tool ➡️ Generated Code ➡️ Server (Server Stub)

➡️ RPC Code Generation Tool ➡️ Generated Code ➡️ Client (Client Stub)

---

**Data Transfer Objects (DTO)**

DTOs (auto-generated) are compiled to classes or structs depending on the programming languages

**RPC in Action**

Client ➡️ `Response response = debitAccount(userInfo, 100);` 

➡️ Client Stub

➡️ Data Serialization / Marshalling

➡️ Transport layer

➡️ Data Deserialization / Unmarshalling

➡️ Server Stub

➡️ Debit Account Implementation

---

**RPC Over Time**

- This concept of implementing an API using an RPC has been around for decades
- The only thing that changes over time are
  - The frameworks
  - The details of their implementation
  - Their efficiency

---

**RPC and Developers**

- Our job as API developers is
  - To pick an appropriate framework
  - Define the API and the relevant data types using IDL
  - Publish that description

---

## Benefits of RPC

- Convenience to the developers of the client applications
- They can communicate with our system easily by _calling methods on objects_ similar to calling _normal_, local methods
- The details of _communication establishment_ or _data transfer between client to server_ are abstracted away from the developers
- Failures in communication with server result is an _error or exception_ depending on the programming language

---

## Drawbacks of RPC

- Unlike local methods executed on the client-side, remote methods are
  - Slower
    - The client never knows how long those remote method invocations can take
    - Slowness can be addressed by introducing _asynchronous versions_ for slow methods
  - Less reliable
    - The client is remotely running on a computer and is using the network to communicate with our system
    - Carelessness in designing the API can introduce confusing situations for the client application developers

**Example**

Online Store Company ➡️ `debitAccount()` ➡️ API ➡️ Server ➡️ Credit Card Company

A failure or an exception in calling such a method can leave them with a dilemma of whether they should retry calling the method (and running the risk of charging the user twice) or not retrying it (and running the risk of not charging the user at all)

- There is no real way for the client to know whether
  - The server _received the message_ and the _acknowledgement message got lost_ in the network
  - The _server crushed_ and _never received the message_
- To solve the unreliability problem, we can stick to the best practice of _making our operations idempotent_ when possible

---

### When to Use RPC

- RPC are used in communication between _two backend systems_
- Frameworks that support RPC from frontend clients are less common
- RPC is a perfect choice for
  - API provided to a different company instead of an end user app/web page
  - Communication between different components within a large system
  - Abstracting away the network communication and focusing only on the actions the client wants to perform
- RPC approach would not be a good fit
  - Where we don't want to abstract the network communication away
  - When we want to take direct advantage of HTTP cookies or headers


**Importance of RPC**

- RPC revolves more around _actions_ and les around _data/resources_
- In RPC, every action is a new method with a _different name and signature_
- We can define many methods / actions _without limitation_

**Other API Styles**

- Other styles of APIs can be a better fit when
  - Designing an API that is more _data-centric_
  - All the operations needed are simple _CRUD (Create, Read, Update, Delete)_ operations

---

## Summary

- RPC - Remote Procedure Calls
- 3 Components
  - Interface Description Language (IDL)
  - Client Stub
  - Server Stub
- Benefits
  - Local Transparency
  - Network communication abstraction
- Drawbacks
  - Slowness
  - Unreliability

---

## REST API

### REST API - Introduction

- A new style that originated from a dissertation published by Roy Fielding in 2000
- REST stands for _Representational State Transfer_
- Set of **architectural constraints** and **best practices** for defining APIs for the web
- It is NOT a standard or a protocol
- It is an architectural style for designing APIs that are easy for our clients to use and understand
- It makes it easy for us to build a system with quality attributes such as
  - Scalability
  - High availability
  - Performance

---

### RESTful API

> An API that obeys the REST architectural constraints is called a RESTful API

---

## RPC vs REST API

**The RPC API style**

- Resolves around **methods** that are exposed to the client
- **Methods** are organized in an interface / set of interfaces
- Our system is abstracted away from the client through a set of **methods** the client can call
- API expansion through adding more **methods**

**REST API style**

- Takes a more _resource_-oriented approach
- The main abstraction to the user is a **named resource**
- The **resources** encapsulate different entities in our system
- REST API allows the user to manipulate those _resources_ through small number of methods


Client ➡️ HTTP Request ➡️ Server ➡️ Representation of the Resource's State ➡️ HTTP Response

Only returns a Representation of the state, resource Implementation can be implemented in a completely different way

---

### RPC vs REST API - HATEOAS

- REST API is dynamic in nature
- In RPC, the actions the client can take regardless of its state are statically defined ahead of time by IDL
- In RESTful APIs, this interface is a lot more dynamic through **Hypermedia as the Engine of the Application State** (HATEOAS)
- Achieved by accompanying a state representation response to the client with hypermedia links

---

### Hypermedia as the Engine of Application State

```
GET .../users/john-smith

Response:
{
  "accounts_status": {
    "user_id": 123,
    "total_incoming_messages": 1003,
    "unread_messages": 5,
    "total_sent_messages": 567
  },
  "links": {
    "messages": "/users/123/messages",
    "profile": "/users/123/profile",
    "threads": "/users/123/threads"
  }
}
```
---

### REST API Quality Attributes

**1. Statelessness**

- The server is stateless
- It does NOT maintain any session information about client
- Each message should be served in isolation without any information about previous requests

If the server doesn't maintain any session information, we can easily **run a group of servers** and spread
the load of the request from the client among a large number of machines, completely transparently to the client

**2. Cacheability**

- The server has to either explicity / implicitly define _each response_ as either
  -  Cacheable
  -  Non-cacheable

Allows the client to eliminate the potential round trip to the server and back, if the response is cached somewhere closer to the client / reduce the load to our system

---

### Named Resources

- Each resource is **named** and **addressed** using a **URI** (Uniform Resource Identifier)
- The resources are organized in a hierarchy
- Each resource is either
  - _Simple resource_
  - _Collection resource_

---

### Hierarchy of Resources

- The hierarchy is represented using "/"
- **A simple resource**
  - Has a state
  - Can contain one / more sub-resources
- **A collection resource**
  - Contains a list of resources of the _same type_

---

### Named Resources - Example

```
http://best-movies-service/movies <- Collection Resource

http://best-movies-service/movies/movie-01 <- Simple Resource of Type Movie
http://best-movies-service/movies/movie-02 <- Simple Resource of Type Movie
http://best-movies-service/movies/movie-03 <- Simple Resource of Type Movie
...
http://best-movies-service/movies/movie-20 <- Simple Resource of Type Movie

http://best-movies-service/movies/movie-01/directors <- Collection Resource
http://best-movies-service/movies/movie-01/actors <- Collection Resource

http://best-movies-service/movies/movie-01/actors/john-smith
http://best-movies-service/movies/movie-01/actors/john-smith/profile-picture
http://best-movies-service/movies/movie-01/actors/john-smith/contact-information
```

---

### Representation of Resources

- The representation of each resource state can be expressed in a variety of ways such as
  - An image
  - A link to a movie stream
  - An object
  - An HTML page
  - Binary blob
  - Executable code like javascript

---

### Resources - Best Practices

**1. Naming our resources using nouns**

- It makes a clear distinction from the actions that we're going to take
- We're going to use verbs for the actions on those resources

**2. Making a distinction between collection resources and simple resources**

- The distinction is made by using
  - Plural names for _collections_
  - Singular names for _simple resources_

**3. Giving the resources clear and meaningful names**

- The users will find it very easy to use our API
- It will help prevent incorrect usages and mistakes
- Overly generic collection **should be avoided**
  - elements
  - entities
  - items
  - instances
  - objects

**4. The resource identifiers should be unique and URL friendly**

- They can be used easily and safely on the web


---

### REST API Operations

- Unlike RPCs, the REST API limits the number of methods we can perform on each resource
- These predefined operations are
  - **Creating** a new resource
  - **Updating** an existing resource
  - **Deleting** an existing resource
  - **Getting** the current state of the resource (list of sub-resources in case of collection resource)
 
---

### REST Operations to HTTP Methods

- REST operations are mapped to HTTP methods as follows
  - **Create** a new resource                   ➡️ POST
  - **Update** an existing resource             ➡️ PUT
  - **Delete** an existing resource             ➡️ DELETE
  - **Get** the state of a resource             ➡️ GET
  - **List** the sub-resources of a collection  ➡️ GET
- In some situations, we define additional custom methods

---

### HTTP Methods - Guarantees

1. **GET** method is considered **safe** - applying it to a resource would not change its state
2. **GET,PUT,DELETE** methods are **idempotent** - applying them multiple times would result in the same state change as applying them once
3. **GET** requests are considered cacheable by default
4. Responses to **POST** requests can be made cacheable
   
---

### Sending Additional Information

- To send additional information to our system as part of a **POST** or **PUT** command use
  - JSON
  - XML

---

## Defining REST API Step by Step

**Movie Streaming Service - Example**

1. Identifying Entities
2. Mapping Entities to URIs
3. Defining Resources' Representations
4. Assigning HTTP Methods to Operations on Resources


**1. Entities in Movie Streaming Service**

- Users
- Movies
- Reviews
- Actors


**2. Mapping Entities to URIs**

- Users
  - `/users`
  - `/users/{user-id}`
- Movies
  - `/movies`
  - `/movies/{movie-id}`
- Actors
  - `/actors`
  - `/actors/{actor-id}`
- Reviews
  - `/movies/{movie-id}/reviews`
  - `/movies/{movie-id}/reviews/{review-id}`


**3. Defining Resources' Representations**

```
GET /movies

{
  "movies": [
    {
      "name": "Pirates of the Caribbeans",
      "id": "movie-123"
    },
    {
      "name": "Lord of the Rings",
      "id": "movie-456"
    }
    ...
  ]
}
```

```
GET /movies/{movie-id}

{
  "movie-info": {
    "name": "Pirates of the Caribbeans",
    "Id": "movie-123"
  },
  "links": {
    "movie-stream": "...",
    "reviews": "/movies/movie-123/reviews",
    "actors": "/actors?movie-id=movie-123"
  }
}
```

**4. Assigning HTTP Methods to Operations on Resources**

Users
- POST `/users`             ➡️ Create New User
- GET `/users/{user-id}`    ➡️ Get User Information
- PUT `/users/{user-id}`    ➡️ Update User Information
- DELETE `/users/{user-id}` ➡️ Delete Existing user

---

## Summary

- We learned about a new style of API which is called _Representational State Transfer_ or REST
- We compared the REST API to the general RPC approach
  - More resource-oriented
  - Limits the number of operations we can perform on those named resources to just a few methods
- REST API requirements allow us to provide
  - High performance
  - High availability
  - Scalability
- What resources are, how they're organized and what operations we can perform on them
- We provided a step-by-step process on defining the REST API using a real world example

---







