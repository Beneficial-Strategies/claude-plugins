---
name: dotnet-messages
description: The BeneficialStrategies.Iso20022.Messages NuGet package - an interface for every ISO 20022 message, component and choice, the contract the other C# packages are written against. Use when a C# project already has its own classes (entity, DTO, another message framework's type) that must take part in an ISO 20022 architecture by implementing the interface, when code should be typed to the interface and not to one implementation, or when asking what the interfaces look like.
---

# BeneficialStrategies.Iso20022.Messages

An interface for every ISO 20022 message, component and choice, with ISO's descriptions on the members. Status: stable. This package is the family's contract: readers, factories and samples are written to the interfaces, so anything that implements one works with them.

## What it gives

- A **component** interface is plain C# (`IGenericIdentification3`): a **required** member is not nullable, an **optional** one is, one that **may repeat** is an `IEnumerable<T>`. Members use the primitive types of `Primitives`.
- A **choice** is an interface (`IAccountIdentification4Choice`) and each variant is an interface of its own, in a namespace named for the choice, that derives from it. Test for a variant with a type pattern (`account switch { IIBAN iban => ..., IOther other => ... }`).
- A **message** interface (`IFIToFICustomerCreditTransferV14`) is shaped like a component and also says how it is read and written: `IsoMessageAttribute.Find(typeof(IMessage))` gives the identifier, the XML element and the document namespace. `Iso.KindOf(Type)` says whether a type is a message, component, choice or variant.
- An interface is **flat**: it lists every member of its own version.
- Namespaces: components in `BeneficialStrategies.Iso20022.Components`, a message in its business area (`BeneficialStrategies.Iso20022.pacs`), choices in `BeneficialStrategies.Iso20022.Choices`.

## Use it when

- A legacy class (`Invoice`, `Payment`) must be accepted by something that wants an ISO 20022 shape: implement the interface, mapping its members, one component at a time, and keep the domain object.
- Code (a handler, a mapper, a consumer) should accept any implementation of a message shape, not a specific class.

## Do not use it when

- Classes for the messages are wanted: the interfaces have no implementation by themselves. Add the records (`dotnet-records`) or the Factory (`dotnet-factory`).

## Where the objects come from

| One wants | Package |
|---|---|
| a class of one's own | this package, implementing the interface |
| a ready-made class for each message | `Messages.Records` |
| classes made on the fly as an interface is asked for | `Factory` |
| valid objects for tests | `Messages.Samples` |

## Install

`dotnet add package BeneficialStrategies.Iso20022.Messages`. Targets netstandard2.0, net8.0 and net10.0. The interfaces are generated from the model; do not expect to edit them.

More: https://github.com/Beneficial-Strategies/iso-20022-messages and the package page on nuget.org.
