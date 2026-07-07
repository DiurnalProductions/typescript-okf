---
type: Concept
title: Type Erasure
description: The process by which all TypeScript type information is removed during compilation.
tags: [typescript, erasure, compile-time, emit]
prerequisites:
  - concepts/javascript_interop
related:
  - concepts/runtime_vs_compile_time
  - concepts/ambient_types
resource: "https://www.typescriptlang.org/docs/handbook/2/classes.html#types-at-runtime"
timestamp: 2026-07-06
---

## Summary

Type erasure means TypeScript types, interfaces, type aliases, generic parameters, and most annotations **do not exist** in emitted JavaScript. The compiler deletes them. (This holds across compilers: TypeScript 6.x's JS implementation and the Go-native port — the basis for TypeScript 7, roughly 10× faster — share the erasure model.) What may remain: class syntax (as JS classes), enums (unless `const enum` inlined), namespaces merged with values, and decorators/metadata if configured — but **type expressions themselves are always erased**.

## Mental model

```
Source (.ts)                    Emit (.js)
─────────────────────────────────────────────
let x: number = 1        →      let x = 1;
interface User { ... }   →      (nothing)
type ID = string         →      (nothing)
function f<T>(v: T)      →      function f(v) { ... }
```

Implications:

* `typeof` cannot see TypeScript types.
* `instanceof` checks constructors, not interfaces.
* Generics do not create per-`T` functions at runtime.
* Runtime validation requires separate libraries or manual code.

## Example

```typescript
interface Point {
  x: number;
  y: number;
}

function distance(a: Point, b: Point): number {
  return Math.hypot(a.x - b.x, a.y - b.y);
}

// Emits approximately:
// function distance(a, b) {
//   return Math.hypot(a.x - b.x, a.y - b.y);
// }
```

## Common mistakes

* Expecting interfaces to exist for `instanceof` or reflection.
* Assuming generic type arguments affect runtime branching — they do not.
* Using `enum` without understanding emit modes (`const enum`, `preserveConstEnum`).
* Believing `as` casts insert runtime conversions — they are erased completely.

## Related concepts

* [Runtime vs Compile-Time](/concepts/runtime_vs_compile_time.md) — two-layer mental model
* [JavaScript Interop](/concepts/javascript_interop.md) — types layered on untyped JS
* [Ambient Types](/concepts/ambient_types.md) — declarations with no emit at all
