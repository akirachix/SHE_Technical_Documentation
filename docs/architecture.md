# 2. System Architecture

---

## 2.1 Architecture Overview
The system is built on a decoupled **Client-Server architecture** utilizing a centralized, monolithic backend API that services multiple frontend client targets. The engineering stack relies on highly scalable, modern runtimes designed for fast data processing and real-time AI orchestration:

* **Administrative & Support Interfaces**: Built using **Next.js** and deployed to **Vercel**.
* **Player Client Application**: Formulated as a cross-platform mobile application using **Flutter**.
* **Backend Integration Layer**: Driven by a unified **FastAPI** framework deployed to **Heroku**.
* **Persistence Layer**: Powered by a relational **PostgreSQL** database instance hosted on **Heroku**.
* **AI Integration & Grounding**: Orchestrated via **Google Gemini** and **Gemini File Search** using structured Markdown storage.
* **Transactional Communication**: Handled asynchronously through **Resend**.

All user interfaces (Next.js web and Flutter mobile) converge on the single, shared FastAPI backend. The backend acts as the authoritative orchestration hub for routing business logic, database queries, and third-party integrations.

---

## 2.2 System Architecture Diagram
<div class="backend-card" autocomplete="off">
    <img src="../assets/architecture/system_architecture.png" alt="System Architecture Diagram" style="max-width: 100%; border-radius: 4px; display: block; margin: 0 auto;">
</div>
---


## 2.3 Application Components

### 2.3.1 Web Application (Next.js)
Serves exclusively as the desktop-optimized command interface for Administrators and Support Providers. It handles heavy operations such as file asset ingestion, structural game-generation auditing, user suspension queues, and support ticket management. It features a custom interactive JSON renderer for real-time model response validation.

### 2.3.2 Mobile Application (Flutter)
Acts as the exclusive interface for the Player persona. All educational activities, quizzes, puzzles, branching scenarios, and support requests are native to this environment. No player-facing game elements or learning modules exist on the web platform at this time.

### 2.3.3 Backend API (FastAPI)
The central engine governing the system. It handles structural API routing, authentication token issuance, Multi-Factor Authentication (MFA) workflows, database connection pooling, custom schema evaluations, and algorithmic game transformation processing.

### 2.3.4 Database (PostgreSQL)
The relational store hosting persistent relational schemas. This includes user identities, encrypted authentication metadata, active support tickets, multi-tenant logging data, and the final validated payloads for AI-generated games.

### 2.3.5 AI Generation Layer (Gemini + File Search)
A composite pipeline using Google Gemini API to drive context-guided learning content. It couples LLM generation with Gemini File Search, which relies on curated Markdown documents to serve as the ground-truth knowledge repository for digital-safety compliance.

### 2.3.6 External Services (Resend)
Provides programmatic email delivery infrastructure, functioning primarily as the message broker for security alerts and the verification channel for the platform's multi-factor authentication (MFA) layer.

---

## 2.4 Data Flow
A key data flow begins when an administrator supplies a parent theme, topic, and generation prompt. The backend identifies the relevant grounding document, obtains the required file-search reference, and uses the grounded AI generation workflow to produce a structured game payload. 

```text
 [ Admin Prompt ] 
        │
        ▼
 [ FastAPI Intercepts ] ──► [ Gemini File Search ] (Match Markdown Vectors)
        │
        ▼
 [ Google Gemini API ] (Generates JSON Game Payload)
        │
        ▼
 [ Next.js Review Queue ] (Mounted in Interactive JSON Renderer)
        │
        ▼
 [ Pydantic Verification ] ──► Written to [ PostgreSQL ] ──► Available to [ Flutter Mobile ]
```

### Operational Workflow Steps:
1. **Request Initiation**: The administrator defines a parent theme, specific topic parameters, and an instructions prompt within the Next.js panel.
2. **Context Injection**: FastAPI intercepts the parameters, calling Gemini File Search to match the prompt against specialized Markdown reference documents stored in the knowledge vector space.
3. **Generation**: The compiled prompt and Markdown references are securely passed to Google Gemini, which generates a structured JSON educational game payload.
4. **Human Review**: The raw generation payload is returned to the Next.js web application and mounted inside an administrative interactive JSON renderer.
5. **Schema Enforcement & Storage**: Once an administrator reviews, modifies, and signs off on the content, the backend applies custom Pydantic schemas to validate the structural integrity of the payload. After successful structural verification, the game object is written to the PostgreSQL database on Heroku, instantly making it available to the Flutter mobile clients.

---

## 2.5 Authentication & Authorization Flow

### 2.5.1 Authentication Mechanism
* **Protocol Specification**: The system implements the OAuth2 standard with a password bearer flow (`OAuth2PasswordBearer`) as the foundation for secure client-server handshake procedures.
* **Token Model**: Upon successful identity verification, the backend issues stateless JSON Web Tokens (JWT) conforming to the OAuth2 specification, carried over secure HTTPS `Authorization: Bearer <token>` headers for all downstream API-level verification.
* **Multi-Factor Authentication (MFA)**: Access requests require dynamic out-of-band verification. Upon receiving valid primary OAuth2 credentials, FastAPI intercepts the authentication lifecycle, halts token generation, generates an ephemeral cryptographic PIN, and fires it asynchronously via Resend to the user's registered communication channel. The user must supply this verification PIN to complete the OAuth2 flow and obtain an active bearer session token.

### 2.5.2 Role-Based Access Control (RBAC) Enforcement
The platform enforces role-based access control symmetrically at both the client and server application layers to prevent unauthorized escalation:

* **Frontend Guarding (Next.js & Flutter)**: Client routers inspect the user role payload claims embedded within the verified JWT. UI route targets are structurally blocked, and specific view capabilities (such as the JSON renderer or user suspension buttons) are fully hidden if the user profile fails to match the required authorization group.
* **Backend Guarding (FastAPI Middleware)**: Symmetrical protection is applied via backend dependency injection tied directly to the OAuth2 bearer security scheme. Every protected route handler relies on specialized security sub-dependencies that parse the incoming OAuth2 bearer tokens, re-verify signature parameters against system environment configurations, and audit role permissions against the targeted database records. Attempts to bypass UI restrictions trigger an immediate, deterministic `403 Forbidden` response.

---

## 2.6 External Services
The platform isolates specific infrastructure, computational, and communication responsibilities to vetted third-party service providers. These integrations are configured via strict environment variables injected at the runtime layer:

* **Google Gemini API / Gemini File Search**: Serves as the core Large Language Model (LLM) orchestration provider. The platform utilizes Gemini for grounded AI game-content generation, relying directly on its native File Search vector storage capacities to index, reference, and parse local Markdown safety documents.
* **Resend**: Acts as the primary transactional email delivery network and message broker. It handles the out-of-band delivery of cryptographic multi-factor authentication (MFA) PINs and account lifecycle notifications.
* **Heroku PaaS**: Host infrastructure for both the core Python runtime executing the FastAPI backend API and the managed, high-availability relational PostgreSQL database instance.
* **Vercel**: Cloud edge-hosting infrastructure utilized for the static compilation, global distribution, and automated deployment pipelines of the Next.js Administrative and Support Provider web applications.
