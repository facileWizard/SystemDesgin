# 🚀 Interview Case Studies – FAANGM (System Design + Integration)

## 📌 Purpose

This repository documents real interview-style system design and integration problems with clear explanations, trade-offs, and production-ready thinking.

It is intended to:

* Help candidates prepare for FAANGM-level interviews
* Provide structured approaches to complex problems
* Demonstrate real-world distributed system thinking

---

# 🧠 Case Study 1: ERP → Application Sync + Search System

## 🏗️ Problem Statement

Design a system where:

* ERP is the source of truth (products, pricing, inventory)
* Application has its own database for serving users
* Updates must reflect in search within **2 seconds**
* No duplicate processing
* Support analytics + notifications

---

## 📐 High-Level Architecture

### Flow:

1. ERP emits events (create/update)
2. API Gateway / Ingress validates request
3. Kafka buffers and streams events
4. Consumers:

   * Application DB updater
   * Search index updater
   * Analytics pipeline
   * Notification service

---

## ⚙️ Components

### 1. Event Producer (ERP)

* Emits events with:

  * event_id
  * timestamp/version
  * payload

### 2. API Gateway / Ingress

* Authentication
* Rate limiting
* Correlation ID

### 3. Kafka (Core Backbone)

* Partitioning by product_id
* Guarantees ordering per key
* DLQ for failures

### 4. Application Service

* Consumes events
* Validates
* Performs idempotency check

### 5. Idempotency Table

| event_id | status | timestamp |
| -------- | ------ | --------- |

Prevents duplicate updates

---

## 🔄 Data Flow (Detailed)

ERP → Ingress → Kafka → Consumer →

1. Check idempotency
2. Validate timestamp/version
3. Update DB
4. Emit success/failure

---

## 🔍 Search Optimization

### ❌ Wrong Approach

Search directly from ERP

* High latency
* Tight coupling

### ✅ Correct Approach

* Maintain **Search Index (ElasticSearch)**
* Consume directly from Kafka

Benefits:

* Near real-time (<2s)
* No DB dependency

---

## ⚠️ Key Problems + Solutions

### 1. Duplicate Events

Solution:

* Idempotency table
* Event ID tracking

---

### 2. Out-of-Order Events

Solution:

* Version/timestamp check

```
if incoming_version < stored_version:
    ignore
```

---

### 3. Failure Handling

* Kafka retries
* DLQ
* Replay capability

---

### 4. Scalability

* Kafka partitions
* Horizontal consumers

---

## 📊 Trade-offs

| Decision     | Pros        | Cons                 |
| ------------ | ----------- | -------------------- |
| Kafka        | scalability | complexity           |
| App DB copy  | fast reads  | data duplication     |
| Search index | fast search | eventual consistency |

---

# 🧠 Case Study 2: Order Processing (Saga Pattern)

## 🏗️ Problem

When user places order:

* Payment
* Fraud
* Inventory
* Order confirmation

Constraints:

* <3 sec latency
* No double inventory
* Exactly-once order

---

## 🧩 Architecture

### Pattern: Orchestration-based Saga

Order Service = Orchestrator

---

## 🔄 Flow

1. Create Order (INIT)
2. Call Payment Service
3. Call Fraud Service
4. Reserve Inventory
5. Confirm Order

---

## ⚠️ Failure Handling

| Step           | Failure Action  |
| -------------- | --------------- |
| Payment fail   | cancel order    |
| Fraud fail     | cancel + refund |
| Inventory fail | release payment |

---

## 🔐 Idempotency

Each service:

* Uses request_id
* Prevents duplicate execution

---

## ⏱️ Timeout Handling

Fraud service slow (2s):

* async processing
* fallback decision

---

## 📊 Guarantees

* At least once delivery
* Exactly-once via idempotency

---

# 🧠 Interview Learnings

## ✅ What Worked

* Structured thinking
* Tradeoff discussion
* Event-driven design

## ❌ What to Improve

* Faster pattern recognition
* Cleaner communication under pressure

---

# 🧠 Template for Any System Design

1. Clarify requirements
2. Define entities
3. High-level architecture
4. Data flow
5. Scale
6. Failure handling
7. Trade-offs

---

# 🎯 Final Thoughts

System design interviews are not about perfection.
They test:

* clarity
* reasoning
* trade-offs

Focus on thinking, not memorization.

---

**Author:** Aditya

