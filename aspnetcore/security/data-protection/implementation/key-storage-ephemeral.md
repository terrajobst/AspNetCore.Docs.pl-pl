---
title: Dostawcy efemerycznej ochrony danych w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, szczegóły implementacji platformy ASP.NET Core dostawcy efemerycznej ochrony danych.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: e4b0014ab3bdbf90b91383e8a33102f94faa8153
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901627"
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a>Dostawcy efemerycznej ochrony danych w programie ASP.NET Core

<a name="data-protection-implementation-key-storage-ephemeral"></a>

Istnieją scenariusze, w której aplikacja musi throwaway `IDataProtectionProvider`. Na przykład deweloper może po prostu eksperymentować w aplikacji konsoli jednorazowe lub sama aplikacja jest przejściowy (jest pisanie skryptów lub projekt testów jednostkowych). Do obsługi tych scenariuszy [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) pakietu zawiera typ `EphemeralDataProtectionProvider`. Ten typ zapewnia podstawową implementację `IDataProtectionProvider` którego klucza repozytorium są przechowywane wyłącznie w pamięci i nie jest zapisywane w dowolnym magazynie pomocniczym.

Każde wystąpienie `EphemeralDataProtectionProvider` używa swój własny unikatowy klucz główny. W związku z tym jeśli `IDataProtector` na `EphemeralDataProtectionProvider` generuje ładunek chronionego, ten ładunek tylko można bez ochrony przez odpowiednik `IDataProtector` (biorąc pod uwagę taki sam [przeznaczenia](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) łańcucha) dostęp do konta root, w tym samym `EphemeralDataProtectionProvider` wystąpienie.

W poniższym przykładzie pokazano tworzenie wystąpienia `EphemeralDataProtectionProvider` i używanie ich do włączania i wyłączania ochrony danych.

```csharp
using System;
using Microsoft.AspNetCore.DataProtection;

public class Program
{
    public static void Main(string[] args)
    {
        const string purpose = "Ephemeral.App.v1";

        // create an ephemeral provider and demonstrate that it can round-trip a payload
        var provider = new EphemeralDataProtectionProvider();
        var protector = provider.CreateProtector(purpose);
        Console.Write("Enter input: ");
        string input = Console.ReadLine();

        // protect the payload
        string protectedPayload = protector.Protect(input);
        Console.WriteLine($"Protect returned: {protectedPayload}");

        // unprotect the payload
        string unprotectedPayload = protector.Unprotect(protectedPayload);
        Console.WriteLine($"Unprotect returned: {unprotectedPayload}");

        // if I create a new ephemeral provider, it won't be able to unprotect existing
        // payloads, even if I specify the same purpose
        provider = new EphemeralDataProtectionProvider();
        protector = provider.CreateProtector(purpose);
        unprotectedPayload = protector.Unprotect(protectedPayload); // THROWS
    }
}

/*
* SAMPLE OUTPUT
*
* Enter input: Hello!
* Protect returned: CfDJ8AAAAAAAAAAAAAAAAAAAAA...uGoxWLjGKtm1SkNACQ
* Unprotect returned: Hello!
* << throws CryptographicException >>
*/
```
