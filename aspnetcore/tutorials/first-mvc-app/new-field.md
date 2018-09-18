---
title: Dodawanie nowego pola do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się, jak użyć migracje Code First Framework jednostki Dodawanie nowego pola do modelu, i przeprowadzić migrację tej zmiany do bazy danych.
ms.author: riande
ms.date: 10/06/2017
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: b63bad99c4a966703634c711e5406d86e5bd140c
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010887"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a>Dodawanie nowego pola do aplikacji ASP.NET Core MVC

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tej sekcji użyjesz [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) migracje Code First dodaje nowe pole do modelu i migracji, które zmiany w bazie danych.

Korzystając z programów EF Code First automatycznie utworzyć bazę danych, rozwiązanie Code First dodaje tabelę do bazy danych, aby śledzić, czy schemat bazy danych jest zsynchronizowany z klasy modelu, który został wygenerowany z. Jeśli nie są zsynchronizowane, EF zgłasza wyjątek. Ułatwia to znajdowanie problemów z niespójne bazy danych/kodu.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Dodawanie właściwości klasyfikacji do modelu Movie

Otwórz *Models/Movie.cs* pliku i Dodaj `Rating` właściwości:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

Utwórz aplikację (Ctrl + Shift + B).

Ponieważ zostały dodane nowe pole do `Movie` klasy, należy również zaktualizować powiązania białą listę dzięki tej nowej właściwości zostaną dołączone. W *MoviesController.cs*, zaktualizuj `[Bind]` atrybutu dla obu `Create` i `Edit` metody akcji, aby uwzględnić `Rating` właściwości:

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

Należy również zaktualizować Przeglądanie szablonów, aby można było wyświetlić, tworzyć i edytować nowe `Rating` właściwości w widoku przeglądarki.

Edytuj */Views/Movies/Index.cshtml* pliku i Dodaj `Rating` pola:

[!code-HTML[](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

Aktualizacja */Views/Movies/Create.cshtml* z `Rating` pola. Można kopiowanie/wklejanie poprzedniego formularza grupy"" i umożliwić pomoc intelliSense, zaktualizuj pola. Technologia IntelliSense działa z [pomocników tagów](xref:mvc/views/tag-helpers/intro). Uwaga: W wersji RTM programu Visual Studio 2017 musisz zainstalować [usługi języka Razor](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) Razor funkcji IntelliSense. Ten problem zostanie rozwiązany w następnej wersji.

![Deweloper wpisał literę R wartość atrybutu asp — dla w elemencie drugiego etykiety widoku. Menu kontekstowe Intellisense okazało, wyświetlanie dostępnych pól, w tym klasyfikacji, który jest automatycznie wyróżniona na liście. Gdy deweloper kliknie pole lub naciśnie klawisz Enter na klawiaturze, wartość zostanie ustawiona na ocenę.](new-field/_static/cr.png)

Aplikacja nie będzie działać, dopóki nie możemy zaktualizować bazy danych, aby uwzględnić nowe pole. Jeśli uruchamiasz go teraz, uzyskasz następujące `SqlException`:

`SqlException: Invalid column name 'Rating'.`

Widzisz ten błąd, ponieważ zaktualizowane klasy modelu Movie różni się od schematu tabeli filmu istniejącej bazy danych. (Brak jest żadnej kolumny ocenę w tabeli bazy danych).

Istnieje kilka sposobów rozwiązania problemu:

1. Ma automatycznie Porzuć i ponownie utworzyć bazę danych na podstawie nowego schematu klasy modelu Entity Framework. To podejście jest bardzo wygodne na wczesnym etapie cyklu tworzenia oprogramowania, gdy robią active rozwoju w bazie danych testu; Umożliwia szybkie razem rozwijania schematu za jego modelu i bazie danych. Wadą jednak jest utraty istniejących danych w bazie danych — dzięki czemu nie chcesz używać tej metody w produkcyjnej bazie danych! Automatycznie zapełnić bazę danych przy użyciu danych testowych za pomocą inicjatora jest często produktywny sposób tworzenia aplikacji.

2. Jawnie zmodyfikować schemat istniejącej bazy danych, aby odpowiadały one klasy modelu. Zaletą tego podejścia jest, aby zachować dane. Można to zrobić to ręcznie lub przez tworzenie bazy danych zmiana skryptu.

3. Aby zaktualizować schemat bazy danych, należy użyć migracje Code First.

W tym samouczku użyjemy migracje Code First.

Aktualizacja `SeedData` klasy tak, aby go oferuje wartości dla nowej kolumny. Poniżej przedstawiono przykładowe zmiany, ale należy to zrobić dla każdego `new Movie`.

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

Skompiluj rozwiązanie.

Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet > Konsola Menedżera pakietów**.

  ![Menu konsoli zarządzania Pakietami](adding-model/_static/pmc.png)

W konsoli zarządzania Pakietami wprowadź następujące polecenia:

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` Polecenie informuje platformę migracji, aby sprawdzić bieżące `Movie` modelu z bieżącymi `Movie` schematu bazy danych i utworzyć niezbędny kod, aby przeprowadzić migrację bazy danych do nowego modelu. Nazwa "Ocena" dowolnej i jest używany do nazywania plików migracji. Warto użyć znaczącą nazwę pliku migracji.

Jeśli usuniesz wszystkie rekordy w bazie danych, inicjowania będzie obsługiwał bazy danych i obejmują `Rating` pola. Można to zrobić, wraz z łączami delete w przeglądarce lub SSOX.

Uruchom aplikację i sprawdź, można tworzenia/edycji/wyświetlania filmów z `Rating` pola. Należy również dodać `Rating` pole `Edit`, `Details`, i `Delete` wyświetlać szablony.

> [!div class="step-by-step"]
> [Poprzednie](search.md)
> [dalej](validation.md)  
