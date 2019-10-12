---
title: Formatowanie danych odpowiedzi w ASP.NET Core Web API
author: ardalis
description: Dowiedz się, jak sformatować dane odpowiedzi w ASP.NET Core Web API.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 8/22/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: 0dd8b3b5ec58a199db086c4c0b0f057d26afd589
ms.sourcegitcommit: 7a2c820692f04bfba04398641b70f27fa15391f5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2019
ms.locfileid: "72290633"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a>Formatowanie danych odpowiedzi w ASP.NET Core Web API

Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC obsługuje formatowanie danych odpowiedzi. Dane odpowiedzi można sformatować przy użyciu określonych formatów lub w odpowiedzi na żądany format klienta.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="format-specific-action-results"></a>Wyniki akcji specyficzne dla formatu

Niektóre typy wyników akcji są specyficzne dla określonego formatu, takiego jak <xref:Microsoft.AspNetCore.Mvc.JsonResult> i <xref:Microsoft.AspNetCore.Mvc.ContentResult>. Akcje mogą zwracać wyniki sformatowane w określonym formacie, niezależnie od preferencji klienta. Na przykład zwrócenie `JsonResult` zwraca dane w formacie JSON. Zwrócenie `ContentResult` lub ciągu zwraca dane ciągu w formacie zwykłego tekstu.

Akcja nie jest wymagana do zwrócenia żadnego określonego typu. ASP.NET Core obsługuje dowolną wartość zwracaną przez obiekt.  Wyniki akcji, które zwracają obiekty, które nie są typu <xref:Microsoft.AspNetCore.Mvc.IActionResult> są serializowane przy użyciu odpowiedniej implementacji <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter>. Aby uzyskać więcej informacji, zobacz <xref:web-api/action-return-types>.

Wbudowana metoda pomocnika <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> zwraca dane sformatowane w formacie JSON: [!code-csharp @ no__t-2

Pobieranie próbek zwraca listę autorów. Za pomocą narzędzi deweloperskich przeglądarki F12 lub [po](https://www.getpostman.com/tools) powyższym kodzie:

* Zostanie wyświetlony nagłówek odpowiedzi zawierający **Typ zawartości:** `application/json; charset=utf-8`.
* Wyświetlane są nagłówki żądań. Na przykład nagłówek `Accept`. Nagłówek `Accept` jest ignorowany przez poprzedni kod.

Aby zwrócić dane sformatowane przy użyciu zwykłego tekstu, użyj <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> i pomocnika <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content>:

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_about)]

W poprzednim kodzie zwracana wartość `Content-Type` jest `text/plain`. Zwracanie ciągu zapewnia `Content-Type` z `text/plain`:

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_string)]

W przypadku akcji z wieloma zwracanymi typami zwraca `IActionResult`. Na przykład zwrócenie różnych kodów stanu HTTP w oparciu o wynik wykonanych operacji.

## <a name="content-negotiation"></a>Negocjowanie zawartości

Negocjowanie zawartości odbywa się, gdy klient określi [nagłówek Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html). Domyślny format używany przez ASP.NET Core to [JSON](https://json.org/). Negocjowanie zawartości:

* Zaimplementowane przez <xref:Microsoft.AspNetCore.Mvc.ObjectResult>.
* Wbudowane wyniki akcji specyficzne dla kodu stanu zwracane z metod pomocnika. Metody pomocnika wyników akcji są oparte na `ObjectResult`.

Po zwróceniu typu modelu typem zwracanym jest `ObjectResult`.

Poniższa metoda działania używa metod pomocnika `Ok` i `NotFound`:

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_search)]

Domyślnie ASP.NET Core obsługuje typy nośników `application/json`, `text/json` i `text/plain`. Narzędzia takie jak [programu Fiddler](https://www.telerik.com/fiddler) lub [Poster](https://www.getpostman.com/tools) mogą ustawić nagłówek żądania `Accept` w celu określenia formatu powrotu. Gdy nagłówek `Accept` zawiera typ obsługiwany przez serwer, zwracany jest ten typ. W następnej sekcji pokazano, jak dodać dodatkowe elementy formatujące.

Akcje kontrolera mogą zwracać POCOs (zwykłe stare obiekty CLR). Gdy POCO jest zwracany, środowisko uruchomieniowe automatycznie tworzy `ObjectResult`, które zawija obiekt. Klient pobiera sformatowany obiekt Zserializowany. Jeśli zwracany obiekt jest `null`, zwracana jest odpowiedź `204 No Content`.

Zwracanie typu obiektu:

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_alias)]

W poprzednim kodzie żądanie poprawnego aliasu autora zwraca odpowiedź `200 OK` z danymi autora. Żądanie dotyczące nieprawidłowego aliasu zwraca odpowiedź `204 No Content`.

### <a name="the-accept-header"></a>Nagłówek Accept

*Negocjowanie* zawartości odbywa się, gdy w żądaniu pojawi się nagłówek `Accept`. Gdy żądanie zawiera nagłówek Accept, ASP.NET Core:

* Wylicza typy nośników w nagłówku Accept w kolejności preferencji.
* Próbuje znaleźć program formatujący, który może wygenerować odpowiedź w jednym z określonych formatów.

Jeśli nie zostanie znaleziony żaden program formatujący, który może spełnić żądanie klienta, ASP.NET Core:

* Zwraca wartość `406 Not Acceptable`, jeśli ustawiono <xref:Microsoft.AspNetCore.Mvc.MvcOptions>, lub-
* Próbuje znaleźć pierwszy program formatujący, który może wygenerować odpowiedź.

Jeśli nie skonfigurowano programu formatującego dla żądanego formatu, jest używany pierwszy program formatujący, który może formatować obiekt. Jeśli w żądaniu nie pojawi się nagłówek `Accept`:

* Pierwszy program formatujący, który może obsłużyć obiekt, jest używany do serializacji odpowiedzi.
* Nie ma żadnej negocjacji. Serwer określa format do zwrócenia.

Jeśli nagłówek Accept zawiera `*/*`, nagłówek jest ignorowany, chyba że `RespectBrowserAcceptHeader` ma wartość true w <xref:Microsoft.AspNetCore.Mvc.MvcOptions>.

### <a name="browsers-and-content-negotiation"></a>Przeglądarki i negocjacje zawartości

W przeciwieństwie do typowych klientów interfejsu API, przeglądarki sieci Web dostarczają nagłówki `Accept`. Przeglądarka sieci Web określa wiele formatów, w tym symboli wieloznacznych. Domyślnie, gdy struktura wykryje, że żądanie pochodzi z przeglądarki:

* Nagłówek `Accept` jest ignorowany.
* Zawartość jest zwracana w formacie JSON, o ile nie została skonfigurowana inaczej.

Zapewnia to bardziej spójne środowisko w przeglądarkach podczas używania interfejsów API.

Aby skonfigurować aplikację do honorowania nagłówków akceptowanych przez przeglądarkę, ustaw wartość <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> na `true`:

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end

### <a name="configure-formatters"></a>Konfigurowanie elementów formatujących

Aplikacje, które muszą obsługiwać dodatkowe formaty, mogą dodać odpowiednie pakiety NuGet i skonfigurować obsługę. Istnieją osobne elementy formatujące dla danych wejściowych i wyjściowych. Wejściowe elementy formatującego są używane przez [powiązanie modelu](xref:mvc/models/model-binding). Wyjściowe elementy formatujące są używane do formatowania odpowiedzi. Aby uzyskać informacje na temat tworzenia niestandardowego programu formatującego, zobacz [niestandardowe elementy formatujące](xref:web-api/advanced/custom-formatters).

::: moniker range=">= aspnetcore-3.0"

### <a name="add-xml-format-support"></a>Dodawanie obsługi formatu XML

Elementy formatujące XML zaimplementowane przy użyciu <xref:System.Xml.Serialization.XmlSerializer> są konfigurowane przez wywołanie <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:

[!code-csharp[](./formatting/3.0sample/Startup.cs?name=snippet)]

Poprzedni kod serializacji wyników przy użyciu `XmlSerializer`.

W przypadku korzystania z powyższego kodu metody kontrolera powinny zwrócić odpowiedni format na podstawie nagłówka `Accept` żądania.

### <a name="configure-systemtextjson-based-formatters"></a>Skonfiguruj elementy formatujące system. Text. JSON w oparciu o

Funkcje dla @no__t programu formatującego oparte na -0 można skonfigurować przy użyciu `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.SerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

Opcje serializacji danych wyjściowych dla poszczególnych akcji można skonfigurować przy użyciu `JsonResult`. Na przykład:

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerOptions
    {
        options.WriteIndented = true,
    });
}
```

### <a name="add-newtonsoftjson-based-json-format-support"></a>Dodawanie obsługi formatu JSON opartego na Newtonsoft. JSON

Przed ASP.NET Core 3,0, domyślne używane elementy formatujące JSON zaimplementowane przy użyciu pakietu `Newtonsoft.Json`. W ASP.NET Core 3,0 lub nowszych domyślne elementy formatujące JSON są oparte na `System.Text.Json`. Obsługa programów formatującego i funkcji opartych na `Newtonsoft.Json` jest dostępna przez zainstalowanie pakietu NuGet [Microsoft. AspNetCore. MVC. NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) i skonfigurowanie go w `Startup.ConfigureServices`.

[!code-csharp[](./formatting/3.0sample/StartupNewtonsoftJson.cs?name=snippet)]

Niektóre funkcje mogą nie współpracować z @no__tami programu formatującego opartymi na -0 i wymagają odwołania do programu formatującego @no__t opartego na -1. Kontynuuj korzystanie z @no__t programu formatującego opartych na -0, jeśli aplikacja:

* Używa atrybutów `Newtonsoft.Json`. Na przykład: `[JsonProperty]` lub `[JsonIgnore]`.
* Dostosowuje ustawienia serializacji.
* Opiera się na funkcjach, które są dostępne `Newtonsoft.Json`.
* Konfiguruje `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`. Przed ASP.NET Core 3,0 `JsonResult.SerializerSettings` akceptuje wystąpienie `JsonSerializerSettings`, które jest specyficzne dla `Newtonsoft.Json`.
* Generuje dokumentację [openapi](<xref:tutorials/web-api-help-pages-using-swagger>) .

Funkcje dla @no__t programu formatującego oparte na -0 można skonfigurować przy użyciu `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefaultContractResolver();

    // Configure a custom converter
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

Opcje serializacji danych wyjściowych dla poszczególnych akcji można skonfigurować przy użyciu `JsonResult`. Na przykład:

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerSettings
    {
        options.Formatting = Formatting.Indented,
    });
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

### <a name="add-xml-format-support"></a>Dodawanie obsługi formatu XML

Formatowanie XML wymaga pakietu NuGet [Microsoft. AspNetCore. MVC. formatującegos. XML](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) .

Elementy formatujące XML zaimplementowane przy użyciu <xref:System.Xml.Serialization.XmlSerializer> są konfigurowane przez wywołanie <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet)]

Poprzedni kod serializacji wyników przy użyciu `XmlSerializer`.

W przypadku korzystania z powyższego kodu metody kontrolera powinny zwrócić odpowiedni format na podstawie nagłówka `Accept` żądania.

::: moniker-end

### <a name="specify-a-format"></a>Określ format

Aby ograniczyć formaty odpowiedzi, Zastosuj filtr [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) . Podobnie jak większość [filtrów](xref:mvc/controllers/filters), `[Produces]` można zastosować do akcji, kontrolera lub zakresu globalnego:

[!code-csharp[](./formatting/3.0sample/Controllers/WeatherForecastController.cs?name=snippet)]

Poprzedni filtr [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) :

* Wymusza, aby wszystkie akcje w ramach kontrolera zwracały odpowiedzi w formacie JSON.
* Jeśli inne elementy formatujące są skonfigurowane, a klient określi inny format, zwracany jest kod JSON.

Aby uzyskać więcej informacji, zobacz [filtry](xref:mvc/controllers/filters).

### <a name="special-case-formatters"></a>Specjalne elementy formatujące Case

Niektóre specjalne przypadki są implementowane przy użyciu wbudowanych elementów formatujących. Domyślnie typy zwracane `string` są formatowane jako *tekst/zwykły* (*tekst/HTML* , jeśli żąda się za pośrednictwem nagłówka `Accept`). To zachowanie można usunąć, usuwając <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter>. Elementy formatujące są usuwane w metodzie `Configure`. Akcje z typem zwracanym obiektu modelu zwracają `204 No Content` podczas zwracania `null`. To zachowanie można usunąć, usuwając <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>. Poniższy kod usuwa `TextOutputFormatter` i `HttpNoContentOutputFormatter`.

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupTextOutputFormatter.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupTextOutputFormatter.cs?name=snippet)]
::: moniker-end

Bez `TextOutputFormatter` typy zwracane `string` zwracają `406 Not Acceptable`. Jeśli element formatujący XML istnieje, formatuje `string` zwracanych typów, jeśli `TextOutputFormatter` zostanie usunięty.

Bez `HttpNoContentOutputFormatter` obiekty o wartości null są formatowane przy użyciu skonfigurowanego programu formatującego. Na przykład:

* Program formatujący JSON zwraca odpowiedź z treścią `null`.
* Program formatujący XML zwraca pusty element XML z atrybutem `xsi:nil="true"` Set.

## <a name="response-format-url-mappings"></a>Mapowania adresów URL w formacie odpowiedzi

Klienci mogą zażądać określonego formatu w ramach adresu URL, na przykład:

* W ciągu zapytania lub części ścieżki.
* Przy użyciu rozszerzenia pliku specyficznego dla formatu, takiego jak. XML lub. JSON.

Mapowanie ze ścieżki żądania należy określić w marszrucie używanej przez interfejs API. Na przykład:

[!code-csharp[](./formatting/sample/Controllers/ProductsController.cs?name=snippet)]

Poprzednia trasa pozwala określić żądany format jako opcjonalne rozszerzenie pliku. Atrybut [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) sprawdza obecność wartości format w `RouteData` i mapuje format odpowiedzi do odpowiedniego programu formatującego podczas tworzenia odpowiedzi.

|           Trasa        |             EQ              |
|------------------------|------------------------------------|
|   `/api/products/5`    |    Domyślny program formatujący dane wyjściowe    |
| `/api/products/5.json` | Program formatujący JSON (jeśli jest skonfigurowany) |
| `/api/products/5.xml`  | Program formatujący XML (jeśli jest skonfigurowany)  |
