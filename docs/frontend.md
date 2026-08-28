<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Frontend Web | DadaSafe Technical Documentation</title>

  <link rel="stylesheet" href="../stylesheets/frontend.css">
</head>

<body>

<main class="frontend-documentation">


  <section class="page-header">
    <h1>Frontend Web</h1>
    <p class="subtitle">
      Admin &amp; Support Interface — Next.js dashboard, deployed on Vercel
    </p>
  </section>



  <section>

    <h2>1. Authentication Flows</h2>


    <!-- LOGIN -->

    <h3>1.1 Login Page</h3>

    <p>
      Used by Admins and Support Providers to authenticate
      and receive an access token.
    </p>

    <div class="field-list">
      <strong>Fields:</strong> Email, Password
    </div>

    <div class="label">API Endpoint</div>

    <div class="endpoint">
      <span class="method">POST</span>
      /auth/login
    </div>


    <div class="label">Sample Request</div>

    <pre class="code-block">{
  <span class="k">"email"</span>: <span class="s">"admin@dadasafe.org"</span>,
  <span class="k">"password"</span>: <span class="s">"Password123!"</span>
}</pre>


    <div class="label">
      Sample Response
      <span class="status-tag">200 OK</span>
    </div>

    <pre class="code-block">{
  <span class="k">"access_token"</span>: <span class="s">"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."</span>,
  <span class="k">"token_type"</span>: <span class="s">"bearer"</span>
}</pre>


    <div class="note">
      All subsequent authenticated requests must include the token as
      <code>Authorization: Bearer &lt;access_token&gt;</code>.
    </div>


    <!-- LOGIN SCREENSHOT -->

    <div class="screenshot-card">

      <div>

        <img
          src="../assets/dashboardassets/dadasafe_login_screenshot.png"
          alt="DadaSafe login screen showing email and password fields"
        >

        <div class="screenshot-caption">
          DadaSafe Login screen — Email, Password, Login
        </div>

      </div>

    </div>


    <hr class="divider">


    <!-- REGISTRATION -->

    <h3>1.2 Registration</h3>

    <p>
      Creates a new account.
      <code>user_type</code> determines whether the account is an
      Admin or Support Provider.
    </p>


    <div class="field-list">

      <strong>Fields:</strong>

      Email, Password (10+ characters, must include an uppercase
      letter and a digit), User Type
      (admin / support_provider), Name,
      Gender (male / female),
      Organisation ID <em>(optional)</em>

    </div>


    <div class="label">API Endpoint</div>

    <div class="endpoint">
      <span class="method">POST</span>
      /auth/register
    </div>


    <div class="label">Sample Request</div>

    <pre class="code-block">{
  <span class="k">"email"</span>: <span class="s">"jane@dadasafe.org"</span>,
  <span class="k">"password"</span>: <span class="s">"SecurePass1"</span>,
  <span class="k">"user_type"</span>: <span class="s">"admin"</span>,
  <span class="k">"name"</span>: <span class="s">"Jane Doe"</span>,
  <span class="k">"gender"</span>: <span class="s">"female"</span>,
  <span class="k">"organisation_id"</span>: <span class="s">null</span>
}</pre>


    <div class="label">

      Sample Response

      <span class="status-tag">
        201 Created
      </span>

    </div>


    <pre class="code-block">{
  <span class="k">"user_id"</span>: <span class="s">"a7f3c1e2-..."</span>,
  <span class="k">"avatar_id"</span>: <span class="s">"d92b0a4f-..."</span>,
  <span class="k">"email"</span>: <span class="s">"jane@dadasafe.org"</span>,
  <span class="k">"user_type"</span>: <span class="s">"admin"</span>,
  <span class="k">"name"</span>: <span class="s">"Jane Doe"</span>,
  <span class="k">"gender"</span>: <span class="s">"female"</span>,
  <span class="k">"account_status"</span>: <span class="s">"active"</span>,
  <span class="k">"organisation_id"</span>: <span class="s">null</span>
}</pre>


    <hr class="divider">


    <!-- CURRENT USER -->

    <h3>1.3 Current User Profile</h3>

    <p>
      Fetch or update the logged-in user's profile.
      Requires a bearer token.
    </p>


    <div class="endpoint">
      <span class="method">GET</span>
      /users/me
    </div>

    <div class="endpoint">
      <span class="method">PATCH</span>
      /users/me
    </div>


    <hr class="divider">


    <!-- DELETE ACCOUNT -->

    <h3>1.4 Account Deletion</h3>

    <p>
      Soft-deletes the current account by setting
      <code>deleted_at</code> and
      <code>account_status</code> to
      <code>suspended</code>.
    </p>


    <div class="endpoint">
      <span class="method">DELETE</span>
      /users/me
    </div>


    <div class="note">

      A token used after deletion still passes authentication,
      since deleted accounts aren't filtered at that step —
      it fails later with

      <strong>
        403 "Account is not active"
      </strong>

      rather than 401.

    </div>

  </section>




  <section>

    <hr class="divider">

    <h2>2. Project Structure</h2>

    <p>
      The web dashboard lives in the
      <code>SHE_Dashboard</code> repo,
      built with Next.js App Router.
    </p>


    <pre class="code-block folder-tree"><span class="c">SHE_Dashboard/</span>
├── .github/
├── .next/
├── .vercel/
├── app/
│   ├── <span class="c">(auth)/</span>
│   │   ├── forgot-password/
│   │   │   └── page.jsx
│   │   ├── login/
│   │   │   └── page.jsx
│   │   ├── signup/
│   │   └── layout.jsx
│   │
│   ├── admin/
│   │   ├── control/
│   │   │   ├── uploads/
│   │   │   │   └── page.jsx
│   │   │   ├── control.css
│   │   │   └── page.jsx
│   │   ├── dashboard/
│   │   │   └── page.jsx
│   │   ├── settings/
│   │   │   └── page.jsx
│   │   └── layout.jsx
│   │
│   ├── support/
│   │   ├── dashboard/
│   │   │   └── page.jsx
│   │   ├── profile/
│   │   │   └── page.jsx
│   │   ├── settings/
│   │   │   └── page.jsx
│   │   └── layout.jsx
│   │
│   ├── favicon.ico
│   ├── globals.css
│   ├── layout.jsx
│   ├── middleware.js
│   └── page.tsx
│
├── components/
│   ├── admin/
│   │   ├── GamePreview.jsx
│   │   ├── GameStatcard.jsx
│   │   ├── JsonEditor.jsx
│   │   ├── PromptBox.jsx
│   │   ├── RecentPrompts.jsx
│   │   ├── sidebar.jsx
│   │   ├── statCard.jsx
│   │   └── TopicSelector.jsx
│   │
│   ├── shared/
│   │   ├── Button.jsx
│   │   ├── Input.jsx
│   │   └── Logo.jsx
│   │
│   └── support/
│       ├── CasestatsChart.jsx
│       ├── CaseTable.jsx
│       └── Sidebar.jsx
│
├── hooks/
│   ├── useAuth.js
│   └── useRoleGuard.js
│
├── lib/
│   ├── api/
│   │   ├── auth.js
│   │   ├── avatars.js
│   │   ├── client.js
│   │   ├── game_files.js
│   │   ├── game_progress.js
│   │   ├── games.js
│   │   ├── organisations.js
│   │   ├── registration.js
│   │   └── support_requests.js
│   └── utils.js
│
├── public/
│   └── assets/
│       ├── background.png
│       ├── control.png
│       ├── controls.png
│       ├── dashboard.png
│       ├── file.svg
│       ├── globe.svg
│       ├── image2.png
│       ├── lady_onphone_forward.png
│       ├── lady_onphone.png
│       ├── lady_pink.png
│       ├── lady_red.png
│       ├── logoblack.png
│       ├── mentalhealth.png
│       ├── next.svg
│       ├── setting.png
│       ├── settings.png
│       ├── she-logo.png
│       ├── vercel.svg
│       ├── window.svg
│       ├── woman.png
│       └── woman1.png
│
├── node_modules/
├── playwright-report/
└── test-results/</pre>


    <div class="note">

      Routes are grouped by role:

      <code>(auth)</code> holds unauthenticated pages,

      <code>admin/</code> and <code>support/</code> each have
      their own layout and dashboard,

      and <code>middleware.js</code> handles role-based
      route protection.

    </div>

  </section>



  <section>

    <hr class="divider">

    <h2>3. First Steps After Login</h2>

    <ul>

      <li>
        <strong>Support Providers</strong> —
        view and respond to assigned support requests.
      </li>

      <li>
        <strong>Admins</strong> —
        manage organisations, monitor game content,
        and review support activity.
      </li>

    </ul>

  </section>

</main>

</body>
</html>