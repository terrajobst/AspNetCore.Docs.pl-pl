---
title: 'Samouczek: Za pomocą funkcji migracje — ASP.NET MVC z programem EF Core'
description: W ramach tego samouczka możesz rozpocząć korzystanie z funkcji migracje EF Core dla zarządzania zmianami modelu danych w aplikacji ASP.NET Core MVC.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/migrations
ms.openlocfilehash: 3d2ae12bf8eda4f7997008758d4d29434a8371a7
ms.sourcegitcommit: 1a7000630e55da90da19b284e1b2f2f13a393d74
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2019
ms.locfileid: "59012607"
---
# <a name="tutorial-using-the-migrations-feature---aspnet-mvc-with-ef-core"></a>Samouczek: Za pomocą funkcji migracje — ASP.NET MVC z programem EF Core

W ramach tego samouczka możesz rozpocząć korzystanie z funkcji migracje EF Core dla zarządzania zmianami modelu danych. W kolejnych samouczkach należy dodać więcej migracji w przypadku zmiany modelu danych.

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Dowiedz się więcej o migracji
> * Zmień parametry połączenia
> * Tworzenie początkowej migracji
> * Sprawdź metod w górę i w dół
> * Dowiedz się więcej o migawki modelu danych
> * Zastosuj migracji

## <a name="prerequisites"></a>Wymagania wstępne

* [Sortowanie, filtrowanie i stronicowanie](sort-filter-page.md)

## <a name="about-migrations"></a>Dotyczące migracji

Podczas tworzenia nowej aplikacji swój model danych zmienia często i za każdym razem zmiany modelu, otrzymuje zsynchronizowana z bazą danych. Te samouczki są uruchomione przez konfigurowanie platformy Entity Framework do tworzenia bazy danych, jeśli nie istnieje. Następnie zawsze możesz zmienić model danych — dodać, usunąć, lub zmienić klas jednostek lub zmienić klasy DbContext — można usunąć bazy danych i EF utworzony zostaje nowy indeks, który odpowiada modelu i inicjowania inicjuje ją z danymi.

Ta metoda synchronizacja bazy danych z modelem danych działa poprawnie, dopóki nie możesz wdrożyć aplikację do środowiska produkcyjnego. Gdy aplikacja jest uruchomiona w środowisku produkcyjnym zazwyczaj zapisuje dane, które mają być przechowywane i nie chcesz utracić wszystko, czego zawsze upewnij się zmiany, takie jak dodawanie nowej kolumny. Funkcja migracji programu EF Core rozwiązuje ten problem, włączając EF do zaktualizowania schematu bazy danych zamiast tworzenia nowej bazy danych.

Aby pracować z migracji, można użyć **Konsola Menedżera pakietów** (PMC) lub interfejsu wiersza polecenia (CLI).  Tych samouczkach przedstawiono sposób użycia interfejsu wiersza polecenia. Informacje o konsoli zarządzania Pakietami wynosi [po ukończeniu tego samouczka](#pmc).

## <a name="change-the-connection-string"></a>Zmień parametry połączenia

W *appsettings.json* pliku, Zmień nazwę bazy danych w parametrach połączenia ContosoUniversity2 lub innej nazwy, które nie były używane na komputerze używasz.

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

Ta zmiana konfiguruje projekt tak, aby pierwszej migracji utworzy nową bazę danych. Nie jest to wymagane, aby rozpocząć pracę z migracji, ale zostanie wyświetlony później dlaczego jest dobrym pomysłem.

> [!NOTE]
> Alternatywnie można zmienić nazwy bazy danych można usunąć bazy danych. Użyj **Eksplorator obiektów SQL Server** (SSOX) lub `database drop` interfejsu wiersza polecenia:
>
> ```console
> dotnet ef database drop
> ```
>
> Poniższej sekcji opisano sposób uruchamiania poleceń interfejsu wiersza polecenia platformy.

## <a name="create-an-initial-migration"></a>Tworzenie początkowej migracji

Zapisz zmiany i skompiluj projekt. Następnie otwórz okno polecenia i przejdź do folderu projektu. Poniżej przedstawiono szybki sposób, aby to zrobić:

* W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Otwórz Folder w Eksploratorze plików** z menu kontekstowego.

  ![Otwórz w elemencie menu Eksploratora plików](migrations/_static/open-in-file-explorer.png)

* Wprowadź "cmd" na pasku adresu i naciśnij klawisz Enter.

  ![Otwórz okno polecenia](migrations/_static/open-command-window.png)

Wprowadź następujące polecenie w oknie wiersza polecenia:

```console
dotnet ef migrations add InitialCreate
```

Zostaną wyświetlone dane wyjściowe podobne do następujących w oknie wiersza polecenia:

```console
info: Microsoft.EntityFrameworkCore.Infrastructure[10403]
      Entity Framework Core 2.2.0-rtm-35687 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> Jeśli zostanie wyświetlony komunikat o błędzie *nie plik wykonywalny znaleziono pasującego polecenia "dotnet-ef"*, zobacz [ten wpis w blogu](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) do rozwiązywania problemów w Pomocy.

Jeśli zostanie wyświetlony komunikat o błędzie "*uzyskać dostępu do pliku... ContosoUniversity.dll, ponieważ jest on używany przez inny proces.* ", Znajdź ikonę usług IIS Express w zasobniku systemu Windows i kliknij go prawym przyciskiem myszy, a następnie kliknij przycisk **ContosoUniversity > Zatrzymaj witrynę**.

## <a name="examine-up-and-down-methods"></a>Sprawdź metod w górę i w dół

Kiedy wykonać `migrations add` polecenia EF wygenerowany kod, który spowoduje utworzenie bazy danych od podstaw. Ten kod znajduje się w *migracje* folderu, w pliku o nazwie  *\<sygnatura czasowa > _InitialCreate.cs*. `Up` Metody `InitialCreate` klasy tworzy tabele bazy danych, które odpowiadają zestawy jednostek modelu danych i `Down` metoda spowoduje usunięcie ich, jak pokazano w poniższym przykładzie.

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

Migracje wywołania `Up` metodę, aby wdrożyć model danych zmienia się do migracji. Po wprowadzeniu polecenia można wycofać aktualizację, wywołania migracje `Down` metody.

Ten kod jest początkowej migracji, który został utworzony po wprowadzeniu `migrations add InitialCreate` polecenia. Parametr name migracji ("InitialCreate" w przykładzie) jest używany dla nazwy pliku i może być dowolnie. Najlepiej wybrać wyrazu lub frazy, który podsumowuje, co to jest wykonywana w procesie migracji. Na przykład można nazwać późniejszej migracji "AddDepartmentTable".

Jeśli utworzono początkowej migracji bazy danych już istnieje, kod tworzenia bazy danych jest generowany, ale nie ma działać, ponieważ baza danych już jest zgodny z modelem danych. Podczas wdrażania aplikacji do innego środowiska, w którym baza danych nie istnieje jeszcze uruchamiane ten kod, aby utworzyć bazę danych, więc to dobry pomysł, aby przetestować go najpierw. Dlatego właśnie zmieniono nazwę bazy danych w parametrach połączenia wcześniej — tak aby migracji można utworzyć nową od podstaw.

## <a name="the-data-model-snapshot"></a>Migawka modelu danych

Tworzy migracje *migawki* bieżącego schematu bazy danych w *Migrations/SchoolContextModelSnapshot.cs*. Podczas dodawania migracji EF Określa, co zmieniło się przez porównanie modelu danych do pliku migawki.

Podczas usuwania migracji, należy użyć [Usuń migracji ef dotnet](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) polecenia. `dotnet ef migrations remove` Usuwa migracji i gwarantuje, że migawka poprawnie zostanie zresetowana.

Zobacz [EF Core migracji w środowiskach zespołu](/ef/core/managing-schemas/migrations/teams) Aby uzyskać więcej informacji o sposobie korzystania z pliku migawki.

## <a name="apply-the-migration"></a>Zastosuj migracji

W oknie wiersza polecenia wprowadź następujące polecenie, aby utworzyć bazę danych i tabele w nim.

```console
dotnet ef database update
```

Dane wyjściowe polecenia są podobne do `migrations add` poleceń, z tą różnicą, że Zobacz dzienniki dla polecenia SQL, który konfigurowania bazy danych. Większość dzienników zostały pominięte w następujących przykładowych danych wyjściowych. Jeśli wolisz nie wyświetlić ten poziom szczegółów komunikatów dziennika można zmienić poziom dziennika w *appsettings. Development.JSON* pliku. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/logging/index>.

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

Użyj **Eksplorator obiektów SQL Server** do inspekcji bazy danych, tak jak w pierwszym samouczku.  Można zauważyć dodanie \_ \_EFMigrationsHistory tabeli, która przechowuje informacje o migracji, które zostały zastosowane do bazy danych. Wyświetlanie danych w tej tabeli, a następnie zostanie wyświetlony jeden wiersz dla pierwszej migracji. (Ostatni dziennik w powyższym przykładzie danych wyjściowych interfejsu wiersza polecenia wyświetla instrukcji INSERT, która tworzy ten wiersz).

Uruchom aplikację, aby zweryfikować, że wszystko nadal działa tak jak wcześniej.

![Strona indeksu uczniów](migrations/_static/students-index.png)

<a id="pmc"></a>

## <a name="compare-cli-and-pmc"></a>Porównanie interfejsu wiersza polecenia i konsolę zarządzania Pakietami

EF narzędzi do zarządzania migracji jest niedostępna, z poleceń interfejsu wiersza polecenia platformy .NET Core lub poleceń cmdlet programu PowerShell w programie Visual Studio **Konsola Menedżera pakietów** okna (PMC). W tym samouczku pokazano, jak używać interfejsu wiersza polecenia, ale można użyć konsoli zarządzania Pakietami, jeśli użytkownik sobie tego życzy.

Polecenia EF poleceń PMC znajdują się w [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) pakietu. Ten pakiet jest objęta [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), więc nie musisz dodać odwołanie do pakietu, jeśli aplikacja ma odwołania do pakietu dla `Microsoft.AspNetCore.App`.

**Ważne:** Nie jest to ten sam pakiet, instalowanie interfejsu wiersza polecenia, edytując *.csproj* pliku. Nazwa tego z nich kończy się na `Tools`, w odróżnieniu od nazwy pakiet interfejsu wiersza polecenia, którego nazwa kończy się na `Tools.DotNet`.

Aby uzyskać więcej informacji na temat poleceń interfejsu wiersza polecenia, zobacz [interfejsu wiersza polecenia platformy .NET Core](/ef/core/miscellaneous/cli/dotnet).

Aby uzyskać więcej informacji na temat poleceń konsoli zarządzania Pakietami, zobacz [Konsola Menedżera pakietów (Visual Studio)](/ef/core/miscellaneous/cli/powershell).

## <a name="get-the-code"></a>Pobierz kod

[Pobieranie i wyświetlanie ukończonej aplikacji.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-step"></a>Następny krok

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Uzyskaliśmy informacje dotyczące migracji
> * Przedstawia informacje na temat pakietów migracji NuGet
> * Zmienić parametry połączenia
> * Utworzone początkowej migracji
> * Zbadanie metod w górę i w dół
> * Przedstawia informacje na temat migawek modelu danych
> * Stosowane migracji

Przejdź do następnego samouczka, aby rozpocząć spojrzenie na bardziej zaawansowanych tematów dotyczących Rozszerzanie modelu danych. Po drodze możesz utworzyć i zastosować potrzeby dodatkowych migracji.

> [!div class="nextstepaction"]
> [Tworzenie i stosowanie potrzeby dodatkowych migracji](complex-data-model.md)
