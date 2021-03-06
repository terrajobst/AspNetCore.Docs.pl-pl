---
title: Artykuły oparte na ASP.NET Core projektach utworzonych przy użyciu poszczególnych kont użytkowników
author: rick-anderson
description: Odkryj artykuły na podstawie ASP.NET Core projektów utworzonych przy użyciu poszczególnych kont użytkowników.
ms.author: riande
ms.date: 12/11/2019
uid: security/authentication/individual
ms.openlocfilehash: 7ef0d5eabded61d04fb9fe7be384a663ad7ea5f4
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659622"
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

Zobacz [ten problem](https://github.com/dotnet/AspNetCore/issues/5833) w usłudze GitHub, aby uzyskać uwierzytelnianie interfejsu API sieci Web.

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

## <a name="dotnet-new-webapp-authentication-options"></a>Opcje uwierzytelniania w programie dotnet New webapp

W poniższej tabeli przedstawiono opcje uwierzytelniania dostępne dla nowych aplikacji sieci Web:

| Opcja | Typ uwierzytelniania | Link, aby uzyskać więcej informacji |
 | ----------------- | ------------ | ---------- |
| None            |  Brak uwierzytelniania | | 
| Szczegółowe      |  Uwierzytelnianie indywidualne | <xref:security/authentication/identity>
| IndividualB2C   |  Uwierzytelnianie indywidualne hostowane w chmurze za pomocą Azure AD B2C | [Azure AD B2C](/azure/active-directory-b2c/) |
| SingleOrg       |  Uwierzytelnianie organizacyjne dla pojedynczej dzierżawy | [Azure AD](/azure/active-directory/develop/quickstart-v2-aspnet-core-webapp) |
| MultiOrg        |  Uwierzytelnianie organizacyjne dla wielu dzierżawców | [Azure AD](/azure/active-directory/develop/quickstart-v2-aspnet-core-webapp) |
| System Windows         |  Uwierzytelnianie systemu Windows | [Uwierzytelnianie systemu Windows](xref:security/authentication/windowsauth)

## <a name="visual-studio-new-webapp-authentication-options"></a>Opcje uwierzytelniania programu Visual Studio New webapp

W poniższej tabeli przedstawiono opcje uwierzytelniania dostępne podczas tworzenia nowej aplikacji sieci Web przy użyciu programu Visual Studio:

| Opcja | Typ uwierzytelniania | Link, aby uzyskać więcej informacji |
 | ----------------- | ------------ | ---------- |
| None            |  Brak uwierzytelniania | | 
| Konta użytkowników indywidualnych/konta użytkowników sklepu w aplikacji |  Uwierzytelnianie indywidualne | <xref:security/authentication/identity> |
| Indywidualne konta użytkowników/łączenie z istniejącym magazynem użytkowników w chmurze |  Uwierzytelnianie indywidualne hostowane w chmurze za pomocą Azure AD B2C | [Azure AD B2C](/azure/active-directory-b2c/) |
| Chmura służbowa/jedna organizacja  |  Uwierzytelnianie organizacyjne dla pojedynczej dzierżawy | [Azure AD](/azure/active-directory/develop/quickstart-v2-aspnet-core-webapp) |
| Chmura służbowa/usługa szkoły i wiele organizacji |  Uwierzytelnianie organizacyjne dla wielu dzierżawców | [Azure AD](/azure/active-directory/develop/quickstart-v2-aspnet-core-webapp) |
| System Windows         |  Uwierzytelnianie systemu Windows | [Uwierzytelnianie systemu Windows](xref:security/authentication/windowsauth)

## <a name="additional-resources"></a>Dodatkowe zasoby

W poniższych artykułach pokazano, jak używać kodu wygenerowanego w szablonach ASP.NET Core, które używają poszczególnych kont użytkowników:

* [Potwierdzenie konta i odzyskiwanie hasła w ASP.NET Core](xref:security/authentication/accconfirm)
* [Tworzenie aplikacji ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację](xref:security/authorization/secure-data)
