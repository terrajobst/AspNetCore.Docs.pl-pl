
Firma Microsoft obejmuje [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) w następnym samouczku. [Wyświetlić](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) Określa atrybut, co ma być wyświetlany dla nazwy pola (w tym przypadku "Data wydania" zamiast "ReleaseDate"). [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) atrybut określa typ danych (Data), co nie jest wyświetlane w polu informacje o godzinie.

`[Column(TypeName = "decimal(18, 2)")]` Adnotacji danych elementu jest wymagany, aby poprawnie mapować Entity Framework Core `Price` walutę w bazie danych. Aby uzyskać więcej informacji, zobacz [typy danych](/ef/core/modeling/relational/data-types).

Przejdź do `Movies` kontrolera i umieść wskaźnik myszy nad **Edytuj** łącze, aby wyświetlić docelowy adres URL.

![Okno przeglądarki z myszą łącza edycji i łącze adres Url http://localhost:1234/Movies/Edit/5 jest wyświetlany](~/tutorials/first-mvc-app/controller-methods-views/_static/edit7.png)

**Edytuj**, **szczegóły**, i **usunąć** łącza są generowane przez pomocnika Tag kotwicy MVC Core w *Views/Movies/Index.cshtml* pliku.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1-3&range=46-50)]

[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor. W powyższym kodzie `AnchorTagHelper` dynamicznie generuje kod HTML `href` wartość atrybutu z identyfikator metody i tras akcji kontrolera. Możesz użyć **Wyświetl źródło** z ulubionych przeglądarki lub użyj narzędzia dla deweloperów do sprawdzenia wygenerowanego kodu znaczników. Poniżej przedstawiono fragment wygenerowanego kodu HTML:

```html
 <td>
    <a href="/Movies/Edit/4"> Edit </a> |
    <a href="/Movies/Details/4"> Details </a> |
    <a href="/Movies/Delete/4"> Delete </a>
</td>
```

Odwołaj format [routingu](xref:mvc/controllers/routing) w *Startup.cs* pliku:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

Tłumaczy platformy ASP.NET Core `http://localhost:1234/Movies/Edit/4` na żądanie, aby `Edit` metody akcji `Movies` kontrolera z parametrem `Id` 4. (Metod kontrolera są również znane jako metody akcji).

[Pomocników tagów](xref:mvc/views/tag-helpers/intro) są jednym z najpopularniejszych nowe funkcje programu ASP.NET Core. Zobacz [dodatkowe zasoby](#additional-resources) Aby uzyskać więcej informacji.

Otwórz `Movies` kontrolera i sprawdź, czy dwa `Edit` metody akcji. Poniższy kod przedstawia `HTTP GET Edit` metodę, która pobiera filmu i wypełnienie formularza edycji generowane przez *Edit.cshtml* pliku Razor.

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit1)]

Poniższy kod przedstawia `HTTP POST Edit` metodę, która przetwarza wartości oczekujących na opublikowanie film:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]

Poniższy kod przedstawia `HTTP POST Edit` metodę, która przetwarza wartości oczekujących na opublikowanie film:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]
::: moniker-end

`[Bind]` Atrybut jest jednym ze sposobów ochrony przed [zbyt księgowej](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost). Powinna zawierać tylko właściwości w `[Bind]` atrybut, który chcesz zmienić. Zobacz [chronić kontroler z nadmiernego publikowanie](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application) Aby uzyskać więcej informacji. [ViewModels](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/) zapewnić alternatywne podejście, aby uniknąć nadmiernego publikowanie.

Zwróć uwagę, drugi `Edit` metody akcji jest poprzedzony `[HttpPost]` atrybutu.

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit2&highlight=1)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2&highlight=4)]
::: moniker-end

`HttpPost` Atrybut określa, że to `Edit` można wywołać metody *tylko* dla `POST` żądań. Można zastosować `[HttpGet]` pierwszy dla atrybutu Edytuj metodę, ale nie jest to niezbędne ponieważ `[HttpGet]` jest ustawieniem domyślnym.

`ValidateAntiForgeryToken` Atrybutów jest używane do [zapobiegania fałszowaniu żądania](xref:security/anti-request-forgery) i wraz z tokenu zabezpieczającego przed sfałszowaniem wygenerowany w pliku widok edycji (*Views/Movies/Edit.cshtml*). Edytuj plik widoku generuje token zabezpieczający przed sfałszowaniem z [pomocnika Tag formularza](xref:mvc/views/working-with-forms).

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/Edit.cshtml?range=9)]

[Pomocnika Tag formularza](xref:mvc/views/working-with-forms) generuje token zabezpieczający przed sfałszowaniem ukryte, który musi być zgodna `[ValidateAntiForgeryToken]` wygenerowany token zabezpieczający przed sfałszowaniem w `Edit` filmy kontrolera. Aby uzyskać więcej informacji, zobacz [żądania przed sfałszowaniem](xref:security/anti-request-forgery).

`HttpGet Edit` Metoda przyjmuje filmu `ID` parametru wyszukuje filmu przy użyciu programu Entity Framework `SingleOrDefaultAsync` metody i zwraca wybrany film do widoku edycji. Jeśli nie można odnaleźć film, `NotFound` (HTTP 404) jest zwracany.

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit1)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]
::: moniker-end

Podczas tworzenia widoku edycji systemu szkieletów zbadane `Movie` klasy i kodu do renderowania `<label>` i `<input>` elementy dla każdej właściwości klasy. W poniższym przykładzie pokazano widok edycji został wygenerowany przez system szkieletów Visual Studio:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/EditCopy.cshtml?highlight=1)]

Zwróć uwagę, jak szablon widoku ma `@model MvcMovie.Models.Movie` instrukcji w górnej części pliku. `@model MvcMovie.Models.Movie` Określa, czy widok oczekuje modelu widoku szablonu typu `Movie`.

Kod z utworzonym szkieletem korzysta kilka metod pomocnika tagów usprawnić kod znaczników HTML. [Pomocnika tagów etykiety](xref:mvc/views/working-with-forms) Wyświetla nazwę pola ("Title", "ReleaseDate", "Rodzaju" lub "Price"). [Pomocnika Tag danych wejściowych](xref:mvc/views/working-with-forms) renderowania kodu HTML `<input>` elementu. [Pomocnika tagów weryfikacji](xref:mvc/views/working-with-forms) wyświetla komunikaty weryfikacji skojarzony z tą właściwością.

Uruchom aplikację i przejdź do `/Movies` adresu URL. Kliknij przycisk **Edytuj** łącza. W przeglądarce Wyświetl źródło strony. Wygenerowany kod HTML pod kątem `<form>` elementu są wyświetlane poniżej.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/edit_view_source.html?highlight=1,6,10,17,24,28)]

`<input>` Elementy znajdują się w `HTML <form>` element których `action` ustawiono atrybut do wysłania do `/Movies/Edit/id` adresu URL. Dane formularza zostaną opublikowane na serwerze po `Save` kliknięciu przycisku. Ostatni wiersz przed tagiem zamykającym `</form>` element zawiera ukrytego [XSRF](xref:security/anti-request-forgery) token generowane przez [pomocnika Tag formularza](xref:mvc/views/working-with-forms).

## <a name="processing-the-post-request"></a>Przetwarzanie żądania POST

Poniżej przedstawiono listę `[HttpPost]` wersji `Edit` metody akcji.

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Controllers/MC1.cs?name=snippet_edit2)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]
::: moniker-end

`[ValidateAntiForgeryToken]` Weryfikuje ukrytego atrybutu [XSRF](xref:security/anti-request-forgery) token wygenerowana przez generator token zabezpieczający przed sfałszowaniem w [pomocnika Tag formularza](xref:mvc/views/working-with-forms)

[Modelu powiązania](xref:mvc/models/model-binding) systemu przyjmuje wartości przesłanego formularza i tworzy `Movie` obiekt, który jest przekazywany jako `movie` parametru. `ModelState.IsValid` Metoda sprawdza, czy dane dostarczone w formie może służyć do modyfikowania (edycji lub aktualizacji) `Movie` obiektu. Jeśli dane są prawidłowe są zapisywane. Dane zaktualizowane movie (edytowanych) są zapisywane w bazie danych przez wywołanie metody `SaveChangesAsync` metody kontekst bazy danych. Po zapisaniu danych, kod przekierowuje użytkownika do `Index` metody akcji `MoviesController` klasy, która wyświetla kolekcję film, w tym tylko zmiany.

Przed opublikowania formularza do serwera, weryfikacji po stronie klienta sprawdza reguł sprawdzania poprawności w polach. Jeśli wystąpią jakieś błędy sprawdzania poprawności, zostanie wyświetlony komunikat o błędzie i formularz nie jest opublikowane. Jeśli JavaScript jest wyłączona, nie będziesz mieć weryfikacji po stronie klienta, ale serwer wykryje opublikowanych wartości, które nie są prawidłowe, a wartości formularza zostanie wyświetlony ponownie, z komunikatów o błędach. Później w samouczku omówione [sprawdzania poprawności modelu](xref:mvc/models/validation) bardziej szczegółowo. [Pomocnika tagów weryfikacji](xref:mvc/views/working-with-forms) w *Views/Movies/Edit.cshtml* widok szablonu zajmuje się wyświetlanie odpowiednie komunikaty o błędach.

![Edytowanie widoku: wyjątek niepoprawną wartość ceny ABC określają, czy pole Cena musi być liczbą. Wyjątek niepoprawną wartość Data wydania stanów xyz należy wprowadzić prawidłową datą.](~/tutorials/first-mvc-app/controller-methods-views/_static/val.png)

Wszystkie `HttpGet` metod w kontrolerze filmu wykonaj podobnego wzorca. Otrzymują obiektu movie (lub listę obiektów, w przypadku `Index`) oraz przekazania obiektu (model) do widoku. `Create` Metoda przekazuje obiekt pusty film, aby `Create` widoku. Wszystkie metody, które tworzenia, edytowania, usuwania lub modyfikację danych należy więc w `[HttpPost]` przeciążenia metody. Modyfikowanie danych w `HTTP GET` metody stanowi zagrożenie bezpieczeństwa. Modyfikowanie danych w `HTTP GET` metody również narusza HTTP najlepszych rozwiązań i architektury [REST](http://rest.elkstein.org/) wzorca, który określa, że żądania GET nie należy zmienić stan aplikacji. Innymi słowy wykonanie operacji GET powinny być bezpieczne operację, która nie ma skutków ubocznych i nie modyfikuje danych trwałych.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Globalizacja i lokalizacja](xref:fundamentals/localization)
* [Wprowadzenie do pomocników tagów](xref:mvc/views/tag-helpers/intro)
* [Autor pomocników tagów](xref:mvc/views/tag-helpers/authoring)
* [Ochrona przed fałszerstwem żądań](xref:security/anti-request-forgery)
* Ochrona kontroler z [zbyt księgowej](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application)
* [ViewModels](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/)
* [Pomocnik tagu formularza](xref:mvc/views/working-with-forms)
* [Pomocnik tagu wejściowego](xref:mvc/views/working-with-forms)
* [Pomocnik tagu etykiety](xref:mvc/views/working-with-forms)
* [Pomocnik tagu wyboru](xref:mvc/views/working-with-forms)
* [Sprawdzanie poprawności pomocnika tagów](xref:mvc/views/working-with-forms)
