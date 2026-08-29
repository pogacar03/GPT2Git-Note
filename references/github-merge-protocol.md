# GitHub Merge Protocol

GitHub is the persistence layer. Treat every capture as a knowledge-maintenance operation, not as a file dump.

## Operation

```text
capture intent
→ identify topic
→ search repository
→ inspect strongest candidate
→ classify CREATE / MERGE / NO-OP
→ write
→ verify result
→ report
```

## Search Before Write

Search using several signals when available:

- canonical topic name;
- technical keywords;
- common synonyms;
- likely headings;
- closely related parent topic.

Filename similarity alone is not sufficient.

## CREATE

Use CREATE only when no existing file or section can absorb the knowledge cleanly.

Prefer a stable domain file such as:

```text
knowledge/java/spring.md
knowledge/database/mysql.md
knowledge/ai/agent.md
```

over one-file-per-chat naming such as:

```text
spring-transaction-2026-08-30.md
spring-transaction-2.md
spring-final.md
```

## MERGE

Use MERGE when the new conversation adds one or more of:

- a new follow-up question;
- a new explicit misconception and correction;
- a stronger causal explanation;
- a useful contrast or boundary condition;
- a new interview-ready formulation;
- a new related concept that belongs under the same stable topic.

### Merge invariants

- Keep useful old content.
- Remove true duplication.
- Resolve contradictions in favor of the better-supported explanation.
- Preserve distinct learner reasoning paths even when their final conclusion overlaps.
- Organize by concept, not by capture date.
- Do not append raw chat logs.

## NO-OP

Use NO-OP when the target already captures the same useful learning and the new conversation adds no meaningful reasoning, clarification, or correction.

Avoid meaningless commits for whitespace-only or wording-only changes unless the user explicitly requests editing.

## Existing Repository Structure

If the user already has a coherent taxonomy, follow it.

Example existing structure:

```text
notes/backend/spring.md
notes/backend/mysql.md
notes/ai/agents.md
```

Do not create a parallel `knowledge/` tree just because it is the default recommendation.

## Write Truthfulness

Only say a write succeeded after the GitHub action reports success.

Allowed after verified success:

```text
MERGE  notes/backend/spring.md
Commit: abc123...
```

Required after failure:

```text
未写入 GitHub：写操作失败。
已整理好内容，但不能声称已提交。
```

Never invent a commit SHA.

## Sensitive Data Filter

Before writing, exclude:

- API keys;
- access tokens;
- passwords;
- private keys;
- session secrets;
- accidental credentials;
- personally sensitive content that is not necessary to the technical knowledge.

Preserve the technical lesson with placeholders where needed, e.g. `<REDACTED_TOKEN>` only if the existence of a token is itself relevant.

## Commit Messages

Use semantic, topic-oriented messages:

```text
knowledge: add Spring transaction self-invocation
knowledge: deepen MySQL next-key lock notes
knowledge: capture Agent checkpoint follow-ups
```

Commit messages describe the knowledge change, not the chat session.
