---
title: Dodaj nowe pole do Razor strony platformy ASP.NET Core
author: rick-anderson
description: Pokazuje, jak dodać nowe pole do stron Razor z programu Entity Framework Core
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: d9bf8c7cea20bf38aacf432465d7b33514bcd64d
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277296"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a>Dodaj nowe pole do Razor strony platformy ASP.NET Core

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tej sekcji użyjesz [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) migracje Code First, aby dodać nowe pole do modelu i migracji, które zmiany w bazie danych.

Podczas używania EF Code First, aby automatycznie utworzyć bazę danych, Code First:

* Dodaje tabelę w bazie danych do sprawdzenia, czy schemat bazy danych jest zsynchronizowana z klasy modelu, który został wygenerowany.
* Jeśli klasy modelu nie są zsynchronizowane z bazy danych, EF zgłasza wyjątek. 

Automatyczne weryfikacji schematu/modelu synchronizację ułatwia znaleźć problemy niespójne bazy danych/kod.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Dodawanie właściwości klasyfikacji do modelu film

Otwórz *Models/Movie.cs* plik i dodać `Rating` właściwości:
::: moniker range="= aspnetcore-2.0"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]

::: moniker-end

Tworzenie aplikacji (Ctrl + Shift + B).

Edytuj *Pages/Movies/Index.cshtml*i Dodaj `Rating` pola:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

Dodaj `Rating` pola do usunięcia i szczegóły stron.

Aktualizacja *Create.cshtml* z `Rating` pola. Użytkownik może kopiowania/wklejania poprzedniej `<div>` element i umożliwiają pomoc intelliSense, zaktualizuj pola. IntelliSense współpracuje z [pomocników tagów](xref:mvc/views/tag-helpers/intro).

![Deweloper wpisał litera R dla wartości atrybutu asp — dla w drugi element label w widoku. Menu kontekstowe Intellisense okazało pokazujący dostępne pola, w tym ocenę, która jest automatycznie wyróżnione na liście. Gdy dewelopera kliknie pole lub naciśnie klawisz Enter na klawiaturze, wartość będzie równa klasyfikacji.](new-field/_static/cr.png)

Poniższy kod przedstawia *Create.cshtml* z `Rating` pola:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

Dodaj `Rating` pole do edycji strony.

Aplikacja nie będzie działać, dopóki bazy danych zostanie zaktualizowana, aby uwzględnić nowe pole. Jeśli teraz uruchomić zgłasza aplikacji `SqlException`:

```
SqlException: Invalid column name 'Rating'.
```

Ten błąd jest spowodowany przez zaktualizowane klasy modelu film jest inny niż schemat tabeli filmu bazy danych. (Brak nie `Rating` kolumny w tabeli bazy danych.)

Istnieje kilka sposobów rozwiązania problemu:

1. Ma automatycznie Porzuć i ponownie utworzyć bazę danych przy użyciu nowego schematu klasy modelu Entity Framework. Ta metoda jest wygodne wczesnym etapie cyklu programowanie; pozwala na szybkie razem rozwijać schematu modelu i bazy danych. Wadą podwyższonego jest, że utracić istniejące dane w bazie danych. Nie chcesz użyć tej metody w produkcyjnej bazie danych! Usunięcie bazy danych na zmiany schematu i automatycznie inicjatora bazy danych z danych testowych za pomocą inicjatora jest często produktywności możliwości opracowywania aplikacji.

2. Jawnie modyfikować schemat z istniejącej bazy danych, tak aby był zgodny z klasy modelu. Zaletą tej metody jest, aby zachować dane. Można to zrobić to ręcznie lub przez tworzenie bazy danych należy zmienić skryptu.

3. Użyj migracje Code First, aby zaktualizować schemat bazy danych.

W tym samouczku Użyj migracje Code First.

Aktualizacja `SeedData` klasy, dzięki czemu zapewnia wartość dla nowej kolumny. Poniżej przedstawiono przykładowe zmiany, ale należy to zrobić dla każdego `new Movie` bloku.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

::: moniker range="= aspnetcore-2.0"
Zobacz [ukończone pliku SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
Zobacz [ukończone pliku SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).
::: moniker-end

Skompiluj rozwiązanie.

<a name="pmc"></a> Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet > konsoli Menedżera pakietów**.
W kryterium wprowadź następujące polecenia:

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` Polecenie informuje platformę, by:

* Porównaj `Movie` modelu z `Movie` schemat bazy danych.
* Utwórz kod, aby przeprowadzić migrację schemat bazy danych do nowego modelu.

Nazwa "Klasyfikacja" jest dowolnego i jest używany do nazywania plików migracji. Warto użyć opisową nazwę pliku migracji.

<a name="ssox"></a> Jeśli usuniesz wszystkie rekordy w bazie danych, inicjator będzie inicjatora bazy danych i obejmują `Rating` pola. Można to zrobić z łączami Usuń w przeglądarce lub z [Eksplorator obiektów Sql Server](xref:tutorials/razor-pages/sql#ssox) (SSOX). Aby usunąć bazę danych z SSOX:

* Wybierz bazę danych w SSOX.
* Kliknij prawym przyciskiem myszy w bazie danych, a następnie wybierz *usunąć*.
* Sprawdź **Zamknij istniejące połączenia**.
* Wybierz **OK**.
* W [PMC](xref:tutorials/razor-pages/new-field#pmc), aktualizacji bazy danych:

  ```powershell
  Update-Database
  ```

Uruchom aplikację i sprawdzić, można utworzyć/edycji/wyświetlania filmów `Rating` pola. Jeśli bazy danych nie jest obsługiwany, Zatrzymaj usługi IIS Express, a następnie uruchom aplikację.

> [!div class="step-by-step"]
> [Poprzedni: Dodawanie wyszukiwania](xref:tutorials/razor-pages/search)
> [dalej: Dodawanie walidacji](xref:tutorials/razor-pages/validation)
