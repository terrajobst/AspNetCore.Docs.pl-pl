[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Poprzedni kod:

* Definiuje klasę pusty kontroler. W kolejnych sekcjach metody są dodawane do zaimplementowania interfejsu API.
* Używa konstruktora [iniekcji zależności](xref:fundamentals/dependency-injection) iniekcję kontekst bazy danych (`TodoContext `) z kontrolerem. Kontekst bazy danych jest używany w każdym [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metod w kontrolerze.
* Konstruktor dodaje element do bazy danych w pamięci, jeśli jeszcze nie istnieje.

## <a name="getting-to-do-items"></a>Pobieranie elementów do wykonania

Aby uzyskać elementów do wykonania, Dodaj następujące metody umożliwiające `TodoController` klasy.

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

Te metody zaimplementować te dwie metody GET.

* `GET /api/todo`
* `GET /api/todo/{id}`

Oto przykład HTTP odpowiedzi dla `GetAll` metody:

```
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
   ```

W dalszej części tego samouczka I opisano sposób można wyświetlić odpowiedzi HTTP z [Postman](https://www.getpostman.com/) lub [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).

### <a name="routing-and-url-paths"></a>Ścieżki routingu i adres URL

`[HttpGet]` Atrybut określa metody HTTP GET. Ścieżka adresu URL dla każdej metody jest tworzony w następujący sposób:

* Wykonaj ciąg szablonu w kontrolera `Route` atrybutu:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* Zastąp `[controller]` nazwę kontrolera, który jest nazwa klasy kontrolera minus sufiks "Controller". Ten przykład jest nazwa klasy kontrolera **Todo**kontrolera, a nazwa katalogu głównego to "todo". Platformy ASP.NET Core [routingu](xref:mvc/controllers/routing) nie jest uwzględniana wielkość liter.
* Jeśli `[HttpGet]` atrybut ma szablon trasy (takich jak `[HttpGet("/products")]`, które dołącza do ścieżki. W tym przykładzie nie używać szablonu. Zobacz [atrybutu routingu z atrybutami Http [zlecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) Aby uzyskać więcej informacji.

W `GetById` metody:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

`"{id}"`to symbol zastępczy zmienna identyfikatora `todo` elementu. Gdy `GetById` jest wywołane, przypisuje wartość "{id}" w adresie URL do metody `id` parametru.

`Name = "GetTodo"`Tworzy nazwanej trasy. Trasy o nazwie:

* Włączanie aplikacji do utworzenia łącza HTTP, używając nazwy trasy.
* Opisano szczegółowo w dalszej części tego samouczka.

### <a name="return-values"></a>Zwracane wartości

`GetAll` Metoda zwraca `IEnumerable`. MVC automatycznie serializuje obiekt do [JSON](http://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi. Kod odpowiedzi dla tej metody, 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków. (Nieobsługiwane wyjątki są przekształcane na błędy 5xx).

Z kolei `GetById` metoda zwraca więcej ogólnych `IActionResult` typu, który przedstawia szeroką gamę zwracanych typów. `GetById`ma dwa różne typy zwracane:

* Jeśli element nie jest zgodny z Identyfikatorem żądanego, metoda zwraca błąd 404. Zwracanie `NotFound` zwraca odpowiedź HTTP 404.

* W przeciwnym razie metoda zwraca 200 z treści odpowiedzi JSON. Zwracanie `ObjectResult` zwraca odpowiedź 200 protokołu HTTP.
