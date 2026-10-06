# Leruchi Architecture

Status: DECIDED / Stage 28 IMPLEMENTED — NOT YET VALIDATED

## Core execution
Developer/Agent
→ SDK / REST / CLI / Studio / MCP
→ canonical IR
→ validation
→ trusted tenant/capability context
→ planner
→ Secure Execution
→ PostgreSQL / AGE / pgvector / RLS

## Agent-native planning
Agent/application intent
→ Cross-Modal Plan IR
→ dependency validation
→ canonical target IR validation
→ capability derivation
→ non-executing explanation

Cross-Modal Plan may compose Query, Retrieval, Context and Mutation IR. It does not execute targets or grant authorization.

## Repository boundary
- Leruchi-internal: private engineering control.
- Leruchi-development: private implementation.
- Leruchi: public OSS release target via explicit allowlisted export.

Cloud/Enterprise must depend on Core, never the reverse.
