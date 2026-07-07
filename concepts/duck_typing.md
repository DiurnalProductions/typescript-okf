---
type: Concept
title: Duck Typing in TypeScript
description: "How JavaScript's duck-typing philosophy is formalized as static structural checking."
tags: [typescript, duck-typing, structural, javascript]
prerequisites:
  - concepts/structural_typing
  - concepts/type_compatibility
related:
  - concepts/javascript_interop
  - concepts/structural_mismatch_risks
resource: "https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes-func.html"
timestamp: 2026-01-01
---

## Summary

"If it walks like a duck and quacks like a duck, use it as a duck." TypeScript encodes this JavaScript idiom as **structural typing**: focus on capabilities (properties and methods), not class hierarchies or interface names. The check is compile-time; runtime still performs no shape validation.

## Mental model

Duck typing in TS bridges two worlds:

| Layer | Behavior |
|-------|----------|
| Compile time | Checker verifies required shape before emit |
| Runtime | Only JavaScript property access — missing methods throw at call time |

This enables typing third-party objects, JSON payloads (optimistically), and test doubles without inheritance. It also creates **false confidence** when external data lacks promised fields.

## Example

```typescript
interface Logger {
  log(message: string): void;
}

function useLogger(logger: Logger) {
  logger.log("ready");
}

// Any object with log(string): void works — no implements keyword required
useLogger({
  log(msg) {
    console.log(`[app] ${msg}`);
  },
});
```

## Common mistakes

* Trusting duck-typed API responses without runtime parsing (e.g., `fetch` JSON).
* Using `implements` keyword thinking it adds runtime enforcement — it is compile-time only.
* Mocking objects missing optional-but-expected fields that real implementations have.
* Confusing structural typing with protocol/interface enforcement at runtime.

## Related concepts

* [Structural Typing](/concepts/structural_typing.md) — formal model behind duck typing
* [Structural Mismatch Risks](/concepts/structural_mismatch_risks.md) — when ducks aren't ducks at runtime
* [JavaScript Interop](/concepts/javascript_interop.md) — duck typing meets untyped JS
