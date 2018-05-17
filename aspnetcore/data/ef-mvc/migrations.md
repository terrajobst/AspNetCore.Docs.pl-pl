---
title: Platformy ASP.NET Core MVC podstawowych EF - Migrations - 4 10
author: rick-anderson
description: W tym samouczku możesz uruchomić przy użyciu funkcji migracji EF Core zarządzania zmianami modelu danych w aplikacji platformy ASP.NET Core MVC.
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/migrations
ms.openlocfilehash: 0a3ff28c9edefd2c7f96222060a0df76d538012b
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/14/2018
---
# <a name="aspnet-core-mvc-with-ef-core---migrations---4-of-10"></a>Platformy ASP.NET Core MVC podstawowych EF - Migrations - 4 10

Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET Core MVC przy użyciu programu Entity Framework Core i Visual Studio. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](intro.md).

W tym samouczku możesz uruchomić przy użyciu funkcji migracji EF Core zarządzania zmianami modelu danych. W kolejnych samouczkach zostanie dodany więcej migracji w przypadku zmiany modelu danych.

## <a name="introduction-to-migrations"></a>Wprowadzenie do migracji

Podczas opracowywania nowej aplikacji modelu danych zmienia często i zawsze zmian modelu, pobiera zsynchronizowane z bazą danych. Te samouczki jest uruchomiona przez skonfigurowanie programu Entity Framework, aby utworzyć bazę danych, jeśli nie istnieje. Następnie zawsze można zmienić modelu na model danych — dodać, usunąć, lub zmienić klas jednostek lub zmienić klasy DbContext — można usunąć bazy danych i EF tworzy nową, zgodny z modelem, którą nasiona go z danych testowych.

Synchronizacja bazy danych w modelu danych z tej metody działa poprawnie, dopóki możesz wdrożyć aplikację w środowisku produkcyjnym. Gdy aplikacja działa w środowisku produkcyjnym zwykle jest przechowywana dane, które chcesz zachować, a nie chcesz utracić wszystkie zawsze wprowadzeniu zmiany takie jak dodawanie nowej kolumny. Funkcji EF Core migracje rozwiązuje ten problem, należy włączyć EF do aktualizacji schematu bazy danych zamiast tworzenia nowej bazy danych.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Entity Framework Core NuGet pakietów dla migracji

Aby pracować z migracji, można użyć **Konsola Menedżera pakietów** (PMC) lub interfejsu wiersza polecenia (CLI).  Te samouczki wyjaśniają, jak używać polecenia interfejsu wiersza polecenia. Informacje o PMC są obecnie [końcu tego samouczka](#pmc).

Narzędzia EF dla interfejsu wiersza polecenia (CLI) są dostępne w [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Aby zainstalować ten pakiet, należy dodać go do `DotNetCliToolReference` kolekcji w *.csproj* plików, jak pokazano. **Uwaga:** należy zainstalować ten pakiet, edytując *.csproj* pliku; nie można użyć `install-package` polecenia lub graficznego interfejsu użytkownika Menedżera pakietów. Można edytować *.csproj* plików przez kliknięcie prawym przyciskiem myszy nazwę projektu w **Eksploratora rozwiązań** i wybierając **Edytuj ContosoUniversity.csproj**.

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
(Numery wersji w tym przykładzie zostały bieżącej, gdy samouczka został zapisany). 

## <a name="change-the-connection-string"></a>Zmień parametry połączenia

W *appsettings.json* pliku, Zmień nazwę bazy danych w parametrach połączenia ContosoUniversity2 lub inna nazwa, który nie był używany na komputerze korzystasz.

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

Ta zmiana skonfigurowanie projektu, aby pierwszy migracji spowoduje utworzenie nowej bazy danych. Nie jest to wymagane, aby rozpocząć korzystanie z migracji, ale pojawi się później dlaczego jest dobrym rozwiązaniem.

> [!NOTE]
> Alternatywą wobec zmiana nazwy bazy danych można usunąć bazy danych. Użyj **Eksplorator obiektów SQL Server** (SSOX) lub `database drop` polecenia interfejsu wiersza polecenia:
> ```console
> dotnet ef database drop
> ```
> Poniższej sekcji opisano sposób uruchamiania poleceń interfejsu wiersza polecenia.

## <a name="create-an-initial-migration"></a>Tworzenie początkowej migracji

Zapisz zmiany i skompilować projekt. Następnie otwórz okno polecenia i przejdź do folderu projektu. Poniżej przedstawiono szybki sposób, w tym:

* W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Otwórz w Eksploratorze plików** z menu kontekstowego.

  ![Otwórz w Eksploratorze plików elementu menu](migrations/_static/open-in-file-explorer.png)

* Wprowadź "cmd" na pasku adresu i naciśnij klawisz Enter.

  ![Otwórz okno polecenia](migrations/_static/open-command-window.png)

Wprowadź następujące polecenie w oknie wiersza polecenia:

```console
dotnet ef migrations add InitialCreate
```

Zostaną wyświetlone dane wyjściowe podobne do następujących w oknie wiersza polecenia:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> Jeśli zostanie wyświetlony komunikat o błędzie *nie pliku wykonywalnego odnaleźć pasującego polecenia "dotnet-ef"*, zobacz [ten wpis w blogu](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) do rozwiązywania problemów w Pomocy.

Jeśli zostanie wyświetlony komunikat o błędzie "*nie może uzyskać dostępu do pliku... ContosoUniversity.dll, ponieważ jest on używany przez inny proces.* ", odnaleźć usług IIS Express ikonę na pasku zadań systemu Windows i kliknij go prawym przyciskiem myszy, a następnie kliknij przycisk **ContosoUniversity > Zatrzymaj witrynę**.

## <a name="examine-the-up-and-down-methods"></a>Sprawdź w górę i w dół metody

Kiedy wykonać `migrations add` polecenia EF wygenerowanego kodu, który spowoduje utworzenie bazy danych od podstaw. Ten kod jest *migracje* folder, w pliku o nazwie  *\<sygnatury czasowej > _InitialCreate.cs*. `Up` Metody `InitialCreate` klasy tworzy tabele bazy danych, które odpowiadają zestawów jednostek modelu danych, i `Down` metoda usuwa je, jak pokazano w poniższym przykładzie.

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

Migracje wywołania `Up` metody implementacji zmian modelu danych do migracji. Po wprowadzeniu polecenia, aby wycofać aktualizacji, migracje wywołania `Down` metody.

Ten kod jest początkowej migracji, który został utworzony po wprowadzeniu `migrations add InitialCreate` polecenia. Parametr name migracji ("InitialCreate" w przykładzie) jest używany dla nazwy pliku i może być dowolne. Najlepiej wybrać słowo lub frazę podsumowanie, co jest wykonywana w procesie migracji. Na przykład możesz nazwać nowsze migracji "AddDepartmentTable".

Jeśli utworzono początkowej migracji, gdy baza danych już istnieje, jest generowany kod tworzenia bazy danych, ale nie ma działać, ponieważ w bazie danych jest już zgodny z modelem danych. Gdy wdrażasz aplikację do innego środowiska, w których bazy danych nie istnieje jeszcze tego kodu zostaną uruchomione, aby utworzyć bazę danych, dlatego jest dobrym rozwiązaniem jest przetestowanie go. Dlatego zmienić nazwę bazy danych w parametrach połączenia wcześniej — tak, aby migracji można utworzyć nową od początku.

## <a name="the-data-model-snapshot"></a>Migawki modelu danych

Tworzy migracje *migawki* bieżącego schematu bazy danych w *Migrations/SchoolContextModelSnapshot.cs*. Po dodaniu migracji EF określa co zmienione przez porównanie modelu danych do pliku migawki.

Podczas usuwania migracji, użyj [Usuń migracje ef dotnet](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) polecenia. `dotnet ef migrations remove` Usuwa migracji i gwarantuje, że poprawnie zresetowania migawki.

Zobacz [migracje Core EF w środowiskach zespołu](/ef/core/managing-schemas/migrations/teams) Aby uzyskać więcej informacji o sposobie używania pliku migawki.

## <a name="apply-the-migration-to-the-database"></a>Dotyczą migracji bazy danych

W oknie wiersza polecenia wprowadź następujące polecenie, aby utworzyć bazę danych i tabele w nim.

```console
dotnet ef database update
```

Dane wyjściowe polecenia jest podobny do `migrations add` poleceń, z wyjątkiem tego, zobacz dzienniki dla poleceń SQL, który konfigurowania bazy danych. Większość Dzienniki zostały pominięte w następujących przykładowych danych wyjściowych. Jeśli wolisz nie wyświetlić ten poziom szczegółów komunikatów dziennika, można zmienić poziom dziennika w *appsettings. Development.JSON* pliku. Aby uzyskać więcej informacji, zobacz [wprowadzenie do rejestrowania](xref:fundamentals/logging/index).

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

Użyj **Eksplorator obiektów SQL Server** przeprowadzać inspekcję bazy danych, tak jak w pierwszym samouczku.  Można zauważyć dodanie __EFMigrationsHistory tabeli, która przechowuje informacje o migracji, które zostały zastosowane do bazy danych. Wyświetlanie danych w tej tabeli, i zobaczysz jednego wiersza dla pierwszego migracji. (Ostatniego logowania w poprzednim przykładzie danych wyjściowych interfejsu wiersza polecenia pokazuje instrukcji INSERT, która tworzy ten wiersz).

Uruchom aplikację, aby sprawdzić, czy wszystkie elementy nadal działa tak samo jak przed.

![Strona indeksu uczniów lub studentów](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Vs interfejsu wiersza polecenia (CLI). Konsola Menedżera pakietów (PMC)

EF narzędzi do zarządzania migracji jest niedostępna, z platformy .NET Core polecenia interfejsu wiersza polecenia lub poleceń cmdlet programu PowerShell w programie Visual Studio **Konsola Menedżera pakietów** okna (PMC). Ten samouczek przedstawia sposób użycia interfejsu wiersza polecenia, ale jeśli wolisz użyć PMC.

Polecenia EF poleceń PMC znajdują się w [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) pakietu. Ten pakiet jest już uwzględniony w [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, dzięki czemu nie trzeba go zainstalować.

**Ważne:** nie jest tym samym pakiecie, jak zainstalować dla interfejsu wiersza polecenia, edytując *.csproj* pliku. Nazwa tego kończy się `Tools`, w odróżnieniu od nazwy pakietu interfejsu wiersza polecenia, która kończy się `Tools.DotNet`.

Aby uzyskać więcej informacji na temat poleceń interfejsu wiersza polecenia, zobacz [interfejsu wiersza polecenia platformy .NET Core](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet). 

Aby uzyskać więcej informacji na temat poleceń PMC zobacz [Konsola Menedżera pakietów (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób tworzenia i stosowania pierwszy migracji. W następnym samouczku zostaną patrzeć bardziej zaawansowanych tematów, rozwijając modelu danych. Wzdłuż sposób możesz utworzyć i zastosować dodatkowe migracji.

> [!div class="step-by-step"]
> [Poprzednie](sort-filter-page.md)
> [dalej](complex-data-model.md)  
