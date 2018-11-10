::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Powyższy kod definiuje klasę kontrolera interfejsu API bez metody. W kolejnych sekcjach metody są dodawane do implementacji interfejsu API.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Powyższy kod:

* Definiuje klasę kontrolera interfejsu API bez metody.
* Tworzy nowy Todo elementu, gdy `TodoItems` jest pusty. Nie można usunąć wszystkie elementy zadań do wykonania, ponieważ Konstruktor tworzy nowy Jeśli jeden `TodoItems` jest pusty.

W kolejnych sekcjach metody są dodawane do implementacji interfejsu API. Klasa jest oznaczona za pomocą `[ApiController]` atrybutu, aby włączyć funkcje wygodne. Aby uzyskać informacji na temat funkcji włączone za pomocą atrybutu, zobacz [adnotacja o ApiControllerAttribute](xref:web-api/index#annotation-with-apicontrollerattribute).

::: moniker-end

Używa kontrolera Konstruktor [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) iniekcję kontekst bazy danych (`TodoContext`) do kontrolera. Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze. Konstruktor dodaje element do bazy danych w pamięci, jeśli nie istnieje.

## <a name="get-to-do-items"></a>Pobierz elementy zadań do wykonania

Aby uzyskać elementów do wykonania, Dodaj następujące metody umożliwiające `TodoController` klasy:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

::: moniker-end

Te metody zaimplementować te dwie metody GET.

* `GET /api/todo`
* `GET /api/todo/{id}`

Poniżej przedstawiono przykład odpowiedzi HTTP dla `GetAll` metody:

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

W dalszej części tego samouczka I pokazano, jak mogą być wyświetlane odpowiedzi HTTP z [Postman](https://www.getpostman.com/) lub [curl](https://curl.haxx.se/docs/manpage.html).

### <a name="routing-and-url-paths"></a>Ścieżki routingu i adres URL

`[HttpGet]` Atrybut oznacza metodę, która odpowiada na żądania HTTP GET. Ścieżka adresu URL, dla każdej z metod jest zbudowany w następujący sposób:

* Wykonaj ciąg szablonu na kontrolerze `Route` atrybutu:

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

::: moniker-end

* Zastąp `[controller]` nazwę kontrolera, który zwyczajowo jest nazwa klasy kontrolera minus sufiks "Controller". W tym przykładzie nazwa klasy kontrolera jest **Todo**kontrolera i nazwę katalogu głównego jest "todo". Platforma ASP.NET Core [routingu](xref:mvc/controllers/routing) jest uwzględniana wielkość liter.
* Jeśli `[HttpGet]` atrybut ma szablon trasy (takie jak `[HttpGet("/products")]`, dołączania, do ścieżki. W tym przykładzie nie używa szablonu. Aby uzyskać więcej informacji, zobacz [atrybutu, routing za pomocą atrybutów Http [polecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

W następującym `GetById` metody `"{id}"` jest zmienną symbolu zastępczego dla Unikatowy identyfikator elementu do wykonania. Gdy `GetById` jest wywołana, przypisuje wartość `"{id}"` w adresie URL do metody `id` parametru.

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

`Name = "GetTodo"` tworzy trasę o nazwie. Nazwane trasy:

* Korzystanie z aplikacji utworzyć link HTTP przy użyciu nazwy trasy.
* Zostały omówione w dalszej części tego samouczka.

### <a name="return-values"></a>Zwracane wartości

`GetAll` Metoda zwraca kolekcję `TodoItem` obiektów. MVC automatycznie serializuje obiekt do [JSON](https://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi. Kod odpowiedzi dla tej metody jest równy 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków. Nieobsługiwane wyjątki są tłumaczone na błędy 5xx.

::: moniker range="<= aspnetcore-2.0"

Z kolei `GetById` metoda zwraca bardziej ogólnej [typu IActionResult](xref:web-api/action-return-types#iactionresult-type), która reprezentuje szeroką gamę zwracanych typów. `GetById` ma dwa różne typy zwracane:

* Jeśli żaden element jest zgodny z żądanym Identyfikatorem, metoda zwraca błąd 404. Zwracanie [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) zwraca komunikat odpowiedzi HTTP 404.
* W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON. Zwracanie [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) skutkuje odpowiedź HTTP 200.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Z kolei `GetById` metoda zwraca [ActionResult\<T > typu](xref:web-api/action-return-types#actionresultt-type), która reprezentuje szeroką gamę zwracanych typów. `GetById` ma dwa różne typy zwracane:

* Jeśli żaden element jest zgodny z żądanym Identyfikatorem, metoda zwraca błąd 404. Zwracanie [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) zwraca komunikat odpowiedzi HTTP 404.
* W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON. Zwracanie `item` skutkuje odpowiedź HTTP 200.

::: moniker-end
