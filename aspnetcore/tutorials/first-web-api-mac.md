---
title: "Tworzenie składnika Web API platformy ASP.NET Core i programu Visual Studio dla komputerów Mac"
author: rick-anderson
description: "Tworzenie składnika Web API platformy ASP.NET Core MVC i programu Visual Studio dla komputerów Mac"
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: c7825530146e5e4a879bf44db5a92bc7700de73b
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>Tworzenie składnika Web API platformy ASP.NET Core MVC i programu Visual Studio dla komputerów Mac

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Wasson Jan](https://github.com/mikewasson)

W tym samouczku kompilacji interfejsu API sieci web do zarządzania listę elementów "do wykonania". Nie jest zbudowana w Interfejsie użytkownika.

Istnieją 3 wersje tego samouczka:

* System macOS: składnika Web API z programem Visual Studio dla komputerów Mac (w tym samouczku)
* System Windows: [sieci Web interfejsu API z programem Visual Studio dla systemu Windows](xref:tutorials/first-web-api)
* System macOS, Linux, Windows: [interfejsu API sieci Web z kodem Visual Studio](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* Zobacz [wprowadzenie do platformy ASP.NET MVC rdzeni na Mac lub Linux](xref:tutorials/first-mvc-app-xplat/index) przykład korzystającego z trwałe bazy danych.

## <a name="prerequisites"></a>Wymagania wstępne

Zainstaluj następujące czynności:

- [Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszy
- [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a>Utwórz projekt

W programie Visual Studio, wybierz **Plik > nowe rozwiązanie**.

![System macOS nowego rozwiązania](first-web-api-mac/_static/sln.png)

Wybierz **aplikacji .NET Core > interfejsu API sieci Web platformy ASP.NET Core > Dalej**.

![okno dialogowe macOS nowego projektu](first-web-api-mac/_static/1.png)

Wprowadź **TodoApi** dla **Nazwa projektu**, a następnie wybierz przycisk Utwórz.

![okno dialogowe konfiguracji](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>Uruchom aplikację

W programie Visual Studio, wybierz **Uruchom > Rozpocznij z debugowanie** do uruchomienia aplikacji. Program Visual Studio spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:5000`. Błąd HTTP 404 (nie znaleziono).  Zmień adres URL do `http://localhost:port/api/values`. `ValuesController` Zostaną wyświetlone dane:

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Dodaj obsługę Entity Framework Core

Zainstaluj [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) dostawcy bazy danych. Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z bazy danych w pamięci.

* Z **projektu** menu, wybierz opcję **Dodawanie pakietów NuGet**. 

  *  Alternatywnie możesz kliknąć prawym przyciskiem myszy **zależności**, a następnie wybierz **Dodawanie pakietów**.

* Wprowadź `EntityFrameworkCore.InMemory` w polu wyszukiwania.
* Wybierz `Microsoft.EntityFrameworkCore.InMemory`, a następnie wybierz **Dodaj pakiet**.

### <a name="add-a-model-class"></a>Dodaj klasę modelu

Model jest obiekt, który reprezentuje dane w aplikacji. W takim przypadku tylko model jest zadanie do wykonania.

Dodaj folder o nazwie *modele*. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt. Wybierz **dodać** > **nowy Folder**. Nazwa folderu *modele*.

![Nowy folder](first-web-api-mac/_static/folder.png)

Uwaga: Możesz też zaznaczyć klasy modelu dowolne miejsce w projekcie, ale *modele* folder jest używany przez Konwencję.

Dodaj `TodoItem` klasy. Kliknij prawym przyciskiem myszy *modele* i wybierz polecenie **Dodaj > Nowy plik > Ogólne > pustą klasę**. Nazwa klasy `TodoItem`, a następnie wybierz **nowy**.

Zamień z wygenerowanego kodu:

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

Generuje bazy danych `Id` podczas `TodoItem` jest tworzony.

### <a name="create-the-database-context"></a>Tworzenie kontekstu bazy danych

*Kontekst bazy danych* jest główna klasa, która koordynuje funkcji programu Entity Framework o dany model danych. Utworzyć tę klasę przez pochodny `Microsoft.EntityFrameworkCore.DbContext` klasy.

Dodaj `TodoContext` klasy do *modele* folderu.

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Dodawanie kontrolera

W Eksploratorze rozwiązań w *kontrolerów* folderu, Dodaj klasę `TodoController`.

Zamień na wygenerowany kod poniżej (i dodać klamrowe nawiasy zamykające):

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Uruchom aplikację

W programie Visual Studio, wybierz **Uruchom > Rozpocznij z debugowanie** do uruchomienia aplikacji. Program Visual Studio spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port`, gdzie *portu* jest liczbą losowo wybranego portu. Błąd HTTP 404 (nie znaleziono).  Zmień adres URL do `http://localhost:port/api/values`. `ValuesController` Zostaną wyświetlone dane:

```
["value1","value2"]
```

Przejdź do `Todo` kontroler na`http://localhost:port/api/todo`:

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>Implementowanie inne operacje CRUD

Dodamy `Create`, `Update`, i `Delete` metody kontrolera. Odmiany motywu, są tak właśnie będzie I wyświetlić kod i zaznacz główne różnice. Po dodaniu lub zmiana kodu, skompiluj projekt.

### <a name="create"></a>Create

[!code-csharp[](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Jest to metoda HTTP POST, wskazane przez [ `[HttpPost]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) atrybutu. [ `[FromBody]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) Atrybut informuje MVC, aby uzyskać wartość elementu zadań do wykonania z treści żądania HTTP.

`CreatedAtRoute` Metoda zwraca odpowiedź 201 standardowa odpowiedź metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze. `CreatedAtRoute` również dodaje do odpowiedzi nagłówek lokalizacji. Nagłówek lokalizacji Określa identyfikator URI elementu nowo utworzone zadanie do wykonania. Zobacz [10.2.2 201 utworzony](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Umożliwia wysłanie żądanie utworzenia Postman

* Uruchom aplikację (**Uruchom > Start z debugowaniem**).
* Uruchom Postman.

![Konsola postman](first-web-api/_static/pmc.png)

* Ustawia metodę HTTP `POST`
* Wybierz **treści** przycisku radiowego
* Wybierz **raw** przycisku radiowego
* Ustaw typ do ciągu JSON
* W edytorze klucz wartość wprowadzać pozycji zadania, takie jak

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* Wybierz **wysyłania**

* Wybierz kartę nagłówków w dolnym okienku i skopiuj **lokalizacji** nagłówka:

![Karta nagłówki konsoli Postman](first-web-api/_static/pmget.png)

Nagłówek lokalizacji URI umożliwia dostęp do utworzonego zasobu. Odwołaj `GetById` metody utworzony `"GetTodo"` o nazwie trasy:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a>Aktualizacja

[!code-csharp[](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update` przypomina `Create`, ale używa HTTP PUT. Odpowiedź jest [204 (bez zawartości)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Według specyfikacji HTTP żądania PUT wymaga klienta do wysyłania całego zaktualizowaną jednostkę, nie tylko różnice. Aby obsługiwać aktualizacje częściowe, użyj HTTP PATCH.

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Postman Konsola z wyświetlonymi 204 odpowiedzi (bez zawartości)](first-web-api/_static/pmcput.png)

### <a name="delete"></a>Usuwanie

[!code-csharp[](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

Odpowiedź jest [204 (bez zawartości)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Postman Konsola z wyświetlonymi 204 odpowiedzi (bez zawartości)](first-web-api/_static/pmd.png)

## <a name="next-steps"></a>Następne kroki

* [Routing do akcji kontrolera](xref:mvc/controllers/routing)
* Aby uzyskać informacje dotyczące wdrażania interfejsu API, zobacz [hosta i wdrażanie](xref:host-and-deploy/index).
* [Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))
* [Postman](https://www.getpostman.com/)
* [Fiddler](https://www.telerik.com/download/fiddler)
