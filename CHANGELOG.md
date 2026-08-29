# Changelog

All notable changes to GPT2Git-Note will be documented here.

## 0.1.0 — 2026-08-30

### Added

- Initial `gpt2git-note` skill.
- Follow-up-first `KnowledgeUnit` schema.
- Explicit misconception / correction handling.
- GitHub CREATE / MERGE / NO-OP persistence protocol.
- Developer knowledge taxonomy for Java, database, middleware, distributed systems, AI engineering, and algorithms.
- Spring transaction self-invocation capture example.
- Skill eval suite covering follow-up loss, invented misconceptions, duplicate-note explosion, raw transcript export, cross-topic contamination, write verification, and sensitive data handling.
- Initial architecture/design document.

### V1 Constraints

- No MCP server.
- No custom backend.
- No database or vector database.
- No background capture of arbitrary conversations.
- GitHub read/write capability is expected from the host runtime.
