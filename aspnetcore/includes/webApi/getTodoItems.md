[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Poprzedni kod:

* Definiuje klasę pusty kontroler. W kolejnych sekcjach dodamy metody służące do implementacji interfejsu API.
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
HTTP/1.1 200 OK
   Content-Type: application/json; charset=utf-8
   Server: Microsoft-IIS/10.0
   Date: Thu, 18 Jun 2015 20:51:10 GMT
   Content-Length: 82

   [{"Key":"1", "Name":"Item1","IsComplete":false}]
   ```

W dalszej części tego samouczka I opisano sposób można wyświetlić przy użyciu odpowiedzi HTTP [Postman](https://www.getpostman.com/) lub [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).

### <a name="routing-and-url-paths"></a>Ścieżki routingu i adres URL

`[HttpGet]` Atrybut określa metody HTTP GET. Ścieżka adresu URL dla każdej metody jest tworzony w następujący sposób:

* Wykonaj ciąg szablonu w atrybucie trasy kontrolera:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* Zamień na nazwę kontrolera, który jest nazwa klasy kontrolera minus sufiks "Controller" "[kontrolera]". Ten przykład jest nazwa klasy kontrolera **Todo**kontrolera, a nazwa katalogu głównego to "todo". Platformy ASP.NET Core [routingu](xref:mvc/controllers/routing) nie jest uwzględniana wielkość liter.
* Jeśli `[HttpGet]` atrybut ma szablon trasy (takich jak `[HttpGet("/products")]`, które dołącza do ścieżki. W tym przykładzie nie używać szablonu. Zobacz [atrybutu routingu z atrybutami Http [zlecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) Aby uzyskać więcej informacji.

W `GetById` metody:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

`"{id}"`to symbol zastępczy zmienna identyfikatora `todo` elementu. Gdy `GetById` jest wywołane, przypisuje wartość "{id}" w adresie URL do metody `id` parametru.

`Name = "GetTodo"`tworzy trasę nazwane i umożliwia łącze do tej trasy w odpowiedzi HTTP. Będzie opisano go z przykładem później. Zobacz [routingu do akcji kontrolera](xref:mvc/controllers/routing) Aby uzyskać szczegółowe informacje.

### <a name="return-values"></a>Zwracane wartości

`GetAll` Metoda zwraca `IEnumerable`. MVC automatycznie serializuje obiekt do [JSON](http://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi. Kod odpowiedzi dla tej metody, 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków. (Nieobsługiwane wyjątki są przekształcane na błędy 5xx).

Z kolei `GetById` metoda zwraca więcej ogólnych `IActionResult` typu, który przedstawia szeroką gamę zwracanych typów. `GetById`ma dwa różne typy zwracane:

* Jeśli element nie jest zgodny z Identyfikatorem żądanego, metoda zwraca błąd 404.  Polega to na zwracanie `NotFound`.

* W przeciwnym razie metoda zwraca 200 z treści odpowiedzi JSON. Polega to na zwracanie`ObjectResult`
