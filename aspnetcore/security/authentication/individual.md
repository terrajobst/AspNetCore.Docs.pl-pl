---
title: Artykuły oparte na ASP.NET Core projektach utworzonych przy użyciu poszczególnych kont użytkowników
author: rick-anderson
description: Odkryj artykuły na podstawie ASP.NET Core projektów utworzonych przy użyciu poszczególnych kont użytkowników.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: 91c5665dc50124b3ba09bdcfbf3ba501f684c604
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2019
ms.locfileid: "73463034"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Artykuły oparte na ASP.NET Core projektach utworzonych przy użyciu poszczególnych kont użytkowników

ASP.NET Core tożsamość jest dołączona do szablonów projektu w programie Visual Studio z opcją "indywidualne konta użytkowników".

Szablony uwierzytelniania są dostępne w interfejs wiersza polecenia platformy .NET Core z `-au Individual`:

::: moniker range=">= aspnetcore-2.1"

```dotnetcli
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```dotnetcli
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

Zobacz [ten problem](https://github.com/aspnet/AspNetCore/issues/5833) w usłudze GitHub, aby uzyskać uwierzytelnianie interfejsu API sieci Web.

<a name="no"></a>

## <a name="no-authentication"></a>Bez uwierzytelniania

Uwierzytelnianie jest określone w interfejs wiersza polecenia platformy .NET Core z opcją `-au`. W programie Visual Studio okno dialogowe **Zmienianie uwierzytelniania** jest dostępne dla nowych aplikacji sieci Web. Wartość domyślna dla nowych aplikacji sieci Web w programie Visual Studio **nie jest uwierzytelnianiem**.

Projekty utworzone bez uwierzytelniania:

* Nie zawierają stron sieci Web i interfejsu użytkownika w celu zalogowania się i wylogowania.
* Nie zawierają kodu uwierzytelniania.

<a name="win"></a>

## <a name="windows-authentication"></a>Uwierzytelnianie systemu Windows

Uwierzytelnianie systemu Windows jest określone dla nowych aplikacji sieci Web w interfejs wiersza polecenia platformy .NET Core z opcją `-au Windows`. W programie Visual Studio w oknie dialogowym **Zmienianie uwierzytelniania** dostępne są opcje **uwierzytelniania systemu Windows** .

Jeśli wybrano opcję uwierzytelnianie systemu Windows, aplikacja jest skonfigurowana do korzystania z [modułu usług IIS uwierzytelniania systemu Windows](xref:host-and-deploy/iis/modules). Uwierzytelnianie systemu Windows jest przeznaczone dla intranetowych witryn sieci Web.

## <a name="additional-resources"></a>Dodatkowe zasoby

W poniższych artykułach pokazano, jak używać kodu wygenerowanego w szablonach ASP.NET Core, które używają poszczególnych kont użytkowników:

* [Potwierdzenie konta i odzyskiwanie hasła w ASP.NET Core](xref:security/authentication/accconfirm)
* [Tworzenie aplikacji ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację](xref:security/authorization/secure-data)
