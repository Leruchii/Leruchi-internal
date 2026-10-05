# Stage 28 — Cross-Modal Planning

Status: IMPLEMENTED — NOT YET VALIDATED

Cross-Modal Plan v1 is a bounded, engine-neutral composition contract. It sequences canonical Query, Retrieval, Context and Mutation IR steps using explicit dependencies.

Security invariants:
- trusted ExecutionContext is required;
- capability requirements are derived from target IR;
- plans cannot carry tenant identity, credentials or capability grants;
- dependency graphs are bounded and acyclic;
- mutation steps are descriptive and approval-aware but non-executing;
- observability is sanitized;
- plan explanation never executes a target.

Future execution must delegate each step to the existing authoritative Query/Retrieval/Context/Mutation execution path. Cross-Modal Planning must never become a second executor.
