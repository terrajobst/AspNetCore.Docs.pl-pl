## <a name="implement-the-other-crud-operations"></a>Implementowanie inne operacje CRUD

W poniższych sekcjach `Create`, `Update`, i `Delete` metody są dodawane do kontrolera.

### <a name="create"></a>Create

Dodaj następujące `Create` metody.

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Poprzedni kod jest metodą HTTP POST, wskazane przez [ `[HttpPost]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu. [ `[FromBody]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) Atrybut informuje MVC, aby uzyskać wartość elementu zadań do wykonania z treści żądania HTTP.

`CreatedAtRoute` Metody:

* Zwraca odpowiedź 201. 201 protokołu HTTP jest standardowe odpowiedzi dla metody POST protokołu HTTP, która tworzy nowy zasób na serwerze.
* Dodaje do odpowiedzi nagłówek lokalizacji. Nagłówek lokalizacji Określa identyfikator URI elementu nowo utworzone zadanie do wykonania. Zobacz [10.2.2 201 utworzony](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Używa "GetTodo" o nazwie trasy w celu utworzenia adresu URL. "GetTodo" o nazwie trasa jest zdefiniowana w `GetById`:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="use-postman-to-send-a-create-request"></a>Umożliwia wysłanie żądanie utworzenia Postman

![Konsola postman](../../tutorials/first-web-api/_static/pmc.png)

* Ustawia metodę HTTP`POST`
* Wybierz **treści** przycisku radiowego
* Wybierz **raw** przycisku radiowego
* Ustaw typ do ciągu JSON
* W edytorze klucz wartość wprowadzać pozycji zadania, takie jak

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* Wybierz **wysyłania**
* Wybierz kartę nagłówków w dolnym okienku i skopiuj **lokalizacji** nagłówka:

![Karta nagłówki konsoli Postman](../../tutorials/first-web-api/_static/pmget.png)

Nagłówek lokalizacji URI może służyć do dostępu do nowego elementu.

### <a name="update"></a>Aktualizacja

Dodaj następujące `Update` metody:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update`przypomina `Create`, ale używa HTTP PUT. Odpowiedź jest [204 (bez zawartości)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Według specyfikacji HTTP żądania PUT wymaga klienta do wysyłania całego zaktualizowaną jednostkę, nie tylko różnice. Aby obsługiwać aktualizacje częściowe, użyj HTTP PATCH.

![Postman Konsola z wyświetlonymi 204 odpowiedzi (bez zawartości)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Usuwanie

Dodaj następujące `Delete` metody:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

`Delete` Odpowiedź jest [204 (bez zawartości)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

Test `Delete`: 

![Postman Konsola z wyświetlonymi 204 odpowiedzi (bez zawartości)](../../tutorials/first-web-api/_static/pmd.png)
