---
title: Migrowanie z interfejsu API sieci Web ASP.NET do ASP.NET Core
author: ardalis
description: Dowiedz się, jak migrować implementację internetowego interfejsu API z ASP.NET 4. x sieci Web API do ASP.NET Core MVC.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: migration/webapi
ms.openlocfilehash: 7f61b78c589fc9d01061b50554e5a639e372c3d8
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661848"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Migrowanie z interfejsu API sieci Web ASP.NET do ASP.NET Core

Przez [Scott Addie](https://twitter.com/scott_addie) i [Steve Smith](https://ardalis.com/)

Internetowy interfejs API ASP.NET 4. x to usługa HTTP, która dociera do szerokiego zakresu klientów, w tym przeglądarek i urządzeń przenośnych. ASP.NET Core modele aplikacji MVC i Web API łączy ASP.NET 4. x w prostszy model programowania znany jako ASP.NET Core MVC. W tym artykule przedstawiono kroki wymagane do przeprowadzenia migracji z interfejsu API sieci Web ASP.NET 4. x do ASP.NET Core MVC.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [prerequisites](../includes/net-core-prereqs-vs2019-2.2.md)]

## <a name="review-aspnet-4x-web-api-project"></a>Przegląd projektu interfejsu API sieci Web ASP.NET 4. x

Jako punkt początkowy, w tym artykule używany jest projekt *ProductsApp* utworzony w [wprowadzenie z ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api). W tym projekcie jest skonfigurowany prosty projekt interfejsu API sieci Web ASP.NET 4. x w następujący sposób.

W *Global.asax.cs*, wykonano wywołanie do `WebApiConfig.Register`:

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

Klasa `WebApiConfig` znajduje się w folderze *App_Start* i ma statyczną metodę `Register`:

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs)]

Ta klasa konfiguruje [Routing atrybutów](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), chociaż nie jest faktycznie używana w projekcie. Konfiguruje również tabelę routingu, która jest używana przez interfejs API sieci Web ASP.NET. W takim przypadku interfejs API sieci Web ASP.NET 4. x oczekuje, że adresy URL są zgodne z formatem `/api/{controller}/{id}`, z opcjonalną `{id}`.

Projekt *ProductsApp* zawiera jeden kontroler. Kontroler dziedziczy po `ApiController` i zawiera dwie akcje:

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=28,33)]

Model `Product` używany przez `ProductsController` jest prostą klasą:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

W poniższych sekcjach przedstawiono migrację projektu interfejsu API sieci Web do ASP.NET Core MVC.

## <a name="create-destination-project"></a>Utwórz projekt docelowy

Wykonaj następujące kroki w programie Visual Studio:

* Przejdź do **pliku** > **nowy** > **project** > **innych typów projektów** > **rozwiązania programu Visual Studio**. Wybierz pozycję **puste rozwiązanie**i nazwij rozwiązanie *WebAPIMigration*. Kliknij przycisk **OK**.
* Dodaj istniejący projekt *ProductsApp* do rozwiązania.
* Dodaj nowy projekt **aplikacji sieci Web ASP.NET Core** do rozwiązania. Wybierz platformę docelową **platformy .NET Core** z listy rozwijanej i wybierz szablon projektu **interfejsu API** . Nazwij projekt *ProductsCore*, a następnie kliknij przycisk **OK** .

Rozwiązanie zawiera teraz dwa projekty. W poniższych sekcjach opisano Migrowanie zawartości projektu *ProductsApp* do projektu *ProductsCore* .

## <a name="migrate-configuration"></a>Migruj konfigurację

ASP.NET Core nie używa folderu *App_Start* ani pliku *Global. asax* , a plik *Web. config* jest dodawany w czasie publikacji. *Startup.cs* jest zamiennikiem elementu *Global. asax* i znajduje się w katalogu głównym projektu. Klasa `Startup` obsługuje wszystkie zadania uruchamiania aplikacji. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.

W ASP.NET Core MVC, routing atrybutu jest uwzględniany domyślnie, gdy <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> jest wywoływana w `Startup.Configure`. Następujące wywołanie `UseMvc` zastępuje plik */webapiconfig.cs App_Start* projektu *ProductsApp* :

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a>Migrowanie modeli i kontrolerów

Kopiuj przez kontroler projektu *ProductApp* oraz model, którego używa. Wykonaj następujące kroki:

1. Skopiuj *Kontrolery/ProductsController. cs* z oryginalnego projektu do nowego.
1. Skopiuj cały folder *modele* z oryginalnego projektu do nowego.
1. Zmień przestrzenie nazw skopiowane pliki na pasujące do nowej nazwy projektu (*ProductsCore*). Dostosuj również instrukcję `using ProductsApp.Models;` w *ProductsController.cs* .

W tym momencie Kompilowanie aplikacji powoduje szereg błędów kompilacji. Błędy występują, ponieważ następujące składniki nie istnieją w ASP.NET Core:

* Klasa `ApiController`
* `System.Web.Http` przestrzeń nazw
* Interfejs `IHttpActionResult`

Usuń błędy w następujący sposób:

1. Zmień `ApiController`, aby <xref:Microsoft.AspNetCore.Mvc.ControllerBase>. Dodaj `using Microsoft.AspNetCore.Mvc;`, aby rozwiązać `ControllerBase` odwołanie.
1. Usuń folder `using System.Web.Http;`.
1. Zmień typ zwracany akcji `GetProduct` z `IHttpActionResult` na `ActionResult<Product>`.

Uprość instrukcję `return` akcji `GetProduct` w następujący sposób:

```csharp
return product;
```

## <a name="configure-routing"></a>Configure routing (Konfigurowanie routingu)

Skonfiguruj Routing w następujący sposób:

1. Oznacz klasę `ProductsController` następującymi atrybutami:

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    Poprzedni atrybut [`[Route]`](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) konfiguruje wzorzec routingu atrybutu kontrolera. Atrybut [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) powoduje, że atrybut routingu wymaga dla wszystkich akcji w tym kontrolerze.

    Routing atrybutów obsługuje tokeny, takie jak `[controller]` i `[action]`. W czasie wykonywania każdy token jest zastępowany odpowiednio nazwą kontrolera lub akcji, do którego zastosowano atrybut. Tokeny zmniejszają liczbę ciągów magicznych w projekcie. Tokeny zapewniają również, że trasy pozostają zsynchronizowane z odpowiednimi kontrolerami i akcjami, gdy stosowane są automatyczne refaktoryzacje zmiany nazwy.
1. Ustaw tryb zgodności projektu na ASP.NET Core 2,2:

    [!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    Poprzednia zmiana:

    * Jest wymagany do użycia atrybutu `[ApiController]` na poziomie kontrolera.
    * Umożliwia potencjalne uszkodzenie zachowań wprowadzonych w ASP.NET Core 2,2.
1. Włącz żądania HTTP GET do akcji `ProductController`:
    * Zastosuj atrybut [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) do akcji `GetAllProducts`.
    * Zastosuj atrybut `[HttpGet("{id}")]` do akcji `GetProduct`.

Po powyższych zmianach i usunięciu nieużywanych instrukcji `using` plik *ProductsController.cs* wygląda następująco:

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

Uruchom migrowany projekt i przejdź do `/api/products`. Zostanie wyświetlona pełna lista trzech produktów. Przejdź do `/api/products/1`. Zostanie wyświetlony pierwszy produkt.

## <a name="compatibility-shim"></a>Podkładka zgodności

Biblioteka [Microsoft. AspNetCore. MVC. WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) zawiera podkładkę zgodności do przenoszenia projektów interfejsu API sieci Web ASP.NET 4. x do ASP.NET Core. Podkładka zgodności rozszerza ASP.NET Core do obsługi wielu konwencji z ASP.NET 4. x Web API 2. Przykładowy port podany wcześniej w tym dokumencie jest wystarczająco mały, że podkładka zgodności była niepotrzebna. W przypadku większych projektów użycie podkładki zgodności może być przydatne do tymczasowego mostkowania przerwy między ASP.NET Core i ASP.NET 4. x Web API 2.

Podkładka zgodności dla interfejsu API sieci Web ma służyć jako tymczasowa miara do obsługi migrowania dużych ASP.NETych projektów interfejsu API sieci Web do ASP.NET Core. W miarę upływu czasu projekty należy zaktualizować tak, aby korzystały z wzorców ASP.NET Core zamiast polegać na podkładki zgodności.

Funkcje zgodności zawarte w `Microsoft.AspNetCore.Mvc.WebApiCompatShim` obejmują:

* Dodaje typ `ApiController`, aby nie trzeba było aktualizować typów podstawowych kontrolerów.
* Włącza powiązanie modelu internetowego interfejsu API. ASP.NET Core funkcji powiązania modelu MVC podobnie jak w przypadku ASP.NET 4. x MVC 5, domyślnie. W ramach podkładki zgodności zmiany powiązania modelu są bardziej podobne do Konwencji powiązań modelu ASP.NET 4. x sieci Web. Na przykład typy złożone są automatycznie powiązane z treścią żądania.
* Rozszerza powiązanie modelu, aby akcje kontrolera mogły przyjmować parametry typu `HttpRequestMessage`.
* Dodaje elementy formatujące komunikaty umożliwiające działanie zwracające wyniki typu `HttpResponseMessage`.
* Dodaje dodatkowe metody odpowiedzi, które mogą być używane przez działania Web API 2 do obsłużenia odpowiedzi:
  * Generatory `HttpResponseMessage`:
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * Metody wyniku akcji:
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* Dodaje wystąpienie `IContentNegotiator` do kontenera iniekcji zależności aplikacji i udostępnia typy powiązane z negocjowaniem zawartości z [Microsoft. ASPNET. WebApi. Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/). Przykłady takich typów obejmują `DefaultContentNegotiator` i `MediaTypeFormatter`.

Aby użyć podkładek zgodności:

1. Zainstaluj pakiet NuGet [Microsoft. AspNetCore. MVC. WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) .
1. Zarejestruj usługi podkładki zgodności z kontenerem DI aplikacji, wywołując `services.AddMvc().AddWebApiConventions()` w `Startup.ConfigureServices`.
1. Zdefiniuj trasy specyficzne dla interfejsu API sieci Web przy użyciu `MapWebApiRoute` na `IRouteBuilder` w wywołaniu `IApplicationBuilder.UseMvc` aplikacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>
