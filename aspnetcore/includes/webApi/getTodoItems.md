::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Poprzedni kod definiuje klasę kontrolera interfejsu API bez metody. W kolejnych sekcjach metody są dodawane do zaimplementowania interfejsu API.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Poprzedni kod definiuje klasę kontrolera interfejsu API bez metody. W kolejnych sekcjach metody są dodawane do zaimplementowania interfejsu API. Klasa jest oznaczony za pomocą `[ApiController]` atrybutu, aby włączyć funkcje wygodne. Informacji o funkcjach, włączone przez atrybut, zobacz [adnotacji klasy z ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).
::: moniker-end

Używa kontrolera konstruktora [iniekcji zależności](xref:fundamentals/dependency-injection) iniekcję kontekst bazy danych (`TodoContext`) z kontrolerem. Kontekst bazy danych jest używany w każdym [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metod w kontrolerze. Konstruktor dodaje element do bazy danych w pamięci, jeśli jeszcze nie istnieje.

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

Oto przykładowe odpowiedzi HTTP dla `GetAll` metody:

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

W dalszej części samouczka I opisano sposób można wyświetlić odpowiedzi HTTP z [Postman](https://www.getpostman.com/) lub [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).

### <a name="routing-and-url-paths"></a>Ścieżki routingu i adres URL

`[HttpGet]` Atrybut oznacza metodę, która odpowiada na żądania HTTP GET. Ścieżka adresu URL dla każdej metody jest tworzony w następujący sposób:

* Wykonaj ciąg szablonu w kontrolera `Route` atrybutu:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end

* Zastąp `[controller]` nazwę kontrolera, który jest nazwa klasy kontrolera minus sufiks "Controller". Ten przykład jest nazwa klasy kontrolera **Todo**kontrolera, a nazwa katalogu głównego to "todo". Platformy ASP.NET Core [routingu](xref:mvc/controllers/routing) nie uwzględnia wielkości liter.
* Jeśli `[HttpGet]` atrybut ma szablon trasy (takich jak `[HttpGet("/products")]`, które dołącza do ścieżki. W tym przykładzie nie używać szablonu. Aby uzyskać więcej informacji, zobacz [atrybutu routingu z atrybutami Http [zlecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

W następujących `GetById` metody `"{id}"` jest zmienną symbolu zastępczego dla Unikatowy identyfikator elementu zadań do wykonania. Gdy `GetById` jest wywołane, przypisuje wartość `"{id}"` w adresie URL do metody `id` parametru.

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

`Name = "GetTodo"` Tworzy nazwanej trasy. Trasy o nazwie:

* Włączanie aplikacji do utworzenia łącza HTTP, używając nazwy trasy.
* Opisano szczegółowo w dalszej części tego samouczka.

### <a name="return-values"></a>Zwracane wartości

`GetAll` Metoda zwraca kolekcję `TodoItem` obiektów. MVC automatycznie serializuje obiekt do [JSON](https://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi. Kod odpowiedzi dla tej metody, 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków. Nieobsługiwane wyjątki są przetłumaczyć 5xx błędy.

::: moniker range="<= aspnetcore-2.0"
Z kolei `GetById` metoda zwraca więcej ogólnych [typu IActionResult](xref:web-api/action-return-types#iactionresult-type), który przedstawia szeroką gamę zwracane typy. `GetById` ma dwa różne typy zwracane:

* Jeśli element nie jest zgodny z Identyfikatorem żądanego, metoda zwraca błąd 404. Zwracanie [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) zwraca odpowiedź HTTP 404.
* W przeciwnym razie metoda zwraca 200 z treści odpowiedzi JSON. Zwracanie [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) powoduje odpowiedź 200 protokołu HTTP.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
Z kolei `GetById` metoda zwraca [ActionResult\<T > typ](xref:web-api/action-return-types#actionresultt-type), który przedstawia szeroką gamę zwracane typy. `GetById` ma dwa różne typy zwracane:

* Jeśli element nie jest zgodny z Identyfikatorem żądanego, metoda zwraca błąd 404. Zwracanie [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) zwraca odpowiedź HTTP 404.
* W przeciwnym razie metoda zwraca 200 z treści odpowiedzi JSON. Zwracanie `item` powoduje odpowiedź 200 protokołu HTTP.
::: moniker-end