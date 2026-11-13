---
name: npm-validate
description: The @beneficial-strategies/iso20022-validate npm package (pre-release) - Zod schemas, field rules and English validation text for 363 ISO 20022 messages. Use when a TypeScript or JavaScript project must validate ISO 20022 message data (types, lengths, patterns, code lists, required and repeating elements, choices, business rules) in a browser or Node, without a UI, or asks whether a message is covered.
---

# @beneficial-strategies/iso20022-validate

> **Pre-release.** Published under the dist-tag `alpha` (`npm install <package>@alpha`); the API may change. The spec data covers a **subset** of ISO 20022. The libraries are provided as is; do not present their output as a legal, financial or compliance decision, and tell the user which messages are covered before relying on them.

Logic only: no UI. The rules ship with the package, generated from the specification.

## What it gives

- **Zod schemas per message module**, imported from a subpath: `import { schemas } from '@beneficial-strategies/iso20022-validate/pain001'; schemas.GroupHeader114.safeParse(value);`. Every type in a message is available on its own, so one component (a postal address, a debtor agent) can be validated without the whole message.
- Field rules: types, lengths, patterns, code lists, required and repeating elements, choices, and the message's **business rules** where they can be expressed. Some business rules exist only as prose; the repository's rules documentation says which.
- English validation text. Other languages come from the text hosts (`iso-20022-text-<language>`), passed in by the application through `I18nOverrides`; see `docs/languages.md` in the repository.
- Spec definitions are a separate entry point, so validation-only consumers do not bundle the prose.

## Coverage (subset)

363 messages across `pain`, `pacs`, `caam`, `camt`, `admi`, `remt`, `trck`, `acmt`, `semt`, `seev`, `sese` and `head` (the Business Application Header, its own root `<AppHdr>`, not `<Document>`). The specification has more messages than that: check that the message is in the package before promising it, and say so when it is not. For the C# libraries, every message is covered.

## Use it when

- A TypeScript or JavaScript project validates ISO 20022 message data in a form or an API.
- Zod is already in use (the repository has a demo with Zod only, no form library).

## Do not use it when

- The messages are not in the subset above, or a schema-level check against ISO's own XSDs is required: validate against ISO's schemas and the counterparty's rules.
- The project is C#: use the NuGet packages (see `choosing-a-library`).

## Pairs with

`iso20022-types` (types), `iso20022-serialize` (XML and ISO JSON in and out), `iso20022-react` and `iso20022-react-ui` (forms).

More: https://github.com/Beneficial-Strategies/Iso20022Headless
