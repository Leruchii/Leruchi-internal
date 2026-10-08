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


## Authentication trust-boundary update

The development Graph API now supports explicit JWT issuer and audience validation in addition to signature, expiry and tenant-claim validation. This establishes the verifier-side trust boundary, but it does not by itself prove production identity-provider claim issuance. Production deployment must configure a trusted issuer and audience and provide executable evidence that the tenant claim is bound by that identity system.


## Live CI diagnosis — 2026-10-08

The exact current development PR head inspected for continuation is `db3c71af8b3a5275401ba5c77a64cd2b77a5bd3a` (PR #63 remains draft/open and is not merged). The Stage 32 candidate workflow and Stage 31 readiness workflow passed on that head. Stage 03 Supabase compatibility failed because Auth and PostgREST could not authenticate to PostgreSQL.

The failure is caused by a mismatch between passwords defined in `infra/supabase/roles.sql` and the connection URLs in `infra/supabase/docker-compose.yml`: the SQL role setup uses former `vibe_compat_*` password values while the compose URLs use `leruchi_compat_*` values. The currently checked compose file contains only one `GOTRUE_JWT_AUD` mapping; the duplicate-key diagnosis belongs to an older commit snapshot and is not the current failure.

Next action is to align role setup passwords with the compose credentials, then verify Stage 03 and the full required workflow matrix on the exact resulting head. This does not clear the separate production identity-provider tenant-claim issuance release blocker. Do not merge or publish.


## Superseding checkpoint — 2026-10-08

The earlier CI diagnosis above is historical and referred to head `db3c71af8b3a5275401ba5c77a64cd2b77a5bd3a`. The credential mismatch was corrected in `Leruchii/Leruchi-development` commit `199e5100557c533cc287573baf2483e377608868`; Stage 03 Supabase compatibility passed on that exact head at run https://github.com/Leruchii/Leruchi-development/actions/runs/37769250756.

The current development head is `6a3a507c2b4494c33115fbf2672cceeb32fda250`. It upgrades legacy workflow action pins across 26 workflow files to `actions/checkout@v5` and `actions/setup-node@v6`, keeping Node.js 24 configured. The exact-head matrix snapshot is 34 successes and 0 failures, with Stage 13 Graph Studio still in progress at https://github.com/Leruchii/Leruchi-development/actions/runs/37769880831. See `BUILD_STATE.md` for the latest run-by-run handoff. Do not make another development change until this final workflow completes.

The separate production tenant-claim issuance blocker remains unresolved. Verifier-side JWT issuer/audience/signature/expiry and tenant-context validation are not proof that the production identity provider issues and binds the authoritative tenant claim.


## Final exact-head validation — 2026-10-08

Development head `6a3a507c2b4494c33115fbf2672cceeb32fda250` completed its exact-head workflow matrix with 35 successes and zero failures. Stage 13 Graph Studio was the final workflow to finish successfully: https://github.com/Leruchii/Leruchi-development/actions/runs/37769880831. Stage 32 candidate and Stage 31 readiness also passed on this head. The earlier Stage 03 compatibility password mismatch was fixed and validated at https://github.com/Leruchii/Leruchi-development/actions/runs/37769250756.

Legacy workflow action pins were updated across the remaining 26 workflow files to checkout v5 and setup-node v6, with explicit Node.js 24 setup retained. The current exact-head Stage 32 candidate passed at https://github.com/Leruchii/Leruchi-development/actions/runs/37769880841.

The public repository remains unpublished, PR #63 remains a draft/open PR, and production identity-provider tenant-claim issuance remains a release blocker. The next implementation area is to establish the supported production identity-provider tenant-claim issuance/binding contract, then continue scoped MCP/agent authorization and revocation/audit work. Do not merge or publish without explicit user approval.


## Latest handoff — 2026-10-08

The current development head `6a3a507c2b4494c33115fbf2672cceeb32fda250` completed 35 workflow runs successfully with no failures. Stage 13 Graph Studio finished successfully at https://github.com/Leruchii/Leruchi-development/actions/runs/37769880831.

A follow-on read-only audit found optional Supabase service-profile password drift: Storage and Realtime/Supavisor roles in `infra/supabase/roles.sql` still use legacy passwords while Compose expects the Leruchi-prefixed values. The Stage 03 test covers Auth and PostgREST only, so this optional-profile issue is not covered by its passing result.

Next action is to align the two optional-role passwords and add a config consistency test before the next exact-head matrix. Production tenant-claim issuance remains a separate release blocker. Do not merge or publish.


## Tenant-claim hook validation branch — 2026-10-08

The first unreferenced candidate commit was reviewed and found to have an incomplete workflow: it called the hook without installing it. The corrected workflow is now in commit `e93dde43445fd2f665e04679a049640abb72c4dc` on isolated branch `stage32-tenant-claim-hook-validation`. It runs the hook contract test, installs the SQL hook after Auth/PostgREST initialization, verifies least-privilege grants, and checks fail-closed behavior.

The validation branch has not run CI yet. The next step is a validation PR targeting `stage32-oss-publication-prep`, then inspect the exact-head Stage 03 run and update the handoff. The current PR #63 branch remains unchanged. Actual Auth-issued token claim issuance has not been proven; keep the production tenant-claim release blocker open. Do not merge or publish.


## Validation PR #64 result — 2026-10-08

Validation commit `e93dde43445fd2f665e04679a049640abb72c4dc` completed its exact-head matrix with 17 successes and zero failures. Stage 03 passed with hook SQL installation, contract/permission checks, fail-closed unbound-claim behavior and the existing signed-JWT RLS test: https://github.com/Leruchii/Leruchi-development/actions/runs/37772310562. Stage 13 Graph Studio also passed: https://github.com/Leruchii/Leruchi-development/actions/runs/37772310466.

This does not yet validate actual Auth-issued token claims for valid and revoked memberships. Keep the production tenant-claim issuance release blocker open. PR #64 is a draft validation PR targeting the active Stage 32 branch; PR #63 remains unchanged. Next: add real Auth-issued token integration coverage, then scoped MCP/agent capability authorization and audit/revocation. Do not merge or publish.


## Real Auth-issued token test candidate — 2026-10-08

The prior validation head `e93dde43445fd2f665e04679a049640abb72c4dc` passed 17 workflows. Candidate commit `b1b2ce78a0cc2e2ef47b1c9700638fae56e8a23a` adds a deterministic auth.users/membership fixture and a Stage 03 integration step that signs in through Supabase Auth, verifies active-membership tenant claim issuance, verifies revoked selection omits the claim, and checks PostgREST RLS isolation. The test workflow enables email/password only through a one-run environment override; the Compose default remains disabled. Candidate is not yet on the validation branch and has not run CI.

Next: advance the validation branch only after confirming this handoff, then record exact-head CI results. Keep the release blocker open until the actual Auth-issued token test passes. Do not merge or publish.


The real Auth token integration candidate `b1b2ce78a0cc2e2ef47b1c9700638fae56e8a23a` has been advanced to the isolated validation branch. Exact-head Actions runs were not visible at the first check. The handoff records the current SHA and the test scope; inspect CI and update the handoff after results. Production claim issuance remains blocked until this integration test passes.


The first real Auth-issued token integration attempt failed with `400 invalid_credentials` because the deterministic fixture created `auth.users` rows without their associated `auth.identities` email-provider rows. Supabase Auth migration review confirmed the required identity table fields. Candidate fix `b651a880f72474a7dbe09557f6efaa291ebea8c7` adds those rows. It is not yet on the validation branch and has not run CI. Production tenant-claim issuance remains blocked.


The second real Auth token integration attempt still failed with `400 invalid_credentials` after adding `auth.identities` rows. The direct-SQL user fixture is unreliable for GoTrue login. Proposed correction is to create test users through the supported Auth signup endpoint under test-only Compose overrides, then insert membership rows and sign in again. Keep default signup/email settings unchanged outside CI. This has not yet been implemented or run.


The direct-SQL Auth user fixture still failed with `400 invalid_credentials` on commit `b651a880f72474a7dbe09557f6efaa291ebea8c7`, despite identity rows. Candidate `10e736a2fc16c34e0fdfd288c129642fb6192b94` replaces direct user inserts with Auth's supported signup endpoint under test-only overrides, then seeds tenant memberships and checks issued claims plus PostgREST isolation. Candidate is not yet on the validation branch and has not run CI.


The Auth-signup test candidate `10e736a2fc16c34e0fdfd288c129642fb6192b94` has been advanced to the isolated validation branch. Exact-head Actions runs were not visible at the first check. Inspect Stage 03 and update the handoff after results. Do not merge or publish.


The Auth-signup test candidate `10e736a2fc16c34e0fdfd288c129642fb6192b94` successfully signed up both test users and logs confirm the custom access-token hook ran for each signup, but the Stage 03 integration step failed before claim assertions because shell variables were accidentally stored with literal backslashes (for example `\${SIGNUP_A}`). Correct the shell expansion and rerun; actual token claim assertions remain unvalidated.


Shell expansion fix candidate `d3b251ddee29e9d16a12a42fa0bbfc87d551962f` removes unintended backslashes from the integration script's variable references. It is not yet on the validation branch and has not run CI. Keep the production claim issuance blocker open until the real Auth-issued claim and RLS assertions pass.


Validation branch advanced to `d3b251ddee29e9d16a12a42fa0bbfc87d551962f` to correct shell variable expansion. CI runs were not visible at the first check; inspect Stage 03 and record exact results in `BUILD_STATE.md`. Do not merge or publish.


## Real Auth-issued tenant claim integration passed — 2026-10-08

Stage 03 passed on head `d3b251ddee29e9d16a12a42fa0bbfc87d551962f`: https://github.com/Leruchii/Leruchi-development/actions/runs/37773858431. The test creates users via Auth signup, seeds authoritative membership rows, signs in via password grant, asserts an active membership issues `tenant_id=tenant_a`, asserts a revoked tenant selector omits `tenant_id`, and verifies PostgREST tenant isolation. Hook installation, private grants, fail-closed behavior and the existing signed-JWT RLS check passed too.

This validates the self-hosted test deployment's actual Auth issuance path; production readiness still requires verifying equivalent configuration and membership provisioning in the supported production setup. At handoff update time, 11 workflows had passed and 6 remained in progress. PR #64 remains draft, PR #63 unchanged; do not merge or publish.


## Final Auth-issued tenant claim integration result — 2026-10-08

Head `d3b251ddee29e9d16a12a42fa0bbfc87d551962f` completed 17 workflow runs successfully with zero failures. Stage 03 passed real Auth signup/password-grant token issuance and PostgREST isolation: https://github.com/Leruchii/Leruchi-development/actions/runs/37773858431. Stage 13 Graph Studio was the final workflow and passed: https://github.com/Leruchii/Leruchi-development/actions/runs/37773858093.

Active membership issues the expected `tenant_id`; revoked tenant selection omits it; PostgREST returns no cross-tenant rows. This validates the self-hosted test configuration, not production deployment. Before release, verify production Auth hook configuration and server-managed membership provisioning. Next development area: scoped MCP/agent capability authorization, tool-level authorization, revocation and audit. PR #64 remains draft; PR #63 unchanged; do not merge or publish without explicit approval.


## MCP Graph API capability gap found — 2026-10-08

The green tenant-claim integration matrix was followed by a source audit. Graph query and mutation route handlers did not explicitly enforce `graph:read` / `graph:write` at the API boundary, and delete operations did not explicitly require `graph:delete`. Candidate `6ed63d32f395ed30f1105bfa13d98cc7383ab575` adds those checks before catalog/database access, focused denial tests, and Stage 14 workflow coverage. It is not yet on the validation branch and has not run CI. Scoped grant issuance and revocation integration remain a separate gap.


MCP route capability candidate `6ed63d32f395ed30f1105bfa13d98cc7383ab575` is now on the isolated validation branch. Exact-head Actions runs were not visible at the first check. Inspect Stage 14/02/25 and update `BUILD_STATE.md` with the exact results. Capability grant issuance/revocation control-plane integration remains open.
