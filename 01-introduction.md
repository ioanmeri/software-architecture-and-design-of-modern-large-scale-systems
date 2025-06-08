# Section 1: Introduction

## Software Architecture - Motivation

<b>Analogies from Outside of the Software World</b>

- Everything we build has a structure
- The more we invest in building a product, the harder it is to change its structure
- The structure of our system describes
  - The intent of our product
  - The qualities of the product

<b>Applications to Software</b>

- Infinite ways to organize our code
- Different organizations will give us different properties
- The software architecture impacts
  - Performance and scale of the product
  - Ease of adding new features
  - Response to failure or security attack
- Cost of redesign will be significant in terms of time and money

---

## Software Architecture - Definition

<b>Definition of Software Architecture</b>

- Many ways to define software architecture
- The definition we're going to use is as follows

> The software architecture of a system is a high-level description of the system's structure, its different components,
> and how those components communicate with each other to fulfill the systems' requirements and constrains

First part:

- An abstraction that shows us the important components while hiding the implementation details
- Technologies or programming languages are not a part of the software architecture but a part of the implementation
- Decisions about implementation should be delayed to the very end of the design

Second part:

- The components here are "black box" elements defined by their behavior and APIs
- The components may themselves be complex systems with their own software architecture diagrams

Thirt part:

- Components come together to do what the <i>system must do</i>, which are our requirements
- The system <i>does not do what is shouldn't do</i>, which are the constrains

---

