# Stage 32 — Tenant Claim Issuance

Status: PROPOSED — needs Fikun's confirmation of the two decisions below. Not a release approval.

## Context

The production tenant-claim issuance mechanism was the open release-blocking security decision. The development repo implements a Supabase GoTrue custom access-token hook (`vibe_auth.custom_access_token_hook`) that sets the top-level `tenant_id` claim only from `vibe_auth.user_tenant_memberships`.

## Decision 1 — Reference issuance path (PROPOSED)

The Supabase GoTrue custom access-token hook is the tested reference path for self-hosted Leruchi Core.

Contract any issuer must meet:
- `tenant_id` is set only by the trusted issuer from server-side membership data, never from client payloads or user-editable metadata.
- `user_metadata.active_tenant_id` is a selector only. An invalid, unknown or revoked selection yields no `tenant_id` claim (fail closed).
- Any pre-existing top-level `tenant_id` is removed before resolution.
- The verifier (Graph API) accepts a token only with valid signature, expiry, issuer, audience and a tenant claim; PostgreSQL RLS remains the data-plane authority.

Other OIDC identity providers are supported through this contract but are **not certified** by CI.

## Decision 2 — Revocation window (PROPOSED)

A revoked membership stops producing claims on the next token refresh. Access tokens issued before revocation remain valid until expiry (`GOTRUE_JWT_EXP`, 3600s in the local compose).

Proposal: accept and document this window for the OSS release; treat shorter default lifetimes or membership-aware revocation checks as a Cloud/Enterprise hardening item. Alternative if rejected: lower the self-hosted default `GOTRUE_JWT_EXP` (for example 300s) before release.

## Evidence

Development repo, Stage 03 Supabase workflow:
- real Auth-issued claim for an active membership; no claim for a revoked selector; RLS isolation through PostgREST (pre-existing);
- tamper and revocation step added on branch `stage32-tenant-revocation-evidence` (draft PR #65); passed in Stage 03 run 37801292894 on head `5d3d7ef`, with all 21 workflows green.

## Still required before this blocker can close

1. The new CI step passes on the exact release-candidate head (passed on PR #65 head `5d3d7ef`; must be re-confirmed after merge).
2. Graph API leg: a GoTrue-issued token accepted by `createGraphApiServer` with the configured issuer/audience, and wrong issuer/audience rejected.
3. Fikun confirms Decisions 1 and 2, then this file's status changes to DECIDED.
