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

| user_id | password | details |

#### Orders Table

| order_id | user_id (FK) | product_id |

#### Subscription Table

| subscription_id | user_id (FK) | product_id | quantity |

---

### 3. ERP Layer

* SAP / Oracle systems
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

  * SAP → API-based
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

**Author:** Aditya
