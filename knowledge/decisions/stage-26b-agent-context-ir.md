# Stage 26B — Agent-Native Context IR

Status: DECIDED / IMPLEMENTED — NOT YET VALIDATED

Context IR is the declarative contract for agent/application context needs. It is deliberately separate from execution and authorization.

## Contract
Context IR v1 declares purpose, bounded sources, output budget and freshness. It may reference existing Query/Retrieval IR but cannot carry tenant identity, credentials, authorization grants, physical engine names or raw SQL/Cypher.

## Security
The authenticated server context remains authoritative for tenant identity and capabilities. Context explanation is non-executing and returns bounded, engine-neutral diagnostics.

## Convergence
SDK and MCP submit the same Context IR to the authenticated API boundary. MCP safety annotations describe behavior but do not grant authorization.

## Deferred
Actual context execution, context ranking/compression, autonomous authorization and LLM-based intent translation are deferred until this contract is validated and the next architecture stage is explicitly started.
