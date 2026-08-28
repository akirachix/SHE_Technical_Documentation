<!DOCTYPE html>
<html lang="en">

<head>

  <meta charset="UTF-8">

  <meta
    name="viewport"
    content="width=device-width, initial-scale=1.0"
  >

  <title>
    Frontend Web | DadaSafe Technical Documentation
  </title>

  <link
    rel="stylesheet"
    href="../stylesheets/frontend.css"
  >

</head>

<body>

<main class="frontend-documentation">

  <section class="page-header">

    <h1>
      Frontend Web
    </h1>

    <p class="subtitle">

      DadaSafe Web Dashboard — Admin and Support Provider Interface

    </p>

  </section>

  <section>

    <h2>
      1. Overview
    </h2>

    <p>

      The DadaSafe web frontend is a role-based web dashboard
      designed for administrators and support providers.
      It provides authenticated users with access to the
      functionality required to manage the DadaSafe platform.

    </p>

    <p>

      The dashboard is built using Next.js and communicates
      with the DadaSafe backend through REST API endpoints.

    </p>


    <div class="field-list">

      <strong>Primary users:</strong>

      Administrators and Support Providers

    </div>


    <h3>
      1.1 Administrators
    </h3>

    <p>

      Administrators use the dashboard to manage organisations,
      manage and monitor game content, manage user accounts,
      and review platform activity.

    </p>


    <h3>
      1.2 Support Providers
    </h3>

    <p>

      Support providers use the dashboard to access assigned
      support cases, review case information, and respond to
      support requests.

    </p>

  </section>

  <section>

    <hr class="divider">

    <h2>
      2. Technology Stack
    </h2>

    <p>

      The DadaSafe web frontend uses a modern JavaScript-based
      technology stack.

    </p>


    <div class="field-list">

      <strong>Framework:</strong>
      Next.js

      <br><br>

      <strong>UI Library:</strong>
      React

      <br><br>

      <strong>Programming Language:</strong>
      JavaScript / JSX

      <br><br>

      <strong>Styling:</strong>
      CSS

      <br><br>

      <strong>Backend Communication:</strong>
      REST API / FastAPI

      <br><br>

      <strong>Deployment:</strong>
      Vercel

    </div>


    <h3>
      2.1 Next.js
    </h3>

    <p>

      Next.js provides the application framework, routing,
      layouts, and frontend application structure.

    </p>


    <h3>
      2.2 React
    </h3>

    <p>

      React is used to build reusable user-interface components
      such as buttons, forms, dashboards, tables, charts,
      navigation elements, and game-related components.

    </p>


    <h3>
      2.3 JavaScript and JSX
    </h3>

    <p>

      JavaScript is used for application logic while JSX is
      used to define React components and user-interface
      structures.

    </p>


    <h3>
      2.4 FastAPI Backend
    </h3>

    <p>

      The frontend communicates with the DadaSafe FastAPI
      backend through HTTP requests.

    </p>


    <h3>
      2.5 Vercel
    </h3>

    <p>

      Vercel is used to deploy and host the Next.js frontend.

    </p>

  </section>

  <section>

    <hr class="divider">

    <h2>
      3. Prerequisites
    </h2>

    <p>

      Before running the DadaSafe frontend locally,
      ensure that the development environment contains
      the following:

    </p>


    <ul>

      <li>
        Node.js installed on the development machine.
      </li>

      <li>
        npm installed with Node.js.
      </li>

      <li>
        Git installed for repository management.
      </li>

      <li>
        Access to the DadaSafe frontend repository.
      </li>

      <li>
        Access to the DadaSafe backend API.
      </li>

      <li>
        Required environment variables configured.
      </li>

    </ul>


    <div class="note">

      <strong>Important:</strong>

      The backend API should be running and accessible
      before testing frontend features that require
      backend communication.

    </div>

  </section>

  <section>

    <hr class="divider">

    <h2>
      4. Setup and Installation
    </h2>

    <p>

      Follow the steps below to run the DadaSafe frontend
      locally.

    </p>


    <h3>
      4.1 Clone the Repository
    </h3>

    <pre class="code-block">git clone &lt;repository-url&gt;

cd SHE_Dashboard</pre>

    <h3>
      4.2 Install Dependencies
    </h3>

    <pre class="code-block">npm install</pre>


    <h3>
      4.3 Configure Environment Variables
    </h3>

    <p>

      Create a <code>.env.local</code> file in the project
      root and configure the backend API URL.

    </p>


    <pre class="code-block">NEXT_PUBLIC_FASTAPI_URL=http://127.0.0.1:8000</pre>


    <div class="note">

      Environment files should not be committed to the
      repository when they contain sensitive configuration
      values.

    </div>


    <h3>
      4.4 Start the Development Server
    </h3>

    <pre class="code-block">npm run dev</pre>


    <p>

      The application can then be accessed through the
      local development URL provided by Next.js.

    </p>


    <h3>
      4.5 Production Build
    </h3>

    <pre class="code-block">npm run build

npm start</pre>

  </section>

  <section>

    <hr class="divider">

    <h2>
      5. Project Structure
    </h2>

    <p>

      The DadaSafe web dashboard follows the Next.js
      App Router project structure.

    </p>


    <pre class="code-block folder-tree">SHE_Dashboard/

│
├── app/
│ │
│ ├── (auth)/
│ │ ├── forgot-password/
│ │ │ └── page.jsx
│ │ ├── login/
│ │ │ └── page.jsx
│ │ ├── signup/
│ │ └── layout.jsx
│ │
│ ├── admin/
│ │ ├── control/
│ │ │ ├── uploads/
│ │ │ │ └── page.jsx
│ │ │ ├── control.css
│ │ │ └── page.jsx
│ │ ├── dashboard/
│ │ │ └── page.jsx
│ │ ├── settings/
│ │ │ └── page.jsx
│ │ └── layout.jsx
│ │
│ ├── support/
│ │ ├── dashboard/
│ │ │ └── page.jsx
│ │ ├── profile/
│ │ │ └── page.jsx
│ │ ├── settings/
│ │ │ └── page.jsx
│ │ └── layout.jsx
│ │
│ ├── globals.css
│ ├── layout.jsx
│ ├── middleware.js
│ └── page.tsx
│
├── components/
│ ├── admin/
│ │ ├── GamePreview.jsx
│ │ ├── GameStatcard.jsx
│ │ ├── JsonEditor.jsx
│ │ ├── PromptBox.jsx
│ │ ├── RecentPrompts.jsx
│ │ ├── sidebar.jsx
│ │ ├── statCard.jsx
│ │ └── TopicSelector.jsx
│ │
│ ├── shared/
│ │ ├── Button.jsx
│ │ ├── Input.jsx
│ │ └── Logo.jsx
│ │
│ └── support/
│ ├── CasestatsChart.jsx
│ ├── CaseTable.jsx
│ └── Sidebar.jsx
│
├── hooks/
│ ├── useAuth.js
│ └── useRoleGuard.js
│
├── lib/
│ ├── api/
│ │ ├── auth.js
│ │ ├── avatars.js
│ │ ├── client.js
│ │ ├── game_files.js
│ │ ├── game_progress.js
│ │ ├── games.js
│ │ ├── organisations.js
│ │ ├── registration.js
│ │ └── support_requests.js
│ │
│ └── utils.js
│
└── public/
└── assets/</pre>

    <div class="note">

      The project separates authentication,
      administrator functionality, support-provider
      functionality, reusable components, API services,
      and authentication hooks.

    </div>

  </section>

  <section>

    <hr class="divider">

    <h2>
      6. Coding Standards
    </h2>

    <p>

      The frontend follows a component-based structure to
      keep the application modular and maintainable.

    </p>


    <h3>
      6.1 Component Structure
    </h3>

    <p>

      Components should have a single clear responsibility.
      Reusable components such as buttons, inputs, sidebars,
      tables, and cards should be placed in the appropriate
      component directories.

    </p>


    <h3>
      6.2 Naming Conventions
    </h3>

    <ul>

      <li>
        React components use PascalCase.
        Example: <code>GamePreview.jsx</code>
      </li>

      <li>
        Hooks use the <code>use</code> prefix.
        Example: <code>useAuth.js</code>
      </li>

      <li>
        API modules use descriptive lowercase names.
        Example: <code>support_requests.js</code>
      </li>

      <li>
        Variables and functions use camelCase.
      </li>

    </ul>


    <h3>
      6.3 Imports
    </h3>

    <p>

      Imports should be organized clearly and unused imports
      should be removed.

    </p>


    <h3>
      6.4 Reusable Components
    </h3>

    <p>

      Common interface elements should be implemented as
      reusable components instead of being duplicated across
      multiple pages.

    </p>

  </section>

  <section>

    <hr class="divider">

    <h2>
      7. Authentication Flow
    </h2>


    <h3>
      7.1 Login
    </h3>

    <p>

      Administrators and Support Providers authenticate
      through the login page using their email address
      and password.

    </p>


    <div class="field-list">

      <strong>Fields:</strong>

      Email, Password

    </div>


    <div class="label">
      API Endpoint
    </div>


    <div class="endpoint">

      <span class="method">POST</span>

      /auth/login

    </div>


    <div class="label">
      Sample Request
    </div>


    <pre class="code-block">{

<span class="k">"email"</span>: <span class="s">"admin@dadasafe.org"</span>,
<span class="k">"password"</span>: <span class="s">"Password123!"</span>
}</pre>

    <div class="label">

      Sample Response

      <span class="status-tag">
        200 OK
      </span>

    </div>


    <pre class="code-block">{

<span class="k">"access_token"</span>: <span class="s">"eyJhbGciOiJIUzI1NiIs..."</span>,
<span class="k">"token_type"</span>: <span class="s">"bearer"</span>
}</pre>

    <div class="note">

      Authenticated API requests use the returned access
      token as a Bearer token.

    </div>


    <h3>
      7.2 Role-Based Access
    </h3>

    <p>

      Access to dashboard functionality is controlled
      according to the authenticated user's role.

    </p>


    <ul>

      <li>
        <strong>Admin:</strong>
        access to administrator dashboard functionality.
      </li>

      <li>
        <strong>Support Provider:</strong>
        access to support dashboard functionality.
      </li>

    </ul>


    <h3>
      7.3 Authentication Helpers
    </h3>

    <p>

      Authentication-related functionality is organized
      through reusable hooks and API modules, including
      <code>useAuth.js</code> and the authentication API
      service.

    </p>

  </section>

  <section>

    <hr class="divider">

    <h2>
      8. API Integration
    </h2>

    <p>

      The frontend communicates with the DadaSafe backend
      using REST API endpoints.

    </p>


    <h3>
      8.1 API Client
    </h3>

    <p>

      API communication is centralized through the
      frontend API client and service modules.

    </p>


    <pre class="code-block">lib/

└── api/
├── auth.js
├── avatars.js
├── client.js
├── game_files.js
├── game_progress.js
├── games.js
├── organisations.js
├── registration.js
└── support_requests.js</pre>

    <h3>
      8.2 Authentication API
    </h3>

    <div class="endpoint">

      <span class="method">POST</span>
      /auth/login

    </div>


    <div class="endpoint">

      <span class="method">POST</span>
      /auth/register

    </div>


    <h3>
      8.3 User API
    </h3>

    <div class="endpoint">

      <span class="method">GET</span>
      /users/me

    </div>


    <div class="endpoint">

      <span class="method">PATCH</span>
      /users/me

    </div>


    <div class="endpoint">

      <span class="method">DELETE</span>
      /users/me

    </div>


    <h3>
      8.4 API Service Organization
    </h3>

    <p>

      API operations are separated into service files so
      that pages and components do not need to contain
      repeated API request logic.

    </p>

  </section>

  <section>

    <hr class="divider">

    <h2>
      9. Pages and Features
    </h2>


    <h3>
      9.1 Authentication Pages
    </h3>

    <ul>

      <li>
        <strong>Login:</strong>
        Allows Admins and Support Providers to authenticate.
      </li>

      <li>
        <strong>Signup:</strong>
        Allows creation of supported dashboard accounts.
      </li>

      <li>
        <strong>Forgot Password:</strong>
        Provides the password recovery flow.
      </li>

    </ul>


    <h3>
      9.2 Admin Dashboard
    </h3>

    <p>

      The Admin dashboard provides administrative functionality
      for managing and monitoring the DadaSafe platform.

    </p>


    <ul>

      <li>
        Dashboard overview and statistics.
      </li>

      <li>
        Organisation management.
      </li>

      <li>
        Game and educational content management.
      </li>

      <li>
        Game file uploads and management.
      </li>

      <li>
        Administrative settings.
      </li>

    </ul>


    <h3>
      9.3 Support Provider Dashboard
    </h3>

    <p>

      The Support Provider dashboard is designed for users
      responsible for handling support requests.

    </p>


    <ul>

      <li>
        View support cases.
      </li>

      <li>
        Review case statistics.
      </li>

      <li>
        Access case information.
      </li>

      <li>
        Respond to assigned support requests.
      </li>

      <li>
        Manage support-provider profile and settings.
      </li>

    </ul>


    <h3>
      9.4 Role Protection
    </h3>

    <p>

      Protected routes use authentication and role-checking
      mechanisms to ensure that users only access functionality
      appropriate to their role.

    </p>

  </section>

  <section>

    <hr class="divider">

    <h2>
      10. Styling and Design
    </h2>

    <p>

      The DadaSafe frontend uses CSS to provide a consistent
      visual design across the dashboard.

    </p>


    <h3>
      10.1 Design Tokens
    </h3>

    <p>

      Shared colours and interface values are defined using
      CSS variables.

    </p>


    <pre class="code-block">:root {

--pink: #E6007C;
--pink-dark: #B8005F;
--pink-light: #FDE6F2;
--header-purple: #6B2D8E;
--purple-light: #F3E5F5;
--ink: #22202A;
--muted: #6B6673;
}</pre>

    <h3>
      10.2 Responsive Design
    </h3>

    <p>

      Responsive CSS rules are used to adapt the dashboard
      layout for smaller screens.

    </p>


    <h3>
      10.3 Shared UI Components
    </h3>

    <p>

      Shared components such as buttons, inputs, logos,
      navigation elements, tables, and dashboard cards
      are reused throughout the application.

    </p>

  </section>

  <section>

    <hr class="divider">

    <h2>
      11. Error Handling
    </h2>

    <p>

      The frontend handles errors from both API requests
      and user-interface operations.

    </p>


    <h3>
      11.1 API Errors
    </h3>

    <p>

      API responses are checked for unsuccessful HTTP status
      codes. Appropriate error messages can then be displayed
      to the user.

    </p>


    <h3>
      11.2 Authentication Errors
    </h3>

    <p>

      Authentication failures prevent users from accessing
      protected dashboard functionality.

    </p>


    <h3>
      11.3 Permission Errors
    </h3>

    <p>

      Users attempting to access functionality outside their
      assigned role should receive an appropriate access
      restriction.

    </p>


    <h3>
      11.4 User Interface Errors
    </h3>

    <p>

      Forms and dashboard components should provide clear
      feedback when an operation fails, allowing users to
      understand what went wrong and what action to take.

    </p>


    <div class="note">

      <strong>Example:</strong>

      An inactive account may receive a
      <strong>403 — Account is not active</strong>
      response when attempting to access protected resources.

    </div>

  </section>

<section>

  <hr class="divider">

  <h2>12. Deployment</h2>

  <p>
    The DadaSafe web frontend is deployed using Vercel. The production
    application is automatically built and hosted from the GitHub repository,
    providing secure and reliable access for Administrators and Support Providers.
  </p>

  <h3>12.1 Production Build</h3>

  <pre class="code-block">npm run build</pre>

  <h3>12.2 Start Production Server</h3>

  <pre class="code-block">npm start</pre>

  <h3>12.3 Vercel Deployment</h3>

  <p>
    The project is connected to a Vercel project. Every production deployment
    is automatically built from the Next.js application and published online.
  </p>

  <h3>12.4 Environment Variables & Live Links</h3>

  <p>
    During development, the frontend communicates with the local FastAPI backend.
    In production, users access the deployed dashboard and the public
    informational website hosted on Vercel.
  </p>

  <div class="note">

    <strong>Development Backend API</strong><br>
    <code>http://127.0.0.1:8000</code>

    <br><br>

    <strong>Production Web Dashboard</strong><br>
    <a
      href="https://she-dashboard-alpha.vercel.app"
      target="_blank"
      rel="noopener noreferrer"
      class="vercel-link"
    >
      https://she-dashboard-alpha.vercel.app ↗
    </a>

    <br><br>

    <strong>DadaSafe Informational Website</strong><br>
    <a
      href="https://sheinformationalwebsite-sable.vercel.app/"
      target="_blank"
      rel="noopener noreferrer"
      class="vercel-link"
    >
      https://sheinformationalwebsite-sable.vercel.app ↗
    </a>

  </div>

</section>

  <section>

    <hr class="divider">

    <h2>
      13. Summary
    </h2>

    <p>

      The DadaSafe web frontend is a Next.js-based dashboard
      designed specifically for Administrators and Support
      Providers. It provides authentication, role-based
      access, administrative functionality, support-case
      management, API integration, responsive styling, and
      deployment through Vercel.

    </p>

  </section>

</main>

</body>

</html>
