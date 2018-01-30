---
title: Stron razor podstawowych EF - Migrations - 4, 8
author: rick-anderson
description: "W tym samouczku możesz uruchomić przy użyciu funkcji migracji EF Core zarządzania zmianami modelu danych w aplikacji ASP.NET Core MVC."
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: e89d95702cb94556bc6e5dc73253c51acaa11578
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a>Migracje - Core EF z samouczka stron Razor (4 8)

Przez [Dykstra Tomasz](https://github.com/tdykstra), [Jan Kowalski P](https://twitter.com/thereformedprog), i [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

W tym samouczku jest używany EF podstawowych funkcji migracji do zarządzania zmianami modelu danych.

Jeśli wystąpiły problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji dla tego etapu](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Gdy do opracowania nowej aplikacji, danych często modelu zmiany. Zawsze zmiany modelu modelu jest niezsynchronizowana z bazą danych. W tym samouczku uruchomiona przez skonfigurowanie programu Entity Framework, aby utworzyć bazę danych, jeśli nie istnieje. Zawsze danych model zmiany:

* Bazy danych zostało przerwane.
* EF tworzy nową, który jest zgodny z modelem.
* Aplikacja nasion bazę danych z danych testowych.

Takie podejście do przechowywania bazy danych w modelu danych działa poprawnie, dopóki wdrażania aplikacji w środowisku produkcyjnym. Gdy aplikacja działa w środowisku produkcyjnym, jest zazwyczaj przechowywana dane, które muszą być kontynuowana. Aplikacja nie może zaczynać się od testu bazy danych zawsze zmianie (na przykład dodawanie nowej kolumny). Funkcji EF Core migracje rozwiązuje ten problem, należy włączyć EF Core zaktualizować schemat bazy danych zamiast tworzenia nowej bazy danych.

Zamiast porzucenie i ponowne utworzenie bazy danych w przypadku zmiany modelu danych, migracje aktualizacje schematu i zachowa istniejące dane.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Entity Framework Core NuGet pakietów dla migracji

Aby pracować z migracji, należy użyć **Konsola Menedżera pakietów** (PMC) lub interfejsu wiersza polecenia (CLI). Te samouczki wyjaśniają, jak używać polecenia interfejsu wiersza polecenia. Informacje o PMC są obecnie [końcu tego samouczka](#pmc).

Narzędzia EF Core dla interfejsu wiersza polecenia (CLI) są dostępne w [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Aby zainstalować ten pakiet, należy dodać go do `DotNetCliToolReference` kolekcji w *.csproj* plików, jak pokazano. **Uwaga:** można zainstalować tego pakietu, edytując *.csproj* pliku. `install-package` Polecenia lub graficznego interfejsu użytkownika Menedżera pakietów nie może służyć do instalowania tego pakietu. Edytuj *.csproj* plików przez kliknięcie prawym przyciskiem myszy nazwę projektu w **Eksploratora rozwiązań** i wybierając **Edytuj ContosoUniversity.csproj**.

Następujący kod przedstawia zaktualizowanego *.csproj* pliku przy użyciu narzędzi interfejsu wiersza polecenia EF Core wyróżnione:

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
Numery wersji w poprzednim przykładzie zostały bieżącej, gdy samouczka został zapisany. Użyj tej samej wersji dla narzędzi EF Core interfejsu wiersza polecenia, dostępnych w innych pakietach.

## <a name="change-the-connection-string"></a>Zmień parametry połączenia

W *appsettings.json* pliku, Zmień nazwę bazy danych w parametrach połączenia ContosoUniversity2.

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

Zmiana nazwy bazy danych w parametrach połączenia powoduje, że pierwszy migracji do utworzenia nowej bazy danych. Nowe bazy danych jest tworzony, ponieważ o tej nazwie nie istnieje. Zmiana parametrów połączenia nie jest wymagane wprowadzenie do migracji.

Zmiana nazwy bazy danych zamiast jest usunięcie bazy danych. Użyj **Eksplorator obiektów SQL Server** (SSOX) lub `database drop` polecenia interfejsu wiersza polecenia:

 ```console
 dotnet ef database drop
 ```

Poniższej sekcji opisano sposób uruchamiania poleceń interfejsu wiersza polecenia.

## <a name="create-an-initial-migration"></a>Tworzenie początkowej migracji

Skompiluj projekt.

Otwórz okno polecenia i przejdź do folderu projektu. Folder projektu zawiera *Startup.cs* pliku.

W oknie wiersza polecenia, wprowadź następujące:

```console
dotnet ef migrations add InitialCreate
```

Okno polecenie wyświetla informacje podobne do następujących:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

Jeśli migracja nie powiedzie się z komunikatem "*nie może uzyskać dostępu do pliku... ContosoUniversity.dll, ponieważ jest on używany przez inny proces.* " wyświetlane są:

* Zatrzymaj usługi IIS Express.

   * Zamknij i uruchom ponownie program Visual Studio, lub
   * Znajdowanie usług IIS Express ikonę na pasku zadań systemu Windows.
   * Kliknij prawym przyciskiem myszy ikonę programu IIS Express, a następnie kliknij przycisk **ContosoUniversity > Zatrzymaj witrynę**.

Jeśli komunikat o błędzie "kompilacja nie powiodła się." zostanie wyświetlona, ponownie uruchom polecenie. Jeśli ten błąd należy pozostawić notatki w dolnej części tego samouczka.

### <a name="examine-the-up-and-down-methods"></a>Sprawdź w górę i w dół metody

Polecenie EF Core `migrations add` wygenerowany kod w celu utworzenia bazy danych z. Ten kod migracji znajduje się w *migracje\<sygnatury czasowej > _InitialCreate.cs* pliku. `Up` Metody `InitialCreate` klasy tworzy tabele bazy danych, które odpowiadają zestawów jednostek modelu danych. `Down` Metoda usuwa je, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

Migracje wywołania `Up` metody implementacji zmian modelu danych do migracji. Po wprowadzeniu polecenia, aby wycofać aktualizacji, migracje wywołania `Down` metody.

Poprzedni kod jest początkowej migracji. Ten kod został utworzony podczas `migrations add InitialCreate` polecenia. Parametr name migracji ("InitialCreate" w przykładzie) jest używany dla nazwy pliku. Nazwa migracji może być dowolną prawidłową nazwę pliku. Najlepiej wybrać słowo lub frazę podsumowanie, co jest wykonywana w procesie migracji. Na przykład migracji, które dodano tabelę działu może mieć nazwę "AddDepartmentTable."

Jeśli zostanie utworzona początkowa migracji i kończy działanie bazy danych:

* Zostaje wygenerowany kod tworzenia bazy danych.
* Kod tworzenia bazy danych nie wymaga się uruchomić, ponieważ bazy danych jest już zgodny z modelem danych. Jeśli uruchomiony jest kod tworzenia bazy danych, nie wprowadzać żadnych zmian, ponieważ bazy danych jest już zgodny z modelem danych.

Po wdrożeniu aplikacji do nowego środowiska, aby utworzyć bazę danych należy uruchomić kod tworzenia bazy danych.

Wcześniej parametry połączenia została zmieniona na nową nazwę dla bazy danych użycia. Określonej bazy danych nie istnieje, więc migracji tworzy bazę danych.

### <a name="examine-the-data-model-snapshot"></a>Sprawdź migawki modelu danych

Tworzy migracje *migawki* bieżącego schematu baz danych w *Migrations/SchoolContextModelSnapshot.cs*:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

Ponieważ bieżący schemat bazy danych jest reprezentowana w kodzie, EF Core nie ma na interakcję z bazy danych, aby utworzyć migracji. Po dodaniu migracji EF Core określa co zmienione przez porównanie modelu danych do pliku migawki. Podstawowe EF współdziała z bazy danych tylko wtedy, gdy musi zaktualizować bazę danych.

Plik migawki musi być zsynchronizowane z migracji, które go utworzył. Nie można usunąć migracji przez usunięcie pliku o nazwie  *\<sygnatury czasowej > _\<migrationname > .cs*. Jeśli ten plik zostanie usunięty, pozostałe migracji nie są zsynchronizowane z plikiem migawki bazy danych. Aby usunąć ostatniego migracji dodany, użyj [Usuń migracje ef dotnet](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) polecenia.

## <a name="remove-ensurecreated"></a>Remove EnsureCreated

Wczesne rozwoju `EnsureCreated` użyto polecenia. W tym samouczku jest używany migracji. `EnsureCreated`ma następujące ograniczenia:

* Pomija migracji i tworzy bazę danych i schematu.
* Nie tworzy tabelę migracji.
* Można *nie* można używać z migracji.
* Jest przeznaczony do testowania lub szybkie tworzenie prototypów gdzie bazy danych jest porzucona i utworzona ponownie często.

Usuń następujący wiersz z `DbInitializer`:

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a>Dotyczą migracji bazę danych w rozwoju

W oknie wiersza polecenia wprowadź następujące polecenie, aby utworzyć bazę danych i tabele.

```console
dotnet ef database update
```

Uwaga: Jeśli `update` polecenie zwraca błąd "Kompilacji nie powiodło się.":

* Ponownie uruchom polecenie.
* Jeśli jej nie powiedzie się ponownie, zamknij program Visual Studio, a następnie uruchom `update` polecenia.
* Pozostaw wiadomości w dolnej części strony.

Dane wyjściowe polecenia jest podobny do `migrations add` danych wyjściowych polecenia. W poprzednim poleceniu dzienniki dla poleceń SQL, które Konfigurowanie bazy danych są wyświetlane. Większość Dzienniki zostały pominięte w następujących przykładowych danych wyjściowych:

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

Aby zmniejszyć poziom szczegółów komunikatów dziennika, można zmienić poziom dziennika w *appsettings. Development.JSON* pliku. Aby uzyskać więcej informacji, zobacz [wprowadzenie do rejestrowania](xref:fundamentals/logging/index).

Użyj **Eksplorator obiektów SQL Server** przeprowadzać inspekcję bazy danych. Zwróć uwagę, dodanie `__EFMigrationsHistory` tabeli. `__EFMigrationsHistory` Tabeli przechowuje informacje o migracji, które zostały zastosowane do bazy danych. Wyświetlanie danych w `__EFMigrationsHistory` tabeli, zawiera jeden wiersz dla pierwszej migracji. Ostatni dziennika w poprzednim przykładzie danych wyjściowych interfejsu wiersza polecenia zawiera instrukcji INSERT, która tworzy tego wiersza.

Uruchom aplikację i sprawdź, czy wszystko działa.

## <a name="appling-migrations-in-production"></a>Appling migracji w środowisku produkcyjnym

Firma Microsoft zaleca aplikacji w środowisku produkcyjnym należy **nie** wywołać [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) podczas uruchamiania aplikacji. `Migrate`Nie można wywołać z aplikacji w farmie serwerów. Na przykład, jeśli aplikacja została chmury z skalowalnego w poziomie (uruchomionych wiele wystąpień aplikacji).

Migracja bazy danych powinno być wykonywane w ramach wdrożenia, a następnie w kontrolowany sposób. Podejścia do produkcyjnej bazy danych migracji obejmują:

* Przy użyciu migracji można utworzyć skrypty SQL i skrypty SQL we wdrożeniu.
* Uruchomiona `dotnet ef database update` z kontrolą środowiska.

Używa EF Core `__MigrationsHistory` tabeli, aby wyświetlić wszystkie migracje trzeba uruchamiać. Jeśli bazy danych jest aktualne, migracja nie jest uruchamiany.

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Vs interfejsu wiersza polecenia (CLI). Konsola Menedżera pakietów (PMC)

Podstawowe EF narzędzi do zarządzania migracji jest dostępne z:

* .NET core polecenia interfejsu wiersza polecenia.
* Polecenia cmdlet programu PowerShell w programie Visual Studio **Konsola Menedżera pakietów** okna (PMC).

Ten samouczek przedstawia sposób użycia interfejsu wiersza polecenia, niektórzy deweloperzy preferowane przy użyciu kryterium.

Polecenia EF Core PMC znajdują się w [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) pakietu. Ten pakiet jest uwzględniona w [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, dzięki czemu nie trzeba go zainstalować.

**Ważne:** nie jest tym samym pakiecie, jak zainstalować dla interfejsu wiersza polecenia, edytując *.csproj* pliku. Nazwa tego kończy się `Tools`, w odróżnieniu od nazwy pakietu interfejsu wiersza polecenia, która kończy się `Tools.DotNet`.

Aby uzyskać więcej informacji na temat poleceń interfejsu wiersza polecenia, zobacz [interfejsu wiersza polecenia platformy .NET Core](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).

Aby uzyskać więcej informacji na temat poleceń PMC zobacz [Konsola Menedżera pakietów (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Pobierz [ukończonej aplikacji dla tego etapu](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Aplikacja generuje następujący wyjątek:

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Rozwiązanie: Uruchom`dotnet ef database update`

Jeśli `update` polecenie zwraca błąd "Kompilacji nie powiodło się.":

* Ponownie uruchom polecenie.
* Pozostaw wiadomości w dolnej części strony.

>[!div class="step-by-step"]
[Poprzednie](xref:data/ef-rp/sort-filter-page)
[dalej](xref:data/ef-rp/complex-data-model)
