---
title: Dostawców ochrony danych tymczasowych w ASP.NET Core
author: rick-anderson
description: Szczegóły dotyczące implementacji dostawcy ochrony danych tymczasowych platformy ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: e4b0014ab3bdbf90b91383e8a33102f94faa8153
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279470"
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a><span data-ttu-id="0a0b2-103">Dostawców ochrony danych tymczasowych w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a0b2-103">Ephemeral data protection providers in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-ephemeral"></a>

<span data-ttu-id="0a0b2-104">Istnieją scenariusze, w których aplikacja wymaga throwaway `IDataProtectionProvider`.</span><span class="sxs-lookup"><span data-stu-id="0a0b2-104">There are scenarios where an application needs a throwaway `IDataProtectionProvider`.</span></span> <span data-ttu-id="0a0b2-105">Na przykład deweloper może po prostu eksperymentować w aplikacji konsoli jednorazowe lub sama aplikacja jest przejściowy (jest pisanie skryptów lub testu jednostkowego do projektu).</span><span class="sxs-lookup"><span data-stu-id="0a0b2-105">For example, the developer might just be experimenting in a one-off console application, or the application itself is transient (it's scripted or a unit test project).</span></span> <span data-ttu-id="0a0b2-106">Do obsługi tych scenariuszy [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) pakietu zawiera typ `EphemeralDataProtectionProvider`.</span><span class="sxs-lookup"><span data-stu-id="0a0b2-106">To support these scenarios the [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) package includes a type `EphemeralDataProtectionProvider`.</span></span> <span data-ttu-id="0a0b2-107">Ten typ zapewnia Podstawowa implementacja `IDataProtectionProvider` których klucza repozytorium jest przechowywany wyłącznie w pamięci i nie jest zapisywany do dowolnego magazynu zapasowego.</span><span class="sxs-lookup"><span data-stu-id="0a0b2-107">This type provides a basic implementation of `IDataProtectionProvider` whose key repository is held solely in-memory and isn't written out to any backing store.</span></span>

<span data-ttu-id="0a0b2-108">Każde wystąpienie `EphemeralDataProtectionProvider` używa własny unikatowy klucz główny.</span><span class="sxs-lookup"><span data-stu-id="0a0b2-108">Each instance of `EphemeralDataProtectionProvider` uses its own unique master key.</span></span> <span data-ttu-id="0a0b2-109">W związku z tym jeśli `IDataProtector` początek w `EphemeralDataProtectionProvider` generuje chronionych ładunku tego ładunku tylko może bez ochrony za pomocą równoważnych `IDataProtector` (podane takie same [celu](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) łańcucha) umieszczone w tym samym `EphemeralDataProtectionProvider` wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="0a0b2-109">Therefore, if an `IDataProtector` rooted at an `EphemeralDataProtectionProvider` generates a protected payload, that payload can only be unprotected by an equivalent `IDataProtector` (given the same [purpose](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) chain) rooted at the same `EphemeralDataProtectionProvider` instance.</span></span>

<span data-ttu-id="0a0b2-110">W poniższym przykładzie pokazano tworzenie wystąpień `EphemeralDataProtectionProvider` i używanie go do zabezpieczania i odbezpieczania danych.</span><span class="sxs-lookup"><span data-stu-id="0a0b2-110">The following sample demonstrates instantiating an `EphemeralDataProtectionProvider` and using it to protect and unprotect data.</span></span>

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
