---
type: Concept
title: "any, unknown, and never"
description: "The top type, safe top type, and bottom type — boundaries of TypeScript's type lattice."
tags: [typescript, fundamentals, any, unknown, never]
prerequisites:
  - concepts/basic_types
  - concepts/type_annotations
related:
  - concepts/type_narrowing
  - concepts/runtime_vs_compile_time
  - concepts/javascript_interop
resource: "https://www.typescriptlang.org/docs/handbook/2/functions.html#unknown"
timestamp: 2026-01-01
---

## Summary

`any` disables type checking for a value — the escape hatch. `unknown` is the type-safe counterpart: you must narrow before use. `never` represents values that never occur — uninhabited types used for exhaustiveness and impossible branches. All three are compile-time only; at runtime, values remain ordinary JavaScript values.

## Mental model

```
        any  (checking off — assignable everywhere)
         |
      unknown  (safe top — must narrow before use)
         |
    ... all other types ...
         |
       never  (bottom — assignable to everything, nothing assignable in)
```

* **`any`**: Compiler trusts you. No compile-time safety. Still erased — no runtime tag.
* **`unknown`**: "I don't know yet." Forces narrowing like a union of everything.
* **`never`**: "This cannot happen." Used when control flow proves impossibility.

`never` underpins exhaustiveness checking in `switch` on discriminated unions. Functions that always throw or never return have return type `never`.

## Example

```typescript
// unknown requires narrowing
function parse(input: unknown): string {
  if (typeof input === "string") {
    return input;
  }
  throw new Error("expected string");
}

// never for exhaustiveness
type Shape = { kind: "circle"; r: number } | { kind: "square"; s: number };

function area(shape: Shape): number {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.r ** 2;
    case "square":
      return shape.s ** 2;
    default:
      const _exhaustive: never = shape;
      return _exhaustive;
  }
}
```

## Common mistakes

* Using `any` to "fix" errors — propagates unsafety across the codebase.
* Treating `unknown` like `any` — defeats its purpose.
* Confusing `never[]` (empty array type) with `[]` inference edge cases.
* Assuming `never` throws at runtime — it is purely a type-level signal.

## Related concepts

* [Type Narrowing](/concepts/type_narrowing.md) — required to use `unknown` safely
* [Runtime vs Compile-Time](/concepts/runtime_vs_compile_time.md) — why these types don't exist in JS
* [JavaScript Interop](/concepts/javascript_interop.md) — `any` often enters via untyped JS
