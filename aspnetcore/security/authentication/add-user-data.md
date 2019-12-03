---
title: Dodawanie, pobieranie i usuwanie danych użytkownika do tożsamości w projekcie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak dodać niestandardowe dane użytkownika do tożsamości w projekcie ASP.NET Core. Usuń dane na Rodo.
ms.author: riande
ms.date: 06/18/2019
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 6daca5776930f80eec8d81132b5a5c4d4d5c13ad
ms.sourcegitcommit: 0dd224b2b7efca1fda0041b5c3f45080327033f6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/02/2019
ms.locfileid: "74681165"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Dodawanie, pobieranie i usuwanie niestandardowych danych użytkownika do tożsamości w projekcie ASP.NET Core

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym artykule pokazano, jak:

* Dodawanie niestandardowych danych użytkownika do aplikacji sieci Web ASP.NET Core.
* Dekorować niestandardowy model danych użytkownika przy użyciu atrybutu <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute>, aby był on automatycznie dostępny do pobrania i usunięcia. Aby można było pobrać i usunąć dane, można spełnić wymagania [Rodo](xref:security/gdpr) .

Przykład projektu jest tworzony na podstawie Razor Pages aplikacji sieci Web, ale instrukcje są podobne do ASP.NET Core aplikacji sieci Web MVC.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a>Tworzenie aplikacji sieci Web Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* Z menu **plik** programu Visual Studio wybierz pozycję **Nowy** **projekt** > . Nadaj projektowi nazwę **WebApp1** , jeśli chcesz dopasować ją do przestrzeni nazw [przykładowego kodu pobierania](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) .
* Wybierz **ASP.NET Core aplikację sieci Web** > **OK**
* Na liście rozwijanej wybierz pozycję **ASP.NET Core 3,0**
* Wybierz **aplikację sieci Web** > **OK**
* Skompiluj i Uruchom projekt.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Z menu **plik** programu Visual Studio wybierz pozycję **Nowy** **projekt** > . Nadaj projektowi nazwę **WebApp1** , jeśli chcesz dopasować ją do przestrzeni nazw [przykładowego kodu pobierania](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) .
* Wybierz **ASP.NET Core aplikację sieci Web** > **OK**
* Na liście rozwijanej wybierz pozycję **ASP.NET Core 2,2**
* Wybierz **aplikację sieci Web** > **OK**
* Skompiluj i Uruchom projekt.

::: moniker-end


# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a>Uruchamianie szkieletu tożsamości

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt > **Dodaj** > **nowy element szkieletowy**.
* W lewym okienku okna dialogowego **Dodawanie szkieletu** wybierz pozycję **tożsamość** > **Dodaj**.
* W oknie dialogowym **Dodawanie tożsamości** można wykonać następujące czynności:
  * Wybierz istniejący plik układu *~/Pages/Shared/_Layout. cshtml*
  * Wybierz następujące pliki do przesłonięcia:
    * **Konto/rejestr**
    * **Konto/Zarządzanie/indeks**
  * Wybierz przycisk **+** , aby utworzyć nową **klasę kontekstu danych**. Zaakceptuj typ (**WebApp1. models. WebApp1Context** , jeśli projekt jest nazwany **WebApp1**).
  * Wybierz przycisk **+** , aby utworzyć nową **klasę użytkownika**. Zaakceptuj typ (**WebApp1User** , jeśli projekt ma nazwę **WebApp1**) > **Dodaj**.
* Wybierz pozycję **Dodaj**.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Jeśli jeszcze nie zainstalowano szkieletu ASP.NET Core, zainstaluj go teraz:

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Dodaj odwołanie do pakietu do [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do pliku projektu (. csproj). Uruchom następujące polecenie w katalogu projektu:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Uruchom następujące polecenie, aby wyświetlić listę opcji szkieletu tożsamości:

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

W folderze projektu uruchom program Identity rusztowaer:

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

Postępuj zgodnie z instrukcjami w sekcji [migracje, UseAuthentication i układ](xref:security/authentication/scaffold-identity#efm) , aby wykonać następujące czynności:

* Utwórz migrację i zaktualizuj bazę danych.
* Add `UseAuthentication` to `Startup.Configure`.
* Dodaj `<partial name="_LoginPartial" />` do pliku układu.
* Przetestuj aplikację:
  * Rejestrowanie użytkownika
  * Wybierz nową nazwę użytkownika (obok linku **wylogowania** ). Może być konieczne rozszerzenie okna lub wybranie ikony paska nawigacyjnego, aby wyświetlić nazwę użytkownika i inne linki.
  * Wybierz kartę **dane osobowe** .
  * Wybierz przycisk **Pobierz** i zbadaj plik *personaldata. JSON* .
  * Przetestuj przycisk **Usuń** , który spowoduje usunięcie zalogowanego użytkownika.

## <a name="add-custom-user-data-to-the-identity-db"></a>Dodawanie niestandardowych danych użytkownika do bazy danych tożsamości

Zaktualizuj klasę pochodną `IdentityUser` przy użyciu właściwości niestandardowych. Jeśli nazwa projektu WebApp1, plik ma nazwę *obszary/tożsamość/Data/WebApp1User. cs*. Zaktualizuj plik przy użyciu następującego kodu:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

Właściwości oznaczone atrybutem [personaldata](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) są następujące:

* Usunięte, gdy strona Razor *obszary/tożsamość/Pages/Account/Manage/DeletePersonalData. cshtml* jest wywoływana `UserManager.Delete`.
* Zawarte w pobranych danych przez stronę *obszary/tożsamość/strony/konto/Zarządzanie/DownloadPersonalData. cshtml* Razor.

### <a name="update-the-accountmanageindexcshtml-page"></a>Aktualizowanie strony account/Manage/index. cshtml

Zaktualizuj `InputModel` w *obszarze obszary/tożsamość/strony/konto/Zarządzanie/index. cshtml. cs* przy użyciu następującego wyróżnionego kodu:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

Zaktualizuj *obszary/tożsamość/strony/konto/Zarządzanie/index. cshtml* przy użyciu następującego wyróżnionego znacznika:

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

Zaktualizuj *obszary/tożsamość/strony/konto/Zarządzanie/index. cshtml* przy użyciu następującego wyróżnionego znacznika:

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a>Aktualizowanie strony account/register. cshtml

Zaktualizuj `InputModel` w *obszarach/Identity/Pages/Account/register. cshtml. cs* przy użyciu następującego wyróżnionego kodu:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

Zaktualizuj *obszary/tożsamość/strony/account/register. cshtml* przy użyciu następującego wyróżnionego znacznika:

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

Zaktualizuj *obszary/tożsamość/strony/account/register. cshtml* przy użyciu następującego wyróżnionego znacznika:

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


Skompiluj projekt.

### <a name="add-a-migration-for-the-custom-user-data"></a>Dodawanie migracji niestandardowych danych użytkownika

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W **konsoli Menedżera pakietów**programu Visual Studio:

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a>Testowanie tworzenia, wyświetlania, pobierania, usuwania niestandardowych danych użytkownika

Przetestuj aplikację:

* Zarejestruj nowego użytkownika.
* Wyświetlanie niestandardowych danych użytkownika na stronie `/Identity/Account/Manage`.
* Pobierz i Przejrzyj dane osobowe użytkowników na stronie `/Identity/Account/Manage/PersonalData`.
