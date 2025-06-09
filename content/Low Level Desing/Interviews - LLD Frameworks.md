---
title: '"Interviews - LLD Frameworks"'
draft: 
tags:
  - interviews
  - LLD
  - frameworks
---

## **LLD Problem Solving Framework (Phase-wise with Examples)**

---
### **Phase 1: Understand the Problem (Time: 5 minutes)**

**Goal:** Get absolute clarity on the problem statement.
#### **Questions to ask:**

- What are the inputs/outputs?
    
- What real-world entities exist?
    
- What is out of scope?
    
#### **Examples:**

|**Problem**|**Clarifying Questions**|
|---|---|
|Parking Lot|Multi-level? Handicapped spots? Reservations?|
|Elevator|How many elevators? Multiple floors? Prioritization logic?|
|Expression Evaluator|Supported operators? Data types? What happens if field missing in input?|

---


### **Phase 2: Identify Core Responsibilities (Time: 5–10 minutes)**

  

**Goal:** Break the problem into **distinct functional components**.

  

#### **Examples:**

|**Problem**|**Core Responsibilities**|
|---|---|
|Parking Lot|Spot allocation, availability tracking, entry/exit flow|
|Elevator|Request handling, scheduling, movement control|
|Expression Evaluator|Parsing condition rules, comparing fields, applying operators|

---


### **Phase 3: Define Key Entities & Interfaces (Time: 10 minutes)**

  

**Goal:** Design key classes, enums, and interfaces that model the system.

  

#### **Examples:**

|**Problem**|**Key Classes & Interfaces**|
|---|---|
|Parking Lot|ParkingLot, Spot, Vehicle, Ticket, SpotAllocator|
|Elevator|Elevator, Request, Scheduler, Direction, ElevatorState|
|Expression Evaluator|Condition, Operator, ExpressionEvaluator, OperatorRegistry|

> Design Tip: Follow **OOP principles** — Identify areas for abstraction (via interfaces), encapsulation, and extensibility.

--- 

### **Phase 4: Strategy Pattern or Plug-in Logic (Time: 10–15 minutes)**

  

**Goal:** Make the system flexible and extensible using patterns like **Strategy**, **Factory**, or **Registry**.

  

#### **Examples:**

|**Problem**|**Plug-in Logic**|
|---|---|
|Parking Lot|Use SpotAllocator interface with implementations for Nearest, Reserved, etc.|
|Elevator|Use Scheduler interface with FCFS, Optimized, or PriorityBased strategies|
|Expression Evaluator|Use Operator interface and plug different comparisons via strategy pattern|

---

### **Phase 5: Build Orchestrator / Engine (Time: 10–15 minutes)**

  

**Goal:** Assemble all components into a working system that can be tested and extended.

  

#### **Examples:**

|**Problem**|**Core Engine Logic**|
|---|---|
|Parking Lot|Entry/exit controller calling allocator and updating map of spots|
|Elevator|Scheduler assigns elevators, moves elevator state machine|
|Expression Evaluator|Iterate conditions → get operator → compare values → return result|

---

### **Phase 6: Validate & Extend (Time: 5 minutes)**

  

**Goal:** Handle edge cases, support future extensions, and demonstrate understanding.

  

#### **Examples:**

|**Problem**|**Additions & Validations**|
|---|---|
|Parking Lot|Support for electric vehicles, pricing strategies|
|Elevator|Fire alarm interrupt, maintenance mode, weight handling|
|Expression Evaluator|Support nested expressions (AND/OR), add new data types|

---

## **Total Time: 45–50 minutes**

|**Phase**|**Time**|**Deliverable**|
|---|---|---|
|Understand|5 mins|Clarified scope and constraints|
|Define Roles|5–10|Core responsibilities|
|Model Entities|10|Classes, enums, interfaces|
|Plug-in Logic|10–15|Strategy/Factory/Registry|
|Engine Design|10–15|Integrate the system|
|Extend/Test|5|Edge cases + extensibility plan|

---

## **Summary Table Across All 3 Examples:**

|**Phase**|**Parking Lot**|**Elevator**|**Expression Evaluator**|
|---|---|---|---|
|Entities|Spot, Vehicle, Ticket|Elevator, Request, Scheduler|Condition, Operator, Evaluator|
|Core Functions|Assign/release spot, track capacity|Schedule requests, move elevators|Compare fields with rules|
|Patterns Used|Strategy (Allocator)|Strategy (Scheduler), FSM for movement|Strategy (Operator), Registry|
|Extensibility Points|New spot types, pricing|Scheduler strategies, smart movement|New operators, nested expressions|
|Common Mistakes|No spot tracking per level/floor|No prioritization logic|Hardcoded logic instead of extensible|
