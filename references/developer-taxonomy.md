# Developer Knowledge Taxonomy

This is a default routing guide for developer learning notes. It is not a rigid ontology.

## Principle

Prefer the user's existing coherent structure. Use this taxonomy only when the repository is new or ambiguous.

## Recommended Domains

```text
knowledge/
├── java/
│   ├── java-base.md
│   ├── jvm.md
│   ├── juc.md
│   └── spring.md
├── database/
│   ├── mysql.md
│   └── redis.md
├── middleware/
│   ├── kafka.md
│   └── mq.md
├── distributed-system/
│   └── fundamentals.md
├── ai/
│   ├── rag.md
│   ├── agent.md
│   ├── mcp.md
│   └── sandbox.md
└── algorithm/
    └── patterns.md
```

## Routing Examples

| Topic | Suggested location |
|---|---|
| Spring @Transactional self-invocation | `knowledge/java/spring.md` |
| CompletableFuture Completion / UniApply | `knowledge/java/juc.md` |
| JVM GC / class loading | `knowledge/java/jvm.md` |
| Next-Key Lock / MVCC / binlog | `knowledge/database/mysql.md` |
| Redis cache consistency / distributed lock | `knowledge/database/redis.md` |
| Kafka partition / ISR / throughput | `knowledge/middleware/kafka.md` |
| Distributed transaction / idempotency | `knowledge/distributed-system/fundamentals.md` |
| RAG chunking / rerank / retrieval | `knowledge/ai/rag.md` |
| ReAct / Plan-and-Execute / checkpoint | `knowledge/ai/agent.md` |
| MCP architecture / tool exposure | `knowledge/ai/mcp.md` |
| E2B / Firecracker / code execution sandbox | `knowledge/ai/sandbox.md` |
| Sliding window / monotonic stack | `knowledge/algorithm/patterns.md` |

## Stable File Rule

Route by stable domain, not by each individual question.

Good:

```text
knowledge/java/spring.md
```

Then sections inside:

```markdown
## Spring 事务
### 自调用失效
### 传播机制
### rollbackFor
```

Avoid:

```text
spring-self-invocation.md
spring-propagation.md
spring-error-rollback.md
```

unless the repository intentionally uses one-topic-per-file structure.

## Ambiguous Topics

Choose the location that best matches the mechanism being learned.

Examples:

- `Redisson 分布式锁` usually belongs under Redis if the discussion is implementation-focused; under distributed systems if the discussion is about mutual exclusion, leases, fencing tokens, or correctness models.
- `Agent checkpoint + Redis` belongs under Agent if Redis is merely persistence; under Redis only if the main lesson is Redis mechanics.
- `Kafka 幂等消费` belongs under Kafka when focused on consumer semantics; under distributed systems when focused on end-to-end idempotency patterns across transports.

When uncertain, prefer the parent topic that is most likely to be searched by the learner later.
