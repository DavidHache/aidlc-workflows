# BASELINE SECURITY RULES

## OVERVIEW

Cross-cutting MANDATORY constraints across all AI-DLC phases. Hard constraints, not guidance.

Enforcement: verify compliance before presenting stage completion message.

Blocking finding behavior:
1. List finding in stage completion under "Security Findings" with SECURITY rule ID + description
2. [REQ] DO NOT present "Continue to Next Stage" until all blocking findings resolved
3. Present only "Request Changes" with explanation
4. Log in `aidlc-docs/audit.md` with rule ID, description, stage context

N/A rule (e.g., SECURITY-01 when no data stores) → mark N/A in compliance summary, not blocking.

All rules blocking by default. Verification items are compliance checks (not `- [ ]` checkboxes).

## SECURITY-01: Encryption at Rest and in Transit

Rule: every data store (DB, object storage, file systems, caches) MUST have encryption at rest (managed/customer keys) + encryption in transit (TLS 1.2+).

Verification: no storage without encryption config; no unencrypted DB connection strings; object storage enforces encryption + rejects non-TLS; DB instances have storage encryption + enforce TLS.

## SECURITY-02: Access Logging on Network Intermediaries

Rule: every network-facing intermediary handling external traffic MUST have access logging. Load balancers → access logs; API gateways → execution + access logging; CDN → standard/real-time logs.

Verification: no LB without access logging; no API gateway stage without access logging; no CDN without logging config.

## SECURITY-03: Application-Level Logging

Rule: every deployed component MUST have structured logging: configured framework, output to centralized log service, include timestamp + correlation/request ID + log level + message. Sensitive data (passwords, tokens, PII) MUST NOT appear in logs.

Verification: every entry point has configured logger; no ad-hoc logging as primary mechanism; log config routes to centralized service; no secrets/tokens/PII logged.

## SECURITY-04: HTTP Security Headers for Web Applications

Rule: required headers on all HTML-serving endpoints:

| Header | Required Value |
|---|---|
| `Content-Security-Policy` | Restrictive policy (min: `default-src 'self'`) |
| `Strict-Transport-Security` | `max-age=31536000; includeSubDomains` |
| `X-Content-Type-Options` | `nosniff` |
| `X-Frame-Options` | `DENY` (or `SAMEORIGIN` if framing required) |
| `Referrer-Policy` | `strict-origin-when-cross-origin` |

`X-XSS-Protection` deprecated; use CSP instead.

Verification: middleware/interceptor sets all headers; CSP no `unsafe-inline`/`unsafe-eval` without documented justification; HSTS max-age >= 31536000.

## SECURITY-05: Input Validation on All API Parameters

Rule: every API endpoint (REST, GraphQL, gRPC, WebSocket) MUST validate all input: type checking, length/size bounds, format validation (allowlists), sanitization (escape/reject HTML/script), injection prevention (parameterized queries only).

Verification: every handler uses validation library/schema; no raw input concatenated into SQL/NoSQL/OS commands; string inputs have max-length; request body size limits configured.

## SECURITY-06: Least-Privilege Access Policies

Rule: every IAM policy/role/permission MUST follow least privilege: specific resource IDs (no wildcard unless API lacks resource-level permissions — document exception), specific actions (no wildcard), scoped conditions, separate read/write statements.

Verification: no wildcard actions/resources without documented exception; no over-broad service roles; managed policies preferred; trust policies scoped to specific service/account.

## SECURITY-07: Restrictive Network Configuration

Rule: all network configs (security groups, NACLs, route tables) MUST follow deny-by-default: only required ports open; no inbound `0.0.0.0/0` except public LB on 80/443; no outbound `0.0.0.0/0` all ports without justification; private subnets no direct IGW routes; use private endpoints where available.

Verification: no inbound `0.0.0.0/0` except public LB 80/443; DB/app rules restrict source to specific CIDR/SG; private subnets route through NAT; private endpoints for high-traffic service calls.

## SECURITY-08: Application-Level Access Control

Rule: every endpoint accessing/mutating resources MUST enforce authorization: deny by default (all routes require auth unless explicitly public); object-level authorization (verify ownership/permission per resource ID — prevent IDOR); function-level authorization (admin/privileged ops check role server-side); CORS restricted to explicit origins (no `*` on authenticated endpoints); token validation server-side every request (signature, expiration, audience, issuer).

Verification: every handler has auth middleware/guard; no endpoint returns data without verifying caller ownership; admin routes have server-side role checks; CORS no wildcard on authenticated endpoints; token validation server-side every request.

## SECURITY-09: Security Hardening and Misconfiguration Prevention

Rule: all deployed components MUST follow hardening baseline: no default credentials; minimal installation (remove unused features/samples); production errors no stack traces/internal paths/versions/DB details; disable directory listing; cloud storage block public access unless documented exception; current supported runtime/framework/OS versions.

Verification: no default credentials in config/env/IaC; production errors return generic messages; cloud storage public access blocked unless documented; no sample/demo apps deployed; current supported versions.

## SECURITY-10: Software Supply Chain Security

Rule: dependency pinning (exact versions/lock files); vulnerability scanner configured; no unused dependencies; trusted sources only (official/verified registries); SBOM for production; CI/CD pinned tool versions + verified base images (no `latest` tags in production).

Verification: lock file committed; vulnerability scanning in CI/CD or documented; no unused/abandoned deps; no `latest`/unpinned image tags in production; official/verified registries.

## SECURITY-11: Secure Design Principles

Rule: separation of concerns (security-critical logic in dedicated modules); defense in depth (layer controls); rate limiting on public-facing endpoints; design considers misuse cases.

Verification: security logic encapsulated in dedicated modules/services; rate limiting on public APIs; design docs address at least one misuse/abuse scenario.

## SECURITY-12: Authentication and Credential Management

Rule: password policy (min 8 chars, check breached lists); credential storage (adaptive hashing algorithms); MFA (MUST for admin, SHOULD for all); session management (server-side expiration, invalidate on logout, secure/httpOnly/sameSite cookies); brute-force protection (lockout/delay/CAPTCHA); no hardcoded credentials (use secrets manager).

Verification: adaptive password hashing; session cookies set Secure + HttpOnly + SameSite; brute-force protection on login; no hardcoded credentials; MFA for admin; sessions invalidated on logout with defined expiration.

## SECURITY-13: Software and Data Integrity Verification

Rule: deserialization safety (no untrusted deserialization without validation); artifact integrity (checksums/signatures for deps/plugins/updates); CI/CD pipeline security (restrict pipeline definition modification, separate code authors/deployment approvers); CDN/external resources use SRI hashes; critical data modifications auditable (who, what, when).

Verification: no unsafe deserialization of untrusted input; external scripts include SRI; CI/CD definitions access-controlled + auditable; critical data changes logged with actor + timestamp + before/after.

## SECURITY-14: Alerting and Monitoring

Rule: security event alerting (repeated auth failures, privilege escalation, unusual access, authorization failures); log integrity (append-only/tamper-evident, app MUST NOT delete/modify own audit logs); log retention (min 90 days default); monitoring dashboard/alarm config for key metrics.

Verification: alerting for auth failures + authorization violations; log retention >= 90 days; app roles cannot delete own log groups; security events generate alerts.

## SECURITY-15: Exception Handling and Fail-Safe Defaults

Rule: catch and handle all external calls (DB, API, file I/O); fail closed (deny/halt on error, never fail open); resource cleanup on error paths (try/finally, using, equivalent); user-facing errors generic (no internal details); global error handler catches unhandled exceptions + logs per SECURITY-03 + returns safe response.

Verification: all external calls have explicit error handling; global error handler at entry point; error paths don't bypass auth/validation (fail closed); resources cleaned up on error; no unhandled promise rejections/uncaught exceptions.

## ENFORCEMENT_INTEGRATION

Cross-cutting at every stage: evaluate all SECURITY verification criteria against artifacts; include "Security Compliance" section in completion summary (compliant/non-compliant/N/A per rule); non-compliant → blocking finding; include security rule references in design docs + test instructions.

## OWASP_REFERENCE_MAPPING

| SECURITY Rule | OWASP Category |
|---|---|
| SECURITY-08 | A01:2025 Broken Access Control |
| SECURITY-09 | A02:2025 Security Misconfiguration |
| SECURITY-10 | A03:2025 Software Supply Chain Failures |
| SECURITY-11 | A06:2025 Insecure Design |
| SECURITY-12 | A07:2025 Authentication Failures |
| SECURITY-13 | A08:2025 Software or Data Integrity Failures |
| SECURITY-14 | A09:2025 Logging & Alerting Failures |
| SECURITY-15 | A10:2025 Mishandling of Exceptional Conditions |
