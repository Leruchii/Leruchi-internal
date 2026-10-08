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

## CI result update — 2026-10-08, MCP route capability + Node.js 24 fix validated

**Exact-head matrix complete: 30 successes, 0 failures** on `d754c43aa666cf37cbfba8fe874411e5ff27add1`.

- Stage 02 RLS/AGE security passed with Node.js 24 and the corrected capability-denial expectation: https://github.com/Leruchii/Leruchi-development/actions/runs/37775397959
- Stage 14 MCP Agent Gateway passed, including Graph API route capability enforcement tests: https://github.com/Leruchii/Leruchi-development/actions/runs/37775398178
- Stage 25 MCP Agent Tool Contract passed: https://github.com/Leruchii/Leruchi-development/actions/runs/37775397907
- Stage 13 Graph Studio passed as the final workflow: https://github.com/Leruchii/Leruchi-development/actions/runs/37775397964

Graph API now requires `graph:read` for graph queries, `graph:write` for mutations, and `graph:delete` additionally for destructive deletes. These checks happen before catalog/database access. Stage 02/06/07/08 now explicitly use `actions/setup-node@v6` with Node.js 24.

Next security work: integrate signed scoped capability grants with a required control-plane revocation decision; current grant validation helpers are not yet wired to production token verification/revocation. Production Supabase hook configuration and membership provisioning also still require validation. PR #64 remains draft; PR #63 unchanged. Do not merge or publish without explicit approval.

## Next checkpoint

Complete the tenant authorization/security hardening and ASVS Level-2 evidence work in development, synchronize the resulting durable decisions here, then perform the final controlled OSS export review before publication.


## Pre-run checkpoint — signed capability grant enforcement, 2026-10-08

Handoff was re-read before advancing the validation branch. Baseline exact-head matrix: `d754c43aa666cf37cbfba8fe874411e5ff27add1`, 30 successes and zero failures. Primary Stage 32 PR #63 remains separate; PR #64 is draft and targets the Stage 32 branch.

Candidate commit prepared on top of the validated baseline: `e268ade8f5caf265e5277fbbccd239fecde69396`. It wires signed capability-grant validation into Graph API authentication, requires a control-plane revocation decision in strict mode, fails closed on missing/revoked/unavailable decisions, enforces optional route/graph scope, adds an authenticated HTTPS-enforcing control-plane adapter, enables strict mode in the production launcher, and adds unit/API/MCP integration coverage using a test-only control-plane stub.

**No CI has run on this candidate yet.** Next: advance only the isolated validation branch, inspect all exact-head workflow results (especially Stage 14 and Stage 02), then update this handoff and the Stage 32 decision record with actual results. The test stub does not prove production control-plane deployment. Do not merge or publish.


## CI checkpoint — signed capability grant candidate, exact head e268ade8f5caf265e5277fbbccd239fecde69396

The isolated validation branch now points to `e268ade8f5caf265e5277fbbccd239fecde69396`. The full PR workflow matrix has started. Initial snapshot: 30 workflows discovered, 4 in progress and 26 queued; no completed conclusions yet. Stage 14 MCP Agent Gateway is currently in progress after the capability vocabulary, catalog capability, Graph API route capability and MCP contract test steps passed; the capability-grant contract test is running.

Stage 32 candidate workflow is also in progress: https://github.com/Leruchii/Leruchi-development/actions/runs/37777679790. Stage 14 run: https://github.com/Leruchii/Leruchi-development/actions/runs/37777685258.

Do not claim this candidate passed until every exact-head workflow finishes. After completion, inspect any failure logs, fix only after updating this handoff, and rerun the exact new head. Production control-plane deployment remains a release gate.


## CI progress refresh — exact head e268ade8f5caf265e5277fbbccd239fecde69396

Latest exact-head snapshot: **16 successes, 14 in progress, 0 failures**. Stage 14 has now also passed the capability-grant contract tests and mutation-approval/audit tests; database image build is running. No failure has been observed yet. The full matrix is still running, so this is not a green-matrix claim.


## CI progress refresh — exact head e268ade8f5caf265e5277fbbccd239fecde69396

Latest snapshot: **28 successes, 2 in progress, 0 failures**. Stage 14 MCP Agent Gateway passed, including strict grant tests, control-plane adapter contract tests, Graph API route checks, live MCP → Graph API → PostgreSQL integration, and architecture regression audit: https://github.com/Leruchii/Leruchi-development/actions/runs/37777685258.

Only Stage 03 Supabase compatibility and Stage 13 Graph Studio remain in progress. Wait for both before declaring the exact-head matrix complete. The live workflow used a test-only control-plane stub; it does not prove production issuer/revocation deployment.


## CI result — exact head e268ade8f5caf265e5277fbbccd239fecde69396

The matrix finished with **29 successes, 1 failure**. Stage 14 passed as recorded above. Stage 13 Graph Studio failed: https://github.com/Leruchii/Leruchi-development/actions/runs/37777685084. No other failures are reported on this exact head.

Do not rerun or change the branch until the Stage 13 failure log is inspected and this handoff is re-read. Next step is determine whether the failure is caused by this candidate's strict grant integration or an independent regression; then record the diagnosis and fix plan here before a new commit/run. The production control-plane release gate remains open.


## Stage 13 failure diagnosis — exact head e268ade8f5caf265e5277fbbccd239fecde69396

Root cause confirmed from Stage 13 job log: `scripts/start-stage13-graph-api.mjs` now correctly requires strict grant-mode configuration, but the Stage 13 workflow still starts it without `LERUCHI_CAPABILITY_ISSUER` (and without a control-plane endpoint/token). The launcher exits before the Graph API starts, so the wait-for-health step times out. This is workflow fixture/configuration drift introduced by making strict mode mandatory in the production launcher; it is not a graph renderer or Graph API route test failure.

Fix plan before the next run: update Stage 13 workflow to start the same test-only control-plane stub, wait for readiness, pass the issuer/control-plane configuration to the launcher, and update any signed tokens in the Stage 13 workflow to include `iss`, `jti`, `aud=leruchi`, `tenant_id`, canonical capabilities and `exp`. Keep production control-plane deployment as a release blocker. No new run should start until the handoff is re-read after this diagnosis.


## Pre-run checkpoint — Stage 13 workflow fix, 2026-10-08

The prior head e268ade8f5caf265e5277fbbccd239fecde69396 had 29 successes and one Stage 13 failure. The log showed the launcher exited because the Stage 13 workflow omitted required strict capability-grant configuration. Candidate b806511f854b33b125e04d6c787351813acf1fd5 adds the test-only control-plane service and passes its configuration to the launcher. It has not run CI. Before advancing the branch, re-read this handoff and confirm the branch is still on e268ade8f5caf265e5277fbbccd239fecde69396. Production control-plane deployment remains a release blocker; do not merge or publish.


## CI restart — Stage 13 workflow fix candidate b806511f854b33b125e04d6c787351813acf1fd5

The isolated validation branch advanced from e268ade8f5caf265e5277fbbccd239fecde69396 to b806511f854b33b125e04d6c787351813acf1fd5 after the handoff was re-read. The first observed run is Stage 32 candidate, queued: https://github.com/Leruchii/Leruchi-development/actions/runs/37778305792. The rest of the exact-head workflow matrix was not yet visible at this check. No pass claim; inspect all runs on this exact SHA and update this handoff when the matrix completes.


## CI progress — Stage 13 fix candidate b806511f854b33b125e04d6c787351813acf1fd5

Latest exact-head snapshot: 10 successes, 20 in progress, zero failures. Stage 13 Graph Studio is running: https://github.com/Leruchii/Leruchi-development/actions/runs/37778312543. Stage 14 is running: https://github.com/Leruchii/Leruchi-development/actions/runs/37778312512. The Stage 32 candidate passed: https://github.com/Leruchii/Leruchi-development/actions/runs/37778305792. Matrix remains in progress; no overall pass claim yet.


## CI progress refresh — exact head b806511f854b33b125e04d6c787351813acf1fd5

Latest snapshot: 25 successes, 5 in progress, zero failures. Stage 13 and Stage 14 are still running; Stage 03, Stage 02, Stage 09 and Stage 15 are also in progress. No failures are reported so far. Keep waiting for the complete exact-head matrix before calling it green.


## CI progress refresh — exact head b806511f854b33b125e04d6c787351813acf1fd5

Latest snapshot: 29 successes, 1 in progress, zero failures. Stage 13 has now passed database build/startup, live tenant fixture seeding, test control-plane startup, strict Graph API startup/health, renderer benchmark and tenant-render contract. Chromium/browser tests and final build/audits remain to complete.


## CI result — Stage 13 workflow fix candidate b806511f854b33b125e04d6c787351813acf1fd5

The exact-head matrix completed with 29 successes and one failure. Stage 13 now starts the strict Graph API successfully; database startup, control-plane stub readiness, Graph API health, renderer benchmark and tenant-render contract all passed. The remaining Stage 13 failure is Browser tenant/responsive evidence: https://github.com/Leruchii/Leruchi-development/actions/runs/37778312543. This is a different failure from the fixed launcher configuration issue. Do not rerun until the browser failure log is inspected, diagnosed, and this handoff is re-read. No other failures are reported on this exact head.


## Stage 13 browser failure diagnosis — exact head b806511f854b33b125e04d6c787351813acf1fd5

The strict launcher/configuration fix worked: Graph API startup and health checks passed. The remaining failure is in `tests/browser/live-composition.spec.ts`: its cookie token is legacy-shaped and lacks the new signed-grant claims (`iss`, `jti`, `aud=leruchi`), so `/api/studio/catalog` never returns the expected HTTP 200 and Playwright times out waiting for that response. Six other browser tests passed. This is test-fixture drift from strict grant enforcement, not a Studio renderer failure.

Fix plan: update the browser test token helper to include the configured issuer, unique jti, audience, tenant, expiry and canonical graph capabilities; the test control-plane stub already authorizes active JTIs. Then record a new candidate SHA here before moving the branch, and re-read this handoff before the next run. Keep production control-plane deployment as a release blocker.


## Pre-run checkpoint — Stage 13 browser signed-grant fixture, 2026-10-08

Prior exact head b806511f854b33b125e04d6c787351813acf1fd5 completed with 29 successes and one failure in Stage 13 browser evidence. The log showed Playwright timed out waiting for /api/studio/catalog to return 200 because the browser test token lacked strict grant claims. Graph API startup/health and six other browser tests passed.

Candidate commit 57dd4815d7137945945c234f5d7ca9970e637bef updates the Playwright token to include the test control-plane issuer, unique jti, tenant_id, aud=leruchi, expiry and graph:read capability, using the configured JWT secret. The candidate has not run CI. Next: re-read this handoff, confirm the branch still points to b806511f854b33b125e04d6c787351813acf1fd5, then advance the isolated branch and inspect the exact-head matrix. Production control-plane deployment remains a release blocker.


## CI restart — Stage 13 browser grant fixture candidate 57dd4815d7137945945c234f5d7ca9970e637bef

After re-reading the handoff, the isolated validation branch advanced from b806511f854b33b125e04d6c787351813acf1fd5 to 57dd4815d7137945945c234f5d7ca9970e637bef. The first observed run is Stage 32 candidate, queued: https://github.com/Leruchii/Leruchi-development/actions/runs/37778842705. The rest of the exact-head matrix was not yet visible at this check. No pass claim until all workflows finish.


## CI progress — Stage 13 browser grant fixture candidate 57dd4815d7137945945c234f5d7ca9970e637bef

Exact-head matrix snapshot: 7 successes, 22 in progress, 1 queued, zero failures. Stage 13 Graph Studio is running: https://github.com/Leruchii/Leruchi-development/actions/runs/37778853869. Stage 14 MCP Agent Gateway is running: https://github.com/Leruchii/Leruchi-development/actions/runs/37778853920. Continue checking the exact SHA and update this handoff as results change; no overall pass claim yet.


## CI progress refresh — exact head 57dd4815d7137945945c234f5d7ca9970e637bef

Latest matrix snapshot: 16 successes, 13 in progress, 1 queued, zero failures. Stage 13 and Stage 14 remain in progress. The new browser-token grant fixture has not yet been validated by the full Stage 13 browser suite. Wait for completion and inspect any failures before the next code change.


## CI progress refresh — exact head 57dd4815d7137945945c234f5d7ca9970e637bef

Latest snapshot: 19 successes, 11 in progress, zero failures. The previously queued workflow has started; Stage 13 and Stage 14 remain in progress. No failures are reported on this head so far.


## CI progress refresh — exact head 57dd4815d7137945945c234f5d7ca9970e637bef

Latest snapshot: 28 successes, 2 in progress, zero failures. Stage 14 MCP Agent Gateway passed again with strict grant/revocation integration: https://github.com/Leruchii/Leruchi-development/actions/runs/37778853920. Stage 13 is still installing Chromium; Stage 20 Production Readiness is the other outstanding workflow. Await both before declaring the matrix complete.


## CI progress refresh — exact head 57dd4815d7137945945c234f5d7ca9970e637bef

Latest snapshot: 29 successes, 1 in progress, zero failures. Stage 20 Production Readiness passed. Stage 13 Graph Studio has completed Chromium installation and is running the browser tenant/responsive evidence suite. It is the only remaining workflow.
