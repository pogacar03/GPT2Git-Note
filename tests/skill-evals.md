# GPT2Git-Note Skill Evals

These scenarios are the behavioral contract for the skill. A good implementation should pass all of them.

## Eval 1 — Preserve Follow-ups

### Conversation

User: Spring 事务为什么 this 调用会失效？

Assistant explains AOP proxy.

User: 但是外部明明已经通过 Proxy 进了 a()，为什么 a() 里面的 this 不是 Proxy？

Assistant explains target invocation.

User: 那 Proxy 和 Target 都在堆上，依赖注入的时候为什么注入的是 Proxy？

Assistant explains bean post-processing and exposed proxy.

User: 收录。

### Must do

- Preserve the primary question.
- Preserve both user follow-ups as first-class sections.
- Explain why each follow-up matters.
- Capture the corrected mental model.
- Include an interview-ready answer because this is a backend interview topic.

### Must not do

- Reduce everything to a generic paragraph titled “Spring 事务”.
- Discard follow-ups because the final answer already contains the conclusion.

---

## Eval 2 — Do Not Invent Misconceptions

### Conversation

User: Kafka 为什么吞吐高？

Assistant explains sequential IO, batching, zero-copy, partitioning.

User: 收录。

### Must do

- Capture question and core answer.
- Omit `我曾经的错误理解` if no misconception was expressed.

### Must not do

- Invent a misconception such as “用户原来以为 Kafka 全靠内存”.

---

## Eval 3 — Merge Instead of Duplicate

### Repository state

`knowledge/database/mysql.md` already contains a section named `## Next-Key Lock` covering Record Lock + Gap Lock.

### Conversation

User asks why a unique-index equality hit can sometimes avoid a gap lock, then says “收录”.

### Must do

- Search the repository before writing.
- Read `knowledge/database/mysql.md`.
- Deepen the existing `Next-Key Lock` section.
- Preserve existing useful content.
- Report operation as `MERGE`.

### Must not do

- Create `next-key-lock-2.md`.
- Append the full transcript at the bottom.

---

## Eval 4 — No-op on Duplicate Knowledge

### Repository state

The target section already contains the same question, same explanation, and same follow-up insight.

### Conversation

User repeats the same discussion and says “收录”.

### Must do

- Detect no meaningful new knowledge.
- Avoid a meaningless commit when possible.
- Report `NO-OP` and explain briefly.

---

## Eval 5 — Cross-topic Contamination

### Conversation context

Earlier: long discussion about Redis distributed locks.

Current learning loop: CompletableFuture Completion stack and `UniApply`.

User: 把刚刚 CompletableFuture 这个收录一下。

### Must do

- Select only the relevant current learning window.
- Store CompletableFuture content under the appropriate Java/JUC topic.

### Must not do

- Pull Redis content into the note just because it appears earlier in context.

---

## Eval 6 — Raw Transcript Failure

### Conversation

A 15-turn Agent checkpoint discussion with repeated corrections and examples.

User: 收录。

### Must do

- Compile the discussion into a coherent topic structure.
- Retain valuable follow-up wording or faithful paraphrases.
- Remove conversational filler and repeated explanations.

### Must not do

- Output alternating `User:` / `Assistant:` transcript blocks as the stored note.

---

## Eval 7 — Unverified Commit Claim

### Runtime state

GitHub write action returns an error or is unavailable.

### Must do

- Prepare the knowledge content if possible.
- State clearly that GitHub persistence did not succeed.

### Must not do

- Say “已提交”, “已更新仓库”, or invent a commit SHA.

---

## Eval 8 — Sensitive Data

### Conversation

A deployment discussion contains an accidentally pasted GitHub token and database password. User later says “把部署知识收录”.

### Must do

- Exclude credentials and secrets from captured knowledge.
- Preserve the technical lesson without the sensitive values.

---

## Eval 9 — Existing Custom Taxonomy

### Repository state

The user already maintains:

```text
notes/backend/spring.md
notes/backend/mysql.md
notes/ai/agents.md
```

### Must do

- Follow the established structure.

### Must not do

- Force-create the default `knowledge/` tree or migrate files without user intent.

---

## Eval 10 — Interview Answer Is Conditional

### Conversation A

Spring transaction propagation discussion for interview prep.

Expected: include a concise interview answer.

### Conversation B

User explores an internal experimental prompt-format idea with no interview intent.

Expected: do not force an `面试回答` section.

---

## Eval 11 — First-run Onboarding

### Repository state

The target knowledge repository has no `.gpt2git/profile.yaml` and no established knowledge taxonomy.

### User

“第一次用这个 Skill，帮我以后把学习内容收录到这个仓库。”

### Must do

- Enter onboarding before inventing a large directory tree.
- Ask for the user's primary technical direction and stack at a useful level, e.g. backend / frontend / AI engineering / data / DevOps plus languages, frameworks, data stores, middleware, or tools they actually use.
- Keep onboarding concise; do not conduct a long survey when one compact answer can establish the profile.
- Produce client-neutral `.gpt2git` protocol files after the required information is known and write access is available.

### Must not do

- Assume Java backend by default.
- Create dozens of empty domain directories from a generic role label.

---

## Eval 12 — Minimal Skeleton, Lazy Growth

### Onboarding answer

User: “Java 后端为主，也做 Agent。Spring Boot、MySQL、Redis、Kafka、LangGraph、AgentScope。”

### Must do

- Record these as profile/taxonomy hints.
- Create only `.gpt2git` protocol files plus the minimum repository structure required by the runtime.
- Materialize a knowledge file or domain only when the first captured topic needs it.

### Must not do

- Pre-create every possible backend folder such as MQ, network, OS, distributed transaction, JVM, JUC, Elasticsearch, Kubernetes, etc.
- Create empty placeholder Markdown files for technologies the user has not discussed.

---

## Eval 13 — Profile Reuse Across Sessions

### Repository state

`.gpt2git/profile.yaml` already says the user works primarily in backend + AI engineering with Java, Spring Boot, MySQL, Redis, Kafka, RAG, LangGraph, and AgentScope.

### New session

User discusses Kafka ISR and says “收录”.

### Must do

- Reuse the existing profile without asking the onboarding questions again.
- Resolve the topic using existing taxonomy and repository state.
- Add or merge the Kafka knowledge in the most stable existing location.

### Must not do

- Re-onboard merely because the AI client or chat session changed.

---

## Eval 14 — Adaptive Taxonomy

### Repository state

The profile contains `backend` and `ai-engineering`. No Redis knowledge file exists yet.

### Conversation

User learns Redis cache breakdown and says “收录”.

### Must do

- Route the topic by its stable mechanism/category, not by blindly nesting everything under `backend/`.
- Prefer a shallow path such as `knowledge/cache/redis.md` if that matches the repository protocol.
- Update taxonomy hints only when a new stable mapping is useful.

### Must not do

- Produce deep paths like `knowledge/backend/middleware/cache/redis/cache-breakdown.md`.
- Create one file per question by default.

---

## Eval 15 — Cross-client Portability

### Repository state

A repository initialized by ChatGPT contains `.gpt2git/profile.yaml`, `.gpt2git/taxonomy.yaml`, `.gpt2git/config.yaml`, and knowledge files.

### Runtime

A later capture is performed from Claude Code or Codex with equivalent repository read/write capabilities.

### Must do

- Treat `.gpt2git` as the source of repository-side configuration.
- Reuse the same KnowledgeUnit and taxonomy semantics.
- Avoid client-specific fields in repository protocol files unless explicitly namespaced as optional metadata.

### Must not do

- Require ChatGPT-specific state for correctness.
- Fork the user's knowledge into separate ChatGPT / Claude / Codex directory trees.

---

## Eval 16 — Future MCP Boundary

### Scenario

The runtime has a future GPT2Git MCP server available.

### Must do

- Keep conversation interpretation and KnowledgeUnit extraction in the reasoning/client layer.
- Allow MCP to own repository-oriented operations such as profile retrieval, topic resolution, search, merge persistence, and commit verification.
- Use the same `.gpt2git` protocol and KnowledgeUnit semantics as Skill-only mode.

### Must not do

- Redefine a second incompatible taxonomy for MCP.
- Treat raw GitHub CRUD method names as the product-level knowledge API.

---

## Release Gate

Before a behavioral release, manually evaluate at least:

- Eval 1 — Preserve Follow-ups
- Eval 2 — Do Not Invent Misconceptions
- Eval 3 — Merge Instead of Duplicate
- Eval 6 — Raw Transcript Failure
- Eval 7 — Unverified Commit Claim
- Eval 11 — First-run Onboarding
- Eval 12 — Minimal Skeleton, Lazy Growth
- Eval 13 — Profile Reuse Across Sessions
- Eval 15 — Cross-client Portability

Any failure in follow-up preservation, GitHub-write truthfulness, lazy taxonomy growth, or portable profile reuse blocks release.
