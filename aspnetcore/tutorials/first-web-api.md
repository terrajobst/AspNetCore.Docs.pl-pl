---
title: 'Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core'
author: rick-anderson
description: Dowiedz się, jak utworzyć internetowy interfejs API za pomocą ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/27/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 2d0eb24641c3d1f795b9e85ce10d42ee96d30846
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187309"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a>Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Mike Wasson](https://github.com/mikewasson)

W tym samouczku pokazano podstawy tworzenia internetowego interfejsu API za pomocą programu ASP.NET Core.

::: moniker range=">= aspnetcore-3.0"

Z tego samouczka dowiesz się, jak wykonywać następujące czynności:

> [!div class="checklist"]
> * Utwórz projekt interfejsu API sieci Web.
> * Dodaj klasę modelu i kontekst bazy danych.
> * Tworzy szkielet kontrolera z metodami CRUD.
> * Skonfiguruj Routing, ścieżki URL i wartości zwracane.
> * Wywoływanie internetowego interfejsu API za pomocą narzędzia Postman.

Na końcu znajduje się internetowy interfejs API, który może zarządzać elementami do wykonania przechowywanymi w bazie danych.

## <a name="overview"></a>Omówienie

Ten samouczek tworzy następujący interfejs API:

|interfejs API | Opis | Treść żądania | Treść odpowiedzi |
|--- | ---- | ---- | ---- |
|Pobierz/api/TodoItems | Pobierz wszystkie elementy zadań do wykonania | Brak | Tablica elementów do wykonania|
|Pobierz/api/TodoItems/{id} | Umieść element według Identyfikatora | Brak | Zadania do wykonania|
|Opublikuj/api/TodoItems | Dodaj nowy element | Zadania do wykonania | Zadania do wykonania |
|Umieść/api/TodoItems/{id} | Zaktualizuj istniejący element &nbsp; | Zadania do wykonania | Brak |
|Usuń/api/TodoItems/{id} &nbsp;&nbsp; | Usuwanie elementu &nbsp; &nbsp; | Brak | Brak|

Na poniższym diagramie przedstawiono projekt aplikacji.

![Klient jest reprezentowany przez pole po lewej stronie. Przesyła żądanie i odbiera odpowiedź z aplikacji, pole rysowane po prawej stronie. W polu aplikacji trzy pola reprezentują kontrolera, model i warstwy dostępu do danych. Żądanie jest dostarczany do kontrolera aplikacji, a operacje odczytu/zapisu występują między kontrolerem i warstwy dostępu do danych. Model jest serializowany i zwracany do klienta w odpowiedzi.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a>Tworzenie projektu sieci web

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z menu **plik** wybierz pozycję **Nowy** > **projekt**.
* Wybierz szablon **aplikacja sieci Web ASP.NET Core** a następnie kliknij przycisk **dalej**.
* Nazwij projekt *TodoApi* i kliknij pozycję **Utwórz**.
* W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 3,0** . Wybierz szablon **interfejsu API** i kliknij przycisk **Utwórz**. **Nie** zaznaczaj opcji **Włącz obsługę platformy Docker**.

![Okno dialogowe programu VS nowego projektu](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Zmień katalog (`cd`) do folderu, który będzie zawierać folder projektu.
* Uruchom następujące polecenia:

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoAPI
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* Gdy zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz dodać wymagane zasoby do projektu, wybierz opcję **tak**.

  Poprzedniego polecenia:

  * Tworzy nowy projekt internetowego interfejsu API i otwiera go w Visual Studio Code.
  * Dodaje pakiety NuGet, które są wymagane w następnej sekcji.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* Wybierz pozycję **plik** > **nowe rozwiązanie**.

  ![Nowe rozwiązanie w systemie macOS](first-web-api-mac/_static/sln.png)

* Wybierz pozycję **interfejs API** > > aplikacji> .NET Core.

  ![okno dialogowe z systemem macOS nowego projektu](first-web-api-mac/_static/1.png)
  
* W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** wybierz pozycję **docelowa platforma** * *.NET Core 3,0*.

* Wprowadź *TodoApi* dla **Nazwa projektu** , a następnie wybierz **Utwórz**.

  ![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

Otwórz Terminal poleceń w folderze projektu i uruchom następujące polecenia:

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a>Testowanie interfejsu API

Szablon projektu umożliwia utworzenie `WeatherForecast` interfejsu API. Wywołaj `Get` metody z przeglądarki, aby przetestować aplikację.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację. Program Visual Studio otworzy w przeglądarce i przechodzi do `https://localhost:<port>/WeatherForecast`, gdzie `<port>` jest numer portu wybranego losowo.

Jeśli pojawi się okno dialogowe z pytaniem, czy należy ufać certyfikat usług IIS Express, wybierz **tak**. W **ostrzeżenie o zabezpieczeniach** okno dialogowe, które pojawia się obok, wybierz **tak**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację. W przeglądarce przejdź do następującego adresu URL: [ https://localhost:5001/WeatherForecast ](https://localhost:5001/WeatherForecast).

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

Wybierz pozycję **Uruchom** > **Rozpocznij debugowanie** , aby uruchomić aplikację. Program Visual Studio for Mac otworzy w przeglądarce i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest numer portu wybranego losowo. Jest zwracany błąd HTTP 404 (nie znaleziono). Dołącz `/WeatherForecast` do adresu URL (adres URL, aby zmienić `https://localhost:<port>/WeatherForecast`).

---

Zwracany jest kod JSON podobny do następującego:

```json
[
    {
        "date": "2019-07-16T19:04:05.7257911-06:00",
        "temperatureC": 52,
        "temperatureF": 125,
        "summary": "Mild"
    },
    {
        "date": "2019-07-17T19:04:05.7258461-06:00",
        "temperatureC": 36,
        "temperatureF": 96,
        "summary": "Warm"
    },
    {
        "date": "2019-07-18T19:04:05.7258467-06:00",
        "temperatureC": 39,
        "temperatureF": 102,
        "summary": "Cool"
    },
    {
        "date": "2019-07-19T19:04:05.7258471-06:00",
        "temperatureC": 10,
        "temperatureF": 49,
        "summary": "Bracing"
    },
    {
        "date": "2019-07-20T19:04:05.7258474-06:00",
        "temperatureC": -1,
        "temperatureF": 31,
        "summary": "Chilly"
    }
]
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

* Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz pozycję **Dodaj** > **nowy plik** > **ogólna** > **pusta Klasa**.

* Nazwa klasy *TodoItem*, a następnie kliknij przycisk **New**.

* Zastąp kod szablonu poniższym kodem:

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

`Id` Właściwości działa jako unikatowego klucza w relacyjnej bazie danych.

Klasy modeli może przejść w dowolnym miejscu w projekcie, ale *modeli* folder jest używany przez Konwencję.

## <a name="add-a-database-context"></a>Dodawanie kontekstu bazy danych

*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework. Ta klasa jest tworzona przez pochodząca od `Microsoft.EntityFrameworkCore.DbContext` klasy.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a>Dodaj Microsoft. EntityFrameworkCore. SqlServer

* W menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet > Zarządzanie pakietami NuGet dla rozwiązania**.
* Zaznacz pole wyboru **Uwzględnij wersję wstępną** .
* Wybierz kartę **Przeglądaj** , a następnie w polu wyszukiwania wprowadź ciąg **Microsoft. EntityFrameworkCore. SqlServer** .
* W lewym okienku wybierz pozycję **Microsoft. EntityFrameworkCore. SqlServer v 3.0.0 — wersja zapoznawcza** .
* Zaznacz pole wyboru **projekt** w prawym okienku, a następnie wybierz pozycję **Zainstaluj**.
* Aby dodać `Microsoft.EntityFrameworkCore.InMemory` pakiet NuGet, użyj powyższych instrukcji.

![Menedżer pakietów NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a>Dodawanie kontekstu bazy danych TodoContext

* Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**. Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

* Dodaj `TodoContext` klasy *modeli* folderu.

---

* Wprowadź następujący kod:

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a>Zarejestruj kontekst bazy danych

W programie ASP.NET Core, usługami, takimi jak kontekst bazy danych muszą być zarejestrowane w usłudze [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera. Kontener zawiera usługę do kontrolerów.

Aktualizacja *Startup.cs* przy użyciu następujących wyróżniony kod:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

Powyższy kod:

* Usuwa nieużywane `using` deklaracji.
* Dodaje kontener DI kontekst bazy danych.
* Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.

## <a name="scaffold-a-controller"></a>Tworzenie szkieletu kontrolera

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Kliknij prawym przyciskiem myszy *kontrolerów* folderu.
* Wybierz pozycję **Dodaj** > **nowy element szkieletowy**.
* Wybierz pozycję **kontroler interfejsu API z akcjami, używając Entity Framework**, a następnie wybierz pozycję **Dodaj**.
* Na stronie **Dodawanie kontrolera interfejsu API z akcjami przy użyciu Entity Framework** dialogowego:

  * Wybierz pozycję **TodoItem (TodoAPI. models)** w **klasie model**.
  * W **klasie kontekstu danych**wybierz pozycję **TodoContext (TodoAPI. models)** .
  * Wybierz pozycję **Dodaj**

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

Uruchom następujące polecenia:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext  -outDir Controllers
```

Poprzedniego polecenia:

* Dodaj pakiety NuGet wymagane do tworzenia szkieletów.
* Instaluje aparat tworzenia szkieletu (`dotnet-aspnet-codegenerator`).
* Szkielety `TodoItemsController`.

---

Wygenerowany kod:

* Definiuje klasę kontrolera interfejsu API bez metody.
* Zdobi klasę z atrybutem [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) . Ten atrybut wskazuje, czy kontroler ma odpowiadać na żądania sieci web interfejsu API. Aby uzyskać informacje o określonych zachowaniach, które włącza atrybut <xref:web-api/index>, zobacz.
* Używa DI iniekcję kontekst bazy danych (`TodoContext`) do kontrolera. Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.

## <a name="examine-the-posttodoitem-create-method"></a>Badanie metody PostTodoItem Create

Zastąp instrukcję return w, `PostTodoItem` aby użyć operatora [nameof](/dotnet/csharp/language-reference/operators/nameof) :

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

Powyższy kod jest metodą HTTP POST, wskazane przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu. Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.

<xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> Metody:

* W razie powodzenia zwraca kod stanu HTTP 201. Protokół HTTP 201 jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.
* Dodaje nagłówek [lokalizacji](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) do odpowiedzi. Nagłówek określa identyfikator URI nowo utworzonego elementu do wykonania. [](https://developer.mozilla.org/docs/Glossary/URI) `Location` Aby uzyskać więcej informacji, zobacz [10.2.2 201 utworzono](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Odwołuje `GetTodoItem` się do akcji `Location` tworzenia identyfikatora URI nagłówka. Słowo C# `nameof` kluczowe jest używane, aby zapobiec twardemu kodowaniu nazwy `CreatedAtAction` akcji w wywołaniu.

### <a name="install-postman"></a>Zainstaluj program Poster

Ten samouczek używa narzędzia Postman do testowania internetowego interfejsu API.

* Zainstaluj [narzędzia Postman](https://www.getpostman.com/downloads/)
* Uruchamiają aplikację sieci web.
* Uruchom narzędzie Postman.
* Wyłącz **weryfikacji certyfikatu SSL**
* Z **Plik > Ustawienia** (**ogólne* karty), wyłącz **weryfikacji certyfikatu SSL**.
    > [!WARNING]
    > Ponownie Włącz weryfikację certyfikatu SSL po przetestowaniu kontrolera.

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a>Test PostTodoItem za pomocą programu Poster

* Utwórz nowe żądanie.
* Ustaw metodę HTTP na `POST`.
* Wybierz **treści** kartę.
* Wybierz **pierwotne** przycisku radiowego.
* Ustaw typ **JSON (application/json)** .
* W treści żądania wprowadź JSON element do wykonania:

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* Wybierz pozycję **Wyślij**.

  ![Postman przy użyciu Utwórz żądanie](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a>Testowanie nagłówek location identyfikator URI

* Wybierz **nagłówki** karcie **odpowiedzi** okienka.
* Kopiuj **lokalizacji** wartość nagłówka:

  ![Karta nagłówki konsoli narzędzia Postman](first-web-api/_static/3/create.png)

* Ustaw metodę GET.
* Wklej identyfikator URI (na przykład `https://localhost:5001/api/TodoItems/1`)
* Wybierz pozycję **Wyślij**.

## <a name="examine-the-get-methods"></a>Badanie metod GET

Te metody zaimplementować dwa GET punkty końcowe:

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

Przetestuj aplikację, wywołując dwa punkty końcowe z przeglądarki lub wpisu. Na przykład:

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

Odpowiedź podobna do poniższego jest generowana przez wywołanie `GetTodoItems`:

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a>Test get przy użyciu programu Poster

* Utwórz nowe żądanie.
* Ustawia metodę HTTP **UZYSKAĆ**.
* Ustaw adres URL żądania `https://localhost:<port>/api/TodoItems`. Na przykład `https://localhost:5001/api/TodoItems`.
* Ustaw **widoku dwa okienka** w narzędziu Postman.
* Wybierz pozycję **Wyślij**.

Ta aplikacja używa bazy danych w pamięci. Jeśli aplikacja zostanie zatrzymana i uruchomiona, poprzednie żądanie GET nie zwróci żadnych danych. Jeśli nie zostaną zwrócone żadne dane, [Opublikuj](#post) dane w aplikacji.

## <a name="routing-and-url-paths"></a>Ścieżki routingu i adres URL

[ `[HttpGet]` ](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) Atrybut oznacza metodę, która odpowiada na żądania HTTP GET. Ścieżka adresu URL, dla każdej z metod jest zbudowany w następujący sposób:

* Rozpoczyna się od ciągu szablonu na kontrolerze `Route` atrybutu:

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* Zastąp `[controller]` nazwę kontrolera, który zwyczajowo jest nazwa klasy kontrolera minus sufiks "Controller". Dla tego przykładu nazwa klasy kontrolera to **TodoItems**Controller, więc nazwa kontrolera to "TodoItems". Platforma ASP.NET Core [routingu](xref:mvc/controllers/routing) jest uwzględniana wielkość liter.
* Jeśli atrybut ma szablon trasy (na `[HttpGet("products")]`przykład), Dodaj go do ścieżki. `[HttpGet]` W tym przykładzie nie używa szablonu. Aby uzyskać więcej informacji, zobacz [atrybutu, routing za pomocą atrybutów Http [polecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

W następującym `GetTodoItem` metody `"{id}"` jest zmienną symbolu zastępczego dla Unikatowy identyfikator elementu do wykonania. Gdy `GetTodoItem` zostanie wywołana, wartość `"{id}"` w adresie URL jest przekazane do metody w jego`id` parametru.

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a>Zwracane wartości

Zwracany typ `GetTodoItems` i `GetTodoItem` metody jest [ActionResult\<T > typu](xref:web-api/action-return-types#actionresultt-type). Platforma ASP.NET Core automatycznie serializuje obiekt do [JSON](https://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi. Kod odpowiedzi dla tego typu zwracanego jest równy 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków. Nieobsługiwane wyjątki są tłumaczone na błędy 5xx.

`ActionResult` zwracane typy może reprezentować kodów stanu szeroki zakres protokołu HTTP. Na przykład `GetTodoItem` może zwrócić dwie wartości inny stan:

* Jeśli żaden element jest zgodny z żądanym Identyfikatorem, metoda zwraca odpowiedź 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) kod błędu.
* W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON. Zwracanie `item` skutkuje odpowiedź HTTP 200.

## <a name="the-puttodoitem-method"></a>Metoda PutTodoItem

`PutTodoItem` Badanie metody:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

`PutTodoItem` jest podobny do `PostTodoItem`, z wyjątkiem używa HTTP PUT. Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Zgodnie ze specyfikacją protokołu HTTP żądania PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko zmiany. Aby obsługiwać aktualizacje częściowe, należy użyć [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).

Jeśli wystąpi błąd podczas wywoływania `PutTodoItem`, wywołaj `GET` , aby upewnić się, że w bazie danych znajduje się element.

### <a name="test-the-puttodoitem-method"></a>Metoda PutTodoItem testu

Ten przykład korzysta z bazy danych w pamięci, która musi zostać zainicjowana za każdym razem, gdy aplikacja zostanie uruchomiona. Przed wykonaniem wywołania PUT musi istnieć element w bazie danych. Wywołaj polecenie GET, aby upewnić się, że w bazie danych znajduje się element, przed wykonaniem wywołania PUT.

Zaktualizuj element do wykonania o IDENTYFIKATORze 1 i ustaw jego nazwę na "Źródło danych":

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

Na poniższej ilustracji przedstawiono aktualizacji Postman:

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a>Metoda DeleteTodoItem

`DeleteTodoItem` Badanie metody:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

`DeleteTodoItem` Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

### <a name="test-the-deletetodoitem-method"></a>Metoda DeleteTodoItem testu

Użyj narzędzia Postman, aby usunąć zadanie do wykonania:

* Ustawia metodę `DELETE`.
* Ustaw identyfikator URI obiektu, aby usunąć, na przykład `https://localhost:5001/api/TodoItems/1`
* Wybierz **wysyłania**

## <a name="call-the-web-api-with-javascript"></a>Wywoływanie interfejsu API sieci Web przy użyciu języka JavaScript

Zobacz [samouczek: Wywołaj ASP.NET Core interfejs API sieci Web](xref:tutorials/web-api-javascript)przy użyciu języka JavaScript.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Z tego samouczka dowiesz się, jak wykonywać następujące czynności:

> [!div class="checklist"]
> * Utwórz projekt interfejsu API sieci Web.
> * Dodaj klasę modelu i kontekst bazy danych.
> * Dodawanie kontrolera.
> * Dodaj metody CRUD.
> * Konfigurowanie routingu i ścieżki adresu URL.
> * Określ wartości zwracane.
> * Wywoływanie internetowego interfejsu API za pomocą narzędzia Postman.
> * Wywołaj interfejs API sieci Web za pomocą języka JavaScript.

Na koniec masz internetowego interfejsu API, która może zarządzać "wykonania", przechowywane w relacyjnej bazie danych.

## <a name="overview"></a>Omówienie

Ten samouczek tworzy następujący interfejs API:

|interfejs API | Opis | Treść żądania | Treść odpowiedzi |
|--- | ---- | ---- | ---- |
|Pobierz/api/TodoItems | Pobierz wszystkie elementy zadań do wykonania | Brak | Tablica elementów do wykonania|
|Pobierz/api/TodoItems/{id} | Umieść element według Identyfikatora | Brak | Zadania do wykonania|
|Opublikuj/api/TodoItems | Dodaj nowy element | Zadania do wykonania | Zadania do wykonania |
|Umieść/api/TodoItems/{id} | Zaktualizuj istniejący element &nbsp; | Zadania do wykonania | Brak |
|Usuń/api/TodoItems/{id} &nbsp;&nbsp; | Usuwanie elementu &nbsp; &nbsp; | Brak | Brak|

Na poniższym diagramie przedstawiono projekt aplikacji.

![Klient jest reprezentowany przez pole po lewej stronie. Przesyła żądanie i odbiera odpowiedź z aplikacji, pole rysowane po prawej stronie. W polu aplikacji trzy pola reprezentują kontrolera, model i warstwy dostępu do danych. Żądanie jest dostarczany do kontrolera aplikacji, a operacje odczytu/zapisu występują między kontrolerem i warstwy dostępu do danych. Model jest serializowany i zwracany do klienta w odpowiedzi.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a>Tworzenie projektu sieci web

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z menu **plik** wybierz pozycję **Nowy** > **projekt**.
* Wybierz szablon **aplikacja sieci Web ASP.NET Core** a następnie kliknij przycisk **dalej**.
* Nazwij projekt *TodoApi* i kliknij pozycję **Utwórz**.
* W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 2,2** . Wybierz szablon **interfejsu API** i kliknij przycisk **Utwórz**. **Nie** zaznaczaj opcji **Włącz obsługę platformy Docker**.

![Okno dialogowe programu VS nowego projektu](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Zmień katalog (`cd`) do folderu, który będzie zawierać folder projektu.
* Uruchom następujące polecenia:

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  Te polecenia tworzą nowy projekt internetowego interfejsu API i otwierają nowe wystąpienie Visual Studio Code w nowym folderze projektu.

* Gdy zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz dodać wymagane zasoby do projektu, wybierz opcję **tak**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* Wybierz pozycję **plik** > **nowe rozwiązanie**.

  ![Nowe rozwiązanie w systemie macOS](first-web-api-mac/_static/sln.png)

* Wybierz pozycję **interfejs API** > > aplikacji> .NET Core.

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

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

Wybierz pozycję **Uruchom** > **Rozpocznij debugowanie** , aby uruchomić aplikację. Program Visual Studio for Mac otworzy w przeglądarce i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest numer portu wybranego losowo. Jest zwracany błąd HTTP 404 (nie znaleziono). Dołącz `/api/values` do adresu URL (adres URL, aby zmienić `https://localhost:<port>/api/values`).

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

* Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz pozycję **Dodaj** > **nowy plik** > **ogólna** > **pusta Klasa**.

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

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

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
* Wybierz pozycję **Dodaj** > **nowy element**.
* W **Dodaj nowy element** okno dialogowe, wybierz opcję **klasa formantu API** szablonu.
* Nazwa klasy *TodoController*i wybierz **Dodaj**.

  ![Dodaj okno dialogowe nowego elementu za pomocą kontrolera w wyszukiwania sieci web i pole Kontroler interfejsu api wybrane](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

* W *kontrolerów* folderu, Utwórz klasę o nazwie `TodoController`.

---

* Zastąp kod szablonu poniższym kodem:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Powyższy kod:

* Definiuje klasę kontrolera interfejsu API bez metody.
* Zdobi klasę z atrybutem [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) . Ten atrybut wskazuje, czy kontroler ma odpowiadać na żądania sieci web interfejsu API. Aby uzyskać informacje o określonych zachowaniach, które włącza atrybut <xref:web-api/index>, zobacz.
* Używa DI iniekcję kontekst bazy danych (`TodoContext`) do kontrolera. Kontekst bazy danych jest używany we wszystkich [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metodami w kontrolerze.
* Dodaje element o nazwie `Item1` do bazy danych, jeśli baza danych jest pusta. Ten kod jest w konstruktorze, aby była uruchamiania za każdym razem, gdy zostanie nowe żądanie HTTP. Jeśli usuniesz wszystkie elementy, Konstruktor tworzy `Item1` ponownie przy kolejnym wywoływana jest metoda interfejsu API. Może to wyglądać tak jak usunięcie nie działało, gdy rzeczywiście działa.

## <a name="add-get-methods"></a>Dodaj metody Get

Aby dostarczać interfejs API, który umożliwia pobranie elementów do wykonania, Dodaj następujące metody umożliwiające `TodoController` klasy:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

Te metody zaimplementować dwa GET punkty końcowe:

* `GET /api/todo`
* `GET /api/todo/{id}`

Zatrzymaj aplikację, jeśli jest nadal uruchomiona. Następnie uruchom ją ponownie, aby uwzględnić najnowsze zmiany.

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
* Jeśli atrybut ma szablon trasy (na `[HttpGet("products")]`przykład), Dodaj go do ścieżki. `[HttpGet]` W tym przykładzie nie używa szablonu. Aby uzyskać więcej informacji, zobacz [atrybutu, routing za pomocą atrybutów Http [polecenie]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

W następującym `GetTodoItem` metody `"{id}"` jest zmienną symbolu zastępczego dla Unikatowy identyfikator elementu do wykonania. Gdy `GetTodoItem` zostanie wywołana, wartość `"{id}"` w adresie URL jest przekazane do metody w jego`id` parametru.

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a>Zwracane wartości

Zwracany typ `GetTodoItems` i `GetTodoItem` metody jest [ActionResult\<T > typu](xref:web-api/action-return-types#actionresultt-type). Platforma ASP.NET Core automatycznie serializuje obiekt do [JSON](https://www.json.org/) i zapisuje dane JSON w treści komunikatu odpowiedzi. Kod odpowiedzi dla tego typu zwracanego jest równy 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków. Nieobsługiwane wyjątki są tłumaczone na błędy 5xx.

`ActionResult` zwracane typy może reprezentować kodów stanu szeroki zakres protokołu HTTP. Na przykład `GetTodoItem` może zwrócić dwie wartości inny stan:

* Jeśli żaden element jest zgodny z żądanym Identyfikatorem, metoda zwraca odpowiedź 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) kod błędu.
* W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON. Zwracanie `item` skutkuje odpowiedź HTTP 200.

## <a name="test-the-gettodoitems-method"></a>Metoda GetTodoItems testu

Ten samouczek używa narzędzia Postman do testowania internetowego interfejsu API.

* Zainstaluj [narzędzia Postman](https://www.getpostman.com/downloads/)
* Uruchamiają aplikację sieci web.
* Uruchom narzędzie Postman.
* Wyłącz **weryfikacji certyfikatu SSL**

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W obszarze **Ustawienia** **pliku** > (karta**Ogólne** ) Wyłącz **weryfikację certyfikatu SSL**.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

* Z poziomu**preferencji** programu **Poster** > (karta**Ogólne** ) Wyłącz **weryfikację certyfikatu SSL**. Alternatywnie wybierz klucz i wybierz pozycję **Ustawienia**, a następnie wyłącz weryfikację certyfikatu SSL.

---
  
> [!WARNING]
> Ponownie Włącz weryfikację certyfikatu SSL po przetestowaniu kontrolera.

* Utwórz nowe żądanie.
  * Ustawia metodę HTTP **UZYSKAĆ**.
  * Ustaw adres URL żądania `https://localhost:<port>/api/todo`. Na przykład `https://localhost:5001/api/todo`.
* Ustaw **widoku dwa okienka** w narzędziu Postman.
* Wybierz pozycję **Wyślij**.

![Postman przy użyciu żądania Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a>Dodawanie metody Create

Dodaj następującą `PostTodoItem` metodę w obszarze *controllers/TodoController. cs*: 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Powyższy kod jest metodą HTTP POST, wskazane przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu. Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.

`CreatedAtAction` Metody:

* Zwraca kod stanu HTTP 201, jeśli powodzenie. Protokół HTTP 201 jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.
* `Location` Dodaje nagłówek do odpowiedzi. `Location` Nagłówek określa identyfikator URI nowo utworzonego elementu do wykonania. Aby uzyskać więcej informacji, zobacz [10.2.2 201 utworzono](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Odwołuje `GetTodoItem` się do akcji `Location` tworzenia identyfikatora URI nagłówka. Słowo C# `nameof` kluczowe jest używane, aby zapobiec twardemu kodowaniu nazwy `CreatedAtAction` akcji w wywołaniu.

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a>Metoda PostTodoItem testu

* Skompiluj projekt.
* W narzędziu Postman, Ustawia metodę HTTP `POST`.
* Wybierz **treści** kartę.
* Wybierz **pierwotne** przycisku radiowego.
* Ustaw typ **JSON (application/json)** .
* W treści żądania wprowadź JSON element do wykonania:

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* Wybierz pozycję **Wyślij**.

  ![Postman przy użyciu Utwórz żądanie](first-web-api/_static/create.png)

  Jeśli wystąpi błąd 405 metody niedozwolonej, jest to prawdopodobnie wynik niekompilowania projektu po dodaniu `PostTodoItem` metody.

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

Jeśli wystąpi błąd podczas wywoływania `PutTodoItem`, wywołaj `GET` , aby upewnić się, że w bazie danych znajduje się element.

### <a name="test-the-puttodoitem-method"></a>Metoda PutTodoItem testu

Ten przykład korzysta z bazy danych w pamięci, która musi zostać zainicjowana za każdym razem, gdy aplikacja zostanie uruchomiona. Przed wykonaniem wywołania PUT musi istnieć element w bazie danych. Wywołaj polecenie GET, aby upewnić się, że w bazie danych znajduje się element, przed wykonaniem wywołania PUT.

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

Przykładowa aplikacja umożliwia usunięcie wszystkich elementów. Jednak po usunięciu ostatniego elementu jest on tworzony przez konstruktora klasy modelu przy następnym wywołaniu interfejsu API.

## <a name="call-the-web-api-with-javascript"></a>Wywoływanie interfejsu API sieci Web przy użyciu języka JavaScript

W tej sekcji zostanie dodana strona HTML, która używa języka JavaScript do wywoływania internetowego interfejsu API. Interfejs API pobierania inicjuje żądanie. Język JavaScript aktualizuje stronę ze szczegółowymi informacjami z odpowiedzi internetowego interfejsu API.

Skonfiguruj aplikację do [obsługi plików statycznych](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [Włącz domyślne mapowanie plików](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) , aktualizując *Startup.cs* z następującym wyróżnionym kodem:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

Tworzenie *wwwroot* folder w katalogu projektu.

Dodaj plik HTML o nazwie *index.html* do *wwwroot* katalogu. Zastąp jego zawartość następującym kodem:

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

Dodaj plik języka JavaScript o nazwie *site.js* do *wwwroot* katalogu. Zastąp jego zawartość następującym kodem:

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

Zmiana ustawień uruchamiania projektów ASP.NET Core może być konieczne test lokalnie za pomocą strony HTML:

* Otwórz *Properties\launchSettings.json*.
* Usuń `launchUrl` właściwości, aby wymusić na aplikacji, aby otworzyć w *index.html*&mdash;pliku domyślnego projektu.

Ten przykład wywołuje wszystkie metody CRUD internetowego interfejsu API. Poniżej przedstawiono objaśnienia dotyczące wywołań interfejsu API.

### <a name="get-a-list-of-to-do-items"></a>Pobierz listę elementów do wykonania

Polecenie pobrania wysyła żądanie HTTP GET do internetowego interfejsu API, który zwraca kod JSON reprezentujący tablicę elementów do wykonania. `success` Wywołaniu funkcji wywołania zwrotnego, jeśli żądanie zakończy się powodzeniem. Podczas wywołania zwrotnego model DOM jest aktualizowana informacjami zadań do wykonania.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Dodaj element do wykonania

Polecenie pobrania wysyła żądanie HTTP POST z elementem do wykonania w treści żądania. `accepts` i `contentType` opcje są ustawione na `application/json` Aby określić typ nośnika odbieranych i wysyłanych. Element do wykonania jest konwertowana na format JSON za pomocą [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Gdy interfejs API zwraca kod stanu powodzenia `getData` wywołaniu funkcji można zaktualizować tabeli HTML.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Zaktualizuj element do wykonania

Aktualizowanie zadanie do wykonania jest podobne do dodawania jednego. `url` Zmiany do Dodaj Unikatowy identyfikator elementu, a `type` jest `PUT`.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Usuń element do wykonania

Trwa usuwanie zadania do wykonania odbywa się przez ustawienie `type` na wywołanie AJAX do `DELETE` i podając unikatowy identyfikator elementu w adresie URL.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

[Wyświetlanie lub pobieranie przykładowego kodu w tym samouczku](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples). Zobacz [sposobu pobierania](xref:index#how-to-download-a-sample).

Aby uzyskać więcej informacji, zobacz następujące zasoby:

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [Wersja tego samouczka usługi YouTube](https://www.youtube.com/watch?v=TTkhEyGBfAk)
