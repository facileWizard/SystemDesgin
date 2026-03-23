# 🔗 Integration Patterns (Single vs Multiple Systems)

## 🧠 Problem

How to design integration between ERP and external systems.

---

## 🧩 Patterns

### 1. Single System Integration


Client → API → SAP


✅ Simple  
❌ Not scalable  

---

### 2. Multiple Systems Integration


SAP → Kafka → Multiple Consumers


✅ Scalable  
✅ Decoupled  

---

## ⚖️ Decision Table

| Scenario | Use |
|--------|-----|
| One consumer | API |
| Multiple consumers | Event Bus |
| High traffic | Kafka |

---

## 🔥 Golden Rule

> If more than one system consumes the same data → use events
