---
title: Dodawanie nowego pola do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się, jak użyć migracje Code First Framework jednostki Dodawanie nowego pola do modelu, i przeprowadzić migrację tej zmiany do bazy danych.
ms.author: riande
ms.custom: mvc
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: c6e7fe13a55a14533949d212bfb149ccd91103e5
ms.sourcegitcommit: e1cc4c1ef6c9e07918a609d5ad7fadcb6abe3e12
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53997243"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a>Dodawanie nowego pola do aplikacji ASP.NET Core MVC

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tej sekcji [Entity Framework](/ef/core/get-started/aspnetcore/new-db) migracje Code First jest używana do:

* Dodawanie nowego pola do modelu.
* Przeprowadź migrację nowe pole do bazy danych.

Gdy EF Code First służy do automatycznego tworzenia bazy danych, Code First:

* Dodaje tabelę w bazie danych do śledzenia schematu bazy danych.
* Sprawdź, czy baza danych jest zsynchronizowany z klasy modelu, który został wygenerowany z. Jeśli nie są zsynchronizowane, EF zgłasza wyjątek. Ułatwia to znajdowanie problemów z niespójne bazy danych/kodu.

## <a name="add-a-rating-property-to-the-movie-model"></a>Dodawanie właściwości klasyfikacji do modelu Movie

Dodaj `Rating` właściwości *Models/Movie.cs*:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

Utwórz aplikację (Ctrl + Shift + B).

Ponieważ zostały dodane nowe pole do `Movie` klasy, należy zaktualizować powiązania białą listę dzięki tej nowej właściwości zostaną dołączone. W *MoviesController.cs*, zaktualizuj `[Bind]` atrybutu dla obu `Create` i `Edit` metody akcji, aby uwzględnić `Rating` właściwości:

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

Aktualizowanie szablonów widoku, aby można było wyświetlić, tworzyć i edytować nowe `Rating` właściwości w widoku przeglądarki.

Edytuj */Views/Movies/Index.cshtml* pliku i Dodaj `Rating` pola:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

Aktualizacja */Views/Movies/Create.cshtml* z `Rating` pola.

<!-- VS -------------------------->
# <a name="visual-studio--visual-studio-for-mactabvisual-studiovisual-studio-mac"></a>[Program Visual Studio / Visual Studio dla komputerów Mac](#tab/visual-studio+visual-studio-mac)

Można kopiowanie/wklejanie poprzedniego formularza grupy"" i umożliwić pomoc intelliSense, zaktualizuj pola. Technologia IntelliSense działa z [pomocników tagów](xref:mvc/views/tag-helpers/intro).

![Deweloper wpisał literę R wartość atrybutu asp — dla w elemencie drugiego etykiety widoku. Menu kontekstowe Intellisense okazało, wyświetlanie dostępnych pól, w tym klasyfikacji, który jest automatycznie wyróżniona na liście. Gdy deweloper kliknie pole lub naciśnie klawisz Enter na klawiaturze, wartość zostanie ustawiona na ocenę.](new-field/_static/cr.png)

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)
<!-- This tab intentionally left blank. -->
---  
<!-- End of VS tabs -->

Aktualizacja `SeedData` klasy tak, aby go oferuje wartości dla nowej kolumny. Poniżej przedstawiono przykładowe zmiany, ale należy to zrobić dla każdego `new Movie`.

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

Aplikacja nie będzie działać, dopóki baza danych została zaktualizowana do nowego pola. Jeśli ma ona Uruchom teraz następujące `SqlException` jest generowany:

`SqlException: Invalid column name 'Rating'.`

Ten błąd występuje, ponieważ zaktualizowane klasy modelu Movie różni się od schematu tabeli filmu istniejącej bazy danych. (Brak nie `Rating` kolumny w tabeli bazy danych.)

Istnieje kilka sposobów rozwiązania problemu:

1. Ma automatycznie Porzuć i ponownie utworzyć bazę danych na podstawie nowego schematu klasy modelu Entity Framework. To podejście jest bardzo wygodne na wczesnym etapie cyklu tworzenia oprogramowania, gdy wykonujesz active rozwoju w bazie danych testu; Umożliwia szybkie razem rozwijania schematu za jego modelu i bazie danych. Wadą jednak jest utraty istniejących danych w bazie danych — dzięki czemu nie chcesz używać tej metody w produkcyjnej bazie danych! Automatycznie zapełnić bazę danych przy użyciu danych testowych za pomocą inicjatora jest często produktywny sposób tworzenia aplikacji. Jest to dobra metoda opracowywania wczesne i, gdy przy użyciu systemu SQLite.

2. Jawnie zmodyfikować schemat istniejącej bazy danych, aby odpowiadały one klasy modelu. Zaletą tego podejścia jest, aby zachować dane. Można to zrobić to ręcznie lub przez tworzenie bazy danych zmiana skryptu.

3. Aby zaktualizować schemat bazy danych, należy użyć migracje Code First.

W tym samouczku jest używana migracje Code First.

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet > Konsola Menedżera pakietów**.

  ![Menu konsoli zarządzania Pakietami](adding-model/_static/pmc.png)

W konsoli zarządzania Pakietami wprowadź następujące polecenia:

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` Polecenie informuje platformę migracji, aby sprawdzić bieżące `Movie` modelu z bieżącymi `Movie` schematu bazy danych i utworzyć niezbędny kod, aby przeprowadzić migrację bazy danych do nowego modelu.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Program Visual Studio Code / Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

Uruchom następujące polecenie:

```cli
dotnet ef migrations add Rating
dotnet ef database update
```

---  
<!-- End of VS tabs -->

Nazwa "Ocena" dowolnej i jest używany do nazywania plików migracji. Warto użyć znaczącą nazwę pliku migracji.

Jeśli zostaną usunięte wszystkie rekordy w bazie danych, metoda inicjowania będzie obsługiwał bazy danych i zawiera `Rating` pola.

Uruchom aplikację i sprawdź, można tworzenia/edycji/wyświetlania filmów z `Rating` pola. Należy dodać `Rating` pole `Edit`, `Details`, i `Delete` wyświetlać szablony.

> [!div class="step-by-step"]
> [Poprzednie](search.md)
> [dalej](validation.md)  
