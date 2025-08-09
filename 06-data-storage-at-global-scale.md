# Section 6: Data Storage at Global Scale

- [Relational Databases & ACID Transactions](#relational-databases--acid-transactions)
- [Non-Relational Databases](#non-relational-databases)

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
