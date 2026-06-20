---
id: typescript.type_inference
type: concept
title: Type Inference
description: How TypeScript deduces types from initialization, context, and control flow without explicit annotations.
tags: [typescript, fundamentals, inference]
prerequisites:
  - typescript.basic_types
  - typescript.type_annotations
related:
  - typescript.literal_types
  - typescript.unions
  - typescript.type_narrowing
  - typescript.generics
resource: https://www.typescriptlang.org/docs/handbook/type-inference.html
timestamp: 2026-01-01
---

## Summary

Type inference is the compiler's ability to determine types from context: initializers, return expressions, generic arguments, and contextual typing at call sites. Inference is **not** a fixed table of rules — it is context-sensitive and can widen literal types to their base primitives unless constrained.

## Mental model

Inference happens entirely at **compile time**. The compiler simulates possible value types through the program graph. No inference metadata ships to runtime.

Key inference modes:

* **Initialization inference** — `const x = 1` infers `number` (widened); `const x = 1 as const` preserves `1`.
* **Contextual typing** — callback parameters inherit expected types from outer signatures.
* **Best common type** — array literals unify element types.
* **Return type inference** — function return types flow from `return` statements.

Inference competes with explicit annotations: explicit types on variables can **prevent** widening but also **block** narrower inferred types if written too broadly.

## Example

```typescript
// Widening: inferred as number, not 42
const count = 42;

// Preserved literal via const assertion
const exact = 42 as const;

// Contextual typing
const names = ["Ada", "Lin"]; // string[]
[1, 2, 3].map((n) => n * 2); // n inferred as number

function identity<T>(value: T): T {
  return value;
}
const s = identity("hi"); // T inferred as "hi" (literal) or string depending on flow
```

## Common mistakes

* Expecting `let x = "a"` to keep literal type `"a"` — `let` widens mutable bindings.
* Fighting inference with redundant annotations that widen types unintentionally.
* Assuming failed inference means runtime failure — only compile-time errors result.
* Ignoring that generic inference at call sites can fail or default to overly broad types.

## Related concepts

* [Literal Types](/concepts/literal_types.md) — precision vs widening in inference
* [Union Types](/concepts/unions.md) — inferred unions from branches
* [Type Narrowing](/concepts/type_narrowing.md) — inference refined by control flow
* [Generics](/concepts/generics.md) — inference of type arguments
