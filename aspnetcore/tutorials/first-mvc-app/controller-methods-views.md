---
title: Metody kontrolera i widoki w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak pracować z metody kontrolera, widoków i DataAnnotations w programie ASP.NET Core.
ms.author: riande
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: cd1b0f2ccffaa660f137b8d739933e56018f2702
ms.sourcegitcommit: ec71fd5a988f927ae301813aae5ff764feb3bb6a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54249415"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a>Metody kontrolera i widoki w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Mamy dobry początek aplikacji filmu, ale prezentacji nie jest najlepszym rozwiązaniem, na przykład **ReleaseDate** powinna być dwa słowa.

![Widok indeksu: Data wydania jest jedno słowo (bez spacji) i każdy film Data wydania zawiera godzinę 12: 00](working-with-sql/_static/m55.png)

Otwórz *Models/Movie.cs* pliku i Dodaj wyróżnione wiersze, które przedstawiono poniżej:

[!code-csharp[](start-mvc/sample/MvcMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]

Omówione [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) w następnym samouczku. [Wyświetlić](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) atrybut określa, co będzie wyświetlone nazwy pola (w tym przypadku "Data wydania" zamiast "ReleaseDate"). [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) atrybut określa typ danych (Data), co nie jest wyświetlane informacje o godzinie, w tym polu.

`[Column(TypeName = "decimal(18, 2)")]` Wymagana jest adnotacja danych, dzięki czemu można poprawnie mapowane na platformy Entity Framework Core `Price` walutę w bazie danych. Aby uzyskać więcej informacji, zobacz [typy danych](/ef/core/modeling/relational/data-types).

Przejdź do `Movies` kontrolera i przytrzymasz wskaźnik myszy **Edytuj** link, aby wyświetlić docelowy adres URL.

![Okno przeglądarki z myszy nad łącze edycji i łącze adres Url https://localhost:5001/Movies/Edit/5 jest wyświetlany](~/tutorials/first-mvc-app/controller-methods-views/_static/edit7.png)

**Edytuj**, **szczegóły**, i **Usuń** łącza są generowane przez Pomocnik tagu kotwicy MVC Core, w *Views/Movies/Index.cshtml* pliku.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1-3&range=46-50)]

[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor. W powyższym kodzie `AnchorTagHelper` dynamicznie generuje kod HTML `href` wartość atrybutu z id metody i wyznaczać trasy akcji kontrolera. Możesz użyć **Wyświetl źródło** ze swojej ulubionej przeglądarce lub użyj narzędzia dla deweloperów do sprawdzenia wygenerowany kod znaczników. Poniżej przedstawiono część wygenerowanego kodu HTML:

```html
 <td>
    <a href="/Movies/Edit/4"> Edit </a> |
    <a href="/Movies/Details/4"> Details </a> |
    <a href="/Movies/Delete/4"> Delete </a>
</td>
```

Odwołaj format [routingu](xref:mvc/controllers/routing) w *Startup.cs* pliku:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

Tłumaczy platformy ASP.NET Core `https://localhost:5001/Movies/Edit/4` na żądanie, aby `Edit` metody akcji `Movies` kontroler z parametrem `Id` 4. (Metody kontrolera są również znane jako metody akcji).

[Pomocników tagów](xref:mvc/views/tag-helpers/intro) są jednymi z najbardziej popularnych nowe funkcje w programie ASP.NET Core. Aby uzyskać więcej informacji, zobacz [dodatkowe zasoby](#additional-resources).

Otwórz `Movies` kontrolera i zbadaj dwa `Edit` metody akcji. Poniższy kod przedstawia `HTTP GET Edit` metody, która pobiera film i wypełnia formularz edycji generowane przez *Edit.cshtml* pliku Razor.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit1)]

Poniższy kod przedstawia `HTTP POST Edit` metody, która przetwarza wartości przesłanych film:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]

Poniższy kod przedstawia `HTTP POST Edit` metody, która przetwarza wartości przesłanych film:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

`[Bind]` Atrybut jest jednym ze sposobów, aby zapewnić ochronę przed [polegającymi](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost). Powinien zawierać tylko właściwości w `[Bind]` atrybut, który chcesz zmienić. Aby uzyskać więcej informacji, zobacz [chronić kontroler z nadmiernego księgowania](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application). [Modele widoków](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/) zawierają alternatywne podejście, aby uniknąć nadmiernego ogłaszania.

Zwróć uwagę, drugi `Edit` metody akcji jest poprzedzony `[HttpPost]` atrybutu.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit2&highlight=1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2&highlight=4)]

::: moniker-end

`HttpPost` Atrybut określa, że to `Edit` może być wywołana metoda *tylko* dla `POST` żądań. Można zastosować `[HttpGet]` atrybutu do pierwszego Edytuj metodę, ale nie jest konieczne ponieważ `[HttpGet]` jest ustawieniem domyślnym.

`ValidateAntiForgeryToken` Atrybut jest używany do [zapobiegania fałszowaniu żądania](xref:security/anti-request-forgery) i parę przy użyciu tokenu zabezpieczającego przed sfałszowaniem generowane w pliku widoku edycji (*Views/Movies/Edit.cshtml*). Edytuj plik widoku generuje token zabezpieczający przed sfałszowaniem z [Pomocnik tagu formularza](xref:mvc/views/working-with-forms).

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/Edit.cshtml?range=9)]

[Pomocnik tagu formularza](xref:mvc/views/working-with-forms) generuje ukryte token zabezpieczający przed sfałszowaniem, które muszą być zgodne `[ValidateAntiForgeryToken]` wygenerowany token zabezpieczający przed sfałszowaniem w `Edit` metody kontrolera filmów. Aby uzyskać więcej informacji, zobacz [ochrona przed fałszerstwem żądań](xref:security/anti-request-forgery).

`HttpGet Edit` Metoda przyjmuje filmu `ID` parametru wyszukuje filmu używający narzędzia Entity Framework `SingleOrDefaultAsync` metodę i zwraca wybrany film do widoku edycji. Jeśli nie można odnaleźć filmu, `NotFound` (HTTP 404) jest zwracany.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit1)]

Podczas tworzenia widoku edycji system scaffoldingu zbadane `Movie` klasy i utworzony kod do renderowania `<label>` i `<input>` elementy dla każdej właściwości klasy. Poniższy przykład przedstawia widok edycji, który został wygenerowany przez system scaffoldingu programu Visual Studio:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/EditOriginal.cshtml)]

Zwróć uwagę, jak szablon widoku ma `@model MvcMovie.Models.Movie` instrukcji w górnej części pliku. `@model MvcMovie.Models.Movie` Określa, czy widok oczekuje modelu dla widoku szablonu typu `Movie`.

Utworzony szkielet kodu używa kilka metod pomocniczych znaczników uprościć kod znaczników HTML. [Pomocnik tagu etykiet](xref:mvc/views/working-with-forms) Wyświetla nazwę pola ("Title", "ReleaseDate", "Gatunku" lub "Price"). [Pomocnik tagu dane wejściowe](xref:mvc/views/working-with-forms) renderowanie kodu HTML `<input>` elementu. [Pomocnik tagu weryfikacji](xref:mvc/views/working-with-forms) wyświetla komunikaty weryfikacji skojarzony z tej właściwości.

Uruchom aplikację, a następnie przejdź do `/Movies` adresu URL. Kliknij przycisk **Edytuj** łącza. W przeglądarce Wyświetl źródło strony. Wygenerowany kod HTML dla `<form>` element znajdują się poniżej.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/edit_view_source.html?highlight=1,6,10,17,24,28)]

`<input>` Elementy znajdują się w `HTML <form>` elementu którego `action` ma ustawioną wartość atrybutu Opublikuj `/Movies/Edit/id` adresu URL. Dane formularza zostaną opublikowane na serwerze po `Save` przycisku. Ostatni wiersz przed tagiem zamykającym `</form>` element pokazuje ukryte [XSRF](xref:security/anti-request-forgery) token generowane przez [Pomocnik tagu formularza](xref:mvc/views/working-with-forms).

## <a name="processing-the-post-request"></a>Przetwarzanie żądania POST

Poniższej przedstawiono listę `[HttpPost]` wersję `Edit` metody akcji.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

`[ValidateAntiForgeryToken]` Atrybut weryfikuje ukryte [XSRF](xref:security/anti-request-forgery) token wygenerowany przez generator tokenów zabezpieczających przed sfałszowaniem w [Pomocnik tagu formularza](xref:mvc/views/working-with-forms)

[Wiązanie modelu](xref:mvc/models/model-binding) systemu przyjmuje wartości przesłanego formularza i tworzy `Movie` obiektu, który jest przekazywany jako `movie` parametru. `ModelState.IsValid` Metoda sprawdza, czy dane dostarczone w formie może służyć do modyfikowania (edycji lub aktualizacja) `Movie` obiektu. Jeśli dane są prawidłowe, są zapisywane. Dane zaktualizowane movie (edytowanych) są zapisywane w bazie danych przez wywołanie metody `SaveChangesAsync` metoda kontekst bazy danych. Po zapisaniu danych, kod przekierowuje użytkownika do `Index` metody akcji `MoviesController` klasy, która wyświetla kolekcji film, w tym zmiany wprowadzone przed chwilą.

Zanim opublikowania formularza z serwerem, weryfikacji po stronie klienta sprawdza reguł sprawdzania poprawności w polach. Jeśli występują błędy sprawdzania poprawności, jest wyświetlany komunikat o błędzie i formularza nie jest opublikowane. Obsługa języka JavaScript jest wyłączona, nie będziesz mieć weryfikacji po stronie klienta, ale serwer wykryje opublikowanych wartości, które nie są prawidłowe i wartości formularza zostanie wyświetlony ponownie, z komunikatów o błędach. W dalszej części tego samouczka omówiony [sprawdzania poprawności modelu](xref:mvc/models/validation) bardziej szczegółowo. [Pomocnik tagu weryfikacji](xref:mvc/views/working-with-forms) w *Views/Movies/Edit.cshtml* Wyświetl szablon dba o wyświetlaniu odpowiedniego komunikatu o błędzie.

![Edytuj widok: Wyjątek dla nieprawidłowej wartości cen ABC, informacja o tym, że pola Cena musi być liczbą. Wyjątek dla nieprawidłowej wartości Data wydania xyz stanów należy wprowadzić prawidłową datę.](~/tutorials/first-mvc-app/controller-methods-views/_static/val.png)

Wszystkie `HttpGet` metodami w kontrolerze filmu wykonaj podobny wzorzec. Staną się obiekt filmu (lub listę obiektów, w przypadku `Index`) i przekazać obiekt (model) do widoku. `Create` Metoda przekazuje obiekt pusty filmu w taki sposób, aby `Create` widoku. Wszystkie metody, które tworzenie, edytowanie, usuwanie lub inny sposób modyfikować danych, należy więc w `[HttpPost]` przeciążenia metody. Modyfikowanie danych w `HTTP GET` metoda stanowi zagrożenie bezpieczeństwa. Modyfikowanie danych w `HTTP GET` metoda również narusza HTTP najlepszych rozwiązań i architektury [REST](http://rest.elkstein.org/) wzorca, który określa, że żądania GET nie powinno się zmieniać stan aplikacji. Innymi słowy wykonywanie operacji GET powinna być bezpieczne operacji, która ma żadnych efektów ubocznych i nie modyfikuje utrwalonych danych.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Globalizacja i lokalizacja](xref:fundamentals/localization)
* [Wprowadzenie do pomocnicy tagów](xref:mvc/views/tag-helpers/intro)
* [Tworzenie pomocników tagów](xref:mvc/views/tag-helpers/authoring)
* [Ochrona przed fałszerstwem żądań](xref:security/anti-request-forgery)
* Chroń swoje kontroler z [polegającymi](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application)
* [Modele widoków](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/)
* [Pomocnik tagu formularza](xref:mvc/views/working-with-forms)
* [Pomocnik tagu wejściowego](xref:mvc/views/working-with-forms)
* [Pomocnik tagu etykiety](xref:mvc/views/working-with-forms)
* [Pomocnik tagu wyboru](xref:mvc/views/working-with-forms)
* [Pomocnik tagu sprawdzania poprawności](xref:mvc/views/working-with-forms)

> [!div class="step-by-step"]
> [Poprzednie](working-with-sql.md)
> [dalej](search.md)  
