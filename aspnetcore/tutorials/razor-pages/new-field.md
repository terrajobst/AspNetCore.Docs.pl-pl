---
title: Dodaj nowe pole do strony Razor w ASP.NET Core
author: rick-anderson
description: Pokazuje, jak dodać nowe pole do strony Razor przy użyciu Entity Framework Core
ms.author: riande
ms.custom: mvc
ms.date: 7/23/2019
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 0629605f4d5597a9694cb20ce00b91ff4a768468
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082468"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a>Dodaj nowe pole do strony Razor w ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

W tej sekcji [Entity Framework](/ef/core/get-started/aspnetcore/new-db) migracje Code First służy do:

* Dodaj nowe pole do modelu.
* Migruj nową zmianę schematu pola do bazy danych.

W przypadku automatycznego tworzenia bazy danych przy użyciu narzędzia EF Code First Code First:

* Dodaje tabelę do bazy danych, aby sprawdzić, czy schemat bazy danych jest zsynchronizowany z klasami modelu, z których została wygenerowana.
* Jeśli klasy modelu nie są zsynchronizowane z bazą danych, EF zgłasza wyjątek.

Automatyczna weryfikacja schematu/modelu w ramach synchronizacji ułatwia znalezienie niespójnych problemów z bazą danych i kodem.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Dodawanie właściwości oceny do modelu filmu

Otwórz plik *models/Movie. cs* i Dodaj `Rating` Właściwość:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRating.cs?highlight=13&name=snippet)]

Skompiluj aplikację.

Edycja *stron/filmów/index. cshtml*i Dodawanie `Rating` pola:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/SnapShots/IndexRating.cshtml?highlight=40-42,62-64)]

Aktualizowanie następujących stron:

* `Rating` Dodaj pole do stron usuwanie i szczegóły.
* Zaktualizuj element `Rating` [Create. cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml) przy użyciu pola.
* `Rating` Dodaj pole do strony Edycja.

Aplikacja nie będzie działała, dopóki baza danych nie zostanie zaktualizowana w celu uwzględnienia nowego pola. Jeśli uruchomisz teraz, aplikacja zgłasza `SqlException`:

`SqlException: Invalid column name 'Rating'.`

Ten błąd jest spowodowany przez zaktualizowaną klasę filmu inną niż schemat tabeli filmów bazy danych. (Brak `Rating` kolumn w tabeli bazy danych).

Istnieje kilka metod rozpoznawania błędu:

1. Entity Framework automatycznie porzucić i ponownie utworzyć bazę danych przy użyciu nowego schematu klasy modelu. Takie podejście jest wygodnie wczesne w cyklu rozwoju; pozwala ona szybko rozwijać model i schemat bazy danych. Minusem polega na utracie istniejących danych w bazie danych. Nie używaj tego podejścia w produkcyjnej bazie danych. Porzucenie bazy danych w ramach zmian schematu i użycie inicjatora do automatycznego wypełniania bazy danych za pomocą danych testowych jest często wydajnym sposobem na tworzenie aplikacji.

2. Jawnie zmodyfikuj schemat istniejącej bazy danych, tak aby pasował do klas modelu. Zaletą tego podejścia jest utrzymywanie danych. Tę zmianę można wprowadzić ręcznie lub przez utworzenie skryptu zmiany bazy danych.

3. Użyj Migracje Code First, aby zaktualizować schemat bazy danych.

Na potrzeby tego samouczka Użyj Migracje Code First.

`SeedData` Zaktualizuj klasę, aby zapewnić wartość nowej kolumny. Poniżej przedstawiono przykładową zmianę, ale trzeba wprowadzić tę zmianę dla każdego `new Movie` bloku.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

Zobacz [ukończony plik SeedData.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/SeedDataRating.cs).

Skompiluj rozwiązanie.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a>Dodawanie migracji dla pola oceny

W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet > konsola Menedżera pakietów**.
W konsoli zarządzania Pakietami wprowadź następujące polecenia:

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` Polecenie informuje platformę, aby:

* Porównaj model ze schematem `Movie`bazydanych. `Movie`
* Utwórz kod, aby zmigrować schemat bazy danych do nowego modelu.

Nazwa "Rating" jest arbitralna i jest używana do nazwy pliku migracji. Warto użyć zrozumiałej nazwy dla pliku migracji.

`Update-Database` Polecenie informuje platformę, aby zastosować zmiany schematu do bazy danych programu.

<a name="ssox"></a>

W przypadku usunięcia wszystkich rekordów w bazie danych inicjator będzie wypełniać bazę danych i zawierać `Rating` pole. Można to zrobić za pomocą linków usuwania w przeglądarce lub z [programu SQL Server Eksplorator obiektów](xref:tutorials/razor-pages/sql#ssox) (SSOX).

Innym rozwiązaniem jest usunięcie bazy danych i użycie migracji w celu ponownego utworzenia bazy danych. Aby usunąć bazę danych w programie SSOX:

* Wybierz bazę danych w SSOX.
* Kliknij prawym przyciskiem myszy bazę danych, a następnie wybierz pozycję *Usuń*.
* Zaznacz pole wyboru **Zamknij istniejące połączenia**.
* Kliknij przycisk **OK**.
* W obszarze [PMC](xref:tutorials/razor-pages/new-field#pmc)zaktualizuj bazę danych:

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

### <a name="drop-and-re-create-the-database"></a>Porzuć i ponownie utwórz bazę danych

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

Usuń folder migracji.  Użyj następujących poleceń, aby ponownie utworzyć bazę danych.

```dotnetcli
dotnet ef database drop
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

Uruchom aplikację i sprawdź, czy można tworzyć/edytować/wyświetlać filmy z `Rating` polem. Jeśli baza danych nie jest zainicjowana, ustaw punkt przerwania `SeedData.Initialize` w metodzie.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wersja tego samouczka usługi YouTube](https://youtu.be/3i7uMxiGGR8)

> [!div class="step-by-step"]
> [Ubiegł Dodawanie wyszukiwania](xref:tutorials/razor-pages/search)
> dalej[: Dodawanie walidacji](xref:tutorials/razor-pages/validation)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

W tej sekcji [Entity Framework](/ef/core/get-started/aspnetcore/new-db) migracje Code First służy do:

* Dodaj nowe pole do modelu.
* Migruj nową zmianę schematu pola do bazy danych.

W przypadku automatycznego tworzenia bazy danych przy użyciu narzędzia EF Code First Code First:

* Dodaje tabelę do bazy danych, aby sprawdzić, czy schemat bazy danych jest zsynchronizowany z klasami modelu, z których została wygenerowana.
* Jeśli klasy modelu nie są zsynchronizowane z bazą danych, EF zgłasza wyjątek.

Automatyczna weryfikacja schematu/modelu w ramach synchronizacji ułatwia znalezienie niespójnych problemów z bazą danych i kodem.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Dodawanie właściwości oceny do modelu filmu

Otwórz plik *models/Movie. cs* i Dodaj `Rating` Właściwość:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

Skompiluj aplikację.

Edycja *stron/filmów/index. cshtml*i Dodawanie `Rating` pola:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml?highlight=40-42,61-63)]

Aktualizowanie następujących stron:

* `Rating` Dodaj pole do stron usuwanie i szczegóły.
* Zaktualizuj element `Rating` [Create. cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) przy użyciu pola.
* `Rating` Dodaj pole do strony Edycja.

Aplikacja nie będzie działała, dopóki baza danych nie zostanie zaktualizowana w celu uwzględnienia nowego pola. Jeśli uruchomisz teraz, aplikacja zgłasza `SqlException`:

`SqlException: Invalid column name 'Rating'.`

Ten błąd jest spowodowany przez zaktualizowaną klasę filmu inną niż schemat tabeli filmów bazy danych. (Brak `Rating` kolumn w tabeli bazy danych).

Istnieje kilka metod rozpoznawania błędu:

1. Entity Framework automatycznie porzucić i ponownie utworzyć bazę danych przy użyciu nowego schematu klasy modelu. Takie podejście jest wygodnie wczesne w cyklu rozwoju; pozwala ona szybko rozwijać model i schemat bazy danych. Minusem polega na utracie istniejących danych w bazie danych. Nie używaj tego podejścia w produkcyjnej bazie danych. Porzucenie bazy danych w ramach zmian schematu i użycie inicjatora do automatycznego wypełniania bazy danych za pomocą danych testowych jest często wydajnym sposobem na tworzenie aplikacji.

2. Jawnie zmodyfikuj schemat istniejącej bazy danych, tak aby pasował do klas modelu. Zaletą tego podejścia jest utrzymywanie danych. Tę zmianę można wprowadzić ręcznie lub przez utworzenie skryptu zmiany bazy danych.

3. Użyj Migracje Code First, aby zaktualizować schemat bazy danych.

Na potrzeby tego samouczka Użyj Migracje Code First.

`SeedData` Zaktualizuj klasę, aby zapewnić wartość nowej kolumny. Poniżej przedstawiono przykładową zmianę, ale trzeba wprowadzić tę zmianę dla każdego `new Movie` bloku.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

Zobacz [ukończony plik SeedData.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).

Skompiluj rozwiązanie.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a>Dodawanie migracji dla pola oceny

W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet > konsola Menedżera pakietów**.
W konsoli zarządzania Pakietami wprowadź następujące polecenia:

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` Polecenie informuje platformę, aby:

* Porównaj model ze schematem `Movie`bazydanych. `Movie`
* Utwórz kod, aby zmigrować schemat bazy danych do nowego modelu.

Nazwa "Rating" jest arbitralna i jest używana do nazwy pliku migracji. Warto użyć zrozumiałej nazwy dla pliku migracji.

`Update-Database` Polecenie informuje platformę, aby zastosować zmiany schematu do bazy danych programu.

<a name="ssox"></a>

W przypadku usunięcia wszystkich rekordów w bazie danych inicjator będzie wypełniać bazę danych i zawierać `Rating` pole. Można to zrobić za pomocą linków usuwania w przeglądarce lub z [programu SQL Server Eksplorator obiektów](xref:tutorials/razor-pages/sql#ssox) (SSOX).

Innym rozwiązaniem jest usunięcie bazy danych i użycie migracji w celu ponownego utworzenia bazy danych. Aby usunąć bazę danych w programie SSOX:

* Wybierz bazę danych w SSOX.
* Kliknij prawym przyciskiem myszy bazę danych, a następnie wybierz pozycję *Usuń*.
* Zaznacz pole wyboru **Zamknij istniejące połączenia**.
* Kliknij przycisk **OK**.
* W obszarze [PMC](xref:tutorials/razor-pages/new-field#pmc)zaktualizuj bazę danych:

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

### <a name="drop-and-re-create-the-database"></a>Porzuć i ponownie utwórz bazę danych

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

Usuń bazę danych i użyj migracji, aby ponownie utworzyć bazę danych. Aby usunąć bazę danych, usuń plik bazy danych (*MvcMovie. DB*). Następnie uruchom `ef database update` polecenie:

```dotnetcli
dotnet ef database update
```

---

Uruchom aplikację i sprawdź, czy można tworzyć/edytować/wyświetlać filmy z `Rating` polem. Jeśli baza danych nie jest zainicjowana, ustaw punkt przerwania `SeedData.Initialize` w metodzie.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wersja tego samouczka usługi YouTube](https://youtu.be/3i7uMxiGGR8)

> [!div class="step-by-step"]
> [Ubiegł Dodawanie wyszukiwania](xref:tutorials/razor-pages/search)
> dalej[: Dodawanie walidacji](xref:tutorials/razor-pages/validation)

::: moniker-end
