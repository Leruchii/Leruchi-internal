# Stage 27 — Agent Intent → VibeDB IR Boundary

Status: DECIDED / IMPLEMENTED — NOT YET VALIDATED

## Decision

VibeDB introduces a closed Agent Intent v1 envelope around one canonical VibeDB IR. The intent layer is provider-neutral and non-executing.

Canonical flow:

Agent / developer / future model adapter
→ Agent Intent
→ trusted ExecutionContext preflight
→ deterministic action ↔ IR-kind check
→ server-derived capability and safety semantics
→ canonical Query/Retrieval/Context/Mutation IR validation
→ bounded non-executing explanation

## Security

- Agent Intent cannot supply tenant identity, credentials or capability grants.
- Required capabilities are derived from the target IR.
- Bindings are bounded separately from authorization.
- Natural-language/model output remains untrusted input.
- Destructive mutation intent reports approval-required semantics but cannot execute in Stage 27.
- Capability denial occurs before Schema Catalog/database access.
- Diagnostics never record tenant IDs, bindings, raw target IR, credentials or catalog internals.

## Convergence

SDK, CLI, REST and MCP use the same authenticated Agent Intent explanation boundary.

## Deferred

- free-form natural-language interpretation;
- provider-specific LLM adapters;
- autonomous authorization or capability issuance;
- direct Agent Intent execution;
- any second Query/Retrieval/Context/Mutation executor.
