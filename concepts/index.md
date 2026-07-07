# Concepts

* [Ambient Types](ambient_types.md) - Declaration-only type definitions that describe JavaScript values without emitting implementation code.
* [any, unknown, and never](any_unknown_never.md) - The top type, safe top type, and bottom type — boundaries of TypeScript's type lattice.
* [Basic Types](basic_types.md) - Primitive and foundational types in TypeScript's static type system under strict mode.
* [Conditional Types](conditional_types.md) - Type-level if-then-else expressions that select types based on assignability tests.
* [Declaration Merging](declaration_merging.md) - Combining multiple declarations with the same name into a single type or value entity.
* [Default Generic Parameters](default_generics.md) - Optional type arguments with fallback types when callers omit explicit instantiation.
* [Duck Typing in TypeScript](duck_typing.md) - How JavaScript's duck-typing philosophy is formalized as static structural checking.
* [Excess Property Checks](excess_property_checks.md) - Stricter validation when assigning fresh object literals to target types with fewer known properties.
* [Function Typing](function_typing.md) - Typing callable values including parameters, return types, overloads, and contextual signatures.
* [Generic Constraints](generic_constraints.md) - Bounding type parameters with extends to require capabilities or shapes before use.
* [Generics](generics.md) - Type parameters that make functions, classes, and types reusable while preserving precise type relationships.
* [Index Signatures](index_signatures.md) - Typing objects with dynamic string, number, or symbol keys alongside known properties.
* [The infer Keyword](infer_keyword.md) - Declaring type variables inside conditional types to extract and name inferred types.
* [Interfaces](interfaces.md) - Named object shape contracts that support extension and declaration merging.
* [Intersection Types](intersections.md) - Types that combine multiple type members into one requirement, written with the intersection operator.
* [JavaScript Interop](javascript_interop.md) - Typing JavaScript libraries, dynamic values, and mixed TS/JS codebases without runtime type support.
* [Literal Types](literal_types.md) - Types that represent exact primitive values rather than broad primitives.
* [Mapped Types](mapped_types.md) - Transforming object types by iterating keys with the in keyof pattern.
* [Module Augmentation](module_augmentation.md) - Extending types exported by an existing module without modifying its source code.
* [Runtime vs Compile-Time](runtime_vs_compile_time.md) - Separating what TypeScript checks before execution from what JavaScript actually does at runtime.
* [Structural Mismatch Risks](structural_mismatch_risks.md) - Failure modes when compile-time structural types do not match actual runtime JavaScript values.
* [Structural Typing](structural_typing.md) - TypeScript's core compatibility model — types match by shape, not by declared name.
* [Type Aliases](type_aliases.md) - Named definitions for any type expression including unions, intersections, and primitives.
* [Type Annotations](type_annotations.md) - Explicit type syntax on variables, parameters, properties, and return positions.
* [Type Compatibility](type_compatibility.md) - Assignability rules determining when one type may be used where another is expected.
* [Type Erasure](type_erasure.md) - The process by which all TypeScript type information is removed during compilation.
* [Type Inference](type_inference.md) - How TypeScript deduces types from initialization, context, and control flow without explicit annotations.
* [Type Narrowing](type_narrowing.md) - Control-flow-based refinement of types from wider unions or unknown to specific members.
* [Union Types](unions.md) - Types representing values that may be one of several alternatives, written with the union operator.
* [Utility Types](utility_types.md) - Built-in type transformations in TypeScript's standard library for common type manipulations.
