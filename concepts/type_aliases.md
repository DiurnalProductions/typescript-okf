---
id: typescript.type_aliases
type: concept
title: Type Aliases
description: Named definitions for any type expression including unions, intersections, and primitives.
tags: [typescript, types, aliases]
prerequisites:
  - typescript.interfaces
related:
  - typescript.intersections
  - typescript.unions
  - typescript.structural_typing
resource: https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-aliases
timestamp: 2026-01-01
---

## Summary

Type aliases assign a name to a type expression via `type Name = ...`. They can represent unions, intersections, primitives, tuples, and conditional types — anything in the type grammar. Aliases are compile-time names only; they erase completely and leave no runtime trace.

## Mental model

`type` creates a **synonym** in the type checker. Unlike interfaces, type aliases:

* Can express unions and primitives directly
* Do **not** participate in declaration merging
* Can be recursive (with caveats for direct self-reference in some positions)
* Can use mapped and conditional types

Choose interfaces for extensible object contracts in public APIs; choose type aliases for unions, tuples, and advanced type computations.

## Example

```typescript
type ID = string | number;
type Point = [number, number];
type Result<T> = { ok: true; value: T } | { ok: false; error: string };

type Tree<T> = {
  value: T;
  children: Tree<T>[];
};
```

## Common mistakes

* Expecting type alias names to appear in error messages as nominal tags — structural expansion still applies.
* Using `type` vs `interface` interchangeably for merged declaration scenarios — only interfaces merge.
* Creating overly complex recursive aliases without base cases — compiler may error or hit depth limits.
* Believing alias names enable runtime discrimination — they do not.

## Related concepts

* [Interfaces](/concepts/interfaces.md) — object shapes with merging and extension
* [Intersection Types](/concepts/intersections.md) — commonly named via aliases
* [Structural Typing](/concepts/structural_typing.md) — aliases participate structurally
