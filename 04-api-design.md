# Section 4: API Design

- [Introduction to API Design for Software Architects](#introduction-to-api-design-for-software-architects)

---

## Introduction to API Design for Software Architects

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

**Categories of API**

- APIs are classified into three groups
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

**Benefits of a well designed API**
1. Client who uses it can immediately and easily enhance their business by using our system
2. They need not know anything about our system's internal design / implementation
3. Once we define and expose our API, _clients can integrate with us_ wihout waiting for full implementation of our system
4. API makes it easier to design and architect the internal structure of our system
4.a It defines the endpoints to the different routes that the user can take to use our system

**API best practices and patterns**

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
