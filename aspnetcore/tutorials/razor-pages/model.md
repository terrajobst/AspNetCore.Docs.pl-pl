---
title: Dodawanie modelu strony Razor aplikacji w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak można dodać klas związanych z zarządzaniem filmów w bazie danych przy użyciu platformy Entity Framework Core (EF Core).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 41a88e06afbe6e7accd03ff7b39aa69e15e0c0b4
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325816"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>Dodawanie modelu strony Razor aplikacji w programie ASP.NET Core

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Dodawanie modelu danych

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** Projekt > **Dodaj** > **nowy Folder**. Nazwa folderu *modeli*.

Kliknij prawym przyciskiem myszy *modeli* folderu. Wybierz **Dodaj** > **klasy**. Nazwa klasy **filmu** i Zastąp zawartość `Movie` klasy z następującym kodem:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a>Tworzenie szkieletu modelu movie

W tej sekcji modelu movie jest szkielet. Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.

Tworzenie *stron/filmów* folderu:

* W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *stron* folder > **Dodaj** > **nowy Folder**.
* Nazwa folderu *filmy*

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *stron/filmów* folder > **Dodaj** > **nowy element szkieletu**.

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **strony Razor za pomocą programu Entity Framework (CRUD)** > **Dodaj**.

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

Wykonaj **dodać strony Razor za pomocą programu Entity Framework (CRUD)** okno dialogowe:

* W **klasa modelu** listę rozwijaną, wybierz **Movie (RazorPagesMovie.Models)**.
* W **klasa kontekstu danych** wiersz, wybierz opcję **+** (plus) Zaloguj się i zaakceptuj wygenerowaną nazwę **RazorPagesMovie.Models.RazorPagesMovieContext**.
* Wybierz **Dodaj**.

![Obraz z poprzednich instrukcji.](model/_static/arp.png)

Proces szkieletu tworzy i aktualizuje następujące pliki:

### <a name="files-created"></a>Utworzone pliki

* *Strony/filmów*: tworzenie, edytowanie usuwania, uzyskać szczegółowe informacje, indeksu. Te strony są szczegółowo opisane w następnym samouczku.
* *Data/RazorPagesMovieContext.cs*

### <a name="file-updated"></a>Zaktualizowano plik

* *Startup.cs*: zmiany w tym pliku opisano szczegółowo w następnej sekcji.
* *appSettings.JSON*: parametry połączenia używane do łączenia z lokalnej bazy danych zostanie dodany.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności

Platforma ASP.NET Core został utworzony za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection). Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji. Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora. Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.

Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.

Sprawdź `Startup.ConfigureServices` metody. Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

Główna klasa, która służy do koordynowania funkcji EF Core dla modelu danych jest z klasy kontekstu bazy danych. Kontekst danych jest tworzony na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Kontekst danych określa, które jednostki są uwzględnione w modelu danych. W tym projekcie nosi nazwę klasy `RazorPagesMovieContext`.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

Powyższy kod tworzy [DbSet\<film >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) właściwość zestawu jednostek. W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych. Jednostki odnosi się do wiersza w tabeli.

Nazwa ciągu połączenia jest przekazywany do kontekstu przez wywołanie metody na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu. Na potrzeby lokalnego programowania dla [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.

<a name="pmc"></a>
## <a name="perform-initial-migration"></a>Przeprowadzenie początkowej migracji

W tej sekcji użyjesz konsoli Menedżera pakietów (PMC) do:

* Dodaj początkowej migracji.
* Zaktualizuj bazy danych przy użyciu początkowej migracji.

Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.

  ![Menu konsoli zarządzania Pakietami](../first-mvc-app/adding-model/_static/pmc.png)

W konsoli zarządzania Pakietami wprowadź następujące polecenia:

```PMC
Add-Migration Initial
Update-Database
```

Alternatywnie można następujące polecenia interfejsu wiersza polecenia platformy .NET Core z folderu projektu:

```console
dotnet ef migrations add Initial
dotnet ef database update
```

Ignoruj następujący komunikat ostrzegawczy, naprawić w dalszych samouczków:

```console
Microsoft.EntityFrameworkCore.Model.Validation[30000]
      No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.
```

`Add-Migration` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych. Schemat jest oparta na modelu, określone w `RazorPagesMovieContext` (w *Data/RazorPagesMovieContext.cs* pliku). `Initial` Argument jest używany do nazywania migracje. Można użyć dowolnej nazwy, ale zgodnie z Konwencją wybierz nazwę, która opisuje migracji. Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.

`Update-Database` Polecenia `Up` method in Class metoda *migracje / {sygnatura czasowa} _InitialCreate.cs* pliku, który tworzy bazę danych.

Jeśli zostanie wyświetlony błąd:

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

Możesz pominąć [krok migracji](#pmc).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Dodawanie modelu danych

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** Projekt > **Dodaj** > **nowy Folder**. Nazwa folderu *modeli*.

Kliknij prawym przyciskiem myszy *modeli* folderu. Wybierz **Dodaj** > **klasy**. Nazwa klasy **filmu** i dodaj następujące właściwości:

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Dodaj parametry połączenia bazy danych

Dodaj parametry połączenia, aby *appsettings.json* pliku.

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Zarejestruj kontekst bazy danych

Zarejestruj kontekst bazy danych za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera w [ConfigureServices metody klasy uruchamiania](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Dodawanie szkieletu narzędzi i wykonywania początkowej migracji

W tej sekcji użyjesz konsoli Menedżera pakietów (PMC) do:

* Dodaj pakiet generowania kodu sieci web programu Visual Studio. Ten pakiet jest wymagana do uruchamiania aparatu tworzenia szkieletów.
* Dodaj początkowej migracji.
* Zaktualizuj bazy danych przy użyciu początkowej migracji.

Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.

  ![Menu konsoli zarządzania Pakietami](../first-mvc-app/adding-model/_static/pmc.png)

W konsoli zarządzania Pakietami wprowadź następujące polecenia:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

Alternatywnie można skorzystać z następujących poleceń interfejsu wiersza polecenia platformy .NET Core:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

Ignoruj następujący komunikat:

```console
Microsoft.EntityFrameworkCore.Model.Validation[30000]
      No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'
```

Które naprawić w następnym samouczku.

`Install-Package` Polecenie instaluje narzędzia wymagane do uruchamiania aparatu tworzenia szkieletów.

`Add-Migration` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych. Schemat jest oparta na modelu, określone w `DbContext` (w *Models/MovieContext.cs* pliku). `Initial` Argument jest używany do nazywania migracje. Można użyć dowolnej nazwy, ale zgodnie z Konwencją wybierz nazwę, która opisuje migracji. Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.

`Update-Database` Polecenia `Up` method in Class metoda *migracje / {sygnatura czasowa} _InitialCreate.cs* pliku, który tworzy bazę danych.

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a>Testowanie aplikacji

* Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).
* Test **Utwórz** łącza.

  ![Tworzenie strony](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Test **Edytuj**, **szczegóły**, i **Usuń** łącza.

Jeśli wystąpi wyjątek SQL, należy sprawdzić, masz Uruchom migracje i aktualizacji bazy danych.

Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.

> [!div class="step-by-step"]
> [Poprzedni: Rozpoczynanie pracy](xref:tutorials/razor-pages/razor-pages-start)
> [dalej: działanie stron Razor](xref:tutorials/razor-pages/page)
