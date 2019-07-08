---
title: Tworzenie interfejsów API sieci web za pomocą platformy ASP.NET Core
author: scottaddie
description: Poznaj podstawy tworzenia internetowego interfejsu API w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/08/2019
uid: web-api/index
ms.openlocfilehash: 4f9c334f74dd2a8b7c31c7a42703fa361ccf9139
ms.sourcegitcommit: 91cc1f07ef178ab709ea42f8b3a10399c970496e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622794"
---
# <a name="create-web-apis-with-aspnet-core"></a>Tworzenie interfejsów API sieci web za pomocą platformy ASP.NET Core

Przez [Scott Addie](https://github.com/scottaddie) i [Tom Dykstra](https://github.com/tdykstra)

Platforma ASP.NET Core obsługuje tworzenia usługi RESTful, nazywana również internetowych interfejsów API, za pomocą C#. Do obsługi żądań, internetowy interfejs API korzysta z kontrolerów. *Kontrolery* w internetowym interfejsie API są klas, które wynikają z `ControllerBase`. W tym artykule pokazano, jak używać kontrolery do obsługi żądań interfejsu API.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples). ([Sposobu pobierania](xref:index#how-to-download-a-sample)).

## <a name="controllerbase-class"></a>Klasa ControllerBase

Interfejs API sieci web ma co najmniej jedną klasę kontrolera, które wynikają z <xref:Microsoft.AspNetCore.Mvc.ControllerBase>. Na przykład szablonu projektu interfejsu API sieci web tworzy kontroler wartości:

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=3)]

Nie należy tworzyć kontroler internetowego interfejsu API przez pochodząca od <xref:Microsoft.AspNetCore.Mvc.Controller> klasy. `Controller` pochodzi od klasy `ControllerBase` i dodaje obsługę widoków, więc w przypadku stron sieci web obsługi żądań internetowego interfejsu API.  Wyjątkiem od tej reguły: Jeśli planujesz używać tego samego kontrolera zarówno dla widoków i interfejsów API, pochodzi z `Controller`.

`ControllerBase` Klasy zawiera wiele właściwości i metod, które są przydatne do obsługi żądań HTTP. Na przykład `ControllerBase.CreatedAtAction` zwraca kod stanu 201:

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=8-9)]

 Poniżej przedstawiono kilka przykładów więcej metod, `ControllerBase` udostępnia.

|Metoda  |Uwagi  |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>| Zwraca kod stanu 400.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> |Zwraca kod stanu 404.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile*>|Zwraca plik.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>|Wywołuje [wiązanie modelu](xref:mvc/models/model-binding).|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel*>|Wywołuje [Walidacja modelu](xref:mvc/models/validation).|

Aby uzyskać listę wszystkich dostępnych metod i właściwości, zobacz <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.

## <a name="attributes"></a>Atrybuty

<xref:Microsoft.AspNetCore.Mvc> Przestrzeń nazw zawiera atrybuty, które mogą służyć do konfigurowania zachowania interfejsu API sieci web kontrolerów i metod akcji. W poniższym przykładzie użyto atrybutów, aby określić metodę HTTP i zwrócony kodów stanu:

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

Poniżej przedstawiono kilka przykładów więcej atrybutów, które są dostępne.

|Atrybut|Uwagi|
|---------|-----|
|[[Trasy]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)      |Określa adres URL wzorzec dla kontrolera lub akcji.|
|[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)        |Określa prefiks i właściwości do włączenia do wiązania modelu.|
|[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)  |Określa akcję, która obsługuje metodę HTTP GET.|
|[[Zużywa]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)|Określa typy danych, które akceptuje akcji.|
|[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)|Określa typy danych, które zwraca akcji.|

Dla listy, która zawiera atrybuty, dostępności, zobacz <xref:Microsoft.AspNetCore.Mvc> przestrzeni nazw.

## <a name="apicontroller-attribute"></a>Atrybut klasy ApiController

[[Klasy ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) można zastosować atrybutów do klasy kontrolera umożliwiają zachowania specyficzne dla interfejsu API:

* [Wymagania routingu atrybutu](#attribute-routing-requirement)
* [Automatyczne odpowiedzi HTTP 400](#automatic-http-400-responses)
* [Powiązanie źródła parametru wnioskowania](#binding-source-parameter-inference)
* [Wnioskowanie multipart/formularza data żądania](#multipartform-data-request-inference)
* [Szczegóły problemu kodów stanu błędu](#problem-details-for-error-status-codes)

Te funkcje wymagają [zgodność wersji](<xref:mvc/compatibility-version>) 2.1 lub nowszej.

### <a name="apicontroller-on-specific-controllers"></a>Klasy ApiController kontrolerach określonych

`[ApiController]` Atrybut można stosować do określonych kontrolerów, jak w poniższym przykładzie, za pomocą szablonu projektu:

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=2)]

### <a name="apicontroller-on-multiple-controllers"></a>Klasy ApiController na wielu kontrolerach

Jedno z podejść do przy użyciu atrybutu na więcej niż jeden kontroler jest utworzenie klasy niestandardowej kontrolera podstawowego, oznaczony za pomocą `[ApiController]` atrybutu. Oto przykład pokazujący niestandardowej klasy bazowej i na kontrolerze, który pochodzi od niego:

[!code-csharp[](index/samples/2.x/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_Inherit)]

### <a name="apicontroller-on-an-assembly"></a>Klasy ApiController w zestawie

Jeśli [zgodność wersji](<xref:mvc/compatibility-version>) jest ustawiony do wersji 2.2 lub nowszej, `[ApiController]` atrybut można stosować do zestawu. Adnotacja w ten sposób dotyczy wszystkich kontrolerów w zestawie zachowanie interfejsu API sieci web. Nie ma możliwości można wycofać się dla poszczególnych kontrolerów. Zastosuj atrybut poziomu zestawu do `Startup` klasy, jak pokazano w poniższym przykładzie:

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

## <a name="attribute-routing-requirement"></a>Wymagania routingu atrybutu

`ApiController` Atrybutu sprawia, że atrybut routingu wymagania. Na przykład:

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=1)]

Akcje są niedostępne za pośrednictwem [konwencjonalne trasy](xref:mvc/controllers/routing#conventional-routing) definicją <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> lub <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> w `Startup.Configure`.

## <a name="automatic-http-400-responses"></a>Automatyczne odpowiedzi HTTP 400

`ApiController` Atrybutu sprawia, że błędy sprawdzania poprawności modelu automatycznie odpowiedzią wyzwalacza HTTP 400. W związku z tym poniższy kod jest konieczny w metodzie akcji:

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

### <a name="default-badrequest-response"></a>Domyślny element BadRequest odpowiedzi 

Za pomocą wersji zgodności 2,2 lub nowszy, jest domyślny typ odpowiedzi na odpowiedzi HTTP 400 <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>. `ValidationProblemDetails` Typ jest zgodny z [specyfikacji RFC 7807](https://tools.ietf.org/html/rfc7807).

Aby zmienić domyślną odpowiedź do <xref:Microsoft.AspNetCore.Mvc.SerializableError>ustaw `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` właściwości `true` w `Startup.ConfigureServices`, jak pokazano w poniższym przykładzie:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,9)]

### <a name="customize-badrequest-response"></a>Dostosowywanie element BadRequest odpowiedzi

Aby dostosować odpowiedzi, który jest wynikiem błędu sprawdzania poprawności, należy użyć <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>. Dodaj następujący wyróżniony kod po `services.AddMvc().SetCompatibilityVersion`:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureBadRequestResponse&highlight=3-20)]

### <a name="log-automatic-400-responses"></a>Zaloguj się automatyczne odpowiedzi 400

Zobacz [sposobu logowania się automatyczne 400 odpowiedzi na błędy sprawdzania poprawności modelu (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).

### <a name="disable-automatic-400"></a>Wyłącz automatyczne 400

Aby wyłączyć automatyczne zachowanie 400, należy ustawić <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> właściwość `true`. Dodaj następujący wyróżniony kod w `Startup.ConfigureServices` po `services.AddMvc().SetCompatibilityVersion`:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

## <a name="binding-source-parameter-inference"></a>Powiązanie źródła parametru wnioskowania

Atrybut źródłowy powiązania Określa lokalizację, w którym znajduje się wartość parametru akcji. Istnieją następujące atrybuty źródło powiązania:

|Atrybut|Źródło wiążące |
|---------|---------|
|[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)     | Treść żądania |
|[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)     | Dane formularza w treści żądania |
|[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) | Nagłówek żądania |
|[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)   | Parametr ciągu zapytania żądania |
|[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)   | Dane trasy z bieżącego żądania |
|[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) | Usługa żądania wprowadzony jako parametru akcji |

> [!WARNING]
> Nie używaj `[FromRoute]` po wartości mogą zawierać `%2f` (to znaczy `/`). `%2f` nie będzie unescaped do `/`. Użyj `[FromQuery]` Jeśli wartość może zawierać `%2f`.

Bez `[ApiController]` atrybutu lub powiązania atrybutów źródłowych, takich jak `[FromQuery]`, środowisko uruchomieniowe programu ASP.NET Core podejmują próbę użycia integratora modelu obiektu złożonego. Integrator modelu obiektu złożonego ściąga dane z dostawców wartości w zdefiniowanej kolejności.

W poniższym przykładzie `[FromQuery]` atrybut wskazuje, że `discontinuedOnly` podano wartość parametru ciągu zapytania adresu URL żądania:

[!code-csharp[](index/samples/2.x/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

`[ApiController]` Atrybutu stosuje reguły wnioskowania dla źródeł danych domyślne parametry akcji. Te reguły Zapisz z konieczności identyfikowanie źródeł powiązania, ręcznie przez zastosowanie atrybutów do parametrów akcji. Powiązanie źródła wnioskowania, dla których zasady mają następujące zachowanie:

* `[FromBody]` jest wnioskowany dla parametrów typu złożonego. Wyjątek dotyczący `[FromBody]` reguły wnioskowania jest dowolny typ złożony, wbudowane o specjalnym znaczeniu, takich jak <xref:Microsoft.AspNetCore.Http.IFormCollection> i <xref:System.Threading.CancellationToken>. Wnioskowanie o kodzie źródłowym powiązania ignoruje te typy specjalne. 
* `[FromForm]` jest wnioskowany dla parametrach akcji danego typu <xref:Microsoft.AspNetCore.Http.IFormFile> i <xref:Microsoft.AspNetCore.Http.IFormFileCollection>. Nie wynika dla wszystkich typów prostych lub zdefiniowanych przez użytkownika.
* `[FromRoute]` jest wnioskowany dla dowolnej nazwy parametru akcji parametrowi w szablonie trasy. Gdy więcej niż jedna trasa jest zgodna z parametrem akcji, wartości trasy jest uważany za `[FromRoute]`.
* `[FromQuery]` jest wnioskowany dla innych parametrów akcji.

### <a name="frombody-inference-notes"></a>Informacje o wnioskowania FromBody

`[FromBody]` nie jest wnioskowany dla typów prostych, takich jak `string` lub `int`. W związku z tym `[FromBody]` atrybut powinien być używany dla typów prostych, gdy te funkcje są potrzebne.

Kiedy akcja ma więcej niż jeden parametr, powiązany z treści żądania, jest zgłaszany wyjątek. Na przykład wszystkie poniższe podpisy metod akcji spowodować wyjątek:

* `[FromBody]` wywnioskowane zarówno, ponieważ są one typy złożone.

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* `[FromBody]` atrybut na jednym, wywnioskowane z drugiej strony, ponieważ jest typem złożonym.

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* `[FromBody]` atrybut na obu.

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

> [!NOTE]
> W programie ASP.NET Core 2.1 parametrów typu kolekcji, takie jak listy i tablice są niepoprawnie wnioskowane jako `[FromQuery]`. `[FromBody]` Atrybut powinien być używany dla tych parametrów, jeśli mają być powiązane z treści żądania. To zachowanie poprawia się w programie ASP.NET Core 2.2 lub nowszej, gdzie wywnioskowana, parametry typu kolekcji można powiązać z treści domyślnie.

### <a name="disable-inference-rules"></a>Wyłącz zasady wnioskowania

Aby wyłączyć wnioskowania źródło powiązania, ustaw <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> do `true`. Dodaj następujący kod w `Startup.ConfigureServices` po `services.AddMvc().SetCompatibilityVersion`:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

## <a name="multipartform-data-request-inference"></a>Wnioskowanie multipart/formularza data żądania

`[ApiController]` Atrybut ma zastosowanie dana zasada wnioskowania, gdy parametr akcji jest oznaczony za pomocą [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) atrybut: `multipart/form-data` żądania jest wnioskowany typ zawartości.

Aby wyłączyć to zachowanie domyślne, należy ustawić <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> do `true` w `Startup.ConfigureServices`, jak pokazano w poniższym przykładzie:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

## <a name="problem-details-for-error-status-codes"></a>Szczegóły problemu kodów stanu błędu

W przypadku 2,2 lub nowszej wersji platformy MVC przekształca wynik błędu (wynik kod stanu 400 lub nowszej) do wyniku z <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>. `ProblemDetails` Typu opiera się na [specyfikacji RFC 7807](https://tools.ietf.org/html/rfc7807) zapewniające szczegóły błędu czytelny dla komputerów w odpowiedzi HTTP.

Poniższy kod w akcji kontrolera należy wziąć pod uwagę:

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

Odpowiedź HTTP dotycząca elementu `NotFound` ma kod stanu 404 z `ProblemDetails` treści. Na przykład:

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="customize-problemdetails-response"></a>Dostosowywanie ProblemDetails odpowiedzi

Użyj `ClientErrorMapping` właściwości, aby skonfigurować zawartość `ProblemDetails` odpowiedzi. Na przykład, poniższy kod aktualizacje `type` właściwość odpowiedzi 404:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

### <a name="disable-problemdetails-response"></a>Wyłącz ProblemDetails odpowiedzi

Automatyczne tworzenie `ProblemDetails` jest wyłączona, gdy `SuppressMapClientErrors` właściwość jest ustawiona na `true`. Dodaj następujący kod w `Startup.ConfigureServices`:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

## <a name="additional-resources"></a>Dodatkowe zasoby 

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
