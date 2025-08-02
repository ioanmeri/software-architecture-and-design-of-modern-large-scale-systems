# Section 5: Large Scale Systems Architectural Building Blocks

- [Load Balancer](#load-balancer)
- [Message Brokers](#message-brokers)
- [API Gateway](#api-gateway)
- [Content Delivery Network - CDN](#content-delivery-network---cdn)

---


## Load Balancer

Role of Load Balancer: **Balance load among a group of servers**

Without a load balancer, the client application will have to know the addresses of the multiple servers, this **tighty couples** the client application to our systems
internal implementation

Added feature: Most balancing solutions also provide an abstraction between the client application and our group of servers. **Single Server Abstraction**

---

## Quality Attributes

**1. Scalability - Auto-scaling**

- In a cloud environment, we can use auto-scaling policies to intelligently add/remove servers based on
  - Requests per second
  - Network bandwidth
 
**2. High Availability**

- Can stop sending traffic to servers, that cannot be reached

**3. Performance (Throughput)**

- When it comes to performance, load balancers may add a little _latency_
- It is an acceptable price to pay for _increased performance_ in terms of throughput

**4. Maintainability**

- we can have a rolling release while still keeping our SLA in terms of availability

---

## Types of Load balancers

### DNS load balancing

- DNS is part of the internet infrastructure that maps human-friendly URLs to IP addresses
  - They can be used by network routers to route requests to individual computers on the web
  - DNS is the "phone book of the internet"
- http://my-online-store.com ➡️ DNS ➡️ IP ➡️ Server Request: 123.53.0.1
- A single DNS record doesn't have to map to a single IP address
  - can be easily configured to return a list of IP addresses
    - 123.53.0.1
    - 123.53.0.2
    - 123.53.0.3
  - DNS returns those IP addresses with different order
  - This way the **Domain Naming System balances the load on our servers by simply rotating this list** in a Round Robin fashion

**Advantages**

- Simple
- Cheap (Comes for free by purchasing a domain name)

**Drawbacks**

- DNS doesn't monitor the health of our servers
  - The list of IP addresses changes based on the TTL configured for that particular DNS record
  - This list of addresses can be cached in different locations such as the client's computer
- The balancing strategy is always just a simple round-robin
  - Some of our applications instances may be running on more powerful servers than others
  - One of our servers may be more overloaded than the other
- The client application gets the direct IP addresses of all our servers
  - This exposes some implementation details of our system
  - It makes our system less secure
  - A malicious client application can pick one IP address and send requests only to that particular server
  - This would overload it more than other servers

---

### Hardware & Software Load Balancers

- Hardware Load Balancers
  - Run on **dedicated devices** designed and optimized specifically for load balancing
- Software Load Balancers
  - **Programs** that can run on a general-purpose computer and perform a load balancing function
- All the communication between the client and the group of servers is done through the load balancer
  - individual IP addresses as well as the number of servers we have behind the load balancer are not exposed to the  users
  - makes our system more secure
- Can actively monitor the health of our servers and send them periodic health checks
  - actively detect if one of our servers became unresponsive

**Software / Hardware Load Balancing Strategies**

- Hardware and Software Load Balancers can balance the load **intelligently** taking into account
  - Different types of hardware
  - Current load on each server
  - Number of open connections

**Software / Hardware Load Balancing - Internal Use**

- SW and HW Load Balancers can be used inside our system
- They can create an abstraction between different services
  - we can scale each service completely independently

**Software / Hardware Load Balancers - Collocation**

They are usually co-located with the group of servers they balance the load on

- if we put the LB too far, we adding extra latency

**Software / Hardware Load Balancers - Multiple Data Centers**

Having only one LB for both groups of servers, will sacrifice the performance for at least one of those locations

**Load balancers - DNS Resolution**

- Load balancers on their own do not solve the DNS resolution problem
- We would still need some kind of DNS solution to map a human-readable domain name to an IP address

---

### Global Server Load Balancing

- GSLB is a hybrid between
  - DNS service
  - Hardware / Software Load Balancer

Can make more intelligent routing decisions. GSLB can figure out the user's location based on the origin IP inside the incoming request.

- has similar monitoring capabilities to a typical Hardware / Software Load Balancer
  - location of each server
  - health of each server

**GSLB - Traffic Routing**

- Most GSLB can be configured to route traffic based on a _variety of strategies_ and not just by physical location
- Since they are in constant communication with our data centers, they can be configured to route users based on
  - The currect traffic
  - CPU load in each data center
  - Best-estimated response time
  - Bandwidth between user and the data center

**Global Server Load Balancing - Disaster Recover**

If there is a natural disaster or a power outage in one of our data centers, the users can be easily routed to different locations
which provides us with higher availability

**GSLB - DNS Load Balancing**

To avoid a LB being a Single Point Of Failure, we can place multiple load balancers and register all the addresses with the GSLB DNS service

---

### Summary

- We learned about a very important software architecture building block, the **Load Balancer**
- We learned about four load balancing solutions, which are
  - DNS load balancing
  - Hardware load balancing
  - Software load balancing
  - Global Server Load Balancing
- We talked about the different _quality attributes_ that a load balancer provides to our system
- We saw how we can combine all those different solutions to architect a large-scale system that can
  - Provide _high performance_ and _high availability_
  - Scale to millions of users located in _different geographical locations_

---

## Message Brokers

### Synchronous Communication Drawbacks


Sender Application ➡️ Connection ➡️ Load Balancer ➡️ Connection ➡️ Receiver Application

- Both application instances have to remain healthy and maintain this connection to complete the transaction
  - It's easy to achieve this when two services exchange small messages that take short time to process and respond
  - It can get complex when a service takes a long time to complete its operation and provide a response 


![Synchronous processing](assets/images/04.png)


- No padding in the system to absorb a sudden increase in traffic or load

---

### Message Broker

- A software architectural building block that uses the _queue data structure to store messages_ between senders and receivers
- A message broker is used _inside our system_ and not exposed externally

--

### Message Broker - Capabilities

- Basic capabilities
  - Storing / temporarily buffering the messages
  - Message routing
  - Transformation validation
  - Load balancing

---

### Loose Coupling Between Senders & Receivers

- Entirely _decouple_ senders from the receivers
- The fundamental building block of **asynchronous** software architecture

The client will receive an acknowledgement immediately after placing the order.

Later will get an email asynchronously with an official confirmation for purchasing the ticker.

We can break the ticket reservation service into multiple services, each for one stage in the transaction.

Frontend Service ➡️ Message Broker ➡️ Reservation Service ➡️ Message Broker ➡️ Billing Service ➡️ Message Broker ➡️ Email Service


---

### Online Store - Buffering

The important benefit that a message broker provides us with is buffering of messages to absorb traffic spikes

---

### Message Broker - Benefits

- Most message broker implementations offer the _publish / subscribe pattern_ where multiple services can
  - Publish messages to a particular channel
  - Subscribe to that channel
  - Get notified when a new event is published

---

### Online Store - Pub / Sub

![Online Store Pub Sub](assets/images/05.png)

---

### Fault Tolerance

- A message broker adds a lot to fault tolerance to our system
- It allows different services to communicate with each other while some of them may be unavailable temporarily
- Message brokers prevent messages from being lost

---

### Availability and Scalability

- The additional fault tolerance helps us provide high availability to our users
- A message broker can queue up messages when there is a traffic spike
- It allows our system to scale to high traffic

---

### Performance

- We pay a little in performance when it comes to latency
- A message broker adds significant indirection between two services
- This performance penalty is not too significant for most systems

---

### Summary

- We learned about a fundamental building block to any asynchronous software architecture - Message Broker
- We got the motivation for using a message broker
- We talked about different benefits and capabilities
  - Asynchronous architecture
  - Buffering
- We talked about the two main quality attributes
  - _High availability_
  - _Scalability_

---

## API Gateway

### Video Streaming and Sharing System Example

- Upload Videos
- Watch Videos
- Comment on Videos

**Intial: Video Streaming and Sharing Service**

- Serve Frontend: HTTP, JS
- User Profile Management
- Channel Subscriptions
- Notifications
- Video Storing and Streaming
- Users Comments
- Security

Too Complex!

**Stage 1: Splitting up Services**

- Frontend Service
- Users Service
- Video Service
- Comments Service

---

### Consequences of Splitting a Service

- The single API now split into _multiple_ APIs
- So now we need to
  - Update the frontend code to be aware of the internal organization of our system
  - Make calls to different services on the browser depending on the task


**Home Page Request**

User ➡️ Frontend Service + Users Service


**Watch a Movie Request**

User ➡️ Frontend Service + Video Service + Comments Service


Now, it's service needs to implement it's own security, authentication and authorization
- adds performance overhead
- duplication


---

### API Gateway

API Gateway is API management service, that seats in between the client and a collection of backend services.

- The API Gateway follows a software architecture pattern called _API composition_
- We compose all the APIs of all our services into one single API
- The client applications can call one single service

---

### API Gateway - Benefits

- Seamless internal modifications / Refactoring
  - e.g. spliting the Frontend Service
    - Desktop Frontend Service
    - Mobile Frontend Service
  - Video Splitting Service to
    - High Resolution Service
    - Low Resolution Service
- Consolidating all security, authorization, and authentication in a single place
  - Depending on the permissions and the role of the user, we can allow different operations such as
    - Viewing private videos
    - Deleting / uploading new videos
  - We can also implement _rate-limiting_ at the API Gateway to block Denial of Service (DoS) attacks from malicious users
- Request Routing
  - We save the overhead of authenticating every request from the user at each service by performing it in a single place
  - We can also save the user from making multiple requests to the different services
- Static content and response caching
- Monitoring and alerting
- Protocol Translation
  - e.g. Externally expose REST API JSON, internally can use gRPC / PROTO Buffer
  - Legacy service that support HTTP 1.0 REST API and represent their objects using XML
  - Third parties which have systems that support Thrift / SOAP and want to use our API protocol and formats

---


### API Gateway - Considerations

- API Gateway **shouldn't contain any business logic**
  - The main purpose of an API Gateway is
    - API composition
    - Routing requests to different services
  - **Anti Pattern**: Following the anti-pattern of adding business logic to our API Gateway will make it too smart
    - We may end up again with a single service that
      - Does all the work
      - Contains an unmanageable amount of code
    - This was the problem we wanted to solve initially by splitting our system into _multiple services_
- API Gateway may become a **Single Point of Failure**
  - We can solve
    - Scalability
    - Availability
    - Performance
    - by deploying multiple instances of API Gateway and placing them all behind a _load balancer_
  - Our entire system becoms unavailable of the client if
    - We push a bad release
    - Introduce a bug that may crush the API Gateway
  - Eliminate any possibility of human error
  - Deploy new releases to the API Gateway with caution
- Avoid bypassing API Gateway from external services

---

### Summary

- We learned about a important architectural building block and design pattern, the _API Gateway_
- We learned about the benefits of an API Gateway such as
  - _API composition_
  - _Security_
  - _Caching_
  - _Monitoring_
- We talked about considerations for correctly using an API Gateway which are
  - Keeping business logic out
  - Being careful in making modifications
  - Not breaking the abstraction by making all external API calls through API Gateway only
 
---

## Content Delivery Network - CDN

### Multi Data Center Deployment

Even with distributed web hosting across multiple datacenters, which are powered by technologies like Global Server Load Balancing,
there is still a significant latency caused by the physical distance between an end user and the location of the hosting servers

**Multiple Network Hops**

Additionally, each request between an end user and the destination server has to go between multiple hops between different network routers.
This adds even more to the overall latency

---

### CDN - Motivation

- A study by Google Analytics indicated

> 53% of mobile users abandoned a website if it took longer than **3 seconds** to load

- We can improve our system's performance by
  - Replicating our service
  - Running it on more data centers
- But our service is not what the users need closer but instead
- The static content like
  - Images
  - HTML pages
  - Javascript
  - CSS files
  - Videos

are what we need to get closer to the users to get them to load faster

---

## CDN - Content Delivery Network

- Globally distributed network of servers
- Located in strategic places
- Main purpose: Speeding up the delivery of content to end-users

--- 

### Wide World Wait - Problem

- CDNs were originally created to address the problem referred to as the "Wide World Wait"
- It is a term describing a bad user experience caused by
  - Slow internet connection
  - Overloaded web server

---

### CDN Edge Servers Advantages

- CDNs provide service by _caching_ our website content on their _edge servers_ which are relocated at different _Points of Presence (PoP)_
- Those _edge servers_ are
  - Physically closer to the user
  - More stategically located in terms of network infrastructure
- This allows them to
  - Transfer content much quicker to the user
  - Improve our perceived system perfromance
  
---

### CDN - Types of Content

- CDNs can be used to deliver
  - Images
  - Text
  - CSS
  - Javascript files
  - Video streams (live an on-demand)

--- 

### CDN - Use Cases

- CDNs are used by _digital service companies_ that interact with users through website / mobile app like
  - E-commerce services
  - Financial institutions
  - Technology and software as a service (SaaS) companies
  - Media copmanies delivering video streaming / news
  - Social media

---

### CDN - Quality Attributes

- Performance - Faster page loads
- High availability - issues/slowness are less noticeable
- Security - Protection against _Distributed Denial of Service_ attacks (DDoS)

**Example**

A TCP three-way handshake without CDN can be 200 + 200 + 200 but with CDN (which is physically closer) 
is 50 + 50 + 50 for a user in Brazil connecting to a US server,
and for all the assets the total latency is 800ms instead of 2500ms without CDN.

---

### CDN - Additional Techniques

- CDN providers use _faster_ and _more optimized_ hard drives to store the cached content
- Reduce bandwidth by compressing the content delivered using algorithms like
  - Gzip
  - Minification of javascript files


---

### Content Publishing Strategies

**1. Pull Strategy**

- We need to tell the content delivery network provider
  - Which content we want on our website to be cached
  - How often this cache needs to be invalidated
- It is configured by setting a Time to Live (TTL) property on each asset/type of asset

**Example**

End User ➡️ Request ➡️ CDN ➡️ Image ⬅️ Server ⬅️ Image

The first time the user requests a certain asset, the CDN will have to populate it's cache by sending a request to a server in our system.

Subsequest Requests by users will be served by the Edge Servers directly

When the user requests an asset that has already expired, the CDN will send a request to our servers to check if we have a new version of that asset

If the version of the asset did not change, the CDN will simply refresh the expiration time for that asset and serve it back to the user.

If a new version of that asset is available, the server will send it back to the CDN and the CDN will serve the new version instead of the old one

**Pull Strategy - Advantages**

- Lower maintenance on our part
- No need to keep the CDN caches up to date once we configure
  - Which assets need to be cached
  - How often the assets need to expire
- Everything is taken care of by the CDN provider
  

**Pull Strategy - Drawbacks**

- The first users to use an asset that hasn't been cached yet will have a longer latency
- CDN needs time to fetch the non-cached asset from our system
- We may see frequent traffic spikes when all the assets expire in the same time
- We still need to maintain a general high availability of our system
- The user will get an error if
  - Certain assets expire on the CDN
  - Our system isn't available for the CDN to pull a new version

---


**2.Push Strategy**

Manually or automatically upload or publish the content through a CDN, republish the new version to the edge servers

- Some content delivery network providers support this model _directly_
- Others enable this strategy by _setting a long TTL_ for our assets so that the cache never expires
  - Whenever we want to publish a new version we **purge** the content from the cache
  - This forces the CDN to fetch that content from our servers whenever a user requests it

**Push Strategy - Advantages**

- If our content doesn't change frequently we can simply push it _once_ to the CDN
- This significantly reduces
  - The traffic to our system
  - The burder for our system to maintain high availability
- Even if our system goes down temporarily, users will still get all the data from the CDN and won't be affected


**Push Strategy - Drawbacks**

- If our content changes frequently, we have to actively publish new versions of the content to the CDN
  - Otherwise, users will get stale and out-of-date content which is undesired
  
---

### Summary

- We learned about the _Content Delivery Network_ which is a service used in conjuction with our system to improve users' experience
- We talked about different _features_ and _capabilities_ of CDNs which add to our systems
  - Performance
  - Availability
  - Security
- We talked about the two strategies for _publishing our content_ to a CDN which are
  - Pull Strategy
  - Push Strategy
- We compared the two by listing the advantages and disadvantages of each publishing model

---






