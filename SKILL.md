---
name: gpt2git-note
description: Use when a developer wants to initialize, capture, save, merge, or maintain useful learning from AI conversations in a Git-backed knowledge repository, especially after technical Q&A with follow-up questions, corrected misunderstandings, interview preparation, or repeated discussion of an existing topic.
---

# GPT2Git-Note

## Overview

Turn developer learning conversations into a portable Git-native knowledge base.

> **Preserve the learning path, not the raw transcript.**

Follow-ups, explicit misconceptions, corrections, and the final mental model are first-class knowledge.

## Required References

Load only what is needed:

| Need | Read |
|---|---|
| First-run onboarding, `.gpt2git` files, cross-client rules | `references/repository-profile.md` |
| KnowledgeUnit shape | `references/knowledge-schema.md` |
| CREATE / MERGE / NO-OP and verified persistence | `references/github-merge-protocol.md` |
| Adaptive topic routing | `references/developer-taxonomy.md` |

## First Run

Before the first capture into a target repository:

1. inspect its existing structure;
2. check for `.gpt2git/profile.yaml`;
3. if a coherent structure/profile exists, reuse it;
4. otherwise run concise onboarding: ask the user's primary technical direction and actual stack at a useful level;
5. initialize client-neutral `.gpt2git/profile.yaml`, `taxonomy.yaml`, and `config.yaml` when write capability exists.

**Do not create a large empty knowledge tree.** Knowledge paths materialize only when captured topics need them.

Do not re-onboard merely because the chat or AI client changed.

## Capture Contract

When the user expresses capture intent such as `收录`, `记一下`, `存到 GitHub`, or `把刚才这段并进去`:

1. select only the relevant learning window;
2. extract primary question, core answer, meaningful follow-ups, explicit misconceptions/corrections, and final mental model;
3. preserve follow-ups faithfully enough to recover why the learner was confused;
4. never invent a misconception;
5. resolve the destination from repository state + `.gpt2git` profile/taxonomy + current mechanism;
6. search before writing and choose CREATE / MERGE / NO-OP;
7. prefer semantic merge and stable files over one-file-per-question;
8. write a durable knowledge artifact, not `User:` / `Assistant:` transcript blocks;
9. verify persistence before reporting success.

## Knowledge Priority

When compression is necessary:

1. follow-ups + resolving answers;
2. misconceptions + corrections;
3. final mental model;
4. primary question + core answer;
5. supplementary examples.

## Adaptive Structure

Route by stable mechanism, not job title.

Prefer shallow homes such as:

```text
knowledge/frameworks/spring.md
knowledge/database/mysql.md
knowledge/cache/redis.md
knowledge/messaging/kafka.md
knowledge/ai/agent.md
```

Avoid deep role-first paths such as `backend/middleware/cache/redis/...`.

If the repository already has a coherent custom taxonomy, preserve it and describe it in `.gpt2git/taxonomy.yaml` rather than migrating it automatically.

## Portable Protocol

Repository correctness must not depend on ChatGPT-specific state.

The same repository should remain usable from ChatGPT, Claude Code, Codex, or a future GPT2Git MCP implementation by reusing:

```text
.gpt2git/profile.yaml
.gpt2git/taxonomy.yaml
.gpt2git/config.yaml
KnowledgeUnit semantics
CREATE / MERGE / NO-OP
```

The current Skill is the reasoning layer. A future MCP may own profile retrieval, topic resolution, repository search/merge, persistence, and commit verification without redefining the protocol.

## Hard Rules

- Never silently persist ordinary conversations without user intent.
- Never raw-export the conversation as the note.
- Never discard meaningful follow-ups.
- Never invent misconceptions, experience, metrics, incidents, or facts.
- Never persist secrets or credentials.
- Never overwrite unrelated knowledge while merging.
- Never claim `已提交` / `已更新` without a successful write result.
- Never eagerly create dozens of empty directories or placeholder files.
- Never create separate knowledge trees for ChatGPT / Claude / Codex.

## Completion Report

After success:

```text
MERGE  knowledge/frameworks/spring.md
Topic: Spring 事务自调用
Commit: <verified commit reference>
```

For duplicate knowledge:

```text
NO-OP  knowledge/database/mysql.md
Reason: no meaningful new follow-up, misconception, or conclusion.
```

If persistence fails, say so plainly and do not fabricate success.
