# Section 8: Strategies for Processing Infinite Streams of Events

## Introduction to Event-Stream Processing and Tumbling Window Strategy

In many cases, to get meaningful insights, we need to aggregate and analyze a series of events.

We use finite subsets of events by chopping those infite streams into manageable pieces of data.

By using **finite windows**, we can analyze streams of data almost in real time.

---

### Importance of Event-Stream Processing

- Anomaly Detection
- Fraud Detection
- Pattern Recognition
- Rate Limiting
- Recommendations Service

when each event own each own, does not carry sufficient information

---

### Usage of Event-Stream Processing Strategies

- Microservices and Event-Driven Architecture
- Big Data Pipelines
  - the volume of incoming events, is not only unbounted but also very high

---


### Tumbling Window

The consumer application or microservice, devides the time into fixed back-to-back, non-overlapping time buckets
where each event falls only to one window.

Once a window it's open, every event that the consumer consumes, will be added only to the currently active window.

When we reach the end of the window, the consumer aggregates those events and produces the result as a single event.

This result, can be 
- a new event to another channel
- a new data point
- an alert
- or a DB record

![Tumbling Window](assets/images/17.png)


Once the current window it's closed, a new window it's created for the same fixed duration. All the new events will be bucketed in this new window.
The old data from the previous window, it is discarted and doesn't consume any more resources.

Message broker is generally unaware of the concept of this window. The consumer is full responsible for buffering those events 
typically in memory till the window expires. 
- the longer the window, the larger the memory footprint

---

### Tumbling Window - Example 1

Capturing and displaying the average meaurument from sensor
- temperature of weather tower / energy usage of a vacuum cleaner in a 10 second window

We can apply a technique called: trimmed mean. Allows us to throught away e.g. 
- 10% of the highest values
- 10% of lowest values

and calculate a more accurate average measurement for each window

---

### Tumbling Window - Example 2

Log analysis coming from production machines.

We can use the tumbling window aligned to every round hour and:
- look at the logs of production machines
- calculate the error rate in the last hour

If we detect in a certain window that the error rate exceeds some threshold, we can send an alert to the notification service
- will send a text or email to an engineer on call

---

### Tumbling Window - Example 3

On commerce systems, where different sellers can sell their products to our user base:

We can use the tumbling window to analyze the revenue in our system every 24 hours.

At the end of each window, we group sales by seller and produce the total revenue for each seller in the last 24 hours
- data can be stored in a Database
- published as an event which can be consumed by email service and be send to each seller daily
- can also be sent to the Payout Service

---

### Tumbling Window Strategy - Props

- Simple
- Ideal for
  - Periodic / discrete aggregations
  - Minimal memory / computation overhead
  - Clear time boundaries: minures / hours / days
- Ideal when each event logically belongs to only one window

---

### Tumbling Window Strategy - Cons

- Low granularity of results
  - The frequency of insights is limited to the window size
    - e.g. in log analysis if we have a spike in errors, we will get that insight
      - only at the end of the hour long window
      - can already have serious financial impact
    - e.g. in the ecommerce system, the seller will only get fresh data about revenue
      - when the 24h window closes and results are published
- **Not** a good strategy for pattern / trend recognition
  - e.g. trend will not be discovered if there are spikes around the window boundary

---




