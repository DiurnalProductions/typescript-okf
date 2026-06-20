---
id: typescript.function_typing
type: concept
title: Function Typing
description: Typing callable values including parameters, return types, overloads, and contextual signatures.
tags: [typescript, functions, signatures]
prerequisites:
  - typescript.type_inference
  - typescript.unions
  - typescript.type_narrowing
related:
  - typescript.generics
  - typescript.generic_constraints
resource: https://www.typescriptlang.org/docs/handbook/2/functions.html
timestamp: 2026-01-01
---

## Summary

Function types describe **call signatures**: parameter types (required, optional, rest), return type, and optionally `this` type and overload lists. Functions are first-class JavaScript values; TypeScript only adds static call checking. Overloads are compile-time only — emit uses a single implementation signature.

## Mental model

A function type `(a: A) => R` is checked at **call sites** and **implementations**:

* Parameters are **contravariant** in strict function types checking (with practical exceptions for method parameters).
* Return types are **covariant**.
* Optional parameters `x?: T` are equivalent to `x: T | undefined` at the type level.
* Rest parameters collect into tuple/array types.

Overload declarations present multiple call signatures to callers; the implementation signature must be compatible with all overloads but is often broader. Overloads vanish at runtime — only one function body remains.

## Example

```typescript
function sum(a: number, b: number): number {
  return a + b;
}

type Handler = (event: string) => void;

// Overloads (compile-time call typing)
function parse(input: string): number;
function parse(input: string[]): number[];
function parse(input: string | string[]) {
  return typeof input === "string" ? Number(input) : input.map(Number);
}
```

## Common mistakes

* Annotating callback parameters too narrowly, breaking contextual inference from `Array.map` and similar APIs.
* Implementation signature visible to callers when overloads are intended — callers should see overloads only.
* Forgetting that `void` return means return value is intentionally ignored, not "returns undefined only."
* Using incorrect `this` typing in standalone functions vs methods.

## Related concepts

* [Generics](/concepts/generics.md) — generic functions and inference at call sites
* [Type Narrowing](/concepts/type_narrowing.md) — type predicates in return position
* [Generic Constraints](/concepts/generic_constraints.md) — bounded type parameters on functions
