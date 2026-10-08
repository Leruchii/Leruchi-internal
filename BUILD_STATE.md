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

## CI result update — 2026-10-08, exact head `199e5100557c533cc287573baf2483e377608868`

Stage 03 Supabase compatibility now **PASS** on the exact fix commit — [run 37769250756](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250756). This validates that the compatibility role credentials now align with the Auth/PostgREST connection configuration and the integration workflow completes successfully. Job evidence: `[{"job":"supabase-compatibility","conclusion":"success","failedSteps":[]}]`.

Stage 32 OSS Core Publication Candidate **PASS** — [run 37769250397](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250397). Stage State Gate **PASS** — [run 37769251598](https://github.com/Leruchii/Leruchi-development/actions/runs/37769251598). Stage 12 Graph Realtime **PASS** — [run 37769251173](https://github.com/Leruchii/Leruchi-development/actions/runs/37769251173).

Exact-head workflow snapshot: {"success":34,"in_progress":1}.
- Stage State Gate: success — [run 37769251598](https://github.com/Leruchii/Leruchi-development/actions/runs/37769251598)
- Stage 12 Graph Realtime: success — [run 37769251173](https://github.com/Leruchii/Leruchi-development/actions/runs/37769251173)
- Stage 13 Graph Studio: in_progress — [run 37769250709](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250709)
- Stage 10 JavaScript SDK: success — [run 37769250646](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250646)
- Stage 25 MCP Agent Tool Contract and Input Safety: success — [run 37769250429](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250429)
- Stage 26B - Agent Native Context IR: success — [run 37769250849](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250849)
- Stage 16 Observability: success — [run 37769250722](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250722)
- Stage 26A - Repository Separation: success — [run 37769250787](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250787)
- Architecture Regression Audit: success — [run 37769250441](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250441)
- Stage 27 - Agent Intent Boundary: success — [run 37769250570](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250570)
- Stage 23 Unified Retrieval Surface: success — [run 37769250582](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250582)
- Stage 28 - Cross-Modal Planning: success — [run 37769250733](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250733)
- Stage 29 - Agent Evaluation & Observability: success — [run 37769250439](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250439)
- Stage 32 - OSS Core Publication Candidate: success — [run 37769250397](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250397)
- Stage 06 Query Validation: success — [run 37769250446](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250446)
- Stage 22 Retrieval Planner: success — [run 37769250753](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250753)
- Stage 26C - Context Resolution: success — [run 37769250393](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250393)
- Stage 05 Query IR: success — [run 37769250991](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250991)
- Stage 31 - OSS Core Readiness: success — [run 37769250470](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250470)
- Stage 24 Retrieval Explainability and Agent Safety: success — [run 37769250453](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250453)
- Stage 30 Agent Trace Checks: success — [run 37769250412](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250412)
- Stage 08 Secure Execution Engine: success — [run 37769250458](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250458)
- Stage 17 Backup and Recovery: success — [run 37769250805](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250805)
- Stage 04 Schema Catalog: success — [run 37769250653](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250653)
- Stage 03 Supabase compatibility: success — [run 37769250756](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250756)
- Stage 02 RLS and AGE security: success — [run 37769250482](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250482)
- Stage 20 Production Readiness: success — [run 37769250847](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250847)
- Stage 15 GraphRAG: success — [run 37769250920](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250920)
- Stage 09 Graph Mutations: success — [run 37769250616](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250616)
- Stage 21 PostgreSQL Recursive Compiler: success — [run 37769250711](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250711)
- Stage 01 database foundation: success — [run 37769250713](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250713)
- Stage 14 MCP Agent Gateway: success — [run 37769250488](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250488)
- Stage 11 CLI: success — [run 37769250776](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250776)
- Stage 07 Apache AGE Compiler: success — [run 37769250826](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250826)
- Stage 32 - OSS Core Publication Candidate: success — [run 37769243099](https://github.com/Leruchii/Leruchi-development/actions/runs/37769243099)

One workflow remains in progress at this update: Stage 13 Graph Studio [run 37769250709](https://github.com/Leruchii/Leruchi-development/actions/runs/37769250709). Wait for its result and refresh this handoff. No failures are recorded. Do not merge or publish; production identity-provider tenant-claim issuance remains a release blocker.

## Next checkpoint

Complete the tenant authorization/security hardening and ASVS Level-2 evidence work in development, synchronize the resulting durable decisions here, then perform the final controlled OSS export review before publication.
