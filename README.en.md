<div align="center">

# 🧠 GPT2Git-Note

### Turn AI conversations into a portable, Git-native developer learning memory

**Chat → Follow-up → Misconception → Mental Model → Git**

<p>
  <img src="https://img.shields.io/badge/Agent-Skill-111827?style=for-the-badge" alt="Agent Skill" />
  <img src="https://img.shields.io/badge/Storage-GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
  <img src="https://img.shields.io/badge/Taxonomy-Adaptive-2563EB?style=for-the-badge" alt="Adaptive Taxonomy" />
  <img src="https://img.shields.io/badge/version-0.2.0-7C3AED?style=for-the-badge" alt="Version" />
</p>

**Don't save the chat. Save how you learned.**

**English** · [简体中文](README.md)

</div>

---

## ✨ What is GPT2Git-Note?

GPT2Git-Note is an Agent Skill for developers who learn through AI conversations.

Ask about Java, databases, distributed systems, Agents, RAG, frontend, DevOps, or any other technical topic. Keep following up until the concept actually clicks. Then say:

```text
capture this
```

or simply:

```text
收录
```

GPT2Git-Note compiles that learning loop into durable knowledge and writes it to your Git repository.

It stores a structured learning unit instead of a transcript:

```text
KnowledgeUnit
├── Primary Question
├── Core Answer
├── Follow-ups[]          ⭐
├── Misconceptions[]      ⭐
├── Corrections[]
├── Final Mental Model
├── Interview Answer?
└── Related Topics[]
```

> The most personal part of your knowledge is often not the final answer, but the exact point where you got stuck.

---

## 🆕 V0.2: No More Hard-coded Directory Tree

V0.2 replaces the original backend-biased taxonomy with **Profile-driven + Adaptive Taxonomy**.

On first use in a new knowledge repository, GPT2Git-Note learns just enough about the user to route future knowledge safely:

```text
What do you primarily work on?
Which languages / frameworks / databases / middleware / AI tools do you actually use?
Do you have special learning preferences such as interview-ready answers?
```

Example:

```text
Mostly Java backend, plus Agent engineering.
Spring Boot, MySQL, Redis, Kafka,
RAG, LangGraph, and AgentScope.
```

GPT2Git then initializes a portable repository protocol:

```text
.gpt2git/
├── profile.yaml
├── taxonomy.yaml
└── config.yaml
```

These files belong to the repository, not to a specific AI client.

---

## 🌱 The Knowledge Tree Grows Lazily

Saying “I am a backend developer” should not create dozens of empty folders.

### ❌ Not this

```text
knowledge/
├── java/
├── jvm/
├── juc/
├── spring/
├── mysql/
├── redis/
├── kafka/
├── network/
├── os/
├── kubernetes/
└── ...
```

### ✅ Start with protocol only

```text
.gpt2git/
├── profile.yaml
├── taxonomy.yaml
└── config.yaml
```

First captured Spring topic:

```text
knowledge/frameworks/spring.md
```

Later, after learning Kafka:

```text
knowledge/
├── frameworks/
│   └── spring.md
└── messaging/
    └── kafka.md
```

**The repository grows from real learning, not hypothetical future interests.**

---

## 🧭 Route by Mechanism, Not Job Title

GPT2Git prefers shallow, stable knowledge domains:

```text
knowledge/
├── languages/
├── frameworks/
├── database/
├── cache/
├── messaging/
├── distributed/
├── networking/
├── operating-system/
├── ai/
├── devops/
├── security/
└── algorithms/
```

These are possible domains, not folders that must exist at initialization.

Examples:

```text
Spring  → frameworks/spring.md
MySQL   → database/mysql.md
Redis   → cache/redis.md
Kafka   → messaging/kafka.md
TCP     → networking/network.md
RAG     → ai/rag.md
Agent   → ai/agent.md
```

Avoid deep role-first paths such as:

```text
backend/middleware/cache/redis/cache-breakdown.md
```

because the mechanism remains stable even if the user's role changes later.

---

## ⭐ Follow-ups Are First-class Data

Most AI note tools optimize for:

```text
Question → Final Answer
```

GPT2Git-Note preserves:

```text
Question
   ↓
Initial Answer
   ↓
Follow-up ⭐
   ↓
Misconception exposed ⭐
   ↓
Clarification
   ↓
Another Follow-up ⭐
   ↓
Final Mental Model
```

When compression is necessary, priority is:

```text
1. Follow-ups + resolving answers
2. Misconceptions + corrections
3. Final mental model
4. Primary question + core answer
5. Supplementary examples
```

---

## 🔀 Git-native Merge

GPT2Git-Note does not default to one file per conversation.

Before writing, it searches and decides:

| Operation | Meaning |
|---|---|
| **CREATE** | No suitable existing knowledge can absorb the topic |
| **MERGE** | The conversation deepens existing knowledge |
| **NO-OP** | No meaningful new knowledge was added |

Bad:

```text
spring-transaction.md
spring-transaction-2.md
spring-final.md
```

Preferred:

```text
knowledge/frameworks/spring.md

## Spring Transactions
├── Self-invocation
├── rollbackFor
└── Propagation
```

Git history becomes part of knowledge evolution:

```text
knowledge: add Spring transaction basics
knowledge: capture proxy-vs-target follow-up
knowledge: clarify self-invocation mental model
```

---

## ⚡ Quick Start

### First use

Initialize the target knowledge repository with a short profile:

```text
I want to use GPT2Git-Note for this repository.
I mainly work on Java backend and Agent systems:
Spring Boot, MySQL, Redis, Kafka, LangGraph, AgentScope.
```

The Skill initializes `.gpt2git` metadata instead of creating a large empty note tree.

### Learn normally

```text
Why does Spring transaction self-invocation fail?
```

Keep asking follow-ups until the mental model is clear.

Then:

```text
capture this
```

Flow:

```text
Conversation
    ↓
KnowledgeUnit
    ↓
Profile + Taxonomy
    ↓
Search existing knowledge
    ↓
CREATE / MERGE / NO-OP
    ↓
Verified Git commit
```

---

## 🔌 Why MCP Later?

V0.2 is still **Skill-first**:

```text
AI Client
   ↓
GPT2Git Skill
   ↓
Git / GitHub capability
```

But the repository protocol is intentionally client-neutral:

```text
ChatGPT
Claude Code
Codex
Other Agents
     │
     ↓
.gpt2git + KnowledgeUnit
     │
     ↓
One Knowledge Repository
```

A future GPT2Git MCP server can provide shared knowledge infrastructure such as:

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

rather than merely exposing raw GitHub CRUD.

Responsibility split:

```text
Skill / Client Reasoning
├── Understand conversation
├── Extract follow-ups
├── Extract explicit misconceptions
└── Build KnowledgeUnit

Future GPT2Git MCP
├── Read profile
├── Resolve topic
├── Search / merge
├── Persist
└── Verify commit
```

**The MCP server is not implemented yet. V0.2 stabilizes the protocol first.**

---

## 📦 Installation

### Generic Agent Skills

```bash
git clone https://github.com/pogacar03/GPT2Git-Note.git \
  ~/.agents/skills/gpt2git-note
```

### Claude Code-style

```bash
git clone https://github.com/pogacar03/GPT2Git-Note.git \
  ~/.claude/skills/gpt2git-note
```

Other runtimes can load `SKILL.md` as project/agent instructions and provide compatible Git/GitHub read/write actions.

---

## 🏗️ Repository Structure

```text
GPT2Git-Note/
├── SKILL.md
├── references/
│   ├── repository-profile.md       # Profile / taxonomy / config protocol
│   ├── knowledge-schema.md         # KnowledgeUnit
│   ├── github-merge-protocol.md    # CREATE / MERGE / NO-OP
│   └── developer-taxonomy.md       # Adaptive routing
├── examples/
├── tests/
│   └── skill-evals.md
├── docs/
├── CHANGELOG.md
└── README.md
```

---

## 🛡️ Hard Rules

GPT2Git-Note must never:

- raw-export the chat as the final note;
- invent misconceptions the user never expressed;
- discard meaningful follow-ups;
- create one Markdown file per conversation by default;
- eagerly create dozens of empty directories;
- fork one knowledge base into ChatGPT / Claude / Codex copies;
- claim a Git write succeeded when it did not;
- persist passwords, tokens, private keys, or secrets.

---

## 🗺️ Roadmap

### V0.2 — Adaptive Knowledge Repository ✅

- [x] Follow-up-first KnowledgeUnit
- [x] CREATE / MERGE / NO-OP
- [x] `.gpt2git/profile.yaml`
- [x] Adaptive Taxonomy
- [x] Lazy directory growth
- [x] Cross-client repository protocol

### V0.3

- [ ] Profile update / migration rules
- [ ] Weak-point / mastery tracking
- [ ] Review mode from stored misconceptions
- [ ] Related-topic linking

### V1

- [ ] GPT2Git MCP Server
- [ ] ChatGPT / Claude Code / Codex adapters
- [ ] PR mode for shared knowledge bases
- [ ] GitLab / Local Git storage adapters

---

<div align="center">

## 🧠 GPT2Git-Note

### Don't save the chat. Save how you learned.

**Your knowledge. Your Git. Any AI client.**

</div>
