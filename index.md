---
okf_version: "0.1"
id: typescript-okf
name: TypeScript Knowledge Pack
version: "0.1"
description: "OKF knowledge base for TypeScript (type system, inference, generics, structural typing, and JS interop)"
tags: [typescript, ts, javascript, typing, programming]
---

# TypeScript Knowledge Pack

A graph-based learning module for modern TypeScript under **strict mode**. Every concept in this bundle describes **compile-time type behavior** — not runtime execution.

## Start here

If you are new to TypeScript's type system, begin with [Basic Types](/concepts/basic_types.md). TypeScript does not add new runtime types to JavaScript; it adds a static analysis layer that is **fully erased** when code is compiled. Understanding that separation is the foundation for everything else.

Recommended first path:

1. [Basic Types](/concepts/basic_types.md) — primitive and object-level types
2. [Type Annotations](/concepts/type_annotations.md) — explicit typing syntax
3. [Type Inference](/concepts/type_inference.md) — how the compiler infers types from context
4. [Literal Types](/concepts/literal_types.md) — precise string, number, and boolean subtypes
5. [Union Types](/concepts/unions.md) — values that may be one of several types
6. [Type Narrowing](/concepts/type_narrowing.md) — control-flow reasoning about unions
7. [Function Typing](/concepts/function_typing.md) — parameters, returns, and call signatures
8. [Generics](/concepts/generics.md) — type parameters as compile-time functions

## Why TypeScript is compile-time only

JavaScript at runtime knows only `typeof` results: `"string"`, `"number"`, `"boolean"`, `"object"`, `"function"`, `"undefined"`, `"symbol"`, and `"bigint"`. It has no notion of interfaces, generics, union members, or branded types.

TypeScript analyzes your source **before** execution. All type annotations, interfaces, type aliases, and generic parameters exist solely for the checker. After compilation (or transpilation), they vanish — see [Type Erasure](/concepts/type_erasure.md) and [Runtime vs Compile-Time](/concepts/runtime_vs_compile_time.md).

This bundle treats TypeScript as a **structural**, **context-sensitive**, **flow-based** type system layered on JavaScript — not as a new runtime language.

## Recommended learning progression

### Fundamentals

* [Basic Types](/concepts/basic_types.md) — `string`, `number`, `boolean`, `null`, `undefined`, `void`, objects, arrays
* [Type Annotations](/concepts/type_annotations.md) — writing types on variables, parameters, and returns
* [Type Inference](/concepts/type_inference.md) — contextual and best-common-type inference
* [any, unknown, and never](/concepts/any_unknown_never.md) — top, bottom, and escape-hatch types
* [Literal Types](/concepts/literal_types.md) — exact value types and `as const`
* [Union Types](/concepts/unions.md) — discriminated and non-discriminated unions
* [Intersection Types](/concepts/intersections.md) — combining types with `&`
* [Type Narrowing](/concepts/type_narrowing.md) — `typeof`, `in`, discriminated unions, type predicates

### Type modeling

* [Interfaces](/concepts/interfaces.md) — object shape contracts and extension
* [Type Aliases](/concepts/type_aliases.md) — named type definitions with unions and intersections
* [Index Signatures](/concepts/index_signatures.md) — dynamic string/number keys on objects
* [Structural Typing](/concepts/structural_typing.md) — compatibility by shape, not name
* [Type Compatibility](/concepts/type_compatibility.md) — assignability rules in strict mode
* [Duck Typing in TypeScript](/concepts/duck_typing.md) — structural matching in practice
* [Excess Property Checks](/concepts/excess_property_checks.md) — fresh object literal validation

### Generics system

* [Function Typing](/concepts/function_typing.md) — overloads, optional/rest parameters, `this` types
* [Generics](/concepts/generics.md) — type parameters and instantiation
* [Generic Constraints](/concepts/generic_constraints.md) — `extends` bounds on type parameters
* [Default Generic Parameters](/concepts/default_generics.md) — optional type arguments with defaults

### Advanced type mechanics

* [Conditional Types](/concepts/conditional_types.md) — `T extends U ? X : Y`
* [Mapped Types](/concepts/mapped_types.md) — transforming properties over keys
* [The infer Keyword](/concepts/infer_keyword.md) — extracting types inside conditionals
* [Utility Types](/concepts/utility_types.md) — `Partial`, `Pick`, `Omit`, `Record`, and built-ins

### JS interop model

* [JavaScript Interop](/concepts/javascript_interop.md) — typing untyped JS libraries and values
* [Type Erasure](/concepts/type_erasure.md) — what survives compilation
* [Runtime vs Compile-Time](/concepts/runtime_vs_compile_time.md) — two layers, one program
* [Structural Mismatch Risks](/concepts/structural_mismatch_risks.md) — when types lie about runtime data

### Module typing system

* [Ambient Types](/concepts/ambient_types.md) — `.d.ts` declarations without implementation
* [Declaration Merging](/concepts/declaration_merging.md) — combining interface and namespace declarations
* [Module Augmentation](/concepts/module_augmentation.md) — extending third-party module types

## Concept graph

Prerequisites form a **directed acyclic graph**. Follow prerequisite links in each concept's frontmatter for dependency-safe reading order. The graph enforces:

* fundamentals before unions and narrowing
* object modeling before structural compatibility
* generics before conditional and mapped types
* JS interop layer after structural typing, before module augmentation
