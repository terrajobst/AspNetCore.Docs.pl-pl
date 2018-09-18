## <a name="implement-the-other-crud-operations"></a>Implementowanie inne operacje CRUD

W poniższych sekcjach `Create`, `Update`, i `Delete` metody są dodawane do kontrolera.

### <a name="create"></a>Create

Dodaj następujący kod `Create` metody:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Powyższy kod jest metodą HTTP POST, wskazane przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu. [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) atrybut informuje MVC, aby uzyskać wartość elementu do wykonania z treści żądania HTTP.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Powyższy kod jest metodą HTTP POST, wskazane przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu. MVC pobiera wartość elementu do wykonania z treści żądania HTTP.

::: moniker-end

`CreatedAtRoute` Metody:

* Zwraca odpowiedź 201. Protokół HTTP 201 jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.
* Dodaje do odpowiedzi nagłówek lokalizacji. Nagłówek Location określa identyfikator URI nowo utworzonego zadania do wykonania. Zobacz [10.2.2 201 utworzone](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Używa "GetTodo" o nazwie tras w celu utworzenia adresu URL. "GetTodo" o nazwie trasa jest zdefiniowana w `GetById`:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a>Wyślij żądanie utworzenia przy użyciu narzędzia Postman

* Uruchom aplikację.
* Otwórz narzędzie Postman.

![Konsola narzędzia postman](../../tutorials/first-web-api/_static/pmc.png)

* Zaktualizuj numer portu w adresie URL localhost.
* Ustawia metodę HTTP *WPIS*.
* Kliknij przycisk **treści** kartę.
* Wybierz **pierwotne** przycisku radiowego.
* Ustaw typ *JSON (application/json)*.
* Wprowadź treść żądania z elementem zadań do wykonania przypominającą następujące dane JSON:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Kliknij przycisk **wysyłania** przycisku.

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> Jeśli odpowiedź nie jest wyświetlany po kliknięciu przycisku **wysyłania**, wyłącz **weryfikacji certyfikacji SSL** opcji. To znajduje się w folderze **pliku** > **ustawienia**. Kliknij przycisk **wysyłania** przycisk ponownie po wyłączeniu ustawienia.

::: moniker-end

Kliknij przycisk **nagłówki** karcie **odpowiedzi** okienka i skopiuj **lokalizacji** wartość nagłówka:

![Karta nagłówki konsoli narzędzia Postman](../../tutorials/first-web-api/_static/pmc2.png)

Nagłówek Location identyfikator URI może służyć do uzyskania dostępu do nowego elementu.

### <a name="update"></a>Aktualizacja

Dodaj następujący kod `Update` metody:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

`Update` jest podobny do `Create`, z wyjątkiem używa HTTP PUT. Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Zgodnie ze specyfikacją protokołu HTTP żądania PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko różnice. Aby obsługiwać aktualizacje częściowe, należy użyć HTTP PATCH.

Zaktualizuj nazwę elementu do wykonania "przeprowadzi cat" przy użyciu narzędzia Postman:

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Usuwanie

Dodaj następujący kod `Delete` metody:

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

`Delete` Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

Użyj narzędzia Postman, aby usunąć element zadania do wykonania:

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](../../tutorials/first-web-api/_static/pmd.png)
