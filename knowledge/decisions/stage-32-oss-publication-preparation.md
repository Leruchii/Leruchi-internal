# Stage 32 — OSS Core Publication Preparation

Status: IN_PROGRESS / CONTROLLED RELEASE PREPARATION

## Purpose

Stage 32 prepares the reviewed, sanitized Leruchi Core candidate for eventual publication to `Leruchii/Leruchi`.

It does not publish automatically and does not authorize merging the development release branch.

## Repository roles

- `Leruchii/Leruchi-development`: implementation, executable tests, CI, security evidence and candidate construction.
- `Leruchii/Leruchi-internal`: architecture, release policy, durable decisions and handoff state.
- `Leruchii/Leruchi`: public OSS Core destination.

## Security gate

OWASP ASVS 5.0.0 is the current verification baseline.

The project does not claim full ASVS compliance merely because the Stage 32 workflow passes. Requirement-level evidence must exist for every requirement claimed as PASS.

The current security hardening sequence prioritizes:
1. authoritative production tenant claim issuance;
2. tenant/RLS isolation;
3. realtime role separation;
4. MCP and agent capability authorization;
5. token/session/revocation controls;
6. API and canonical IR validation;
7. cryptography and secret handling;
8. secure communications;
9. security logging and error handling;
10. data protection and configuration.

## Tenant authorization decision

Tenant identity is authoritative only when established by trusted identity/authentication infrastructure. Normal request payloads and IR fields must not override it.

PostgreSQL/RLS remains the authoritative data-plane boundary.

The production tenant-claim issuance mechanism remains unresolved and is a release blocker until its intended deployment path has executable evidence.

## Realtime decision

Realtime notifications remain ID-only or minimal wherever possible. Clients refetch through authorized APIs so PostgreSQL/RLS remains authoritative.

The runtime database identity and privileged realtime relay identity are separate. Privileged relay access must never be exposed through ordinary runtime credentials.

## OSS export decision

Public publication uses the explicit export manifest and sanitized candidate audit. Private/cloud/enterprise material is not copied wholesale into the public repository.

Repository visibility does not redefine the OSS boundary.

## Release rule

Do not publish `Leruchii/Leruchi` until:
- Stage 32 candidate workflow is green on the exact release candidate;
- required security hardening is validated;
- ASVS evidence is honestly classified;
- the production tenant authorization blocker is resolved or explicitly redesigned;
- public export audit passes;
- final release review is explicitly approved.
