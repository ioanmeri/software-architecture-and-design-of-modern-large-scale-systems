# Section 6: Data Storage at Global Scale

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


 

