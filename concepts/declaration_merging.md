---
type: Concept
title: Declaration Merging
description: Combining multiple declarations with the same name into a single type or value entity.
tags: [typescript, declarations, merging, modules]
prerequisites:
  - concepts/interfaces
  - concepts/ambient_types
related:
  - concepts/module_augmentation
  - concepts/ambient_types
resource: "https://www.typescriptlang.org/docs/handbook/declaration-merging.html"
timestamp: 2026-01-01
---

## Summary

Declaration merging allows multiple declarations of the same name to **fuse** into one definition. Interfaces with the same name merge their members. Namespaces merge with classes, functions, or enums under specific rules. Merging is a compile-time composition mechanism — runtime may still be a single JS entity (e.g., one function object with merged type declarations).

## Mental model

```
interface Box { height: number; }
interface Box { width: number; }
// equivalent to interface Box { height: number; width: number; }
```

Mergeable entities:

* **Interfaces** — member fusion (duplicate properties must match)
* **Namespaces** — nested export merging
* **Namespace + function/class/enum** — value + type namespace pattern

**Not mergeable**: `type` aliases with the same name — duplicate identifier error.

Merging enables incremental typing of libraries and plugin patterns but can surprise readers when spread across files.

## Example

```typescript
interface User {
  name: string;
}

interface User {
  age: number;
}

// User now requires name and age

function createUser(name: string): User;
namespace createUser {
  export const defaultAge = 18;
}

const u = createUser("Ada");
// createUser.defaultAge available at runtime from namespace merge
```

## Common mistakes

* Expecting type aliases to merge — use interfaces for mergeable extension points.
* Conflicting property types across merged interfaces — compiler errors.
* Unintentional global merges from script `.d.ts` files without module scope.
* Over-merging creating types too permissive for safe narrowing.

## Related concepts

* [Module Augmentation](/concepts/module_augmentation.md) — merging into existing modules
* [Ambient Types](/concepts/ambient_types.md) — common host for merged declarations
* [Interfaces](/concepts/interfaces.md) — primary mergeable object type form
