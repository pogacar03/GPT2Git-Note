# Changelog

All notable changes to GPT2Git-Note will be documented here.

## 0.2.0 — 2026-08-30

### Added

- Portable repository-side `.gpt2git` protocol:
  - `profile.yaml` — user technical profile and learning preferences;
  - `taxonomy.yaml` — stable concept-to-path routing hints;
  - `config.yaml` — capture, merge, structure, and Git behavior.
- Concise first-run onboarding for new knowledge repositories.
- Profile reuse across chats and compatible AI clients.
- Lazy knowledge-tree materialization: directories and files are created only when real captured knowledge needs them.
- Cross-client contract for ChatGPT, Claude Code, Codex, and future compatible runtimes.
- Future MCP responsibility boundary that reuses the same KnowledgeUnit and repository protocol.
- New evals for onboarding, lazy growth, adaptive routing, cross-client portability, and MCP compatibility.

### Changed

- Replaced the Java/backend-biased fixed taxonomy with an adaptive, shallow, mechanism-oriented taxonomy.
- Routing now prefers stable domains such as `frameworks`, `database`, `cache`, `messaging`, `distributed`, `networking`, `ai`, and `devops` instead of deep role-first trees.
- `SKILL.md` now checks and reuses `.gpt2git/profile.yaml` before capture and avoids repeated onboarding across clients.

### Not Yet Implemented

- GPT2Git MCP server.
- Non-GitHub storage adapters.
- Automatic background capture.

The V0.2 protocol is intentionally designed so these can be added without changing the KnowledgeUnit model.

---

## 0.1.0 — 2026-08-30

### Added

- Initial `gpt2git-note` skill.
- Follow-up-first `KnowledgeUnit` schema.
- Explicit misconception / correction handling.
- GitHub CREATE / MERGE / NO-OP persistence protocol.
- Initial developer knowledge taxonomy.
- Spring transaction self-invocation capture example.
- Skill eval suite covering follow-up loss, invented misconceptions, duplicate-note explosion, raw transcript export, cross-topic contamination, write verification, and sensitive data handling.
- Initial architecture/design document.

### V0.1 Constraints

- No MCP server.
- No custom backend.
- No database or vector database.
- No background capture of arbitrary conversations.
- GitHub read/write capability is expected from the host runtime.
