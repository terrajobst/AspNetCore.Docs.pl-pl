---
title: Tworzenie internetowych interfejsów API za pomocą ASP.NET Core
author: scottaddie
description: Poznaj podstawy tworzenia internetowego interfejsu API w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 11/22/2019
uid: web-api/index
ms.openlocfilehash: 5ef8b4d012f4ed90339ffea191612e4dc365d958
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880527"
---
# <a name="create-web-apis-with-aspnet-core"></a>Tworzenie internetowych interfejsów API za pomocą ASP.NET Core

Przez [Scott Addie](https://github.com/scottaddie) i [Tomasz Dykstra](https://github.com/tdykstra)

Platforma ASP.NET Core obsługuje tworzenie usług RESTful, znanych także jako internetowe interfejsy API, za pomocą języka C#. Aby obsługiwać żądania, interfejs API sieci Web używa kontrolerów. *Kontrolery* w INTERNETowym interfejsie API są klasami pochodnymi od `ControllerBase`. W tym artykule pokazano, jak używać kontrolerów do obsługi żądań interfejsu API sieci Web.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples). ([Jak pobrać](xref:index#how-to-download-a-sample)).

## <a name="controllerbase-class"></a>Klasa ControllerBase

Internetowy interfejs API składa się z co najmniej jednej klasy kontrolera, która pochodzi od <xref:Microsoft.AspNetCore.Mvc.ControllerBase>. Szablon projektu internetowego interfejsu API zawiera kontroler początkowy:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

Nie twórz kontrolera interfejsu API sieci Web, pobierając z klasy <xref:Microsoft.AspNetCore.Mvc.Controller>. `Controller` pochodzi od `ControllerBase` i dodaje obsługę widoków, dlatego służy do obsługi stron sieci Web, a nie żądań interfejsu API sieci Web. Istnieje wyjątek od tej reguły: Jeśli planujesz używać tego samego kontrolera dla widoków i interfejsów API sieci Web, utwórz go z `Controller`.

Klasa `ControllerBase` dostarcza wiele właściwości i metod, które są przydatne do obsługi żądań HTTP. Na przykład `ControllerBase.CreatedAtAction` zwraca kod stanu 201:

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_400And201&highlight=10)]

Poniżej przedstawiono kilka przykładów metod, które zapewnia `ControllerBase`.

|Metoda   |Uwagi    |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest%2A>| Zwraca kod stanu 400.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound%2A>|Zwraca kod stanu 404.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile%2A>|Zwraca plik.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync%2A>|Wywołuje [powiązanie modelu](xref:mvc/models/model-binding).|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel%2A>|Wywołuje [walidację modelu](xref:mvc/models/validation).|

Aby uzyskać listę wszystkich dostępnych metod i właściwości, zobacz <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.

## <a name="attributes"></a>{1&gt;{2&gt;Atrybuty&lt;2}&lt;1}

Przestrzeń nazw <xref:Microsoft.AspNetCore.Mvc> zawiera atrybuty, których można użyć do skonfigurowania zachowania kontrolerów internetowego interfejsu API i metod akcji. Poniższy przykład używa atrybutów, aby określić obsługiwane zlecenie akcji HTTP i wszystkie znane kody stanu HTTP, które mogą zostać zwrócone:

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

Poniżej przedstawiono kilka przykładów dostępnych atrybutów.

|Atrybut|Uwagi|
|---------|-----|
|[`[Route]`](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)      |Określa wzorzec adresu URL dla kontrolera lub akcji.|
|[`[Bind]`](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)        |Określa prefiks i właściwości, które mają zostać dołączone do powiązania modelu.|
|[`[HttpGet]`](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)  |Identyfikuje akcję, która obsługuje czasownik HTTP GET.|
|[`[Consumes]`](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)|Określa typy danych, które akcja akceptuje.|
|[`[Produces]`](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)|Określa typy danych, które zwraca akcja.|

Aby zapoznać się z listą zawierającą dostępne atrybuty, zapoznaj się z przestrzenią nazw <xref:Microsoft.AspNetCore.Mvc>.

## <a name="apicontroller-attribute"></a>ApiController — atrybut

Atrybut [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) można zastosować do klasy kontrolera, aby włączyć następujące zachowania dotyczące interfejsów API ceniona:

* [Wymagania dotyczące routingu atrybutów](#attribute-routing-requirement)
* [Automatyczne odpowiedzi HTTP 400](#automatic-http-400-responses)
* [Wnioskowanie parametru źródła powiązania](#binding-source-parameter-inference)
* [Wieloczęściowe/formularz-wnioskowanie dotyczące danych](#multipartform-data-request-inference)
* [Szczegóły problemu dotyczące kodów stanu błędu](#problem-details-for-error-status-codes)

Te funkcje wymagają [wersji](xref:mvc/compatibility-version) 2,1 lub nowszej.

### <a name="attribute-on-specific-controllers"></a>Atrybut na określonych kontrolerach

Atrybut `[ApiController]` można zastosować do określonych kontrolerów, jak w poniższym przykładzie z szablonu projektu:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2])]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=2)]

::: moniker-end

### <a name="attribute-on-multiple-controllers"></a>Atrybut na wielu kontrolerach

Jednym z metod używania atrybutu na więcej niż jednym kontrolerze jest utworzenie niestandardowej klasy kontrolera podstawowego z adnotacją z atrybutem `[ApiController]`. Poniższy przykład przedstawia niestandardową klasę bazową i kontroler, który pochodzi od niego:

[!code-csharp[](index/samples/2.x/2.2/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="attribute-on-an-assembly"></a>Atrybut w zestawie

Jeśli [wersja zgodności](xref:mvc/compatibility-version) jest ustawiona na 2,2 lub nowsza, atrybut `[ApiController]` może zostać zastosowany do zestawu. Adnotacja w ten sposób stosuje zachowanie internetowego interfejsu API do wszystkich kontrolerów w zestawie. Nie ma możliwości rezygnacji z poszczególnych kontrolerów. Zastosuj atrybut poziomu zestawu do deklaracji przestrzeni nazw otaczającej klasę `Startup`:

```csharp
[assembly: ApiController]
namespace WebApiSample
{
    public class Startup
    {
        ...
    }
}
```

::: moniker-end

## <a name="attribute-routing-requirement"></a>Wymagania dotyczące routingu atrybutów

Atrybut `[ApiController]` powoduje, że atrybut routingu wymaga. Na przykład:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2)]

Akcje są niedostępne za pośrednictwem [konwencjonalnych tras](xref:mvc/controllers/routing#conventional-routing) zdefiniowanych przez `UseEndpoints`, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A>lub <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> w `Startup.Configure`.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=1)]

Akcje są niedostępne za pośrednictwem [konwencjonalnych tras](xref:mvc/controllers/routing#conventional-routing) zdefiniowanych przez <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A> lub <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> w `Startup.Configure`.

::: moniker-end

## <a name="automatic-http-400-responses"></a>Automatyczne odpowiedzi HTTP 400

`[ApiController]` atrybutu powoduje, że błędy walidacji modelu automatycznie wyzwalają odpowiedź HTTP 400. W związku z tym Poniższy kod jest zbędny w metodzie akcji:

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

ASP.NET Core MVC używa filtru akcji <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter>, aby wykonać poprzednią kontrolę.

### <a name="default-badrequest-response"></a>Domyślna odpowiedź nieprawidłowego żądania

W przypadku zgodności z wersją 2,1, domyślny typ odpowiedzi dla odpowiedzi HTTP 400 jest <xref:Microsoft.AspNetCore.Mvc.SerializableError>. Następująca treść żądania jest przykładem serializowanego typu:

```json
{
  "": [
    "A non-empty request body is required."
  ]
}
```

::: moniker range=">= aspnetcore-2.2"

W przypadku zgodności z wersją 2,2 lub nowszą domyślnym typem odpowiedzi dla odpowiedzi HTTP 400 jest <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>. Następująca treść żądania jest przykładem serializowanego typu:

```json
{
  "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",
  "title": "One or more validation errors occurred.",
  "status": 400,
  "traceId": "|7fb5e16a-4c8f23bbfc974667.",
  "errors": {
    "": [
      "A non-empty request body is required."
    ]
  }
}
```

Typ `ValidationProblemDetails`:

* Zapewnia czytelny dla maszyn format służący do określania błędów w odpowiedziach interfejsu API sieci Web.
* Jest zgodna ze [specyfikacją RFC 7807](https://tools.ietf.org/html/rfc7807).

::: moniker-end

### <a name="log-automatic-400-responses"></a>Rejestruj automatyczne odpowiedzi 400

Zobacz [, jak rejestrować 400 automatyczne odpowiedzi na błędy walidacji modelu (ASPNET/AspNetCore. Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).

### <a name="disable-automatic-400-response"></a>Wyłącz automatyczną odpowiedź 400

Aby wyłączyć zachowanie automatycznego 400, ustaw właściwość <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> na `true`. Dodaj następujący wyróżniony kod w `Startup.ConfigureServices`:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,6)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,5)]

::: moniker-end

## <a name="binding-source-parameter-inference"></a>Wnioskowanie parametru źródła powiązania

Atrybut źródłowy powiązania definiuje lokalizację, w której zostanie znaleziona wartość parametru akcji. Istnieją następujące atrybuty źródła powiązania:

|Atrybut|Źródło powiązania |
|---------|---------|
|[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)     | Treść żądania |
|[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)     | Formularz danych w treści żądania |
|[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) | Nagłówek żądania |
|[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)   | Parametr ciągu zapytania żądania |
|[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)   | Kierowanie danych z bieżącego żądania |
|[`[FromServices]`](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) | Usługa żądania wstrzykiwana jako parametr akcji |

> [!WARNING]
> Nie używaj `[FromRoute]`, gdy wartości mogą zawierać `%2f` (`/`). `%2f` nie zostanie wystąpić do `/`. Użyj `[FromQuery]`, jeśli wartość może zawierać `%2f`.

Bez atrybutów `[ApiController]` lub źródłowych powiązań, takich jak `[FromQuery]`, środowisko uruchomieniowe ASP.NET Core próbuje użyć spinacza modelu obiektów złożonych. Segregator modelu obiektów złożonych pobiera dane od dostawców wartości w zdefiniowanej kolejności.

W poniższym przykładzie atrybut `[FromQuery]` wskazuje, że wartość parametru `discontinuedOnly` jest podana w ciągu zapytania w adresie URL żądania:

[!code-csharp[](index/samples/2.x/2.2/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

Atrybut `[ApiController]` stosuje reguły wnioskowania dla domyślnych źródeł danych parametrów akcji. Te reguły zapisują nie trzeba ręcznie identyfikować źródeł powiązań przez zastosowanie atrybutów do parametrów akcji. Reguły wnioskowania źródła powiązań zachowują się w następujący sposób:

* `[FromBody]` jest wnioskowany dla parametrów typu złożonego. Wyjątek dla reguły wnioskowania `[FromBody]` jest dowolnym złożonym, wbudowanym typem ze specjalnym znaczeniem, takim jak <xref:Microsoft.AspNetCore.Http.IFormCollection> i <xref:System.Threading.CancellationToken>. Kod wnioskowania źródła powiązania ignoruje te typy specjalne.
* `[FromForm]` są wywnioskowane dla parametrów akcji typu <xref:Microsoft.AspNetCore.Http.IFormFile> i <xref:Microsoft.AspNetCore.Http.IFormFileCollection>. Nie jest wywnioskowane dla żadnego prostego lub zdefiniowanego przez użytkownika typu.
* `[FromRoute]` jest wywnioskowany dla każdej nazwy parametru akcji pasującej do parametru w szablonie trasy. Jeśli więcej niż jedna trasa pasuje do parametru akcji, dowolna wartość trasy jest uznawana za `[FromRoute]`.
* `[FromQuery]` jest wywnioskowany dla wszystkich innych parametrów akcji.

### <a name="frombody-inference-notes"></a>FromBody informacje o wnioskach

`[FromBody]` nie są wywnioskowane dla typów prostych, takich jak `string` lub `int`. W związku z tym, atrybut `[FromBody]` powinien być używany dla typów prostych, gdy ta funkcja jest wymagana.

Gdy akcja ma więcej niż jeden parametr powiązany z treścią żądania, zgłaszany jest wyjątek. Na przykład, wszystkie następujące sygnatury metody akcji powodują wyjątek:

* `[FromBody]` wywnioskowane na obu, ponieważ są to typy złożone.

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* atrybut `[FromBody]` na jednym, wywnioskowany na drugim, ponieważ jest typem złożonym.

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* atrybut `[FromBody]` obu.

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

::: moniker range="= aspnetcore-2.1"

> [!NOTE]
> W ASP.NET Core 2,1 parametry typu kolekcji, takie jak listy i tablice, są nieprawidłowo wnioskowane jako `[FromQuery]`. Atrybut `[FromBody]` powinien być używany dla tych parametrów, jeśli mają być powiązane z treścią żądania. To zachowanie jest korygowane w ASP.NET Core 2,2 lub nowszych, gdzie parametry typu kolekcji są wyrzucane jako powiązane z treścią domyślnie.

::: moniker-end

### <a name="disable-inference-rules"></a>Wyłącz reguły wnioskowania

Aby wyłączyć wnioskowanie źródłowe powiązania, ustaw <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> na `true`. Dodaj następujący kod w `Startup.ConfigureServices`:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,5)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,4)]

::: moniker-end

## <a name="multipartform-data-request-inference"></a>Wieloczęściowe/formularz-wnioskowanie dotyczące danych

Atrybut `[ApiController]` stosuje regułę wnioskowania, gdy parametr akcji ma adnotację z atrybutem [`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) . Typ zawartości żądania `multipart/form-data` jest wywnioskowany.

Aby wyłączyć domyślne zachowanie, ustaw właściwość <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> na `true` w `Startup.ConfigureServices`:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,4)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="problem-details-for-error-status-codes"></a>Szczegóły problemu dotyczące kodów stanu błędu

Gdy wersja zgodności to 2,2 lub nowsza, MVC przeprowadzi wynik błędu (wynik z kodem stanu 400 lub nowszym) do wyniku z <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>. Typ `ProblemDetails` jest oparty na [specyfikacji RFC 7807](https://tools.ietf.org/html/rfc7807) do udostępniania szczegółowych informacji o błędach w odpowiedzi HTTP.

Rozważmy następujący kod w akcji kontrolera:

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

Metoda `NotFound` generuje kod stanu HTTP 404 z treścią `ProblemDetails`. Na przykład:

```json
{
  type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
  title: "Not Found",
  status: 404,
  traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="disable-problemdetails-response"></a>Wyłącz odpowiedź ProblemDetails

Automatyczne tworzenie wystąpienia `ProblemDetails` jest wyłączone, gdy właściwość <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors%2A> jest ustawiona na `true`. Dodaj następujący kod w `Startup.ConfigureServices`:

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,7)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:web-api/action-return-types>
* <xref:web-api/handle-errors>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
