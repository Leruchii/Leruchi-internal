# Stage 26C — Context Resolution & Secure Execution Contract

Status: DECIDED / IMPLEMENTED — NOT YET VALIDATED

## Decision

Context Resolution composes existing trusted VibeDB execution boundaries. It does not introduce a new query language, database executor, authorization service or physical-engine API.

Canonical flow:

Context IR + per-source parameter envelope
→ trusted ExecutionContext
→ preflight every source
→ tenant-scoped Schema Catalog
→ Query IR / Retrieval IR validation
→ existing planner/compiler
→ existing Secure Execution / Retrieval Execution
→ bounded context result

## Security

- Context Resolution requires a trusted ExecutionContext.
- Every source is preflighted before Schema Catalog or database access.
- Tenant identity, credentials and capability grants cannot be supplied in Context IR.
- Query/schema sources require graph:read.
- Retrieval capability requirements are derived from nested Retrieval IR; vector retrieval requires vector:read.
- Parameters are bounded separately from Context IR.
- Aggregate item and byte budgets fail closed.
- records context resolution is unsupported in v1.
- Context Resolution is read-only; it cannot execute Mutation IR or grant authorization.

## Developer and agent convergence

SDK `ContextBuilder.resolve()`, REST `POST /v1/context/resolve`, and MCP `context.resolve` submit the same Context IR plus source parameter envelope to the same authenticated server boundary.

## Deferred

- generic records selection;
- LLM intent-to-IR translation;
- autonomous authorization;
- write-capable context workflows;
- context caching infrastructure beyond existing freshness semantics;
- any second query/retrieval executor.
