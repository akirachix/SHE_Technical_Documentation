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

## Group members

- Dorcas Wanjiru
- Susan Wanjiru
- Stacey Nduta
- Sumaya Abdi
- Ntirenganya Cynthia