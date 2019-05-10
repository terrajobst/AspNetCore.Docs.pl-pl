---
title: Zasady programów w programie ASP.NET Core
author: rick-anderson
description: Schematy zasady uwierzytelniania ułatwiają ma jeden logiczne schemat uwierzytelniania
ms.author: riande
ms.date: 2/28/2019
uid: security/authentication/policyschemes
ms.openlocfilehash: c310b61e14df2b7846e32a602bb75914a5850aff
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901648"
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

Poniższy przykład umożliwia dynamiczne Wybieranie schematów na podstawie danego żądania. Oznacza to jak łączyć pliki cookie i uwierzytelniania interfejsu API.

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
