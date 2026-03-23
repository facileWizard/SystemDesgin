# 🔁 Idempotency & Reliability in Distributed Systems

## 🧠 Problem

In distributed systems, the same request or event can be processed multiple times due to:
- Network retries
- Client timeouts
- Message re-delivery (Kafka, queues)
- System failures

This can lead to:
- Duplicate orders
- Multiple payments
- Data inconsistency

---

## 🎯 Goal

Ensure that:

> **Processing the same request multiple times produces the same result as processing it once**

---

## 🧩 Where Idempotency is Required

| Scenario | Required |
|--------|--------|
| Payment processing | ✅ Mandatory |
| Order creation | ✅ Mandatory |
| Event consumption | ✅ Mandatory |
| Read operations | ❌ Not required |

---

## 🧱 High-Level Flow


Client → API → Idempotency Check → Process → Store Result → Return Response


---

## 🧩 Architecture Diagram


Client
│
▼
API Gateway
│
▼
Backend Service
│
├── Check Idempotency Table
│ │
│ ├── Exists → Return Stored Response
│ └── Not Exists → Process Request
│
▼
ERP / SAP System
│
▼
Store Result in Idempotency Table
│
▼
Return Response


---

## 🧠 Idempotency Key

### 🔑 What is it?

A unique identifier for a request.

### 🧾 Examples:
- `order_123_user_456`
- `payment_<transaction_id>`
- `event_id` (for Kafka)

---

## 🧱 Idempotency Table Design

| Column | Description |
|-------|------------|
| idempotency_key | Unique request identifier |
| status | SUCCESS / FAILED |
| response | Stored API response |
| created_at | Timestamp |
| expiry | Optional TTL |

---

## 🔄 Execution Flow (Step-by-Step)

### 1. Request Comes In
- Client sends request with `Idempotency-Key`

---

### 2. Check Table
- If key exists:
  - Return stored response
- If not:
  - Process request

---

### 3. Process Business Logic
- Call SAP / BAPI / DB

---

### 4. Store Result
- Save:
  - Key
  - Response
  - Status

---

### 5. Return Response

---

## 🔁 Idempotency in Event-Driven Systems

### 🧠 Problem

Kafka guarantees **at-least-once delivery**  
→ Same event can be processed multiple times

---

### ✅ Solution

Use:
- `event_id`
- `version`

---

### 🧩 Event Schema Example

```json
{
  "event_id": "12345",
  "version": 1,
  "type": "ORDER_CREATED",
  "timestamp": "2026-01-01T10:00:00Z"
}
🧠 Deduplication Strategy
Store event_id in DB
Ignore if already processed
⚖️ Design Decisions
1. Where to Store Idempotency?
Option	Use Case
Database	Strong consistency
Cache (Redis)	High performance
Hybrid	Best of both
2. TTL (Time to Live)
Prevent table from growing infinitely
Example:
Payments → long TTL
Requests → short TTL
🔥 Common Pitfalls
❌ No Idempotency Key
Leads to duplicate data
❌ Using Random Keys
Must be deterministic
❌ Not Storing Response
Cannot return same result
❌ Ignoring Failed State
Retries may behave incorrectly
⚖️ Trade-offs
Approach	Pros	Cons
DB-based	Reliable	Slower
Cache-based	Fast	Risk of data loss
Hybrid	Balanced	Complex
🧠 Real-World Example
🛒 Order Creation
User clicks "Place Order"
API receives request with key: order_user123_cart456
System processes order
Stores response
🔁 Retry Scenario
User retries request
System detects key exists
Returns same order ID
No duplicate order created
🧠 SAP-Specific Considerations
🔹 BAPI Calls
Wrap BAPI with idempotency layer
Prevent duplicate transactions
🔹 bgRFC / Async Processing
Use idempotency for:
Background jobs
Retry queues
🌍 Multi-Region Considerations
Ensure global uniqueness of keys
Use:
UUID
Region prefix (optional)
🔥 Golden Rules
Idempotency is mandatory for all write operations
Always store:
Key
Response
Design for retries from day one
🚫 When NOT Needed
Read-only APIs
Internal computations
📌 Summary
Distributed systems are unreliable by nature
Retries are inevitable
Idempotency ensures correctness

“Without idempotency, retries become data corruption.”
