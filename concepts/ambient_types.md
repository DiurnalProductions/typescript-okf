---
id: typescript.ambient_types
type: concept
title: Ambient Types
description: Declaration-only type definitions that describe JavaScript values without emitting implementation code.
tags: [typescript, declarations, ambient, dts]
prerequisites:
  - typescript.type_erasure
  - typescript.javascript_interop
related:
  - typescript.declaration_merging
  - typescript.module_augmentation
resource: https://www.typescriptlang.org/docs/handbook/declaration-files/by-example.html
timestamp: 2026-01-01
---

## Summary

Ambient declarations use `declare` to tell the type checker that a value exists at runtime without generating JavaScript for it. They appear in `.d.ts` files and `declare global` blocks. Ambient types are **pure compile time** — zero bytes in emit from declaration files themselves.

## Mental model

Ambient typing separates **description** from **implementation**:

| Construct | Emits JS? | Purpose |
|-----------|-----------|---------|
| `function f() {}` | Yes | implementation |
| `declare function f(): void` | No | type contract for existing JS |
| `interface` | No | shape description |
| `.d.ts` file | No | external typing |

Use ambient declarations for:

* Typing npm packages written in JavaScript
* Global browser APIs (`window`, `document`)
* Legacy script tags loaded outside modules

The checker trusts `declare` — incorrect ambient types cause false confidence.

## Example

```typescript
// types/global.d.ts
declare const BUILD_VERSION: string;

declare namespace NodeJS {
  interface ProcessEnv {
    NODE_ENV: "development" | "production" | "test";
  }
}

// describe a JS module
declare module "legacy-lib" {
  export function init(config: { debug?: boolean }): void;
}
```

## Common mistakes

* Putting implementations in `.d.ts` files — should be declarations only.
* Duplicate ambient declarations conflicting across `@types` packages.
* Using `declare` for values you should implement in TS — creates phantom symbols.
* Forgetting `export {}` to make a file a module vs script (affects global scope).

## Related concepts

* [Declaration Merging](/concepts/declaration_merging.md) — ambient interfaces merge
* [Module Augmentation](/concepts/module_augmentation.md) — extending module types
* [Type Erasure](/concepts/type_erasure.md) — `.d.ts` never emits
