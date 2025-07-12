# Section 4: API Design

- [Introduction to API Design for Software Architects](#introduction-to-api-design-for-software-architects)

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

**2. Easy to Use**
- Easy to Use
- Easy to understand
- Imposssible to misuse
- The ways to make an API simple can be
  - Only one way to get certain data / perform a task
  - Descriptive names for actions and resources
  - Exposing only the information and actions that users need
  - Keeping things consistent all across our API

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

**6. Versioning our API**

- Best API design allows us to make _changes to the internal design and implementation_ without changing the API
- In practice, we may need to make _non-backward compatible API changes_
- If we explicitly version the APIs we can
  - Maintain two versions of the API at the same time
  - Deprecate the older one gradually

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





