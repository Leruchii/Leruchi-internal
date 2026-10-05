# Stage 26A — OSS Product Boundary

Status: DECIDED

VibeDB Core is designed as a self-hostable open-source product. Portable developer and agent capabilities belong in Core; hosted infrastructure, commercial operations and enterprise control-plane capabilities remain outside Core.

Allowed dependency direction:
VibeDB Cloud / Enterprise → VibeDB Core

Forbidden:
VibeDB Core → private Cloud / Enterprise services

Every new component must be classified before implementation as OSS_CORE, OSS_ADAPTER, CLOUD_PRIVATE, ENTERPRISE_PRIVATE or UNKNOWN. UNKNOWN requires an explicit architecture decision.

The public repository must remain useful for local development, testing, CLI/SDK/API usage and safe MCP usage without Cloud access.

This decision does not define pricing, final Cloud features or enterprise packaging.
