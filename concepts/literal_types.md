---
id: typescript.literal_types
type: concept
title: Literal Types
description: Types that represent exact primitive values rather than broad primitives.
tags: [typescript, types, literals, inference]
prerequisites:
  - typescript.basic_types
  - typescript.type_inference
related:
  - typescript.unions
  - typescript.type_narrowing
resource: https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-types
timestamp: 2026-01-01
---

## Summary

Literal types pin a type to a specific value: `"GET"`, `200`, `true`. They enable precise APIs, discriminated unions, and configuration keys. Literal types exist only at compile time; runtime values are ordinary primitives.

## Mental model

A literal type is a **subtype** of its base primitive:

* `"hello"` is a subtype of `string`
* `42` is a subtype of `number`
* `true` is a subtype of `boolean`

Inference widens literals to primitives for mutable bindings (`let`) unless you use `as const` or a type annotation that preserves literals. `const` declarations infer literal types for primitives when not widened.

`as const` performs a **deep readonly freeze** on inference, producing literal types and `readonly` tuples/objects — still erased at runtime except `readonly` becomes plain properties.

## Example

```typescript
type Method = "GET" | "POST";
const GET: Method = "GET";

const config = {
  mode: "strict",
  level: 1,
} as const;
// config.mode: "strict", config.level: 1

type Mode = typeof config.mode; // "strict"
```

## Common mistakes

* Expecting string literal unions to enforce runtime values — only compile-time checking.
* Losing literal types by assigning to `string` or passing to unconstrained generics.
* Forgetting that `let x = "a"` widens to `string`, breaking discriminated union flows.
* Overusing `as const` on large objects — can make types too narrow for mutation needs.

## Related concepts

* [Union Types](/concepts/unions.md) — literal unions for enums and states
* [Type Inference](/concepts/type_inference.md) — widening vs preservation
* [Type Narrowing](/concepts/type_narrowing.md) — refining to literals in branches
