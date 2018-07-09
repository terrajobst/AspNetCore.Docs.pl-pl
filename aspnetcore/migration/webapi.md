---
title: Migrowanie z interfejsu API sieci Web platformy ASP.NET do ASP.NET Core
author: ardalis
description: Dowiedz się, jak przeprowadzić migrację z implementacją interfejsu API sieci Web z interfejsu API sieci Web platformy ASP.NET do ASP.NET Core MVC.
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 8dd969c8644525606227989ca87e41fbfae5aed1
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894195"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Migrowanie z interfejsu API sieci Web platformy ASP.NET do ASP.NET Core

Przez [Steve Smith](https://ardalis.com/) i [Scott Addie](https://scottaddie.com)

Interfejsy API sieci Web są usług HTTP, docierających do szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych. Platforma ASP.NET Core MVC obejmuje obsługę tworzenia interfejsów API sieci Web, dzięki czemu powinny być zawsze w procesie tworzenia aplikacji sieci web. W tym artykule pokażemy, kroki wymagane do migracji z implementacją interfejsu API sieci Web z interfejsu API sieci Web platformy ASP.NET do ASP.NET Core MVC.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>Przegląd projektu Web API platformy ASP.NET

W tym artykule wykorzystano przykładowy projekt *ProductsApp*, utworzonego w artykule [wprowadzenie do wzorca ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) jako punktu początkowego. W tym projekcie prosty projekt ASP.NET Web API jest skonfigurowany w następujący sposób.

W *Global.asax.cs*, połączenie jest nawiązywane w przypadku `WebApiConfig.Register`:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` jest zdefiniowany w *App_Start*, i ma tylko jedną statyczną `Register` metody:

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]

Ta klasa umożliwia skonfigurowanie [trasowanie atrybutów](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), chociaż w rzeczywistości nie jest on używany w projekcie. Umożliwia również skonfigurowanie tabeli routingu, który jest używany przez interfejs API sieci Web platformy ASP.NET. W tym przypadku interfejsu API sieci Web platformy ASP.NET będzie oczekiwać adresy URL służące do być zgodny z formatem */api/ {controller} / {id}*, za pomocą *{id}* opcjonalne.

*ProductsApp* projekt zawiera tylko jeden kontroler prostą, która dziedziczy z `ApiController` i udostępnia dwie metody:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

Na koniec modelu *produktu*, który jest używany przez *ProductsApp*, jest prostą klasę:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

Teraz, gdy mamy już prostego projektu, który ma wyznaczać początek, firma Microsoft pokazują, jak przeprowadzić migrację tego projektu interfejsu API sieci Web do programu ASP.NET Core MVC.

## <a name="create-the-destination-project"></a>Utwórz projekt docelowy

W programie Visual Studio Utwórz nową, pustą rozwiązanie i nadaj mu nazwę *WebAPIMigration*. Dodaj istniejące *ProductsApp* projektu do niego, a następnie, Dodaj nowy projekt aplikacji sieci Web platformy ASP.NET Core z rozwiązaniem. Nazwa nowego projektu *ProductsCore*.

![Otwórz okno dialogowe Nowy projekt, aby szablonów sieci Web](webapi/_static/add-web-project.png)

Następnie wybierz szablon projektu interfejsu API sieci Web. Będziemy migrować *ProductsApp* zawartość do tego nowego projektu.

![Okno dialogowe Nowy aplikacji sieci Web za pomocą szablonu projektu składnika Web API zaznaczony na liście szablonów platformy ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

Usuń `Project_Readme.html` plik z nowym projektem. Rozwiązanie powinno teraz wyglądać następująco:

![Otwórz w Eksploratorze rozwiązań, wyświetlanie plików i folderów projektów ProductsApp i ProductsCore rozwiązanie aplikacji](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>Migracja konfiguracji

Platforma ASP.NET Core nie używa już *Global.asax*, *web.config*, lub *App_Start* folderów. Zamiast tego wszystkie zadania uruchamiania są wykonywane w *Startup.cs* w katalogu głównym projektu (zobacz [uruchamiania aplikacji](../fundamentals/startup.md)). W aplikacji ASP.NET Core MVC, routing oparty na atrybut jest teraz domyślnie uwzględniana podczas `UseMvc()` nosi nazwę; i, jest to zalecane podejście do konfigurowania tras interfejsu API sieci Web (a jak projekt startowy internetowy interfejs API obsługuje routing).

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

Przy założeniu, że chcesz użyć trasowanie atrybutów w projekcie idąc dalej, dodatkowa konfiguracja nie jest potrzebna. Wystarczy zastosować atrybutów zgodnie z potrzebami do kontrolerów i akcji, jak odbywa się w próbce `ValuesController` klasę, która znajduje się w projekcie starter interfejsu API sieci Web:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

Należy zwrócić uwagę na obecność *[controller]* w wierszu 8. Routing oparty na atrybut teraz obsługuje niektóre tokeny, takie jak *[controller]* i *[action]*. Tokeny te są zastępowane w czasie wykonywania nazwę kontroler lub akcję, odpowiednio, do którego zastosowano atrybut. Dzięki temu można zmniejszyć liczbę magic ciągów w projekcie i gwarantuje, że trasy zostaną zachowane synchronizowana z ich odpowiednich kontrolerów i akcji po zastosowaniu refaktoryzacji automatyczna zmiana nazwy.

Aby przeprowadzić migrację Kontroler interfejsu API produktów, możemy najpierw skopiować *ProductsController* do nowego projektu. Następnie po prostu Dołącz atrybut trasy na kontrolerze:

```csharp
[Route("api/[controller]")]
```

Musisz również dodać `[HttpGet]` atrybutu dwie metody, ponieważ oba powinna być wywoływana za pośrednictwem HTTP Get. Obejmują oczekuje parametru "id" w atrybucie dla `GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

W tym momencie routing jest prawidłowo skonfigurowany; Jednak firma Microsoft nie może jeszcze ją przetestować. Dodatkowe zmiany muszą zostać wprowadzone przed *ProductsController* zostanie skompilowany.

## <a name="migrate-models-and-controllers"></a>Migrowanie modeli i kontrolerów

Ostatnim krokiem w procesie migracji dla tego prostego projektu interfejsu API sieci Web jest do skopiowania kontrolerów i żadnych modeli, które używają. W takim przypadku po prostu skopiować *Controllers/ProductsController.cs* z oryginalnego projektu na nową. Następnie skopiuj cały folder modeli z oryginalnego projektu na nową. Dostosuj przestrzenie nazw, aby dopasować nową nazwę projektu (*ProductsCore*).  W tym momencie można skompilować aplikację i znajdzie się liczba błędów kompilacji. Te powinny ogólnie można podzielić na następujące kategorie:

* *Klasy ApiController* nie istnieje

* *System.Web.Http* przestrzeni nazw nie istnieje.

* *IHttpActionResult* nie istnieje

Na szczęście są wszystkie można łatwo rozwiązać:

* Zmiana *klasy ApiController* do *kontrolera* (może być konieczne dodanie *przy użyciu Microsoft.AspNetCore.Mvc*)

* Usuń wszystkie za pomocą instrukcji odnoszące się do *System.Web.Http*

* Zmień wszystkie metody, zwracając *IHttpActionResult* do zwrócenia *IActionResult*

Gdy te zmiany zostały dokonane i nieużywanych za pomocą instrukcji usunięciu zmigrowanych *ProductsController* klasy wygląda następująco:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

Teraz powinno być możliwe do uruchomienia projektu zmigrowane, a następnie przejdź do */api/produktów*; i przeczytaj pełną listę produktów 3. Przejdź do */api/products/1* powinien zostać wyświetlony pierwszy produkt.

## <a name="aspnet-4x-web-api-2-compatibility-shim"></a>Podkładki zgodności programu ASP.NET 4.x Web API 2

Jest przydatne narzędzie podczas migrowania ASP.NET Web API projektów ASP.NET Core [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) biblioteki. Podkładki zgodności rozszerza platformy ASP.NET Core, aby zezwolić na wiele różnych konwencji Web API 2 ma być używany. Przykładowe przenoszone wcześniej w tym dokumencie jest wystarczająco podstawowe, że podkładki zgodność nie jest konieczne. W przypadku większych projektów przy użyciu podkładki zgodności może być przydatne do tymczasowo unaoczni API między platformą ASP.NET Core i ASP.NET Web API 2.

Podkładki zgodności internetowy interfejs API jest przeznaczone do służyć jako tymczasowy środek do ułatwienia migrowania dużych projektów interfejsu API sieci Web platformy ASP.NET Core. Wraz z upływem czasu projektów powinny zostać uaktualnione do użycia wzorców platformy ASP.NET Core, zamiast polegania na poprawkę zgodności.

Zgodność funkcji dostępnych w `Microsoft.AspNetCore.Mvc.WebApiCompatShim` obejmują:

* Dodaje `ApiController` wpisz, aby kontrolerami typów podstawowych, nie muszą zostać zaktualizowane.
* Umożliwia powiązanie modelu w stylu interfejsu API sieci Web. Platforma ASP.NET Core MVC model funkcji wiązania, podobnie jak MVC 5, domyślnie. Zmiany podkładki zgodności modelu powiązania, które są bardziej podobne do Konwencji powiązanie modelu Web API 2. Na przykład typy złożone są automatycznie powiązany z treści żądania.
* Rozszerza wiązania modelu akcji kontrolera można korzystać z parametrami typu `HttpRequestMessage`.
* Dodaje elementy formatujące komunikaty umożliwiając akcji do zwrócenia wyników typu `HttpResponseMessage`.
* Dodaje metody dodatkowe odpowiedzi, które akcje Web API 2 może być używane do udostępniania odpowiedzi:
  * Generatory obiektu HttpResponseMessage:
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * Metody wynik akcji:
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* Dodaje wystąpienie `IContentNegotiator` do kontenera DI aplikacji i sprawia, że zawartość powiązane negocjacji typów z [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) dostępne. Obejmuje to typy, takie jak `DefaultContentNegotiator`, `MediaTypeFormatter`itp.

Aby użyć podkładek zgodności, należy:

* Zainstaluj [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) pakietu NuGet.
* Zarejestruj podkładki zgodności usług za pomocą kontenera DI aplikacji przez wywołanie metody `services.AddMvc().AddWebApiConventions()` aplikacji `Startup.ConfigureServices` metody.
* Definiowanie tras specyficzne dla interfejsu API sieci Web przy użyciu `MapWebApiRoute` na `IRouteBuilder` aplikacji `IApplicationBuilder.UseMvc` wywołania.

## <a name="summary"></a>Podsumowanie

Migrowanie prosty projekt interfejsu API sieci Web platformy ASP.NET do ASP.NET Core MVC jest dość prosta, Dziękujemy za wbudowaną obsługę interfejsów API sieci Web na platformie ASP.NET Core MVC. Każdy projekt interfejsu API sieci Web platformy ASP.NET, należy przeprowadzić migrację wybrane elementy główne są tras, kontrolerów i modeli, wraz z aktualizacjami typy używane przez kontrolerów i akcji.
