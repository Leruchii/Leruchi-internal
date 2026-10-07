# Leruchi Architecture

Status: DECIDED / Stage 32 IMPLEMENTED — SECURITY HARDENING IN PROGRESS

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
- Leruchi-internal: architecture/release control and durable engineering handoff.
- Leruchi-development: implementation, tests, CI and controlled publication candidate.
- Leruchi: public OSS release target via explicit allowlisted export.

Repository visibility is an operational GitHub setting and is not itself the OSS boundary. The public repository must receive only the reviewed, sanitized export candidate.

Cloud/Enterprise must depend on Core, never the reverse.
