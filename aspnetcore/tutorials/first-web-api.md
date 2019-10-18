---
title: 'Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core'
author: rick-anderson
description: Dowiedz się, jak utworzyć internetowy interfejs API za pomocą ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: tutorials/first-web-api
ms.openlocfilehash: b4c88f5dc7853396448a2a6122f3652f92079e68
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2019
ms.locfileid: "72541842"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a>Samouczek: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core

Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i [Jan Wasson](https://github.com/mikewasson)

Ten samouczek uczy się podstaw tworzenia interfejsu API sieci Web za pomocą ASP.NET Core.

Z tego samouczka dowiesz się, jak wykonywać następujące czynności:

> [!div class="checklist"]
> * Utwórz projekt interfejsu API sieci Web.
> * Dodaj klasę modelu i kontekst bazy danych.
> * Tworzy szkielet kontrolera z metodami CRUD.
> * Skonfiguruj Routing, ścieżki URL i wartości zwracane.
> * Wywołaj interfejs API sieci Web za pomocą programu Poster.

Na końcu znajduje się internetowy interfejs API, który może zarządzać elementami do wykonania przechowywanymi w bazie danych.

## <a name="overview"></a>Omówienie

Ten samouczek tworzy internetowy interfejs API zawierający następujące punkty końcowe:

|Punkt końcowy                  |Opis            |Treść żądania|Treść odpowiedzi       |
|--------------------------|-----------------------|------------|--------------------|
|Pobierz/api/TodoItems        |Pobierz wszystkie elementy do wykonania    |Brak        |Tablica elementów do wykonania|
|Pobierz/api/TodoItems/{id}   |Pobieranie elementu według identyfikatora      |Brak        |Element do wykonania          |
|Opublikuj/api/TodoItems       |Dodaj nowy element         |Element do wykonania  |Element do wykonania          |
|Umieść/api/TodoItems/{id}   |Aktualizowanie istniejącego elementu|Element do wykonania  |Brak                |
|Usuń/api/TodoItems/{id}|Usuń element         |Brak        |Brak                |

Na poniższym diagramie przedstawiono projekt aplikacji.

![Klient jest reprezentowany przez pole po lewej stronie. Przesyła żądanie i odbiera odpowiedź z aplikacji, pole rysowane po prawej stronie. W polu aplikacja trzy pola reprezentują kontroler, model i warstwę dostępu do danych. Żądanie znajduje się w kontrolerze aplikacji, a operacje odczytu i zapisu są wykonywane między kontrolerem a warstwą dostępu do danych. Model jest serializowany i zwracany do klienta w odpowiedzi.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a>Tworzenie projektu sieci Web

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Z menu **plik** wybierz pozycję **Nowy** **projekt** > .
1. Wybierz szablon **aplikacja sieci Web ASP.NET Core** a następnie kliknij przycisk **dalej**.
1. Nazwij projekt *TodoApi* i kliknij pozycję **Utwórz**.
1. W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 3,0** . Wybierz szablon **interfejsu API** i kliknij przycisk **Utwórz**.

![Okno dialogowe programu VS New Project](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).
1. Zmień katalog (`cd`) do folderu, który będzie zawierać folder projektu.
1. Uruchom następujące polecenia:

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

1. Gdy zostanie wyświetlone okno dialogowe z pytaniem, czy chcesz dodać wymagane zasoby do projektu, wybierz opcję **tak**.

  Poprzednie polecenia:

  * Utwórz nowy projekt internetowego interfejsu API i otwórz go w Visual Studio Code.
  * Dodaj pakiety NuGet, które są wymagane w następnej sekcji.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

1. Wybierz pozycję **plik**  > **nowe rozwiązanie**.

    ![macOS nowe rozwiązanie](first-web-api-mac/_static/sln.png)

1. Wybierz pozycję **.NET Core**  > **App**  > **API**  > **dalej**.

    ![okno dialogowe nowego projektu macOS](first-web-api-mac/_static/1.png)
  
1. W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** wybierz pozycję **docelowa platforma** * *.NET Core 3,0*.
1. Wprowadź *TodoApi* jako **nazwę projektu** , a następnie wybierz pozycję **Utwórz**.

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

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Naciśnij <kbd>klawisze CTRL + F5</kbd> , aby uruchomić aplikację. Program Visual Studio uruchamia przeglądarkę i przechodzi do `https://localhost:<port>/WeatherForecast`, gdzie `<port>` to losowo wybierany numer portu.

Jeśli zostanie wyświetlone okno dialogowe z pytaniem, czy należy zaufać certyfikatowi IIS Express, wybierz pozycję **tak**. W wyświetlonym oknie dialogowym **ostrzeżenia o zabezpieczeniach** wybierz pozycję **tak**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Naciśnij <kbd>klawisze CTRL + F5</kbd> , aby uruchomić aplikację. W przeglądarce przejdź do następującego adresu URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

Wybierz pozycję **uruchom**  > **Rozpocznij debugowanie** , aby uruchomić aplikację. Visual Studio dla komputerów Mac uruchamia przeglądarkę i przechodzi do `https://localhost:<port>`, gdzie `<port>` jest losowo wybranym numerem portu. Zwracany jest błąd HTTP 404 (nie znaleziono). Dołącz `/WeatherForecast` do adresu URL (Zmień adres URL na `https://localhost:<port>/WeatherForecast`).

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

## <a name="add-a-model-class"></a>Dodaj klasę modelu

*Model* to zestaw klas, które reprezentują dane zarządzane przez aplikację. Model tej aplikacji jest pojedynczym `TodoItem` klasą.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt. Wybierz pozycję **dodaj**  > **Nowy folder**. Nazwij *modele*folderów.
1. Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** **klasę** > . Nadaj klasie nazwę *TodoItem* i wybierz pozycję **Dodaj**.
1. Zastąp kod szablonu następującym kodem:

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Dodaj folder o nazwie *models*.
1. Dodaj klasę `TodoItem` do folderu *models* o następującym kodzie:

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

1. Kliknij prawym przyciskiem myszy projekt. Wybierz pozycję **dodaj**  > **Nowy folder**. Nazwij *modele*folderów.

    ![Nowy folder](first-web-api-mac/_static/folder.png)

1. Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj**  > **nowy plik**  > **Ogólne**  > **pustej klasy**.
1. Nazwij klasę *TodoItem*, a następnie kliknij pozycję **New (nowy**).
1. Zastąp kod szablonu następującym kodem:

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

Właściwość `Id` działa jako unikatowy klucz w relacyjnej bazie danych.

Klasy modelu mogą przejść do dowolnego miejsca w projekcie, ale folder *modele* jest używany przez Konwencję.

## <a name="add-a-database-context"></a>Dodawanie kontekstu bazy danych

*Kontekst bazy danych* jest główną klasą, która koordynuje Entity Framework funkcji dla modelu danych. Ta klasa jest tworzona przez wyprowadzanie z klasy `Microsoft.EntityFrameworkCore.DbContext`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a>Dodaj Microsoft. EntityFrameworkCore. SqlServer

1. W menu **Narzędzia** wybierz pozycję **menedżer pakietów NuGet > Zarządzanie pakietami NuGet dla rozwiązania**.
1. Wybierz kartę **Przeglądaj** , a następnie w polu wyszukiwania wprowadź ciąg **Microsoft. EntityFrameworkCore. SqlServer** .
1. W lewym okienku wybierz pozycję **Microsoft. EntityFrameworkCore. SqlServer** .
1. Zaznacz pole wyboru **projekt** w prawym okienku, a następnie wybierz pozycję **Zainstaluj**.
1. Aby dodać `Microsoft.EntityFrameworkCore.InMemory` pakiet NuGet, użyj powyższych instrukcji.

![Menedżer pakietów NuGet](first-web-api/_static/vs3nuget.png)

## <a name="add-the-todocontext-database-context"></a>Dodawanie kontekstu bazy danych TodoContext

* Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** **klasę** > . Nadaj klasie nazwę *TodoContext* i kliknij przycisk **Dodaj**.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

* Dodaj klasę `TodoContext` do folderu *models* .

---

* Wprowadź następujący kod:

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a>Rejestrowanie kontekstu bazy danych

W ASP.NET Core usługi, takie jak kontekst bazy danych, muszą być zarejestrowane z kontenerem [iniekcji zależności (di)](xref:fundamentals/dependency-injection) . Kontener udostępnia usługę kontrolerom.

Zaktualizuj *Startup.cs* o następujący wyróżniony kod:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

Poprzedni kod:

* Usuwa nieużywane deklaracje `using`.
* Dodaje kontekst bazy danych do kontenera DI.
* Określa, że kontekst bazy danych będzie używać bazy danych w pamięci.

## <a name="scaffold-a-controller"></a>Tworzenie szkieletu kontrolera

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Kliknij prawym przyciskiem myszy folder *controllers* .
1. Wybierz pozycję **dodaj**  > **nowy element szkieletowy**.
1. Wybierz pozycję **kontroler interfejsu API z akcjami, używając Entity Framework**, a następnie wybierz pozycję **Dodaj**.
1. Na stronie **Dodawanie kontrolera interfejsu API z akcjami przy użyciu Entity Framework** dialogowego:
    * Wybierz pozycję **TodoItem (TodoApi. models)** w **klasie model**.
    * W **klasie kontekstu danych**wybierz pozycję **TodoContext (TodoApi. models)** .
    * Wybierz pozycję **Dodaj**.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

Uruchom następujące polecenia:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

Poprzednie polecenia:

* Dodaj pakiety NuGet wymagane do tworzenia szkieletów.
* Instaluje aparat tworzenia szkieletu (`dotnet-aspnet-codegenerator`).
* Szkieletuje `TodoItemsController`.

---

Wygenerowany kod:

* Definiuje klasę kontrolera interfejsu API bez metod.
* Zdobi klasę z atrybutem [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) . Ten atrybut wskazuje, że kontroler odpowiada na żądania interfejsu API sieci Web. Aby uzyskać informacje o określonych zachowaniach, które włącza atrybut, zobacz <xref:web-api/index>.
* Używa funkcji DI do iniekcji kontekstu bazy danych (`TodoContext`) do kontrolera. Kontekst bazy danych jest używany w każdej z metod [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) w kontrolerze.

## <a name="examine-the-posttodoitem-create-method"></a>Badanie metody PostTodoItem Create

Zastąp instrukcję return w `PostTodoItem`, aby użyć operatora [nameof](/dotnet/csharp/language-reference/operators/nameof) :

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

Poprzedni kod jest metodą POST protokołu HTTP, jak wskazano w atrybucie [[HTTPPOST]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) . Metoda pobiera wartość elementu do wykonania z treści żądania HTTP.

Metoda <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>:

* W razie powodzenia zwraca kod stanu HTTP 201. HTTP 201 to standardowa odpowiedź dla metody POST protokołu HTTP, która tworzy nowy zasób na serwerze.
* Dodaje nagłówek [lokalizacji](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) do odpowiedzi. Nagłówek `Location` określa [Identyfikator URI](https://developer.mozilla.org/docs/Glossary/URI) nowo utworzonego elementu do wykonania. Aby uzyskać więcej informacji, zobacz [10.2.2 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Odwołuje się do akcji `GetTodoItem`, aby utworzyć identyfikator URI nagłówka `Location`. Słowo C# kluczowe `nameof` jest używane w celu uniknięcia twardej kodowania nazwy akcji w wywołaniu `CreatedAtAction`.

### <a name="install-postman"></a>Zainstaluj program Poster

Ten samouczek używa programu do testowania interfejsu API sieci Web.

1. Zainstaluj program [Poster](https://www.getpostman.com/downloads/)
1. Uruchom aplikację internetową.
1. Uruchom wpis.
1. Wyłącz **weryfikację certyfikatu SSL**
1. Z**ustawień**  >  **plików** (karta**Ogólne** ), wyłącz **weryfikację certyfikatu SSL**.

    > [!WARNING]
    > Po przetestowaniu kontrolera ponownie Włącz weryfikację certyfikatu SSL.

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a>Test PostTodoItem za pomocą programu Poster

1. Utwórz nowe żądanie.
1. Ustaw metodę HTTP na `POST`.
1. Wybierz kartę **treść** .
1. Wybierz przycisk radiowy **RAW** .
1. Ustaw typ na **JSON (Application/JSON)** .
1. W treści żądania wprowadź kod JSON dla elementu do wykonania:

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

1. Wybierz pozycję **Wyślij**.

  ![Ogłoś przy użyciu żądania Create](first-web-api/_static/create.png)

### <a name="test-the-location-header-uri"></a>Testowanie identyfikatora URI nagłówka lokalizacji

1. Wybierz kartę **nagłówki** w okienku **odpowiedź** .
1. Skopiuj wartość nagłówka **lokalizacji** :

    ![Karta nagłówki w konsoli programu Poster](first-web-api/_static/create.png)

1. Ustaw metodę, aby uzyskać.
1. Wklej URI (na przykład `https://localhost:5001/api/TodoItems/1`).
1. Wybierz pozycję **Wyślij**.

## <a name="examine-the-get-methods"></a>Badanie metod GET

Te metody implementują dwa punkty końcowe GET:

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

1. Utwórz nowe żądanie.
1. Ustaw metodę HTTP, aby **uzyskać**.
1. Ustaw adres URL żądania na `https://localhost:<port>/api/TodoItems`. Na przykład `https://localhost:5001/api/TodoItems`.
1. Ustaw **dwa widoki okienka** w programie Poster.
1. Wybierz pozycję **Wyślij**.

Ta aplikacja używa bazy danych w pamięci. Jeśli aplikacja zostanie zatrzymana i uruchomiona, poprzednie żądanie GET nie zwróci żadnych danych. Jeśli nie zostaną zwrócone żadne dane, [Opublikuj](#post) dane w aplikacji.

## <a name="routing-and-url-paths"></a>Ścieżki routingu i adresów URL

Atrybut [[narzędzia HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) oznacza metodę, która reaguje na żądanie HTTP GET. Ścieżka adresu URL dla każdej metody jest zbudowana w następujący sposób:

* Rozpocznij od ciągu szablonu w atrybucie `Route` kontrolera:

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* Zastąp `[controller]` nazwą kontrolera, którą Konwencją jest nazwa klasy kontrolera minus sufiks "Controller". Dla tego przykładu nazwa klasy kontrolera to **TodoItems**Controller, więc nazwa kontrolera to "TodoItems". W ASP.NET Core [routingu](xref:mvc/controllers/routing) jest rozróżniana wielkość liter.
* Jeśli atrybut `[HttpGet]` ma szablon trasy (na przykład `[HttpGet("products")]`), dołącz go do ścieżki. Ten przykład nie używa szablonu. Aby uzyskać więcej informacji, zobacz temat [Routing atrybutów z atrybutami http [Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

W poniższej metodzie `GetTodoItem` `"{id}"` jest zmienną zastępczą dla unikatowego identyfikatora elementu do wykonania. Po wywołaniu `GetTodoItem` wartość `"{id}"` w adresie URL jest podawana do metody w `id` parametr.

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a>Zwracane wartości

Typem zwracanym `GetTodoItems` i `GetTodoItem` Metoda jest [ActionResult \<T > typ](xref:web-api/action-return-types#actionresultt-type). ASP.NET Core automatycznie serializować obiektu do [formatu JSON](https://www.json.org/) i zapisuje kod JSON w treści komunikatu odpowiedzi. Kod odpowiedzi dla tego typu zwracanego to 200, przy założeniu, że nie istnieją Nieobsłużone wyjątki. Nieobsłużone wyjątki są tłumaczone na błędy 5xx.

`ActionResult` zwracane typy mogą reprezentować szeroką gamę kodów stanu HTTP. Na przykład `GetTodoItem` mogą zwracać dwie różne wartości stanu:

* Jeśli żaden element nie jest zgodny z żądanym IDENTYFIKATORem, metoda zwróci kod błędu 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound>.
* W przeciwnym razie metoda zwraca 200 z treścią odpowiedzi JSON. Zwracanie `item` wyników w odpowiedzi HTTP 200.

## <a name="the-puttodoitem-method"></a>Metoda PutTodoItem

Przeanalizuj metodę `PutTodoItem`:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

`PutTodoItem` jest podobna do `PostTodoItem`, z tą różnicą, że używa protokołu HTTP PUT. Odpowiedź to [204 (brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Zgodnie ze specyfikacją protokołu HTTP żądanie PUT wymaga, aby klient wysłał całą zaktualizowaną jednostkę, a nie tylko te zmiany. Aby zapewnić obsługę częściowych aktualizacji, użyj [poprawki http](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).

Jeśli wystąpi błąd podczas wywoływania `PutTodoItem`, wywołaj `GET`, aby upewnić się, że w bazie danych znajduje się element.

### <a name="test-the-puttodoitem-method"></a>Testowanie metody PutTodoItem

Ten przykład korzysta z bazy danych w pamięci, która musi zostać zainicjowana za każdym razem, gdy aplikacja zostanie uruchomiona. Przed wykonaniem wywołania PUT musi istnieć element w bazie danych. Wywołaj polecenie GET, aby upewnić się, że w bazie danych znajduje się element, przed wykonaniem wywołania PUT.

Zaktualizuj element do wykonania o IDENTYFIKATORze 1. Ustaw jej nazwę na "Źródło danych":

```json
{
  "ID":1,
  "name":"feed fish",
  "isComplete":true
}
```

Na poniższej ilustracji przedstawiono aktualizację programu Poster:

![Konsola programu Poster pokazująca odpowiedź 204 (brak zawartości)](first-web-api/_static/pmcput.png)

## <a name="the-deletetodoitem-method"></a>Metoda DeleteTodoItem

Przeanalizuj metodę `DeleteTodoItem`:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

Odpowiedź `DeleteTodoItem` to [204 (brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

### <a name="test-the-deletetodoitem-method"></a>Testowanie metody DeleteTodoItem

Użyj programu Poster, aby usunąć element do wykonania:

1. Ustaw metodę na `DELETE`.
1. Ustaw identyfikator URI obiektu do usunięcia (na przykład `https://localhost:5001/api/TodoItems/1`).
1. Wybierz pozycję **Wyślij**.

## <a name="call-the-web-api-with-javascript"></a>Wywoływanie interfejsu API sieci Web przy użyciu języka JavaScript

Zobacz [Samouczek: wywoływanie interfejsu API sieci web ASP.NET Core przy użyciu języka JavaScript](xref:tutorials/web-api-javascript).

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a>Dodawanie obsługi uwierzytelniania do internetowego interfejsu API

Zobacz samouczek [usługi identityserver4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) .

## <a name="additional-resources"></a>Dodatkowe zasoby

[Wyświetl lub Pobierz przykładowy kod dla tego samouczka](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples). Zobacz artykuł [jak pobrać](xref:index#how-to-download-a-sample).

Aby uzyskać więcej informacji, zobacz następujące zasoby:

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [Wersja tego samouczka usługi YouTube](https://www.youtube.com/watch?v=TTkhEyGBfAk)
