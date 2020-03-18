---
title: Przykłady uwierzytelniania dla ASP.NET Core
author: rick-anderson
description: Zawiera łącza do przykładów uwierzytelniania w repozytorium ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: b013d02a65e752bbb61a87a0bf502785125378a2
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511616"
---
# <a name="authentication-samples-for-aspnet-core"></a>Przykłady uwierzytelniania dla ASP.NET Core

Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

[Repozytorium ASP.NET Core](https://github.com/dotnet/AspNetCore) zawiera następujące przykłady uwierzytelniania w folderze *AspNetCore/src/Security/Samples* :

* [Przekształcanie oświadczeń](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/ClaimsTransformation)
* [Uwierzytelnianie plików cookie](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Cookies)
* [Niestandardowy dostawca zasad — IAuthorizationPolicyProvider](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)
* [Dynamiczne schematy i opcje uwierzytelniania](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/DynamicSchemes)
* [Oświadczenia zewnętrzne](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Identity.ExternalClaims)
* [Wybieranie między plikiem cookie a innym schematem uwierzytelniania na podstawie żądania](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/PathSchemeSelection)
* [Ogranicza dostęp do plików statycznych](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Uruchamianie przykładów

* Wybierz [gałąź](https://github.com/dotnet/AspNetCore). Na przykład: `Tag:v3.0.0`
* Klonuj lub Pobierz [repozytorium ASP.NET Core](https://github.com/dotnet/AspNetCore).
* Sprawdź, czy zainstalowano wersję [zestaw .NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) zgodną z klonem ASP.NET Core repozytorium.
* Przejdź do przykładu w *AspNetCore/src/Security/Samples* i uruchom przykład za pomocą `dotnet run`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[Repozytorium ASP.NET Core](https://github.com/dotnet/AspNetCore) zawiera następujące przykłady uwierzytelniania w folderze *AspNetCore/src/Security/Samples* :

* [Przekształcanie oświadczeń](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Uwierzytelnianie plików cookie](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [Niestandardowy dostawca zasad — IAuthorizationPolicyProvider](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [Dynamiczne schematy i opcje uwierzytelniania](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [Oświadczenia zewnętrzne](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [Wybieranie między plikiem cookie a innym schematem uwierzytelniania na podstawie żądania](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [Ogranicza dostęp do plików statycznych](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Uruchamianie przykładów

* Wybierz [gałąź](https://github.com/dotnet/AspNetCore). Na przykład: `release/2.2`
* Klonuj lub Pobierz [repozytorium ASP.NET Core](https://github.com/dotnet/AspNetCore).
* Sprawdź, czy zainstalowano wersję [zestaw .NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) zgodną z klonem ASP.NET Core repozytorium.
* Przejdź do przykładu w *AspNetCore/src/Security/Samples* i uruchom przykład za pomocą `dotnet run`.

::: moniker-end
