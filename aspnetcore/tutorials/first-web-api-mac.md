---
title: Tworzenie internetowego interfejsu API platformy ASP.NET Core i programu Visual Studio dla komputerów Mac
author: rick-anderson
description: Tworzenie internetowego interfejsu API platformy ASP.NET Core MVC i programu Visual Studio dla komputerów Mac
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 4caa6d9057de8d0e821c4abefe22985f43ff95ad
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38156143"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a>Tworzenie internetowego interfejsu API platformy ASP.NET Core i programu Visual Studio dla komputerów Mac

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Mike Wasson](https://github.com/mikewasson)

W tym samouczku Tworzenie internetowego interfejsu API do zarządzania listę elementów "do wykonania". Interfejs użytkownika nie jest konstruowany.

Istnieją trzy wersje tego samouczka:

* System macOS: Web API za pomocą programu Visual Studio dla komputerów Mac (w tym samouczku)
* Windows: [internetowego interfejsu API z programem Visual Studio for Windows](xref:tutorials/first-web-api)
* macOS i Linux, Windows: [interfejsu API sieci Web za pomocą programu Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

Zobacz [wprowadzenie do ASP.NET Core MVC w systemie macOS lub Linux](xref:tutorials/first-mvc-app-xplat/index) na przykład, który korzysta z trwałego bazy danych.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a>Utwórz projekt

W programie Visual Studio, wybierz **pliku** > **nowe rozwiązanie**.

![Nowe rozwiązanie w systemie macOS](first-web-api-mac/_static/sln.png)

Wybierz **aplikacji programu .NET Core** > **interfejsu API sieci Web platformy ASP.NET Core** > **dalej**.

![okno dialogowe z systemem macOS nowego projektu](first-web-api-mac/_static/1.png)

Wprowadź *TodoApi* dla **Nazwa projektu**, a następnie kliknij przycisk **Utwórz**.

![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>Uruchom aplikację

W programie Visual Studio, wybierz **Uruchom** > **Rozpocznij z debugowanie** do uruchomienia aplikacji. Program Visual Studio otworzy w przeglądarce i przechodzi do `http://localhost:5000`. Wystąpi błąd HTTP 404 (nie znaleziono). Zmień adres URL do `http://localhost:<port>/api/values`. `ValuesController` Dane są wyświetlane:

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Dodaj obsługę platformy Entity Framework Core

Zainstaluj [Entity Framework Core InMemory](/ef/core/providers/in-memory/) dostawcy bazy danych. Ten dostawca bazy danych umożliwia platformy Entity Framework Core ma być używany z bazą danych w pamięci.

* Z **projektu** menu, wybierz opcję **Dodaj pakiety NuGet**.

  * Alternatywnie możesz kliknąć prawym przyciskiem myszy **zależności**, a następnie wybierz pozycję **Dodawanie pakietów**.

* Wprowadź `EntityFrameworkCore.InMemory` w polu wyszukiwania.
* Wybierz `Microsoft.EntityFrameworkCore.InMemory`, a następnie wybierz pozycję **Dodaj pakiet**.

### <a name="add-a-model-class"></a>Dodawanie klasy modelu

Model jest obiekt reprezentujący dane w aplikacji. W takim przypadku tylko model jest element do wykonania.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt. Wybierz **Dodaj** > **nowy Folder**. Nazwa folderu *modeli*.

![Nowy folder](first-web-api-mac/_static/folder.png)

> [!NOTE]
> Klasy modeli można umieścić dowolne miejsce w projekcie, ale *modeli* folder jest używany przez Konwencję.

Kliknij prawym przyciskiem myszy *modeli* folder, a następnie wybierz **Dodaj** > **nowy plik** > **ogólne**  >  **Pusta klasa**. Nazwa klasy *TodoItem*, a następnie kliknij przycisk **New**.

Zastąp wygenerowany kod za pomocą:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Baza danych generuje `Id` podczas `TodoItem` zostanie utworzony.

### <a name="create-the-database-context"></a>Utwórz kontekst bazy danych

*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework. Tworzenie tej klasy, wynikające z `Microsoft.EntityFrameworkCore.DbContext` klasy.

Dodaj `TodoContext` klasy *modeli* folderu.

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Dodawanie kontrolera

W Eksploratorze rozwiązań w *kontrolerów* folderu, dodać klasę `TodoController`.

Zastąp wygenerowany kod następujących czynności:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Uruchom aplikację

W programie Visual Studio, wybierz **Uruchom** > **Rozpocznij z debugowanie** do uruchomienia aplikacji. Program Visual Studio otworzy w przeglądarce i przechodzi do `http://localhost:<port>`, gdzie `<port>` jest numer portu wybranego losowo. Wystąpi błąd HTTP 404 (nie znaleziono). Zmień adres URL do `http://localhost:<port>/api/values`. `ValuesController` Dane są wyświetlane:

```json
["value1","value2"]
```

Przejdź do `Todo` kontroler na `http://localhost:<port>/api/todo`. Zwracane są następujące dane JSON:

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>Implementowanie inne operacje CRUD

Dodamy `Create`, `Update`, i `Delete` metody kontrolera. Te metody są odmiany motywu, więc po prostu będzie I wyświetlić kod i wyróżnić najważniejsze różnice. Po dodaniu lub zmodyfikowaniu kodu, skompiluj projekt.

### <a name="create"></a>Create

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Powyższa metoda reaguje na metodę POST protokołu HTTP, wskazane przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu. [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) atrybut informuje MVC, aby uzyskać wartość elementu do wykonania z treści żądania HTTP.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Powyższa metoda reaguje na metodę POST protokołu HTTP, wskazane przez [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu. MVC pobiera wartość elementu do wykonania z treści żądania HTTP.
::: moniker-end

`CreatedAtRoute` Metoda zwraca odpowiedź 201. Jest to standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze. `CreatedAtRoute` również dodaje do odpowiedzi nagłówek lokalizacji. Nagłówek Location określa identyfikator URI nowo utworzonego zadania do wykonania. Zobacz [10.2.2 201 utworzone](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Wyślij żądanie utworzenia przy użyciu narzędzia Postman

* Uruchom aplikację (**Uruchom** > **rozpocząć debugowanie**).
* Otwórz narzędzie Postman.

![Konsola narzędzia postman](first-web-api/_static/pmc.png)

* Zaktualizuj numer portu w adresie URL localhost.
* Ustawia metodę HTTP *WPIS*.
* Kliknij przycisk **treści** kartę.
* Wybierz **pierwotne** przycisku radiowego.
* Ustaw typ *JSON (application/json)*.
* Wprowadź treść żądania z elementem zadań do wykonania przypominającą następujące dane JSON:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Kliknij przycisk **wysyłania** przycisku.

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> Jeśli odpowiedź nie jest wyświetlany po kliknięciu przycisku **wysyłania**, wyłącz **weryfikacji certyfikacji SSL** opcji. To znajduje się w folderze **pliku** > **ustawienia**. Kliknij przycisk **wysyłania** przycisk ponownie po wyłączeniu ustawienia.
::: moniker-end

Kliknij przycisk **nagłówki** karcie **odpowiedzi** okienka i skopiuj **lokalizacji** wartość nagłówka:

![Karta nagłówki konsoli narzędzia Postman](first-web-api/_static/pmc2.png)

Dostęp do zasobu, który został utworzony, można użyć w nagłówku Location identyfikator URI. `Create` Metoda zwraca [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_). Pierwszy parametr przekazany do `CreatedAtRoute` reprezentuje trasą mającą nazwę do użycia podczas generowania adresu URL. Pamiętamy `GetById` metoda utworzone `"GetTodo"` o nazwie trasy:

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

`Update` jest podobny do `Create`, ale używa HTTP PUT. Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Według specyfikacji protokołu HTTP żądanie PUT wymaga to klientowi wysłanie całego zaktualizowaną jednostkę, nie tylko różnice. Aby obsługiwać aktualizacje częściowe, należy użyć HTTP PATCH.

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](first-web-api/_static/pmcput.png)

### <a name="delete"></a>Usuwanie

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

Odpowiedź jest [204 (Brak zawartości)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Konsola postman z wyświetlonymi 204 (Brak zawartości) odpowiedzi](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
