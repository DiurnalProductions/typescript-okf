---
type: Concept
title: Union Types
description: "Types representing values that may be one of several alternatives, written with the union operator."
tags: [typescript, types, unions]
prerequisites:
  - concepts/literal_types
  - concepts/type_inference
related:
  - concepts/type_narrowing
  - concepts/intersections
  - concepts/conditional_types
resource: "https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types"
timestamp: 2026-01-01
---

## Summary

A union type `A | B` means a value may be assignable to `A` or to `B`. Operations are allowed only on members common to all constituents unless narrowed. Unions are a compile-time set of possibilities; runtime values are still a single JavaScript value at any moment.

## Mental model

Unions describe **uncertainty the compiler tracks**. To call type-specific APIs, you must **narrow** — prove which member you hold.

Discriminated unions add a shared literal field (e.g., `kind`, `type`) so control-flow analysis can reliably split members.

Union types are erased: `string | number` leaves a string or number at runtime with no tag indicating which branch of the union was intended in the type system.

## Example

```typescript
type Result =
  | { ok: true; value: string }
  | { ok: false; error: string };

function handle(result: Result) {
  if (result.ok) {
    console.log(result.value); // narrowed to success branch
  } else {
    console.log(result.error);
  }
}

type Id = string | number;
```

## Common mistakes

* Accessing properties not shared by all union members without narrowing.
* Building non-discriminated unions when a discriminant field would enable safer narrowing.
* Assuming union tags exist at runtime — discriminant fields must be real properties you set.
* Using `| null` without handling `null` in all code paths under strict mode.

## Related concepts

* [Type Narrowing](/concepts/type_narrowing.md) — required to use unions safely
* [Intersection Types](/concepts/intersections.md) — dual combinator (`&` vs `|`)
* [Conditional Types](/concepts/conditional_types.md) — type-level branching on unions
