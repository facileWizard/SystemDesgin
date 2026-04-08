# 🧠 System Design Fundamentals (Enterprise + ERP Context)

## 🎯 Goal

Understand how to design scalable, reliable systems when working with:
- ERP systems (SAP, Oracle)
- External services
- Distributed architectures

---

## 🧩 Core Concepts

### 1. Scalability
- Handle increasing load
- Horizontal > Vertical scaling

---

### 2. Reliability
- System should recover from failures
- Use:
  - Retries
  - Queues
  - Idempotency

---

### 3. Availability
- System should remain operational
- Use:
  - Load balancers
  - Multi-region deployments

---

### 4. Consistency vs Availability

| Type | Description |
|------|------------|
| Strong Consistency | Immediate correctness |
| Eventual Consistency | Delayed correctness |

---

## 🧠 ERP Reality

- ERP systems are:
  - Slow
  - Transaction-heavy
  - Not designed for high concurrency

---

## 🔥 Golden Rules

- Never expose ERP directly
- Always introduce abstraction layers
- Prefer async for scale 
