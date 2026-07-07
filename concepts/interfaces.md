---
type: Concept
title: Interfaces
description: Named object shape contracts that support extension and declaration merging.
tags: [typescript, objects, interfaces, structural]
prerequisites:
  - concepts/basic_types
  - concepts/type_annotations
related:
  - concepts/type_aliases
  - concepts/structural_typing
  - concepts/declaration_merging
resource: "https://www.typescriptlang.org/docs/handbook/2/objects.html"
timestamp: 2026-01-01
---

## Summary

Interfaces describe the **shape** of objects: property names, types, optionality, and readonly modifiers. They use structural compatibility — names do not matter at check time, only properties. Interfaces are erased at compile time; they do not exist as runtime schemas or classes unless you also emit classes separately.

## Mental model

`interface User { id: number; name: string }` is a compile-time contract. Any object with at least those properties (and compatible types) is assignable.

Interfaces support:

* **Extension** — `interface B extends A`
* **Declaration merging** — multiple declarations with the same name merge (see declaration merging concept)
* **Callable/constructable/index signatures** — hybrid function-object shapes

Unlike classes, interfaces never emit JavaScript. They cannot enforce private fields at runtime.

## Example

```typescript
interface User {
  readonly id: number;
  name: string;
  email?: string;
}

interface Admin extends User {
  role: "admin";
}

const user: User = { id: 1, name: "Ada" };
```

## Common mistakes

* Treating interfaces as runtime validators — no automatic validation occurs.
* Using interfaces for primitive aliases — prefer type aliases for unions/intersections.
* Expecting nominal typing — two interfaces with identical shapes are interchangeable.
* Relying on `private` in classes for the same guarantees interfaces provide — different mechanism.

## Related concepts

* [Type Aliases](/concepts/type_aliases.md) — alternative for naming types
* [Structural Typing](/concepts/structural_typing.md) — how interfaces participate in compatibility
* [Declaration Merging](/concepts/declaration_merging.md) — interface-specific feature
