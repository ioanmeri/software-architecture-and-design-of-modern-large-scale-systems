# Section 3: Most Important Quality Attributes in Large Scale Systems

[Performance](#performance)

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

**Response Time = Processing Time + Waiting Time**

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

#### 1. Measuring Response Time Correctly
- Processing Time + **Waiting Time**
- take into account waiting time in network or software queues


#### 2. Response Time Distribution
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


#### 3. Performance Degradation

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

