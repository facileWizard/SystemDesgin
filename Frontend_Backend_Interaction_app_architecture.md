# 🏗️ Frontend + Backend + ERP Architecture

## 🧠 Flow


Frontend → API → Backend → ERP


---

## 🧠 Responsibilities

### Frontend
- UI
- User interaction

---

### Backend
- Validation
- Business logic
- Security

---

### ERP
- Core data
- Transactions

---

## 🔥 Golden Rule

> ERP should never be directly exposed

# 🧱 Application Schema Design

## 🧠 Problem

Design database separate from ERP.

---

## 🧠 Approach

- Maintain application DB
- Sync with ERP

---

## 🧠 Benefits

- Performance
- Flexibility

---

## 🔥 Golden Rule

> Do not rely solely on ERP DB

# 🔍 Search Design (Fuzzy Search)

## 🧠 Problem

Efficient searching in applications.

---

## 🧩 Approaches

### 1. SQL LIKE
- Simple
- Limited

---

### 2. ElasticSearch
- Fast
- Scalable

---

### 3. Pattern Matching
- Custom logic

---

## 🔥 Golden Rule

> Use Elastic for scale
