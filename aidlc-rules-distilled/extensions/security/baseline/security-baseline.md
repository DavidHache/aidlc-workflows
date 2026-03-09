## BASELINE_SECURITY_RULES

Cross-cutting [REQ] constraints across all AI-DLC phases. Not optional guidance — hard constraints stages MUST enforce when generating questions, producing design artifacts, generating code, presenting completion messages.

Enforcement: at each applicable stage, model MUST verify compliance before presenting stage completion message.

### BLOCKING_SECURITY_FINDING_BEHAVIOR

Blocking finding means:
1. Finding MUST be listed in stage completion message under "Security Findings" section with SECURITY rule ID and description
2. Stage MUST NOT present "Continue to Next Stage" option until all blocking findings resolved
3. Model MUST present only "Request Changes" option with clear explanation of what needs to change
4. Finding MUST be logged in `aidlc-docs/audit.md` with SECURITY rule ID, description, stage context

[COND] SECURITY rule not applicable to current project (e.g., SECURITY-01 when no data stores exist) → mark N/A in compliance summary — not a blocking finding.

Default: all rules blocking. Verification criteria not met → blocking security finding → follow blocking finding behavior above.

Verification criteria format: plain bullet points describing compliance checks, distinct from `- [ ]` / `- [x]` progress-tracking checkboxes. Evaluate each as compliant or non-compliant during review.

## APPLICABILITY_QUESTION

Auto-included in Requirements Analysis clarifying questions when this extension is loaded:

```markdown
## Question: Security Extensions
Should security extension rules be enforced for this project?

A) Yes — enforce all SECURITY rules as blocking constraints (recommended for production-grade applications)
B) No — skip all SECURITY rules (suitable for PoCs, prototypes, and experimental projects)
X) Other (please describe after [Answer]: tag below)

[Answer]: 
```

## SECURITY-01: ENCRYPTION_AT_REST_AND_IN_TRANSIT

Rule: every data persistence store (databases, object storage, file systems, caches, or equivalent) MUST have:
- Encryption at rest enabled using managed key service or customer-managed keys
- Encryption in transit enforced (TLS 1.2+ for all data movement in/out of store)

Verification:
- No storage resource defined without encryption configuration block
- No database connection string uses unencrypted protocol
- Object storage enforces encryption at rest and rejects non-TLS requests via policy
- Database instances have storage encryption enabled and enforce TLS connections

## SECURITY-02: ACCESS_LOGGING_ON_NETWORK_INTERMEDIARIES

Rule: every network-facing intermediary handling external traffic MUST have access logging enabled:
- Load balancers → access logs to persistent store
- API gateways → execution logging and access logging to centralized log service
- CDN distributions → standard logging or real-time logs

Verification:
- No load balancer resource defined without access logging enabled
- No API gateway stage defined without access logging configured
- No CDN distribution defined without logging configuration

## SECURITY-03: APPLICATION_LEVEL_LOGGING

Rule: every deployed application component MUST include structured logging infrastructure:
- Logging framework MUST be configured
- Log output MUST be directed to centralized log service
- Logs MUST include: timestamp, correlation/request ID, log level, message
- Sensitive data (passwords, tokens, PII) MUST NOT appear in log output

Verification:
- Every service/function entry point includes configured logger
- No ad-hoc logging as primary logging mechanism in production code
- Log configuration routes output to centralized log service
- No secrets, tokens, or PII logged

## SECURITY-04: HTTP_SECURITY_HEADERS

Rule: following HTTP response headers MUST be set on all HTML-serving endpoints:

| Header | Required Value |
|---|---|
| `Content-Security-Policy` | Define restrictive policy (at minimum: `default-src 'self'`) |
| `Strict-Transport-Security` | `max-age=31536000; includeSubDomains` |
| `X-Content-Type-Options` | `nosniff` |
| `X-Frame-Options` | `DENY` (or `SAMEORIGIN` if framing required) |
| `Referrer-Policy` | `strict-origin-when-cross-origin` |

`X-XSS-Protection` deprecated in modern browsers. Use `Content-Security-Policy` instead.

Verification:
- Middleware or response interceptor sets all required headers
- CSP policy does not use `unsafe-inline` or `unsafe-eval` without documented justification
- HSTS max-age at least 31536000 (1 year)

## SECURITY-05: INPUT_VALIDATION

Rule: every API endpoint (REST, GraphQL, gRPC, WebSocket) MUST validate all input parameters before processing:
- Type checking: reject unexpected types
- Length/size bounds: enforce maximum lengths on strings, maximum sizes on arrays and payloads
- Format validation: use allowlists (regex or schema) for structured inputs (emails, dates, IDs)
- Sanitization: escape or reject HTML/script content in user-supplied strings to prevent XSS
- Injection prevention: use parameterized queries for all database operations (never string concatenation)

Verification:
- Every API handler uses validation library or schema
- No raw user input concatenated into SQL, NoSQL, or OS commands
- String inputs have explicit max-length constraints
- Request body size limits configured at framework or gateway level

## SECURITY-06: LEAST_PRIVILEGE_ACCESS

Rule: every IAM policy, role, or permission boundary MUST follow least privilege:
- Use specific resource identifiers — NEVER wildcard resources unless API does not support resource-level permissions (document exception)
- Use specific actions — NEVER wildcard actions
- Scope conditions where possible
- Separate read and write permissions into distinct policy statements

Verification:
- No policy contains wildcard actions or wildcard resources without documented exception
- No service role has broader permissions than what service actually calls
- Inline policies avoided in favor of managed policies where possible
- Every role has trust policy scoped to specific service or account

## SECURITY-07: RESTRICTIVE_NETWORK_CONFIG

Rule: all network configurations (security groups, network ACLs, route tables) MUST follow deny-by-default:
- Firewall rules: only open specific ports required by application
- No inbound rule with source `0.0.0.0/0` except for public-facing load balancers on ports 80/443
- No outbound rule with `0.0.0.0/0` on all ports unless explicitly justified
- Private subnets MUST NOT have direct internet gateway routes
- Use private endpoints for cloud service access where available

Verification:
- No firewall rule allows inbound `0.0.0.0/0` on any port other than 80/443 on public load balancer
- Database and application firewall rules restrict source to specific CIDR blocks or security group references
- Private subnets route through NAT gateway (not internet gateway)
- Private endpoints used for high-traffic cloud service calls

## SECURITY-08: APPLICATION_LEVEL_ACCESS_CONTROL

Rule: every application endpoint accessing or mutating a resource MUST enforce authorization at application layer:
- Deny by default: all routes/endpoints MUST require authentication unless explicitly marked public
- Object-level authorization: every request referencing resource by ID MUST verify requesting user/principal owns or has permission (prevent IDOR)
- Function-level authorization: admin/privileged operations MUST check caller role/permissions server-side — never rely on client-side hiding
- CORS policy: CORS MUST be restricted to explicitly allowed origins — never `Access-Control-Allow-Origin: *` on authenticated endpoints
- Token validation: JWTs or session tokens MUST be validated server-side on every request (signature, expiration, audience, issuer)

Verification:
- Every controller/handler has authorization middleware or guard applied
- No endpoint returns data for resource ID without verifying caller ownership or permission
- Admin/privileged routes have explicit role checks enforced server-side
- CORS configuration does not use wildcard origins on authenticated endpoints
- Token validation occurs server-side on every request (not just at login)

## SECURITY-09: HARDENING_AND_MISCONFIGURATION_PREVENTION

Rule: all deployed components MUST follow hardening baseline:
- No default credentials: default usernames/passwords MUST be changed or disabled before deployment
- Minimal installation: remove or disable unused features, frameworks, sample applications, documentation endpoints
- Error handling: production error responses MUST NOT expose stack traces, internal paths, framework versions, database details to end users
- Directory listing: web servers MUST disable directory listing
- Cloud storage: cloud object storage MUST block public access unless explicitly required and documented
- Patch management: runtime environments, frameworks, OS images MUST use current supported versions

Verification:
- No default credentials in configuration files, environment variables, or IaC templates
- Error responses in production return generic messages (no stack traces or internal details)
- Cloud object storage has public access blocked unless documented exception exists
- No sample/demo applications or default pages deployed
- Framework and runtime versions current and supported

## SECURITY-10: SOFTWARE_SUPPLY_CHAIN

Rule: every project MUST manage software supply chain:
- Dependency pinning: all dependencies MUST use exact versions or lock files
- Vulnerability scanning: dependency vulnerability scanner MUST be configured
- No unused dependencies: remove packages not actively used
- Trusted sources only: dependencies MUST be pulled from official registries or verified private registries — no unvetted third-party sources
- SBOM: projects MUST generate Software Bill of Materials for production deployments
- CI/CD integrity: build pipelines MUST use pinned tool versions and verified base images — no `latest` tags in production Dockerfiles or CI configurations

Verification:
- Lock file exists and committed to version control
- Dependency vulnerability scanning step included in CI/CD or documented in build instructions
- No unused or abandoned dependencies included
- Dockerfiles and CI configs do not use `latest` or unpinned image tags for production
- Dependencies sourced from official or verified registries

## SECURITY-11: SECURE_DESIGN_PRINCIPLES

Rule: application design MUST incorporate security from start:
- Separation of concerns: security-critical logic (authentication, authorization, payment processing) MUST be isolated in dedicated modules — not scattered across codebase
- Defense in depth: no single control as sole line of defense — layer controls (validation + authorization + encryption)
- Rate limiting: public-facing endpoints MUST implement rate limiting or throttling to prevent abuse
- Business logic abuse: design MUST consider misuse cases — not just happy-path scenarios

Verification:
- Security-critical logic encapsulated in dedicated modules or services
- Rate limiting configured on public-facing APIs
- Design documentation addresses at least one misuse/abuse scenario

## SECURITY-12: AUTHENTICATION_AND_CREDENTIAL_MANAGEMENT

Rule: every application with user authentication MUST implement:
- Password policy: minimum 8 characters, check against breached password lists
- Credential storage: passwords MUST be hashed using adaptive algorithms — never weak or non-adaptive hashing
- Multi-factor authentication: MFA MUST be supported for admin accounts, SHOULD be available for all users
- Session management: sessions MUST have server-side expiration, be invalidated on logout, use secure/httpOnly/sameSite cookie attributes
- Brute-force protection: login endpoints MUST implement account lockout, progressive delays, or CAPTCHA after repeated failures
- No hardcoded credentials: no passwords, API keys, or secrets in source code or IaC templates — use secrets manager

Verification:
- Password hashing uses adaptive algorithms (not weak or non-adaptive hashing)
- Session cookies set `Secure`, `HttpOnly`, `SameSite` attributes
- Login endpoints have brute-force protection (lockout, delay, or CAPTCHA)
- No hardcoded credentials in source code or configuration files
- MFA supported for admin accounts
- Sessions invalidated on logout with defined expiration

## SECURITY-13: SOFTWARE_AND_DATA_INTEGRITY

Rule: systems MUST verify integrity of software and data:
- Deserialization safety: untrusted data MUST NOT be deserialized without validation — use safe deserialization libraries or allowlists of permitted types
- Artifact integrity: downloaded dependencies, plugins, updates MUST be verified via checksums or digital signatures
- CI/CD pipeline security: build pipelines MUST restrict who can modify pipeline definitions — separate duties between code authors and deployment approvers
- CDN and external resources: scripts or resources loaded from external CDNs MUST use Subresource Integrity (SRI) hashes
- Data integrity: critical data modifications MUST be auditable (who changed what, when)

Verification:
- No unsafe deserialization of untrusted input
- External scripts include SRI integrity attributes when loaded from CDNs
- CI/CD pipeline definitions access-controlled and changes auditable
- Critical data changes logged with actor, timestamp, before/after values

## SECURITY-14: ALERTING_AND_MONITORING

Rule: in addition to logging (SECURITY-02, SECURITY-03), systems MUST include:
- Security event alerting: alerts MUST be configured for high-value security events: repeated authentication failures, privilege escalation attempts, access from unusual locations, authorization failures
- Log integrity: logs MUST be stored in append-only or tamper-evident storage — application code MUST NOT delete or modify its own audit logs
- Log retention: logs MUST be retained for minimum period appropriate to compliance requirements (default: 90 days minimum)
- Monitoring dashboards: monitoring dashboard or alarm configuration MUST be defined for key operational and security metrics

Verification:
- Alerting configured for authentication failures and authorization violations
- Application log groups have retention policies set (minimum 90 days)
- Application roles do not have permission to delete their own log groups/streams
- Security-relevant events (login failures, access denied, privilege changes) generate alerts

## SECURITY-15: EXCEPTION_HANDLING_AND_FAIL_SAFE

Rule: every application MUST handle exceptional conditions safely:
- Catch and handle: all external calls (database, API, file I/O) MUST have explicit error handling — no unhandled promise rejections or uncaught exceptions in production
- Fail closed: on error, system MUST deny access or halt operation — never fail open
- Resource cleanup: error paths MUST release resources (connections, file handles, locks) — use try/finally, using statements, or equivalent
- User-facing errors: error messages shown to users MUST be generic — no internal details or system information
- Global error handler: applications MUST have global/top-level error handler that catches unhandled exceptions, logs them (per SECURITY-03), returns safe response

Verification:
- All external calls (DB, HTTP, file I/O) have explicit error handling (try/catch, .catch(), error callbacks)
- Global error handler configured at application entry point
- Error paths do not bypass authorization or validation checks (fail closed)
- Resources cleaned up in error paths (connections closed, transactions rolled back)
- No unhandled promise rejections or uncaught exception warnings in application code

## ENFORCEMENT_INTEGRATION

Cross-cutting constraints applying to every AI-DLC stage. At each stage:
- Evaluate all SECURITY rule verification criteria against artifacts produced
- Include "Security Compliance" section in stage completion summary listing each rule as compliant, non-compliant, or N/A
- [COND] any rule non-compliant → blocking security finding → follow blocking finding behavior
- Include security rule references in design documentation and test instructions

## OWASP_REFERENCE_MAPPING

| SECURITY Rule | OWASP Category |
|---|---|
| SECURITY-08 | A01:2025 - Broken Access Control |
| SECURITY-09 | A02:2025 - Security Misconfiguration |
| SECURITY-10 | A03:2025 - Software Supply Chain Failures |
| SECURITY-11 | A06:2025 - Insecure Design |
| SECURITY-12 | A07:2025 - Authentication Failures |
| SECURITY-13 | A08:2025 - Software or Data Integrity Failures |
| SECURITY-14 | A09:2025 - Logging & Alerting Failures |
| SECURITY-15 | A10:2025 - Mishandling of Exceptional Conditions |
