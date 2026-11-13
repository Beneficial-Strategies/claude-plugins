---
name: dotnet-records
description: The BeneficialStrategies.Iso20022.Messages.Records NuGet package - ready-made immutable C# records for every ISO 20022 message, component and choice, with XML and JSON serialization. Use when a C# project must build, read, write, compare or send ISO 20022 messages (pacs.008, pain.001, camt.053 and the rest) and wants classes it does not have to write (required members enforced by the compiler, `with` copies, the exact interface shape, content checked by the primitive types), or asks which package gives the message classes.
---

# BeneficialStrategies.Iso20022.Messages.Records

The ready-made C# classes for every ISO 20022 message, and every component and choice they are made of. Status: stable. The records are written at compile time by a source generator from the interfaces of `Messages`, so the two cannot disagree.

## What it gives

- **Immutable records** with `required` members: the compiler asks for every required field, so a message with missing data does not compile. Equal members mean equal records. Fields that may repeat are collections; optional ones are nullable.
- Typed all the way down to the **primitive types**, so data that does not fit ISO 20022 cannot get in.
- **XML**: `Iso20022XmlSerializer.SerializeToString(message)` and `TryRead<TMessage>(xml)`; nothing is thrown for bad input, the result is the message or the list of problems with their element paths.
- **JSON** with ISO naming: `Iso20022JsonSerializer.SerializeToString` and `Deserialize<TMessage>`.
- Each record implements the **interface** of its shape, so it works wherever the interface is asked for.
- Descriptions from ISO on the types, for the IDE.

## Use it when

- The project builds or reads standard ISO 20022 messages and wants the simplest start: install this one package.
- A mapper or an API must produce an ISO message as an object, then XML or JSON.

## Do not use it when

- The target is **older than net8.0** (.NET Framework, Mono, Unity): the records are net8.0 and net10.0 only. Use `Messages` with the `Factory` there.
- Only some components are needed, or one's own classes must be used: use `Messages` and, if objects must be made, `Factory`.

## Limits (as stated by the package)

No runtime XML Schema validation (the XML it writes is checked against ISO's schemas in the package's own tests). Digital signatures, external content and external schema are not supported yet. Rules beyond the type system (cross-field rules) are not checked by the records themselves.

## Install

`dotnet add package BeneficialStrategies.Iso20022.Messages.Records` (net8.0 and net10.0). It depends on `Primitives` and `Messages`.

## Pairs with

`Messages` (the interfaces it implements), `Primitives` (same namespaces), `Factory` and `Samples` when tests or interface-typed code are wanted.

More: https://github.com/Beneficial-Strategies/iso-20022-messages-records and the package page on nuget.org.
