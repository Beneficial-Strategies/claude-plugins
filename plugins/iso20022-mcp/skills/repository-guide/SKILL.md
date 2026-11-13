---
name: repository-guide
description: How the ISO 20022 specification is organized, so you navigate it correctly the first time. Use when asked how the specification is structured, what a business component is versus a message component, how a message identifier like pain.001.001.12 is built, what a choice component or a code set derivation is, how a field traces back to a business concept, or where to start when exploring ISO 20022 with the iso20022 MCP server.
---

# How the ISO 20022 repository is organized

ISO 20022 is the standard for financial messaging: payments, securities, trade, foreign exchange and more. The iso20022 MCP server serves the specification as data. This skill explains how that data is organized. It holds no counts and no specification text: read the live resources below for those.

A shorter version of this guide is on the server for people who do not have this plugin: `iso20022://instructions/repository-guide`. Live sizes: `iso20022://statistics`. Table of contents of the data dictionary: `iso20022://dictionary`.

## The two layers

ISO 20022 separates **what** a message means from **how** it is written on the wire. Most confusion comes from mixing the two.

**Business layer: meaning, stable.** *Business components* are abstract business concepts: `Payment`, `Account`, `Party`. They have no XML tags and do not change when a message is revised. A business component owns *business attributes* (its leaf fields) and *business association ends* (navigable relations to other components). Business components form an inheritance hierarchy through supertype and subtype links: follow the supertype toward the general concept, the subtypes toward specializations.

**Message layer: serialization, versioned.** *Message definitions* are the versioned wire formats, `pain.001.001.12` for example. A message owns *message building blocks*, its top-level sections, and each block references a *message component* from the data dictionary. A component holds *message elements*, the individual fields with their XML tag, cardinality and type.

**The bridge.** A message component traces to the business component whose meaning it implements, and a message element traces to a business element or a business component. To answer "what does this field mean?", follow the trace from the wire format back to the business concept. An element with neither trace is purely technical: structure with no business meaning.

## The path from a message to a field value

```
business area         the domain: pain, camt, pacs, sese ...
  message definition    one versioned wire format
    building block        a top-level section of the message
      message component     a reusable group of fields, shared across messages
        message element       one field
          leaf: a typed value (code set, indicator, amount, text ...)
          or: another message component, or a choice component
```

Message components are shared. One component can be used by a dozen messages across several versions, so always look one up by its ISO ID, never by name.

## Message elements: leaf or keep drilling

A message element is one of two kinds, and the kind tells you whether you have reached the bottom:

| Kind | Typed by | Meaning |
|---|---|---|
| message attribute with a simple type | a data type | a leaf field; nothing deeper |
| message attribute with a complex type | a message component or choice component | an inline structure; drill in |
| message association end | a message component or choice component | a navigable relation; always drill in |

**Choice components.** When the type is a choice component, the field is polymorphic: exactly one of several alternatives applies, and each alternative is itself a message component. List the alternatives, pick the one that fits, and continue.

## Reading a message identifier

An identifier has four parts: `pain.001.001.12`.

- `pain`: the business area (here payments initiation)
- `001`: the message functionality (here customer credit transfer initiation)
- `001`: the flavour, a variation of that function (almost always `001`)
- `12`: the version, which increases with each revision

Searching `pain.001` narrows level by level to the versions of that message. A number at the end of a component name, as in `PaymentInstruction44`, is a revision counter and not a count: `PaymentInstruction45` is the next revision of the same concept.

## Data types

When an element has a simple type, the type family tells you what constrains it:

| Family | Types | What to check |
|---|---|---|
| text | text, code set, identifier set | length limits and pattern; a code set adds its codes; an identifier set names its scheme |
| boolean | indicator | what it means when true and when false: always read both before interpreting |
| decimal | amount, rate, quantity, decimal | amount: allowed currencies; rate: its base; quantity: its unit; decimal: bounds and digits |
| binary | binary | length limits |
| date and time | date, date time, time, duration, year, month and similar | bounds and pattern |
| schema | schema type | maps to an XML Schema built-in type |

## Code sets that restrict other code sets

A code set can be derived from another through a trace link: the derived set restricts its parent to a subset of the parent's codes. **If a code set shows no codes, follow its trace to the parent** to find the full list. The chain can be several levels deep, and a code set that is published separately by ISO usually traces back to the internal definition. The element details resource for a code set resolves the wire values for you where it can.

## Registration status

Everything carries a registration status.

| Status | Meaning | Advice |
|---|---|---|
| Registered | officially ratified and stable | safe to implement against |
| Provisionally Registered | accepted but may still change | use with care; look for a newer registered version |
| Obsolete | withdrawn | do not use in new work |

When several versions of a message exist, prefer the highest registered one. For a library of current versions only, use the `current`, `provisional` and `neededBy` filters on the listings and on search, not "the latest of everything": the latest messages still use older components.

## Where to start

| Question | Read |
|---|---|
| What exists, and how big is it? | `iso20022://statistics`, `iso20022://dictionary`, `iso20022://catalogue` |
| What payment messages are there? | `iso20022://search?q=pain` |
| What is `pain.001`? | `iso20022://search?q=pain.001`, then the hit's `uri` |
| What fields does a message have? | the message's `iso20022://element/{id}/details` |
| What does a field mean? | follow the `trace` on its component to the business component |
| What are the valid codes? | `iso20022://element/{id}/details` of the code set |
| What does a boolean mean? | the indicator type's meaning when true and when false |
| What changed between versions? | `iso20022://snapshots` |
| What must I implement to use a message or type? | `iso20022://element/{id}/downward-dependency-requirements` |
| How are the business concepts around one related, to design a database or object model? | `iso20022://element/{id}/entity-relationships` |
| Find anything by keyword | `iso20022://search?q=...` (spaces as `%20`; `-term` excludes) |

## Two reads that give the whole picture in one response

Walking a message level by level takes many reads. Two resources return a complete answer at once; neither is ever paged, because a partial answer would be wrong and not only short.

**Everything a type needs: `iso20022://element/{id}/downward-dependency-requirements`.** For a message identifier (`pain.001.001.13`), an `xmi:id` or a type name, it lists every building block, component, choice (with every alternative), code set and simple type used, directly or through others, down to the leaves, each once and sorted by name. Use it to know what a library or a schema has to define before it can handle that message. A code set says whether it has members, and for a code set that restricts another it names the parent, whose codes it takes. Codes themselves are values, not types, so they are not listed: read them from the code set's `children`.

**The business model around one concept: `iso20022://element/{id}/entity-relationships`.** `id` is a business component (`DebtorRole`, `PartyIdentificationInformation`), not a message component. `shape=relational` (the default) gives tables, columns, foreign keys and join tables; `shape=hierarchical` gives the supertype trees. `scope=lineage` (the default) covers the component, its supertypes and its subtypes, `scope=family` the whole tree under its topmost supertype, and `depth` follows association ends outward that many hops. It also gives the component's own flat list of members, with where each is defined. Both shapes map onto Mermaid (`erDiagram` for relational, `classDiagram` for hierarchical): draw only what a reader can take in, about 30 entities, and narrow with `scope` and `depth` before drawing.

The dependency list is never cut. Without a subscription, `entity-relationships` lists at most a few attributes and association ends per component: `page.capped` is true, `meta.notice` says so, and `attributeCount` and `associationCount` give the real sizes.

## Search habits that pay off

- **Use ISO IDs for follow-on reads.** Every entry has a globally unique id; names are not unique.
- **Search the literal token first.** If someone gives you `:35B:`, `SETR` or `pain.001`, search that exact text before splitting it into synonyms.
- **ISO 15022 references are searchable.** Field references such as `22a COST` are indexed against the ISO 20022 entries that replace them, which is the direct route from an older message to its ISO 20022 equivalent.
- **Narrow by type.** A type word in the query keeps only that kind of entry (`payment codeset` keeps code sets, `-codeset payment` drops them). The match is exact, so `code` does not match `codeset`.
