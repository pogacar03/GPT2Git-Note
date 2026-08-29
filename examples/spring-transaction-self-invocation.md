# Example — Spring 事务自调用

This example shows the intended result after a multi-turn learning conversation. It is a compiled knowledge artifact, not a raw transcript.

# Spring 事务自调用为什么失效

## 1. 原始问题

> Spring 事务中，为什么同一个类里 `this.b()` 调用会导致 `b()` 上的 `@Transactional` 不生效？

## 2. 核心回答

`@Transactional` 通常依赖 Spring AOP 代理。外部调用 Bean 时，调用链是：

```text
Caller
  ↓
Proxy
  ↓
Target.b()
```

事务逻辑由 Proxy 在目标方法前后织入。

但类内部调用：

```text
Target.a()
   ↓
this.b()
```

属于 Target 对象内部的普通 Java 方法调用，没有重新经过 Proxy，因此 `b()` 上的事务增强不会再次触发。

## 3. 我的追问 ⭐

### 追问 1：外部明明已经通过 Proxy 进入 `a()`，为什么 `a()` 里面的 `this` 不是 Proxy？

**为什么这是重点：** 这个问题直接关系到是否真正理解“代理负责拦截调用”和“真正执行方法的对象”是两件事。

**回答：**

Proxy 负责接住外部调用，然后把调用转发给 Target。真正执行 `a()` 方法体的是 Target，因此进入 `a()` 后：

```text
this == Target
```

而不是：

```text
this == Proxy
```

所以 `this.b()` 只是 Target 内部的直接调用。

### 追问 2：Proxy 和 Target 都是堆上的对象，依赖注入时为什么拿到的是 Proxy？

**为什么这是重点：** 这决定了外部调用为什么能触发 AOP，而内部 `this` 调用为什么绕过 AOP。

**回答：**

Spring 完成 Bean 创建和后置处理后，对需要 AOP 增强的 Bean，会把代理对象作为容器对外暴露的 Bean 引用。因此其他 Bean 注入 `UserService` 时，通常拿到的是 Proxy。

但 Proxy 内部仍然会调用实际 Target。

典型关系：

```text
OtherBean
   ↓
UserService Proxy
   ↓
UserService Target
```

## 4. 我曾经的错误理解

**错误模型：**

既然外部已经通过 Proxy 进入 `a()`，那么 `a()` 内部的 `this` 也应该指向 Proxy。

**纠正：**

Proxy 只是调用入口。真正执行 `a()` 方法代码的是 Target，因此方法体中的 `this` 指向 Target。内部调用不会自动“跳回” Proxy。

## 5. 最终心智模型

记住一条调用边界：

```text
外部调用：Caller → Proxy → Target
内部自调用：Target → Target
```

`@Transactional` 是否生效，关键不是“方法上有没有注解”，而是这次调用有没有经过事务代理。

## 6. 面试回答

Spring 的 `@Transactional` 通常基于 AOP 代理实现。外部调用 Bean 时，先经过 Proxy，Proxy 在目标方法前后开启、提交或回滚事务。但同一个类内部通过 `this` 调用另一个方法时，调用发生在 Target 对象内部，不会重新经过 Proxy，因此被调用方法上的事务增强不会触发。

## 7. 相关知识

- JDK Dynamic Proxy vs CGLIB
- Spring BeanPostProcessor
- AOP self-invocation
- Transaction propagation
