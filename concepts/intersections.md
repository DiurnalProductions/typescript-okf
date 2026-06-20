---
id: typescript.intersections
type: concept
title: Intersection Types
description: Types that combine multiple type members into one requirement, written with the intersection operator.
tags: [typescript, types, intersections]
prerequisites:
  - typescript.unions
  - typescript.type_aliases
related:
  - typescript.interfaces
  - typescript.mapped_types
resource: https://www.typescriptlang.org/docs/handbook/2/objects.html#intersection-types
timestamp: 2026-01-01
---

## Summary

An intersection type `A & B` requires a value to satisfy **both** `A` and `B` simultaneously. For object types, properties merge. For primitives, intersections often reduce to `never` when incompatible. Intersections are compile-time combinations; runtime values are unchanged plain objects or primitives.

## Mental model

* **Objects**: `A & B` means "has all properties of A and all properties of B."
* **Unions inside intersections**: distribute carefully — `(A | B) & C` is `(A & C) | (B & C)`.
* **Primitives**: `string & number` is `never` — no value can be both.

Intersections model **mixins**, **layered capabilities**, and **combined constraints** on generics. They do not create wrapper objects at runtime.

## Example

```typescript
type Named = { name: string };
type Aged = { age: number };
type Person = Named & Aged;

const ada: Person = { name: "Ada", age: 36 };

type Admin = User & { role: "admin" };
```

## Common mistakes

* Confusing `A & B` with `A | B` — intersection demands all; union allows either.
* Expecting intersection to deep-merge nested objects at runtime — it is not a runtime operation.
* Creating impossible primitive intersections without noticing they collapse to `never`.
* Overusing intersections where a single interface with all fields is clearer.

## Related concepts

* [Union Types](/concepts/unions.md) — complementary combinator
* [Type Aliases](/concepts/type_aliases.md) — common way to name intersections
* [Mapped Types](/concepts/mapped_types.md) — often intersect with utility result types
