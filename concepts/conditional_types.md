---
type: Concept
title: Conditional Types
description: Type-level if-then-else expressions that select types based on assignability tests.
tags: [typescript, advanced, conditional-types]
prerequisites:
  - concepts/generics
  - concepts/unions
related:
  - concepts/infer_keyword
  - concepts/mapped_types
  - concepts/utility_types
resource: "https://www.typescriptlang.org/docs/handbook/2/conditional-types.html"
timestamp: 2026-01-01
---

## Summary

Conditional types have the form `T extends U ? X : Y`. They resolve at **compile time** based on whether `T` is assignable to `U`. Distributive conditionals apply over naked type parameters that are unions, splitting the union across branches.

## Mental model

Conditional types are **type programs** evaluated by the compiler:

```
Is T assignable to U?
  yes → result type X
  no  → result type Y
```

They enable utility patterns: extracting return types, filtering properties, unwrapping promises. No JavaScript `?:` is emitted — entire expression vanishes after type resolution.

Distributivity: `T extends U ? X : Y` when `T` is `A | B` becomes `(A extends U ? X : Y) | (B extends U ? X : Y)` if `T` is a naked type parameter.

## Example

```typescript
type IsString<T> = T extends string ? true : false;
type A = IsString<"hi">; // true
type B = IsString<number>; // false

type Flatten<T> = T extends Array<infer Item> ? Item : T;
type Element = Flatten<string[]>; // string
```

## Common mistakes

* Expecting conditional types to branch at runtime — they are erased before emit.
* Forgetting distributive behavior over unions — causes surprising expanded results.
* Non-distributive needs: wrap `T` in `[T] extends [U] ? X : Y` to disable distribution.
* Recursive conditional types without termination — compiler hits depth limits.

## Related concepts

* [The infer Keyword](/concepts/infer_keyword.md) — extract types inside conditionals
* [Mapped Types](/concepts/mapped_types.md) — often combined in advanced utilities
* [Utility Types](/concepts/utility_types.md) — built on conditional patterns
