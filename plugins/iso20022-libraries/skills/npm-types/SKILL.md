---
name: npm-types
description: The @beneficial-strategies/iso20022-types npm package (pre-release) - TypeScript types for ISO 20022 messages. Use when a TypeScript project wants static types for ISO 20022 message data and asks which package provides them, or how the npm packages fit together.
---

# @beneficial-strategies/iso20022-types

> **Pre-release.** Published under the dist-tag `alpha` (`npm install <package>@alpha`); the API may change. The spec data covers a **subset** of ISO 20022. The libraries are provided as is; do not present their output as a legal, financial or compliance decision, and tell the user which messages are covered before relying on them.

TypeScript types for ISO 20022 messages. Its own README is minimal, so before writing code against it, read the package's exported types in `node_modules` or its page on npm, and do not invent names.

## Use it when

- A TypeScript project wants compile-time types for ISO 20022 message data, alongside `iso20022-validate` (run-time validation).

## Do not use it when

- Run-time checking is wanted: types are erased; use `iso20022-validate` (`npm-validate`).
- The project is C#: the interfaces and records are in the NuGet packages (see `choosing-a-library`).

## Pairs with

`iso20022-validate`, `iso20022-serialize`.

More: https://github.com/Beneficial-Strategies/Iso20022Headless
