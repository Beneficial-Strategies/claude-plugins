---
name: npm-react
description: The @beneficial-strategies/iso20022-react npm package (pre-release) - React hooks (TanStack Form) that drive an ISO 20022 form, with no markup. Use when a React project wants its own inputs and design system for an ISO 20022 form and needs the form state, validation and structure from a hook; for a ready-made form use iso20022-react-ui instead.
---

# @beneficial-strategies/iso20022-react

> **Pre-release.** Published under the dist-tag `alpha` (`npm install <package>@alpha`); the API may change. The spec data covers a **subset** of ISO 20022. The libraries are provided as is; do not present their output as a legal, financial or compliance decision, and tell the user which messages are covered before relying on them.

React hooks that drive an ISO 20022 form from the `validate` schemas, on TanStack Form. **No markup**: the project renders its own inputs. The main entry point is `useIso20022Form`; its README is minimal, so read the package's exports and the demo with the form hook before writing code, and do not invent options.

## Use it when

- The application has a design system and wants only the behavior (state, validation, which fields exist, what is required).

## Do not use it when

- A ready-made form is enough: use `iso20022-react-ui` (`npm-react-ui`), which includes a renderer and skins.
- The project does not use React or TanStack Form.

## Pairs with

`iso20022-validate` (schemas and type descriptors), `iso20022-react-ui` (`SchemaForm` can render a form made with the hook).

More: https://github.com/Beneficial-Strategies/Iso20022Headless (a demo with the form hook is deployed from there).
