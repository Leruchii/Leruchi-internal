# VibePlatform Architecture

Status: DECIDED / Stage 26B VALIDATED / Stage 26C IMPLEMENTED — NOT YET VALIDATED

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

Stage 26B validated Context IR as the declarative contract for context needs:

Agent/Application
→ Context IR
→ validation
→ authorization-aware explanation

Context IR may reference Query/Retrieval IR, but never carries tenant identity, credentials, authorization grants, physical engine selectors, raw SQL or raw Cypher.

Stage 26C adds bounded Context Resolution:

Context IR + source parameter envelope
→ trusted ExecutionContext
→ preflight every source
→ tenant-scoped Schema Catalog
→ existing Query/Retrieval validation + planner/compiler
→ existing Secure Execution / Retrieval Execution
→ bounded normalized context result

Rules:
- preflight happens before catalog/database access;
- query context reuses the canonical Query IR executor;
- retrieval context reuses the canonical Retrieval IR executor;
- vector retrieval requires vector:read; graph/schema/query context requires graph:read;
- source parameters are execution bindings, not part of Context IR;
- aggregate item/byte/input budgets fail closed;
- records resolution is unsupported until a safe record-selection IR is explicitly decided;
- Context Resolution is read-only and cannot grant capabilities or authorize writes;
- no second execution path is introduced.

## Repository boundary

- vibeDB-internal: internal development control, strategy, agent rules and decisions.
- vibeDB-development: private implementation history and active engineering.
- vibeDB: public OSS release target, populated only through explicit allowlisted export.

Cloud/Enterprise may consume Core. Core must never depend on private Cloud/Enterprise implementation.
