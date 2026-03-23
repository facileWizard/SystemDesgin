# 🟢 Kafka & Event-Driven Architecture

## 🧠 Problem

Avoid tight coupling between systems and enable scalability.

---

## 🧩 Architecture


Producer → Topic → Consumer


---

## 🧠 When to Use Kafka

- Multiple consumers
- High throughput
- Retry required
- Loose coupling

---

## 🔴 When NOT to Use

- Simple CRUD
- Low traffic systems

---

## ⚖️ Key Concepts

### Partitioning
- Enables parallel processing

---

### Ordering
- Guaranteed per partition

---

### Delivery Semantics

| Type | Meaning |
|------|--------|
| At least once | Possible duplicates |
| At most once | Possible loss |
| Exactly once | Complex |

---

## 🔥 Golden Rule

> Kafka is for scale, not simplicity
