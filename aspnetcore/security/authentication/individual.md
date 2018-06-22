---
title: Artykułów opartych na projektów platformy ASP.NET Core utworzone za pomocą indywidualne konta użytkowników
author: rick-anderson
description: Odnajdywanie artykułów opartych na projektów platformy ASP.NET Core utworzone za pomocą indywidualne konta użytkowników.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: f7fb9e8cd1b5c4cc3283ddd7606a0bbd30f554d5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274430"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Artykułów opartych na projektów platformy ASP.NET Core utworzone za pomocą indywidualne konta użytkowników

Tożsamość platformy ASP.NET Core znajduje się w szablonach projektu w programie Visual Studio z opcją "Indywidualnych kont użytkowników".

Szablony uwierzytelniania są dostępne w .NET Core interfejsu wiersza polecenia z `-au Individual`:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

Następujące artykuły pokazują, jak używać kod wygenerowany w szablonach platformy ASP.NET Core, które używają indywidualne konta użytkowników:

* [Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS](xref:security/authentication/2fa)
* [Potwierdzenie konta i hasła odzyskiwania w ASP.NET Core](xref:security/authentication/accconfirm)
* [Tworzenie aplikacji platformy ASP.NET Core z danych użytkownika chronione przez autoryzacji](xref:security/authorization/secure-data)
