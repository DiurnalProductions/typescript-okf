---
id: typescript.runtime_vs_compile_time
type: concept
title: Runtime vs Compile-Time
description: Separating what TypeScript checks before execution from what JavaScript actually does at runtime.
tags: [typescript, runtime, compile-time, erasure]
prerequisites:
  - typescript.type_erasure
related:
  - typescript.structural_mismatch_risks
  - typescript.any_unknown_never
  - typescript.javascript_interop
resource: https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html
timestamp: 2026-01-01
---

## Summary

TypeScript adds a **compile-time layer** for static analysis. JavaScript adds a **runtime layer** for execution. They share source files but operate in different phases. All type system features — inference, narrowing, generics, utilities — belong to compile time unless explicitly implemented in runtime JavaScript code you write.

## Mental model

```
┌─────────────────────────────────────┐
│  COMPILE TIME (TypeScript checker)  │
│  - types, interfaces, generics      │
│  - narrowing, inference, errors     │
│  - structural compatibility         │
└─────────────────┬───────────────────┘
                  │ emit (erase types)
                  ▼
┌─────────────────────────────────────┐
│  RUNTIME (JavaScript engine)        │
│  - values, prototypes, exceptions   │
│  - typeof, instanceof, JSON.parse   │
│  - no interfaces, no generics       │
└─────────────────────────────────────┘
```

Questions to ask:

* "Does this exist after `tsc`?" → If it's a type-only construct, **no**.
* "Can this throw at runtime?" → Only JavaScript operations and your explicit checks.
* "Who enforces this?" → Compiler enforces types; you enforce runtime invariants.

## Example

```typescript
type Role = "admin" | "user";

function authorize(role: Role) {
  // Compile-time: role must be admin | user
  // Runtime: role is just a string — could be anything if caller lied
  if (role === "admin") {
    return true;
  }
  return false;
}

// Runtime validation is separate:
function isRole(value: unknown): value is Role {
  return value === "admin" || value === "user";
}
```

## Common mistakes

* Treating TypeScript as a runtime type system (like Rust or Java).
* Assuming compiled code throws on type violations — it does not.
* Forgetting that `strict` mode affects only compile-time checking.
* Using compile-time exhaustiveness without runtime validation at system boundaries.

## Related concepts

* [Type Erasure](/concepts/type_erasure.md) — mechanism removing compile-time layer
* [Structural Mismatch Risks](/concepts/structural_mismatch_risks.md) — when compile-time and runtime diverge
* [any, unknown, and never](/concepts/any_unknown_never.md) — compile-time safety boundaries
