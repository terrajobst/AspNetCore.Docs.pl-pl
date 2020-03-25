---
title: Metody i widoki kontrolera w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak korzystać z metod kontrolera, widoków i adnotacji DataAnnotations w ASP.NET Core.
ms.author: riande
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 87b3cb2f4429157123d30274d1f12cd589c1cc99
ms.sourcegitcommit: 99e71ae03319ab386baf2ebde956fc2d511df8b8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/25/2020
ms.locfileid: "80242513"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a>Metody i widoki kontrolera w ASP.NET Core

Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)

Dobrze zaczynasz korzystać z aplikacji filmowej, ale prezentacja nie jest idealna, na przykład **ReleaseDate** powinny być dwa słowa.

![Widok indeksu: Data wydania to jeden wyraz (bez spacji), a każda Data wydania filmu przedstawia godzinę 12 AM](working-with-sql/_static/m55.png)

Otwórz plik *models/Movie. cs* i Dodaj wyróżnione wiersze poniżej:

[!code-csharp[](start-mvc/sample/MvcMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]

W następnym samouczku omówiono wszystkie [Adnotacje](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) . Atrybut [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) określa, co ma być wyświetlane dla nazwy pola (w tym przypadku "Data wydania" zamiast "ReleaseDate"). Atrybut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) określa typ danych (Data), więc informacje o czasie przechowywane w polu nie są wyświetlane.

`[Column(TypeName = "decimal(18, 2)")]` adnotacji danych jest wymagana, aby Entity Framework Core prawidłowo mapować `Price` do waluty w bazie danych. Aby uzyskać więcej informacji, zobacz [typy danych](/ef/core/modeling/relational/data-types).

Przejdź do kontrolera `Movies` i przytrzymaj wskaźnik myszy nad linkiem **edycji** , aby zobaczyć docelowy adres URL.

![Okno przeglądarki z myszą nad linkiem edycji i adresem URL linku https://localhost:5001/Movies/Edit/5 jest pokazywany](~/tutorials/first-mvc-app/controller-methods-views/_static/edit7.png)

Linki **Edytuj**, **szczegóły**i **Usuń** są generowane przez pomocnika podstawowego tagu zakotwiczenia MVC w pliku *views/filmy/index. cshtml* .

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1-3&range=46-50)]

[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor. W powyższym kodzie `AnchorTagHelper` dynamicznie generuje wartość atrybutu HTML `href` z metody akcji kontrolera i identyfikatora trasy. Możesz użyć **widoku źródła** z ulubionej przeglądarki lub użyć narzędzi programistycznych do sprawdzenia wygenerowanego znacznika. Poniżej przedstawiono część wygenerowanego kodu HTML:

```html
 <td>
    <a href="/Movies/Edit/4"> Edit </a> |
    <a href="/Movies/Details/4"> Details </a> |
    <a href="/Movies/Delete/4"> Delete </a>
</td>
```

Odwołaj format zestawu [routingu](xref:mvc/controllers/routing) w pliku *Startup.cs* :

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_1&highlight=5)]

ASP.NET Core tłumaczy `https://localhost:5001/Movies/Edit/4` na żądanie do metody akcji `Edit` kontrolera `Movies` z parametrem `Id` 4. (Metody kontrolera są również nazywane metodami akcji).

[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) to jedna z najpopularniejszych nowych funkcji w programie ASP.NET Core. Aby uzyskać więcej informacji, zobacz [dodatkowe zasoby](#additional-resources).

<a name="get-post"></a>

Otwórz kontroler `Movies` i Przeanalizuj dwie metody akcji `Edit`. Poniższy kod przedstawia metodę `HTTP GET Edit`, która pobiera film i wypełnia formularz edycji wygenerowany przez plik Razor *Edit. cshtml* .

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit1)]

Poniższy kod przedstawia metodę `HTTP POST Edit`, która przetwarza ogłoszone wartości filmu:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]

Poniższy kod przedstawia metodę `HTTP POST Edit`, która przetwarza ogłoszone wartości filmu:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

Atrybut `[Bind]` jest jednym ze sposobów ochrony przed [nadmiernym publikowaniem](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost). Należy uwzględnić tylko właściwości w atrybucie `[Bind]`, który chcesz zmienić. Aby uzyskać więcej informacji, zobacz [Ochrona kontrolera przed nadmiernym publikowaniem](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application). [Modele widoków](https://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/) zapewniają alternatywne podejście do zapobiegania nadmiernemu księgowaniu.

Zwróć uwagę, że druga metoda działania `Edit` jest poprzedzona atrybutem `[HttpPost]`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit2&highlight=1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2&highlight=4)]

::: moniker-end

Atrybut `HttpPost` określa, że ta metoda `Edit` może być wywoływana *tylko* dla żądań `POST`. Do pierwszej metody edycji można zastosować atrybut `[HttpGet]`, ale nie jest to konieczne, ponieważ `[HttpGet]` jest wartością domyślną.

Atrybut `ValidateAntiForgeryToken` służy do [zapobiegania fałszowaniu żądania](xref:security/anti-request-forgery) i jest sparowany z tokenem chroniącym przed fałszerstwem wygenerowanym w pliku widoku edycji (*widoki/filmy/Edit. cshtml*). Plik widoku edycji generuje token chroniący przed fałszerstwem za pomocą [pomocnika tagu formularza](xref:mvc/views/working-with-forms).

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/Edit.cshtml?range=9)]

[Pomocnik tagu formularza](xref:mvc/views/working-with-forms) generuje ukryty token chroniący przed fałszerstwem, który musi być zgodny z `[ValidateAntiForgeryToken]` wygenerowanym tokenem chroniącym przed fałszerstwem w metodzie `Edit` kontrolera filmów. Aby uzyskać więcej informacji, zobacz [zabezpieczenia przed fałszowaniem](xref:security/anti-request-forgery).

Metoda `HttpGet Edit` przyjmuje parametr `ID` filmu, wyszukuje film przy użyciu metody Entity Framework `FindAsync` i zwraca wybrany film do widoku edycji. Jeśli nie można znaleźć filmu, zwracany jest `NotFound` (HTTP 404).

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit1)]

Gdy system szkieletu utworzył widok edycji, zbadamy klasę `Movie` i utworzono kod w celu renderowania `<label>` i `<input>` elementów dla każdej właściwości klasy. W poniższym przykładzie przedstawiono widok edycji, który został wygenerowany przez system szkieletu programu Visual Studio:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/EditOriginal.cshtml)]

Zwróć uwagę, jak szablon widoku zawiera instrukcję `@model MvcMovie.Models.Movie` w górnej części pliku. `@model MvcMovie.Models.Movie` określa, że widok oczekuje modelu dla szablonu widoku, który ma być typu `Movie`.

Kod szkieletowy używa kilku metod pomocnika tagów do uproszczenia znacznika HTML. [Pomocnik tagów etykiet](xref:mvc/views/working-with-forms) wyświetla nazwę pola ("title", "ReleaseDate", "gatunek" lub "price"). [Pomocnik tagu wejściowego](xref:mvc/views/working-with-forms) renderuje element HTML `<input>`. [Pomocnik tagów walidacji](xref:mvc/views/working-with-forms) wyświetla wszystkie komunikaty weryfikacyjne skojarzone z tą właściwością.

Uruchom aplikację i przejdź do adresu URL `/Movies`. Kliknij link **Edytuj** . Sprawdź Źródło strony w przeglądarce. Poniżej przedstawiono wygenerowany kod HTML dla elementu `<form>`.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/edit_view_source.html?highlight=1,6,10,17,24,28)]

Elementy `<input>` znajdują się w `HTML <form>` elemencie, którego atrybut `action` jest ustawiony na wartość post na adres URL `/Movies/Edit/id`. Dane formularza zostaną opublikowane na serwerze po kliknięciu przycisku `Save`. Ostatni wiersz przed zamykającym `</form>` elementu wyświetla ukryty token [XSRF](xref:security/anti-request-forgery) generowany przez [pomocnika tagów formularza](xref:mvc/views/working-with-forms).

## <a name="processing-the-post-request"></a>Przetwarzanie żądania POST

Na poniższej liście przedstawiono `[HttpPost]` wersji metody akcji `Edit`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]

::: moniker-end

Atrybut `[ValidateAntiForgeryToken]` sprawdza poprawność ukrytego tokenu [XSRF](xref:security/anti-request-forgery) wygenerowanego przez generatora tokenów chroniących przed fałszowaniem w [Pomocniku tagów formularza](xref:mvc/views/working-with-forms)

System [powiązań modelu](xref:mvc/models/model-binding) przyjmuje wartości ogłoszonych formularzy i tworzy obiekt `Movie`, który jest przesyłany jako parametr `movie`. Metoda `ModelState.IsValid` sprawdza, czy dane przesyłane w formularzu mogą być używane do modyfikowania (edycji lub aktualizowania) `Movie` obiektu. Jeśli dane są prawidłowe, zostaną zapisane. Zaktualizowane (edytowane) dane filmu są zapisywane w bazie danych przez wywołanie metody `SaveChangesAsync` kontekstu bazy danych. Po zapisaniu danych kod przekierowuje użytkownika do metody akcji `Index` klasy `MoviesController`, co spowoduje wyświetlenie kolekcji filmów, w tym właśnie wprowadzonych zmian.

Przed opublikowaniem formularza na serwerze sprawdzanie poprawności po stronie klienta sprawdza wszystkie reguły sprawdzania poprawności w polach. Jeśli wystąpią jakieś błędy sprawdzania poprawności, zostanie wyświetlony komunikat o błędzie z informacją, że formularz nie zostanie opublikowany. Jeśli język JavaScript jest wyłączony, nie będzie można sprawdzić poprawności po stronie klienta, ale serwer wykryje ogłoszone wartości, które nie są prawidłowe, a wartości formularza zostaną wyświetlone ponownie przy użyciu komunikatów o błędach. W dalszej części samouczka sprawdzimy [Sprawdzanie poprawności modelu](xref:mvc/models/validation) w bardziej szczegółowy sposób. [Pomocnik tagów walidacji](xref:mvc/views/working-with-forms) w szablonie *widoki/filmy/edytowanie. cshtml* ma zadbać o wyświetlenie odpowiednich komunikatów o błędach.

![Widok edycji: wyjątek dla nieprawidłowej wartości ceny ABC wskazuje, że cena pola musi być liczbą. Wyjątek dla nieprawidłowej wartości daty wydania dla Stanów XYZ Wprowadź prawidłową datę.](~/tutorials/first-mvc-app/controller-methods-views/_static/val.png)

Wszystkie metody `HttpGet` w kontrolerze filmu są zgodne z podobnym wzorcem. Uzyskują one obiekt filmu (lub listę obiektów, w przypadku `Index`) i przekaże obiekt (model) do widoku. Metoda `Create` przekazuje pusty obiekt filmu do widoku `Create`. Wszystkie metody, które tworzą, edytują, usuwają lub w inny sposób modyfikują dane, to w `[HttpPost]` Przeciążenie metody. Modyfikowanie danych w metodzie `HTTP GET` stanowi zagrożenie bezpieczeństwa. Modyfikowanie danych w metodzie `HTTP GET` również narusza najlepszych rozwiązań protokołu HTTP i wzorca [rest](http://rest.elkstein.org/) architektury, który określa, że żądania GET nie powinny zmieniać stanu aplikacji. Innymi słowy wykonanie operacji GET powinno być operacją bezpieczną, która nie ma efektów ubocznych i nie modyfikuje utrwalonych danych.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Globalizacja i lokalizacja](xref:fundamentals/localization)
* [Wprowadzenie do pomocy tagów](xref:mvc/views/tag-helpers/intro)
* [Autorzy tagów](xref:mvc/views/tag-helpers/authoring)
* [Ochrona przed fałszerstwem żądań](xref:security/anti-request-forgery)
* Chroń kontroler przed [nadmiernym publikowaniem](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application)
* [Modele widoków](https://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/)
* [Pomocnik tagu formularza](xref:mvc/views/working-with-forms)
* [Pomocnik tagu wejściowego](xref:mvc/views/working-with-forms)
* [Pomocnik tagu etykiety](xref:mvc/views/working-with-forms)
* [Pomocnik tagu wyboru](xref:mvc/views/working-with-forms)
* [Pomocnik tagów walidacji](xref:mvc/views/working-with-forms)

> [!div class="step-by-step"]
> [Poprzednie](working-with-sql.md)
> [dalej](search.md)  
