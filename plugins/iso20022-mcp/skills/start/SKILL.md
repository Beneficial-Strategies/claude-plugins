---
name: start
description: First steps with the iso20022 MCP server, for someone new to it. Use when asked what you can do with the server, how to get started, what to ask first, which plan or tier the user is on, why a read was refused or is held (a license or billing-address prompt), or what the free plan returns. Also the command /iso20022-mcp:start.
---

# Getting started with the ISO 20022 MCP server

The iso20022 MCP server serves the ISO 20022 financial messaging specification as data: messages, their components and fields, code sets, business concepts, and what changed between versions. This skill tells you what to do first and how to answer a newcomer. It holds no limits, prices or counts: they change, and the server states them. Read the live resources named below.

## First: find out who the user is to the server

Read `iso20022-mcp://account/session`. It says who is signed in, which plan applies, which features that plan has (`features`), how the plans compare (`tierComparison`), and, if the user is held (`access.state`), which step is pending and which tool to call. It can be read even while held.

- **Not signed in.** The client prompts for a sign-in (Google or GitHub). Tell the user to complete it, then try again. Do not ask for a password or an API key in the chat.
- **Held at a license step.** Every signed-in plan, the free one included, accepts an agreement once. The read says which. Show the user what it says and the link it gives to the agreement's text, and call the tool it names (`review_and_agree_to_terms`) only after the user has said they accept. Never accept on their behalf.
- **Held at the billing-address step.** For the plans that need one, a billing email address is confirmed once through a link sent to that address; the tool is `confirm_billing_email`. Ask the user for the address, do not invent one.

A new account starts with a time-limited trial, and after it the free plan. `iso20022-mcp://account/subscription` says which plan the user is on and when it ends or renews, and `iso20022-mcp://pricing/organization` quotes an organization plan for a number of seats. Say what those resources say; do not recite a price or a limit from memory.

## What the plan changes

Be honest about it. A plan limits how much a read returns, not whether it works:

- A capped answer says so: `page.capped` is true and `meta.notice` (and `meta.notices`) explain, with the real size where it is known. Pass that on instead of presenting the capped list as complete.
- Some resources are only for paid plans and are not listed for the others. If something the user needs is missing, check the index at `iso20022-mcp://`, which lists exactly what they may read, and tell them what upgrading would add by reading `iso20022-mcp://account/subscription`.
- Subscribing, managing billing and organizations are tools (`subscribe`, `manage_billing`, `create_organization`, and so on). Use them only when the user asks, and describe what they do first.

## What to ask first

Offer a few concrete starts, in the user's own domain if they have said it:

| The user wants | Start with |
|---|---|
| to understand what exists | `iso20022://statistics`, then `iso20022://dictionary` |
| to find a message or a field | `iso20022://search?q=...` with the literal token they gave (`pain.001`, `:35B:`, `SETR`) |
| the fields of a message | the hit's `uri`, then `iso20022://element/{id}/details` |
| to know which messages a scheme uses | the `popular-messages` skill |
| to move from one version to another | `iso20022://element/{id}/diff/{otherId}`, or `iso20022://snapshots` |
| everything needed to implement a message | `iso20022://element/{id}/downward-dependency-requirements` |

For how the specification is organized (business layer and message layer, identifiers, choices, code sets), use the `repository-guide` skill. The server has a shorter version for people without the plugin: `iso20022://instructions/repository-guide`.

## Habits that keep answers correct

- **Cite identifiers and versions.** Say `pain.001.001.12`, not "the credit transfer message", and give the ISO ID of anything a follow-up read will need. Names are not unique; ISO IDs are.
- **Never guess a field.** If a read does not show it, say it was not found and search again. A field, a code or a multiplicity comes from the server, not from memory.
- **Say how current it is.** The server serves one ISO snapshot at a time; `iso20022://snapshots` gives its date.
- **ISO's text is ISO's.** The definitions the server returns are ISO's, provided as is; the only authoritative source is <https://www.iso20022.org/>.

## When something fails

- **A read is refused or returns `data: null`** with a notice marked blocking: it is the license or billing step above. Do the step; do not retry the read in a loop.
- **The first call after a long idle period is slow:** a cold start is rare in production but possible. Retry once.
- **Anything else:** the user can write to support@beneficialstrategies.com with the time and what they asked.
