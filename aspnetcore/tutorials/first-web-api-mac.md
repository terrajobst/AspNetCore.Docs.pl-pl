---
title: Tworzenie składnika Web API platformy ASP.NET Core i programu Visual Studio dla komputerów Mac
author: rick-anderson
description: Tworzenie składnika Web API platformy ASP.NET Core MVC i programu Visual Studio dla komputerów Mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 699fbbf54abf1dc5c4156c559761110cdb375558
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
ms.locfileid: "33923207"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a>Tworzenie składnika Web API platformy ASP.NET Core i programu Visual Studio dla komputerów Mac

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Wasson Jan](https://github.com/mikewasson)

W tym samouczku kompilacji interfejsu API sieci web do zarządzania listę elementów "do wykonania". Nie jest zbudowana w Interfejsie użytkownika.

Istnieją trzy wersje tego samouczka:

* System macOS: składnika Web API z programem Visual Studio dla komputerów Mac (w tym samouczku)
* System Windows: [sieci Web interfejsu API z programem Visual Studio dla systemu Windows](xref:tutorials/first-web-api)
* System macOS, Linux, Windows: [interfejsu API sieci Web z kodem Visual Studio](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

Zobacz [wprowadzenie do platformy ASP.NET MVC rdzeni na macOS lub Linux](xref:tutorials/first-mvc-app-xplat/index) przykład korzystającego z trwałe bazy danych.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a>Utwórz projekt

W programie Visual Studio, wybierz **pliku** > **nowe rozwiązanie**.

![System macOS nowego rozwiązania](first-web-api-mac/_static/sln.png)

Wybierz **aplikacji .NET Core** > **interfejsu API sieci Web platformy ASP.NET Core** > **dalej**.

![okno dialogowe macOS nowego projektu](first-web-api-mac/_static/1.png)

Wprowadź *TodoApi* dla **Nazwa projektu**, a następnie kliknij przycisk **Utwórz**.

![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>Uruchom aplikację

W programie Visual Studio, wybierz **Uruchom** > **Rozpocznij z debugowanie** do uruchomienia aplikacji. Program Visual Studio spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:5000`. Błąd HTTP 404 (nie znaleziono). Zmień adres URL do `http://localhost:<port>/api/values`. `ValuesController` Dane są wyświetlane:

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Dodaj obsługę Entity Framework Core

Zainstaluj [Entity Framework Core InMemory](/ef/core/providers/in-memory/) dostawcy bazy danych. Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z bazy danych w pamięci.

* Z **projektu** menu, wybierz opcję **Dodawanie pakietów NuGet**.

  * Alternatywnie możesz kliknąć prawym przyciskiem myszy **zależności**, a następnie wybierz **Dodawanie pakietów**.

* Wprowadź `EntityFrameworkCore.InMemory` w polu wyszukiwania.
* Wybierz `Microsoft.EntityFrameworkCore.InMemory`, a następnie wybierz **Dodaj pakiet**.

### <a name="add-a-model-class"></a>Dodaj klasę modelu

Model jest obiekt reprezentujący dane w aplikacji. W takim przypadku tylko model jest zadanie do wykonania.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt. Wybierz **dodać** > **nowy Folder**. Nazwa folderu *modele*.

![Nowy folder](first-web-api-mac/_static/folder.png)

> [!NOTE]
> Klasy modeli można umieścić dowolne miejsce w projekcie, ale *modele* folder jest używany przez Konwencję.

Kliknij prawym przyciskiem myszy *modele* folder, a następnie wybierz **Dodaj** > **nowy plik** > **ogólne**  >  **Pusta klasa**. Nazwa klasy *TodoItem*, a następnie kliknij przycisk **nowy**.

Zamień z wygenerowanego kodu:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Generuje bazy danych `Id` podczas `TodoItem` jest tworzony.

### <a name="create-the-database-context"></a>Tworzenie kontekstu bazy danych

*Kontekst bazy danych* jest główna klasa, która koordynuje funkcji programu Entity Framework o dany model danych. Utworzyć tę klasę przez pochodny `Microsoft.EntityFrameworkCore.DbContext` klasy.

Dodaj `TodoContext` klasy do *modele* folderu.

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Dodawanie kontrolera

W Eksploratorze rozwiązań w *kontrolerów* folderu, Dodaj klasę `TodoController`.

Zastąp wygenerowany kod poniżej:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Uruchom aplikację

W programie Visual Studio, wybierz **Uruchom** > **Rozpocznij z debugowanie** do uruchomienia aplikacji. Program Visual Studio spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:<port>`, gdzie `<port>` jest liczbą losowo wybranego portu. Błąd HTTP 404 (nie znaleziono). Zmień adres URL do `http://localhost:<port>/api/values`. `ValuesController` Dane są wyświetlane:

```json
["value1","value2"]
```

Przejdź do `Todo` kontroler na `http://localhost:<port>/api/todo`. Zwrócono następujący JSON:

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>Implementowanie inne operacje CRUD

Dodamy `Create`, `Update`, i `Delete` metody kontrolera. Te metody są odmiany motywu, więc będzie tylko wyświetlić kod i zaznacz główne różnice. Po dodaniu lub zmiana kodu, skompiluj projekt.

### <a name="create"></a>Create

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Powyższa metoda odpowiada HTTP POST wskazywany przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu. [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) atrybut informuje MVC, aby uzyskać wartość elementu zadań do wykonania z treści żądania HTTP.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Powyższa metoda odpowiada HTTP POST wskazywany przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu. MVC pobiera wartość elementu zadań do wykonania z treści żądania HTTP.
::: moniker-end

`CreatedAtRoute` Metoda zwraca odpowiedź 201. Jest standardowe odpowiedzi dla metody POST protokołu HTTP, która tworzy nowy zasób na serwerze. `CreatedAtRoute` również dodaje do odpowiedzi nagłówek lokalizacji. Nagłówek lokalizacji Określa identyfikator URI elementu nowo utworzone zadanie do wykonania. Zobacz [10.2.2 201 utworzony](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Umożliwia wysłanie żądanie utworzenia Postman

* Uruchom aplikację (**Uruchom** > **rozpoczynać debugowania**).
* Otwórz Postman.

![Konsola postman](first-web-api/_static/pmc.png)

* Zaktualizuj numer portu w adresie URL localhost.
* Ustawia metodę HTTP *POST*.
* Kliknij przycisk **treści** kartę.
* Wybierz **raw** przycisk radiowy.
* Ustaw typ *JSON (application/json)*.
* Wprowadź treść żądania z zadanie do wykonania podobne do następujących JSON:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Kliknij przycisk **wysyłania** przycisku.

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> Jeśli odpowiedź nie są wyświetlane po kliknięciu przycisku **wysyłania**, wyłącz **weryfikacji certyfikacji SSL** opcji. To znajduje się w **pliku** > **ustawienia**. Kliknij przycisk **wysyłania** przycisk ponownie po wyłączeniu ustawienia.
::: moniker-end

Kliknij przycisk **nagłówki** karcie **odpowiedzi** okienko i skopiuj **lokalizacji** wartość nagłówka:

![Karta nagłówki konsoli Postman](first-web-api/_static/pmc2.png)

Nagłówek lokalizacji URI umożliwia dostęp do zasobu, który został utworzony. `Create` Metoda zwraca [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_). Pierwszy parametr przekazany do `CreatedAtRoute` reprezentuje nazwanej trasy do użycia podczas generowania adresu URL. Odwołania, który `GetById` metody utworzony `"GetTodo"` o nazwie trasy:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a>Aktualizacja

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

`Update` przypomina `Create`, ale używa HTTP PUT. Odpowiedź jest [204 (bez zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Według specyfikacji HTTP żądania PUT wymaga klienta do wysyłania całego zaktualizowaną jednostkę, nie tylko różnice. Aby obsługiwać aktualizacje częściowe, użyj HTTP PATCH.

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Postman Konsola z wyświetlonymi 204 odpowiedzi (bez zawartości)](first-web-api/_static/pmcput.png)

### <a name="delete"></a>Usuwanie

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

Odpowiedź jest [204 (bez zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Postman Konsola z wyświetlonymi 204 odpowiedzi (bez zawartości)](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
