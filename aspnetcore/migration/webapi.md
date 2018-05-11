---
title: Migracja z interfejsu API sieci Web ASP.NET do platformy ASP.NET Core
author: ardalis
description: Dowiedz się, jak przeprowadzić migrację implementacji interfejsu API sieci Web z interfejsu API sieci Web platformy ASP.NET do platformy ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/webapi
ms.openlocfilehash: 8d842877e49e317323d453e71ebb3302245f388d
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Migracja z interfejsu API sieci Web ASP.NET do platformy ASP.NET Core

Przez [Steve Smith](https://ardalis.com/) i [Scott Addie](https://scottaddie.com)

Interfejsy API sieci Web są usług HTTP, które są używane przez szeroki wachlarz klientów, w tym przeglądarki i urządzenia przenośne. Podstawowe ASP.NET MVC obsługuje tworzenie interfejsów API sieci Web, dzięki czemu powinny być zawsze tworzenia aplikacji sieci web. W tym artykule przedstawiony kroki wymagane do migracji implementacji interfejsu API sieci Web z interfejsu API sieci Web platformy ASP.NET do platformy ASP.NET Core MVC.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>Projekt interfejsu API sieci Web ASP.NET przeglądu

W tym artykule wykorzystano przykładowy projekt *ProductsApp*, utworzonych w artykule [wprowadzenie do korzystania z programu ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) jako punktu początkowego. W tym projekcie prostego projektu interfejsu API sieci Web platformy ASP.NET jest skonfigurowane w następujący sposób.

W *Global.asax.cs*, połączenie jest nawiązywane w przypadku `WebApiConfig.Register`:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` jest zdefiniowany w *App_Start*, i ma tylko jedną statyczną `Register` metody:

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


Ta klasa konfiguruje [trasami atrybutów](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), mimo że faktycznie nie jest on używany w projekcie. Umożliwia również skonfigurowanie tabeli routingu, który jest używany przez interfejs API sieci Web ASP.NET. W takim przypadku interfejsu API sieci Web platformy ASP.NET będzie oczekiwać adresy URL zgodne z formatem */api/ {controller} / {id}*, z *{id}* opcjonalne.

*ProductsApp* projekt zawiera tylko jeden proste kontrolera, który dziedziczy `ApiController` i udostępnia dwie metody:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

Na koniec modelu *produktu*, używana przez *ProductsApp*, jest prostą klasę:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

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

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

Zakładając, że chcesz używać atrybutu routingu w projekcie w przyszłości, dodatkowa konfiguracja nie jest potrzebna. Po prostu stosowanie atrybutów w razie potrzeby do kontrolerów i akcji, co jest wykonywane w próbce `ValuesController` klasy, która znajduje się w projekcie starter interfejsu API sieci Web:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

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

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

Teraz powinno być możliwe do uruchomienia projektu zmigrowane, a następnie przejdź do *produkty/api/*; i będzie widoczna z pełną listą produktów 3. Przejdź do */api/products/1* i powinien zostać wyświetlony pierwszy produkt.

## <a name="microsoftaspnetcoremvcwebapicompatshim"></a>Microsoft.AspNetCore.Mvc.WebApiCompatShim

Jest przydatne narzędzie podczas migrowania składnika ASP.NET Web API projekty do platformy ASP.NET Core [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) biblioteki. Podkładki zgodności rozszerza platformy ASP.NET Core, aby umożliwić szereg różnych konwencji 2 interfejsu API sieci Web do użycia. Przykładowe przenoszone wcześniej w tym dokumencie jest wystarczająco podstawowe, że podkładki zgodności nie jest konieczne. Dla większych projektów przy użyciu podkładki zgodności może być przydatne mostkowania tymczasowo interfejsu API odstęp między platformy ASP.NET Core i ASP.NET Web API 2.

Oznacza, że podkładki zgodności interfejsu API sieci Web można użyć jako środek tymczasowy w celu ułatwienia migrowania dużych projektów interfejsu API sieci Web platformy ASP.NET Core. Wraz z upływem czasu projekty powinny zostać uaktualnione do korzystania z platformy ASP.NET Core wzorce zamiast polegania na podkładki zgodności. 

Funkcje zgodności objęte Microsoft.AspNetCore.Mvc.WebApiCompatShim:

* Dodaje `ApiController` wpisz, aby kontrolery typów podstawowych nie muszą zostać zaktualizowane.
* Umożliwia powiązanie modelu stylu interfejsu API sieci Web. ASP.NET MVC model powiązania funkcji podstawowych podobnie jak MVC 5, domyślnie. Zmiany podkładki zgodności modelu powiązania wyglądać mniej więcej konwencje powiązanie modelu 2 interfejsu API sieci Web. Na przykład typy złożone automatycznie są powiązane z treści żądania.
* Rozszerza wiązania modelu tak, aby kontroler akcji może zająć parametrów typu `HttpRequestMessage`.
* Dodaje elementy formatujące komunikaty stosowanie akcji do zwracania wyników typu `HttpResponseMessage`.
* Dodaje metody odpowiedzi dodatkowe, które akcje 2 interfejsu API sieci Web mógł zostać użyty do obsługi odpowiedzi:
    * Generatory HttpResponseMessage:
        * `CreateResponse<T>`
        * `CreateErrorResponse`
    * Metody wynik akcji:
        * `BadResuestErrorMessageResult`
        * `ExceptionResult`
        * `InternalServerErrorResult`
        * `InvalidModelStateResult`
        * `NegotiatedContentResult`
        * `ResponseMessageResult`
* Dodaje wystąpienie `IContentNegotiator` do aplikacji kontenera Podpisane i sprawia, że zawartość związanych z negocjacji typów z [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) dostępne. Dotyczy to również wykresach `DefaultContentNegotiator`, `MediaTypeFormatter`itp.

Aby użyć podkładek zgodności, musisz:

* Odwołanie [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) pakietu NuGet.
* Zarejestruj usługi podkładki zgodności aplikacji kontenera Podpisane przez wywołanie metody `services.AddWebApiConventions()` w aplikacji `Startup.ConfigureServices` metody.
* Definiowanie tras specyficzne dla interfejsu API sieci Web przy użyciu `MapWebApiRoute` na `IRouteBuilder` w aplikacji `IApplicationBuilder.UseMvc` wywołania.

## <a name="summary"></a>Podsumowanie

Migrowanie prostego projektu interfejsu API sieci Web platformy ASP.NET do platformy ASP.NET Core MVC jest bardzo prosta dzięki wbudowaną obsługę interfejsów API sieci Web na platformie ASP.NET Core MVC. Główne elementy, które mają być każdy projekt interfejsu API sieci Web platformy ASP.NET, aby przeprowadzić migrację są tras, kontrolerów i modeli, wraz z aktualizacji na typy używane przez kontrolerów i akcji.
