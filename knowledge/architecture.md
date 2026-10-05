# VibePlatform Architecture

Status: DECIDED / Stage 26B IMPLEMENTED — NOT YET VALIDATED

## Core execution architecture
Developer/Agent
→ SDK / REST / CLI / Studio / MCP
→ engine-neutral IR
→ validation
→ trusted tenant/capability context
→ planner
→ Secure Execution
→ PostgreSQL / AGE / pgvector / RLS

## Agent-native context
Stage 26B adds Context IR as a declarative request contract:

Agent/Application
→ Context IR
→ server validation
→ authorization-aware non-executing explanation

Context IR can reference existing Query/Retrieval IR but does not execute them itself. It never accepts tenant identity, credentials, authorization grants, physical engine names or raw SQL/Cypher.

## Repository boundary
- vibeDB-internal: internal development control, strategy, agent rules and decisions.
- vibeDB-development: private implementation history and active engineering.
- vibeDB: public OSS release target, populated only through explicit allowlisted export.

Cloud/Enterprise must depend on Core, never the reverse.
