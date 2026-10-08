# Leruchi Build State

This file is the canonical handoff checkpoint for coding agents.

Agents must verify this state against Git history, implementation, tests, CI, and `BUILD_PLAN.md` before continuing. If evidence conflicts with this file, executable repository evidence wins and this file must be corrected.

## Current checkpoint

- Current stage: **Stage 32 — OSS Core Publication Preparation**
- Current status: **IN_PROGRESS — controlled publication candidate / security hardening**
- Last validated development stage: **Stage 31 — OSS Core Readiness Gate**
- Development repository: `Leruchii/Leruchi-development`
- Internal control repository: `Leruchii/Leruchi-internal`
- Public OSS Core destination: `Leruchii/Leruchi`
- Active development branch: `stage32-oss-publication-prep`
- Stage 32 PR: `Leruchii/Leruchi-development#63` — draft, not merged
- Public publication: **NOT PERFORMED**
- Merge authority: explicit user approval required

## Stage 31 validation

Stage 31 OSS Core readiness was validated on the development branch before Stage 32 publication preparation.

Validated areas include:
- Node.js 24-only runtime policy;
- OSS export allowlist/audit;
- sanitized candidate construction;
- Agent Governance v1 tests;
- architecture regression audit;
- required historical workflow/regression repairs;
- Apache-2.0 licensing;
- Leruchi product identity and repository separation.

Stage 31 validation does **not** mean the public repository has been published.

## Stage 32 publication candidate

Stage 32 is a controlled release-preparation stage.

The candidate workflow:
1. installs dependencies under Node 24;
2. enforces the runtime policy;
3. runs the OWASP ASVS 5.0.0 verification profile and evidence-ledger integrity tests;
4. verifies the public export manifest;
5. audits the source export contract;
6. builds a sanitized OSS candidate;
7. runs the OSS readiness audit against that candidate;
8. packages the candidate as an artifact.

The workflow does **not** publish or merge the public repository.

## Security baseline

OWASP ASVS 5.0.0 is the current security verification baseline for the publication candidate.

Important distinction:
- Stage 32 CI green = the publication machinery and current automated gates pass.
- ASVS 5.0.0 compliance = **not claimed**.
- Requirement-level evidence is tracked in the development repository.
- Unsupported requirements remain UNMAPPED, PARTIAL, BLOCKED, NOT_APPLICABLE, or DEFERRED rather than being treated as PASS without evidence.

Current security priorities:
1. authoritative production tenant-claim issuance;
2. tenant/RLS enforcement across relational and graph paths;
3. realtime role separation and tenant isolation;
4. MCP/agent capability authorization;
5. token/session/revocation controls;
6. API/IR validation and injection boundaries;
7. cryptography/secrets;
8. secure communications;
9. audit/security logging;
10. data protection and configuration/supply-chain controls.

## Known security decision

The architecture requires:

```
trusted identity
  ↓
authoritative tenant context
  ↓
capability / authorization policy
  ↓
canonical IR validation
  ↓
planner
  ↓
Secure Execution
  ↓
PostgreSQL / AGE / pgvector / RLS
```

Tenant identity must never be accepted as an authoritative value from a normal client payload.

The remaining production authorization question is the mechanism that issues and binds the authoritative tenant claim from the supported identity system. This remains a **release-blocking security decision** until executable evidence exists for the intended deployment path.

## Realtime security

Realtime follows:

```
database mutation
  ↓
outbox / trigger
  ↓
ID-only or minimal event
  ↓
client refetch
  ↓
Graph/API authorization + PostgreSQL RLS
```

Runtime and realtime database identities remain separate. Realtime relay access is explicitly privileged and must not be available through normal runtime credentials.

## Repository responsibilities

### Leruchii/Leruchi-development
Implementation, tests, CI, candidate construction and executable security evidence.

### Leruchii/Leruchi-internal
Architecture decisions, durable security/release policy, build checkpoints, OSS boundary, handoff state and strategic control documents.

### Leruchii/Leruchi
Public OSS Core destination. Publication occurs only through an explicit, reviewed, sanitized release action.

## Handoff rule

Before continuing:
1. Read `PRODUCT_IDENTITY.md`.
2. Read `BUILD_PLAN.md`.
3. Read this `BUILD_STATE.md`.
4. Read relevant `knowledge/decisions/*.md`.
5. Verify the current development branch/CI state.
6. Treat executable evidence as authoritative over stale documentation.
7. Do not publish or merge Stage 32 without explicit release approval.

## Live continuation checkpoint — 2026-10-08

This live checkpoint supersedes any older head SHA recorded in conversation summaries or handoff snippets.

- Development PR: [#63](https://github.com/Leruchii/Leruchi-development/pull/63), open draft, not merged.
- Active branch: `stage32-oss-publication-prep`.
- Verified current development head before this checkpoint update: `db3c71af8b3a5275401ba5c77a64cd2b77a5bd3a`.
- Public publication: NOT PERFORMED.
- Exact-head Stage 32 candidate workflow: PASS on `db3c71af8b3a5275401ba5c77a64cd2b77a5bd3a`.
- Exact-head Stage 31 readiness and architecture regression audit: PASS.
- Exact-head Stage 03 Supabase compatibility: FAIL in `Wait for Auth and PostgREST`.
- Verified failure cause from job logs: Auth and PostgREST cannot authenticate to PostgreSQL because `infra/supabase/roles.sql` creates compatibility roles with `vibe_compat_authenticator` / `vibe_compat_auth`, while `infra/supabase/docker-compose.yml` supplies `leruchi_compat_authenticator` / `leruchi_compat_auth`. This is a credentials mismatch, not the duplicate YAML key reported in an older snapshot. The current compose file has one `GOTRUE_JWT_AUD` key.
- Last run inspected: [Stage 03 Supabase compatibility run 37765214108](https://github.com/Leruchii/Leruchi-development/actions/runs/37765214108).
- Node runtime policy remains Node.js 24 only. Before declaring the whole workflow layer compliant, audit any remaining legacy action pins that run on Node 20.

### Next action (do not skip)

1. Fix the Stage 03 compatibility role passwords in `infra/supabase/roles.sql` to match the corresponding compose service credentials.
2. Update this handoff after the resulting workflow run, recording the exact tested head, run URL, and pass/fail evidence.
3. Recheck all required workflows on the exact new head. Do not merge or publish.
4. After CI is healthy, resume the security sequence at authoritative production tenant-claim issuance, then scoped MCP/agent authorization and revocation/audit controls.

## Next checkpoint

Complete the tenant authorization/security hardening and ASVS Level-2 evidence work in development, synchronize the resulting durable decisions here, then perform the final controlled OSS export review before publication.
