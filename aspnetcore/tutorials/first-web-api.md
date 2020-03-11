---
title: 'Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core'
author: rick-anderson
description: Dowiedz się, jak utworzyć internetowy interfejs API za pomocą ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 2/25/2020
uid: tutorials/first-web-api
ms.openlocfilehash: 55dfc05b5c96f7fa060d537745bac969e92daa9b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655590"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a>Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core

Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT), [Kirka Larkin](https://twitter.com/serpent5)i [Jan Wasson](https://github.com/mikewasson)

W tym samouczku pokazano podstawy tworzenia internetowego interfejsu API za pomocą programu ASP.NET Core.

::: moniker range=">= aspnetcore-3.0"

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz projekt interfejsu API sieci Web.
> * Dodaj klasę modelu i kontekst bazy danych.
> * Tworzy szkielet kontrolera z metodami CRUD.
> * Skonfiguruj Routing, ścieżki URL i wartości zwracane.
> * Wywoływanie internetowego interfejsu API za pomocą narzędzia Postman.

Na końcu znajduje się internetowy interfejs API, który może zarządzać elementami do wykonania przechowywanymi w bazie danych.

## <a name="overview"></a>Omówienie

Ten samouczek tworzy następujący interfejs API:

|Interfejs API | Opis | Treść żądania | Treść odpowiedzi |
|--- | ---- | ---- | ---- |
|Pobierz/api/TodoItems | Pobierz wszystkie elementy zadań do wykonania | None | Tablica elementów do wykonania|
|Pobierz/api/TodoItems/{id} | Umieść element według Identyfikatora | None | Zadania do wykonania|
|Opublikuj/api/TodoItems | Dodaj nowy element | Zadania do wykonania | Zadania do wykonania |
|Umieść/api/TodoItems/{id} | Aktualizowanie istniejącego elementu &nbsp; | Zadania do wykonania | None |
|Usuń/api/TodoItems/{id} &nbsp; &nbsp; | Usuń element &nbsp; &nbsp; | None | None|

Na poniższym diagramie przedstawiono projekt aplikacji.

![Klient jest reprezentowany przez pole po lewej stronie. Przesyła żądanie i odbiera odpowiedź z aplikacji, pole rysowane po prawej stronie. W polu aplikacji trzy pola reprezentują kontrolera, model i warstwy dostępu do danych. Żądanie jest dostarczany do kontrolera aplikacji, a operacje odczytu/zapisu występują między kontrolerem i warstwy dostępu do danych. Model jest serializowany i zwracany do klienta w odpowiedzi.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-project"></a>Tworzenie projektu sieci web

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z menu **plik** wybierz pozycję **Nowy** **projekt**>.
* Wybierz szablon **aplikacja sieci Web ASP.NET Core** a następnie kliknij przycisk **dalej**.
* Nazwij projekt *TodoApi* i kliknij pozycję **Utwórz**.
* W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 3,1** . Wybierz szablon **interfejsu API** i kliknij przycisk **Utwórz**.

![Okno dialogowe programu VS nowego projektu](first-web-api/_static/vs3.png)

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Zmień katalog (`cd`) do folderu, który będzie zawierać folder projektu.
* Uruchom następujące polecenia:

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* Gdy zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz dodać wymagane zasoby do projektu, wybierz opcję **tak**.

  Poprzedniego polecenia:

  * Tworzy nowy projekt internetowego interfejsu API i otwiera go w Visual Studio Code.
  * Dodaje pakiety NuGet, które są wymagane w następnej sekcji.

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* Wybierz pozycję **plik** > **nowe rozwiązanie**.

  ![Nowe rozwiązanie w systemie macOS](first-web-api-mac/_static/sln.png)

* Wybierz pozycję **.NET Core** > **App** > **API** > **dalej**.

  ![okno dialogowe z systemem macOS nowego projektu](first-web-api-mac/_static/1.png)
  
* W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** wybierz pozycję **docelowa platforma** * *.NET Core 3,1*.

* Wprowadź *TodoApi* jako **nazwę projektu** , a następnie wybierz pozycję **Utwórz**.

  ![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

Otwórz Terminal poleceń w folderze projektu i uruchom następujące polecenia:

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a>Testowanie interfejsu API

Szablon projektu tworzy interfejs API `WeatherForecast`. Wywołaj metodę `Get` z poziomu przeglądarki, aby przetestować aplikację.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację. Program Visual Studio uruchamia przeglądarkę i przechodzi do `https://localhost:<port>/WeatherForecast`, gdzie `<port>` to losowo wybierany numer portu.

Jeśli zostanie wyświetlone okno dialogowe z pytaniem, czy należy zaufać certyfikatowi IIS Express, wybierz pozycję **tak**. W wyświetlonym oknie dialogowym **ostrzeżenia o zabezpieczeniach** wybierz pozycję **tak**.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację. W przeglądarce przejdź do następującego adresu URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

Wybierz pozycję **uruchom** > **Rozpocznij debugowanie** , aby uruchomić aplikację. Visual Studio dla komputerów Mac uruchamia przeglądarkę i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest losowo wybranym numerem portu. Jest zwracany błąd HTTP 404 (nie znaleziono). Dołącz `/WeatherForecast` do adresu URL (Zmień adres URL na `https://localhost:<port>/WeatherForecast`).

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

*Model* to zestaw klas, które reprezentują dane zarządzane przez aplikację. Model tej aplikacji jest pojedynczym `TodoItem` klasą.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt. Wybierz pozycję **dodaj** > **Nowy folder**. Nazwij *modele*folderów.

* Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** **klasę** > . Nadaj klasie nazwę *TodoItem* i wybierz pozycję **Dodaj**.

* Zastąp kod szablonu poniższym kodem:

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Dodaj folder o nazwie *models*.

* Dodaj klasę `TodoItem` do folderu *models* o następującym kodzie:

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* Kliknij prawym przyciskiem myszy projekt. Wybierz pozycję **dodaj** > **Nowy folder**. Nazwij *modele*folderów.

  ![Nowy folder](first-web-api-mac/_static/folder.png)

* Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** > **nowy plik** > **Ogólne** > **pustej klasy**.

* Nazwij klasę *TodoItem*, a następnie kliknij pozycję **New (nowy**).

* Zastąp kod szablonu poniższym kodem:

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs?name=snippet)]

Właściwość `Id` działa jako unikatowy klucz w relacyjnej bazie danych.

Klasy modelu mogą przejść do dowolnego miejsca w projekcie, ale folder *modele* jest używany przez Konwencję.

## <a name="add-a-database-context"></a>Dodawanie kontekstu bazy danych

*Kontekst bazy danych* jest główną klasą, która koordynuje Entity Framework funkcji dla modelu danych. Ta klasa jest tworzona przez wyprowadzanie z klasy `Microsoft.EntityFrameworkCore.DbContext`.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a>Dodaj Microsoft. EntityFrameworkCore. SqlServer

* W menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet > Zarządzanie pakietami NuGet dla rozwiązania**.
* Wybierz kartę **Przeglądaj** , a następnie w polu wyszukiwania wprowadź ciąg **Microsoft. EntityFrameworkCore. SqlServer** .
* W lewym okienku wybierz pozycję **Microsoft. EntityFrameworkCore. SqlServer** .
* Zaznacz pole wyboru **projekt** w prawym okienku, a następnie wybierz pozycję **Zainstaluj**.
* Aby dodać `Microsoft.EntityFrameworkCore.InMemory` pakiet NuGet, użyj powyższych instrukcji.

![Menedżer pakietów NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a>Dodawanie kontekstu bazy danych TodoContext

* Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** **klasę** > . Nadaj klasie nazwę *TodoContext* i kliknij przycisk **Dodaj**.

# <a name="visual-studio-code--visual-studio-for-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

* Dodaj klasę `TodoContext` do folderu *models* .

---

* Wprowadź następujący kod:

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a>Zarejestruj kontekst bazy danych

W ASP.NET Core usługi, takie jak kontekst bazy danych, muszą być zarejestrowane z kontenerem [iniekcji zależności (di)](xref:fundamentals/dependency-injection) . Kontener zawiera usługę do kontrolerów.

Zaktualizuj *Startup.cs* o następujący wyróżniony kod:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

Powyższy kod:

* Usuwa nieużywane deklaracje `using`.
* Dodaje kontener DI kontekst bazy danych.
* Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.

## <a name="scaffold-a-controller"></a>Tworzenie szkieletu kontrolera

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Kliknij prawym przyciskiem myszy folder *controllers* .
* Wybierz pozycję **dodaj** > **nowy element szkieletowy**.
* Wybierz pozycję **kontroler interfejsu API z akcjami, używając Entity Framework**, a następnie wybierz pozycję **Dodaj**.
* Na stronie **Dodawanie kontrolera interfejsu API z akcjami przy użyciu Entity Framework** dialogowego:

  * Wybierz pozycję **TodoItem (TodoApi. models)** w **klasie model**.
  * W **klasie kontekstu danych**wybierz pozycję **TodoContext (TodoApi. models)** .
  * Wybierz pozycję **Dodaj**.

# <a name="visual-studio-code--visual-studio-for-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

Uruchom następujące polecenia:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

Poprzedniego polecenia:

* Dodaj pakiety NuGet wymagane do tworzenia szkieletów.
* Instaluje aparat tworzenia szkieletu (`dotnet-aspnet-codegenerator`).
* Szkieletuje `TodoItemsController`.

---

Wygenerowany kod:

* Oznacza klasę atrybutem [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) . Ten atrybut wskazuje, czy kontroler ma odpowiadać na żądania sieci web interfejsu API. Aby uzyskać informacje o określonych zachowaniach, które włącza atrybut, zobacz <xref:web-api/index>.
* Używa funkcji DI do iniekcji kontekstu bazy danych (`TodoContext`) do kontrolera. Kontekst bazy danych jest używany w każdej z metod [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) w kontrolerze.

Szablony ASP.NET Core dla:

* Kontrolery z widokami obejmują `[action]` w szablonie trasy.
* Kontrolery interfejsu API nie uwzględniają `[action]` w szablonie trasy.

Gdy token `[action]` nie znajduje się w szablonie trasy, nazwa [akcji](xref:mvc/controllers/routing#action) jest wykluczona z trasy. Oznacza to, że nazwa metody skojarzonej z akcją nie jest używana w zgodnej trasie.

## <a name="examine-the-posttodoitem-create-method"></a>Badanie metody PostTodoItem Create

Zastąp instrukcję return w `PostTodoItem`, aby użyć operatora [nameof](/dotnet/csharp/language-reference/operators/nameof) :

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

Poprzedni kod jest metodą POST protokołu HTTP, jak wskazano w atrybucie [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) . Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.

Metoda <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>:

* W razie powodzenia zwraca kod stanu HTTP 201. Protokół HTTP 201 jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.
* Dodaje nagłówek [lokalizacji](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) do odpowiedzi. Nagłówek `Location` określa [Identyfikator URI](https://developer.mozilla.org/docs/Glossary/URI) nowo utworzonego elementu do wykonania. Aby uzyskać więcej informacji, zobacz [10.2.2 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Odwołuje się do akcji `GetTodoItem`, aby utworzyć identyfikator URI nagłówka `Location`. Słowo C# kluczowe `nameof` jest używane w celu uniknięcia twardej kodowania nazwy akcji w wywołaniu `CreatedAtAction`.

### <a name="install-postman"></a>Zainstaluj program Poster

Ten samouczek używa narzędzia Postman do testowania internetowego interfejsu API.

* Zainstaluj program [Poster](https://www.getpostman.com/downloads/)
* Uruchamiają aplikację sieci web.
* Uruchom narzędzie Postman.
* Wyłącz **weryfikację certyfikatu SSL**
  * Z **ustawień** > **plików** (karta**Ogólne** ), wyłącz **weryfikację certyfikatu SSL**.
    > [!WARNING]
    > Ponownie Włącz weryfikację certyfikatu SSL po przetestowaniu kontrolera.

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a>Test PostTodoItem za pomocą programu Poster

* Utwórz nowe żądanie.
* Ustaw metodę HTTP na `POST`.
* Wybierz kartę **Treść**.
* Wybierz przycisk radiowy **RAW** .
* Ustaw typ na **JSON (Application/JSON)** .
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

* Wybierz kartę **nagłówki** w okienku **odpowiedź** .
* Skopiuj wartość nagłówka **lokalizacji** :

  ![Karta nagłówki konsoli narzędzia Postman](first-web-api/_static/3/create.png)

* Ustaw metodę GET.
* Wklej URI (na przykład `https://localhost:5001/api/TodoItems/1`).
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
* Ustaw metodę HTTP, aby **uzyskać**.
* Ustaw adres URL żądania na `https://localhost:<port>/api/TodoItems`. Na przykład `https://localhost:5001/api/TodoItems`.
* Ustaw **dwa widoki okienka** w programie Poster.
* Wybierz pozycję **Wyślij**.

Ta aplikacja używa bazy danych w pamięci. Jeśli aplikacja zostanie zatrzymana i uruchomiona, poprzednie żądanie GET nie zwróci żadnych danych. Jeśli nie zostaną zwrócone żadne dane, [Opublikuj](#post) dane w aplikacji.

## <a name="routing-and-url-paths"></a>Ścieżki routingu i adres URL

Atrybut [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) oznacza metodę, która reaguje na żądanie HTTP GET. Ścieżka adresu URL, dla każdej z metod jest zbudowany w następujący sposób:

* Rozpocznij od ciągu szablonu w atrybucie `Route` kontrolera:

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* Zastąp `[controller]` nazwą kontrolera, którą Konwencją jest nazwa klasy kontrolera minus sufiks "Controller". Dla tego przykładu nazwa klasy kontrolera to **TodoItems**Controller, więc nazwa kontrolera to "TodoItems". W ASP.NET Core [routingu](xref:mvc/controllers/routing) jest rozróżniana wielkość liter.
* Jeśli atrybut `[HttpGet]` ma szablon trasy (na przykład `[HttpGet("products")]`), dołącz go do ścieżki. W tym przykładzie nie używa szablonu. Aby uzyskać więcej informacji, zobacz temat [Routing atrybutów z atrybutami http [Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

W poniższej metodzie `GetTodoItem` `"{id}"` jest zmienną zastępczą dla unikatowego identyfikatora elementu do wykonania. Po wywołaniu `GetTodoItem` wartość `"{id}"` w adresie URL jest podawana do metody w `id` parametr.

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a>Zwracane wartości

Typem zwracanym `GetTodoItems` i `GetTodoItem` Metoda jest [ActionResult\<t > typ](xref:web-api/action-return-types#actionresultt-type). ASP.NET Core automatycznie serializować obiektu do [formatu JSON](https://www.json.org/) i zapisuje kod JSON w treści komunikatu odpowiedzi. Kod odpowiedzi dla tego typu zwracanego jest równy 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków. Nieobsługiwane wyjątki są tłumaczone na błędy 5xx.

`ActionResult` zwracane typy mogą reprezentować szeroką gamę kodów stanu HTTP. Na przykład `GetTodoItem` mogą zwracać dwie różne wartości stanu:

* Jeśli żaden element nie jest zgodny z żądanym IDENTYFIKATORem, metoda zwraca 404 kod błędu [NOTFOUND](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) .
* W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON. Zwracanie `item` wyników w odpowiedzi HTTP 200.

## <a name="the-puttodoitem-method"></a>Metoda PutTodoItem

Przeanalizuj metodę `PutTodoItem`:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

`PutTodoItem` jest podobna do `PostTodoItem`, z tą różnicą, że używa protokołu HTTP PUT. Odpowiedź to [204 (brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Zgodnie ze specyfikacją protokołu HTTP żądania PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko zmiany. Aby zapewnić obsługę częściowych aktualizacji, użyj [poprawki http](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).

Jeśli wystąpi błąd podczas wywoływania `PutTodoItem`, wywołaj `GET`, aby upewnić się, że w bazie danych znajduje się element.

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

Przeanalizuj metodę `DeleteTodoItem`:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

### <a name="test-the-deletetodoitem-method"></a>Metoda DeleteTodoItem testu

Użyj narzędzia Postman, aby usunąć zadanie do wykonania:

* Ustaw metodę na `DELETE`.
* Ustaw identyfikator URI obiektu do usunięcia (na przykład `https://localhost:5001/api/TodoItems/1`).
* Wybierz pozycję **Wyślij**.

<a name="over-post"></a>

## <a name="prevent-over-posting"></a>Zapobiegaj za pośrednictwem księgowania

Obecnie Przykładowa aplikacja uwidacznia cały obiekt `TodoItem`. Aplikacje produkcji zwykle ograniczają dane wejściowe i zwracane przy użyciu podzestawu modelu. Istnieje wiele powodów związanych z tym, a zabezpieczenia są głównymi. Podzestaw modelu jest zwykle określany jako obiekt Transfer danych (DTO), model wejściowy lub model widoku. **DTO** jest używany w tym artykule.

DTO może służyć do:

* Zablokuj nadmierne księgowanie.
* Ukryj właściwości, które nie powinny być wyświetlane dla klientów.
* Pomiń niektóre właściwości, aby zmniejszyć rozmiar ładunku.
* Spłaszcz wykresy obiektów zawierające obiekty zagnieżdżone. Spłaszczone wykresy obiektów mogą być wygodniejsze dla klientów.

Aby zademonstrować podejście DTO, zaktualizuj klasę `TodoItem` w celu uwzględnienia pola tajnego:

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Models/TodoItem.cs?name=snippet&highlight=6)]

Pole tajne musi być ukryte w tej aplikacji, ale aplikacja administracyjna mogła ją uwidocznić.

Sprawdź, czy można opublikować i pobrać pole tajne.

Utwórz model DTO:

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Models/TodoItemDTO.cs?name=snippet)]

Zaktualizuj `TodoItemsController`, aby użyć `TodoItemDTO`:

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet)]

Upewnij się, że nie można opublikować lub pobrać pola tajnego.

## <a name="call-the-web-api-with-javascript"></a>Wywoływanie interfejsu API sieci Web przy użyciu języka JavaScript

Zobacz [Samouczek: wywoływanie interfejsu API sieci web ASP.NET Core przy użyciu języka JavaScript](xref:tutorials/web-api-javascript).

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

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

|Interfejs API | Opis | Treść żądania | Treść odpowiedzi |
|--- | ---- | ---- | ---- |
|Pobierz/api/TodoItems | Pobierz wszystkie elementy zadań do wykonania | None | Tablica elementów do wykonania|
|Pobierz/api/TodoItems/{id} | Umieść element według Identyfikatora | None | Zadania do wykonania|
|Opublikuj/api/TodoItems | Dodaj nowy element | Zadania do wykonania | Zadania do wykonania |
|Umieść/api/TodoItems/{id} | Aktualizowanie istniejącego elementu &nbsp; | Zadania do wykonania | None |
|Usuń/api/TodoItems/{id} &nbsp; &nbsp; | Usuń element &nbsp; &nbsp; | None | None|

Na poniższym diagramie przedstawiono projekt aplikacji.

![Klient jest reprezentowany przez pole po lewej stronie. Przesyła żądanie i odbiera odpowiedź z aplikacji, pole rysowane po prawej stronie. W polu aplikacji trzy pola reprezentują kontrolera, model i warstwy dostępu do danych. Żądanie jest dostarczany do kontrolera aplikacji, a operacje odczytu/zapisu występują między kontrolerem i warstwy dostępu do danych. Model jest serializowany i zwracany do klienta w odpowiedzi.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a>Tworzenie projektu sieci web

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z menu **plik** wybierz pozycję **Nowy** **projekt**>.
* Wybierz szablon **aplikacja sieci Web ASP.NET Core** a następnie kliknij przycisk **dalej**.
* Nazwij projekt *TodoApi* i kliknij pozycję **Utwórz**.
* W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 2,2** . Wybierz szablon **interfejsu API** i kliknij przycisk **Utwórz**. **Nie** zaznaczaj opcji **Włącz obsługę platformy Docker**.

![Okno dialogowe programu VS nowego projektu](first-web-api/_static/vs.png)

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Zmień katalog (`cd`) do folderu, który będzie zawierać folder projektu.
* Uruchom następujące polecenia:

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  Te polecenia tworzą nowy projekt internetowego interfejsu API i otwierają nowe wystąpienie Visual Studio Code w nowym folderze projektu.

* Gdy zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz dodać wymagane zasoby do projektu, wybierz opcję **tak**.

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* Wybierz pozycję **plik** > **nowe rozwiązanie**.

  ![Nowe rozwiązanie w systemie macOS](first-web-api-mac/_static/sln.png)

* Wybierz pozycję **.NET Core** > **App** > **API** > **dalej**.

  ![okno dialogowe z systemem macOS nowego projektu](first-web-api-mac/_static/1.png)
  
* W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** zaakceptuj domyślną **platformę docelową** programu * *.NET Core 2,2*.

* Wprowadź *TodoApi* jako **nazwę projektu** , a następnie wybierz pozycję **Utwórz**.

  ![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a>Testowanie interfejsu API

Szablon projektu tworzy interfejs API `values`. Wywołaj metodę `Get` z poziomu przeglądarki, aby przetestować aplikację.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację. Program Visual Studio uruchamia przeglądarkę i przechodzi do `https://localhost:<port>/api/values`, gdzie `<port>` to losowo wybierany numer portu.

Jeśli zostanie wyświetlone okno dialogowe z pytaniem, czy należy zaufać certyfikatowi IIS Express, wybierz pozycję **tak**. W wyświetlonym oknie dialogowym **ostrzeżenia o zabezpieczeniach** wybierz pozycję **tak**.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Naciśnij klawisze Ctrl + F5, aby uruchomić aplikację. W przeglądarce przejdź do następującego adresu URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

Wybierz pozycję **uruchom** > **Rozpocznij debugowanie** , aby uruchomić aplikację. Visual Studio dla komputerów Mac uruchamia przeglądarkę i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest losowo wybranym numerem portu. Jest zwracany błąd HTTP 404 (nie znaleziono). Dołącz `/api/values` do adresu URL (Zmień adres URL na `https://localhost:<port>/api/values`).

---

Zwracane są następujące dane JSON:

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a>Dodawanie klasy modelu

*Model* to zestaw klas, które reprezentują dane zarządzane przez aplikację. Model tej aplikacji jest pojedynczym `TodoItem` klasą.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt. Wybierz pozycję **dodaj** > **Nowy folder**. Nazwij *modele*folderów.

* Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** **klasę** > . Nadaj klasie nazwę *TodoItem* i wybierz pozycję **Dodaj**.

* Zastąp kod szablonu poniższym kodem:

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Dodaj folder o nazwie *models*.

* Dodaj klasę `TodoItem` do folderu *models* o następującym kodzie:

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* Kliknij prawym przyciskiem myszy projekt. Wybierz pozycję **dodaj** > **Nowy folder**. Nazwij *modele*folderów.

  ![Nowy folder](first-web-api-mac/_static/folder.png)

* Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** > **nowy plik** > **Ogólne** > **pustej klasy**.

* Nazwij klasę *TodoItem*, a następnie kliknij pozycję **New (nowy**).

* Zastąp kod szablonu poniższym kodem:

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

Właściwość `Id` działa jako unikatowy klucz w relacyjnej bazie danych.

Klasy modelu mogą przejść do dowolnego miejsca w projekcie, ale folder *modele* jest używany przez Konwencję.

## <a name="add-a-database-context"></a>Dodawanie kontekstu bazy danych

*Kontekst bazy danych* jest główną klasą, która koordynuje Entity Framework funkcji dla modelu danych. Ta klasa jest tworzona przez wyprowadzanie z klasy `Microsoft.EntityFrameworkCore.DbContext`.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** **klasę** > . Nadaj klasie nazwę *TodoContext* i kliknij przycisk **Dodaj**.

# <a name="visual-studio-code--visual-studio-for-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

* Dodaj klasę `TodoContext` do folderu *models* .

---

* Zastąp kod szablonu poniższym kodem:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a>Zarejestruj kontekst bazy danych

W ASP.NET Core usługi, takie jak kontekst bazy danych, muszą być zarejestrowane z kontenerem [iniekcji zależności (di)](xref:fundamentals/dependency-injection) . Kontener zawiera usługę do kontrolerów.

Zaktualizuj *Startup.cs* o następujący wyróżniony kod:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

Powyższy kod:

* Usuwa nieużywane deklaracje `using`.
* Dodaje kontener DI kontekst bazy danych.
* Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.

## <a name="add-a-controller"></a>Dodawanie kontrolera

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Kliknij prawym przyciskiem myszy folder *controllers* .
* Wybierz pozycję **dodaj** > **nowy element**.
* W oknie dialogowym **Dodaj nowy element** wybierz szablon **Klasa kontrolera interfejsu API** .
* Nadaj klasie nazwę *TodoController*i wybierz pozycję **Dodaj**.

  ![Dodaj okno dialogowe nowego elementu za pomocą kontrolera w wyszukiwania sieci web i pole Kontroler interfejsu api wybrane](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

* W folderze *controllers* Utwórz klasę o nazwie `TodoController`.

---

* Zastąp kod szablonu poniższym kodem:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Powyższy kod:

* Definiuje klasę kontrolera interfejsu API bez metody.
* Oznacza klasę atrybutem [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) . Ten atrybut wskazuje, czy kontroler ma odpowiadać na żądania sieci web interfejsu API. Aby uzyskać informacje o określonych zachowaniach, które włącza atrybut, zobacz <xref:web-api/index>.
* Używa funkcji DI do iniekcji kontekstu bazy danych (`TodoContext`) do kontrolera. Kontekst bazy danych jest używany w każdej z metod [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) w kontrolerze.
* Dodaje element o nazwie `Item1` do bazy danych, jeśli baza danych jest pusta. Ten kod jest w konstruktorze, aby była uruchamiania za każdym razem, gdy zostanie nowe żądanie HTTP. Jeśli usuniesz wszystkie elementy, Konstruktor utworzy `Item1` ponownie przy następnym wywołaniu metody interfejsu API. Może to wyglądać tak jak usunięcie nie działało, gdy rzeczywiście działa.

## <a name="add-get-methods"></a>Dodaj metody Get

Aby udostępnić interfejs API, który pobiera elementy do wykonania, Dodaj następujące metody do klasy `TodoController`:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

Te metody zaimplementować dwa GET punkty końcowe:

* `GET /api/todo`
* `GET /api/todo/{id}`

Zatrzymaj aplikację, jeśli jest nadal uruchomiona. Następnie uruchom ją ponownie, aby uwzględnić najnowsze zmiany.

Testowanie aplikacji, wywołując dwa punkty końcowe w przeglądarce. Na przykład:

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

Wywołanie do `GetTodoItems`jest generowane w następującej odpowiedzi HTTP:

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

Atrybut [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) oznacza metodę, która reaguje na żądanie HTTP GET. Ścieżka adresu URL, dla każdej z metod jest zbudowany w następujący sposób:

* Rozpocznij od ciągu szablonu w atrybucie `Route` kontrolera:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* Zastąp `[controller]` nazwą kontrolera, którą Konwencją jest nazwa klasy kontrolera minus sufiks "Controller". W przypadku tego przykładu nazwa klasy kontrolera to kontroler do **zrobienia**, więc nazwa kontrolera to "do zrobienia". W ASP.NET Core [routingu](xref:mvc/controllers/routing) jest rozróżniana wielkość liter.
* Jeśli atrybut `[HttpGet]` ma szablon trasy (na przykład `[HttpGet("products")]`), dołącz go do ścieżki. W tym przykładzie nie używa szablonu. Aby uzyskać więcej informacji, zobacz temat [Routing atrybutów z atrybutami http [Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

W poniższej metodzie `GetTodoItem` `"{id}"` jest zmienną zastępczą dla unikatowego identyfikatora elementu do wykonania. Po wywołaniu `GetTodoItem` wartość `"{id}"` w adresie URL jest podawana do metody w`id` parametr.

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a>Zwracane wartości

Typem zwracanym `GetTodoItems` i `GetTodoItem` Metoda jest [ActionResult\<t > typ](xref:web-api/action-return-types#actionresultt-type). ASP.NET Core automatycznie serializować obiektu do [formatu JSON](https://www.json.org/) i zapisuje kod JSON w treści komunikatu odpowiedzi. Kod odpowiedzi dla tego typu zwracanego jest równy 200, zakładając, że nie ma żadnych nieobsłużonych wyjątków. Nieobsługiwane wyjątki są tłumaczone na błędy 5xx.

`ActionResult` zwracane typy mogą reprezentować szeroką gamę kodów stanu HTTP. Na przykład `GetTodoItem` mogą zwracać dwie różne wartości stanu:

* Jeśli żaden element nie jest zgodny z żądanym IDENTYFIKATORem, metoda zwraca 404 kod błędu [NOTFOUND](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) .
* W przeciwnym razie metoda zwraca 200 treści odpowiedzi JSON. Zwracanie `item` wyników w odpowiedzi HTTP 200.

## <a name="test-the-gettodoitems-method"></a>Metoda GetTodoItems testu

Ten samouczek używa narzędzia Postman do testowania internetowego interfejsu API.

* Zainstaluj program [Poster](https://www.getpostman.com/downloads/).
* Uruchamiają aplikację sieci web.
* Uruchom narzędzie Postman.
* Wyłącz **weryfikację certyfikatu SSL**.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **ustawień** > **plików** (karta**Ogólne** ), wyłącz **weryfikację certyfikatu SSL**.

# <a name="visual-studio-code--visual-studio-for-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

* Z poziomu **preferencji** > **Poster** (karta**Ogólne** ) Wyłącz **weryfikację certyfikatu SSL**. Alternatywnie wybierz klucz i wybierz pozycję **Ustawienia**, a następnie wyłącz weryfikację certyfikatu SSL.

---
  
> [!WARNING]
> Ponownie Włącz weryfikację certyfikatu SSL po przetestowaniu kontrolera.

* Utwórz nowe żądanie.
  * Ustaw metodę HTTP, aby **uzyskać**.
  * Ustaw adres URL żądania na `https://localhost:<port>/api/todo`. Na przykład `https://localhost:5001/api/todo`.
* Ustaw **dwa widoki okienka** w programie Poster.
* Wybierz pozycję **Wyślij**.

![Postman przy użyciu żądania Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a>Dodawanie metody Create

Dodaj następującą metodę `PostTodoItem` wewnątrz *kontrolera/TodoController. cs*: 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Poprzedni kod jest metodą POST protokołu HTTP, jak wskazano w atrybucie [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) . Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.

Metoda `CreatedAtAction`:

* Zwraca kod stanu HTTP 201, jeśli powodzenie. Protokół HTTP 201 jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.
* Dodaje nagłówek `Location` do odpowiedzi. Nagłówek `Location` określa identyfikator URI nowo utworzonego elementu do wykonania. Aby uzyskać więcej informacji, zobacz [10.2.2 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Odwołuje się do akcji `GetTodoItem`, aby utworzyć identyfikator URI nagłówka `Location`. Słowo C# kluczowe `nameof` jest używane w celu uniknięcia twardej kodowania nazwy akcji w wywołaniu `CreatedAtAction`.

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a>Metoda PostTodoItem testu

* Skompiluj projekt.
* W polu Poster ustaw metodę HTTP na `POST`.
* Wybierz kartę **Treść**.
* Wybierz przycisk radiowy **RAW** .
* Ustaw typ na **JSON (Application/JSON)** .
* W treści żądania wprowadź JSON element do wykonania:

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* Wybierz pozycję **Wyślij**.

  ![Postman przy użyciu Utwórz żądanie](first-web-api/_static/create.png)

  Jeśli wystąpi błąd 405 metody niedozwolonej, jest to prawdopodobnie wynik niekompilowania projektu po dodaniu metody `PostTodoItem`.

### <a name="test-the-location-header-uri"></a>Testowanie nagłówek location identyfikator URI

* Wybierz kartę **nagłówki** w okienku **odpowiedź** .
* Skopiuj wartość nagłówka **lokalizacji** :

  ![Karta nagłówki konsoli narzędzia Postman](first-web-api/_static/pmc2.png)

* Ustaw metodę GET.
* Wklej URI (na przykład `https://localhost:5001/api/Todo/2`).
* Wybierz pozycję **Wyślij**.

## <a name="add-a-puttodoitem-method"></a>Dodaj metodę PutTodoItem

Dodaj następującą metodę `PutTodoItem`:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`PutTodoItem` jest podobna do `PostTodoItem`, z tą różnicą, że używa protokołu HTTP PUT. Odpowiedź to [204 (brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Zgodnie ze specyfikacją protokołu HTTP żądania PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko zmiany. Aby zapewnić obsługę częściowych aktualizacji, użyj [poprawki http](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).

Jeśli wystąpi błąd podczas wywoływania `PutTodoItem`, wywołaj `GET`, aby upewnić się, że w bazie danych znajduje się element.

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

Dodaj następującą metodę `DeleteTodoItem`:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

Odpowiedź `DeleteTodoItem` to [204 (brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

### <a name="test-the-deletetodoitem-method"></a>Metoda DeleteTodoItem testu

Użyj narzędzia Postman, aby usunąć zadanie do wykonania:

* Ustaw metodę na `DELETE`.
* Ustaw identyfikator URI obiektu do usunięcia (na przykład `https://localhost:5001/api/todo/1`).
* Wybierz pozycję **Wyślij**.

Przykładowa aplikacja umożliwia usunięcie wszystkich elementów. Jednak po usunięciu ostatniego elementu jest on tworzony przez konstruktora klasy modelu przy następnym wywołaniu interfejsu API.

## <a name="call-the-web-api-with-javascript"></a>Wywoływanie interfejsu API sieci Web przy użyciu języka JavaScript

W tej sekcji zostanie dodana strona HTML, która używa języka JavaScript do wywoływania internetowego interfejsu API. jQuery inicjuje żądanie. Język JavaScript aktualizuje stronę ze szczegółowymi informacjami z odpowiedzi internetowego interfejsu API.

Skonfiguruj aplikację do [obsługi plików statycznych](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [Włącz domyślne mapowanie plików](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) , aktualizując *Startup.cs* z następującym wyróżnionym kodem:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

Utwórz folder *wwwroot* w katalogu projektu.

Dodaj plik HTML o nazwie *index. html* do katalogu *wwwroot* . Zastąp jego zawartość następującym kodem:

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

Dodaj plik języka JavaScript o nazwie *site. js* do katalogu *wwwroot* . Zastąp jego zawartość następującym kodem:

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

Zmiana ustawień uruchamiania projektów ASP.NET Core może być konieczne test lokalnie za pomocą strony HTML:

* Otwórz *Properties\launchSettings.JSON*.
* Usuń właściwość `launchUrl`, aby wymusić, że aplikacja zostanie otwarta w pliku *index. html*&mdash;domyślnym plikiem projektu.

Ten przykład wywołuje wszystkie metody CRUD internetowego interfejsu API. Poniżej przedstawiono objaśnienia dotyczące wywołań interfejsu API.

### <a name="get-a-list-of-to-do-items"></a>Pobierz listę elementów do wykonania

jQuery wysyła żądanie HTTP GET do internetowego interfejsu API, który zwraca kod JSON reprezentujący tablicę elementów do wykonania. Funkcja wywołania zwrotnego `success` jest wywoływana, jeśli żądanie zakończy się pomyślnie. Podczas wywołania zwrotnego model DOM jest aktualizowana informacjami zadań do wykonania.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Dodaj element do wykonania

jQuery wysyła żądanie HTTP POST z elementem do wykonania w treści żądania. Opcje `accepts` i `contentType` są ustawione na `application/json`, aby określić typ nośnika, który odbiera i wysyła. Element do wykonania jest konwertowany na format JSON przy użyciu [formatu JSON. stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Gdy interfejs API zwraca kod stanu pomyślnego, funkcja `getData` jest wywoływana w celu zaktualizowania tabeli HTML.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Zaktualizuj element do wykonania

Aktualizowanie zadanie do wykonania jest podobne do dodawania jednego. `url` zmieni się, aby dodać unikatowy identyfikator elementu, a `type` jest `PUT`.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Usuń element do wykonania

Usuwanie elementu do wykonania jest realizowane przez ustawienie `type` w wywołaniu AJAX, aby `DELETE` i określić unikatowy identyfikator elementu w adresie URL.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a>Dodawanie obsługi uwierzytelniania do internetowego interfejsu API

[!INCLUDE[](~/includes/IdentityServer4.md)]

## <a name="additional-resources"></a>Dodatkowe zasoby

[Wyświetl lub Pobierz przykładowy kod dla tego samouczka](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples). Zobacz artykuł [jak pobrać](xref:index#how-to-download-a-sample).

Więcej informacji zawierają następujące zasoby:

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [Wersja tego samouczka usługi YouTube](https://www.youtube.com/watch?v=TTkhEyGBfAk)
