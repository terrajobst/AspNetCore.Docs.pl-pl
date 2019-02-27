---
title: 'Samouczek: Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core MVC'
author: rick-anderson
description: Tworzenie internetowego interfejsu API platformy ASP.NET Core MVC
ms.author: riande
ms.custom: mvc
ms.date: 02/4/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 686397cd25248ce7b37e505c7129a3b56d4ada1b
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833764"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core-mvc"></a>Samouczek: Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core MVC

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Mike Wasson](https://github.com/mikewasson)

W tym samouczku pokazano podstawy tworzenia internetowego interfejsu API za pomocą programu ASP.NET Core.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz projekt interfejsu API sieci web.
> * Dodaj klasę modelu.
> * Utwórz kontekst bazy danych.
> * Zarejestruj kontekst bazy danych.
> * Dodawanie kontrolera.
> * Dodaj metody CRUD.
> * Konfigurowanie routingu i ścieżki adresu URL.
> * Określ wartości zwracane.
> * Wywoływanie internetowego interfejsu API za pomocą narzędzia Postman.
> * Wywołanie interfejsu API sieci web przy użyciu jQuery.

Na koniec masz internetowego interfejsu API, która może zarządzać "wykonania", przechowywane w relacyjnej bazie danych.

## <a name="overview"></a>Omówienie

Ten samouczek tworzy następujący interfejs API:

|interfejs API | Opis | Treść żądania | Treść odpowiedzi |
|--- | ---- | ---- | ---- |
|Pobierz /api/todo | Pobierz wszystkie elementy zadań do wykonania | Brak | Tablica elementów do wykonania|
|Pobierz/interfejs API/zadania / {id} | Umieść element według Identyfikatora | Brak | Zadania do wykonania|
|OPUBLIKUJ/api/zadań do wykonania | Dodaj nowy element | Zadania do wykonania | Zadania do wykonania |
|PUT/interfejs API/zadania / {id} | Zaktualizuj istniejący element &nbsp; | Zadania do wykonania | Brak |
|Usuń/interfejs API/zadania / {id} &nbsp; &nbsp; | Usuwanie elementu &nbsp; &nbsp; | Brak | Brak|

Na poniższym diagramie przedstawiono projekt aplikacji.

![Klient jest reprezentowany przez pole po lewej stronie i przesyła żądanie i odbiera odpowiedź od aplikacji rysowania po prawej stronie pola. W polu aplikacji trzy pola reprezentują kontrolera, model i warstwy dostępu do danych. Żądanie jest dostarczany do kontrolera aplikacji, a operacje odczytu/zapisu występują między kontrolerem i warstwy dostępu do danych. Model jest serializowany i zwracany do klienta w odpowiedzi.](first-web-api/_static/architecture.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a>Tworzenie projektu sieci web

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **pliku** menu, wybierz opcję **New** > **projektu**.
* Wybierz **aplikacji sieci Web programu ASP.NET Core** szablonu. Nadaj projektowi nazwę *TodoApi* i kliknij przycisk **OK**.
* W **nowej podstawowej aplikacji sieci Web ASP.NET - TodoApi** okno dialogowe, wybierz wersję platformy ASP.NET Core. Wybierz **API** szablon i kliknij przycisk **OK**. Czy **nie** wybierz **włączyć obsługę platformy Docker**.

![Okno dialogowe programu VS nowego projektu](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Zmień katalog (`cd`) do folderu, który będzie zawierać folderu projektu.
* Uruchom następujące polecenia:

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  Te polecenia Utwórz nowy projekt interfejsu API sieci web i Otwórz nowe wystąpienie programu Visual Studio Code w nowym folderze projektu.

* Gdy okno dialogowe z pytaniem, jeśli chcesz dodać wymagane zasoby do projektu, wybierz **tak**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Wybierz **pliku** > **nowe rozwiązanie**.

  ![Nowe rozwiązanie w systemie macOS](first-web-api-mac/_static/sln.png)

* Wybierz **aplikacji programu .NET Core** > **interfejsu API sieci Web platformy ASP.NET Core** > **dalej**.

  ![okno dialogowe z systemem macOS nowego projektu](first-web-api-mac/_static/1.png)
  
* W **Konfigurowanie nowego programu ASP.NET Core internetowy interfejs API** okno dialogowe, zaakceptuj wartość domyślną **platformę docelową** z **platformy .NET Core 2.2*.

* Wprowadź *TodoApi* dla **Nazwa projektu** , a następnie wybierz **Utwórz**.

  ![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a>Testowanie interfejsu API

Szablon projektu umożliwia utworzenie `values` interfejsu API. Wywołaj `Get` metody z przeglądarki, aby przetestować aplikację.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację. Program Visual Studio otworzy w przeglądarce i przechodzi do `https://localhost:<port>/api/values`, gdzie `<port>` jest numer portu wybranego losowo.

Jeśli pojawi się okno dialogowe z pytaniem, czy należy ufać certyfikat usług IIS Express, wybierz **tak**. W **ostrzeżenie o zabezpieczeniach** okno dialogowe, które pojawia się obok, wybierz **tak**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację. W przeglądarce przejdź do następującego adresu URL: [ https://localhost:5001/api/values ](https://localhost:5001/api/values).

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

Wybierz **Uruchom** > **Rozpocznij z debugowanie** do uruchomienia aplikacji. Program Visual Studio for Mac otworzy w przeglądarce i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest numer portu wybranego losowo. Jest zwracany błąd HTTP 404 (nie znaleziono). Dołącz `/api/values` do adresu URL (adres URL, aby zmienić `https://localhost:<port>/api/values`).

---

Zwracane są następujące dane JSON:

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a>Dodawanie klasy modelu

A *modelu* to zestaw klas, które reprezentują dane, które zarządza aplikacji. Model dla tej aplikacji jest pojedynczym `TodoItem` klasy.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt. Wybierz **Dodaj** > **nowy Folder**. Nazwa folderu *modeli*.

* Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**. Nazwa klasy *TodoItem* i wybierz **Dodaj**.

* Zastąp kod szablonu poniższym kodem:

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Dodaj folder o nazwie *modeli*.

* Dodaj `TodoItem` klasy *modeli* folderu z następującym kodem:

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Kliknij prawym przyciskiem myszy projekt. Wybierz **Dodaj** > **nowy Folder**. Nazwa folderu *modeli*.

  ![Nowy folder](first-web-api-mac/_static/folder.png)

* Kliknij prawym przyciskiem myszy *modeli* folder, a następnie wybierz **Dodaj** > **nowy plik** > **ogólne**  >  **Pusta klasa**.

* Nazwa klasy *TodoItem*, a następnie kliknij przycisk **New**.

* Zastąp kod szablonu poniższym kodem:

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

`Id` Właściwości działa jako unikatowego klucza w relacyjnej bazie danych.

Klasy modeli może przejść w dowolnym miejscu w projekcie, ale *modeli* folder jest używany przez Konwencję.

## <a name="add-a-database-context"></a>Dodawanie kontekstu bazy danych

*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework. Ta klasa jest tworzona przez pochodząca od `Microsoft.EntityFrameworkCore.DbContext` klasy.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**. Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

* Dodaj `TodoContext` klasy *modeli* folderu.

---

* Zastąp kod szablonu poniższym kodem:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a>Zarejestruj kontekst bazy danych

W programie ASP.NET Core, usługami, takimi jak kontekst bazy danych muszą być zarejestrowane w usłudze [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera. Kontener zawiera usługę do kontrolerów.

Aktualizacja *Startup.cs* przy użyciu następujących wyróżniony kod:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

Powyższy kod:

* Usuwa nieużywane `using` deklaracji.
* Dodaje kontener DI kontekst bazy danych.
* Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.

## <a name="add-a-controller"></a>Dodawanie kontrolera

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Kliknij prawym przyciskiem myszy *kontrolerów* folderu.
* Wybierz **Dodaj** > **nowy element**.
* W **Dodaj nowy element** okno dialogowe, wybierz opcję **klasa formantu API** szablonu.
* Nazwa klasy *TodoController*i wybierz **Dodaj**.

  ![Dodaj okno dialogowe nowego elementu za pomocą kontrolera w wyszukiwania sieci web i pole Kontroler interfejsu api wybrane](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

* W *kontrolerów* folderu, Utwórz klasę o nazwie `TodoController`.

---

* Zastąp kod szablonu poniższym kodem:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Powyższy kod:

* Definiuje klasę kontrolera interfejsu API bez metody.
* Rozszerza klasę za pomocą [ `[ApiController]` ](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) atrybutu. Ten atrybut wskazuje, czy kontroler ma odpowiadać na żądania sieci web interfejsu API. Aby uzyskać informacji na temat określonych zachowań, które umożliwia atrybutu, zobacz [adnotacji z atrybutem klasy ApiController](xref:web-api/index#annotation-with-apicontroller-attribute).
* Używa DI iniekcję kontekst bazy danych (`TodoContext`) do kontrolera. Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.
* Dodaje element o nazwie `Item1` do bazy danych, jeśli baza danych jest pusta. Ten kod jest w konstruktorze, aby była uruchamiania za każdym razem, gdy zostanie nowe żądanie HTTP. Jeśli usuniesz wszystkie elementy, Konstruktor tworzy `Item1` ponownie przy kolejnym wywoływana jest metoda interfejsu API. Może to wyglądać tak jak usunięcie nie działało, gdy rzeczywiście działa.

## <a name="add-get-methods"></a>Dodaj metody Get

Aby dostarczać interfejs API, który umożliwia pobranie elementów do wykonania, Dodaj następujące metody umożliwiające `TodoController` klasy:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

Te metody zaimplementować dwa GET punkty końcowe:

* `GET /api/todo`
* `GET /api/todo/{id}`

Testowanie aplikacji, wywołując dwa punkty końcowe w przeglądarce. Na przykład:

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

Następującą odpowiedź HTTP jest tworzony przez wywołanie metody `GetTodoItems`:

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a>Ścieżki routingu i adres URL

[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Atrybut oznacza metodę, która odpowiada na żądania HTTP GET. Ścieżka adresu URL, dla każdej z metod jest zbudowany w następujący sposób:

* Rozpoczyna się od ciągu szablonu na kontrolerze `Route` atrybutu:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* Zastąp `[controller]` nazwę kontrolera, który zwyczajowo jest nazwa klasy kontrolera minus sufiks "Controller". W tym przykładzie nazwa klasy kontrolera jest **Todo**kontrolera, więc nazwa kontrolera jest "todo". Platforma ASP.NET Core [routingu](xref:mvc/controllers/routing) jest uwzględniana wielkość liter.
* Jeśli `[HttpGet]` atrybut ma szablon trasy (na przykład `[HttpGet("products")]`), który Dołącz do ścieżki. W tym przykładzie nie używa szablonu. Aby uzyskać więcej informacji, zobacz [atrybutu, routing za pomocą atrybutów Http [polecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

W następującym `GetTodoItem` metody `"{id}"` jest zmienną symbolu zastępczego dla Unikatowy identyfikator elementu do wykonania. Gdy `GetTodoItem` zostanie wywołana, wartość `"{id}"` w adresie URL jest przekazane do metody w jego`id` parametru.

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a>Zwracane wartości

Zwracany typ `GetTodoItems` i `GetTodoItem` metody jest [ActionResult\<T > typu](xref:web-api/action-return-types#actionresultt-type). Platforma ASP.NET Core automatycznie serializuje obiekt do [JSON](https://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi. Kod odpowiedzi dla tego typu zwracanego jest równy 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków. Nieobsługiwane wyjątki są tłumaczone na błędy 5xx.

`ActionResult` zwracane typy może reprezentować kodów stanu szeroki zakres protokołu HTTP. Na przykład `GetTodoItem` może zwrócić dwie wartości inny stan:

* Jeśli żaden element jest zgodny z żądanym Identyfikatorem, metoda zwraca odpowiedź 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) kod błędu.
* W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON. Zwracanie `item` skutkuje odpowiedź HTTP 200.

## <a name="test-the-gettodoitems-method"></a>Metoda GetTodoItems testu

Ten samouczek używa narzędzia Postman do testowania internetowego interfejsu API.

* Zainstaluj [narzędzia Postman](https://www.getpostman.com/apps)
* Uruchamiają aplikację sieci web.
* Uruchom narzędzie Postman.
* Wyłącz **weryfikacji certyfikatu SSL**
  
  * Z **Plik > Ustawienia** (**ogólne* karty), wyłącz **weryfikacji certyfikatu SSL**.
    > [!WARNING]
    > Ponownie Włącz weryfikację certyfikatu SSL po przetestowaniu kontrolera.

* Utwórz nowe żądanie.
  * Ustawia metodę HTTP **UZYSKAĆ**.
  * Ustaw adres URL żądania `https://localhost:<port>/api/todo`. Na przykład `https://localhost:5001/api/todo`.
* Ustaw **widoku dwa okienka** w narzędziu Postman.
* Wybierz pozycję **Wyślij**.

![Postman przy użyciu żądania Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a>Dodawanie metody Create

Dodaj następujący kod `PostTodoItem` metody:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Powyższy kod jest metodą HTTP POST, wskazane przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu. Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.

`CreatedAtAction` Metody:

* Zwraca kod stanu 201 protokołu HTTP, jeśli to się powiedzie. Protokół HTTP 201 jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.
* Dodaje `Location` nagłówka odpowiedzi. `Location` Nagłówek Określa identyfikator URI nowo utworzonego zadania do wykonania. Aby uzyskać więcej informacji, zobacz [10.2.2 201 utworzono](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Odwołania `GetTodoItem` akcja w celu utworzenia `Location` nagłówka identyfikatora URI. C# `nameof` Słowo kluczowe jest używane w celu uniknięcia kodować Nazwa akcji w `CreatedAtAction` wywołania.

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a>Metoda PostTodoItem testu

* Skompiluj projekt.
* W narzędziu Postman, Ustawia metodę HTTP `POST`.
* Wybierz **treści** kartę.
* Wybierz **pierwotne** przycisku radiowego.
* Ustaw typ **JSON (application/json)**.
* W treści żądania wprowadź JSON element do wykonania:

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* Wybierz pozycję **Wyślij**.

  ![Postman przy użyciu Utwórz żądanie](first-web-api/_static/create.png)

  Jeśli 405 błąd niedozwolona metoda, prawdopodobnie wynik nie Kompilowanie projektu po dodaniu `PostTodoItem` metody.

### <a name="test-the-location-header-uri"></a>Testowanie nagłówek location identyfikator URI

* Wybierz **nagłówki** karcie **odpowiedzi** okienka.
* Kopiuj **lokalizacji** wartość nagłówka:

  ![Karta nagłówki konsoli narzędzia Postman](first-web-api/_static/pmc2.png)

* Ustaw metodę GET.
* Wklej identyfikator URI (na przykład `https://localhost:5001/api/Todo/2`)
* Wybierz pozycję **Wyślij**.

## <a name="add-a-puttodoitem-method"></a>Dodaj metodę PutTodoItem

Dodaj następujący kod `PutTodoItem` metody:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`PutTodoItem` jest podobny do `PostTodoItem`, z wyjątkiem używa HTTP PUT. Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Zgodnie ze specyfikacją protokołu HTTP żądania PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko zmiany. Aby obsługiwać aktualizacje częściowe, należy użyć [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).

Mogę uzyskać wystąpił błąd podczas wywoływania `PutTodoItem`, wywołania `GET` aby upewnić się, Brak elementu w bazie danych.

### <a name="test-the-puttodoitem-method"></a>Metoda PutTodoItem testu

Ta próbka używa bazy danych w pamięci, który musi być inicjowania na każdym razem, gdy aplikacja jest uruchomiona. Musi istnieć element w bazie danych przed wprowadzeniem wywołania PUT. Wywołaj metodę GET, aby upewnić się, że istnieje element w bazie danych przed wprowadzeniem wywołania PUT.

Zaktualizuj element zadania do wykonania, który ma identyfikator = 1 i ustaw jego nazwę na "feed ryb":

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

Na poniższej ilustracji przedstawiono aktualizacji Postman:

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a>Dodaj metodę DeleteTodoItem

Dodaj następujący kod `DeleteTodoItem` metody:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

`DeleteTodoItem` Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

### <a name="test-the-deletetodoitem-method"></a>Metoda DeleteTodoItem testu

Użyj narzędzia Postman, aby usunąć zadanie do wykonania:

* Ustawia metodę `DELETE`.
* Ustaw identyfikator URI obiektu, aby usunąć, na przykład `https://localhost:5001/api/todo/1`
* Wybierz **wysyłania**

Przykładowa aplikacja umożliwia usunięcie wszystkich elementów, ale po usunięciu ostatniego elementu jest tworzony nowy przez konstruktora klasy modelu podczas następnego wywołania interfejsu API.

## <a name="call-the-api-with-jquery"></a>Wywoływanie interfejsu API przy użyciu jQuery

W tej sekcji strony HTML jest dodawany, który używa technologii jQuery do wywołania sieci web interfejsu api. jQuery inicjuje żądanie i aktualizowanie strony ze szczegółami z odpowiedzi interfejsu API.

Konfigurowanie aplikacji w celu [Obsługa plików statycznych](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [włączyć domyślne mapowanie plików](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_):

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

::: moniker range=">= aspnetcore-2.2"
Tworzenie *wwwroot* folder w katalogu projektu.
::: moniker-end

Dodaj plik HTML o nazwie *index.html* do *wwwroot* katalogu. Zastąp jego zawartość następującym kodem:

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

Dodaj plik języka JavaScript o nazwie *site.js* do *wwwroot* katalogu. Zastąp jego zawartość następującym kodem:

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

Zmiana ustawień uruchamiania projektów ASP.NET Core może być konieczne test lokalnie za pomocą strony HTML:

* Otwórz *Properties\launchSettings.json*.
* Usuń `launchUrl` właściwości, aby wymusić na aplikacji, aby otworzyć w *index.html*&mdash;pliku domyślnego projektu.

Istnieje kilka sposobów uzyskania biblioteki jQuery. W poprzednim fragmencie kodu biblioteki jest ładowany z usługi CDN.

Ten przykład wywołuje wszystkie metody CRUD interfejsu API. Poniżej przedstawiono objaśnienia dotyczące wywołań interfejsu API.

### <a name="get-a-list-of-to-do-items"></a>Pobierz listę elementów do wykonania

JQuery [ajax](https://api.jquery.com/jquery.ajax/) funkcji wysyła `GET` żądanie do interfejsu API, która zwraca wartość JSON reprezentująca tablicę elementów do wykonania. `success` Wywołaniu funkcji wywołania zwrotnego, jeśli żądanie zakończy się powodzeniem. Podczas wywołania zwrotnego model DOM jest aktualizowana informacjami zadań do wykonania.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Dodaj element do wykonania

[Ajax](https://api.jquery.com/jquery.ajax/) funkcji wysyła `POST` żądania z elementem zadań do wykonania w treści żądania. `accepts` i `contentType` opcje są ustawione na `application/json` Aby określić typ nośnika odbieranych i wysyłanych. Element do wykonania jest konwertowana na format JSON za pomocą [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Gdy interfejs API zwraca kod stanu powodzenia `getData` wywołaniu funkcji można zaktualizować tabeli HTML.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Zaktualizuj element do wykonania

Aktualizowanie zadanie do wykonania jest podobne do dodawania jednego. `url` Zmiany do Dodaj Unikatowy identyfikator elementu, a `type` jest `PUT`.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Usuń element do wykonania

Trwa usuwanie zadania do wykonania odbywa się przez ustawienie `type` na wywołanie AJAX do `DELETE` i podając unikatowy identyfikator elementu w adresie URL.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

## <a name="additional-resources"></a>Dodatkowe zasoby

[Wyświetlanie lub pobieranie przykładowego kodu w tym samouczku](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples). Zobacz [sposobu pobierania](xref:index#how-to-download-a-sample).

Aby uzyskać więcej informacji, zobacz następujące zasoby:

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz projekt interfejsu api sieci web.
> * Dodaj klasę modelu.
> * Utwórz kontekst bazy danych.
> * Zarejestruj kontekst bazy danych.
> * Dodawanie kontrolera.
> * Dodaj metody CRUD.
> * Konfigurowanie routingu i ścieżki adresu URL.
> * Określ wartości zwracane.
> * Wywoływanie internetowego interfejsu API za pomocą narzędzia Postman.
> * Wywoływanie internetowego interfejsu api przy użyciu jQuery.

Przejdź do następnego samouczka, aby dowiedzieć się, jak można wygenerować stron pomocy interfejsu API:

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
