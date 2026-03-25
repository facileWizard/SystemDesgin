# 🔐 Authentication Methods in System Design

## 🛠️ Configuring OAuth 2.0 via SAP GUI (On-Prem)
If Fiori is unavailable, follow this manual configuration path in the classic ABAP stack.

### 1. Service Activation
* **Transaction:** `SICF`
* **Path:** `/sap/opu/odata4/sap/zui_sdmatgrp_v4`
* **Action:** Ensure the service is **Active**.

### 2. OAuth 2.0 Provider Setup
* **Transaction:** `SOAUTH2`
* **Configuration:** Create a Client Profile with `Client Credentials`.

### 3. User & Security Mapping
| Step | Transaction | Component | Description |
| :--- | :--- | :--- | :--- |
| **A** | `SU01` | Technical User | Create a `System` type user. |
| **B** | `SOAUTH2` | Client Mapping | Map OAuth Client ID to user. |

####

🧠 Doubts Cleared (Interview Perspective)

#### Q: Is the Fiori setup only for SAP Cloud (BTP)?
#### A: No. Modern On-Premise S/4HANA systems also use Fiori apps like "Communication Arrangements" for a simplified setup, but SOAUTH2 remains the underlying engine.

#### Q: Does the RAP (ABAP RESTful Application Programming) layer handle the OAuth validation?
#### A: No. The SAP Kernel handles the OAuth handshake and security validation before the request ever reaches the RAP handler. If the token is invalid, the RAP code is never executed.

---

### 4. Integration Flow

```mermaid
sequenceDiagram
    participant Ext as External System
    participant Kernel as SAP Kernel
    participant RAP as RAP Service
    
    Note over Ext, Kernel: 1. Get Token
    Ext->>Kernel: POST /sap/bc/sec/oauth2/token
    Kernel-->>Ext: Returns Access Token
    
    Note over Ext, RAP: 2. Call Data
    Ext->>Kernel: GET /zui_sdmatgrp_v4
    Kernel->>RAP: Validate Token
    RAP-->>Ext: 200 OK
