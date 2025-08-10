# Section 6: Data Storage at Global Scale

- [Relational Databases & ACID Transactions](#relational-databases--acid-transactions)
- [Non-Relational Databases](#non-relational-databases)
- [Techniques to Improve Performance, Availability & Scalability of Databases](#techniques-to-improve-performance-availability--scalability-of-databases)

---

## Relational Databases & ACID Transactions

### Relational Databases - Table Structure

In a relational database, data is stored in tables. Each row in the record represents a single record, 
and all the records are related to each other through a predefined set of columns.

Each column in the table has a name, a type and optionally and set of constraints

Each record in the table is uniquely identified by a primary key, which can be represented by a column or a set of columns

---

### Relational Database - Schema

- The structure (schema) of each table is defined ahead of time
- This gives us the knowledge of what each record must have
- Because of this, we can use a robust query language to _analyze_ and _update_ the table data

---

### SQL - Structured Query Language

- The industry-standard scripting language to perform such queries is called SQL - _Structured Query Langage_
- Different relational database implementations have their own _additional features_ to their version of that language

---

### Relational Databases - History

- They're a very _well-known_ and _proven_ way of structing data since the 1970s
- Storing data in separate tables allows us to eliminate the need for _data duplication_
- Storage has become cheaper
- The amount of data companies collect is larger than before
- For large-scale systems, storage cost are still a major factor

**Online Store Example**

- As customers place orders for products, we need to store orders in a separate table
- We want the ability to easilly analyze and report on things like
  - Which products sell the most/least at a given time frame
  - Rank companies based on performance of their products
  - Get break down of which category of products does better during certain sales / seasons

--- 

### Relational Databases - Advantages

1. Ability to form complex and flexible queries
2. Efficient storage
3. Natural structure of data for humans
4. ACID transaction guarantees

---

### ACID Transactions

- **Atomicity**
- **Consistency**
- **Isolation**
- **Durability**

---

### Transaction - Definition

> A transaction is a sequence of operations that for an external observer should appear as a single operation

**Money Transfer Example**

- User A Balance = User A Balance - $100
- User B Balance = User B Balance + $100

Single Transaction

---

### ACID

**Atomicity**

- Each set of operations that are part of one transaction either
  - Appear _all at once_
  - Don't _appear at all_

**It's even all or nothing, and never anything in between**


**Consistency**

- A transaction that was _alread commited_ is seen by all **future** queries/ transactions
- A transaction doesn't violate any constraints that we set for our data

e.g. Impossible for a future query to see transfered funds either remaining / transfered in one of the accounts


**Isolation**

Related to _Atomicity_ in the context of **concurrent** operations performed on our database

Isolation guarantees that if there is another transaction happening simultaneously, that second concurrent transaction will not see
an intermediate state of the money being either present in both accounts or missing in both accounts

Those two transactions are isolated from each other, in such a way that they do not see each other's intermediate state


**Durability**

Once a transaction is complete, its _final state_ will **persist** and remain permanently inside the database



---


### Relational Databases - Disadvantages

**1. Rigid structure enforced by table's schema**

- The schema has to be _defined ahead of time_ before we can use the table
- If we want to change the schema of a table by adding/removing a column, we would have some _maintenance time_
- We need to plan ahead in designing the scheme of our tables, so as to _not change the schema_ very often or at all

**2. Hard to maintain/scale**


**3. Slower read operations**

---

### When To Choose a Relational Database

- Perform complex and flexible queries to analyze our data
- Guarantee ACID transactions between different entities in our database

---

### When Not To Choose a Relational Database

- There isn't any inherent relationship between different records that justifies storing our data in tables
- Read performance is the most important quality that we need for providing good user experience

---

### Summary

- We learned about the first type of database called _relational databases_ which are also referred to as SQL databases
- We talked about advantages of relational databases such as
  - Ability to perform powerful and flexible analysis of data
  - Efficient storage which allows to eliminate data duplication
  - The natural structure of the data in human-readable tables
  - ACID transaction guarantees
- We talked about the drawbacks of typical relational databases
  - Rigid schema that we have to define for each table
  - Increased complexity
  - Challenges for scalability and performance

---

## Non-Relational Databases

### Non-Relational Database - History

- A relatively new concept
- Became popular in the mid-2000s
- Solved the drawbacks of relational databases
  - records in a table have the same schema
  - e.g. if we want to add a middle name in a User Table only for certain users

---

### Non-Relational Database - Logical Grouping

- They allow to logically group a set of records **without** forcing all of them to have the same structure
- We can easily add additional attributes to one / multiple records **without** affecting the already existing records

---

### Relational Database - Support Tables Only

- Tables are natural for humans to analyze records **but** are less intuitive for programmers
- Most programming languages
  - Don't have table as a data structure
  - Support computer-science oriented data structures like
    - Lists
    - Arrays
    - Maps

---

### Non-Relational Database - Support Native DS

- Don't store data in tables
- Support more native data structures to programming languages
- This eliminates the need for an ORM (Object Relational Mapping)

---

### Efficient Storage vs Fast Queries

- Relational Databases: designed for **efficient storage**
- Non-Relatinal Databases: designed for **faster queries**

---

### Non-Relational Database - Trade-offs

- When we allow **flexible schemas** we lose the ability to easily analyze those records
- Analyzing multiple groups of records (join operations) also becomes hard
- **ACID** transactions are rarely supported by non-relational databases

---

### Non-Relational Database - Categories

**1. Key / Value Store**

We have a key that uniquely identifies the record and a value that represents the data associated with the record. This value is completely opaque to the database and can as simple as an integer or a string or complex as an array, a set or a binary blob

- Key / Value store can be seen as a large-scale **hashtable** or **dictionary**
- It has very few constraints on the type of values we have for each key

Perfect candidate for counter that multiple instances read or increment, or caching pages or pieces of data that can be easily queried and fetched without needing to do slow / complex queries

---

**2. Document Store**

- We can store collections of documents, with more structure inside each document
- Each document is an object with different attributes
- Those attributes can be of different types
- Documents inside a document store are easily mapped to objects inside a programming language

Examples
- JSON object
- YAML
- XML


---

**3. Graph Database**

- Extension of a document store with additional capabilities to
  - Link
  - Traverse
  - Analyze
  - multiple records more efficiently
- Optimized for navigating and analyzing relationships between different records

Use Cases
- Fraud detection
  - Multiple logical users identified as same person trying to initiate multiple transactions using same email / computer
- Recommendation engines
  - Recommend new products to users based on past purchase history or friends of the user

---

### How to Choose a Non-Relational Database

- Analyze our use case
- Figure out which properties of a database are
  - Most important
  - Can be compromised

---

### When To Choose a Non-Relational Database

- Non-relational databases
  - Superior when it comes to query speed
  - Perfect choice for caching
- Handling real-time big data
- Data is not structured
- Different records can contain different attributes

Examples:

- We can store very common query results that correspond to user views in a Non-Relational Database to improve the user experience
- User Profiles
- Content Management

---

### Summary

- We learned about the second type of database - the Non-Relational Databases or NoSQL Databases
- We learned some of the main advantages of Non-Relational Databases such as
  - Flexible schema
  - Fast query
  - More natural data structures for programming languages
- We talked about the 3 main categories of Non-Relational Databases
  - Key / Value stores
  - Document stores
  - Graph databases
- We talked about few considerations for choosing a database
- We mentioned classic use cases that are suitable for non-relational databases

---

## Techniques to Improve Performance, Availability & Scalability of Databases

### Database Indexing

- Speeds up retrieval operations
- Locate the desired records in a sublinear time
- Without indexing, those operations may
  - Require a "full table scan"
  - Take a long time for large tables

**Example**

```
SELECT * FROM USERS WHERE City = "Los Angeles"
```

Without index, our database would have to scan linearly all the rows, the same for LastName, Age or income

---

### Full Table Scan - Performance

- Those operations if performed very **frequently** or on **large tables** can
  - Become a performance bottleneck
  - Impact our users' experience

---

### Database Index - Definition

> A database index is a helper table, created from a particular column / group of columns

![Index Table](assets/images/06.png)

---

### Index Table - Data Structures

- Once the index table is created we can put it inside a data structure like
  - Hashmap
  - Self-balanced tree (B-Tree)

**Example 1**

If we place the index in a hash table, then the query can return the list immediately, without the need to scan the entire table

```
SELECT * FROM USERS WHERE City = "Los Angeles"
```

![Index Table](assets/images/07.png)

**Example 2**

Age Index Table - Binary Tree, in logarithmic time complexity, avoid scanning and sorting it

```
SELECT * FROM USERS WHERE Age < 85 AND Age > 18 ORDER BY Age
```

![Index Table](assets/images/08.png)


---

### Composite Index

- Indexes can be formed not only from **one column** but from a **set of columns**

**Example**

```
SELECT * FROM USERS WHERE City = "Los Angeles" AND LastName = "Smith"
```

If we create a composite index of both columns, we can have a direct mapping from a pair of values to the rows containing them

![Index Table](assets/images/09.png)

---

### Indexing Tradeoffs

- **Read queries** are faster in the expense of
  - Additional space for storing the index tables
  - Speed of write operations

**Note on Other Types of Databases**

Indexing is also used extensively in Non-Relational Databases such as document stores

---

### Database Replication

When we store a mission critical data about a business in a database, our database instance become a potential Single Point of Failure.

If we replicate our data, and run multiple instances of our database on different computers we can increase the fault tolerance of our system, which in return provides us with higher availability.

Queries can continue going to the available replicas, while we work to either restore or replace the faulty instance

In additional to **higher availability**, we can get **better performance** in the form of higher throughput. We can handle a much larger amount of queries if we distribute them among a larger number of computers

---

### Database Replication Tradeoffs

- Higher complexity when it comes to operations like
  - Write
  - Update
  - Delete
- It is not a trivial task to make sure that concurrent modifications to the same records
  - Don't conflict with each other
  - Provide guarantees in terms of consistency and correctness

---

### Database Replication - Distributed Database

- Distributed databases
  - Difficult to designing, configure and manage on a high scale
  - Require competency in the field of distributed systems

---

### Database Replication - Support

- Database replication is supported by all modern databases
  - Non-Relational Databases: Incorporate replication ouf-of-the-box
  - Relational Databases: Support varies among different implementations

---

### Database Partitioning vs Database Replication

Unlike replication where we run multiple instances of our database with the **same copy of the data** in each one of them.

When we do database partitioning, **we split the data** among different database instances

For increased performance, we typically run each instance on separate computer


---

### Database Partitioning - Advantages

- We can scale our database to store more data
- Different queries can be performed completely in **parallel**
- We get both
  - Better Performance
  - Higher Scalability

---

### Database Partitioning - Drawback

Database sharding turns our database into a distributed database

---

### Database Partitioning - Query Routing

This increases the complexity of the database and also adds some overhead, as now we also need to be able to route queries
to the right shards and make sure that neither of the shards becomes to large in comparison to the others.

---

### Database Partitioning - Non-Relational Databases

- First-class feature in all Non-Relational Databases because
  - Records are decoupled from each other
  - Storing the records on different computers is more natural and easier to implement

---

### Database Partitioning - Relational Databases

- In Relational Databases, the support for partitioning depends on the implementation
- Queries involving multiple records are common - Spreading them across multiple machines is challenging to implement
- When choosing a Relational Database for a use case involving high volume of data, make sure that partitioning is well supported

---

### Infrastructure Partitioning

Partitioning is not only used for databases but can also be used to **logically split our infrastructure**

We can partition our compute instances using configuration files so that requests from paid customers to some machines
and traffic from free users, go to other less powerful machines

Alternatively, we can send traffic from mobile devices to one group of computers and send desktop traffic 
to another group of computers running the same application

This way if we have an outage, we can easily know what type of users are affected and decide how to act upon it.

---

### Final Notes

- Indexing, Replication, and Partitioning are completely orthogonal to each other
- We don't need to choose one over the other
- All three of them are commonly used together in most real-life large-scale systems

---

### Summary

- We learned about 3 techniques that we can apply to our database to make it much more robust in a large-scale system
  - Indexing
  - Replication
  - Partitioning

---




