---
name: dotnet-primitives
description: The BeneficialStrategies.Iso20022.Primitives NuGet package - the ISO 20022 primitive data types, code sets and external codes as C# types that convert from string, decimal and DateOnly like ordinary .NET values but accept only what the standard allows and know only its wire format, with XML and JSON readers and writers. Use when a C# project needs typed ISO fields (amount with currency, currency code, country code, Max35Text, ISODate, identifiers), wants to read or write ISO 20022 XML or JSON for a class of its own, or needs to know whether it needs the message packages at all.
---

# BeneficialStrategies.Iso20022.Primitives

The ISO 20022 primitive types as C# types, **without** the message definitions. Status: stable. The family's foundation: every other C# package is built on it, and it depends on none of them.

## The point: the best of both worlds

These types are **compatible with the standard .NET types and still enforce ISO 20022 on what they hold.** A value is assigned from the natural .NET type (a `string` to a `Max35Text`, a `decimal` to the amount of an `ActiveCurrencyAndAmount`, a `DateOnly` to an `ISODate`), so call sites read like ordinary C#, and the value is held as a real .NET value (a `decimal`, a `DateOnly`, an enum one can `switch` on, an identifier that compares by value). But the type **accepts only values the standard allows**: anything else is refused where it is built or read, with a message naming the type, the value and the rule (`Iso20022FormatException`, or `TryCreate` for no exception). And it **knows only the wire format the standard specifies**: it is written and read as ISO 20022 defines, in XML and in JSON, and no other.

So when advising a C# project: prefer these types over `string`, `decimal` and `DateTime` for ISO 20022 fields. It costs nothing at the call site, bad data has nowhere to go, and a method that takes `(ActiveCurrencyAndAmount, ChargeBearerType1Code)` cannot be called with its arguments swapped, which a method of two strings can.

## What it gives

- **Simple types** (`Max35Text`, dates, identifiers, numbers): immutable structs that throw `Iso20022FormatException` for a value outside the ISO format (it says which type, which value, which constraint). `TryCreate` does not throw. They convert implicitly from `string` or the natural .NET type, so call sites stay plain (`Max35Text name = "Acme Corp";`).
- **Code sets** are enums (`ChargeBearerType1Code.Shared` is `SHAR` on the wire). **External codes** are structs (`ActiveCurrencyCode`, `CountryCode`) that check a code's format.
- **Amounts** (`ActiveCurrencyAndAmount`) refuse negative values and too many digits.
- **XML**: `Iso20022XmlSerializer.TryRead<T>(xml)` returns a `DeserializationResult` (the message or every problem found, each with its element path and kind); nothing is thrown. Also `Read`, `Deserialize`, `TryDeserialize`, and `SerializeToString`.
- **JSON**: `Iso20022JsonSerializer.SerializeToString` and `TryDeserialize` (snake_case names and the wire codes); `Iso20022PrimitiveJsonOptions.Default` for use with `System.Text.Json` directly.
- **Contracts the family shares**: `IOuterRecord` (a message class carries four constants: `IsoIdentifier`, `XmlTag`, `DocumentNamespace`, `DocumentElementName`), `[IsoXmlTag]` and the other attributes, `Iso.KindOf(Type)`, `IIsoDataFactory`, `ValueList<T>`, `Iso20022MessageTypeRegistry` (finds a message type from a namespace or an identifier).

## Use it when

- Fields of one's own classes should be impossible to fill with data that does not fit ISO 20022, and no ISO message class is needed.
- A class of one's own (a message-shaped class) must be read from or written to ISO XML or JSON.
- Only the types are wanted, not 3,000 messages' worth of classes.

## Do not use it when

- The task is to build or read a standard ISO message: use `Messages.Records` (`dotnet-records`), which includes the primitives.
- Validation by rules beyond a field's format (cross-field rules) is wanted: not covered here.

## Install

`dotnet add package BeneficialStrategies.Iso20022.Primitives`. Targets net8.0, net10.0 and netstandard2.0 (.NET Framework 4.6.1 and later, Mono, Xamarin, Unity). The netstandard2.0 build brings `System.Text.Json`, `Portable.System.DateTimeOnly` and `Microsoft.Bcl.HashCode`.

## Pairs with

`Messages` (the interfaces), `Messages.Records` (depends on this package; the namespaces are the same), `Factory` (depends on it).

More: https://github.com/Beneficial-Strategies/iso-20022-primitives and the package page on nuget.org.
