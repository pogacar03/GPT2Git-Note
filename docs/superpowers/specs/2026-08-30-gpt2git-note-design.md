# GPT2Git-Note Design

## Product

GPT2Git-Note is a lightweight Agent Skill for developers who learn through AI conversations and want the useful parts of those conversations to become a durable Git-native knowledge base.

It is not a raw chat exporter and not an Obsidian replacement. It compiles a learning conversation into an evolving knowledge unit and uses the user's GitHub repository as the persistence layer.

> Preserve not only the final answer, but the learner's questions, follow-up questions, misconceptions, corrections, and final mental model.

## V1 Scope

V1 is Skill-only. It does not add an MCP server, custom backend, database, vector database, browser extension, or background ingestion service.

The runtime is expected to already have GitHub read/write capability.

## Capture Trigger

Primary triggers include:

- 收录 / 记一下 / 整理到知识库
- 把刚才这个问题存到 GitHub
- 更新我的后端笔记
- 把这段问答并入已有知识点

The Skill must not silently persist every conversation. Explicit capture is the V1 default.

## Knowledge Unit

```text
KnowledgeUnit
├── Topic
├── Domain
├── PrimaryQuestion
├── CoreAnswer
├── FollowUps[]
│   ├── Question
│   ├── WhyItMatters
│   └── Answer
├── Misconceptions[]
│   ├── WrongModel
│   └── Correction
├── FinalMentalModel
├── InterviewAnswer (when relevant)
├── RelatedTopics[]
└── UpdatedAt
```

### Priority

When compression is necessary, preserve in this order:

1. user follow-up questions and the answers that resolved them;
2. misconceptions and corrections;
3. final mental model;
4. primary question and core answer;
5. supplementary examples.

Never flatten a multi-turn learning path into a generic encyclopedia entry if doing so discards valuable user reasoning.

## Markdown Shape

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

Sections that do not apply may be omitted. `我的追问` must be retained whenever meaningful follow-ups occurred.

## GitHub Persistence

```text
capture intent
→ extract KnowledgeUnit
→ search repository
→ CREATE / MERGE / NO-OP
→ read target when merging
→ produce complete updated Markdown
→ write through GitHub capability
→ commit
```

### CREATE
No sufficiently overlapping topic exists.

### MERGE
The new conversation deepens an existing topic. Merge semantically; do not append a raw transcript.

### NO-OP
The repository already contains the same useful knowledge and the new conversation adds no meaningful clarification.

## Default Repository Taxonomy

```text
knowledge/
├── java/
│   ├── spring.md
│   ├── jvm.md
│   ├── juc.md
│   └── java-base.md
├── database/
│   ├── mysql.md
│   └── redis.md
├── middleware/
│   └── kafka.md
├── distributed-system/
│   └── fundamentals.md
├── ai/
│   ├── rag.md
│   ├── agent.md
│   └── sandbox.md
└── algorithm/
    └── patterns.md
```

Prefer an existing coherent repository structure over forcing this taxonomy.

## Merge Rules

Before writing:

1. search by topic, keywords, synonyms, and likely headings;
2. inspect the strongest candidate;
3. distinguish duplicate knowledge from genuine conceptual deepening;
4. preserve a distinct follow-up path when it exposes a new misconception or reasoning gap;
5. avoid duplicate headings and repeated definitions;
6. organize by topic, not chronological transcript order.

## Commit Policy

Personal knowledge repositories default to direct commits.

Examples:

- `knowledge: add Spring transaction self-invocation`
- `knowledge: deepen MySQL next-key lock notes`
- `knowledge: capture Agent checkpoint follow-ups`

## Safety

- Never claim a commit succeeded without a successful GitHub write.
- Never delete unrelated knowledge during merge.
- Never invent misconceptions or user experience.
- Never persist secrets, credentials, tokens, or private keys.
- If write access is unavailable, prepare the knowledge unit but clearly state that persistence did not occur.

## V1 Success Criteria

A developer can learn normally, ask several follow-ups, say `收录` once, and get a GitHub update that preserves both the concept and why they previously got stuck. Later conversations about the same concept deepen the existing knowledge rather than creating duplicate notes.
