---
title: Dodanie nowego pola
author: rick-anderson
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 046e16b1a9581edb2be9a2315cf7f2677684747b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="adding-a-new-field"></a>Dodanie nowego pola

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tej sekcji użyjesz [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) migracje Code First, aby dodać nowe pole do modelu i migracji, które zmiany w bazie danych.

Gdy używasz EF Code First automatycznie utworzyć bazę danych, Code First dodaje tabeli do bazy danych, aby sprawdzić, czy schemat bazy danych jest zsynchronizowana z klasy modelu, który został wygenerowany. Jeśli nie są zsynchronizowane, EF zgłasza wyjątek. Dzięki temu można łatwiej znaleźć problemy niespójne bazy danych/kod.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Dodawanie właściwości klasyfikacji do modelu film

Otwórz *Models/Movie.cs* plik i dodać `Rating` właściwości:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

Tworzenie aplikacji (Ctrl + Shift + B).

Ponieważ zostały dodane nowe pole do `Movie` klasy, należy również zaktualizować powiązania białą listę dzięki tej nowej właściwości zostaną uwzględnione. W *MoviesController.cs*, zaktualizuj `[Bind]` atrybutu dla obu `Create` i `Edit` metod akcji, aby uwzględnić `Rating` właściwości:

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

Należy również zaktualizować szablony widok, aby wyświetlić, tworzyć i edytować nowe `Rating` właściwości w widoku przeglądarki.

Edytuj */Views/Movies/Index.cshtml* plik i dodać `Rating` pola:

[!code-HTML[Main](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

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

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

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

>[!div class="step-by-step"]
[Poprzednie](search.md)
[dalej](validation.md)  
