# 🚀 Interview Case Studies – FAANGM (Real Onsite Experiences)

## 📌 Purpose

This repository documents **real interview experiences** (system design + integration) with practical decisions, trade-offs, and reasoning.

Focus:

* What I actually designed in interview
* Why I made those choices
* What I would improve later

---

# 🧠 Case Study 1: ERP → Application System with Search

## 🏗️ Problem Statement

Design a system where:

* ERP is the **source of truth** (products, pricing, inventory)
* Application layer serves users
* Fast search required
* Handle high request load

---

## 🎯 Key Design Decision (Important)

👉 Instead of creating a separate search system:

**I chose to search directly from ERP database**

### Why?

* Avoid data duplication
* Keep application layer lightweight
* ERP already contains required data

---

## 🗄️ ERP Data Model (Search Table)

Custom search table created in ERP DB:

| Field         | Description     |
| ------------- | --------------- |
| material_id   | Primary Key     |
| material_name | Used for search |

Search logic:

* Pattern-based search (`LIKE '%term%'`)
* Optimized indexing

---

## 📐 High-Level Architecture

### Flow:

Application → API Layer → Load Balancer → ERP → Response

---

## ⚙️ Components

### 1. Application Layer

Handles:

* User authentication
* Order history
* Subscription system

---

### 2. Application DB Schema

#### Users Table

| user_id | password | details | order_id(FK) | subscription_id(FK) | payment_methods | saved_address | 

#### Orders Table

| order_id | user_id (FK) | product_id | qty | unit_price | total_price | order_total | discount% |

#### Subscription Table

| subscription_id | user_id (FK) | product_id | quantity | trigger_date | order_date | payment_method | order_total |

---

### 3. ERP Layer

* SAP / Oracle/ Workday/ MSFT erp systems
* REST APIs exposed
* Handles:

  * product data
  * inventory
  * pricing

---

### 4. API Layer + Load Balancer

* Authentication
* Rate limiting
* Load distribution

---

## 🔄 Handling Scale

For high traffic:

* Introduced **Load Balancer**
* Introduced **Kafka (for async flows)**

### Use cases for Kafka:

* Bulk updates
* Retry mechanisms
* Decoupling application and ERP

---

## ⚠️ Failure Handling

* DLQ (Dead Letter Queue)
* Retry mechanism
* Idempotency checks

---

## 🔁 Multi-ERP Scenario

* SAP + Oracle
* Same logical architecture
* Different implementation layers:

  * SAP → RAP-based
  * Oracle → REST-based

---

## 📊 Trade-offs

| Decision              | Pros           | Cons                |
| --------------------- | -------------- | ------------------- |
| Search from ERP       | No duplication | Higher latency risk |
| No separate search DB | Simpler        | Scaling challenges  |
| Kafka usage           | Reliable       | Added complexity    |

---

# 🧠 Case Study 2: Integration – In-house Systems ↔ Third-party ERP

## 🏗️ Problem Statement

* Multiple internal systems (Google in-house)
* One third-party ERP system
* Need:

  * Data sync
  * Analytics
  * Reliability
  * No duplicate processing

---

## 📐 Architecture Overview

### Two flows:

### 1️⃣ Inbound Flow

In-house systems → Integration Layer → Third-party ERP

### 2️⃣ Outbound Flow

Third-party ERP → Integration Layer → In-house systems

---

## ⚙️ Components

### 1. API / Ingress Layer

* Authentication
* Load handling
* Correlation ID

---

### 2. Kafka

Used for:

* Buffering
* Retry handling
* Decoupling
* Spike absorption

---

### 3. Processing Layer

* Validation
* Transformation
* Business logic

---

## 🔄 Async Processing Strategy

### Problem:

Some systems take longer to process

### Solution:

* Immediate ACK to caller
* Process asynchronously
* Emit response event later

---

## 🔐 Idempotency Design

To prevent duplicates:

* event_id
* version

Check:

```
if event already processed:
    ignore
```

---

## 📊 Analytics Flow

* Events also sent to analytics system
* Ensures tracking and auditing

---

## ⚠️ Failure Handling

* Retry via Kafka
* DLQ for failed events
* Replay mechanism

---

## 📊 Trade-offs

| Decision         | Pros        | Cons                 |
| ---------------- | ----------- | -------------------- |
| Async processing | scalable    | complexity           |
| Kafka            | reliability | operational overhead |
| Idempotency      | correctness | extra storage        |

---

# 🧠 Key Interview Learnings

## ✅ What Went Well

* Structured explanation
* Trade-offs discussion
* Real-world thinking

## ❌ Improvements

* Faster pattern recognition (DSA)
* Cleaner communication under pressure

---

🧠 Case 1: ERP Search Architecture 


<img width="2069" height="1318" alt="erp" src="https://github.com/user-attachments/assets/c724c955-0565-4c9a-a6dc-b0e7274109a2" />







🧠 Case 2: Integration Architecture (Inbound + Outbound)


<img width="3817" height="489" alt="2erp" src="https://github.com/user-attachments/assets/1757aef6-f7c7-4f19-a109-1038a4b43a6f" />








⚡ Async Processing Pattern 


<img width="2100" height="870" alt="3erp" src="https://github.com/user-attachments/assets/469615af-6e74-46d6-a9cb-eb23e8b4222b" />







🧠 Idempotency Logic 


<img width="826" height="1043" alt="4erp" src="https://github.com/user-attachments/assets/0158f7e0-4087-4d45-8f6e-91c7eabde1f4" />
































## 🧠 What Interviewer Expected

- Why not separate search system?
- How do you handle scaling?
- What about latency?
- Coupling between the systems.
- Schema for the Application layer tables, relationships between the table how they are interconnected.
- The immediate ack was a follow up question -> What if one of the in-house solutions take hours to process the incoming data and respond back?
- Questions are very vague, you are expected to ask everything from top to bottom, they will dig deep if it is interesting or check flaws in the design and ask to answer that or improve that.
  
## 🔁 Improvements (Post Interview)

- Could use search index instead of ERP direct search
- Better handling of pattern search scalability
- Introduce caching layer



















**Author:** Aditya
