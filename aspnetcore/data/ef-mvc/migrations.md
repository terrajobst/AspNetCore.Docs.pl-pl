---
title: Platforma ASP.NET Core MVC z programem EF Core - Migrations - 4 z 10
author: rick-anderson
description: W ramach tego samouczka możesz rozpocząć korzystanie z funkcji migracje EF Core dla zarządzania zmianami modelu danych w aplikacji ASP.NET Core MVC.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-mvc/migrations
ms.openlocfilehash: 21ef3a675579d8a6671343d84cbe4f4b62979679
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090813"
---
# <a name="aspnet-core-mvc-with-ef-core---migrations---4-of-10"></a>Platforma ASP.NET Core MVC z programem EF Core - Migrations - 4 z 10

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

Przez [Tom Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Przykładową aplikację sieci web firmy Contoso University pokazuje, jak tworzyć aplikacje sieci web platformy ASP.NET Core MVC za pomocą platformy Entity Framework Core i Visual Studio. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](intro.md).

W ramach tego samouczka możesz rozpocząć korzystanie z funkcji migracje EF Core dla zarządzania zmianami modelu danych. W kolejnych samouczkach należy dodać więcej migracji w przypadku zmiany modelu danych.

## <a name="introduction-to-migrations"></a>Wprowadzenie do migracji

Podczas tworzenia nowej aplikacji swój model danych zmienia często i za każdym razem zmiany modelu, otrzymuje zsynchronizowana z bazą danych. Te samouczki są uruchomione przez konfigurowanie platformy Entity Framework do tworzenia bazy danych, jeśli nie istnieje. Następnie zawsze możesz zmienić model danych — dodać, usunąć, lub zmienić klas jednostek lub zmienić klasy DbContext — można usunąć bazy danych i EF utworzony zostaje nowy indeks, który odpowiada modelu i inicjowania inicjuje ją z danymi.

Ta metoda synchronizacja bazy danych z modelem danych działa poprawnie, dopóki nie możesz wdrożyć aplikację do środowiska produkcyjnego. Gdy aplikacja jest uruchomiona w środowisku produkcyjnym zazwyczaj zapisuje dane, które mają być przechowywane i nie chcesz utracić wszystko, czego zawsze upewnij się zmiany, takie jak dodawanie nowej kolumny. Funkcja migracji programu EF Core rozwiązuje ten problem, włączając EF do zaktualizowania schematu bazy danych zamiast tworzenia nowej bazy danych.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Entity Framework Core NuGet pakietów dla migracji

Aby pracować z migracji, można użyć **Konsola Menedżera pakietów** (PMC) lub interfejsu wiersza polecenia (CLI).  Tych samouczkach przedstawiono sposób użycia interfejsu wiersza polecenia. Informacje o konsoli zarządzania Pakietami wynosi [po ukończeniu tego samouczka](#pmc).

Narzędzia platformy EF dla interfejsu wiersza polecenia (CLI) znajdują się w [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Aby zainstalować ten pakiet, należy dodać go do `DotNetCliToolReference` kolekcji w *.csproj* pliku, jak pokazano. **Uwaga:** należy zainstalować ten pakiet, edytując *.csproj* pliku; nie można użyć `install-package` polecenia lub graficznego interfejsu użytkownika Menedżera pakietów. Możesz edytować *.csproj* pliku, klikając prawym przyciskiem myszy nazwę projektu w **Eksploratora rozwiązań** i wybierając polecenie **Edytuj ContosoUniversity.csproj**.

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]

(Numery wersji, w tym przykładzie zostały bieżącej, gdy samouczek został napisany).

## <a name="change-the-connection-string"></a>Zmień parametry połączenia

W *appsettings.json* pliku, Zmień nazwę bazy danych w parametrach połączenia ContosoUniversity2 lub innej nazwy, które nie były używane na komputerze używasz.

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

Ta zmiana konfiguruje projekt tak, aby pierwszej migracji utworzy nową bazę danych. Nie jest to wymagane, aby rozpocząć pracę z migracji, ale zostanie wyświetlony później dlaczego jest dobrym pomysłem.

> [!NOTE]
> Alternatywnie można zmienić nazwy bazy danych można usunąć bazy danych. Użyj **Eksplorator obiektów SQL Server** (SSOX) lub `database drop` interfejsu wiersza polecenia:
> ```console
> dotnet ef database drop
> ```
> Poniższej sekcji opisano sposób uruchamiania poleceń interfejsu wiersza polecenia platformy.

## <a name="create-an-initial-migration"></a>Tworzenie początkowej migracji

Zapisz zmiany i skompiluj projekt. Następnie otwórz okno polecenia i przejdź do folderu projektu. Poniżej przedstawiono szybki sposób, aby to zrobić:

* W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Otwórz w Eksploratorze plików** z menu kontekstowego.

  ![Otwórz w elemencie menu Eksploratora plików](migrations/_static/open-in-file-explorer.png)

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
> Jeśli zostanie wyświetlony komunikat o błędzie *nie plik wykonywalny znaleziono pasującego polecenia "dotnet-ef"*, zobacz [ten wpis w blogu](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) do rozwiązywania problemów w Pomocy.

Jeśli zostanie wyświetlony komunikat o błędzie "*uzyskać dostępu do pliku... ContosoUniversity.dll, ponieważ jest on używany przez inny proces.* ", Znajdź ikonę usług IIS Express w zasobniku systemu Windows i kliknij go prawym przyciskiem myszy, a następnie kliknij przycisk **ContosoUniversity > Zatrzymaj witrynę**.

## <a name="examine-the-up-and-down-methods"></a>Sprawdź w górę i w dół metody

Kiedy wykonać `migrations add` polecenia EF wygenerowany kod, który spowoduje utworzenie bazy danych od podstaw. Ten kod znajduje się w *migracje* folderu, w pliku o nazwie  *\<sygnatura czasowa > _InitialCreate.cs*. `Up` Metody `InitialCreate` klasy tworzy tabele bazy danych, które odpowiadają zestawy jednostek modelu danych i `Down` metoda spowoduje usunięcie ich, jak pokazano w poniższym przykładzie.

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

Migracje wywołania `Up` metodę, aby wdrożyć model danych zmienia się do migracji. Po wprowadzeniu polecenia można wycofać aktualizację, wywołania migracje `Down` metody.

Ten kod jest początkowej migracji, który został utworzony po wprowadzeniu `migrations add InitialCreate` polecenia. Parametr name migracji ("InitialCreate" w przykładzie) jest używany dla nazwy pliku i może być dowolnie. Najlepiej wybrać wyrazu lub frazy, który podsumowuje, co to jest wykonywana w procesie migracji. Na przykład można nazwać późniejszej migracji "AddDepartmentTable".

Jeśli utworzono początkowej migracji bazy danych już istnieje, kod tworzenia bazy danych jest generowany, ale nie ma działać, ponieważ baza danych już jest zgodny z modelem danych. Podczas wdrażania aplikacji do innego środowiska, w którym baza danych nie istnieje jeszcze uruchamiane ten kod, aby utworzyć bazę danych, więc to dobry pomysł, aby przetestować go najpierw. Dlatego właśnie zmieniono nazwę bazy danych w parametrach połączenia wcześniej — tak aby migracji można utworzyć nową od podstaw.

## <a name="the-data-model-snapshot"></a>Migawka modelu danych

Tworzy migracje *migawki* bieżącego schematu bazy danych w *Migrations/SchoolContextModelSnapshot.cs*. Podczas dodawania migracji EF Określa, co zmieniło się przez porównanie modelu danych do pliku migawki.

Podczas usuwania migracji, należy użyć [Usuń migracji ef dotnet](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) polecenia. `dotnet ef migrations remove` Usuwa migracji i gwarantuje, że migawka poprawnie zostanie zresetowana.

Zobacz [EF Core migracji w środowiskach zespołu](/ef/core/managing-schemas/migrations/teams) Aby uzyskać więcej informacji o sposobie korzystania z pliku migawki.

## <a name="apply-the-migration-to-the-database"></a>Dotyczą migracji bazy danych

W oknie wiersza polecenia wprowadź następujące polecenie, aby utworzyć bazę danych i tabele w nim.

```console
dotnet ef database update
```

Dane wyjściowe polecenia są podobne do `migrations add` poleceń, z tą różnicą, że Zobacz dzienniki dla polecenia SQL, który konfigurowania bazy danych. Większość dzienników zostały pominięte w następujących przykładowych danych wyjściowych. Jeśli wolisz nie wyświetlić ten poziom szczegółów komunikatów dziennika można zmienić poziom dziennika w *appsettings. Development.JSON* pliku. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/logging/index>.

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

Użyj **Eksplorator obiektów SQL Server** do inspekcji bazy danych, tak jak w pierwszym samouczku.  Można zauważyć dodanie \_ \_EFMigrationsHistory tabeli, która przechowuje informacje o migracji, które zostały zastosowane do bazy danych. Wyświetlanie danych w tej tabeli, a następnie zostanie wyświetlony jeden wiersz dla pierwszej migracji. (Ostatni dziennik w powyższym przykładzie danych wyjściowych interfejsu wiersza polecenia wyświetla instrukcji INSERT, która tworzy ten wiersz).

Uruchom aplikację, aby zweryfikować, że wszystko nadal działa tak jak wcześniej.

![Strona indeksu uczniów](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>W stosunku do interfejsu wiersza polecenia (CLI) Konsola Menedżera pakietów (PMC)

EF narzędzi do zarządzania migracji jest niedostępna, z poleceń interfejsu wiersza polecenia platformy .NET Core lub poleceń cmdlet programu PowerShell w programie Visual Studio **Konsola Menedżera pakietów** okna (PMC). W tym samouczku pokazano, jak używać interfejsu wiersza polecenia, ale można użyć konsoli zarządzania Pakietami, jeśli użytkownik sobie tego życzy.

Polecenia EF poleceń PMC znajdują się w [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) pakietu. Ten pakiet jest objęta [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), więc nie musisz dodać odwołanie do pakietu, jeśli aplikacja ma odwołania do pakietu dla `Microsoft.AspNetCore.App`.

**Ważne:** nie jest to ten sam pakiet, instalowanie interfejsu wiersza polecenia, edytując *.csproj* pliku. Nazwa tego z nich kończy się na `Tools`, w odróżnieniu od nazwy pakiet interfejsu wiersza polecenia, którego nazwa kończy się na `Tools.DotNet`.

Aby uzyskać więcej informacji na temat poleceń interfejsu wiersza polecenia, zobacz [interfejsu wiersza polecenia platformy .NET Core](/ef/core/miscellaneous/cli/dotnet).

Aby uzyskać więcej informacji na temat poleceń konsoli zarządzania Pakietami, zobacz [Konsola Menedżera pakietów (Visual Studio)](/ef/core/miscellaneous/cli/powershell).

## <a name="summary"></a>Podsumowanie

W tym samouczku pokazaliśmy już, jak tworzenie i stosowanie pierwszej migracji. W następnym samouczku rozpocznie się spojrzenie na bardziej zaawansowanych tematów, rozwijając modelu danych. Po drodze możesz utworzyć i zastosować potrzeby dodatkowych migracji.

::: moniker-end

> [!div class="step-by-step"]
> [Poprzednie](sort-filter-page.md)
> [dalej](complex-data-model.md)
