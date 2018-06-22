---
title: Dodawanie, pobieranie i usuwanie danych użytkownika niestandardowego do tożsamości w projekcie platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak dodawać dane użytkownika niestandardowego do tożsamości w projekcie platformy ASP.NET Core. Usuń dane na GDPR.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
uid: security/authentication/add-user-data
ms.openlocfilehash: ecd0e6d1c71b24309fab70fbb06af7731463bb0e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271960"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Dodawanie, pobieranie i usuwanie danych użytkownika niestandardowego do tożsamości w projekcie platformy ASP.NET Core

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym artykule przedstawiono sposób:

* Dodawanie danych użytkownika do aplikacji sieci web platformy ASP.NET Core.
* Dekoracji modelu danych użytkownika z [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) atrybutu, więc jest automatycznie dostępny do pobrania i usuwanie. Udostępniania danych może zostać pobrana i usunąć pomaga spełnić [GDPR](xref:security/gdpr) wymagania.

Przykładowy projekt został utworzony na podstawie stron Razor aplikacji sieci web, ale instrukcje są podobne dla aplikacji sieci web platformy ASP.NET Core MVC.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a>Tworzenie aplikacji sieci web Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W programie Visual Studio **pliku** menu, wybierz opcję **nowy** > **projektu**. Nazwij projekt **WebApp1** aby go odpowiada przestrzeni nazw z [pobieranie próbki](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) kodu.
* Wybierz **aplikacji sieci Web platformy ASP.NET Core** > **OK**
* Wybierz **platformy ASP.NET Core 2.1** na liście rozwijanej
* Wybierz **aplikacji sieci Web**  > **OK**
* Tworzenie i uruchamianie projektu.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

## <a name="run-the-identity-scaffolder"></a>Uruchamianie tworzenia szkieletu tożsamości

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy na Projekt > **Dodaj** > **nowy element szkieletu**.
* W lewym okienku **Dodawanie szkieletu** okno dialogowe, wybierz opcję **tożsamości** > **dodać**.
* W **tożsamości Dodaj** okno dialogowe, następujące opcje:
  * Wybierz istniejący plik układu *~/Pages/Shared/_Layout.cshtml*
  * Wybierz do zastąpienia następujące pliki:
    * **Rejestr/kont**
    * **Konto/Zarządzanie/indeksu**
  * Wybierz **+** przycisk, aby utworzyć nowy **klasa kontekstu danych**. Akceptuje typu (**WebApp1.Models.WebApp1Context** Jeśli nazwę projektu **WebApp1**).
  * Wybierz **+** przycisk, aby utworzyć nowy **klasy użytkownika**. Akceptuje typu (**WebApp1User** Jeśli nazwę projektu **WebApp1**) > **Dodaj**.
* Wybierz **dodać**.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Jeśli tworzenia szkieletu ASP.NET nie został wcześniej zainstalowany, zainstaluj go:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Dodaj odwołanie do pakietu [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do pliku projektu (.csproj). Uruchom następujące polecenie w katalogu projektu:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Uruchom następujące polecenie, aby wyświetlić listę opcji tworzenia szkieletu tożsamości:

```cli
dotnet aspnet-codegenerator identity -h
```

W folderze projektu Uruchom tworzenia szkieletu tożsamości:

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

Postępuj zgodnie z instrukcjami w [migracji, UseAuthentication i układ](xref:security/authentication/scaffold-identity#efm) można wykonać następujące czynności:

* Utwórz migracji i aktualizacji bazy danych.
* Add `UseAuthentication` to `Startup.Configure`.
* Dodaj `<partial name="_LoginPartial" />` do pliku układu.
* Testowanie aplikacji:
  * Zarejestruj użytkownika
  * Wybierz nową nazwę użytkownika (obok **wylogowania** link). Może być konieczne, rozwiń okno lub wybierz ikony paska nawigacji, aby pokazać nazwy użytkownika i inne łącza.
  * Wybierz **danych osobowych** kartę.
  * Wybierz **Pobierz** przycisk i zbadane *PersonalData.json* pliku.
  * Test **usunąć** przycisku, który usuwa zalogowanego użytkownika.

## <a name="add-custom-user-data-to-the-identity-db"></a>Dodaj użytkownika niestandardowego danych do bazy danych tożsamości

Aktualizacja `IdentityUser` klasy z właściwości niestandardowych. Jeśli nazwa projektu WebApp1 nosi nazwę pliku *Areas/Identity/Data/WebApp1User.cs*. Aktualizacja pliku następującym kodem:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

Właściwości ozdobione [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) są:

* Usunięte, gdy *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* wywołuje Razor strony `UserManager.Delete`.
* Uwzględnione w danych pobranych przez *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor strony.

### <a name="update-the-accountmanageindexcshtml-page"></a>Zaktualizuj strony Account/Manage/Index.cshtml

Aktualizacja `InputModel` w *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* z następującymi wyróżniony kod:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95,120)]

Aktualizacja *Areas/Identity/Pages/Account/Manage/Index.cshtml* z następujący wyróżniony kod znaczników:

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a>Zaktualizuj strony Account/Register.cshtml

Aktualizacja `InputModel` w *Areas/Identity/Pages/Account/Register.cshtml.cs* z następującymi wyróżniony kod:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

Aktualizacja *Areas/Identity/Pages/Account/Register.cshtml* z następujący wyróżniony kod znaczników:

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

Skompiluj projekt.

### <a name="add-a-migration-for-the-custom-user-data"></a>Dodaj migracji danych użytkownika niestandardowego

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W programie Visual Studio **Konsola Menedżera pakietów**:

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a>Test tworzenie, wyświetlanie, Pobierz, Usuń dane użytkownika niestandardowego

Testowanie aplikacji:

* Zarejestruj nowego użytkownika.
* Wyświetl dane użytkownika niestandardowego na `/Identity/Account/Manage` strony.
* Pobierz i Wyświetl danych osobowych użytkowników z `/Identity/Account/Manage/PersonalData` strony.
