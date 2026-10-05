# VibePlatform Architecture

Status: DECIDED / Stage 26C VALIDATED / Stage 27 IMPLEMENTED — NOT YET VALIDATED

## Core execution architecture

Developer / Agent
→ SDK / REST / CLI / Studio / MCP
→ engine-neutral IR
→ validation
→ trusted tenant/capability context
→ planner/compiler
→ Secure Execution
→ PostgreSQL / AGE / pgvector / RLS

## Agent-native context

Stage 26B validates Context IR as the declarative contract for bounded context needs.

Stage 26C validates Context Resolution:

Context IR + source parameter envelope
→ trusted ExecutionContext
→ preflight every source
→ tenant-scoped Schema Catalog
→ existing Query/Retrieval validation + planner/compiler
→ existing Secure Execution / Retrieval Execution
→ bounded normalized context result

Context Resolution does not introduce a second executor. `records` remains unsupported until a safe record-selection contract is decided.

## Agent intent

Stage 27 adds a closed, provider-neutral Agent Intent layer above the canonical IRs:

Agent / future model adapter
→ Agent Intent
→ trusted ExecutionContext preflight
→ deterministic action ↔ target-IR validation
→ capability/safety derivation
→ canonical Query / Retrieval / Context / Mutation IR validation
→ non-executing explanation

Agent Intent does not replace the target IR and does not execute it in Stage 27.

Security rules:
- tenant identity and credentials are never caller-controlled through Agent Intent;
- required capabilities are derived by VibeDB from the canonical target IR;
- model output/natural language is untrusted input, never authorization;
- destructive mutation intent can report approval-required semantics without executing;
- bindings are bounded and are not an authorization mechanism;
- diagnostics expose only bounded policy metadata and request correlation, never tenant IDs, bindings, raw IR, credentials or catalog internals;
- no provider-specific model contract or second execution path is introduced.

## Repository boundary

- vibeDB-internal: internal development control, strategy, agent rules and decisions.
- vibeDB-development: private implementation history and active engineering.
- vibeDB: public OSS release target, populated only through explicit allowlisted export.

Cloud/Enterprise may consume Core. Core must never depend on private Cloud/Enterprise implementation.
