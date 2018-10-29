---
title: Migrowanie z interfejsu API sieci Web platformy ASP.NET do ASP.NET Core
author: ardalis
description: Dowiedz się, jak przeprowadzić migrację z implementacją interfejsu API sieci web z interfejsu API sieci Web programu ASP.NET 4.x ASP.NET Core MVC.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/01/2018
uid: migration/webapi
ms.openlocfilehash: f5d886a7c3182b5cd372762ade67c2e748051049
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207280"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Migrowanie z interfejsu API sieci Web platformy ASP.NET do ASP.NET Core

Przez [Scott Addie](https://twitter.com/scott_addie) i [Steve Smith](https://ardalis.com/)

Interfejsu API sieci Web programu ASP.NET 4.x jest usługą HTTP, która osiąga szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych. Aplikacja interfejsu API sieci Web modele w prostszy model programowania, znany jako program ASP.NET Core MVC i platformy ASP.NET Core ujednolica MVC ASP.NET 4.x firmy. W tym artykule przedstawiono kroki wymagane do migracji z interfejsu API sieci Web 4.x ASP.NET do ASP.NET Core MVC.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

* [Zestaw SDK programu .NET core 2.1 lub nowszej](https://www.microsoft.com/net/download/all)
* [Program Visual Studio 2017](https://www.visualstudio.com/downloads/) wersji 15.7.3 lub nowszy z **ASP.NET i tworzenie aplikacji internetowych** obciążenia

## <a name="review-aspnet-4x-web-api-project"></a>Przejrzyj projekt interfejsu API sieci Web programu ASP.NET 4.x

Jako punktu wyjścia, w tym artykule wykorzystano *ProductsApp* projektu utworzonego w [wprowadzenie do wzorca ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api). W tym projekcie prosty projekt interfejsu API sieci Web platformy ASP.NET 4.x jest skonfigurowany w następujący sposób.

W *Global.asax.cs*, połączenie jest nawiązywane w przypadku `WebApiConfig.Register`:

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` jest zdefiniowany w *App_Start* folderu. Ma ona tylko jedną statyczną `Register` metody:

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15-20)]

Ta klasa umożliwia skonfigurowanie [trasowanie atrybutów](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), chociaż w rzeczywistości nie jest on używany w projekcie. Umożliwia również skonfigurowanie tabeli routingu, który jest używany przez interfejs API sieci Web platformy ASP.NET. W tym przypadku interfejsu API sieci Web programu ASP.NET 4.x oczekuje, że adresy URL służące do być zgodny z formatem `/api/{controller}/{id}`, za pomocą `{id}` opcjonalne.

*ProductsApp* projekt zawiera jeden kontroler. Kontroler dziedziczy `ApiController` i udostępnia dwie metody:

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

`Product` Używany przez model `ProductsController` jest prostą klasę:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

W poniższych sekcjach przedstawiono migracji projektu interfejsu API sieci Web platformy ASP.NET Core MVC.

## <a name="create-destination-project"></a>Utwórz projekt docelowy

Wykonaj następujące czynności w programie Visual Studio:

* Przejdź do **pliku** > **nowe** > **projektu** > **innych typów projektów**  >  **Rozwiązań programu visual Studio**. Wybierz **puste rozwiązanie**, a rozwiązanie *WebAPIMigration*. Kliknij przycisk **OK** przycisku.
* Dodaj istniejące *ProductsApp* projektu do rozwiązania.
* Dodaj nową **aplikacji sieci Web programu ASP.NET Core** projektu do rozwiązania. Wybierz **platformy .NET Core** docelowy framework z listy rozwijanej, a następnie wybierz **API** szablonu projektu. Nadaj projektowi nazwę *ProductsCore*i kliknij przycisk **OK** przycisku.

Rozwiązanie zawiera teraz dwa projekty. W poniższych sekcjach opisano Migrowanie *ProductsApp* zawartość projektu, aby *ProductsCore* projektu.

## <a name="migrate-configuration"></a>Migracja konfiguracji

Nie korzysta z platformy ASP.NET Core *App_Start* folderu lub *Global.asax* pliku, a *web.config* plik zostanie dodany na czas publikacji. *Startup.cs* zastępuje *Global.asax* i znajduje się w katalogu głównym projektu. `Startup` Klasa obsługuje wszystkie zadania uruchamiania aplikacji. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.

W programie ASP.NET Core MVC trasowanie atrybutów jest domyślnie włączone podczas <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> jest wywoływana w `Startup.Configure`. Następujące `UseMvc` wywołać zastępuje *ProductsApp* projektu *App_Start/WebApiConfig.cs* pliku:

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a>Migrowanie modeli i kontrolerów

Skopiuj *ProductApp* kontroler projektu i model używa. Wykonaj następujące kroki:

1. Kopiuj *Controllers/ProductsController.cs* z oryginalnego projektu na nową.
1. Skopiuj cały *modeli* folderu z oryginalnego projektu na nową.
1. Zmień obszary nazw kopiowanych plików, aby odpowiadała nowej nazwie projektu (*ProductsCore*). Dostosuj `using ProductsApp.Models;` instrukcji w *ProductsController.cs* zbyt.

Na tym etapie tworzenia wyniki aplikacji na wiele błędów kompilacji. Błędy występują, ponieważ następujące składniki nie istnieją w programie ASP.NET Core:

* `ApiController` Klasa
* `System.Web.Http` Namespace
* `IHttpActionResult` Interfejs

Napraw błędy w następujący sposób:

1. Zmiana `ApiController` do <xref:Microsoft.AspNetCore.Mvc.ControllerBase>. Dodaj `using Microsoft.AspNetCore.Mvc;` rozpoznać `ControllerBase` odwołania.
1. Usuń folder `using System.Web.Http;`.
1. Zmiana `GetProduct` akcja ma typ zwracany z `IHttpActionResult` do `ActionResult<Product>`.

## <a name="configure-routing"></a>Konfigurowanie routingu

Skonfiguruj routing w następujący sposób:

1. Dekoracji `ProductsController` klasy z następującymi atrybutami:

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    Poprzedni [[trasy]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) atrybut konfiguruje wzorzec routingu atrybutów kontrolera. [[Klasy ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) atrybutu sprawia, że atrybut routingu wymagana dla wszystkich akcji w tym kontrolerze.

    Routing atrybutów obsługuje tokenów, takich jak `[controller]` i `[action]`. W czasie wykonywania każdy token jest zastępowana nazwą kontroler lub akcję, odpowiednio, do którego zastosowano atrybut. Tokeny Zmniejsz liczbę magic ciągów w projekcie. Tokeny upewnij się, trasy ciągle synchronizowana z odpowiednich kontrolerów i akcji po automatyczne nazwy refaktoryzacje są stosowane.
1. Włącz żądania HTTP Get do `ProductController` akcje:
    * Zastosuj [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) atrybutu `GetAllProducts` akcji.
    * Zastosuj `[HttpGet("{id}")]` atrybutu `GetProduct` akcji.

Po tych zmian i usuwania nieużywanych `using` instrukcji *ProductsController.cs* pliku wygląda następująco:

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

Uruchom projekt zmigrowane i przejdź do `/api/products`. Zostanie wyświetlona pełna lista trzy produkty. Przejdź do `/api/products/1`. Pojawi się pierwszy produkt.

## <a name="compatibility-shim"></a>Podkładki zgodności

[Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) biblioteka zawiera poprawkę zgodności, aby przenieść projektach programów ASP.NET 4.x interfejsu API sieci Web platformy ASP.NET Core. Podkładki zgodności rozszerza platformy ASP.NET Core, obsługę wielu Konwencji z interfejsu Web API 2 platformy ASP.NET 4.x. Przykładowe przenoszone wcześniej w tym dokumencie jest wystarczająco podstawowe, że podkładki zgodność nie jest konieczne. W przypadku większych projektów przy użyciu podkładki zgodności może być przydatne do tymczasowo unaoczni API między platformą ASP.NET Core i ASP.NET 4.x Web API 2.

Podkładki zgodności internetowy interfejs API jest przeznaczone do służyć jako tymczasowy środek do obsługi migrowania dużych interfejsu API sieci Web projektach programów ASP.NET 4.x ASP.NET Core. Wraz z upływem czasu projektów powinny zostać uaktualnione do użycia wzorców platformy ASP.NET Core, zamiast polegania na poprawkę zgodności.

Zgodność funkcji dostępnych w `Microsoft.AspNetCore.Mvc.WebApiCompatShim` obejmują:

* Dodaje `ApiController` wpisz, aby kontrolerami typów podstawowych, nie muszą zostać zaktualizowane.
* Umożliwia powiązanie modelu w stylu interfejsu API sieci Web. Platforma ASP.NET Core MVC model funkcji wiązania, podobnie jak w przypadku platformy ASP.NET MVC 5 4.x, domyślnie. Zmiany podkładki zgodności modelu powiązania, które są bardziej podobne do Konwencji powiązanie modelu 4.x Web API 2 platformy ASP.NET. Na przykład typy złożone są automatycznie powiązany z treści żądania.
* Rozszerza wiązania modelu akcji kontrolera można korzystać z parametrami typu `HttpRequestMessage`.
* Dodaje elementy formatujące komunikaty umożliwiając akcji do zwrócenia wyników typu `HttpResponseMessage`.
* Dodaje metody dodatkowe odpowiedzi, które akcje Web API 2 może być używane do udostępniania odpowiedzi:
  * `HttpResponseMessage` generatory:
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * Metody wynik akcji:
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* Dodaje wystąpienie `IContentNegotiator` do aplikacji użytkownika kontenera (DI) iniekcji zależności i udostępnia związane z negocjacji typy zawartości z [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/). Przykładami takich typów `DefaultContentNegotiator` i `MediaTypeFormatter`.

Aby użyć podkładek zgodności:

1. Zainstaluj [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) pakietu NuGet.
1. Zarejestruj podkładki zgodności usług za pomocą kontenera DI aplikacji przez wywołanie metody `services.AddMvc().AddWebApiConventions()` w `Startup.ConfigureServices`.
1. Zdefiniuj specyficzne dla interfejsu API tras za pomocą sieci web `MapWebApiRoute` na `IRouteBuilder` aplikacji `IApplicationBuilder.UseMvc` wywołania.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:web-api/index>
* <xref:web-api/action-return-types>
