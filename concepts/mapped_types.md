---
id: typescript.mapped_types
type: concept
title: Mapped Types
description: Transforming object types by iterating keys with the in keyof pattern.
tags: [typescript, advanced, mapped-types]
prerequisites:
  - typescript.generics
  - typescript.index_signatures
related:
  - typescript.conditional_types
  - typescript.utility_types
resource: https://www.typescriptlang.org/docs/handbook/2/mapped-types.html
timestamp: 2026-01-01
---

## Summary

Mapped types construct new object types by transforming each property of an existing type: `{ [K in keyof T]: ... }`. Modifiers `readonly` and `?` can be added or removed via `+`/`-` prefixes. Mapped types are compile-time transformations with zero runtime representation.

## Mental model

Think of mapped types as **for-loops over keys at the type level**:

```
Input object type T
  for each key K in keyof T
    produce property with transformed value type
```

Common patterns:

* Make all properties optional: `{ [K in keyof T]?: T[K] }` → `Partial<T>`
* Make all properties readonly: `{ readonly [K in keyof T]: T[K] }` → `Readonly<T>`
* Remap keys with `as` clause (key remapping in mapped types)

The `keyof` operator produces a union of keys — also compile-time only.

## Example

```typescript
type Optional<T> = {
  [K in keyof T]?: T[K];
};

type Mutable<T> = {
  -readonly [K in keyof T]: T[K];
};

type Getters<T> = {
  [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
};
```

## Common mistakes

* Mapping over non-object types without constraints — requires `T extends object` or similar.
* Expecting key remapping to affect runtime object keys — only types change.
* Hitting `keyof` limitations with index signatures and union keys.
* Deep partial/recursive mapped types without conditional termination — performance and depth errors.

## Related concepts

* [Conditional Types](/concepts/conditional_types.md) — filter or transform mapped values
* [Utility Types](/concepts/utility_types.md) — standard library mapped types
* [Index Signatures](/concepts/index_signatures.md) — interaction with keyof
