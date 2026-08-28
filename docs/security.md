# dadaSafe API Security

## API Security Policy

| Item                 | Details                     |
| -------------------- | --------------------------- |
| **System**           | dadaSafe Digital Safety API |
| **API Framework**    | FastAPI                     |
| **Database**         | PostgreSQL                  |
| **ORM**              | SQLAlchemy                  |
| **Authentication**   | JWT                         |
| **Password Hashing** | bcrypt                      |
| **MFA**              | TOTP / PyOTP                |
| **Rate Limiting**    | SlowAPI where configured    |
| **Deployment**       | Heroku                      |
| **Security Model**   | Defense-in-depth + STRIDE   |

---

## 1. Purpose

This document defines the security controls and policies implemented or applicable to the dadaSafe API.

The purpose of these controls is to protect API endpoints, user authentication, authorization, application data, database access, external service integrations, and sensitive application configuration from unauthorized access, modification, disclosure, and abuse.

Controls that are not currently implemented are not presented as implemented.

---

## 2. API Scope

The security controls in this document apply to the dadaSafe backend API and its associated services, including:

* Authentication endpoints
* Player functionality
* Support Provider functionality
* Admin functionality
* Games endpoints
* Organisation endpoints
* PostgreSQL database access
* JWT authentication
* Password and credential handling
* MFA/TOTP functionality
* Password-reset functionality
* Resend email integration
* Gemini/Google GenAI integration
* API rate limiting where configured
* Application logging and monitoring
* Environment variables and application secrets

---

# 3. API Security Architecture

The dadaSafe API follows a defense-in-depth architecture:

```text
Client / PWA
     |
     | HTTPS
     v
+----------------------+
|    dadaSafe API      |
|       FastAPI        |
+----------------------+
     |
     +--------------------+
     |                    |
     v                    v
 Authentication       Authorization
     |                    |
     +---------+----------+
               |
               v
       Application Services
               |
       +-------+-------+
       |               |
       v               v
 PostgreSQL       External APIs
 Database         Resend / Gemini
```

The API acts as the trusted backend layer between clients, application logic, the database, and approved external services.

---

# 4. Authentication Security

## 4.1 Email and Password Authentication

The API uses email and password authentication for user login.

Security controls include:

* Passwords are never stored in plaintext.
* Passwords are hashed using bcrypt.
* Authentication credentials are verified by the backend.
* Invalid credentials are rejected.
* Protected API endpoints require authentication.
* Authentication failures must not expose sensitive information.

---

## 4.2 JWT Authentication

JWTs are used to authenticate API requests after successful login.

The API security policy requires:

* JWTs to be cryptographically signed.
* JWT expiry to be enforced according to the configured token lifetime.
* Invalid JWTs to be rejected.
* Expired JWTs to be rejected.
* Malformed JWTs to be rejected.
* Incorrectly signed JWTs to be rejected.
* JWT signing secrets to remain outside source code.
* JWT payloads not to contain passwords, MFA secrets, encryption keys, or other sensitive secrets.

JWTs are used to establish the authenticated identity of the API requester.

---

# 5. Authorization and RBAC

The API uses role-based access control (RBAC).

Supported roles are:

* **Player**
* **Support Provider**
* **Admin**

Authorization controls ensure that authentication alone does not automatically provide access to every API operation.

### Player

Players are restricted to permitted player functionality and their own relevant game and progress information.

### Support Provider

Support Providers are restricted to authorized support-provider functionality and records.

### Admin

Administrative functionality is restricted to authorized Admin users.

Administrative endpoints must verify that the authenticated user has the required administrative role.

---

# 6. API Endpoint Protection

Protected API endpoints require an authenticated request.

Security controls include:

* Authentication dependencies on protected endpoints.
* Role-based authorization where required.
* Validation of authenticated user identity.
* Rejection of requests with missing or invalid authentication credentials.
* Restriction of administrative operations to authorized Admin users.
* Restriction of user-specific resources to the appropriate authenticated user.

API authorization must be enforced on the server side and must not rely solely on client-side restrictions.

---

# 7. Input Validation

The API uses FastAPI and Pydantic for request validation.

Input validation is applied to:

* Request bodies
* Query parameters
* Path parameters
* API payloads
* User-provided application data

Security controls include:

* Invalid request data is rejected.
* Query parameters are validated according to their expected types and constraints.
* Malformed requests are rejected before reaching application logic where validation applies.
* External-service inputs are validated before processing where applicable.

Example API validation:

```python
skip: int = Query(0, ge=0)
limit: int = Query(100, ge=1, le=1000)
```

This prevents invalid values from being accepted for parameters with defined constraints.

---

# 8. SQL Injection Protection

The API uses SQLAlchemy for database interaction with PostgreSQL.

Security controls include:

* Database operations are performed through SQLAlchemy.
* Parameterized database operations are used instead of constructing SQL queries from raw user input.
* User-provided values must not be directly concatenated into SQL statements.
* Database access is performed through the application's database layer.

SQL injection prevention is treated as an application-level responsibility.

---

# 9. Database Security

The dadaSafe API uses PostgreSQL.

Database security controls include:

* Database credentials are stored outside application source code.
* Database connection information is provided through protected application configuration.
* Database access is restricted to the application and authorized administrators.
* Database operations are performed through SQLAlchemy.
* Sensitive credentials must not be exposed through API responses or logs.

The application does not claim a separate PostgreSQL backup database at this time.

---

# 10. Backup and Recovery Status

The dadaSafe API currently does **not** have a separate PostgreSQL backup database or an implemented backup-and-recovery system.

Therefore, backup and disaster-recovery controls are **not claimed as implemented**.

Current limitations:

* No separate backup PostgreSQL database is currently maintained.
* No automated backup process is currently managed by the dadaSafe project.
* No implemented database restoration procedure is currently maintained.
* No point-in-time recovery process is currently claimed.

This is an identified operational and security gap.

---

# 11. Password Security

Passwords are protected using bcrypt hashing.

The API security policy requires:

* Passwords must never be stored in plaintext.
* Password hashes must be stored instead of plaintext passwords.
* Passwords must not be returned in API responses.
* Passwords must not be stored in JWT payloads.
* Passwords must not be written to application logs.
* Password-reset operations must use a secure reset mechanism.

---

# 12. Multi-Factor Authentication

The API supports TOTP-based MFA using PyOTP where MFA is enabled or required by the application flow.

Security controls include:

* MFA verification before access to resources protected by the MFA flow.
* MFA secrets are not stored in JWT payloads.
* MFA secrets must not be exposed through API responses.
* Invalid MFA verification attempts are rejected.
* MFA secrets must not be written to application logs.

---

# 13. Password Reset Security

Password-reset functionality is handled by the backend with email delivery through the configured email service.

Security requirements include:

* Reset tokens or links must be validated by the backend.
* Invalid or expired reset tokens must be rejected.
* Password-reset secrets must not be written to application logs.
* Reset responses should not unnecessarily disclose whether sensitive account information exists.
* New passwords must be hashed using bcrypt before storage.
* Reset links or tokens must not expose unnecessary user information.

---

# 14. Rate Limiting and Abuse Protection

The project includes **SlowAPI** as a rate-limiting dependency.

Rate limiting is intended to reduce automated abuse against sensitive API endpoints.

However:

> SlowAPI is only considered an active security control on endpoints where rate limiting has actually been configured.

The project does not claim to operate a separate WAF or dedicated DDoS protection service.

Rate limiting should be considered particularly relevant to:

* Login endpoints
* Password-reset endpoints
* MFA verification endpoints
* Other sensitive or abuse-prone endpoints

---

# 15. HTTPS and Transport Security

Production communication with the dadaSafe API is performed over HTTPS.

HTTPS protects data transmitted between the client and API, including:

* Passwords during authentication
* JWTs
* MFA-related information
* User information
* Game information
* Support-related information
* API requests and responses

Sensitive application data must not intentionally be transmitted over plaintext HTTP in production.

External API integrations must use secure transport supported by the relevant service.

---

# 16. Secrets Management

Sensitive application configuration must not be hardcoded into the API source code.

Protected configuration includes:

* JWT signing secrets
* PostgreSQL database credentials
* Resend API credentials
* Gemini API credentials
* Encryption keys
* Other application secrets

These values should be provided through environment variables or protected application configuration.

Secrets must:

* Not be committed to GitHub.
* Not be hardcoded into source files.
* Not be returned in API responses.
* Not be placed in JWT payloads.
* Not be written to application logs.

The `.env` file containing local secrets must not be committed to the repository.

---

# 17. External Service Security

## 17.1 Resend

Resend is used as an external email delivery service.

It is responsible for sending application emails such as:

* Account-related emails
* Password-reset emails
* Other application notifications where configured

Resend is an **email delivery service** and is not treated as a password-reset function or resend cooldown mechanism.

Resend credentials must be stored as protected application secrets.

Sensitive information should not unnecessarily be included in email content.

---

## 17.2 Gemini / Google GenAI

The API uses Google GenAI/Gemini for AI-related functionality.

Security controls include:

* Only information required for the AI task should be sent to the external service.
* Passwords must not be sent to the AI service.
* MFA secrets must not be sent to the AI service.
* JWT signing secrets must not be sent to the AI service.
* Database credentials must not be sent to the AI service.
* Application encryption keys must not be sent to the AI service.
* AI-generated content is subject to the application's validation and approval workflow where applicable.
* Gemini credentials must be protected as application secrets.

---

# 18. API Error Handling

API errors should not expose sensitive implementation details.

Error responses should avoid exposing:

* Database credentials
* JWT signing secrets
* API keys
* Passwords
* MFA secrets
* Reset tokens
* Internal authentication information
* Sensitive database information

The API should return appropriate HTTP status codes for authentication, authorization, validation, and server errors.

---

# 19. Logging and Monitoring

The project includes application logging and Sentry SDK support for error monitoring.

Security-relevant events should be logged where application logging is implemented.

Relevant events include:

* Authentication failures
* Authorization failures
* Administrative actions
* Security-sensitive application actions
* Application errors

Logs must not intentionally contain:

* Passwords
* Password hashes where unnecessary
* MFA secrets
* JWT signing secrets
* Database credentials
* API keys
* Password-reset tokens

---

# 20. STRIDE Threat Model

| STRIDE Threat              | API Risk                                                                                                    | Relevant Security Controls                                                          |
| -------------------------- | ----------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| **Spoofing**               | An attacker impersonates a Player, Support Provider, or Admin.                                              | Email/password authentication, bcrypt, JWT authentication, MFA/TOTP where enabled.  |
| **Tampering**              | An attacker modifies games, user information, support information, or other API data without authorization. | JWT authentication, RBAC, input validation, SQLAlchemy, authorization checks.       |
| **Repudiation**            | A user denies performing a security-sensitive API operation.                                                | Authenticated user identity and application/audit logging where implemented.        |
| **Information Disclosure** | Sensitive API, user, authentication, or database information is exposed.                                    | HTTPS, RBAC, protected secrets, bcrypt, input validation, controlled API responses. |
| **Denial of Service**      | Excessive requests make sensitive API endpoints unavailable.                                                | SlowAPI rate limiting where configured and Heroku hosting infrastructure.           |
| **Elevation of Privilege** | A Player or Support Provider gains Admin-level API access.                                                  | RBAC, authentication dependencies, authorization checks, least privilege.           |

---

# 21. Security Incident Response

If an API security incident is suspected:

1. Identify and record the incident.
2. Determine the affected API endpoints, accounts, services, or data.
3. Contain compromised accounts, tokens, credentials, or services.
4. Revoke or rotate compromised credentials and secrets.
5. Preserve relevant logs where available.
6. Assess affected users and data.
7. Restore affected services using trusted deployment procedures.
8. Document the root cause and corrective actions.
9. Review the affected security controls.
10. Update this security policy if required.

---

# 22. Current API Security Gaps

The following limitations are intentionally documented:

* No separate PostgreSQL backup database is currently maintained.
* No implemented automated database backup and recovery system is currently maintained by the project.
* No implemented database restoration procedure is currently claimed.
* No point-in-time recovery process is currently claimed.
* No separate dadaSafe-managed WAF is claimed.
* No dedicated dadaSafe-managed DDoS protection service is claimed.
* SlowAPI rate limiting is only considered active on endpoints where it has actually been configured.
* External services such as Resend and Gemini are third-party services and are not treated as part of the dadaSafe API's internal trusted zone.

---

# 23. Security-Relevant Dependencies

| Dependency        | Version | Security Role                      |
| ----------------- | ------: | ---------------------------------- |
| FastAPI           | 0.141.1 | API framework and request handling |
| Pydantic          |  2.13.4 | Input and data validation          |
| pydantic-settings |  2.14.2 | Application configuration          |
| SQLAlchemy        |  2.0.51 | Database access layer              |
| psycopg2-binary   |  2.9.12 | PostgreSQL connectivity            |
| bcrypt            |   4.0.1 | Password hashing                   |
| PyJWT             |  2.13.0 | JWT functionality                  |
| python-jose       |   3.5.0 | JWT-related functionality          |
| PyOTP             |  2.10.0 | TOTP/MFA                           |
| cryptography      |  50.0.0 | Cryptographic operations           |
| fastapi-mail      |   1.6.8 | Email integration                  |
| aiosmtplib        |   5.1.2 | Email delivery support             |
| google-genai      |  2.18.0 | Gemini/Google GenAI integration    |
| slowapi           |   0.1.9 | API rate limiting where configured |
| sentry-sdk        |  2.66.1 | Application error monitoring       |
| python-dotenv     |   1.2.2 | Environment configuration          |

---

# 24. Security Review and Policy Enforcement

The API security controls must be reviewed whenever there are significant changes to:

* Authentication
* JWT configuration
* Authorization/RBAC
* Database architecture
* API endpoints
* Sensitive data flows
* External services
* Application secrets
* Deployment infrastructure
* Cryptographic mechanisms

Significant changes should be assessed against the STRIDE threat model before release.

This document must be updated when security controls are:

* Added
* Removed
* Changed
* Reconfigured
* Replaced

---

## Document Status

**System:** dadaSafeAPI
**Document:** API Security Policy
**Document Link:** https://docs.google.com/document/d/1SFBD4YkUAL_HoLn-PW54bCzgP_rPfN4cAEwgMw0T3BI/edit?usp=sharing
**Security Model:** Defense-in-depth + STRIDE threat modeling
**Deployment:** Heroku
**Status:** Current API Security Policy

---


