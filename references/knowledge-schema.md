# Knowledge Schema

GPT2Git-Note stores **learning structure**, not raw dialogue.

## Canonical Knowledge Unit

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
├── InterviewAnswer?
├── RelatedTopics[]
└── UpdatedAt
```

## Field Rules

### Topic
Use the smallest stable concept that can absorb future deepening, e.g. `Spring 事务自调用` rather than `2026-08-30 Spring 问答`.

### PrimaryQuestion
Preserve the original question faithfully. Light cleanup is allowed; do not rewrite it into a different question.

### CoreAnswer
Keep the minimum explanation needed to establish the concept before the follow-up chain.

### FollowUps
Follow-ups are the highest-value learner-specific content.

Each meaningful follow-up should preserve:

- what the user actually challenged or asked;
- why that question reveals a useful reasoning gap;
- the explanation that resolved it.

Do not create a follow-up entry for filler such as `懂了`, `继续`, or a restatement that adds no reasoning value.

### Misconceptions
Only include misconceptions explicitly supported by the user's statements.

Good:

> 错误理解：既然外部通过 Proxy 进入 a()，那么 a() 内部的 this 也应该是 Proxy。

Bad:

> 用户可能以为所有代理都在编译期生成。

The second statement invents a belief.

### FinalMentalModel
Compress the conversation into a causal model the learner can reconstruct later. Prefer arrows, object relationships, invariants, or a short mechanism over a vague summary.

### InterviewAnswer
Include only when the topic is clearly interview-oriented or the user is building interview knowledge. It should be concise and directly speakable.

## Recommended Markdown Pattern

```markdown
# Spring 事务自调用

## 1. 原始问题
> 为什么同一个类里 this.b() 会让 @Transactional 失效？

## 2. 核心回答
...

## 3. 我的追问 ⭐

### 追问 1：为什么已经从 Proxy 进了 a()，this 还是 Target？
**为什么这是重点：** ...

**回答：** ...

## 4. 我曾经的错误理解
**错误模型：** ...

**纠正：** ...

## 5. 最终心智模型
...

## 6. 面试回答
...
```

## Compression Rule

If the note becomes too long, remove content from the bottom of this value order first:

```text
examples
< repeated core explanation
< primary answer detail
< final mental model
< misconceptions
< follow-ups
```

Never shorten a note by deleting the learner's unique reasoning path while retaining generic textbook exposition.
