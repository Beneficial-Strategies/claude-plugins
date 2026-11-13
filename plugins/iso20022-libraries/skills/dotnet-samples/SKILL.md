---
name: dotnet-samples
description: The BeneficialStrategies.Iso20022.Messages.Samples NuGet package - named, valid ISO 20022 objects for tests, and IsoSample.Minimal<T>() for a minimal valid object of any interface. Use when a C# test project needs a valid ISO 20022 component, choice or message without ten lines of setup and without the message records, or asks how to build test data for the ISO 20022 interfaces.
---

# BeneficialStrategies.Iso20022.Messages.Samples

Named, valid objects of the ISO 20022 interfaces for tests. Status: stable. They are built through the `Factory` against the interfaces of `Messages`, so a sample works with anything that takes the interface (a handler, a mapper, a serializer), and no records are needed. This is a **test** dependency.

## What it gives

- **Named samples** as static properties; the path says what it is: `Samples.AccountIdentification4Choice.Valid.IBAN.Minimal`, `Samples.CashAccount40.Valid.Standard.Minimal`. For a choice the scenario is the variant; for any other type it is `Standard`.
- **Every access builds a new object**: tests cannot change what another test reads. Two samples are equal by value.
- **`Minimal` and `Maximal`**: both valid; `Maximal` has more members set, for the test that needs the optional parts. Bigger samples are made of the samples of their parts.
- **A variation**: `IsoDataFactory.With(sample, b => b.Set(x => x.Identification).To("OTHER-2"))` copies and changes; the sample is untouched.
- **Any other interface**: `Samples.IsoSample.Minimal<T>()` makes a valid object from the interface itself: required members only, a list gets one item, a choice its first variant, a code set its first value. The values are placeholders that fit the format; write no test that depends on what they are.

## Use it when

- A unit test needs a valid ISO 20022 object as input.
- Testing a validator, a consumer or a mapper against objects of the interfaces.

## Do not use it when

- The data must be realistic or business-meaningful: samples are fictitious placeholders.
- Coverage of a particular message is required: the named samples cover a part of the model so far (the rest through `IsoSample.Minimal<T>()`), and invalid samples are not provided yet. Where the rules ask for more than the shape says (one of two optional members must be present, a list of at least two), `IsoSample` builds to the shape and a validator may refuse it; that type needs a named sample.

## Install

`dotnet add package BeneficialStrategies.Iso20022.Messages.Samples` in the test project. It brings `Factory`, `Messages` and `Primitives`; targets netstandard2.0, net8.0 and net10.0.

## Pairs with

`Messages`, `Factory`.

More: https://github.com/Beneficial-Strategies/iso-20022-messages-samples and the package page on nuget.org.
