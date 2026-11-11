---
name: popular-messages
description: Which ISO 20022 messages are most widely used in production, by payment or settlement scheme (Swift CBPR+, SEPA, T2, Fedwire, FedNow, CHAPS, CHIPS, RTP). Use when asked which messages a scheme uses, what to implement first, or how message adoption ranks.
---

# Which messages are widely used

ISO 20022 has thousands of messages and a handful carry most of the world's payment traffic. This skill holds a ranked table of those messages and the schemes that use each, so you can answer "which messages does FedNow use?" or "what should I implement first for CBPR+?" from the table instead of guessing.

The table is in `reference/popular-messages.md`. Read it when the question is about adoption, a scheme, or where to start. Last reviewed 2026-11-11 by the plugin's owner; what was checked against the schemes' public documents, and what was carried over unchecked, is stated at the top of the table.

## How to use the table

1. **For a scheme, filter by scheme.** Keep the rows whose "Used by" column names it. The names in use are Swift CBPR+, SEPA SCT, SEPA SCT Inst, SEPA SDD, SEPA Request-to-Pay, T2 (TARGET2), CHAPS, Fedwire, CHIPS, FedNow, RTP, Euroclear, Clearstream and DTCC. A scheme that is not in the table is not covered: say so, and do not infer its messages from a similar scheme.
2. **For "what first", take the rank.** Rank 1 is the message used on the most schemes. Payments come first, then reporting, then securities and administration. A shared ranking is not the order of one scheme's flow: for a single scheme, build the flow the table lists for it.
3. **A message is a family; a scheme pins a version.** `pacs.008` is the family, `pacs.008.001.08` is one version of it. The table names the versions a scheme pins where they have been checked. Elsewhere, do not state a version as fact: read the versions of the message from the server (below) and the scheme's own guidelines.
4. **Say how current the answer is.** Adoption changes, and a scheme's rules are revised. Give the review date with any answer that depends on the table.

## Then read the live resources

The table says *which* messages. For *what a message contains*, read the server:

| You need | Read |
|---|---|
| the message and its versions | `iso20022://search?q=pacs.008`, then the hit's `uri` |
| everything needed to implement it | `iso20022://element/{id}/details` |
| every version of it, to pick the one a scheme pins | `iso20022://element/{id}/versions` |
| what changed between two versions | `iso20022://element/{id}/diff/{otherId}` |
| the latest registered versions only | the `current=true` filter on a listing or a search |

Do not copy counts or lists of versions out of this skill: they change, and the server has them.

## What the table is not

It is not complete: it lists the messages used on the most schemes, not every message of any scheme. It is not a statement of what a scheme requires: a scheme's rulebook decides that. It holds none of ISO's text (definitions, field lists); the server has those, and this skill points to it.

If you connect to the server without this plugin you do not get this skill. Everything else the server offers is still there, starting at `iso20022://instructions/repository-guide`.
