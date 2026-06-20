---
id: typescript.utility_types
type: concept
title: Utility Types
description: Built-in type transformations in TypeScript's standard library for common type manipulations.
tags: [typescript, advanced, utility-types, std-lib]
prerequisites:
  - typescript.mapped_types
  - typescript.conditional_types
  - typescript.infer_keyword
related:
  - typescript.default_generics
  - typescript.generic_constraints
resource: https://www.typescriptlang.org/docs/handbook/utility-types.html
timestamp: 2026-01-01
---

## Summary

Utility types (`Partial`, `Required`, `Readonly`, `Pick`, `Omit`, `Record`, `Exclude`, `Extract`, `NonNullable`, `ReturnType`, `Parameters`, `Awaited`, etc.) are **predefined type-level functions** shipped with TypeScript. They compose mapped and conditional types to transform object and union types. All utilities erase — they produce types for checking only.

## Mental model

| Utility | Type-level effect | Runtime effect |
|---------|-------------------|----------------|
| `Partial<T>` | all properties optional | none |
| `Pick<T, K>` | subset of properties | none |
| `Omit<T, K>` | remove properties | none |
| `Record<K, V>` | object with keys K, values V | none |
| `ReturnType<F>` | function return type | none |
| `Exclude<U, E>` | remove union members | none |

Utilities are not imported runtime values (unless re-exported as types only). They document idiomatic patterns you could write manually with mapped/conditional types.

## Example

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

type UserPatch = Partial<User>;
type UserPreview = Pick<User, "id" | "name">;
type UserWithoutEmail = Omit<User, "email">;

type Roles = Record<"admin" | "user", string[]>;

type Fn = (a: number, b: string) => boolean;
type R = ReturnType<Fn>; // boolean
type P = Parameters<Fn>; // [number, string]
```

## Common mistakes

* Using `Omit`/`Pick` with keys not in `T` — may silently produce odd types with strict key checking depending on version/settings.
* Expecting `Partial` deep partiality by default — shallow only unless custom recursive utility.
* Confusing `Exclude` (union filter) with `Omit` (object key removal).
* Applying `ReturnType` to overloaded functions — resolves to last overload signature in many cases.

## Related concepts

* [Mapped Types](/concepts/mapped_types.md) — implementation technique behind many utilities
* [Conditional Types](/concepts/conditional_types.md) — `Exclude`, `Extract`, `NonNullable`
* [The infer Keyword](/concepts/infer_keyword.md) — `ReturnType`, `Parameters`, `Awaited`
