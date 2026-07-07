---
type: Concept
title: Type Compatibility
description: Assignability rules determining when one type may be used where another is expected.
tags: [typescript, compatibility, assignability, structural]
prerequisites:
  - concepts/structural_typing
related:
  - concepts/duck_typing
  - concepts/excess_property_checks
  - concepts/javascript_interop
resource: "https://www.typescriptlang.org/docs/handbook/type-compatibility.html"
timestamp: 2026-01-01
---

## Summary

Type compatibility (assignability) answers whether type `S` can be used where type `T` is required. For objects, `S` must have all properties of `T` with types assignable to `T`'s property types. Extra properties on `S` are generally allowed when assigning a **variable** of type `S` to `T`, but not always for **fresh object literals** (see excess property checks).

## Mental model

Assignability is **directional**: `S` assignable to `T` does not imply the reverse.

Key rules under strict mode:

* Primitives match exactly (with literal subtyping).
* `undefined`/`null` require explicit unions unless `strictNullChecks` exceptions apply.
* Functions: parameter types checked with strictFunctionTypes rules; returns covariant.
* Generics are invariant on their type parameters in many positions unless wrapped in conditional/mapped operations.

All compatibility is resolved at compile time. JavaScript assignment at runtime performs no such checks.

## Example

```typescript
interface Animal {
  name: string;
}

interface Dog {
  name: string;
  breed: string;
}

const dog: Dog = { name: "Pip", breed: "mix" };
const animal: Animal = dog; // OK — Dog structurally extends Animal

// Reverse fails:
// const d: Dog = animal;
```

## Common mistakes

* Assuming assignability means equal types — subtyping is one-directional.
* Ignoring strict function types when passing callbacks to libraries expecting narrower parameters.
* Believing generic types with same structure but different type arguments are always compatible — often they are not.
* Using double assertions (`as unknown as T`) to force compatibility without runtime validation.

## Related concepts

* [Structural Typing](/concepts/structural_typing.md) — foundation of compatibility
* [Excess Property Checks](/concepts/excess_property_checks.md) — literal assignment exception
* [JavaScript Interop](/concepts/javascript_interop.md) — compatibility with untyped values
