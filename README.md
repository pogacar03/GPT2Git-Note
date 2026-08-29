<div align="center">

# 🧠 GPT2Git-Note

### 把 AI 对话变成可跨客户端演化的 Git-native 开发者知识库

**对话 → 追问 → 误区 → 心智模型 → Git**

<p>
  <img src="https://img.shields.io/badge/Agent-Skill-111827?style=for-the-badge" alt="Agent Skill" />
  <img src="https://img.shields.io/badge/Storage-GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
  <img src="https://img.shields.io/badge/Taxonomy-Adaptive-2563EB?style=for-the-badge" alt="Adaptive Taxonomy" />
  <img src="https://img.shields.io/badge/version-0.2.0-7C3AED?style=for-the-badge" alt="Version" />
</p>

**不要保存聊天。保存你是怎么学会的。**

[English](README.en.md) · **简体中文**

</div>

---

## ✨ GPT2Git-Note 是什么？

GPT2Git-Note 是一个面向开发者学习场景的 Agent Skill。

你像平时一样问 Java、数据库、分布式系统、Agent、RAG、前端、DevOps 或其他技术问题，不断追问，直到真正理解。最后只需要说：

```text
收录
```

GPT2Git-Note 会把这段学习过程整理成可长期维护的知识，并写入你的 Git 仓库。

它保存的不是聊天流水，而是：

```text
KnowledgeUnit
├── 原始问题
├── 核心回答
├── 关键追问[]          ⭐
├── 错误理解[]          ⭐
├── 纠正过程[]
├── 最终心智模型
├── 面试回答?
└── 相关知识[]
```

> 真正属于你的知识，不只是“答案”，而是你到底在哪一步卡住过。

---

## 🆕 V0.2：目录不再写死

早期版本给了一个偏 Java / 后端的默认目录。V0.2 改成了 **Profile-driven + Adaptive Taxonomy**。

第一次接入一个新的知识仓库时，GPT2Git-Note 会先了解：

```text
你主要做什么方向？
你实际使用哪些语言 / 框架 / 数据库 / 中间件 / AI 工具？
有没有面试复习等特殊偏好？
```

例如：

```text
Java 后端为主，也做 Agent。
Spring Boot、MySQL、Redis、Kafka，
Agent 这边主要 RAG、LangGraph、AgentScope。
```

然后生成可复用的仓库协议：

```text
.gpt2git/
├── profile.yaml
├── taxonomy.yaml
└── config.yaml
```

### `profile.yaml`

记录“这个知识库主要服务谁、常用什么技术”。

### `taxonomy.yaml`

记录“某类知识应该落到哪里”。

### `config.yaml`

记录“GPT2Git 应该怎么收录、合并和提交”。

这些配置属于仓库，而不是某个聊天客户端。

---

## 🌱 目录按知识实际生长

用户说自己是“后端”，并不意味着初始化时就创建几十个空目录。

### ❌ 不做这个

```text
knowledge/
├── java/
├── jvm/
├── juc/
├── spring/
├── mysql/
├── redis/
├── kafka/
├── rocketmq/
├── network/
├── os/
├── kubernetes/
└── ...
```

### ✅ 先只初始化协议

```text
.gpt2git/
├── profile.yaml
├── taxonomy.yaml
└── config.yaml
```

第一次真正收录 Spring 事务：

```text
knowledge/frameworks/spring.md
```

之后学 Kafka：

```text
knowledge/
├── frameworks/
│   └── spring.md
└── messaging/
    └── kafka.md
```

**知识树由真实学习记录长出来，而不是由“职业标签”预先填满。**

---

## 🧭 按机制分类，而不是按职位套娃

GPT2Git-Note 默认偏好浅层、稳定的知识域：

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

这些只是可能出现的领域，不是初始化时必须创建的目录。

例如：

```text
Spring      → frameworks/spring.md
MySQL       → database/mysql.md
Redis       → cache/redis.md
Kafka       → messaging/kafka.md
TCP         → networking/network.md
RAG         → ai/rag.md
Agent       → ai/agent.md
```

避免：

```text
backend/middleware/cache/redis/cache-breakdown.md
```

因为 Redis 的机制不会因为用户以后从后端转到 AI 平台就改变。

---

## 🔥 为什么“追问”是一等数据？

传统 AI 笔记更像：

```text
Question → Final Answer
```

GPT2Git-Note 更关注：

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

当必须压缩时，优先级是：

```text
1. 关键追问 + 解决这些追问的回答
2. 错误理解 + 纠正
3. 最终心智模型
4. 原始问题 + 核心回答
5. 补充示例
```

---

## 🔀 Git-native Merge

GPT2Git-Note 不默认“一轮对话一个文件”。

每次写入前先搜索，再决定：

| 操作 | 含义 |
|---|---|
| **CREATE** | 没有合适的已有知识可以承载 |
| **MERGE** | 新对话深化了已有知识 |
| **NO-OP** | 没有新增有效知识 |

### ❌ 笔记爆炸

```text
spring-transaction.md
spring-transaction-2.md
spring-final.md
```

### ✅ 持续演化

```text
knowledge/frameworks/spring.md

## Spring 事务
├── 自调用失效
├── rollbackFor
└── 传播机制
```

Git Commit 也可以成为认知版本：

```text
knowledge: add Spring transaction basics
knowledge: capture proxy-vs-target follow-up
knowledge: clarify self-invocation mental model
```

---

## ⚡ Quick Start

### 第一次使用

让 Skill 初始化目标知识仓库：

```text
我要用 GPT2Git-Note 管理这个仓库。
我主要做 Java 后端和 Agent，
用 Spring Boot、MySQL、Redis、Kafka、LangGraph、AgentScope。
```

Skill 会建立 `.gpt2git` 协议，而不是创建一堆空笔记。

### 正常学习

```text
为什么 Spring 的 this.b() 不走事务？
```

继续追问，直到真的理解。

最后：

```text
收录
```

流程：

```text
当前学习对话
      ↓
提取 KnowledgeUnit
      ↓
读取 Profile + Taxonomy
      ↓
搜索已有知识
      ↓
CREATE / MERGE / NO-OP
      ↓
Git Commit
```

---

## 🔌 为什么还要做 MCP？

V0.2 目前仍然是 **Skill-first**。

```text
AI Client
   ↓
GPT2Git Skill
   ↓
GitHub / Git capability
```

但仓库协议已经故意设计成不绑定 ChatGPT：

```text
ChatGPT
Claude Code
Codex
Cursor / Other Agents
        │
        ↓
.gpt2git + KnowledgeUnit
        │
        ↓
同一个 Knowledge Repository
```

未来的 GPT2Git MCP 会负责跨客户端统一的知识基础设施，例如：

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

而不是简单暴露：

```text
create_file
update_file
```

### 职责划分

```text
Skill / Client Reasoning
├── 理解当前聊天
├── 提取追问
├── 提取误区
└── 构造 KnowledgeUnit

Future GPT2Git MCP
├── 读取 Profile
├── Topic Resolution
├── Search / Merge
├── Persist
└── Verify Commit
```

**MCP 还没有实现。V0.2 先把协议定稳。**

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

其他支持 Agent Skills / Project Instructions 的 Runtime，可以加载 `SKILL.md`，并提供对应的 Git/GitHub 读写能力。

---

## 🏗️ Repository Structure

```text
GPT2Git-Note/
├── SKILL.md
├── references/
│   ├── repository-profile.md       # Profile / Taxonomy / Config protocol
│   ├── knowledge-schema.md         # KnowledgeUnit
│   ├── github-merge-protocol.md    # CREATE / MERGE / NO-OP
│   └── developer-taxonomy.md       # Adaptive routing
├── examples/
├── tests/
│   └── skill-evals.md
├── docs/
│   └── superpowers/
├── CHANGELOG.md
└── README.md
```

---

## 🛡️ Hard Rules

GPT2Git-Note 不允许：

- 把整段聊天原样导出成笔记；
- 为了“完整”而编造用户没说过的误区；
- 丢掉有价值的追问；
- 每轮聊天都创建一个新 Markdown；
- 首次接入就生成几十个空目录；
- 把同一个知识库拆成 ChatGPT / Claude / Codex 三套；
- Git 写入失败却声称“已提交”；
- 收录 token、密码、私钥等敏感信息。

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
