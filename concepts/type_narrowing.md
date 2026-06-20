---
id: typescript.type_narrowing
type: concept
title: Type Narrowing
description: Control-flow-based refinement of types from wider unions or unknown to specific members.
tags: [typescript, narrowing, control-flow, unions]
prerequisites:
  - typescript.unions
  - typescript.type_inference
related:
  - typescript.any_unknown_never
  - typescript.function_typing
  - typescript.conditional_types
resource: https://www.typescriptlang.org/docs/handbook/2/narrowing.html
timestamp: 2026-01-01
---

## Summary

Type narrowing is how TypeScript **updates** the inferred type of a variable within a branch after checks like `typeof`, `instanceof`, `in`, equality comparisons, discriminant switches, and user-defined type predicates. Narrowing is compile-time reasoning only — no runtime type information is added.

## Mental model

The checker maintains a **control-flow graph**. Each branch that excludes possibilities shrinks the union. This is not reflection — JavaScript does not gain `instanceof` support for interfaces because interfaces do not exist at runtime.

Primary narrowing forms:

| Mechanism | Compile-time effect | Runtime cost |
|-----------|---------------------|--------------|
| `typeof x === "string"` | Excludes non-strings | Normal JS check |
| `"prop" in obj` | Excludes shapes without property | Normal JS check |
| `x === null` | Excludes null from union | Normal JS check |
| Discriminant `switch` | Splits union members | Normal JS check |
| Type predicate `x is T` | Trusts function return boolean | Normal JS check |

## Example

```typescript
function format(input: string | string[] | undefined) {
  if (input === undefined) return "";
  if (typeof input === "string") return input;
  return input.join(", ");
}

function isString(value: unknown): value is string {
  return typeof value === "string";
}
```

## Common mistakes

* Assuming `if (obj)` narrows object types with missing properties — truthiness ≠ shape proof.
* Using `instanceof` for interface-shaped objects — interfaces are not constructors.
* Believing narrowed types persist after mutations that could change the value.
* Writing type predicates that lie — `value is T` when the check is insufficient causes unsoundness.

## Related concepts

* [Union Types](/concepts/unions.md) — primary input to narrowing
* [any, unknown, and never](/concepts/any_unknown_never.md) — narrowing unlocks `unknown`
* [Function Typing](/concepts/function_typing.md) — type predicates in signatures
