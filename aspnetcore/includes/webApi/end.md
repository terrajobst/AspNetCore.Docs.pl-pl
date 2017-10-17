## <a name="implement-the-other-crud-operations"></a>Implementowanie inne operacje CRUD

Dodamy `Create`, `Update`, i `Delete` metody kontrolera. Odmiany motywu, są tak właśnie będzie I wyświetlić kod i zaznacz główne różnice. Po dodaniu lub zmiana kodu, skompiluj projekt.

### <a name="create"></a>Create

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Jest to metoda HTTP POST, wskazane przez [ `[HttpPost]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu. [ `[FromBody]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) Atrybut informuje MVC, aby uzyskać wartość elementu zadań do wykonania z treści żądania HTTP.

`CreatedAtRoute` Metoda zwraca odpowiedź 201 standardowa odpowiedź metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze. `CreatedAtRoute`również dodaje do odpowiedzi nagłówek lokalizacji. Nagłówek lokalizacji Określa identyfikator URI elementu nowo utworzone zadanie do wykonania. Zobacz [10.2.2 201 utworzony](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

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

Nagłówek lokalizacji URI umożliwia dostęp do utworzonego zasobu. Odwołaj `GetById` metody utworzony `"GetTodo"` o nazwie trasy:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a>Aktualizacja

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update`przypomina `Create`, ale używa HTTP PUT. Odpowiedź jest [204 (bez zawartości)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Według specyfikacji HTTP żądania PUT wymaga klienta do wysyłania całego zaktualizowaną jednostkę, nie tylko różnice. Aby obsługiwać aktualizacje częściowe, użyj HTTP PATCH.

![Postman Konsola z wyświetlonymi 204 odpowiedzi (bez zawartości)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Usuwanie

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

Odpowiedź jest [204 (bez zawartości)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Postman Konsola z wyświetlonymi 204 odpowiedzi (bez zawartości)](../../tutorials/first-web-api/_static/pmd.png)
