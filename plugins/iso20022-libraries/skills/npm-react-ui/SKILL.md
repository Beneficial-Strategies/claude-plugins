---
name: npm-react-ui
description: The @beneficial-strategies/iso20022-react-ui npm package (pre-release) - a ready-made React form for any ISO 20022 type in the covered messages, with validation, definitions as prompts and localized messages. Use when a React project wants to show or edit an ISO 20022 message or one component as a form, with skins (plain HTML or Tailwind), locale, field modes and links to ISO's documentation.
---

# @beneficial-strategies/iso20022-react-ui

> **Pre-release.** Published under the dist-tag `alpha` (`npm install <package>@alpha`); the API may change. The spec data covers a **subset** of ISO 20022. The libraries are provided as is; do not present their output as a legal, financial or compliance decision, and tell the user which messages are covered before relying on them.

A ready-made React form for any type of the generated messages. Needs the `validate` package for the schemas.

```tsx
import { Iso20022Form } from '@beneficial-strategies/iso20022-react-ui';
import { schemas, typeDescriptors } from '@beneficial-strategies/iso20022-validate/pain001';

<Iso20022Form
  type="BranchAndFinancialInstitutionIdentification8"
  schemas={schemas}
  typeDescriptors={typeDescriptors}
  onChange={(value, { valid, errors }) => { /* save, send, transform */ }}
/>
```

## What it gives

- **Skins:** the default `plainSkin` is ordinary HTML and needs no CSS; `skin={tailwindSkin}` for Tailwind (three lines in the stylesheet); a design system gets its own `Skin`, a set of small components.
- **Language:** the `locale` prop (for example `locale="es"`); the spec text for a language loads on first use. See `docs/languages.md` in the repository for which languages are covered.
- **More control:** `useIso20022Form` (from `iso20022-react`) and `<SchemaForm form={form} />`.
- **Help on an element:** an "i" button shows the definition, and a document-icon button opens ISO's repository page for the type (`specLink` controls it: leave out for ISO's address, `false` for none, or a function for another registry).
- **Your own control beside the "i":** the `fieldExtra(element)` function.
- **Field modes:** `fieldMode(element)` answers `'editable'` (default), `'label'` or `'hidden'` for an element; `fieldModeView` is `'apply'` or `'mark'`.
- **Sizing:** controls are sized to what the type allows; `fieldSizing="full"` makes them full width.
- It also exports `SchemaForm`, the skins, `I18nProvider`, `DescribedSelect`, `Info` and `Popup`.

## Cost

Passing a message module's `schemas` and `typeDescriptors` brings in that whole message (about 170 KB gzipped for pain.001), not only the type being edited.

## Use it when

- A React application needs a working ISO 20022 form for a message in the covered subset, quickly, or a small editor for one fragment.

## Do not use it when

- The design system must own all markup and behavior: use `iso20022-react` with the project's own inputs.
- The message is not in the subset (see `npm-validate`), or the project is not React.

More: https://github.com/Beneficial-Strategies/Iso20022Headless (two demos are deployed from there; the free Message Explorer is a full application built on these packages).
