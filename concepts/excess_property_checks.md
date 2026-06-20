---
id: typescript.excess_property_checks
type: concept
title: Excess Property Checks
description: Stricter validation when assigning fresh object literals to target types with fewer known properties.
tags: [typescript, structural, literals, strict]
prerequisites:
  - typescript.structural_typing
  - typescript.type_compatibility
related:
  - typescript.structural_mismatch_risks
  - typescript.type_narrowing
resource: https://www.typescriptlang.org/docs/handbook/2/objects.html#excess-property-checks
timestamp: 2026-01-01
---

## Summary

When you assign an **object literal** directly to a type, TypeScript rejects **unknown extra properties** not declared on the target type. This is stricter than general structural assignability and catches typos at compile time. The check does not run at runtime and does not apply the same way when assigning through a variable.

## Mental model

Structural typing normally allows extra properties on the source when assigning variables (width subtyping). Excess property checks are a **deliberate exception** for fresh literals:

```
const x = { a: 1, b: 2 };
TargetType t = x;        // often OK — b may be ignored

TargetType t = { a: 1, b: 2 };  // ERROR if b not in TargetType
```

Workarounds like intermediate variables or type assertions bypass the check — they do not add runtime validation.

## Example

```typescript
interface Options {
  mode: "strict" | "loose";
}

// Error: 'typo' does not exist on Options
// const opts: Options = { mode: "strict", typo: true };

const raw = { mode: "strict" as const, typo: true };
const opts: Options = raw; // OK via variable widening
```

## Common mistakes

* Thinking excess property checks protect runtime API boundaries — they only apply to literal syntax paths.
* Using type assertion to silence legitimate typos instead of fixing property names.
* Expecting the same errors when building objects incrementally vs single literal assignment.
* Confusing excess property errors with index signature conflicts on known keys.

## Related concepts

* [Type Compatibility](/concepts/type_compatibility.md) — general assignability without literal strictness
* [Structural Mismatch Risks](/concepts/structural_mismatch_risks.md) — runtime gaps excess checks cannot catch
* [Structural Typing](/concepts/structural_typing.md) — broader structural model
