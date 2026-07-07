---
type: Concept
title: Default Generic Parameters
description: Optional type arguments with fallback types when callers omit explicit instantiation.
tags: [typescript, generics, defaults]
prerequisites:
  - concepts/generics
  - concepts/generic_constraints
related:
  - concepts/utility_types
  - concepts/conditional_types
resource: "https://www.typescriptlang.org/docs/handbook/2/generics.html"
timestamp: 2026-01-01
---

## Summary

Default generic parameters specify a fallback type when callers omit a type argument: `type Container<T = string> = { value: T }`. Defaults simplify APIs and mirror default function parameter values — but like all generics, defaults exist only at compile time.

## Mental model

```
type Api<Response = unknown> = (url: string) => Promise<Response>;

Api            → Promise<unknown>
Api<User>      → Promise<User>
```

Defaults must appear after required parameters in the parameter list. A default type must satisfy any `extends` constraint on the same parameter.

Inference can still supply `T` from value arguments even when a default exists — defaults apply when inference cannot determine `T`.

## Example

```typescript
interface State<T = string> {
  data: T;
  loading: boolean;
}

type StringState = State; // T defaults to string
type NumberState = State<number>;

function createStore<T = Record<string, unknown>>(initial: T): { get(): T } {
  let state = initial;
  return { get: () => state };
}
```

## Common mistakes

* Placing defaulted parameters before required ones — syntax error.
* Defaulting to `any` — silently removes type safety for omitted arguments.
* Assuming defaults change runtime behavior — they affect checking only.
* Overlapping defaults with inference in ways that surprise callers (e.g., empty array inferring `never[]` vs default).

## Related concepts

* [Generics](/concepts/generics.md) — base generic mechanism
* [Generic Constraints](/concepts/generic_constraints.md) — defaults must respect bounds
* [Utility Types](/concepts/utility_types.md) — many utilities use defaulted parameters
