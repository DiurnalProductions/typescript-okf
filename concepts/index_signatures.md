---
type: Concept
title: Index Signatures
description: "Typing objects with dynamic string, number, or symbol keys alongside known properties."
tags: [typescript, objects, index-signatures]
prerequisites:
  - concepts/interfaces
  - concepts/type_aliases
related:
  - concepts/mapped_types
  - concepts/structural_typing
resource: "https://www.typescriptlang.org/docs/handbook/2/objects.html#index-signatures"
timestamp: 2026-01-01
---

## Summary

Index signatures describe dictionaries: `{ [key: string]: T }` means any string key maps to `T`. They model dynamic property access while remaining a compile-time contract. Runtime objects are still plain JavaScript objects with no enforced key schema.

## Mental model

Known properties and index signatures **interact**:

* All explicit properties must be assignable to the index signature value type.
* Reading via dynamic key returns the index value type (often widened).
* `noImplicitAny` and strict mode discourage untyped dynamic access.

Number index signatures imply string index compatibility for integer keys. Symbol keys use `[sym: symbol]: T` when needed.

Index signatures erase — runtime code uses normal property access with no type enforcement on keys.

## Example

```typescript
interface StringMap {
  [key: string]: string;
}

interface Config {
  version: number;
  [feature: string]: boolean | number;
}

const env: Record<string, string | undefined> = {
  NODE_ENV: "production",
};
```

## Common mistakes

* Declaring incompatible specific properties with a too-narrow index signature value type.
* Assuming index signatures validate keys at runtime — only undefined behavior if keys are wrong.
* Using `Object.keys` results as `string[]` without narrowing when precise keys are needed.
* Forgetting `readonly` index signatures when immutability is required.

## Related concepts

* [Mapped Types](/concepts/mapped_types.md) — type-level key iteration
* [Interfaces](/concepts/interfaces.md) — common host for index signatures
* [Structural Typing](/concepts/structural_typing.md) — dictionaries compared by shape
