---
title: Dodaj nowe pole do aplikacji platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak używać migracje Code First Framework jednostki Dodaj nowe pole do modelu i przeprowadzić migrację tej zmiany do bazy danych.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: a8299871671979264383fe7997e56c6708b2e741
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729759"
---
# <a name="add-a-new-field-to-an-aspnet-core-app"></a>Dodaj nowe pole do aplikacji platformy ASP.NET Core

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tej sekcji użyjesz [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) migracje Code First, aby dodać nowe pole do modelu i migracji, które zmiany w bazie danych.

Gdy używasz EF Code First automatycznie utworzyć bazę danych, Code First dodaje tabeli do bazy danych, aby sprawdzić, czy schemat bazy danych jest zsynchronizowana z klasy modelu, który został wygenerowany. Jeśli nie są zsynchronizowane, EF zgłasza wyjątek. Dzięki temu można łatwiej znaleźć problemy niespójne bazy danych/kod.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Dodawanie właściwości klasyfikacji do modelu film

Otwórz *Models/Movie.cs* plik i dodać `Rating` właściwości:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]
::: moniker-end

Tworzenie aplikacji (Ctrl + Shift + B).

Ponieważ zostały dodane nowe pole do `Movie` klasy, należy również zaktualizować powiązania białą listę dzięki tej nowej właściwości zostaną uwzględnione. W *MoviesController.cs*, zaktualizuj `[Bind]` atrybutu dla obu `Create` i `Edit` metod akcji, aby uwzględnić `Rating` właściwości:

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

Należy również zaktualizować szablony widok, aby wyświetlić, tworzyć i edytować nowe `Rating` właściwości w widoku przeglądarki.

Edytuj */Views/Movies/Index.cshtml* plik i dodać `Rating` pola:

[!code-HTML[](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

Aktualizacja */Views/Movies/Create.cshtml* z `Rating` pola. Można kopiowania/wklejania poprzednią "formularza grupę" i umożliwić pomoc intelliSense, zaktualizuj pola. IntelliSense współpracuje z [pomocników tagów](xref:mvc/views/tag-helpers/intro). Uwaga: W wersji RTM programu Visual Studio 2017 należy zainstalować [usługi języka Razor](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) dla Razor intelliSense. Ten problem zostanie rozwiązany w następnej wersji.

![Deweloper wpisał litera R dla wartości atrybutu asp — dla w drugi element label w widoku. Menu kontekstowe Intellisense okazało pokazujący dostępne pola, w tym ocenę, która jest automatycznie wyróżnione na liście. Gdy dewelopera kliknie pole lub naciśnie klawisz Enter na klawiaturze, wartość będzie równa klasyfikacji.](new-field/_static/cr.png)

Aplikacja nie będzie działać, dopóki nie możemy zaktualizować bazę danych, aby uwzględnić nowe pole. Jeżeli możesz uruchomić go teraz, uzyskasz następujące `SqlException`:

`SqlException: Invalid column name 'Rating'.`

Ten błąd jest wyświetlane, ponieważ zaktualizowane klasy modelu film jest inny niż schemat tabeli filmu istniejącej bazy danych. (Brak żadnej klasyfikacji kolumny w tabeli bazy danych.)

Istnieje kilka sposobów rozwiązania problemu:

1. Ma automatycznie porzucenia i ponownego utworzenia bazy danych na podstawie nowego schematu klasy modelu Entity Framework. Ta metoda jest bardzo wygodny wczesnym etapie cyklu programowanie podczas wprowadzania active Programowanie w testowej bazy danych; pozwala na szybkie razem rozwijać schematu modelu i bazy danych. Wadą interfejsu, jest jednak, że utracić istniejące dane w bazie danych, więc nie chcesz użyć tej metody w produkcyjnej bazie danych! Automatycznie inicjatora bazy danych z danych testowych za pomocą inicjatora jest często produktywności możliwości opracowywania aplikacji.

2. Jawnie modyfikować schemat z istniejącej bazy danych, tak aby był zgodny z klasy modelu. Zaletą tej metody jest, aby zachować dane. Można to zrobić to ręcznie lub przez tworzenie bazy danych należy zmienić skryptu.

3. Użyj migracje Code First, aby zaktualizować schemat bazy danych.

W tym samouczku użyjemy migracje Code First.

Aktualizacja `SeedData` klasy, dzięki czemu zapewnia wartość dla nowej kolumny. Poniżej przedstawiono przykładowe zmiany, ale należy to zrobić dla każdego `new Movie`.

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

Skompiluj rozwiązanie.

Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet > konsoli Menedżera pakietów**.

  ![PMC menu](adding-model/_static/pmc.png)

W kryterium wprowadź następujące polecenia:

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` Polecenie informuje framework migracji, aby sprawdzić bieżące `Movie` modelu z bieżącym `Movie` schemat bazy danych i utworzyć niezbędne kod, aby przeprowadzić migrację bazy danych do nowego modelu. Nazwa "Klasyfikacja" jest dowolnego i jest używany do nazywania plików migracji. Warto użyć opisową nazwę pliku migracji.

Po usunięciu wszystkich rekordów w bazie danych będzie inicjatora bazy danych i obejmują `Rating` pola. Można to zrobić z łączami Usuń w przeglądarce lub SSOX.

Uruchom aplikację i sprawdzić, można utworzyć/edycji/wyświetlania filmów `Rating` pola. Należy również dodać `Rating` do `Edit`, `Details`, i `Delete` wyświetlania szablonów.

> [!div class="step-by-step"]
> [Poprzednie](search.md)
> [dalej](validation.md)  
