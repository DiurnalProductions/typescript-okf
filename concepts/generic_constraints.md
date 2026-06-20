---
id: typescript.generic_constraints
type: concept
title: Generic Constraints
description: Bounding type parameters with extends to require capabilities or shapes before use.
tags: [typescript, generics, constraints, extends]
prerequisites:
  - typescript.generics
related:
  - typescript.default_generics
  - typescript.conditional_types
  - typescript.mapped_types
resource: https://www.typescriptlang.org/docs/handbook/2/generics.html#generic-constraints
timestamp: 2026-01-01
---

## Summary

A generic constraint `T extends U` limits `T` to types assignable to `U`. Inside the generic body, `T` is treated as having at least `U`'s capabilities. Constraints are compile-time bounds only — no runtime interface checks are emitted.

## Mental model

Without constraints, `T` is unknown — you may only assign `T` values to other `T` positions. With `T extends { length: number }`, you can read `.length`. With `T extends keyof Obj`, `T` is a key union subset.

Constraints interact with inference: sometimes the compiler infers a narrower `T` that satisfies the bound; sometimes you must pass explicit type arguments.

`extends` in generics is unrelated to class inheritance at runtime — it is a type bound.

## Example

```typescript
function longest<T extends { length: number }>(a: T, b: T): T {
  return a.length >= b.length ? a : b;
}

function getProperty<Obj, Key extends keyof Obj>(obj: Obj, key: Key): Obj[Key] {
  return obj[key];
}
```

## Common mistakes

* Constraining `T extends object` when primitives should be excluded — still allows boxed types edge cases.
* Using constraints instead of conditional types when return type should depend on argument shape.
* Forgetting that `keyof` constraints still allow invalid keys if `Obj` has index signatures.
* Assuming `extends` validates runtime shapes from external data.

## Related concepts

* [Generics](/concepts/generics.md) — unconstrained type parameters
* [Default Generic Parameters](/concepts/default_generics.md) — constraints with defaults
* [Mapped Types](/concepts/mapped_types.md) — often constrained by `keyof`
