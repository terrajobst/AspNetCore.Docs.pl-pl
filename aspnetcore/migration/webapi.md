---
title: Migrowanie z interfejsu API sieci Web ASP.NET
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/webapi
ms.openlocfilehash: aef00b63c9889100116facc610bec99f889e4c46
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-from-aspnet-web-api"></a>Migrowanie z interfejsu API sieci Web ASP.NET

Przez [Steve Smith](https://ardalis.com/) i [Scott Addie](https://scottaddie.com)

Interfejsy API sieci Web są usług HTTP, które są używane przez szeroki wachlarz klientów, w tym przeglądarki i urządzenia przenośne. Podstawowe ASP.NET MVC obsługuje tworzenie interfejsów API sieci Web, dzięki czemu powinny być zawsze tworzenia aplikacji sieci web. W tym artykule przedstawiony kroki wymagane do migracji implementacji interfejsu API sieci Web z interfejsu API sieci Web platformy ASP.NET do platformy ASP.NET Core MVC.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>Projekt interfejsu API sieci Web ASP.NET przeglądu

W tym artykule wykorzystano przykładowy projekt *ProductsApp*, utworzonych w artykule [wprowadzenie do składnika ASP.NET Web API](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) jako punktu początkowego. W tym projekcie prostego projektu interfejsu API sieci Web platformy ASP.NET jest skonfigurowane w następujący sposób.

W *Global.asax.cs*, połączenie jest nawiązywane w przypadku `WebApiConfig.Register`:

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig`jest zdefiniowany w *App_Start*, i ma tylko jedną statyczną `Register` metody:

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


Ta klasa konfiguruje [trasami atrybutów](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), mimo że faktycznie nie jest on używany w projekcie. Konfiguruje również tabeli routingu, który jest używany przez interfejs API sieci Web ASP.NET. W takim przypadku interfejsu API sieci Web platformy ASP.NET będzie oczekiwać adresy URL zgodne z formatem */api/ {controller} / {id}*, z *{id}* opcjonalne.

*ProductsApp* projekt zawiera tylko jeden proste kontrolera, który dziedziczy `ApiController` i udostępnia dwie metody:

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

Na koniec modelu *produktu*, używana przez *ProductsApp*, jest prostą klasę:

[!code-csharp[Main](webapi/sample/ProductsApp/Models/Product.cs)]

Teraz, gdy mamy prostego projektu, z którego chcesz uruchomić, firma Microsoft pokazują, jak przeprowadzić migrację tego projektu interfejsu API sieci Web do platformy ASP.NET Core MVC.

## <a name="create-the-destination-project"></a>Tworzenie projektu docelowego

Za pomocą programu Visual Studio, Utwórz nowy, pusty rozwiązanie i nadaj mu nazwę *WebAPIMigration*. Dodaj istniejące *ProductsApp* projektu do niego, a następnie, Dodaj nowy projekt aplikacji sieci Web platformy ASP.NET Core do rozwiązania. Nazwa nowego projektu *ProductsCore*.

![Otwórz okno dialogowe nowego projektu do szablonów sieci Web](webapi/_static/add-web-project.png)

Następnie wybierz szablon projektu interfejsu API sieci Web. Będziemy migrować *ProductsApp* zawartość do nowego projektu.

![Okno dialogowe nowego aplikacji sieci Web z wybranych z listy szablonów platformy ASP.NET Core szablonu projektu interfejsu API sieci Web](webapi/_static/aspnet-5-webapi.png)

Usuń `Project_Readme.html` pliku z nowego projektu. Rozwiązania powinna wyglądać następująco:

![Otwórz w Eksploratorze rozwiązań przedstawiający plików i folderów projektów ProductsApp i ProductsCore rozwiązanie aplikacji](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>Migruj konfigurację

Już korzysta z platformy ASP.NET Core *Global.asax*, *web.config*, lub *App_Start* folderów. Zamiast tego, wszystkie zadania uruchamiania są wykonywane w *Startup.cs* w katalogu głównym projektu (zobacz [uruchamiania aplikacji](../fundamentals/startup.md)). W programie ASP.NET MVC Core, opartych na atrybutach routingu jest teraz zawarta domyślnie podczas `UseMvc()` nosi nazwę; i jest to zalecane podejście do konfigurowania tras interfejsu API sieci Web (, jak projekt starter interfejsu API sieci Web obsługuje routing).

[!code-none[Main](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]

Zakładając, że chcesz używać atrybutu routingu w projekcie w przyszłości, dodatkowa konfiguracja nie jest potrzebna. Po prostu stosowanie atrybutów w razie potrzeby do kontrolerów i akcji, co jest wykonywane w próbce `ValuesController` klasy, która znajduje się w projekcie starter interfejsu API sieci Web:

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

Należy zwrócić uwagę na obecność *[kontrolera]* w wierszu 8. Na podstawie atrybutów routingu teraz obsługuje niektórych tokenów, takich jak *[kontrolera]* i *[action]*. Tokeny te są zamieniane w czasie wykonywania o nazwie kontroler lub akcję, odpowiednio, do którego zastosowano atrybut. Dzięki temu można zmniejszyć liczbę magic ciągów w projekcie i gwarantuje, że trasy zostaną zachowane synchronizowana z ich odpowiednich kontrolerów i akcji po zastosowaniu refaktoryzacje automatycznej zmiany nazwy.

Aby przeprowadzić migrację Kontroler interfejsu API produktów, możemy należy najpierw skopiować *ProductsController* do nowego projektu. Następnie wystarczy dołączyć atrybut trasy na kontrolerze:

```csharp
[Route("api/[controller]")]
```

Należy również dodać `[HttpGet]` atrybutu dwie metody, ponieważ obie powinna być wywoływana za pośrednictwem HTTP Get. Obejmują oczekuje parametru "id" w atrybucie dla `GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

W tym momencie routingu jest poprawnie skonfigurowana; Jednak firma Microsoft nie może jeszcze przetestować. Dodatkowe zmiany, musi być dokonana przed *ProductsController* zostanie skompilowany.

## <a name="migrate-models-and-controllers"></a>Migrowanie modeli i kontrolerów

Ostatni etap procesu migracji dla tego prostego projektu interfejsu API sieci Web jest kopiować kontrolery i żadnych modeli korzystają. W takim przypadku wystarczy skopiować *Controllers/ProductsController.cs* z oryginalnego projektu do nowego. Następnie skopiuj cały folder modeli z oryginalnego projektu do nowego. Dostosuj przestrzenie nazw, aby odpowiadała nowej nazwie projektu (*ProductsCore*).  W tym momencie można skompilować aplikację i dostępne są różne błędy kompilacji. Te zazwyczaj powinno należeć do następujących kategorii:

* *Klasy ApiController* nie istnieje

* *System.Web.Http* przestrzeni nazw nie istnieje.

* *IHttpActionResult* nie istnieje

Na szczęście są bardzo łatwo można poprawić:

* Zmień *klasy ApiController* do *kontrolera* (może być konieczne dodanie *przy użyciu Microsoft.AspNetCore.Mvc*)

* Usuń wszystkie za pomocą instrukcji odwołujących się do *System.Web.Http*

* Zmień wszystkie zwracanie — metoda *IHttpActionResult* do zwrócenia *IActionResult*

Po te zmiany zostały dokonane i nieużywanych instrukcje using usunięty, zmigrowane *ProductsController* klasy wygląda następująco:

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

Teraz powinno być możliwe do uruchomienia projektu zmigrowane, a następnie przejdź do *produkty/api/*; i będzie widoczna z pełną listą produktów 3. Przejdź do */api/products/1* i powinien zostać wyświetlony pierwszy produkt.

## <a name="summary"></a>Podsumowanie

Migrowanie prostego projektu interfejsu API sieci Web platformy ASP.NET do platformy ASP.NET Core MVC jest bardzo prosta dzięki wbudowaną obsługę interfejsów API sieci Web na platformie ASP.NET Core MVC. Główne elementy, które mają być każdy projekt interfejsu API sieci Web platformy ASP.NET, aby przeprowadzić migrację są tras, kontrolerów i modeli, wraz z aktualizacji na typy używane przez kontrolerów i akcji.
