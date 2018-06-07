---
title: Dodawanie do aplikacji stron Razor w ASP.NET Core modelu
author: rick-anderson
description: Dowiedzieć się, jak można dodać klasy do zrządzania filmów w bazie danych przy użyciu Entity Framework Core (EF Core).
manager: wpickett
ms.author: riande
ms.date: 5/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/model
ms.openlocfilehash: 50b1b01448ad870a2889db7dfe8367ab9e661840
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729966"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>Dodawanie do aplikacji stron Razor w ASP.NET Core modelu

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Dodawanie modelu danych

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu > **Dodaj** > **nowy Folder**. Nazwa folderu *modele*.

Kliknij prawym przyciskiem myszy *modele* folderu. Wybierz **dodać** > **klasy**. Nazwa klasy **film** i dodaj następujące właściwości:

Zastąp zawartość `Movie` klasy następującym kodem:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a>Tworzenie szkieletu modelu film

W tej sekcji jest szkieletu modelu filmu. Oznacza to, że narzędzie szkieletów zawiera strony dla operacji tworzenia, odczytu, aktualizacji i usuwania (CRUD) dla modelu filmu.

Utwórz *stron/filmów* folderu:

* W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *stron* folder > **Dodaj** > **nowy Folder**.
* Nazwa folderu *filmy*

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *stron/filmów* folder > **Dodaj** > **nowy element szkieletu**.

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **stron Razor przy użyciu programu Entity Framework (CRUD)** > **dodać**.

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

Zakończenie **Dodaj Razor strony przy użyciu programu Entity Framework (CRUD)** okna dialogowego:

* W **klasa modelu** listy rozwijanej, wybierz **Movie (RazorPagesMovie.Models)**.
* W **klasa kontekstu danych** wierszu, wybierz opcję **+** (oraz) podpisywania i zaakceptuj nazwę wygenerowanego **RazorPagesMovie.Models.RazorPagesMovieContext**.
* W **klasa kontekstu danych** listy rozwijanej, wybierz **RazorPagesMovie.Models.RazorPagesMovieContext**
* Wybierz **dodać**.

![Obraz z poprzednich instrukcji.](model/_static/arp.png)

<a name="pmc"></a>
## <a name="perform-initial-migration"></a>Przeprowadź migrację początkowej

W tej sekcji można użyć do konsoli Menedżera pakietów (PMC):

* Dodaj początkowej migracji.
* Aktualizacji bazy danych z początkowej migracji.

Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.

  ![PMC menu](../first-mvc-app/adding-model/_static/pmc.png)

W kryterium wprowadź następujące polecenia:

```PMC
Add-Migration Initial
Update-Database
```

Alternatywnie można użyć następujących poleceń .NET Core interfejsu wiersza polecenia:

```console
dotnet ef migrations add Initial
dotnet ef database update
```

Ignoruj następujący komunikat ostrzegawczy, możesz rozwiązać ten problem, w następnym samouczku:

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

`Add-Migration` Polecenie generuje kod w celu utworzenia schematu początkowej bazy danych. Schemat jest oparta na modelu określone w `DbContext` (w *Models/MovieContext.cs* pliku). `Initial` Argument jest używany do nazywania migracji. Można użyć dowolnej nazwy, ale Konwencja wybierz nazwę, która opisuje migracji. Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.

`Update-Database` Polecenia `Up` metody w *migracje / {sygnatury czasowej} _InitialCreate.cs* pliku, który utworzy bazę danych.

Jeśli zostanie wyświetlony błąd:

SqlException: Nie można otworzyć bazy danych "RazorPagesMovieContext GUID" żądanego podczas logowania. Logowanie nie powiodło się.
Użytkownik "Nazwa użytkownika" Logowanie nie powiodło się.

Można pominąć [krok migracji](#pmc).
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Dodawanie modelu danych

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu > **Dodaj** > **nowy Folder**. Nazwa folderu *modele*.

Kliknij prawym przyciskiem myszy *modele* folderu. Wybierz **dodać** > **klasy**. Nazwa klasy **film** i dodaj następujące właściwości:

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Dodaj parametry połączenia bazy danych

Dodaj parametry połączenia do *appsettings.json* pliku.

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Zarejestruj kontekst bazy danych

Zarejestruj kontekst bazy danych z [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera w [ConfigureServices metody klasy uruchamiania](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Dodawanie szkieletu narzędzi i przeprowadzić migrację początkowej

W tej sekcji można użyć do konsoli Menedżera pakietów (PMC):

* Dodaj pakiet generowania kodu sieci web programu Visual Studio. Ten pakiet jest wymagana do uruchamiania aparatu szkieletów.
* Dodaj początkowej migracji.
* Aktualizacji bazy danych z początkowej migracji.

Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.

  ![PMC menu](../first-mvc-app/adding-model/_static/pmc.png)

W kryterium wprowadź następujące polecenia:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

Alternatywnie można użyć następujących poleceń .NET Core interfejsu wiersza polecenia:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

Ignoruj następujący komunikat:

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

Napraw który w następnym samouczku.

`Install-Package` Polecenie powoduje zainstalowanie narzędzi wymagane do uruchamiania aparatu szkieletów.

`Add-Migration` Polecenie generuje kod w celu utworzenia schematu początkowej bazy danych. Schemat jest oparta na modelu określone w `DbContext` (w *Models/MovieContext.cs* pliku). `Initial` Argument jest używany do nazywania migracji. Można użyć dowolnej nazwy, ale Konwencja wybierz nazwę, która opisuje migracji. Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.

`Update-Database` Polecenia `Up` metody w *migracje / {sygnatury czasowej} _InitialCreate.cs* pliku, który utworzy bazę danych.

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a>Testowanie aplikacji

* Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).
* Test **Utwórz** łącza.

  ![Tworzenie strony](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Test **Edytuj**, **szczegóły**, i **usunąć** łącza.

Jeśli otrzymasz wyjątek SQL, sprawdź zostało uruchomione migracji i aktualizacji bazy danych.

Następny samouczek wyjaśnia plików utworzonych przez funkcję szkieletów.

> [!div class="step-by-step"]
> [Poprzedni: Rozpoczynanie pracy](xref:tutorials/razor-pages/razor-pages-start)
> [dalej: szkieletu stron Razor](xref:tutorials/razor-pages/page)    
