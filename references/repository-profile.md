# GPT2Git Repository Profile Protocol

This reference defines the portable repository-side protocol used by GPT2Git-Note.

The goal is simple: a knowledge repository initialized in one AI client should remain understandable and writable from another client later.

## Core Principle

The repository owns its long-term configuration.

Do not rely on chat history, client-local memory, or a ChatGPT-specific project state to know who the user is or how their knowledge should be organized.

Use:

```text
.gpt2git/
├── profile.yaml
├── taxonomy.yaml
└── config.yaml
```

These files are intentionally client-neutral so ChatGPT, Claude Code, Codex, or a future GPT2Git MCP server can reuse the same repository.

---

## First-run Detection

Before the first capture into a new repository:

1. inspect the repository for an established knowledge structure;
2. look for `.gpt2git/profile.yaml`;
3. if a coherent existing structure already exists, preserve it and create only the minimum GPT2Git metadata needed;
4. if no profile and no stable structure exist, run concise onboarding.

Do not re-run onboarding on every chat or every client.

---

## Onboarding

The onboarding goal is not to interview the user exhaustively. It is to gather enough context to route future knowledge safely.

A compact prompt can ask for:

- primary technical direction: backend, frontend, AI engineering, data engineering, DevOps/cloud, mobile, security, embedded, or other;
- languages actually used;
- major frameworks;
- databases / caches / messaging systems;
- AI / infrastructure tools that materially affect knowledge routing;
- optional learning preference such as interview-oriented notes.

Example user answer:

```text
Java 后端为主，也做 Agent。
Spring Boot、MySQL、Redis、Kafka，
Agent 这边主要 RAG、LangGraph、AgentScope。
面试复习内容希望保留面试回答。
```

That is enough. Do not force the user through a long questionnaire if a short free-form answer establishes the profile.

---

## profile.yaml

`profile.yaml` answers: **Who is this knowledge base for?**

Recommended shape:

```yaml
version: 1

profile:
  primary_domains:
    - backend
    - ai-engineering

  languages:
    - java

  frameworks:
    - spring-boot

  data:
    databases:
      - mysql
    caches:
      - redis
    messaging:
      - kafka

  ai:
    - rag
    - langgraph
    - agentscope

learning:
  interview_mode: true
  preserve_followups: true
  preserve_misconceptions: true
```

### Rules

- Store only information useful for knowledge routing or note behavior.
- Do not store unnecessary personal details.
- Do not store secrets, tokens, account identifiers, employer-private data, or sensitive personal information.
- Keep names canonical and lowercase where practical.
- Update the profile when the user's stack materially changes, not on every new topic.

---

## taxonomy.yaml

`taxonomy.yaml` answers: **How should concepts map into this repository?**

It is a routing hint layer, not a complete ontology.

Example:

```yaml
version: 1

root: knowledge

routes:
  java:
    domain: languages
    file: java.md

  spring:
    domain: frameworks
    file: spring.md

  mysql:
    domain: database
    file: mysql.md

  redis:
    domain: cache
    file: redis.md

  kafka:
    domain: messaging
    file: kafka.md

  rag:
    domain: ai
    file: rag.md

  agent:
    domain: ai
    file: agent.md

  mcp:
    domain: ai
    file: mcp.md
```

### Rules

- Prefer shallow stable domains.
- The taxonomy may contain routes whose files do not exist yet.
- A route is a future placement hint, not an instruction to eagerly create empty directories/files.
- Add new mappings only when a stable repeated concept appears.
- If the repository already has a coherent custom structure, taxonomy should describe that structure rather than migrate it automatically.

---

## config.yaml

`config.yaml` answers: **How should GPT2Git behave in this repository?**

Example:

```yaml
version: 1

capture:
  preserve_followups: true
  preserve_misconceptions: true
  interview_answer: auto

merge:
  prefer_existing_file: true
  one_file_per_question: false

structure:
  create_on_demand: true
  max_recommended_depth: 2

git:
  mode: direct-commit
```

Possible future git modes may include `pull-request`, but V0.2 does not require a PR workflow.

---

## Lazy Materialization

Profile and taxonomy initialization must not create a large empty knowledge tree.

Bad first-run behavior:

```text
knowledge/
├── languages/
├── frameworks/
├── database/
├── cache/
├── messaging/
├── distributed/
├── network/
├── operating-system/
├── ai/
├── devops/
├── algorithms/
└── ... many empty files
```

Preferred behavior:

```text
.gpt2git/
├── profile.yaml
├── taxonomy.yaml
└── config.yaml
```

Then the first real capture creates only what is needed:

```text
knowledge/frameworks/spring.md
```

Later captures grow the tree naturally:

```text
knowledge/
├── frameworks/
│   └── spring.md
├── database/
│   └── mysql.md
└── ai/
    └── agent.md
```

The knowledge tree should reflect actual learning, not hypothetical future interests.

---

## Client-neutral Contract

Repository correctness must not depend on a specific AI product.

A client may have different local instructions or tool names, but the repository protocol remains:

```text
Conversation
→ KnowledgeUnit
→ Resolve topic with profile + taxonomy + repository state
→ Search
→ CREATE / MERGE / NO-OP
→ Persist
→ Verify write
```

Do not create separate repository trees such as:

```text
chatgpt-notes/
claude-notes/
codex-notes/
```

The same knowledge base should evolve regardless of which compatible client performs the capture.

---

## Future MCP Boundary

The Skill-only V0.2 and a future MCP implementation must share this protocol.

Recommended responsibility split:

### Client / Skill reasoning layer

Responsible for:

- selecting the relevant conversation window;
- identifying the primary question;
- extracting follow-ups;
- identifying explicit misconceptions;
- producing the final mental model;
- constructing a KnowledgeUnit.

### Future GPT2Git MCP layer

May own repository-oriented operations such as:

```text
initialize_profile
get_profile
resolve_topic
search_knowledge
get_knowledge
capture_knowledge
get_related_topics
get_weak_points
```

The product-level MCP API should express knowledge semantics, not merely expose raw GitHub CRUD such as `create_file` and `update_file`.

GitHub can remain the first storage adapter, while the protocol leaves room for GitLab, local Git, Gitee, or self-hosted Git later.
