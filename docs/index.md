
# SHE++ Technical Documentation

**Product:** dadaSafe
Technical documentation, developer reference and operational guide.

| | |
|---|---|
| **Version** | `[VERSION]` |
| **Last updated** | `[DATE]` |
| **Repository** | `[GITHUB REPOSITORY URL]` |
| **Production URL(s)** | `[URLS]` |
| **Documentation status** | Draft |

Secrets, API keys, passwords, and private credentials must never appear in this documentation.

---

## How to use this doc

Each section below is its own page in the sidebar. Claim a section by putting your name in the **Owner** column and updating **Status** as you go (`Not started` → `In progress` → `Review` → `Done`). Don't guess technical details — mark anything unverified as `QUESTION / VERIFY` on the page itself rather than inventing it.

---

## Sections

| Page | Covers | Owner | Status |
|---|---|---|---|
| [Architecture](architecture.md) | System overview, architecture diagram, data flow, auth flow, external services | | Not started |
| [Brand guideline](brand.md) | Design system: identity, logo, typography, colour palette, components, wireframes | | Not started |
| [Backend](backend.md) | Tech stack, project structure, getting started, config/env vars, **database reference & data dictionary**, API architecture & reference, auth, error handling, integrations, code standards, testing | | Not started |
| [AI](ai.md) | AI architecture, model, grounding/file search, prompt design, payload schema, generation pipeline, human review, evaluation, limitations | | Not started |
| [Frontend Web](frontend.md) | Web app stack, structure, routing, components, state management, API integration, admin & support-provider interfaces, testing, troubleshooting | | Not started |
| [Frontend Mobile](mobile.md) | Mobile stack, architecture, project structure, navigation, game experience, support requests, API integration, offline behaviour, testing | | Not started |
| [QA Process](qa.md) | Testing strategy, unit/API/E2E testing, test cases, evidence, known issues | | Not started |
| [Security](security.md) | Security architecture, authN/authZ, data protection, API security, AI safety, privacy, known considerations | | Not started |
| [Deployment](deployment.md) | Deployment architecture, environment config, backend/web/mobile/database deployment, CI/CD, monitoring, troubleshooting | | Not started |
| [Glossary](glossary.md) | Project-specific acronyms and domain terms | | Not started |

`Developer Guide` topics (local dev setup, branching strategy, git conventions, PRs, code review, contribution guide) live on **this page**, in the section below, since there's no dedicated page for them yet.

---

## Developer guide

- **Local development** — clone, install dependencies, configure environment, run database, run backend, run web, run mobile. `QUESTION / VERIFY`: confirm exact steps against team practice.
- **Branching strategy** — `QUESTION / VERIFY`: confirm main/develop branch setup and feature/bugfix branch naming actually used.
- **Git conventions** — `QUESTION / VERIFY`: confirm commit message format and branch naming convention. Never commit secrets.
- **Pull requests** — `QUESTION / VERIFY`: confirm PR requirements (description, screenshots/evidence, tests, required reviewers, merge rules).
- **Code review** — `QUESTION / VERIFY`: confirm what reviewers check (correctness, security, tests, maintainability, UI/UX).
- **Contribution guide** — `QUESTION / VERIFY`: confirm the setup → issue → implement → test → PR flow the team actually follows.

---

## Product overview

dadaSafe is an educational gaming platform that teaches digital-safety literacy through interactive, gamified learning, and connects users requesting help with verified external support providers.

The platform has three personas, each with an isolated frontend interface and role-based API access:

- **Player** — learns digital safety, plays games, may request support
- **Administrator** — manages content, game generation/review, users, organisations, platform oversight
- **Support Provider** — receives, claims, and resolves player support requests

`QUESTION / VERIFY`: confirm the official relationship between SHE++ (team/org) and dadaSafe (product name).

---




# SHE++ Platform Technical Documentation

## 1. Introduction

### 1.1 Purpose
This document provides a comprehensive technical overview of the architecture, implementation, configuration, and operational workflows for the **SHE++ platform** and its core product, **dadaSafe**. It details the AI-assisted game-generation engine, system security models, testing strategies, deployment pipelines, and core developer practices.

This documentation serves as the primary technical reference for:
* Developers
* System administrators
* Quality assurance engineers
* UX/UI designers
* Future open-source or internal contributors

As the initial version of the system documentation, this text establishes the baseline technical architecture of the active codebase.

### 1.2 Product Overview
**dadaSafe** is an educational gaming platform engineered to empower young women with digital safety literacy through interactive, gamified learning experiences. Beyond its educational core, the platform serves as a secure bridge, connecting users who request targeted assistance with verified, external support providers.

* **SHE++** acts as the parent organization and engineering team behind the platform.
* **dadaSafe** is the official deployment name of the consumer-facing product.

The platform architecture segregates access control and user workflows into three primary operational personas: **Players**, **Administrators**, and **Support Providers**. Each persona interacts with a dedicated, isolated frontend interface backed by role-based access control (RBAC) at the API level.

### 1.3 Problem Statement
> Young women in Kenya aged 18–24 need a highly engaging, safe, and emotionally supportive way to learn about digital safety and access verified mental health channels so that they can be safer online.

While the internet connects Kenya, it also weaponizes silence. Empirical findings indicate that **51.3% of Kenyans face cyber harassment**. For young women, that number skyrockets to **90%**. Yet, **80% of these survivors suffer in complete isolation**, silenced by shame and a lack of awareness.

#### 1.3.1 Core Curricular Domains
The platform delivers localized digital-safety education categorized under eight primary thematic vectors:

* **Location Tracking & Surveillance Countermeasures**: Focuses on anti-tracking strategies, hardware surveillance mitigation, and personal location privacy.
* **Image-Based Abuse (IBA) & Media Violations**: Addresses the non-consensual sharing of intimate media, digital extortion, and safe media distribution guardrails.
* **Device Security & Spyware Mitigation**: Covers OS-level security hardening, stalkerware identification, and IoT/smart-device protection.
* **Social Media & Online Platform Safety**: Teaches platform-specific identity protection, data exposure risk management, and privacy configurations.
* **Legal Protections, Evidence, & Policy**: Guides users through domestic civil/criminal remedies, protection orders, and digital forensic evidence collection.
* **Interpersonal Relationships, Dating, & Demographic Risks**: Deals with tech-facilitated abuse (TFA), romance scams, and digital consent dynamics.
* **Professional Advocacy & Support Agency Security**: Outlines secure communication infrastructure for local advocates and external service providers.
* **Account Security & Digital Hygiene**: Establishes baselines for network safety, credential hardening, and browser footprint reduction.

### 1.4 Target Users
The system enforces demographic-specific validation gates at registration:

* **Players**: Restricted strictly to young women within the 18–24 age cohort.
* **Administrators**: Privileged internal accounts required to be age 18 or above.
* **Support Providers**: Verified external personnel or institutional actors required to be age 18 or above.

---

## 2. Product Scope

### 2.1 In-Scope Capabilities
The active platform implementation covers the following core capabilities:

* **Gamified Pedagogy**: Delivery of automated quizzes, interactive puzzles, and branched narrative decision trees.
* **Grounded Content Generation**: AI-driven generation of educational content, strictly programmatically bound to approved grounding documents to eliminate model hallucinations.
* **Human-in-the-Loop Moderation**: Mandatory administrative review queues for all AI-generated content prior to production deployment.
* **Support Pipeline**: Secure user-initiated workflows to dispatch structured assistance requests to the support queue.
* **Case Management**: Multi-tenant ticket lifecycle tracking (*Claimed*, *Pending*, *Resolved*) for verified support networks.
* **System Administration**: Unified dashboard for asset management, user permission configuration, and system auditing.
* **Tech Abuse Report Generator**: Programmatic pipeline that compiles verified user incidents into formatted logs, aiding evidence preservation.

### 2.2 Out-of-Scope System Boundaries
To maintain structural compliance and mitigate liability, the following domains are explicitly excluded from the software's functional scope:

* **Native Telehealth/Counseling**: The platform does not host live therapeutic, psychiatric, or psychological counseling sessions within the software layer.
* **In-App Chat Engines**: Direct, real-time messaging between players and support providers is excluded; communication occurs via verified external channels provided during registration.
* **Emergency Dispatch**: The platform is not an emergency response system or a real-time crisis hotline.
* **Law Enforcement Integration**: The platform does not feature automated APIs for reporting incidents directly to state or local law enforcement agencies.
* **Automated NGO Escalation**: Support routing requires manual opt-in from the player and manual case claiming from providers; no automated algorithmic assignments are executed.

---

## 3. System Personas

| Persona | Core API Permissions | Primary Interfaces / Screens | Key Restrictions & Boundaries |
| :--- | :--- | :--- | :--- |
| **Player** | `read:games`<br>`write:support_requests`<br>`execute:game_modules` | Module Selection Matrix, Interactive Activity Interfaces (Quizzes, Puzzles, Scenarios), Support Request Forms, Personal Profile / Progress Tracking Dashboards. | Strict UI isolation. Zero structural exposure to administrative endpoints or cross-tenant data. Can only view/edit metadata of their own unique account. |
| **Admin** | `global:read`<br>`global:write`<br>`user:suspend`<br>`user:ban`<br>`system:config` | Global Operations Metrics Dashboard, System Log Monitor, User Account Moderation Panel, Content Ingestion and Grounding File Configurator, Specialized Interactive JSON Renderer. | Administrative control panel access only. Uses JSON renderer for structural validation and platform configuration mutations. Armed with global suspension tools. |
| **Support Provider** | `read:assigned_tickets`<br>`write:ticket_resolution` | Open Case Marketplace, Active Intake Queue Dashboard, Individual Case Details Panel, Case Resolution Logger. | Scoped exclusively to support workflows. Blind to game configurations, LLM prompts, and admin metrics. Restricted from altering permissions or accessing unassigned cases. |

---

## 4. Key Features

* **Grounded LLM Orchestration**: Programmatic ingestion of authoritative safety PDFs/text docs to steer AI generation parameters using context injection.
* **Deterministic Evaluation Layer**: Quiz engine enforcing exactly five mandatory, structurally validated questions per module.
* **Interactive Mini-Game Engines**: Dynamic generation interfaces for word searches, crosswords, and word scrambles based on generated metadata.
* **Deterministic Branching Scenarios**: Interactive text-adventure frameworks executing condition-based state transitions reflecting user choices.
* **Asynchronous Support Ticketing**: Decoupled messaging architecture tracking stateful transformations of player-initiated support request objects.
* **Granular Asset Management**: Storage infrastructure for securely hosting, versioning, and managing grounding documentation vectors.
* **System Auditing and Analytics**: Logging facilities tracking global platform engagement metrics and system health indicators.

