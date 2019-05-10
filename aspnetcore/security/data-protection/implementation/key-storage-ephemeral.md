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
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a><span data-ttu-id="65545-103">Dostawcy efemerycznej ochrony danych w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="65545-103">Ephemeral data protection providers in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-ephemeral"></a>

<span data-ttu-id="65545-104">Istnieją scenariusze, w której aplikacja musi throwaway `IDataProtectionProvider`.</span><span class="sxs-lookup"><span data-stu-id="65545-104">There are scenarios where an application needs a throwaway `IDataProtectionProvider`.</span></span> <span data-ttu-id="65545-105">Na przykład deweloper może po prostu eksperymentować w aplikacji konsoli jednorazowe lub sama aplikacja jest przejściowy (jest pisanie skryptów lub projekt testów jednostkowych).</span><span class="sxs-lookup"><span data-stu-id="65545-105">For example, the developer might just be experimenting in a one-off console application, or the application itself is transient (it's scripted or a unit test project).</span></span> <span data-ttu-id="65545-106">Do obsługi tych scenariuszy [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) pakietu zawiera typ `EphemeralDataProtectionProvider`.</span><span class="sxs-lookup"><span data-stu-id="65545-106">To support these scenarios the [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) package includes a type `EphemeralDataProtectionProvider`.</span></span> <span data-ttu-id="65545-107">Ten typ zapewnia podstawową implementację `IDataProtectionProvider` którego klucza repozytorium są przechowywane wyłącznie w pamięci i nie jest zapisywane w dowolnym magazynie pomocniczym.</span><span class="sxs-lookup"><span data-stu-id="65545-107">This type provides a basic implementation of `IDataProtectionProvider` whose key repository is held solely in-memory and isn't written out to any backing store.</span></span>

<span data-ttu-id="65545-108">Każde wystąpienie `EphemeralDataProtectionProvider` używa swój własny unikatowy klucz główny.</span><span class="sxs-lookup"><span data-stu-id="65545-108">Each instance of `EphemeralDataProtectionProvider` uses its own unique master key.</span></span> <span data-ttu-id="65545-109">W związku z tym jeśli `IDataProtector` na `EphemeralDataProtectionProvider` generuje ładunek chronionego, ten ładunek tylko można bez ochrony przez odpowiednik `IDataProtector` (biorąc pod uwagę taki sam [przeznaczenia](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) łańcucha) dostęp do konta root, w tym samym `EphemeralDataProtectionProvider` wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="65545-109">Therefore, if an `IDataProtector` rooted at an `EphemeralDataProtectionProvider` generates a protected payload, that payload can only be unprotected by an equivalent `IDataProtector` (given the same [purpose](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) chain) rooted at the same `EphemeralDataProtectionProvider` instance.</span></span>

<span data-ttu-id="65545-110">W poniższym przykładzie pokazano tworzenie wystąpienia `EphemeralDataProtectionProvider` i używanie ich do włączania i wyłączania ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="65545-110">The following sample demonstrates instantiating an `EphemeralDataProtectionProvider` and using it to protect and unprotect data.</span></span>

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
