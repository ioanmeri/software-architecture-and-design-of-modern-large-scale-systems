# Section 2: System Requirements & Architectural Drivers

# 01. Introduction to System Design & Architectural Drivers

- Requirements - Motivation
- Requirements - Classification

<b>Requirements</b>: Formal description of what we need to build

Large scale system requirements are different than the usual requirements 
we typically get for implementing
- A method
- An algorithm
- A class

---

## Big Scope and High Level of Abstraction


Method / function -> Class -> Module -> Library -> Application

The range of possible ways to solve the problem is getting bigger -> More degrees of freedom

<b>Examples of Scope and Abstraction</b>

- File Storage System
- Video Streaming Solution
- Ride Sharing Service

---

## High Level of Ambiguity

- System Design has high level of ambiguity
- Two reasons
  - The person providing the requirements is often not an engineer and may even be not very technical
  - Getting the requirements is part of the solution
    - The client doesn't always know what they need
    - The clinet generally knows only what problem they need solved
   
<b>Example - Hitchhiking Service</b>

High Level Requirement: "Allow people to join drivers on a route, who are willing to take passengers for a fee"

Clarifying Questions:

- Real Time vs Advance Reservation
- User Experience - Mobile? Desktop? Both?
- Payment through us or direct payment?

---

## Importance of Gathering Requirements

- What happens if we don't get the requirements right?
- We can simply build something and then fix it
- Seemingly there's no cost of materials in software so changes should be cheap??

<b>Large scale systems are big projects that cannot be easily changed</b>
- Many enginners are involved
- Many months of engineering work
- Hardware and Software costs
- Contracts include financial obligations
- Reputation and brand

---

## Types of Requirements

- <b>Features of the System</b>
  - Functional requirements
- <b>Quality Attributes</b>
  - Non-Functional requirements
- <b>System Constraints</b>
  - Limitations and boundaries

### Features / Functional Requirements

- Describe the system behavior - what "the system must do"
- Easily tied to the objective of our system

Describe the system as a "black box" function

User Actions, Events --> System --> Result / Outcome


- Functional Requirements do not determine its architecture
- Generally, any architecture can achieve any feature

<b>Examples</b>

"When a rider logs into the service mobile app, the system must display a map
with nearby drivers within 5 miles radius"

- Input: a rider logs into the service mobile app
- Output: display a map with nearby drivers within 5 miles radius

"When a rider is completed, the system will charge the rider's credit card and credit the driver,
minus service fees"

- Input event: completion of the ride
- Outcome of the operation: transfer of the money


### Quality Attributes / Non-Functional Requirements

- System properties that "the system must have"

<b>Examples</b>
- Scalability
- Availability
- Reliability
- Security
- Performance

- The quality attributes <b>dictate the software architecture of our system</b>

The software architecture defines the system quality attributes, and different architectures 
provide us with different quality attributes

### System Constraints

<b>Examples</b>
- Time Contraints - Strict deadlines
- Financial Constraints - Limited budget
- Staffing Constraints - Small number of available engineers

the 3 types of requirements are also referred to as <b>Architectural Drivers</b>

---

## Summary

- Importance of system requirements
- Challenges
  - Hish Scope and Abstraction
  - Ambiguity
- Risks of not getting the requirements correctly up front
- Requirements classification / Architectural drivers:
  - Features - Function requirements
  - Quality Attributes - Non-functional requirements
  - Constraints - Limitations and boundaries
 
---
