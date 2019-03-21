---
title: Artykułów opartych na projektach programu ASP.NET Core utworzona za pomocą indywidualnych kont użytkowników
author: rick-anderson
description: Dowiedz się, artykułów opartych na projektach programu ASP.NET Core utworzona za pomocą indywidualnych kont użytkowników.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: f9c1be16386da935382275815bb5fd5c72894b1c
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265433"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Artykułów opartych na projektach programu ASP.NET Core utworzona za pomocą indywidualnych kont użytkowników

Tożsamość platformy ASP.NET Core znajduje się w szablonach projektu w programie Visual Studio przy użyciu opcji "Pojedyncze konta użytkowników".

Szablony uwierzytelniania są dostępne w interfejsie wiersza polecenia platformy .NET Core za pomocą `-au Individual`:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

Zobacz [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore/issues/5833) uwierzytelniania interfejsu API sieci web.

<a name="no"></a>

## <a name="no-authentication"></a>Bez uwierzytelniania

Uwierzytelnianie jest określona w .NET Core interfejsu wiersza polecenia przy użyciu `-au` opcji. W programie Visual Studio **Zmień uwierzytelnianie** okno dialogowe jest dostępna dla nowych aplikacji sieci web. Wartość domyślna w przypadku nowych aplikacji sieci web w programie Visual Studio to **bez uwierzytelniania**.

Projekty utworzone za pomocą bez uwierzytelniania:

* Nie zawierają stron sieci web i interfejs użytkownika do logowania i Wyloguj.
* Nie zawierają kod uwierzytelniania.

<a name="win"></a>

## <a name="windows-authentication"></a>Uwierzytelnianie systemu Windows

Uwierzytelnianie Windows jest określona dla nowej aplikacji sieci web w .NET Core interfejsu wiersza polecenia przy użyciu `-au Windows` opcji. W programie Visual Studio **Zmień uwierzytelnianie** okno dialogowe udostępnia **uwierzytelniania Windows** opcje.

Jeśli wybrano opcję uwierzytelniania Windows, aplikacja jest skonfigurowana do używania [Moduł IIS uwierzytelniania Windows](xref:host-and-deploy/iis/modules). Uwierzytelnianie Windows jest przeznaczony dla witryn intranetowych.

## <a name="additional-resources"></a>Dodatkowe zasoby

Następujące artykuły pokazują, jak używać kod wygenerowany w szablony ASP.NET Core, korzystających z indywidualnych kont użytkowników:

* [Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS](xref:security/authentication/2fa)
* [Potwierdzenie konta i odzyskiwanie hasła w programie ASP.NET Core](xref:security/authentication/accconfirm)
* [Tworzenie aplikacji platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację](xref:security/authorization/secure-data)
