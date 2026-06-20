---
id: typescript.generics
type: concept
title: Generics
description: Type parameters that make functions, classes, and types reusable while preserving precise type relationships.
tags: [typescript, generics, type-parameters]
prerequisites:
  - typescript.function_typing
  - typescript.type_inference
related:
  - typescript.generic_constraints
  - typescript.default_generics
  - typescript.conditional_types
resource: https://www.typescriptlang.org/docs/handbook/2/generics.html
timestamp: 2026-01-01
---

## Summary

Generics are **compile-time functions over types**. A type parameter `T` stands for an unknown type that callers instantiate (`<string>`, inferred). Generics enable `identity<T>(x: T): T` to relate input and output types without duplication. They erase completely — no `T` exists at runtime.

## Mental model

```
declare:   function wrap<T>(value: T): { value: T }
call:      wrap("hi")  →  T inferred as string
emit:      function wrap(value) { return { value }; }
```

Generics are not Java-style templates that generate specialized runtime code. One JavaScript function handles all instantiations; the checker tracks `T` per call site.

Instantiation can be explicit or inferred from arguments. Multiple type parameters relate types (`<K, V>`, transformer pipelines).

## Example

```typescript
function first<T>(items: T[]): T | undefined {
  return items[0];
}

const n = first([1, 2, 3]); // number | undefined
const s = first(["a", "b"]); // string | undefined

type Box<T> = { value: T };
const box: Box<number> = { value: 42 };
```

## Common mistakes

* Expecting runtime generic specialization — all instantiations share one emitted function.
* Over-constraining or under-constraining `T` — loses inference precision or allows invalid operations.
* Using `any` as a type argument — collapses generic safety for that instantiation.
* Creating overly complex generic signatures when simpler overloads or unions suffice.

## Related concepts

* [Generic Constraints](/concepts/generic_constraints.md) — bounding type parameters
* [Default Generic Parameters](/concepts/default_generics.md) — optional type arguments
* [Conditional Types](/concepts/conditional_types.md) — type-level logic on generics
