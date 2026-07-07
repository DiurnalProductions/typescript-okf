---
type: Concept
title: Structural Mismatch Risks
description: Failure modes when compile-time structural types do not match actual runtime JavaScript values.
tags: [typescript, interop, safety, structural]
prerequisites:
  - concepts/javascript_interop
  - concepts/runtime_vs_compile_time
  - concepts/excess_property_checks
related:
  - concepts/duck_typing
  - concepts/any_unknown_never
resource: "https://www.typescriptlang.org/docs/handbook/declaration-files/do-s-and-don-ts.html"
timestamp: 2026-01-01
---

## Summary

Structural mismatch occurs when TypeScript believes a value has a certain shape but runtime data does not. Because types are erased, **no automatic guard** prevents undefined access, silent logic errors, or security issues. Common sources: `fetch` JSON, third-party JS, incorrect type assertions, and outdated `.d.ts` files.

## Mental model

The compiler proves properties exist **relative to types you declared**, not relative to reality:

```
External data → (often) any/unknown missing → assertion to T → compiler trusts T
                                                      ↓
                                            runtime may disagree
```

Risk hierarchy:

1. **Low** — fully typed TS code with no external input
2. **Medium** — TS with validated parsers at boundaries (Zod, io-ts)
3. **High** — `as T` casts on API/JSON data
4. **Critical** — `any` throughout boundary layers

Structural typing + erasure = fast development with **boundary discipline** required.

## Example

```typescript
interface User {
  id: number;
  name: string;
}

async function loadUser(): Promise<User> {
  const res = await fetch("/api/user");
  // Dangerous: JSON.parse returns any; cast assumes shape
  return (await res.json()) as User;
}

// Safer pattern:
function parseUser(data: unknown): User {
  if (
    typeof data === "object" &&
    data !== null &&
    "id" in data &&
    "name" in data &&
    typeof (data as User).id === "number" &&
    typeof (data as User).name === "string"
  ) {
    return data as User;
  }
  throw new Error("invalid user");
}
```

## Common mistakes

* Casting `as User` on network responses without validation.
* Trusting outdated DefinitelyTyped packages for changed JS APIs.
* Using structural typing on discriminated unions without verifying discriminant at runtime.
* Assuming excess property checks protect API ingestion paths — they do not apply there.

## Related concepts

* [Duck Typing in TypeScript](/concepts/duck_typing.md) — optimistic structural assumptions
* [Runtime vs Compile-Time](/concepts/runtime_vs_compile_time.md) — why mismatches persist silently
* [JavaScript Interop](/concepts/javascript_interop.md) — where mismatches enter
