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

## CI result update — 2026-10-08, exact head `6a3a507c2b4494c33115fbf2672cceeb32fda250`

This commit updates 26 workflows from `actions/checkout@v4` to `@v5` and `actions/setup-node@v4` to `@v6`, preserving explicit Node.js 24 setup. Regression matrix snapshot: {"in_progress":16,"success":19}; no failures at this snapshot.

Completed workflow results:
- Stage 26B - Agent Native Context IR: PASS — [run 37769880829](https://github.com/Leruchii/Leruchi-development/actions/runs/37769880829)
- Stage 16 Observability: PASS — [run 37769881000](https://github.com/Leruchii/Leruchi-development/actions/runs/37769881000)
- Stage 31 - OSS Core Readiness: PASS — [run 37769880835](https://github.com/Leruchii/Leruchi-development/actions/runs/37769880835)
- Stage 26C - Context Resolution: PASS — [run 37769881167](https://github.com/Leruchii/Leruchi-development/actions/runs/37769881167)
- Architecture Regression Audit: PASS — [run 37769880907](https://github.com/Leruchii/Leruchi-development/actions/runs/37769880907)
- Stage 28 - Cross-Modal Planning: PASS — [run 37769880929](https://github.com/Leruchii/Leruchi-development/actions/runs/37769880929)
- Stage State Gate: PASS — [run 37769881298](https://github.com/Leruchii/Leruchi-development/actions/runs/37769881298)
- Stage 23 Unified Retrieval Surface: PASS — [run 37769881224](https://github.com/Leruchii/Leruchi-development/actions/runs/37769881224)
- Stage 27 - Agent Intent Boundary: PASS — [run 37769881303](https://github.com/Leruchii/Leruchi-development/actions/runs/37769881303)
- Stage 25 MCP Agent Tool Contract and Input Safety: PASS — [run 37769881180](https://github.com/Leruchii/Leruchi-development/actions/runs/37769881180)
- Stage 06 Query Validation: PASS — [run 37769881033](https://github.com/Leruchii/Leruchi-development/actions/runs/37769881033)
- Stage 05 Query IR: PASS — [run 37769881297](https://github.com/Leruchii/Leruchi-development/actions/runs/37769881297)
- Stage 26A - Repository Separation: PASS — [run 37769880985](https://github.com/Leruchii/Leruchi-development/actions/runs/37769880985)
- Stage 24 Retrieval Explainability and Agent Safety: PASS — [run 37769880879](https://github.com/Leruchii/Leruchi-development/actions/runs/37769880879)
- Stage 22 Retrieval Planner: PASS — [run 37769881194](https://github.com/Leruchii/Leruchi-development/actions/runs/37769881194)
- Stage 29 - Agent Evaluation & Observability: PASS — [run 37769880803](https://github.com/Leruchii/Leruchi-development/actions/runs/37769880803)
- Stage 10 JavaScript SDK: PASS — [run 37769880925](https://github.com/Leruchii/Leruchi-development/actions/runs/37769880925)
- Stage 30 Agent Trace Checks: PASS — [run 37769880686](https://github.com/Leruchii/Leruchi-development/actions/runs/37769880686)
- Stage 32 - OSS Core Publication Candidate: PASS — [run 37769873602](https://github.com/Leruchii/Leruchi-development/actions/runs/37769873602)

19 workflows remain in progress. Continue polling this exact head and refresh the handoff as results arrive. Do not make another development change until the matrix completes. Do not merge or publish; production identity-provider tenant-claim issuance remains a release blocker.

## Next checkpoint

Complete the tenant authorization/security hardening and ASVS Level-2 evidence work in development, synchronize the resulting durable decisions here, then perform the final controlled OSS export review before publication.
