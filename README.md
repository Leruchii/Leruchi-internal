# VibeDB Internal Development Control Repository

Status: DECIDED

This private repository is the authoritative home for VibeDB internal engineering control material.

## Purpose
- Internal agent instructions and engineering constitution.
- Build state, build plan, architecture decisions and internal handoffs.
- Product strategy and OSS/Cloud/Enterprise boundary decisions.
- Internal release gates and development-only automation.

## Public boundary
The public repository `Fikunmii/vibeDB` is a sanitized OSS product repository. Internal strategy, private roadmap, internal agent instructions and development control documents must not be copied into it.

Public releases must be produced from an explicit allowlist of public-safe content. Denylists alone are not sufficient.

## Dependency direction
Private Cloud/Enterprise may consume VibeDB Core.
VibeDB Core must never depend on private Cloud/Enterprise services.

## Source-of-truth rule
For internal development decisions, this repository is authoritative. The public repository is authoritative only for the public OSS surface.

## Security
Never commit credentials, tokens, customer data or production secrets here. Treat this repository as confidential engineering material despite GitHub private visibility.

## Current migration
Stage 26A is establishing this repository and the public OSS boundary. The original Stage 26A public PR was intentionally closed without merge and is being replaced with a public-safe implementation.
