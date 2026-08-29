# Adaptive Taxonomy V0.2 Implementation Plan

> **For agentic workers:** implement task-by-task and verify repository state after each write.

**Goal:** Upgrade GPT2Git-Note from a fixed developer taxonomy to a profile-driven, lazily growing knowledge repository protocol that can later be shared by ChatGPT, Claude Code, Codex, and MCP clients.

**Architecture:** The Skill remains the reasoning layer. `.gpt2git/profile.yaml`, `.gpt2git/taxonomy.yaml`, and `.gpt2git/config.yaml` become portable repository-side protocol files. Knowledge directories are created on demand rather than pre-populated. A future MCP layer will consume the same protocol instead of redefining it.

**Tech Stack:** Agent Skill Markdown, YAML protocol examples, GitHub persistence.

**Spec:** `docs/superpowers/specs/2026-08-30-gpt2git-note-design.md` plus the V0.2 adaptive-taxonomy design approved in conversation.

## Global Constraints

- Preserve follow-up questions as first-class knowledge.
- Never invent misconceptions.
- Prefer MERGE over duplicate note creation.
- Never claim persistence without a verified write.
- Do not require MCP for V0.2.
- Keep repository protocol client-neutral.

---

### Task 1: Extend behavioral evals

**Files:**
- Modify: `tests/skill-evals.md`

Add scenarios for first-run onboarding, minimal directory creation, profile reuse, lazy taxonomy growth, and cross-client portability.

### Task 2: Add repository protocol reference

**Files:**
- Create: `references/repository-profile.md`

Define `.gpt2git/profile.yaml`, `.gpt2git/taxonomy.yaml`, `.gpt2git/config.yaml`, initialization rules, and client-neutral semantics.

### Task 3: Convert taxonomy to adaptive routing

**Files:**
- Modify: `references/developer-taxonomy.md`

Replace the Java/backend-biased static tree with shallow stable domains, profile hints, and lazy materialization rules.

### Task 4: Update Skill behavior

**Files:**
- Modify: `SKILL.md`

Add first-run detection, onboarding, profile reuse, adaptive path resolution, and explicit separation between Skill reasoning and future MCP persistence.

### Task 5: Update public docs

**Files:**
- Modify: `README.md`
- Modify: `README.zh-CN.md`
- Modify: `CHANGELOG.md`

Document adaptive initialization and future multi-client/MCP architecture without claiming MCP is already implemented.

### Task 6: Verify

Read back all modified files and confirm:
- first run checks `.gpt2git/profile.yaml`;
- onboarding asks only information needed to build the initial profile;
- no eager creation of dozens of empty knowledge folders;
- paths grow on demand;
- protocol files are client-neutral;
- README clearly distinguishes current Skill from future MCP.
