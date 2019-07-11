---
title: Dodaj nowe pole na stronę Razor programu ASP.NET Core
author: rick-anderson
description: Pokazuje, jak dodać nowe pole do strony Razor za pomocą platformy Entity Framework Core
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 904207ed775cc689c36953c29d202788580d8f60
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815310"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a>Dodaj nowe pole na stronę Razor programu ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[](~/includes/rp/download.md)]

W tej sekcji [Entity Framework](/ef/core/get-started/aspnetcore/new-db) migracje Code First jest używana do:

* Dodawanie nowego pola do modelu.
* Migruj nowej zmiany schematu pola w bazie danych.

Jeśli przy użyciu programu EF Code First automatycznie utworzyć bazę danych, Code First:

* Dodanie tabeli do sprawdzenia, czy schemat bazy danych jest zsynchronizowany z klasy modelu, który został wygenerowany z bazy danych.
* Jeśli klasy modelu nie są zsynchronizowane z bazy danych, EF zgłasza wyjątek.

Automatyczne weryfikacji/model schematu synchronizacji ułatwia znajdowanie problemów z niespójne bazy danych/code.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Dodawanie właściwości klasyfikacji do modelu Movie

Otwórz *Models/Movie.cs* pliku i Dodaj `Rating` właściwości:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

Tworzenie aplikacji.

Edytuj *Pages/Movies/Index.cshtml*i Dodaj `Rating` pola:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml.?highlight=40-42,61-63)]

Zaktualizuj następujące strony:

* Dodaj `Rating` pola strony Delete i szczegółowe informacje.
* Aktualizacja [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) z `Rating` pola.
* Dodaj `Rating` pole edytowanie strony.

Aplikacja nie będzie działać, dopóki baza danych została zaktualizowana do nowego pola. Jeśli teraz uruchomić zgłasza aplikacji `SqlException`:

`SqlException: Invalid column name 'Rating'.`

Ten błąd jest spowodowany przez zaktualizowane klasy modelu film jest inny niż schemat tabeli filmu bazy danych. (Brak nie `Rating` kolumny w tabeli bazy danych.)

Istnieje kilka sposobów rozwiązania problemu:

1. Ma automatycznie Porzuć i ponownie utworzyć bazę danych przy użyciu nowego schematu klasy modelu Entity Framework. To podejście jest wygodne na wczesnym etapie cyklu tworzenia oprogramowania; Umożliwia szybkie razem rozwijania schematu za jego modelu i bazie danych. Minusem jest to utraty istniejących danych w bazie danych. Nie używaj tego podejścia w produkcyjnej bazie danych! Usunięcie bazy danych na zmiany schematu i automatycznie inicjowanie bazy danych z danymi za pomocą inicjatora jest często produktywny sposób do tworzenia aplikacji.

2. Jawnie zmodyfikować schemat istniejącej bazy danych, aby odpowiadały one klasy modelu. Zaletą tego podejścia jest, aby zachować dane. Można to zrobić to ręcznie lub przez tworzenie bazy danych zmiana skryptu.

3. Aby zaktualizować schemat bazy danych, należy użyć migracje Code First.

W tym samouczku należy użyć migracje Code First.

Aktualizacja `SeedData` klasy tak, aby go oferuje wartości dla nowej kolumny. Poniżej przedstawiono przykładowe zmiany, ale należy to zrobić dla każdego `new Movie` bloku.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

Zobacz [ukończone pliku SeedData.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).

Skompiluj rozwiązanie.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a>Dodaj migrację dla pola klasyfikacji

Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet > Konsola Menedżera pakietów**.
W konsoli zarządzania Pakietami wprowadź następujące polecenia:

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` Polecenie informuje platformę, by:

* Porównaj `Movie` modelu przy użyciu `Movie` schematu bazy danych.
* Utwórz kod, aby migrować schemat bazy danych do nowego modelu.

Nazwa "Ocena" dowolnej i jest używany do nazywania plików migracji. Warto użyć znaczącą nazwę pliku migracji.

`Update-Database` Polecenie informuje platformę, by zastosować zmiany schematu w bazie danych.

<a name="ssox"></a>

Jeśli usuniesz wszystkie rekordy w bazie danych, inicjatora będzie obsługiwał bazy danych i obejmują `Rating` pola. Można to zrobić za pomocą łącza delete w przeglądarce, albo z [Eksplorator obiektów Sql Server](xref:tutorials/razor-pages/sql#ssox) (SSOX).

Innym rozwiązaniem jest usunięcie bazy danych i użyć migracje ponownie utworzyć bazę danych. Aby usunąć bazę danych w SSOX:

* Wybierz bazę danych w SSOX.
* Kliknij prawym przyciskiem myszy w bazie danych, a następnie wybierz pozycję *Usuń*.
* Sprawdź **Zamknij istniejące połączenia**.
* Kliknij przycisk **OK**.
* W [PMC](xref:tutorials/razor-pages/new-field#pmc), aktualizują bazę danych:

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

### <a name="drop-and-re-create-the-database"></a>Porzuć i ponownie utworzyć bazę danych

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

Usuń bazę danych, a następnie użyć migracje ponownie utworzyć bazę danych. Aby usunąć bazy danych, usuń plik bazy danych (*MvcMovie.db*). Następnie uruchom `ef database update` polecenia:

```console
dotnet ef database update
```

---

Uruchom aplikację i sprawdź, można tworzenia/edycji/wyświetlania filmów z `Rating` pola. Jeśli baza danych nie jest obsługiwany, należy ustawić punkt przerwania w `SeedData.Initialize` metody.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wersja usługi YouTube w tym samouczku](https://youtu.be/3i7uMxiGGR8)

> [!div class="step-by-step"]
> [Poprzednie: Dodawanie wyszukiwania](xref:tutorials/razor-pages/search)
> [dalej: Dodawanie walidacji](xref:tutorials/razor-pages/validation)
