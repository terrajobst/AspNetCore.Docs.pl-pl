---
title: Dodaj nowe pole na stronę Razor programu ASP.NET Core
author: rick-anderson
description: Pokazuje, jak dodać nowe pole do strony Razor za pomocą platformy Entity Framework Core
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: d6d59ff336095e2f1b8b2e9a0338b7791605ad7a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010900"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a>Dodaj nowe pole na stronę Razor programu ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tej sekcji użyjesz [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) migracje Code First dodaje nowe pole do modelu i migracji, które zmiany w bazie danych.

Jeśli przy użyciu programu EF Code First automatycznie utworzyć bazę danych, Code First:

* Dodanie tabeli do sprawdzenia, czy schemat bazy danych jest zsynchronizowany z klasy modelu, który został wygenerowany z bazy danych.
* Jeśli klasy modelu nie są zsynchronizowane z bazy danych, EF zgłasza wyjątek. 

Automatyczne weryfikacji/model schematu synchronizacji ułatwia znajdowanie problemów z niespójne bazy danych/code.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Dodawanie właściwości klasyfikacji do modelu Movie

Otwórz *Models/Movie.cs* pliku i Dodaj `Rating` właściwości:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]

::: moniker-end

Utwórz aplikację (Ctrl + Shift + B).

Edytuj *Pages/Movies/Index.cshtml*i Dodaj `Rating` pola:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

Dodaj `Rating` pola strony Delete i szczegółowe informacje.

Aktualizacja *Create.cshtml* z `Rating` pola. Możesz skopiować lub wkleić poprzedniego `<div>` elementu, dzięki czemu pomoc intelliSense, zaktualizuj pola. Technologia IntelliSense działa z [pomocników tagów](xref:mvc/views/tag-helpers/intro).

![Deweloper wpisał literę R wartość atrybutu asp — dla w elemencie drugiego etykiety widoku. Menu kontekstowe Intellisense okazało, wyświetlanie dostępnych pól, w tym klasyfikacji, który jest automatycznie wyróżniona na liście. Gdy deweloper kliknie pole lub naciśnie klawisz Enter na klawiaturze, wartość zostanie ustawiona na ocenę.](new-field/_static/cr.png)

Poniższy kod przedstawia *Create.cshtml* z `Rating` pola:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

Dodaj `Rating` pole edytowanie strony.

Aplikacja nie będzie działać, dopóki baza danych została zaktualizowana do nowego pola. Jeśli teraz uruchomić zgłasza aplikacji `SqlException`:

```
SqlException: Invalid column name 'Rating'.
```

Ten błąd jest spowodowany przez zaktualizowane klasy modelu film jest inny niż schemat tabeli filmu bazy danych. (Brak nie `Rating` kolumny w tabeli bazy danych.)

Istnieje kilka sposobów rozwiązania problemu:

1. Ma automatycznie Porzuć i ponownie utworzyć bazę danych przy użyciu nowego schematu klasy modelu Entity Framework. To podejście jest wygodne na wczesnym etapie cyklu tworzenia oprogramowania; Umożliwia szybkie razem rozwijania schematu za jego modelu i bazie danych. Minusem jest to utraty istniejących danych w bazie danych. Nie chcesz użyć tej metody w produkcyjnej bazie danych! Usunięcie bazy danych na zmiany schematu i automatycznie inicjowanie bazy danych z danymi za pomocą inicjatora jest często produktywny sposób do tworzenia aplikacji.

2. Jawnie zmodyfikować schemat istniejącej bazy danych, aby odpowiadały one klasy modelu. Zaletą tego podejścia jest, aby zachować dane. Można to zrobić to ręcznie lub przez tworzenie bazy danych zmiana skryptu.

3. Aby zaktualizować schemat bazy danych, należy użyć migracje Code First.

W tym samouczku należy użyć migracje Code First.

Aktualizacja `SeedData` klasy tak, aby go oferuje wartości dla nowej kolumny. Poniżej przedstawiono przykładowe zmiany, ale należy to zrobić dla każdego `new Movie` bloku.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

::: moniker range="= aspnetcore-2.0"

Zobacz [ukończone pliku SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Zobacz [ukończone pliku SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).

::: moniker-end

Skompiluj rozwiązanie.

<a name="pmc"></a> Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet > Konsola Menedżera pakietów**.
W konsoli zarządzania Pakietami wprowadź następujące polecenia:

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` Polecenie informuje platformę, by:

* Porównaj `Movie` modelu przy użyciu `Movie` schematu bazy danych.
* Utwórz kod, aby migrować schemat bazy danych do nowego modelu.

Nazwa "Ocena" dowolnej i jest używany do nazywania plików migracji. Warto użyć znaczącą nazwę pliku migracji.

<a name="ssox"></a> Jeśli usuniesz wszystkie rekordy w bazie danych, inicjatora będzie obsługiwał bazy danych i obejmują `Rating` pola. Można to zrobić za pomocą łącza delete w przeglądarce, albo z [Eksplorator obiektów Sql Server](xref:tutorials/razor-pages/sql#ssox) (SSOX). Aby usunąć bazy danych z SSOX:

* Wybierz bazę danych w SSOX.
* Kliknij prawym przyciskiem myszy w bazie danych, a następnie wybierz pozycję *Usuń*.
* Sprawdź **Zamknij istniejące połączenia**.
* Wybierz **OK**.
* W [PMC](xref:tutorials/razor-pages/new-field#pmc), aktualizują bazę danych:

  ```powershell
  Update-Database
  ```

Uruchom aplikację i sprawdź, można tworzenia/edycji/wyświetlania filmów z `Rating` pola. Jeśli baza danych nie jest obsługiwany, Zatrzymaj usługi IIS Express, a następnie uruchom aplikację.

> [!div class="step-by-step"]
> [Poprzedni: Dodawanie wyszukiwania](xref:tutorials/razor-pages/search)
> [dalej: Dodawanie walidacji](xref:tutorials/razor-pages/validation)
