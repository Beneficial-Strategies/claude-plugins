---
name: dotnet-factory
description: The BeneficialStrategies.Iso20022.Factory NuGet package - builds, at run time, a class for any ISO 20022 interface, fills it from XML or JSON and gives it value equality, without installing the message records. Use when a C# project reads ISO 20022 data into the interfaces of the Messages package (or an interface of its own), wants objects made without writing implementations, or asks how to avoid the 3,000-message records package; also when deciding whether the Factory is the wrong tool.
---

# BeneficialStrategies.Iso20022.Factory

Ask for an interface and the Factory builds a small sealed class that implements it, fills it from XML or JSON, and gives it to you. Status: stable. It depends on `Primitives` (not on the message packages) and on the Microsoft.Extensions dependency-injection and hosting abstractions, and targets netstandard2.0, net8.0 and net10.0. It makes an object for any interface that follows the pattern, ISO's or one of one's own (primitive types as members).

## What it gives

- **Reading:** `Iso20022XmlSerializer.TryRead<IMyMessage>(xml, factory)` with `new IsoDataFactory()`; the result is the object or every problem with its path. The object has value equality, a readable `ToString`, and the serializer writes it back to the same XML.
- **Dependency injection:** `services.AddIso20022Factory(typeof(IMyMessage), ...)` registers one `IsoDataFactory` (also as `IIsoDataFactory`) and, in a host, builds the classes at startup. Without a host, call `factory.Warm(typeof(IFoo), ...)` at startup. The factory is thread-safe.
- **By hand:** `IsoDataFactory.Make<T>(b => b.Set(x => x.Name).To("..."))` and `IsoDataFactory.With(obj, ...)` for a copy with members changed (what `with` is for a record).
- **One's own factory:** `IIsoDataFactory` is one method, `object Create(Type iface)`.
- ISO's own message interfaces: add `Messages` and ask for a message, e.g. `TryRead<IFIToFIPaymentCancellationRequestV10>(xml, factory)`.

## Use it when

- The project wants ISO messages as objects without the records, or with interfaces of its own.
- Tests or samples need objects without reading anything (`Make`, `With`).

## Do not use it when

- **Native AOT or trimmed apps:** it needs `System.Reflection.Emit`, which is not available. Read into the records (`dotnet-records`) instead.
- Code needs `with`, pattern matching on a concrete type or `new Message { ... }`: use the records. The Factory's classes exist to be read into and used through the interface.
- The interface is not public (a nested public interface is fine).
- Shapes that refer to themselves (an entity that points to its owner, which points back): they can be built and read, but the hash does not terminate; do not use one as a key or compare two.

## What it costs

A class is built once per interface for the life of the process and shared by every factory (about 0.1 ms each; a deep message builds in tens of milliseconds). Reading costs what reading into a record costs; there is no proxy. Built classes cannot be unloaded: warm only the messages that are read.

## Install

`dotnet add package BeneficialStrategies.Iso20022.Factory`

## Pairs with

`Messages` (the ISO interfaces), `Primitives`, `Samples` (named valid objects built through it).

More: https://github.com/Beneficial-Strategies/iso-20022-factory and the package page on nuget.org.
