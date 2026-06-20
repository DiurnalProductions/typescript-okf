---
id: typescript.javascript_interop
type: concept
title: JavaScript Interop
description: Typing JavaScript libraries, dynamic values, and mixed TS/JS codebases without runtime type support.
tags: [typescript, interop, javascript, js]
prerequisites:
  - typescript.structural_typing
  - typescript.type_compatibility
related:
  - typescript.type_erasure
  - typescript.structural_mismatch_risks
  - typescript.ambient_types
resource: https://www.typescriptlang.org/docs/handbook/declaration-files/introduction.html
timestamp: 2026-01-01
---

## Summary

JavaScript interop is the practice of applying TypeScript's static types to JavaScript values that have **no intrinsic types**. This includes importing `.js` modules, using DefinitelyTyped (`@types/*`), writing declaration files (`.d.ts`), and gradually migrating JS codebases. Types describe intent at compile time; JavaScript behavior is unchanged at runtime.

## Mental model

```
JavaScript value (untyped runtime)
        ↓
Declaration file or JSDoc types (compile time)
        ↓
TypeScript checker validates usage
        ↓
Emit JavaScript (types erased)
```

Interop strategies:

* **Allow JS** — `allowJs` checks JS files with types from JSDoc.
* **Declaration files** — ambient typing for npm packages.
* **Structural assignment** — TS trusts shape for plain objects from JS.
* **`unknown` over `any`** — safe boundary for external input.

TypeScript never modifies JS semantics — it only reports errors before emit.

## Example

```typescript
// consuming untyped JS module with types from @types/lodash
import _ from "lodash";

// typing dynamic JSON safely
function parseJson(text: string): unknown {
  return JSON.parse(text);
}

// @ts-check + JSDoc in .js files (compile-time only)
```

## Common mistakes

* Importing untyped packages without declarations — implicit `any` under loose settings.
* Assuming `.d.ts` files enforce runtime behavior — they are erased entirely.
* Using `any` at JS boundaries instead of `unknown` + narrowing.
* Enabling `skipLibCheck` to hide real interop mismatches without understanding tradeoffs.

## Related concepts

* [Type Erasure](/concepts/type_erasure.md) — what remains after interop typing
* [Ambient Types](/concepts/ambient_types.md) — declaration-only typing
* [Structural Mismatch Risks](/concepts/structural_mismatch_risks.md) — when JS data doesn't match types
