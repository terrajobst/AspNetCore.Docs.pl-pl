---
title: 'Samouczek: Używanie funkcji migracji ASP.NET MVC z EF Core'
description: W tym samouczku rozpocznie się korzystanie z funkcji migracji EF Core na potrzeby zarządzania zmianami modelu danych w aplikacji ASP.NET Core MVC.
author: rick-anderson
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/migrations
ms.openlocfilehash: 8b3417205457a5ce5fa16994701a06e2a4d7d350
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665726"
---
# <a name="tutorial-using-the-migrations-feature---aspnet-mvc-with-ef-core"></a>Samouczek: Używanie funkcji migracji ASP.NET MVC z EF Core

W tym samouczku Zacznij korzystać z funkcji migracji EF Core, aby zarządzać zmianami modelu danych. W kolejnych samouczkach dodasz więcej migracji w miarę zmieniania modelu danych.

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Więcej informacji na temat migracji
> * Zmień parametry połączenia
> * Tworzenie początkowej migracji
> * Sprawdzanie metod w górę i w dół
> * Informacje o migawce modelu danych
> * Zastosuj migrację

## <a name="prerequisites"></a>Wymagania wstępne

* [Sortowanie, filtrowanie i stronicowanie](sort-filter-page.md)

## <a name="about-migrations"></a>Informacje o migracjach

Podczas tworzenia nowej aplikacji model danych jest często zmieniany, a przy każdym zmianie modelu jest on synchronizowany z bazą danych. Te samouczki zostały rozpoczęte przez skonfigurowanie Entity Framework w celu utworzenia bazy danych, jeśli nie istnieje. Następnie za każdym razem, gdy zmieniasz model danych — Dodaj, Usuń lub Zmień klasy jednostek lub Zmień klasę DbContext — możesz usunąć bazę danych i EF tworzy nową, która jest zgodna z modelem, i wydawaj ją z danymi testowymi.

Ta metoda zachowania synchronizacji bazy danych z modelem danych działa prawidłowo, dopóki aplikacja nie zostanie wdrożona w środowisku produkcyjnym. Gdy aplikacja jest uruchomiona w środowisku produkcyjnym, zazwyczaj przechowuje dane, które mają być zachowane, i nie ma potrzeby utraty wszystkiego przy każdym wprowadzeniu zmiany, takiej jak dodanie nowej kolumny. Funkcja migracji EF Core rozwiązuje ten problem przez włączenie programu EF w celu zaktualizowania schematu bazy danych zamiast tworzenia nowej bazy danych.

Aby móc korzystać z migracji, można użyć **konsoli Menedżera pakietów** (PMC) lub interfejsu wiersza polecenia.  W tych samouczkach pokazano, jak używać poleceń interfejsu wiersza polecenia. Informacje na temat obiektu PMC są na [końcu tego samouczka](#pmc).

## <a name="change-the-connection-string"></a>Zmień parametry połączenia

W pliku *appSettings. JSON* Zmień nazwę bazy danych w parametrach połączenia na ContosoUniversity2 lub inną nazwę, która nie została użyta na komputerze, którego używasz.

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

Ta zmiana konfiguruje projekt w taki sposób, aby podczas pierwszej migracji utworzyć nową bazę danych. Nie jest to wymagane, aby rozpocząć migrację, ale zobaczysz później, dlaczego jest to dobry pomysł.

> [!NOTE]
> Alternatywą dla zmiany nazwy bazy danych jest usunięcie bazy danych. Użyj **Eksplorator obiektów SQL Server** (SSOX) lub `database drop` interfejsu wiersza polecenia:
>
> ```dotnetcli
> dotnet ef database drop
> ```
>
> W poniższej sekcji opisano sposób uruchamiania poleceń interfejsu wiersza polecenia.

## <a name="create-an-initial-migration"></a>Tworzenie początkowej migracji

Zapisz zmiany i skompiluj projekt. Następnie otwórz okno polecenia i przejdź do folderu projektu. Oto szybka metoda:

* W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz polecenie **Otwórz folder w Eksploratorze plików** z menu kontekstowego.

  ![Otwórz w elemencie menu Eksploratora plików](migrations/_static/open-in-file-explorer.png)

* Wprowadź ciąg "cmd" na pasku adresu i naciśnij klawisz ENTER.

  ![Otwórz okno polecenia](migrations/_static/open-command-window.png)

Wprowadź następujące polecenie w oknie polecenia:

```dotnetcli
dotnet ef migrations add InitialCreate
```

W oknie polecenia są wyświetlane dane wyjściowe podobne do następujących:

```console
info: Microsoft.EntityFrameworkCore.Infrastructure[10403]
      Entity Framework Core 2.2.0-rtm-35687 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> Jeśli zostanie wyświetlony komunikat o błędzie *nie znaleziono pliku wykonywalnego zgodnego z poleceniem "dotnet-EF"* , zobacz [ten wpis w blogu](https://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) , aby uzyskać pomoc w rozwiązywaniu problemów.

Jeśli zostanie wyświetlony komunikat o błędzie "*nie można uzyskać dostępu do pliku... ContosoUniversity. dll, ponieważ jest używany przez inny proces.* ", Znajdź ikonę IIS Express na pasku zadań systemu Windows, a następnie kliknij ją prawym przyciskiem myszy, a następnie kliknij polecenie **ContosoUniversity > Zatrzymaj witrynę**.

## <a name="examine-up-and-down-methods"></a>Sprawdzanie metod w górę i w dół

Po wykonaniu polecenia `migrations add` EF wygenerował kod, który spowoduje utworzenie bazy danych od podstaw. Ten kod znajduje się w folderze *migrations* , w pliku o nazwie *\<timestamp > _InitialCreate. cs*. Metoda `Up` klasy `InitialCreate` tworzy tabele bazy danych, które odpowiadają zestawom jednostek modelu danych, a metoda `Down` usuwa je, jak pokazano w poniższym przykładzie.

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

Migracja wywołuje metodę `Up`, aby zaimplementować zmiany modelu danych dla migracji. Po wprowadzeniu polecenia w celu wycofania aktualizacji migracja wywołuje metodę `Down`.

Ten kod jest przeznaczony dla początkowej migracji, która została utworzona po wprowadzeniu polecenia `migrations add InitialCreate`. Parametr name migracji ("InitialCreate" w przykładzie) jest używany jako nazwa pliku i może być dowolnie wybrany. Najlepiej wybrać słowo lub frazę, która podsumowuje zawartość wykonywaną podczas migracji. Można na przykład nazwać migrację do nowszej wersji "adddepartments".

W przypadku utworzenia początkowej migracji, gdy baza danych już istnieje, kod tworzenia bazy danych jest generowany, ale nie musi działać, ponieważ baza danych jest już zgodna z modelem danych. Po wdrożeniu aplikacji w innym środowisku, w którym baza danych jeszcze nie istnieje, ten kod zostanie uruchomiony w celu utworzenia bazy danych, więc dobrym pomysłem jest przetestowanie go w pierwszej kolejności. Z tego powodu zmieniono nazwę bazy danych w parametrach połączenia wcześniej — tak, aby migracje mogły utworzyć nową od podstaw.

## <a name="the-data-model-snapshot"></a>Migawka modelu danych

Migracja tworzy *migawkę* bieżącego schematu bazy danych w *migracji/SchoolContextModelSnapshot. cs*. Po dodaniu migracji, EF określa, co zmieniło się, porównując model danych z plikiem migawki.

Aby usunąć migrację, użyj polecenia [migracji programu dotnet EF Remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) . `dotnet ef migrations remove` usuwa migrację i gwarantuje, że migawka zostanie prawidłowo zresetowana. Jeśli `dotnet ef migrations remove` nie powiedzie się, użyj `dotnet ef migrations remove -v`, aby uzyskać więcej informacji na temat błędu.

Aby uzyskać więcej informacji na temat sposobu użycia pliku migawek, zobacz [EF Core migracji w środowiskach zespołu](/ef/core/managing-schemas/migrations/teams) .

## <a name="apply-the-migration"></a>Zastosuj migrację

W oknie wiersza polecenia wprowadź następujące polecenie, aby utworzyć bazę danych i tabele.

```dotnetcli
dotnet ef database update
```

Dane wyjściowe polecenia są podobne do polecenia `migrations add`, z tą różnicą, że są wyświetlane dzienniki poleceń SQL, które konfigurują bazę danych. Większość dzienników zostanie pominiętych w następujących przykładowych danych wyjściowych. Jeśli wolisz, aby nie wyświetlać tego poziomu szczegółów w komunikatach dziennika, możesz zmienić poziom dziennika w *appSettings. Plik Development. JSON* . Aby uzyskać więcej informacji, zobacz <xref:fundamentals/logging/index>.

```text
info: Microsoft.EntityFrameworkCore.Infrastructure[10403]
      Entity Framework Core 2.2.0-rtm-35687 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (274ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (60ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      IF SERVERPROPERTY('EngineEdition') <> 5
      BEGIN
          ALTER DATABASE [ContosoUniversity2] SET READ_COMMITTED_SNAPSHOT ON;
      END;
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (15ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20190327172701_InitialCreate', N'2.2.0-rtm-35687');
Done.
```

Użyj **Eksplorator obiektów SQL Server** , aby sprawdzić bazę danych zgodnie z pierwszym samouczkiem.  Zobaczysz dodanie tabeli \_\_EFMigrationsHistory, która śledzi, które migracje zostały zastosowane do bazy danych. Wyświetl dane w tej tabeli i po pierwszej migracji zobaczysz jeden wiersz. (Ostatni dziennik w poprzednim przykładzie danych wyjściowych interfejsu wiersza polecenia przedstawia instrukcję INSERT, która tworzy ten wiersz).

Uruchom aplikację, aby upewnić się, że wszystko nadal działa tak samo jak wcześniej.

![Strona indeksu uczniów](migrations/_static/students-index.png)

<a id="pmc"></a>

## <a name="compare-cli-and-pmc"></a>Porównanie interfejsu wiersza polecenia i kryteriów PMC

Narzędzia EF do zarządzania migracjami są dostępne z poleceń interfejs wiersza polecenia platformy .NET Core lub z poleceń cmdlet programu PowerShell w oknie **konsola Menedżera pakietów** programu Visual Studio (PMC). W tym samouczku pokazano, jak używać interfejsu wiersza polecenia, ale możesz użyć kryterium PMC, jeśli wolisz.

Polecenia EF dla poleceń PMC znajdują się w pakiecie [Microsoft. EntityFrameworkCore. Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) . Ten pakiet jest zawarty w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), dlatego nie trzeba dodawać odwołania do pakietu, jeśli aplikacja zawiera odwołanie do pakietu dla `Microsoft.AspNetCore.App`.

**Ważne:** To nie jest ten sam pakiet, który jest instalowany dla interfejsu wiersza polecenia, edytując plik *csproj* . Nazwa tego elementu zostanie zakończona w `Tools`, w przeciwieństwie do nazwy pakietu interfejsu wiersza polecenia kończącej się w `Tools.DotNet`.

Aby uzyskać więcej informacji na temat poleceń interfejsu wiersza polecenia, zobacz [interfejs wiersza polecenia platformy .NET Core](/ef/core/miscellaneous/cli/dotnet).

Aby uzyskać więcej informacji na temat poleceń PMC, zobacz [konsola Menedżera pakietów (Visual Studio)](/ef/core/miscellaneous/cli/powershell).

## <a name="get-the-code"></a>Uzyskiwanie kodu

[Pobierz lub Wyświetl ukończoną aplikację.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-step"></a>Następny krok

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Zapoznaj się z migracjami
> * Zapoznaj się z pakietami migracji NuGet
> * Zmieniono parametry połączenia
> * Tworzenie początkowej migracji
> * Badanie metod w górę i w dół
> * Informacje o migawce modelu danych
> * Zastosowano migrację

Przejdź do następnego samouczka, aby rozpocząć bardziej zaawansowane tematy dotyczące rozszerzania modelu danych. W sposób tworzenia i stosowania dodatkowych migracji.

> [!div class="nextstepaction"]
> [Tworzenie i stosowanie dodatkowych migracji](complex-data-model.md)
