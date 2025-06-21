# Section 3: Most Important Quality Attributes in Large Scale Systems

[Performance](#performance)

[Scalability](#scalability)

- Performance
- Scalability
- Availability
- Testability
- Deployability
- Maintainability
- ...
  
- Portability
- Security
- Observability
- Consistency
- Efficiency
- Usability
- ...

## Performance

### Response Time

> Time between a client sending a request and receiving a response

> Response Time = Processing Time + Waiting Time

- Processing Time
  - Time spend in our system actively process in the request and building / sending the response
- Waiting Time
  - Duration of time request / response spends _inactively_ in our system
  - in transit, network wires, switches, gateways
  - network / software queues
  - a.k.a Latency, in other books Response Time === Latency

Reponse Time = End to End Latency

- Response time is an important metric when the request is in the critical path of a user interaction

---

### Throughput

Example: Distributed Loggin System that aggregates and analyses a constant stream of logs comming from hunders / thousands servers

The most important metric here is the ability to ingest and analyse large amounts of data in a given period of time

More Data ➡️ Better Performance


**Definitions**

- Amount of work performed by our system per unit of time
  - Measured in tasks / seconds
- Amount of data processed by our system per unit of time
  - Measured in bits/second, Bytes/second, MBytes/second

---

### Important Considerations

**1. Measuring Response Time Correctly**

- Processing Time + **Waiting Time**
- take into account waiting time in network or software queues


**2. Response Time Distribution**

- e.g. hunders of servers
- ideally response time samples should be equal, all clients get the same experience
- in practice there is a distribution
  - some very good
  - some surprisingly very bad
- What is the metric that we should
  - Set our goal around
  - Meausure
- Average? / Median? / Maximum?
- Response Time Distribution Histogram

> The "xth percentile" is the _value_ below which x% of the values can be found

e.g. 50th percentile (the median), the repsonse time of 50% of requests was lower than 20ms

The median and the average can completely hide the story of e.g. 20% of users above 80th percentile
- a.k.a tale latency

**Tail Latency**

> The small percentage of response times from a system, that take the longest in comparison to the rest of values

Shorter Tail ➡️ Better

- Define response time goals using percentiles
- Measure and compare to our goals using percentile distribution

**Response Time Goals - Examples**
- 30ms at 95th percentile of response time
- 30ms at 99th percentile of response time


**3. Performance Degradation**

Degradation point is the point at the performace graph, that the performance starts to get significantly worse
as the load increases. It likely means that one or some of our resources are fully utilized.

**Potential overly-utilize resources**
- High CPU utilization
- High memory consumption
- Too many connections / IO
- Message queue is at capacity

---

### Summary

**Performance metrics**
- Response Time: Time between a client sending a request and receiving a response
- Throughput: Amount of work/data processed by our system per unit of time

**Important considerations**
- Proper measuring of response time
- Response time percentile distribution
- Performance degradation

---

## Scalability

### Motivation & Definition

**Traffic Patterns**

- The load/traffic on our system never stays the same
- It can follow different patterns
  - Seasonal pattern (sine wave)
  - Spikes (search engine global events)
  - Day / Night (news website)
  - Weekends low traffic (work tool)
- In the long run, we should expect more users
  - should result in increasing traffic and load

**Scalability Formal Definition**

> The measure of a system's ability to handle a growing amount of work, in an easy and cost effective way,
> by adding resources to the system

**Scalability Graph - Load vs Resources**

- Linear Scalability
  - In practice, very hard to achieve
  - 3 options
    - graph ramp up
    - graph flattening
    - graph ramp down / coherence (least desire)
      - adding more resources will actually give us worst performance
      - overhead of managing and coordinating

**3 Dimension Scaling**

- Scale Up / Vertical Scalability
- Scale Out / Horizontal Scalability
- Team / Organization Scalability

---

### Vertical Scalability

System reaching the point of degradation, one way to solve it is upgrading our service to a faster and newer machine

- faster CPU
- more memory
- network card with higher bandwidth

or 

- upgrade the DB on a stronger harder
  - more storage capacity

> Adding resources or upgrading the existing resources on a single computer, to allow our system to handle higher traffic or load

**Pros**
- Any application can benefit from it
- No code changes are required
- The migration between different machines is very easy

**Cons**
- The scope of upgrade is limited
- We are locked to a centralized system which _cannot_ provide
  - High Availability
  - Fault Tolerance

---

### Horizontal Scalability

Add more units of resources, running our service on multiple computers instead of a single computer

Scale database by running it on multiple machines instead of a single machine

> Adding more resources in a form of new instances running on different machines,
> to allow our system to handle higher traffic or load


**Props**
- No limit on scalability
- It's easy to add/remove machines
- If designed correctly we get
  - High Availability
  - Fault Tolerance
 
**Cons**
- Initial code changes may be required
- Increased complexity, coordination overhead


---

### Team / Organizational Scalability

Regarding the definition of scalability: _a growing amount of work_ could be
- features
- testing
- fixing bugs
- releases

Resources
- Engineers


**Team Productivity as a Function of Number of Engineers**

Throughput / Productivity ➡️ Engineers

- More engineers ➡️ productivity increases
- At some point the more engineers we add  to the team, the less productivity we get

**Reasons for Productivity Degradation**

- Many crowded meetings
- Code merge conflict
- Business Complexity - Longer ramp up time
- Testing is harder and slower
- Releases become very risky

> Software Architecture impacts engineering velocity (team productivity)

**Increase Team Scalability - Attempt #1**

- modules / libraries
  - separate groups of developers can work on each individual module independently and not interfere with each other as much
  - all those pieces are still part of the same service
  - still very tightly coupled
    - especially when it comes to releases


**Increase Team Scalability - Attempt #2**

- services
  - each service has it's own stack / codebase / release schedule
  - services are communicating with each other through loosely coupled networks
  - much better engineering productivity
  - helps us scale the organization
  - breaking monolithic code base into multiple services doesn't come for free

---

### Summary 

Vertical / Horizontal / Team or Organization Scalability are orthogonal to each other
- We can scale to 1 dimention, 2 or 3

- We learned about a very important quality attribute - Scalability
- Motivation - traffic / load may increase
  - over time
  - on the seasonal pattern
  - Sporadically
- Scalability definition: Measure of a system's ability to handle a growing amount of work in an easy and cost effective way by adding resources to the system
- We learned aboute three orthogonal ways to scale our system
  - Vertical Scalability: Adding resources or upgrading the existing resources on a single computer
  - Horizontal Scalability: Adding more resources in a form of new instances running on different machines
  - Team Scalability: increasing productivity while hiring more engineers into the team
    
---

