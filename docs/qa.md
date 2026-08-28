# 9. Testing & Quality Assurance

---

## 9.1 Testing Strategy
The QA matrix for the platform focuses on validating API structural integrity, cross-platform client workflows, and content safety barriers. Rather than relying on isolated code-level unit tests, the team uses an integrated approach: combining manual and automated API validation, frontend regression tests, and schema verification tools.

<div class="ai-arch-card" autocomplete="off">

```text
+--------------------------------------------------------+

|                 End-to-End UI Testing                  |
|       Playwright (Apps)  |  Cypress (Info Site)        |
+--------------------------------------------------------+
                           |
                           v
+--------------------------------------------------------+

|                 Integration & API Layer                |
|       Shared Postman Collection & Env Profiles         |
+--------------------------------------------------------+
                           |
                           v
+--------------------------------------------------------+

|                 Structural Evaluation                  |
|       Manual Admin Review & Custom Pydantic Schemas     |
+--------------------------------------------------------+
```

</div>

---

## 9.2 Unit Testing
* **Implementation Status**: There are no automated unit test suites (such as `pytest` or `flutter_test`) built directly into the repository code.
* **Mitigation Strategy**: Code quality, validation routing, and state changes are guarded through rigorous integration tests via Postman, alongside automated end-to-end browser workflows.

---

## 9.3 API Testing
The team relies on Postman to perform comprehensive API validation and regression testing across all active backend paths.

### 9.3.1 Collection Sharing and Environment Management
To maintain testing consistency across the engineering team, all endpoints are exported and shared as a unified Postman Collection. Team members import this centralized collection and configure localized environment variables to safely execute requests against different stages of the lifecycle:
* `{{baseUrl}}`: The target deployment domain (e.g., local development loop or live Heroku instance).
* `{{jwt_token}}`: The active OAuth2 bearer string, dynamically injected into request headers for role-restricted routes.

### 9.3.2 Monitored API Routes
While the team tests all available endpoint paths during development cycles, the following core routes are continuously targeted to confirm service availability:
* <code class="ai-arch-route-code">POST /auth/login</code>: Validates admin, provider, and player identity verification, ensuring seamless session token generation.
* <code class="ai-arch-route-code">POST /game_files/generate_payload</code>: Evaluates the backend’s ability to call Gemini File Search and return structurally correct game assets.
* <code class="ai-arch-route-code">POST /support/request</code>: Confirms that player-initiated safety tickets route into the database.

---

## 9.4 End-to-End (E2E) UI Testing
The system's user interfaces are covered by automated testing frameworks to ensure that user journeys remain functional across software deployments.

### 9.4.1 Application Interfaces (Playwright)
The team uses Playwright to automate user interactions across the Next.js administrative and support dashboards. Playwright scripts simulate operational tasks—such as logging in, handling multi-factor authentication (MFA) forms, sorting support cases, and interacting with the backend configuration panels—to catch front-end regressions early.

### 9.4.2 Informational Website (Cypress)
The platform’s public-facing informational website is isolated and tested using Cypress. Cypress scripts run automated validation routines, verifying that links, educational resources, media assets, and structural responsive designs behave correctly across browser configurations.

<p style="border-left: 3px solid #662D91; padding-left: 10px; color: #555555; font-style: italic; margin-top: 1rem;">
    <strong>Note:</strong> Refer to the appendix or team repository records for the live link to the Cypress test suite.
</p>

### 9.4.3 Critical User Validation Journeys
Before any release build is approved for production deployment, engineers must manually verify the platform's core operational loops:

1. **Identity Verification Lifecycle**: A new player account is created &rarr; the system generates a multi-factor authentication (MFA) PIN &rarr; the code delivers via Resend &rarr; the user logs in and gets their JWT bearer token.
2. **Support Escalation Workflow**: A logged-in player initiates an assistance ticket from the Flutter client &rarr; the request saves in PostgreSQL &rarr; the ticket instantly shows up on the Support Provider dashboard for triage.
3. **Content Management Pipeline**: An administrator triggers an AI content generation cycle &rarr; the backend catches the raw JSON string &rarr; the node remains locked until an admin manually moves it from a pending draft to an approved game module.

---

## 9.5 Test Cases & Matrix Registry
* **Centralized Test Repository**: To maintain a clear audit trail and prevent document bloat, the team's complete, granular testing matrix—including Test IDs, preconditions, step-by-step actions, and execution history—is managed externally.
* **Active Tracking Link**: Reviewers, QA engineers, and developers can access the live sheets, execution states, and verification steps here: [Google Sheets Test Case Matrix](https://docs.google.com/spreadsheets/d/1kHORuXDbEKuqAAF37_oFAgU9tYATSDDVUQDvVxgjPw0/edit?usp=sharing)

---

## 9.6 Known Issues

<div class="ai-arch-card" autocomplete="off">
    <h3>Gemini Structural Schema Mismatches</h3>
    <p>The Next.js interactive JSON renderer will throw an application exception or fail to draw the configuration model if the underlying Google Gemini generation fails to meet the strict structural layout requirements. If the AI model returns data with missing keys or mismatched array limits, the schema parser blocks processing to protect the Flutter client from corrupt game files.</p>
    <p style="margin-top: 1rem !important;"><strong>Workaround:</strong> Administrators must click <span style="color: #b30065; font-weight: bold;">Reject</span> on malformed payloads to clear the staging queue and run a new content generation cycle with revised prompt parameters.</p>
</div>
