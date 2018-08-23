---
title: Dodawanie, pobieranie i usuwanie danych niestandardowych użytkownika w tożsamości w projektach programu ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak dodać danych niestandardowych użytkownika w tożsamości w projektach programu ASP.NET Core. Usuń dane na RODO.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
uid: security/authentication/add-user-data
ms.openlocfilehash: 3fe329e21b430cc432e97129ffb8499bce002976
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753638"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Dodawanie, pobieranie i usuwanie danych niestandardowych użytkownika w tożsamości w projektach programu ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym artykule przedstawiono sposób:

* Dodawanie danych niestandardowych użytkownika do aplikacji sieci web platformy ASP.NET Core.
* Dekoracji modelu danych niestandardowych użytkownika za pomocą [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) atrybutu, więc jest automatycznie dostępne do pobrania i usuwania. Udostępnianie danych może zostać pobrana i usunąć pomaga spełnić wymagania [RODO](xref:security/gdpr) wymagania.

Przykładowy projekt jest tworzony na podstawie aplikacja internetowa ze stronami Razor, ale instrukcje są podobne dla aplikacji sieci web platformy ASP.NET Core MVC.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a>Tworzenie aplikacji sieci web Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**. Nadaj projektowi nazwę **WebApp1** Jeśli chcesz go odpowiada przestrzeni nazw z [Pobierz przykładowe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) kodu.
* Wybierz **aplikacji sieci Web platformy ASP.NET Core** > **OK**
* Wybierz **platformy ASP.NET Core 2.1** na liście rozwijanej
* Wybierz **aplikacji sieci Web**  > **OK**
* Skompiluj i uruchom projekt.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

## <a name="run-the-identity-scaffolder"></a>Uruchom Generator szkieletu tożsamości

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy na Projekt > **Dodaj** > **nowy element szkieletu**.
* W okienku po lewej stronie **Dodawanie szkieletu** okno dialogowe, wybierz opcję **tożsamości** > **Dodaj**.
* W **tożsamość usługi ADD** okno dialogowe, następujące opcje:
  * Wybierz istniejący plik układu *~/Pages/Shared/_Layout.cshtml*
  * Wybierz następujące pliki do zastąpienia:
    * **Konto/rejestrowanie**
    * **Konto zarządzania/indeks**
  * Wybierz **+** przycisk, aby utworzyć nową **klasa kontekstu danych**. Akceptuje typu (**WebApp1.Models.WebApp1Context** Jeśli projekt nazwano **WebApp1**).
  * Wybierz **+** przycisk, aby utworzyć nową **klasy użytkownika**. Akceptuje typu (**WebApp1User** Jeśli projekt nazwano **WebApp1**) > **Dodaj**.
* Wybierz **Dodaj**.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Jeśli nie zainstalowano wcześniej Generator szkieletu ASP.NET Core, zainstalowanie go teraz:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Dodaj odwołanie do pakietu [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do pliku projektu (.csproj). Uruchom następujące polecenie w katalogu projektu:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Uruchom następujące polecenie, aby wyświetlić listę opcji Generator szkieletu tożsamości:

```cli
dotnet aspnet-codegenerator identity -h
```

W folderze projektu Uruchom Generator szkieletu tożsamości:

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

Postępuj zgodnie z instrukcjami w [migracje, UseAuthentication i układ](xref:security/authentication/scaffold-identity#efm) można wykonać następujące czynności:

* Utwórz migracji i aktualizują bazę danych.
* Add `UseAuthentication` to `Startup.Configure`.
* Dodaj `<partial name="_LoginPartial" />` do pliku układu.
* Testowanie aplikacji:
  * Rejestrowanie użytkownika
  * Wybierz nową nazwę użytkownika (obok **wylogowania** link). Może być konieczne, rozwiń okno lub wybierz ikonę paska nawigacji, aby pokazać, nazwę użytkownika i inne łącza.
  * Wybierz **danych osobowych** kartę.
  * Wybierz **Pobierz** przycisk i zbadać *PersonalData.json* pliku.
  * Test **Usuń** przycisk, który usuwa zalogowanego użytkownika.

## <a name="add-custom-user-data-to-the-identity-db"></a>Dodawanie danych niestandardowych użytkownika do bazy danych tożsamości

Aktualizacja `IdentityUser` pochodne klasy przy użyciu właściwości niestandardowych. Jeśli nazwę projektu WebApp1, plik nosi *Areas/Identity/Data/WebApp1User.cs*. Aktualizowanie pliku następującym kodem:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

Właściwości ozdobione [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) atrybutu:

* Usuwane, gdy *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* wywołuje strony Razor `UserManager.Delete`.
* Uwzględnione w danych pobranych przez *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* strona Razor.

### <a name="update-the-accountmanageindexcshtml-page"></a>Aktualizuj stronę Account/Manage/Index.cshtml

Aktualizacja `InputModel` w *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* przy użyciu następujących wyróżniony kod:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95,120)]

Aktualizacja *Areas/Identity/Pages/Account/Manage/Index.cshtml* z następujący wyróżniony kod znaczników:

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a>Aktualizuj stronę Account/Register.cshtml

Aktualizacja `InputModel` w *Areas/Identity/Pages/Account/Register.cshtml.cs* przy użyciu następujących wyróżniony kod:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

Aktualizacja *Areas/Identity/Pages/Account/Register.cshtml* z następujący wyróżniony kod znaczników:

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

Skompiluj projekt.

### <a name="add-a-migration-for-the-custom-user-data"></a>Dodaj migrację danych niestandardowych użytkownika

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

## <a name="test-create-view-download-delete-custom-user-data"></a>Test tworzenie, wyświetlanie, pobieranie i usuwanie danych niestandardowych użytkownika

Testowanie aplikacji:

* Rejestrowanie nowego użytkownika.
* Wyświetlanie danych niestandardowych użytkownika na `/Identity/Account/Manage` strony.
* Pobieranie i wyświetlanie danych osobowych użytkowników z `/Identity/Account/Manage/PersonalData` strony.
