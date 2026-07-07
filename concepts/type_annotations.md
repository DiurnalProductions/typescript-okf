---
type: Concept
title: Type Annotations
description: "Explicit type syntax on variables, parameters, properties, and return positions."
tags: [typescript, fundamentals, syntax, annotations]
prerequisites:
  - concepts/basic_types
related:
  - concepts/type_inference
  - concepts/function_typing
  - concepts/interfaces
resource: "https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-annotations-on-variables"
timestamp: 2026-07-06
---

## Summary

Type annotations are explicit type expressions written after a colon (`:`) on variables, parameters, properties, and return types. They guide the compiler's checking but are **stripped entirely** during emit. Annotations do not coerce or validate values at runtime.

## Mental model

Annotations are **instructions to the type checker**, not metadata stored on values. Writing `let x: number = "hi"` is a compile-time error; writing `let x: number = 1 as any` may compile but leaves a string at runtime if the value is wrong. When you want to validate a value against a type while preserving its narrower inferred type, use the `satisfies` operator (TS 4.9+) instead of an annotation: `const config = {...} satisfies Config`.

The compiler uses annotations to:

1. Verify assignments and calls at check time
2. Establish context for inference in nested scopes
3. Document intended contracts for readers and tools

At runtime, `let x: number = 1` becomes `let x = 1`.

## Example

```typescript
function greet(name: string): string {
  return `Hello, ${name}`;
}

const user: { id: number; name: string } = {
  id: 1,
  name: "Ada",
};
```

## Common mistakes

* Over-annotating when inference is sufficient — leads to noisy code and fights inference.
* Annotating return types too narrowly on functions that infer wider union returns.
* Assuming annotations enforce runtime safety — only tests, validation libraries, or manual checks do.
* Using type assertions (`as`) to silence errors instead of fixing types — assertions are also erased.

## Related concepts

* [Basic Types](/concepts/basic_types.md) — the vocabulary used in annotations
* [Type Inference](/concepts/type_inference.md) — when annotations can be omitted
* [Function Typing](/concepts/function_typing.md) — annotations on callable signatures
