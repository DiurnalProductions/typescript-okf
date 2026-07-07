---
type: Concept
title: Structural Typing
description: "TypeScript's core compatibility model — types match by shape, not by declared name."
tags: [typescript, structural, compatibility, duck-typing]
prerequisites:
  - concepts/interfaces
  - concepts/type_aliases
related:
  - concepts/type_compatibility
  - concepts/duck_typing
  - concepts/excess_property_checks
resource: "https://www.typescriptlang.org/docs/handbook/type-compatibility.html"
timestamp: 2026-01-01
---

## Summary

Structural typing means a value is assignable to a type if it **has at least** the required properties with compatible types. Declaration names (`interface Foo` vs `interface Bar`) are irrelevant if shapes match. This mirrors JavaScript's duck typing but is enforced statically at compile time only.

## Mental model

TypeScript asks: "Does this value's **structure** satisfy the target type?"

```
Source value shape  --->  assignable?  --->  Target type shape
     (extra props OK in many positions)
```

This is the opposite of **nominal** typing (Java, C#) where type names must match. Structural typing enables JS interop but allows unsound assumptions when runtime data does not match the declared shape.

Structural rules apply to objects, functions, classes (for their instance side), and arrays/tuples with element-wise checking.

**Nothing structural is emitted** — compatibility is purely a checker algorithm.

## Example

```typescript
interface Point2D {
  x: number;
  y: number;
}

interface NamedPoint {
  x: number;
  y: number;
  name: string;
}

const p: Point2D = { x: 1, y: 2, name: "origin" }; // OK — extra property
// when assigning object literal directly to Point2D, excess property checks apply
```

## Common mistakes

* Expecting two differently named interfaces to be incompatible when shapes match — they are interchangeable.
* Assuming structural typing validates deep nested data from external APIs without runtime checks.
* Confusing class **nominal** aspects (private/protected fields) with interface structural rules.
* Ignoring that function parameter checking uses a related but stricter bivariant/h contravariant model.

## Related concepts

* [Type Compatibility](/concepts/type_compatibility.md) — formal assignability rules
* [Duck Typing in TypeScript](/concepts/duck_typing.md) — practical structural usage
* [Excess Property Checks](/concepts/excess_property_checks.md) — exception for fresh literals
