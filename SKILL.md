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
7. **one Markdown file represents one interview/learning question; use the question itself as the filename**;
8. if the same question already exists, semantically MERGE into that question file instead of creating an aggregate topic file;
9. apply an explicit user-provided star importance prefix before writing or renaming;
10. write a durable knowledge artifact, not `User:` / `Assistant:` transcript blocks;
11. verify persistence before reporting success.

## Note Presentation

Captured knowledge should be optimized for later review, not only archival completeness.

- **One `.md` = one question.** Do not aggregate multiple questions into files such as `llm.md`, `mysql.md`, or `spring.md` when using question-note mode.
- Use the learner's actual question, or a stable cleaned-up version of it, as both the Markdown filename and the top-level topic heading.
- When `learning.interview_mode: true`, put a concise `### 面试回答` immediately after the topic heading so the note is useful for rapid interview review.
- Put detailed explanation, follow-up reasoning, misconceptions/corrections, evidence, and mental models after the interview answer.
- Prefer question-shaped filenames/headings because they match how interviewers ask and how learners retrieve knowledge.

## Importance Stars

When the user explicitly gives an importance level in stars, treat that as title metadata expressed through a visible prefix.

Rules:

- `1颗星` / `一颗星` → prefix `⭐`
- `2颗星` / `两颗星` → prefix `⭐⭐`
- `3颗星` / `三颗星` → prefix `⭐⭐⭐`
- Put the star prefix at the **very beginning of both the Markdown filename and the top-level question heading**.
- Do not insert spaces between the star prefix and the question in the filename.
- If the user changes the star level later, rename the existing question file and update its top-level heading instead of creating a duplicate note.
- Star prefixes are not part of semantic duplicate matching. `为什么长上下文中间的信息更容易丢？` and `⭐⭐⭐为什么长上下文中间的信息更容易丢？` are the same question for MERGE/NO-OP purposes.
- If the user does not specify an importance level, do not invent one.

Examples:

```text
knowledge/ai/⭐⭐⭐为什么长上下文中间的信息更容易丢？.md
knowledge/database/⭐SQLite和MySQL怎么选型？.md
```

```markdown
# ⭐Agent架构有哪些？
# ⭐⭐⭐为什么长上下文中间的信息更容易丢？
```

Recommended interview-note shape:

```markdown
# <⭐... + 面试问题原句或稳定的问题表达>

### 面试回答
> <可直接口述的简洁答案>

### 核心结论
...

### 追问 / 易错点 / 原理
...

### 一句话速记
...
```

Recommended path shape:

```text
knowledge/ai/为什么长上下文中间的信息更容易丢？.md
knowledge/database/MySQL为什么使用B+树而不是B树？.md
knowledge/frameworks/Spring事务为什么会自调用失效？.md
```

The directory provides broad taxonomy; the file provides the individual question.

## Knowledge Priority

When compression is necessary:

1. follow-ups + resolving answers;
2. misconceptions + corrections;
3. final mental model;
4. primary question + core answer;
5. supplementary examples.

This priority controls what knowledge survives compression; it does not dictate display order. In interview mode, the interview answer still appears first.

## Adaptive Structure

Route the **directory** by stable mechanism/domain, then store each question as its own Markdown file.

Prefer shallow domain directories such as:

```text
knowledge/frameworks/<question>.md
knowledge/database/<question>.md
knowledge/cache/<question>.md
knowledge/messaging/<question>.md
knowledge/ai/<question>.md
```

Avoid deep role-first paths such as `backend/middleware/cache/redis/...`.

If the repository already has a coherent custom taxonomy, preserve its directory taxonomy and describe it in `.gpt2git/taxonomy.yaml`; question-level files remain the default unit of knowledge.

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
- Never use an aggregate topic file when the intended knowledge unit is an individual interview/learning question.
- Never create a second note merely because the star prefix changed.

## Completion Report

After success:

```text
CREATE  knowledge/ai/⭐⭐⭐为什么长上下文中间的信息更容易丢？.md
Topic: ⭐⭐⭐为什么长上下文中间的信息更容易丢？
Commit: <verified commit reference>
```

For an existing question:

```text
MERGE  knowledge/database/⭐SQLite和MySQL怎么选型？.md
Topic: ⭐SQLite和MySQL怎么选型？
Commit: <verified commit reference>
```

For duplicate knowledge:

```text
NO-OP  <existing-question-file>.md
Reason: no meaningful new follow-up, misconception, or conclusion.
```

If persistence fails, say so plainly and do not fabricate success.
