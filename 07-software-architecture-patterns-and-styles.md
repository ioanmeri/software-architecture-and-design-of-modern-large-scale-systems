# Section 7: Software Architecture Patterns and Styles

- [Introduction to Software Architecture Patterns & Styles](#introduction-to-software-architecture-patterns--styles)

---

## Introduction to Software Architecture Patterns & Styles

### Software Architectural Patterns

- General repeatable solutions to commonly occuring system design problems
- Common solutions to software architectural problems that involve **multiple components** that run as **separate runtine units**

---

### Software Architectural Patterns - History

- Software architects have been observing how other companies in similar industries went about solving similar design problems
- They tried to learn
  - What worked for them
  - What mistakes were made
- Those successful software architecture practices that became the Software Architectural Patterns

---

### Software Architectural Patterns - Incentives

**1. Save valuable time and resources**

- If other companies
  - Had very similar use cases to your problem
  - Operate on a similar scale
  - Already found an architecture and development practice that works for them
- Then it's better to take that knowledge and use it

---

**2. Avoid making our architecture resemble a *Big Ball of Mud***

- Big Ball of Mud: Anti Pattern of system that lucks structure where every service talks to every other service
  - tightly coupled
  - code duplication
  - no clear scope of responsibility for any of the components
- Many companies got into such situations due to
  - Rapid growth
  - Lack of overarching, well-defined software architecture
- A system that gets into this situation is very hard to
  - Develop
  - Maintain
  - Scale
- The way to avoid it is to stick to a well-defined software architectural pattern

---

**3. Other engineers / software architects can follow it**

- Everyone can read about the pattern we're following
- Understand exactly what to do and what not to do
---

### Final Notes

- All the software architectural patterns are just guidelines
- As systems evolve, certain architectural patterns may not fit us anymore
- We would need to migrate to a different architectural pattern that now fits us better
- Many companies already went through such migrations in the past so we can follow their best practices
  
---





