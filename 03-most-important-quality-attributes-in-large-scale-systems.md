# Section 3: Most Important Quality Attributes in Large Scale Systems

[Performance](#performance)

[Scalability](#scalability)

[Availability](#availability)

[Fault Tolerance & High Availability](#fault-tolerance--high-availability)

[SLA / SLI / SLOs](#sla--sli--slos)

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

---

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

## Availability

### Importance and Risks

- Availability is an important attribute when designing a system
- It has the greatest impact on
  - Our users
  - Our business
- If we provide services to other companies, the impact of outage is compounded

**Examples**
- Users try to purchase from online store but:
  - The page doesn't load
  - They get an error at checkout
- Users lose access to their email for considerable time
- AWS simple storage service had an outage in February 2017
  - Since many companies and websites used the service, it almost took down the entire internet


**Mission Critical Services**
- System downtime is not always just an inconvenience
- Our software can be used for mission-critical service like
  - Air traffic control in airports
  - Healthcare in hospitals
- if it crashes/becomes unavailable for prolonged periods, people's lives are on line

**Impact of Downtown on the Business**
- For a business, the consequences of our system going down are
  - If users cannot use our system, _our profits become zero_
  - If users lose access constantly, _they go to our competitors_
- Thus, downtime of our system for a business results in a loss of
  - Money
  - Customers

---

### Availability Definition and Formulas

> The fraction of time/probability that our service is operationally functional and accessible to the user

- **Uptime** = Time that our system is operationally functional and accessible to the user
- **Downtime** = Time that our system is unavailble to the user
- **Availability** (in %) = Uptime / (Entire time our system is running)

> Availability = Uptime / (Uptime + Downtime)

Sometimes Uptime in (%) is mentioned as Availability 


**Alternative Way to Calculate / Estimate Availability**

- **MTBF**
  - **Mean Time Between Failures**
  - Represents the average time our system is operational
  - Useful when
    - Dealing with multiple pieces of hardware
    - Each component has its own operatinal shelf life
    - We are not using cloud/third-party API with a given uptime and availability
- **MTTR**
  - **Mean Time to Recovery**
  - Time average it takes us to detect and recover from a failure
  - Average downtime of our system

> Availability = MTBF / (MTBF + MTTR)

- More statistical and talks about probabilities
- More useful for availability estimation rather than measurement
- In practice, MTTR cannot be zero
- Shows that detectability and fast recovery can help us achieve high availability


---

### What is High Availability

- Users would want 100% availability
- On the engineering side it is
  - Extremely hard to achieve
  - Leaves no time for maintenance / upgrades


**Availability vs Downtime**

| Availability | Daily Downtime | Weekly Downtime | Monthly Downtime | Annual Downtime |
| ----------- | ----------------|----------------|------------------|-----------------|
| 90%      | 2h 24m | 16h 48m | 3d | 36d 12h |
| 95%      | 1h 12m | 8h 24m | 1d 12h | 18d 6h |
| 99%      | 14m 24s | 1h 40m 48s | 7h 18m | 3d 15h 36m  |
| 99.9%    | 1m 26s | 10m 5s | 43m 12s | 8h 45m 36s |


**Industry Standard of Availability**

- No strict definition of what constitutes high availability
- Industry standard set by cloud vendors is typically 99.9% or higher

**The "nines"**
- Percentages are referred to by the number of "nines" in their digits
- For Example
  - 99.9% - 3 "nines"
  - 99.99% - 4 "nines"

---

### Summary

- Importance of high availability both from a user and a business perspective
- Two risks that our business faces if our system goes down
  - Loss of revenue
  - Loss of customers to our competitors
- > "The fraction of time/probability that our service is operationally functional and accessible to the user"
- Two formulas to measure and estimate the system's availability
  - > Availability = Uptime / (Uptime + Downtime)
  - > Availability = MTBF / (MTBF + MTTR)
- High availability based on cloud vendor standards is 99.9% or "3 nines"
  
---

## Fault Tolerance & High Availability

### What Prevents us from achieving HA - Sources of Failure

**1. Human Error**

- Pushing a faulty config in production
- Running the wrong command/script
- Deploying an incompletely tested new version of software

**2. Software Errors**

- Long garbage collections
- Out-of-memory exceptions
- Null pointer exceptions
- Segmentation faults

**3. Hardware Failures**

- Servers / routers / storage devices breaking down due to limited shelf-life
- Power outages due to natural disasters
- Network failures because of
  - Infrastructure issues
  - General congestion

---

### Introduction to Fault Tolerance

- Failures will happen despite
  - Improvements to our code
  - Review, testing, and release processes
  - Performning ongoing maintenance to our hardware
- _Fault Tolerance_ is the best way to achieve _High Availability_ in our system

**Fault Tolerance**

> Fault Tolerance enables our system to remain operational and available to the users
> _despite_ failures within one or multiple of its components

- When failures happen a _fault-tolerant_ system will
  - Continue operating at the same / reduced level of performance
  - Prevent the system from becoming unavailable
 
**Tactics for achieving Fault Tolerance**

- Fault Tolerance resolves around 3 major tactics
  - Failure Prevention
  - Failure Detection and Isolation
  - Recovery

---

## 1. Failure Prevention

- To prevent our entire system from going down, eliminate any **Single Point of Failure** in our system
- Examples of a Single Point of Failure can be
  - _One server_ where we're running our application
  - Storing all our data on the _one instance_ of our databases that runs on a _single computer_
- Best way to eliminate a single point of failure is through _Replication and Redundancy_

**Examples**
- Traffic ➡️ Multiple instances of server ➡️ If an instance goes down, we can redirect traffic to other instances
- Traffic ➡️ Multiple DBs Replicas ➡️ If a replica goes down, we don't lose any data

**Types of Redundancy**

- **Spatial Redundancy** - Running replicas of our application on different computers
- **Time Redundancy** - Repeating the same operation / request multiple times until we succeed / give up

---

### Strategies for Redundancy and Replication

Two strategies which are extensively used in the industry in different systems

- Active-Active architecture
  - e.g. Requests go in all DB replicas, synced with each other
  - Advantages
    - Load is spread among all the replicas
    - Identical to horizontal scalability
    - Allows more traffic / Better performance
  - Disadvantages
    - All the replicas are taking requests
    - Additional coordination required to keep active replicas in sync
- Active-Passive architecture
  - Passive replicas take periodically snapshots (follow) of the primary replica
    - Advantages
      - Implementation is easier
      - There is a clear leader with up-to-date data
      - Rest of the replicas are followers
    - Disadvantages
      - Ability to scale our system is lost
      - All the requests still go to only one machine

---

## 2. Failure Detection & Isolation

If one of the instances crashes because of software / hardware issues, then we need the ability
to detect that this instance **is faulty and isolate** it from the rest of the group

This way we can take actions, like stopping requests from going to this particular server

Typically to detect such failures, we need a **Monitoring Service**

The monitoring service can monitor the health of our instances, by simply sending it periodically
**health check messages**

Alternatively, it can listen to periodic messages called **heardbeats**, which should come periodically from our healthy application instances

In either of those strategies, if the monitoring service does not hear from a particular server or a predefined duration of time, it can assume that that server is no longer available 

**False Positive**

- But the issue might be _the network_ or _long garbage collection_
- Then the monitoring service is going to have a **false positive**
- It assumed that a healthy host is faulty

**False Negative**

- The Monitoring Service shouldn't have false negative
- False negatives mean that
  - The servers may have crashed
  - The monitoring system did not detect that

**Monitoring System Functions**

- Exchange of messages in the form of pings and heartbeats
- Collect data about the number of errors each host gets per minute
  - If the error rate in one of the hosts is high, it can interpret that as _failure of the host_
- Collect information about time taken for each host to respond
  - If the time to respond to requests becomes long, it can decide that the _host is slow_

---

## 3. Recovery from Failure

> Availability = MTBF / ( MTBF + MTTR)

Regardless of our system failure rate, if we can detect and recover from each failure
faster than the user can notice, then our system will have high availability

- Actions after detecting faulty instance / server
  - Stop sending traffic / workload to that host
  - Restart the host to make the problem go away
  - _Rollback_ - going back to a version that was stable and correct

**Rollbacks**

- Common in databases
  - If we get to a state violating some condition / data, we can _roll back to the last correct state_ in the past
  - If we detect errors while rolling out new versions of software, we can _roll back to the previous version_

---

## Summary 

- _Fault Tolerance_ - to achieve High Availability in large-scale systems
- _Sources of Failures_
  - Human Error
  - Software Crashes
  - Hardware Error
- _Fault Tolerance_ is defined as "the ability for our system to remain operational and available to the user despite failures to one or some of its components"
- _Tacticts_ to achieve _fault tolerance_
  - Failure prevention
  - Failure detection and isolation
  - Recovery

---

## SLA / SLI / SLOs

### SLA - Service Level Aggrement

SLA is an agreement between the Service Provider and Clients / Users

- It is a legal contract that represents our quality service such as
  - Availability
  - Performance
  - Data durability
  - Time to respond to system failures
- It states the penalties and financial consequences, if we breach the contract
- The penalties include
  - Full / Partial refunds
  - Subscription / License extensions
  - Service credits

**SLA and Users**

- SLAs exist for
  - External paying users (always)
  - Free external users (sometimes)
  - Internal users within our company (occasionally)
- Internal SLAs don't include any penalties
- SLA for free external users makes sense if our system has major issues during a free trial of our service
- We compensate those users with a _trial extension_ or _credits for future_
- Companies providing entirely free services don't _publish SLA_
- Cooperating third parties should also know our SLA

---

### SLOs - Service Level Objectives

- Individual goals set for our system
- Each SLO represents a _target value/range_ that our service needs to meet
- For example, we can have
  - Availability Service Level Objective of _3 nines_
  - Response Time SLO of _less than 100 milliseconds at the 90th percentile_
  - Issue Resultion Time Objective of _between 24 and 48 hours_
- SLOs include
  - Quality attributes from the beginning of the design process
  - Other objectives of our system

**SLO and SLA**

- SLA
  - Service Level Objective 1
  - Service Level Objective 2
  - Service Level Objective 3
  - Service Level Objective 4
- Systems that don't have SLA still must have SLOs
- If we don't have set SLOs, our users won't know what to expect from our system

---

### SLIs - Service Level Indicators

- Quantitative measure of our compliance with a service-level objective
- It is the actual members
  - Measured using a monitoring service
  - Calculated from our logs
- It can be later compared to our SLOs

**Service Level Indicators - Examples**

- Percentage of user requests receiving a successful response can be used as SLI for availability
  - Later it can be compared to the _availability SLO of three nines_ that we set
- The average  / percentile distribution of the response time experienced by users can be calculated
  - Later it can be compared to the _end-to-end latency SLO of 100 milliseconds at the 90th percentile_ that we set

**SLI, SLO and SLA**

- SLO represent the _target values_ for the important quality attributes
- Quality attributes need to _testable_ and _measurable_
- If they weren't measurable, we wouldn't find any SLIs to validate that we meet our SLOs
- If we can't prove that we meet the SLOs, we can't say that we meet our SLA
- SLAs are crafted by _the business and the legal team_
- SLOs are SLIs are defined and set by _the software engineers and architects_

---

### Imporant Considarations

1. We shouldn't take every SLI that we can measure in our system and define an objective associated with it
- Think about the metrics that users care about the most
- Define the SLOs around those metrics
- From that, right SLIs to track those SLOs can be found
2. Promising fewer SLOs is better
- With many SLOs it's hard to prioritize all of them equally
- With few SLOs, it is easier to focus the entire software architecture around them
3. Set realistic goals with a budget for error
- We shouldn't commit to five-nines of availability even if we can provide that
- We should commit to a _much lower availability_ that we can provide
  - This _saves costs_ and _incorporates unexpected issues_
  - This is true when our SLOs are represented in an _external SLA_
- Companies define separate _external SLOs_ which are looser than internal SLOs
  - Externally we can commit to 99.9% of availability
  - But internally we commit to 99.99% availability
  - We can strive for better quality internally while committing less to clients
  - This would avoid financial penalties
4. Create a recovery plan for when the SLIs show that we are not meeting our SLOs
- We need to decide ahead of time what to do if
  - The system goes down for a long time
  - The performance degrades
  - Reports about issues/bugs in the system
- This plan should include
  - Automatic alerts to engineers / DevOps
  - Automatic failovers / restarts / rollbacks / auto-scaling policies
  - Predefined handbooks on what to do in certain situations

---

### Summary
- Three important terms in designing a real-life system are
  - The Service Level Agreement or SLA
  - The Service Level Objectives or SLOs
  - The Service Level Indicators or SLIs
- SLA is a legal contract between a service provider and its customers
- SLA aggregates the most important SLOs that we promise our users
- SLIs are used to measure our compliance with those SLOs
- Four important considerations when defining SLOs told us to
  - Define the most important SLOs that our users care about and then find the SLIs based on those objectives
  - Commit to the bare minimum in terms of
    - Number of objectives
    - Aggressiveness (Leaving a budget for error)
  - Have a recovery plan ahead of time to deal with situations of potential breach of SLOs
 
---
