---
title: "Dostawców ochrony danych tymczasowych"
author: rick-anderson
description: "W tym dokumencie opisano szczegóły implementacji dostawcy ochrony danych tymczasowych platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: 9c1d03373c9d7fb6dffb3583c58aa593fd3875f4
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="ephemeral-data-protection-providers"></a>Dostawców ochrony danych tymczasowych

<a name="data-protection-implementation-key-storage-ephemeral"></a>

Istnieją scenariusze, w których aplikacja wymaga throwaway `IDataProtectionProvider`. Na przykład deweloper może po prostu eksperymentować w aplikacji konsoli jednorazowe lub sama aplikacja jest przejściowy (jest pisanie skryptów lub testu jednostkowego do projektu). Do obsługi tych scenariuszy [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) pakietu zawiera typ `EphemeralDataProtectionProvider`. Ten typ zapewnia Podstawowa implementacja `IDataProtectionProvider` których klucza repozytorium jest przechowywany wyłącznie w pamięci i nie jest zapisywany do dowolnego magazynu zapasowego.

Każde wystąpienie `EphemeralDataProtectionProvider` używa własny unikatowy klucz główny. W związku z tym jeśli `IDataProtector` początek w `EphemeralDataProtectionProvider` generuje chronionych ładunku tego ładunku tylko może bez ochrony za pomocą równoważnych `IDataProtector` (podane takie same [celu](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes) łańcucha) umieszczone w tym samym `EphemeralDataProtectionProvider` wystąpienie.

W poniższym przykładzie pokazano tworzenie wystąpień `EphemeralDataProtectionProvider` i używanie go do zabezpieczania i odbezpieczania danych.

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
