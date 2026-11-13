---
name: choosing-a-library
description: Which Beneficial Strategies ISO 20022 library to use in a project, and which not to. Use when someone is writing C# or TypeScript code that must handle ISO 20022 messages (build, read, write, serialize to XML or JSON, test, or render a form) and asks which package or library to use, whether one exists for the task, or how the packages fit together.
---

# Choosing an ISO 20022 library

Beneficial Strategies publishes ISO 20022 libraries for two ecosystems. This skill chooses between them and between the packages of each. It holds no version numbers: read the current version on NuGet or npm, never from this text. For what the specification says (which messages exist, which fields a message has, which codes a code set allows), use the ISO 20022 MCP server (plugin `iso20022-mcp`), not these libraries.

## First: C# or TypeScript

| The project is | Look at |
|---|---|
| .NET (C#): services, queue consumers, back ends, tests | the NuGet packages below; each has its own skill (`dotnet-*`) |
| TypeScript or JavaScript: browsers, Node, React forms | the npm packages further down; each has its own skill (`npm-*`); **they are pre-releases covering a subset of the specification** |

## C# (NuGet, released versions)

The packages are built on one idea: **the interfaces are the contract.** Code is written against the interface of a message, component or choice, and something implements it: a ready-made record, a class made on demand, or a class of the user's own.

```
Primitives            the ISO data types (amount, currency code, Max35Text ...); no message definitions
  Messages            an interface for every message, component and choice (uses the primitive types)
    Messages.Records  a ready-made record for every one of them, with XML and JSON (implements the interfaces)
  Factory             builds a class for any interface at run time; needs Primitives (and the Microsoft DI and Hosting abstractions)
  Messages.Samples    named valid objects for tests (built through the Factory against the interfaces)
```

| The task | Package | Skill |
|---|---|---|
| a C# type that converts from `string`, `decimal` or `DateOnly` like an ordinary .NET value but cannot hold a value ISO does not allow; read and write XML or JSON for a class of one's own | `BeneficialStrategies.Iso20022.Primitives` | `dotnet-primitives` |
| the shape of every ISO 20022 message, so an existing class can stand in for one | `BeneficialStrategies.Iso20022.Messages` | `dotnet-messages` |
| ready-made classes for every message: build, read, write, compare by value | `BeneficialStrategies.Iso20022.Messages.Records` | `dotnet-records` |
| read or make objects of the interfaces without installing the records or writing implementations | `BeneficialStrategies.Iso20022.Factory` | `dotnet-factory` |
| valid ISO 20022 objects in unit tests | `BeneficialStrategies.Iso20022.Messages.Samples` | `dotnet-samples` |

Quick rules:

- **Simplest start for a message:** `Messages.Records`. It brings the others it needs.
- **Native AOT or trimmed app:** the Factory cannot be used (it emits code at run time); use the records.
- **Older .NET Framework, Mono, Unity (netstandard2.0):** Primitives, Messages and Factory support it; the records do not (net8.0 and net10.0 only).
- **Existing domain classes that must take part:** Messages (implement the interface) and, if objects must be made for the interface, the Factory.

Not covered by this plugin yet: the C# validation packages (rules beyond the type system, XML and payload validation). Do not guess their API from here; read their own pages on NuGet.

## TypeScript (npm, pre-release)

Published as pre-releases under the dist-tag `alpha` (`npm install <package>@alpha`); the API may still change, and the spec data covers **a subset** of ISO 20022 (see `npm-validate`). Say so to the user before recommending them for production, and never present their output as a compliance decision.

| The task | Package | Skill |
|---|---|---|
| schemas, field rules and messages (Zod) to validate message data | `@beneficial-strategies/iso20022-validate` | `npm-validate` |
| TypeScript types for messages | `@beneficial-strategies/iso20022-types` | `npm-types` |
| write and read XML or ISO JSON | `@beneficial-strategies/iso20022-serialize` | `npm-serialize` |
| drive a form from the schemas (TanStack Form), no markup | `@beneficial-strategies/iso20022-react` | `npm-react` |
| a ready-made React form for a message or a component | `@beneficial-strategies/iso20022-react-ui` | `npm-react-ui` |

## When none of them fits

- A language other than C# or TypeScript: there is no library from us; use the MCP server to read the specification and write the code from it.
- A message that is missing from the npm subset: the C# records cover every ISO 20022 message; for TypeScript, say that the message is not covered yet.
- A question about a field, a code or a message version: the MCP server (`iso20022-mcp`), not a library.
