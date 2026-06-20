---
id: typescript.basic_types
type: concept
title: Basic Types
description: Primitive and foundational types in TypeScript's static type system under strict mode.
tags: [typescript, fundamentals, types, primitives]
prerequisites: []
related:
  - typescript.type_annotations
  - typescript.type_inference
  - typescript.literal_types
resource: https://www.typescriptlang.org/docs/handbook/2/everyday-types.html
timestamp: 2026-01-01
---

## Summary

TypeScript's basic types describe the shapes of values JavaScript can hold at runtime: primitives (`string`, `number`, `boolean`, `bigint`, `symbol`), special literals (`null`, `undefined`), and structural categories (`object`, arrays, functions). Under `strict` mode, `null` and `undefined` are distinct types and are not implicitly assignable to other types. These names exist only in the type checker — they do not create runtime classes or tags.

## Mental model

Think of basic types as **labels the compiler attaches during analysis**. When JavaScript runs, a `string` is just a string; TypeScript's `string` type is a compile-time constraint that disappears after build.

| Compile-time type | Runtime `typeof` | Erased at build? |
|-------------------|------------------|------------------|
| `string` | `"string"` | Yes (annotation only) |
| `number` | `"number"` | Yes |
| `boolean` | `"boolean"` | Yes |
| `object` | `"object"` | Yes |
| `undefined` | `"undefined"` | Yes |
| `null` | `"object"` (JS quirk) | Yes |

Arrays (`string[]`, `Array<number>`) and tuples (`[string, number]`) are compile-time structural descriptions of JavaScript arrays. Functions typed as `(x: number) => string` are still plain functions at runtime.

## Example

```typescript
let name: string = "Ada";
let count: number = 42;
let active: boolean = true;
let items: string[] = ["a", "b"];
let pair: [string, number] = ["id", 1];

// strictNullChecks: null/undefined are their own types
let missing: undefined = undefined;
let empty: null = null;
```

After compilation to JavaScript, all `: type` annotations are removed. Runtime behavior is identical to untyped JavaScript.

## Common mistakes

* Assuming TypeScript adds runtime type checks for basic types — it does not unless you write them.
* Treating `null` and `undefined` as interchangeable under strict mode — they are separate types.
* Using `object` when you mean a specific shape — prefer interfaces or type aliases for structured data.
* Believing `number` distinguishes integers from floats — TypeScript mirrors JavaScript's single `number` type.

## Related concepts

* [Type Annotations](/concepts/type_annotations.md) — syntax for attaching basic types to declarations
* [Type Inference](/concepts/type_inference.md) — when the compiler chooses basic types for you
* [Literal Types](/concepts/literal_types.md) — subtypes of basic primitives with exact values
