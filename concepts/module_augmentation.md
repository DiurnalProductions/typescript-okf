---
type: Concept
title: Module Augmentation
description: Extending types exported by an existing module without modifying its source code.
tags: [typescript, modules, augmentation, declarations]
prerequisites:
  - concepts/declaration_merging
  - concepts/ambient_types
related:
  - concepts/javascript_interop
  - concepts/declaration_merging
resource: "https://www.typescriptlang.org/docs/handbook/declaration-merging.html#module-augmentation"
timestamp: 2026-01-01
---

## Summary

Module augmentation adds declarations to an existing module's type scope using `declare module "name" { ... }`. It merges new interfaces, namespaces, or values (with `declare`) into the module's exported types. Augmentation is **compile-time only** — unless you also ship runtime JS that mutates the module, only types change.

## Mental model

```
Original module types  +  augmentation file  →  merged module types
        (checker view)                         (still one runtime module)
```

Common uses:

* Adding fields to `Express.Request` via middleware plugins
* Extending third-party library interfaces
* Organizing project-wide type extensions in `*.d.ts`

Rules:

* Target module must be resolvable (augmentation merges with existing exports).
* Interfaces inside augmentation merge by name.
* Cannot augment modules you author in the same file pattern without module boundary discipline.

## Example

```typescript
// express-augment.d.ts
import "express";

declare module "express-serve-static-core" {
  interface Request {
    userId?: string;
  }
}

// Now req.userId is typed where Express Request is used
```

```typescript
// augmenting your own module from a separate file
// main.ts exports implementation
// augment.d.ts:
declare module "./config" {
  interface AppConfig {
    featureFlags: Record<string, boolean>;
  }
}
```

## Common mistakes

* Augmenting the wrong module path string — merge silently fails to apply.
* Forgetting side-effect import (`import "module"`) to load augmentation in compilation.
* Expecting augmentation to inject runtime properties — only types unless JS also adds them.
* Augmenting with conflicting member types — merge errors or unsound types.

## Related concepts

* [Declaration Merging](/concepts/declaration_merging.md) — underlying merge mechanics
* [Ambient Types](/concepts/ambient_types.md) — augmentation lives in declaration files
* [JavaScript Interop](/concepts/javascript_interop.md) — extending third-party JS types
