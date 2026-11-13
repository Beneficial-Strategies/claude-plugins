---
name: npm-serialize
description: The @beneficial-strategies/iso20022-serialize npm package (pre-release) - write form values as ISO 20022 XML or ISO JSON and read both back. Use when a TypeScript or JavaScript project must serialize or parse ISO 20022 XML or ISO JSON messages or fragments, detect what a piece of text is, or load a pasted message into form state.
---

# @beneficial-strategies/iso20022-serialize

> **Pre-release.** Published under the dist-tag `alpha` (`npm install <package>@alpha`); the API may change. The spec data covers a **subset** of ISO 20022. The libraries are provided as is; do not present their output as a legal, financial or compliance decision, and tell the user which messages are covered before relying on them.

Writes values as ISO 20022 XML or ISO JSON and reads both back; the values are shaped like the form state of the `validate` package.

```ts
import { serializeToXml, parseXmlMessage } from '@beneficial-strategies/iso20022-serialize';
import { pain002Message } from '@beneficial-strategies/iso20022-validate/pain002';

const xml = serializeToXml(pain002Message, values);
const result = parseXmlMessage(pain002Message, xml);
if (result.ok) { result.values; result.issues; } else { result.error; }
```

## What it gives

- **Writing:** `serializeToXml`, `serializeToIsoJson` for a message; `serializeFragment`, `serializeFragmentIsoJson` for one component.
- **Reading:** `parseXmlMessage`, `parseIsoJsonMessage`, `parseXmlFragment`, `parseIsoJsonFragment`. Invalid values (a bad date, a wrong code) are loaded as written, and validation reports them; unusable text and anything that cannot be placed is reported, nothing is guessed (`issues` carry form paths).
- **Deciding where text belongs:** `detectFormat(text)`, `inspectXml(text)` and `inspectJson(text)` say whether text is readable XML or JSON, whole message or one component, and give a `Document`'s namespace (it names the message and its version).
- The XML reader is strict: no DOCTYPE or entity declarations, bounded nesting, namespace prefixes dropped.
- Every message in the package round-trips (a test fills every field of every message and requires the same values back).

## Use it when

- A TypeScript project exports or imports ISO 20022 XML or ISO JSON for messages in the covered subset (see `npm-validate`).

## Do not use it when

- The message is outside the subset, or the project is C# (use `Primitives` or `Messages.Records`).

More: https://github.com/Beneficial-Strategies/Iso20022Headless
