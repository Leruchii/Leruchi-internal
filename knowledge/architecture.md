# VibePlatform Architecture

> Canonical architecture/build plan is maintained in the private VibeDB development control repository.

Status: DECIDED design / Stage 25 VALIDATED

## Public architecture contract
VibeDB provides a developer-first, agent-native database layer over PostgreSQL, Apache AGE and pgvector.

Trusted request context
→ Query/Retrieval/Mutation IR
→ schema/security/cost validation
→ planner/compiler
→ Secure Execution
→ PostgreSQL/AGE/pgvector/RLS
→ normalized result

Client surfaces must not bypass these boundaries.

## Developer and agent priority
SDK, REST/API, CLI, Studio and MCP converge on the same engine-neutral contracts. MCP is not privileged; tenant identity, capabilities, validation, mutation approval, RLS and secure execution remain server-authoritative.

## OSS boundary
The self-hostable Core contains portable database, developer and agent capabilities. Hosted infrastructure, billing, managed operations and enterprise control-plane services are outside Core.

## Engine neutrality
Apache AGE, PostgreSQL recursive execution and pgvector are implementation capabilities behind VibeDB contracts. Clients should express intent rather than select physical execution engines directly.

## Security
PostgreSQL/RLS remains authoritative for tenant isolation. Raw database execution is not a normal client contract. Destructive agent operations require explicit safety policy and auditability.

## Stage 26A
The repository model is OSS-first: Core must remain independently useful and must never depend on private Cloud or Enterprise services.
