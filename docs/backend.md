

<div class="backend-page-header">
  <h1>Backend Architecture & Reference</h1>
  <p>Welcome to the backend technical documentation for <strong>DadaSafe</strong>. This section covers the database schema, live API docs, core endpoints, authentication, error handling, and deployment pipeline for the FastAPI backend.</p>
</div>
<div class="backend-card database-schema">
  <h2><span class="twemoji"></span> Database Schema</h2>
  <p class="card-subtitle">Relational schema implemented on PostgreSQL, defined as SQLAlchemy models under <code>dadasafe/models/</code> and versioned through Alembic migrations.</p>

  <p>Data is organised across seven core tables:</p>
  <ul class="card-feature-list">
    <li><strong>organisations</strong>: Stores info about support groups. Each group has a unique 5-character <code>organisation_code</code> used when they sign up.</li>
    <li><strong>users</strong>: Stores user accounts for Players, Admins, and Support Providers. It saves hashed passwords, encrypted phone numbers, MFA security settings, and account status.</li>
    <li><strong>avatars</strong>: A list of all selectable profile pictures and their direct web links (URLs).</li>
    <li><strong>game_files</strong>: Stored documents uploaded by admin. They connect directly to the Gemini AI file system to help generate games, sorted by theme and topic.</li>
    <li><strong>games</strong>: Individual AI-generated games. It tracks the game topic, the AI prompt used, the final generated JSON data, review status and version number.</li>
    <li><strong>game_progress</strong>: Tracks a player's journey through a game. It records their current game status, total score, question/answer history and exact start and finish times.</li>
    <li><strong>support_requests</strong>: Help tickets sent by players. Each ticket connects back to the player and an organisation, tracking the issue from start to finish.</li>
  </ul>

  <div class="diagram-container">
    <p>The full entity-relationship diagram is maintained in <a href="https://lucid.app" target="_blank" class="inline-erd-link">this Lucidchart ERD</a>.</p>
  </div>
</div>
<div class="backend-card environment-configuration">
  <h2><span class="twemoji"></span> Configuration &amp; Environment Variables</h2>
  <p class="card-subtitle">Local runtime parameters loaded asynchronously during application startup. Security configurations are maintained strictly in untracked environment profiles.</p>
  
  <p>To configure your local environment instance, create a file named exactly <code>.env</code> in your repository backend root folder using the following structural baseline blueprint:</p>

  <div class="code-box-wrapper">
<pre><code class="language-env">
GEMINI_API_KEY=[YOUR_GOOGLE_GEMINI_API_KEY_PLACEHOLDER]

DATABASE_URL=postgresql+asyncpg://[DB_USER]:[DB_PASSWORD]@[DB_HOST]:5432/[DB_NAME]

JWT_SECRET_KEY=[YOUR_JWT_CRYPTOGRAPHIC_HEX_STRING_PLACEHOLDER]

PHONE_ENCRYPTION_KEY=[YOUR_FERNET_SYMMETRIC_BASE64_KEY_PLACEHOLDER]

ADMIN_EMAIL=admin@dadasafe.org
ADMIN_PASSWORD=[COMPLEX_DEFAULT_ADMIN_PASSWORD_PLACEHOLDER]

SMTP_SERVER=://gmail.com
SMTP_PORT=587
SMTP_USERNAME=notifications@dadasafe.org
SMTP_PASSWORD=[YOUR_SMTP_APPLICATION_PASSWORD_PLACEHOLDER]

ALLOWED_ORIGINS="http://localhost:3000,https://dadasafe.org"</code></pre>
  </div>
</div>
<div class="backend-card live-api-docs">
  <h2><span class="twemoji"></span> Live API Docs</h2>
  <p class="card-subtitle">Interactive OpenAPI schema, generated automatically by FastAPI from every router's request/response models.</p>

  <p>With the backend running locally, the full interactive Swagger UI is available at:</p>

  <div class="code-box-wrapper">
    <pre><code>http://127.0.0</code></pre>
  </div>

  <p>The production Swagger UI is live at <a href="https://herokuapp.com" target="_blank" class="inline-erd-link">https://herokuapp.com</a>.</p>
</div>

<div class="backend-card main-endpoints">
  <h2><span class="twemoji"></span> Backend Endpoints</h2>
  <p class="card-subtitle">Architecture overview and implementation logic mappings.</p>

  <p>The backend architecture exposes modular REST API endpoints grouped across distinct operational domains. This includes user onboarding, organizations, asset management, gameplay mechanics, and support ticketing tracking pipelines. These routes serve as the interactive layer that directly manipulates and queries data across the <strong>Database Schema</strong> tables outlined above.</p>
  
  <p>To ensure documentation remains clean, easily maintainable, and structurally isolated from shifting route signatures, specific endpoint signatures are omitted from this summary. For a comprehensive, live, up-to-date look at the complete routing architecture, query parameters, and operational schemas, please consult our codebase directly inside the <a href="https://github.com" target="_blank" class="inline-erd-link">GitHub Repository</a>.</p>
</div>
<div class="backend-card example-api-usage">
  <h2><span class="twemoji"></span> Example API Usage</h2>
  <p class="card-subtitle">Sample request and response payload for creating or generating a game entry.</p>

  <p><strong>Sample Request Body: POST <code>/games</code></strong></p>
  <div class="code-box-wrapper">
<pre><code class="language-json">{
  "topic": "Digital Safety Basics",
  "source_file_name": "quiz_geography.json",
  "game_title": "Cyber Hygiene Challenge",
  "game_payload": {},
  "prompt": "Generate a 10-question interactive quiz based on the source file grounding text."
}</code></pre>
  </div>

  <p><strong>Sample Response: <code>201 Created</code> (Successful Response)</strong></p>
  <div class="code-box-wrapper">
<pre><code class="language-json">{
  "game_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "topic": "Digital Safety Basics",
  "source_file_name": "quiz_geography.json",
  "generated_by_admin_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "reviewed_by_admin_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "reviewed_at": "2026-08-28T11:37:48.769Z",
  "game_title": "Cyber Hygiene Challenge",
  "game_payload": {},
  "game_status": "pending",
  "version": 0,
  "game_created_at": "2026-08-28T11:37:48.770Z",
  "game_updated_at": "2026-08-28T11:37:48.770Z"
}</code></pre>
  </div>
</div>

<div class="backend-card api-testing">
  <h2><span class="twemoji"></span> API Testing and Documentation</h2>
  <p class="card-subtitle">Automated test suite status and backend tracking metrics.</p>

  <p>The backend routing layer is verified using an <strong>automated Postman test collection</strong>. Tests are executed across every endpoint to validate schema constraints, data types, and status code responses under different environment conditions:</p>
  
  <ul class="card-feature-list">
    <li><strong>Happy Path Automation</strong> — Automates validation of standard request flows, payload creation, and correct <code>200 OK</code> or <code>201 Created</code> response returns.</li>
    <li><strong>Negative &amp; Edge Case Automation</strong> — Automatically verifies that input anomalies, missing fields, and bad schemas trigger predictable validation errors to ensure platform reliability.</li>
  </ul>
</div>
<div class="backend-card security-auth">
  <h2><span class="twemoji"></span> Security and Authentication</h2>
  <p class="card-subtitle">Implemented in <code>dadasafe/core/security.py</code>, <code>dadasafe/core/limiter.py</code>, and <code>dadasafe/core/config.py</code>.</p>

  <ul class="card-feature-list">
    <li><strong>JWT Access Tokens (python-jose)</strong> — Implements stateless, time-bound session management. Users receive a cryptographically signed token upon login, authorizing subsequent API calls until the <code>ACCESS_TOKEN_EXPIRE_MINUTES</code> threshold is reached.</li>
    <li><strong>Bcrypt Password Hashing (passlib)</strong> — Protects user credentials against data breaches. Passwords undergo cryptographic salting and iterative hashing before database storage, ensuring raw passwords cannot be reverse-engineered.</li>
    <li><strong>Multi-Factor Authentication (pyotp)</strong> — Provides secondary identity verification. Users authenticate using Time-Based One-Time Passwords (TOTP) from an authenticator app or email OTPs, monitored by validation window expiration timers and strict attempt lockout thresholds.</li>
    <li><strong>Fernet Symmetric Encryption</strong> — Ensures data privacy for sensitive database records. Restricted fields like user phone numbers are encrypted at rest using a 128-bit key (<code>PHONE_ENCRYPTION_KEY</code>), meaning data is unreadable even if the database layer is directly accessed.</li>
    <li><strong>Rate Limiting (slowapi)</strong> — Mitigates brute-force and Denial of Service (DoS) attacks. Middleware tracks client request frequencies on highly sensitive endpoints (login, registration, password resets, and OTP delivery) and temporarily blocks abusive IP addresses.</li>
    <li><strong>CORS & Security Headers</strong> — Restricts browser-side cross-origin access exclusively to verified domains declared in the <code>ALLOWED_ORIGINS</code> registry. Additionally, standard defense headers (including HSTS and X-Frame-Options) are injected via middleware to prevent clickjacking and session interception.</li>
  </ul>
</div>

<div class="backend-card error-handling">
  <h2><span class="twemoji"></span> Error Handling</h2>
  <p class="card-subtitle">Standard FastAPI and Pydantic validation error schema, returned automatically when a request violates schema constraints.</p>

  <p><strong>Sample Response: <code>422 Unprocessable Entity</code> (Validation Error)</strong></p>
  <div class="code-box-wrapper">
<pre><code class="language-json">{
  "detail": [
    {
      "loc": [
        "body",
        "topic"
      ],
      "msg": "field required",
      "type": "value_error.missing",
      "input": null,
      "ctx": {}
    }
  ]
}</code></pre>
  </div>
</div>

<div class="backend-card backend-deployment">
  <h2><span class="twemoji"></span> Backend Deployment</h2>
  <p class="card-subtitle">Deployed to Heroku via GitHub Actions (<code>.github/workflows/deploy.yml</code>), triggered on push to <code>main</code>.</p>

  <p>Schema migrations run through Alembic. Release and web process, from <code>Procfile</code>:</p>
  <div class="code-box-wrapper">
<pre><code>release: alembic upgrade head && python backfill_security_stamp.py
web: uvicorn main:dadasafe --host 0.0.0.0 --port $PORT</code></pre>
  </div>
</div>
