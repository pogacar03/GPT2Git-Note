# Adaptive Developer Knowledge Taxonomy

This is a routing guide for developer learning knowledge. It is deliberately **adaptive**, shallow, and profile-aware.

It is not a rigid ontology and it must not pre-create a large directory tree.

## Core Principle

Use three sources together:

```text
User Profile
+ Existing Repository Structure
+ Current Topic Mechanism
        ↓
Resolved Knowledge Path
```

Priority:

1. preserve a coherent existing repository structure;
2. reuse `.gpt2git/taxonomy.yaml` when present;
3. use `.gpt2git/profile.yaml` as a routing hint;
4. fall back to the default stable domains below.

The profile tells the Skill what the user commonly works with. It does **not** mean every possible directory for that role should be created.

---

## Stable Default Domains

Prefer shallow categories that describe what a technology fundamentally is, instead of nesting everything under a job title such as `backend/`.

Recommended domains:

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

These are **possible** domains, not initialization requirements.

A new repository may initially contain none of them.

---

## Lazy Growth Rule

Do not create an empty directory or placeholder note merely because the user's profile mentions a field.

Example onboarding profile:

```text
Backend + AI engineering
Java, Spring Boot, MySQL, Redis, Kafka, RAG, LangGraph, AgentScope
```

Initialization should primarily create:

```text
.gpt2git/
├── profile.yaml
├── taxonomy.yaml
└── config.yaml
```

If the first captured topic is Spring transaction self-invocation, then materialize only:

```text
knowledge/frameworks/spring.md
```

If the next topic is Kafka ISR:

```text
knowledge/
├── frameworks/
│   └── spring.md
└── messaging/
    └── kafka.md
```

Knowledge structure grows from actual captured learning.

---

## Routing Examples

| Topic | Default stable location |
|---|---|
| Java language semantics | `knowledge/languages/java.md` |
| JVM GC / class loading | `knowledge/languages/jvm.md` or an established Java runtime file |
| CompletableFuture / JUC | `knowledge/languages/java-concurrency.md` or an established Java concurrency file |
| Spring @Transactional | `knowledge/frameworks/spring.md` |
| MyBatis / MyBatis-Plus | `knowledge/frameworks/mybatis.md` |
| Next-Key Lock / MVCC / binlog | `knowledge/database/mysql.md` |
| PostgreSQL MVCC / indexes | `knowledge/database/postgresql.md` |
| Redis cache consistency | `knowledge/cache/redis.md` |
| Redis distributed lock implementation | `knowledge/cache/redis.md` when Redis mechanics dominate |
| Kafka partition / ISR / throughput | `knowledge/messaging/kafka.md` |
| RocketMQ delay / retry / ordering | `knowledge/messaging/rocketmq.md` |
| Distributed transaction / idempotency / fencing | `knowledge/distributed/fundamentals.md` or a stable existing distributed file |
| TCP / HTTP / connection lifecycle | `knowledge/networking/network.md` or protocol-specific existing file |
| Linux process / memory / epoll | `knowledge/operating-system/linux.md` |
| RAG chunking / rerank / retrieval | `knowledge/ai/rag.md` |
| ReAct / Plan-and-Execute / checkpoint | `knowledge/ai/agent.md` |
| MCP architecture / tool exposure | `knowledge/ai/mcp.md` |
| E2B / Firecracker / code execution sandbox | `knowledge/ai/sandbox.md` |
| Docker / Kubernetes | `knowledge/devops/containers.md` or an established platform file |
| Sliding window / monotonic stack | `knowledge/algorithms/patterns.md` |

Do not treat these paths as mandatory if the repository already uses a coherent alternative.

---

## Route by Mechanism, Not Job Title

Avoid deep role-first paths such as:

```text
knowledge/backend/middleware/cache/redis/cache-breakdown.md
```

Prefer:

```text
knowledge/cache/redis.md
```

Why: a technology can be relevant to multiple roles, while its mechanism remains stable.

Similarly:

- Kafka is messaging, not permanently `backend/middleware/message-queue/kafka`.
- MySQL is database knowledge, even if learned by a backend engineer.
- RAG is AI engineering knowledge, even if invoked from a Java service.
- TCP is networking knowledge, not Java knowledge.

This prevents duplicate knowledge trees when the user's role evolves.

---

## Stable File Rule

Default to stable files that can absorb multiple related subtopics.

Good:

```text
knowledge/frameworks/spring.md
```

With sections:

```markdown
## Spring 事务
### 自调用失效
### 传播机制
### rollbackFor
```

Avoid one-file-per-question by default:

```text
spring-self-invocation.md
spring-propagation.md
spring-error-rollback.md
```

Use one-topic-per-file only when the existing repository intentionally follows that convention or a single file has become genuinely unwieldy.

---

## Profile-aware Routing

`profile.yaml` narrows ambiguity; it does not override the mechanism.

Example:

```yaml
profile:
  primary_domains:
    - backend
    - ai-engineering
  languages:
    - java
  frameworks:
    - spring-boot
  ai:
    - rag
    - langgraph
    - agentscope
```

If the user says “checkpoint”, the profile makes Agent checkpoint more likely than database checkpoint, but the actual conversation remains the primary evidence.

If the current discussion clearly concerns MySQL redo/checkpoint behavior, route to database knowledge even though the user also works on Agents.

---

## Ambiguous Topics

Choose the location that best matches the mechanism the learner will search for later.

Examples:

### Redisson distributed lock

- Redis command, TTL, Lua, watchdog mechanics → `cache/redis.md`
- leases, fencing tokens, mutual exclusion correctness → `distributed/...`

### Agent checkpoint stored in Redis

- resume/state-machine semantics → `ai/agent.md`
- Redis persistence mechanics → `cache/redis.md`

### Kafka idempotent consumption

- consumer offsets / broker semantics → `messaging/kafka.md`
- end-to-end idempotency across DB + MQ → `distributed/...`

### Spring + MySQL transaction issue

- proxy / propagation / rollback semantics → `frameworks/spring.md`
- isolation / locks / MVCC → `database/mysql.md`

When one learning unit spans multiple mechanisms, choose one primary home and add related-topic links instead of duplicating the same explanation across multiple files.

---

## Updating taxonomy.yaml

Add or change a taxonomy route when:

- the same technology appears repeatedly;
- a stable repository path has emerged;
- a custom repository convention needs to be recorded for future clients.

Do not update taxonomy for every individual subtopic.

Good route:

```yaml
redis:
  domain: cache
  file: redis.md
```

Too granular:

```yaml
redis-cache-breakdown:
  domain: cache
  file: cache-breakdown.md
```

The taxonomy is a durable routing map, not a table of contents for every note heading.
