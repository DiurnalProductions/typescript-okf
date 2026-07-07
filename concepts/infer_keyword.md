---
type: Concept
title: The infer Keyword
description: Declaring type variables inside conditional types to extract and name inferred types.
tags: [typescript, advanced, infer, conditional-types]
prerequisites:
  - concepts/conditional_types
related:
  - concepts/utility_types
  - concepts/generics
resource: "https://www.typescriptlang.org/docs/handbook/2/conditional-types.html#inferring-within-conditional-types"
timestamp: 2026-01-01
---

## Summary

`infer` introduces a type variable in the **true branch** of a conditional type, letting the compiler **capture** part of a matched pattern. It powers `ReturnType`, `Parameters`, promise unwrapping, and tuple element extraction. `infer` is purely a type-level binding — no runtime variable is created.

## Mental model

```
T extends SomePattern<infer R> ? R : Default
         ^^^^^^^^^^^^^^^^^^^^^
         if T matches pattern, bind captured piece as R
```

`infer` only works inside conditional type extends clauses. Multiple `infer` positions can appear in one pattern (e.g., function inferring args and return).

If the pattern does not match, the conditional takes the false branch — `infer` variables are not in scope there.

## Example

```typescript
type ReturnType<T> = T extends (...args: never[]) => infer R ? R : never;

type Params<T> = T extends (...args: infer P) => unknown ? P : never;

type Awaited<T> = T extends Promise<infer U> ? U : T;

type First<T> = T extends [infer H, ...unknown[]] ? H : never;
```

## Common mistakes

* Using `infer` outside conditional extends positions — syntax error.
* Expecting `infer` to extract runtime values — types only.
* Patterns too loose — inferring `any` or overly wide types from broad function signatures.
* Forgetting that failed matches skip the true branch entirely — `infer` type may become `never`.

## Related concepts

* [Conditional Types](/concepts/conditional_types.md) — required host for infer
* [Utility Types](/concepts/utility_types.md) — many built-ins use infer
* [Generics](/concepts/generics.md) — infer complements explicit type parameters
