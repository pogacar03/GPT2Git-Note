---
name: gpt2git-note
description: Use when a developer wants to capture, save, merge, or maintain useful learning from an AI conversation in a GitHub knowledge repository, especially after technical Q&A with follow-up questions, corrected misunderstandings, interview preparation, or repeated discussion of an existing topic.
---

# GPT2Git-Note

## Overview

Turn developer learning conversations into a Git-native knowledge base.

The core principle is:

> **Preserve the learning path, not the raw transcript.**

A useful note keeps the primary question, the core explanation, the user's meaningful follow-up questions, any explicitly expressed misconception and correction, and the final mental model. For interview-oriented backend topics, also keep a concise interview-ready answer.

## Trigger

Use this skill when the user expresses capture intent such as:

- `收录`
- `记一下`
- `整理到知识库`
- `存到 GitHub`
- `更新我的笔记`
- `把刚才这段并进去`

Do not silently persist ordinary conversations without user intent.

## Required References

Load only what the task needs:

| Need | Read |
|---|---|
| Decide note shape and what must be preserved | `references/knowledge-schema.md` |
| Search, CREATE / MERGE / NO-OP, write and commit behavior | `references/github-merge-protocol.md` |
| Choose a reasonable developer topic location | `references/developer-taxonomy.md` |

## Capture Contract

When capture intent occurs:

1. **Select the relevant learning window.** Use the current topic and its meaningful follow-ups; do not drag unrelated earlier conversation into the note.
2. **Extract the learning structure.** Identify the primary question, core answer, follow-ups, explicit misconceptions, corrections, and final mental model.
3. **Treat follow-ups as first-class knowledge.** They are not expendable commentary. Preserve their wording faithfully enough that the learner can later remember why they were confused.
4. **Do not invent misconceptions.** Only record a wrong model when the user's words actually support it.
5. **Search GitHub before writing.** Decide whether the new knowledge should CREATE, MERGE, or NO-OP.
6. **Prefer semantic merge over note proliferation.** Deepen an existing coherent topic instead of creating `topic-2.md`, `final.md`, or date-based duplicates.
7. **Write a clean knowledge artifact, not a transcript.** Remove filler, repetition, acknowledgements, and conversational noise while preserving the reasoning path.
8. **Verify persistence.** Only report a successful GitHub update or commit after the write action succeeds.

## Knowledge Priority

When compression is necessary, preserve in this order:

1. meaningful user follow-up questions and their resolving answers;
2. explicit misconceptions and corrections;
3. final mental model;
4. primary question and core answer;
5. supplementary examples.

## Output Shape

A captured topic normally contains:

```markdown
# Topic

## 1. 原始问题

## 2. 核心回答

## 3. 我的追问 ⭐
### 追问 1
### 追问 2

## 4. 我曾经的错误理解

## 5. 最终心智模型

## 6. 面试回答

## 7. 相关知识
```

Omit sections that genuinely do not apply. If meaningful follow-ups occurred, `我的追问` is required. `面试回答` is conditional on interview relevance.

## Hard Rules

- **Never raw-export the conversation as the knowledge note.**
- **Never discard meaningful follow-ups just because the final answer already contains the conclusion.**
- **Never invent a misconception, user experience, metric, production incident, or historical fact.**
- **Never persist secrets, credentials, tokens, private keys, or passwords from the conversation.**
- **Never overwrite unrelated knowledge while merging.**
- **Never claim `已提交` / `已更新` without a successful GitHub write result.**
- **Never force the default taxonomy onto a repository that already has a coherent structure.**

## Completion Report

After a successful capture, report only the useful result:

```text
MERGE  knowledge/java/spring.md
Topic: Spring 事务自调用
Commit: <verified commit sha or available commit reference>
```

For a no-op:

```text
NO-OP  knowledge/database/mysql.md
Reason: 本次问答没有增加新的追问、误区或结论。
```

If persistence fails, say so plainly and do not fabricate success.
