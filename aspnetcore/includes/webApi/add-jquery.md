## <a name="call-the-web-api-with-jquery"></a>Wywołanie interfejsu API sieci Web z jQuery

W tej sekcji strony HTML jest dodać używającą jQuery do wywołania interfejsu API sieci Web. jQuery inicjuje żądanie i aktualizuje strony szczegółów z odpowiedzi interfejsu API.

Konfigurowanie projektu do obsługi plików statycznych i umożliwia domyślne mapowanie plików. Jest to osiągane przez wywoływanie [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) metody rozszerzenia w *Startup.Configure*. Aby uzyskać więcej informacji, zobacz [Praca z plikami statycznych w ASP.NET Core](xref:fundamentals/static-files).

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup2.cs?name=snippet_Configure&highlight=3-4)]

Dodaj plik HTML o nazwie *index.html*, w projekcie *wwwroot* katalogu. Zastąp jego zawartość następujący kod:

[!code-html[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/index.html)]

Dodaj plik JavaScript o nazwie *site.js*, w projekcie *wwwroot* katalogu. Zastąp jego zawartość następującym kodem:

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

Może być konieczna zmiana ustawień uruchamiania projektu platformy ASP.NET Core testów strony HTML lokalnie. Otwórz *launchSettings.json* w *właściwości* katalogu projektu. Usuń `launchUrl` właściwości, aby wymusić na aplikacji, aby otworzyć *index.html*&mdash;pliku projektu domyślnego.

Istnieje kilka sposobów, aby uzyskać jQuery. W poprzednim fragment biblioteki są ładowane z usługi CDN. Ten przykład jest pełny przykład CRUD wywołanie interfejsu API z jQuery. Istnieją dodatkowe funkcje, w tym przykładzie, aby bardziej rozbudowane środowisko. Poniżej znajdują się objaśnienia wokół wywołania interfejsu API.

### <a name="get-a-list-of-to-do-items"></a>Pobierz listę elementów do wykonania

Aby uzyskać listę elementów do wykonania, Wyślij żądanie HTTP GET do */api/todo*.

JQuery [ajax](https://api.jquery.com/jquery.ajax/) funkcja wysyła żądanie AJAX do interfejsu API, która zwraca wartość reprezentującą obiektu ani tablicy JSON. Ta funkcja może obsłużyć wszystkich formularzy interakcji HTTP, wysyłanie żądania HTTP do określonego `url`. `GET` jest używany jako `type`. `success` Jest wywoływana funkcja wywołania zwrotnego, jeśli żądanie zakończy się pomyślnie. Podczas wywołania zwrotnego modelu DOM jest aktualizowane przy użyciu informacji zadań do wykonania.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Dodaj element do wykonania

Aby dodać element do wykonania, Wyślij żądanie HTTP POST do */api/zadania/*. Treść żądania powinien zawierać obiektu zadania do wykonania. [Ajax](https://api.jquery.com/jquery.ajax/) używa funkcji `POST` do wywołania interfejsu API. Aby uzyskać `POST` i `PUT` żądania, treści żądania reprezentuje dane wysyłane do interfejsu API. Interfejs API jest oczekiwano treści żądania JSON. `accepts` i `contentType` opcje są ustawione na `application/json` do klasyfikowania odebranych i wysłanych odpowiednio typ nośnika. Dane są konwertowane na obiekt JSON przy użyciu [ `JSON.stringify` ](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Gdy interfejs API zwraca kod stanu pomyślne, `getData` funkcja wywoływana w celu aktualizacji tabeli HTML.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Aktualizuj element do wykonania

Aktualizowanie zadanie do wykonania jest bardzo podobne do dodawania, ponieważ oba te rozwiązania korzystają w treści żądania. W takim przypadku jest tylko rzeczywistych różnica między nimi `url` zmiany, aby dodać unikatowy identyfikator elementu i `type` jest `PUT`.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Usuń element do wykonania

Usunięcie elementu zadań do wykonania odbywa się przez ustawienie `type` w wywołaniu AJAX `DELETE` i wybrana elementu Unikatowy identyfikator w adresie URL.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]
