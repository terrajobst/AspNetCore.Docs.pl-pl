---
title: Zasady programów w programie ASP.NET Core
author: rick-anderson
description: Schematy zasady uwierzytelniania ułatwiają ma jeden logiczne schemat uwierzytelniania
ms.author: riande
ms.date: 02/28/2019
uid: security/authentication/policyschemes
ms.openlocfilehash: be03f349455c673b0739935ad20e596325c8cb74
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815281"
---
# <a name="policy-schemes-in-aspnet-core"></a>Zasady programów w programie ASP.NET Core

Schematy zasady uwierzytelniania ułatwiają ma jeden logiczne schemat uwierzytelniania za pomocą wielu metod. Na przykład schemat zasad może na użytek uwierzytelniania Google wyzwania i uwierzytelniania plików cookie całą resztą. Uwierzytelnianie zasad schematom:

* Można łatwo przekazywać wszelkie działania uwierzytelniania do innego schematu.
* Do przodu dynamicznie na podstawie żądania.

Wszystkie schematy uwierzytelniania, które pochodne użyj <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> oraz skojarzonych z nimi [ `AuthenticationHandler<TOptions>` ](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):

* Są wykonywane automatycznie schematy zasad w programie ASP.NET Core 2.1 lub nowszej.
* Można ją włączyć za pośrednictwem Konfigurowanie opcje schematu.

[!code-csharp[sample](policyschemes/samples/AuthenticationSchemeOptions.cs?name=snippet)]

## <a name="examples"></a>Przykłady

Poniższy przykład pokazuje wyższe schematu poziomu, który łączy niższym poziomie schematów. Uwierzytelnianie serwisu Google jest używany dla wyzwań i uwierzytelniania plików cookie jest używany dla wszystkich innych:

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet1)]

Poniższy przykład umożliwia dynamiczne Wybieranie schematów na podstawie danego żądania. Oznacza to jak łączyć pliki cookie i uwierzytelniania interfejsu API:

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
