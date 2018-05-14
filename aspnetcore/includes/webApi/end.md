## <a name="implement-the-other-crud-operations"></a>Implementowanie inne operacje CRUD

W poniższych sekcjach `Create`, `Update`, i `Delete` metody są dodawane do kontrolera.

### <a name="create"></a>Create

Dodaj następujące `Create` metody:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Poprzedni kod jest metodą HTTP POST, wskazywany przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu. [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) atrybut informuje MVC, aby uzyskać wartość elementu zadań do wykonania z treści żądania HTTP.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Poprzedni kod jest metodą HTTP POST, wskazywany przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu. MVC pobiera wartość elementu zadań do wykonania z treści żądania HTTP.
::: moniker-end

`CreatedAtRoute` Metody:

* Zwraca odpowiedź 201. 201 protokołu HTTP jest standardowe odpowiedzi dla metody POST protokołu HTTP, która tworzy nowy zasób na serwerze.
* Dodaje do odpowiedzi nagłówek lokalizacji. Nagłówek lokalizacji Określa identyfikator URI elementu nowo utworzone zadanie do wykonania. Zobacz [10.2.2 201 utworzony](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Używa "GetTodo" o nazwie trasy w celu utworzenia adresu URL. "GetTodo" o nazwie trasa jest zdefiniowana w `GetById`:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a>Umożliwia wysłanie żądanie utworzenia Postman

* Uruchom aplikację.
* Otwórz Postman.

![Konsola postman](../../tutorials/first-web-api/_static/pmc.png)

* Zaktualizuj numer portu w adresie URL localhost.
* Ustawia metodę HTTP *POST*.
* Kliknij przycisk **treści** kartę.
* Wybierz **raw** przycisk radiowy.
* Ustaw typ *JSON (application/json)*.
* Wprowadź treść żądania z zadanie do wykonania podobne do następujących JSON:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Kliknij przycisk **wysyłania** przycisku.

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> Jeśli odpowiedź nie są wyświetlane po kliknięciu przycisku **wysyłania**, wyłącz **weryfikacji certyfikacji SSL** opcji. To znajduje się w **pliku** > **ustawienia**. Kliknij przycisk **wysyłania** przycisk ponownie po wyłączeniu ustawienia.
::: moniker-end

Kliknij przycisk **nagłówki** karcie **odpowiedzi** okienko i skopiuj **lokalizacji** wartość nagłówka:

![Karta nagłówki konsoli Postman](../../tutorials/first-web-api/_static/pmc2.png)

Nagłówek lokalizacji URI może służyć do dostępu do nowego elementu.

### <a name="update"></a>Aktualizacja

Dodaj następujące `Update` metody:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

`Update` przypomina `Create`, z wyjątkiem używa HTTP PUT. Odpowiedź jest [204 (bez zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Według specyfikacji HTTP żądania PUT wymaga klienta do wysyłania całego zaktualizowaną jednostkę, nie tylko różnice. Aby obsługiwać aktualizacje częściowe, użyj HTTP PATCH.

Użyj Postman, aby zaktualizować element zadania do wykonania nazwa przeprowadzenie "kot":

![Postman Konsola z wyświetlonymi 204 odpowiedzi (bez zawartości)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Usuwanie

Dodaj następujące `Delete` metody:

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

`Delete` Odpowiedź jest [204 (bez zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

Użyj Postman, aby usunąć element zadania do wykonania:

![Postman Konsola z wyświetlonymi 204 odpowiedzi (bez zawartości)](../../tutorials/first-web-api/_static/pmd.png)
