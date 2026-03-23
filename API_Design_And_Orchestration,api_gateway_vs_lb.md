# 🌐 API Gateway vs Load Balancer vs Ingress

## 🧠 Problem

Manage incoming traffic efficiently.

---

## ⚖️ Comparison

| Component | Purpose |
|----------|--------|
| Load Balancer | Distribute traffic |
| API Gateway | Auth, rate limit |
| Ingress | K8s routing |

---

## 🧠 When to Use API Gateway

- Multiple services
- Authentication required
- Rate limiting needed

---

## 🧠 When to Use Load Balancer

- Simple traffic distribution

---

## 🧠 When to Use Ingress

- Kubernetes-based systems

---

## 🔥 Golden Rule

> Gateway = control, LB = distribution

# 🧩 Orchestration Layer

## 🧠 Problem

Multiple systems need to be coordinated in a single workflow.

---

## 🧩 Architecture


Frontend → Orchestrator → SAP + Payment + Notification


---

## 🧠 Responsibilities

- Data transformation
- Aggregation
- Workflow handling

---

## ⚖️ When Needed

- Multi-step workflows
- Multiple API calls

---

## 🔥 Example

Order flow:
1. Create order in SAP
2. Call payment API
3. Send notification

---

## 🚫 When NOT Needed

- Simple CRUD APIs
