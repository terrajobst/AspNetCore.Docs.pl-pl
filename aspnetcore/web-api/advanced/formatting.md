---
title: Formatowanie danych odpowiedzi w ASP.NET Core Web API
author: ardalis
description: Dowiedz się, jak sformatować dane odpowiedzi w ASP.NET Core Web API.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 05/29/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: e3417c9bfd3824133b86de2fe74f5f71367e1560
ms.sourcegitcommit: 41f2c1a6b316e6e368a4fd27a8b18d157cef91e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886533"
---
<!-- DO NOT EDIT BEFORE https://github.com/aspnet/AspNetCore.Docs/pull/12077 MERGES -->
# <a name="format-response-data-in-aspnet-core-web-api"></a>Formatowanie danych odpowiedzi w ASP.NET Core Web API

Przez [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC ma wbudowaną obsługę formatowania danych odpowiedzi przy użyciu stałych formatów lub w odpowiedzi na specyfikacje klienta.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="format-specific-action-results"></a>Wyniki akcji specyficzne dla formatu

Niektóre typy wyników akcji są specyficzne dla określonego formatu, takiego jak `JsonResult` i. `ContentResult` Akcje mogą zwracać określone wyniki, które są zawsze sformatowane w określony sposób. Na przykład zwrócenie danych w `JsonResult` formacie JSON będzie zwracało, niezależnie od preferencji klienta. Analogicznie, zwrócenie `ContentResult` zwraca dane ciągu w formacie zwykłego tekstu (jak po prostu zwraca ciąg).

> [!NOTE]
> Akcja nie jest wymagana do zwrócenia żadnego określonego typu; MVC obsługuje dowolną wartość zwracaną przez obiekt. Jeśli akcja zwróci `IActionResult` implementację, a kontroler dziedziczy po `Controller`, deweloperzy mają wiele metod pomocniczych odpowiadających wielu wyborom. Wyniki akcji, które zwracają obiekty, które nie `IActionResult` są typami, będą serializowane przy `IOutputFormatter` użyciu odpowiedniej implementacji.

Aby zwrócić dane w określonym formacie z kontrolera, który dziedziczy z `Controller` klasy podstawowej, użyj wbudowanej metody `Json` pomocnika do zwracania kodu JSON i `Content` dla zwykłego tekstu. Metoda działania powinna zwracać określony typ wyniku (na przykład `JsonResult`) lub. `IActionResult`

Zwracanie danych w formacie JSON:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

Przykładowa odpowiedź z tej akcji:

![Karta sieciowa Narzędzia deweloperskie w przeglądarce Microsoft Edge pokazująca typ zawartości odpowiedzi to Application/JSON](formatting/_static/json-response.png)

Należy pamiętać, że typ zawartości odpowiedzi jest `application/json`pokazywany zarówno na liście żądań sieciowych, jak i w sekcji nagłówki odpowiedzi. Należy również zwrócić uwagę na listę opcji przedstawionych w przeglądarce (w tym przypadku Microsoft Edge) w nagłówku Accept w sekcji nagłówki żądania. Bieżąca technika powoduje ignorowanie tego nagłówka. przestrzeganie tych zagadnień jest omówione poniżej.

Aby zwrócić dane w formacie zwykłego tekstu `ContentResult` , użyj `Content` i pomocnika:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

Odpowiedź z tej akcji:

![Karta sieciowa Narzędzia deweloperskie w przeglądarce Microsoft Edge pokazująca typ zawartości odpowiedzi to Text/Plains](formatting/_static/text-response.png)

Zwróć uwagę, że w `Content-Type` tym przypadku `text/plain`zwracana jest wartość. Możesz również osiągnąć takie samo zachowanie, używając tylko typu odpowiedzi ciągu:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> Dla nieuproszczonych akcji z wieloma zwracanymi typami lub opcjami (na przykład innych kodów stanu HTTP w oparciu o wynik wykonanych operacji) Preferuj `IActionResult` jako zwracany typ.

## <a name="content-negotiation"></a>Negocjowanie zawartości

Negocjowanie zawartości (*conneg* for Short) występuje, gdy klient określi [nagłówek Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html). Domyślny format używany przez ASP.NET Core MVC to JSON. Negocjowanie zawartości jest implementowane przez `ObjectResult`program. Jest ona również wbudowana w wyniki akcji specyficzne dla kodu stanu zwracane z metod pomocnika (wszystkie są oparte na `ObjectResult`). Możesz również zwrócić Typ modelu (klasy zdefiniowanej jako typ transferu danych), a platforma automatycznie zawija ją w `ObjectResult` wybranym przez siebie sposób.

W poniższej metodzie działania są `Ok` stosowane `NotFound` metody pomocnika i:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

Odpowiedź w formacie JSON zostanie zwrócona, chyba że zażądano innego formatu i serwer może zwrócić żądany format. Za pomocą narzędzia, takiego jak [programu Fiddler](https://www.telerik.com/fiddler) , można utworzyć żądanie zawierające nagłówek Accept i określić inny format. W takim przypadku, jeśli serwer zawiera program *formatujący* , który może wygenerować odpowiedź w żądanym formacie, wynik zostanie zwrócony w formacie preferowanym przez klienta.

![Konsola programu Fiddler pokazująca ręcznie utworzone żądanie GET z wartością nagłówka Accept dla aplikacji/XML](formatting/_static/fiddler-composer.png)

Na powyższym zrzucie ekranu układacz programu Fiddler został użyty do wygenerowania żądania, określając `Accept: application/xml`. Domyślnie ASP.NET Core MVC obsługuje tylko kod JSON, więc nawet jeśli określono inny format, zwracany wynik jest nadal sformatowany w formacie JSON. Zobaczysz, jak dodać dodatkowe elementy formatujące w następnej sekcji.

Akcje kontrolera mogą zwracać POCOs (zwykłe stare obiekty CLR), w tym przypadku ASP.NET Core MVC automatycznie tworzy `ObjectResult` dla Ciebie obiekt. Klient pobierze sformatowany obiekt serializowany (format JSON jest domyślny; można skonfigurować plik XML lub inne formaty). Jeśli zwracany obiekt to `null`, struktura `204 No Content` zwróci odpowiedź.

Zwracanie typu obiektu:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

W przykładzie żądanie dotyczące prawidłowego aliasu autora otrzyma odpowiedź 200 OK od danych autora. Żądanie dotyczące nieprawidłowego aliasu otrzyma odpowiedź 204 braku zawartości. Poniżej przedstawiono zrzuty ekranu przedstawiające odpowiedź w formatach XML i JSON.

### <a name="content-negotiation-process"></a>Proces negocjacji zawartości

*Negocjowanie* zawartości odbywa się tylko wtedy `Accept` , gdy w żądaniu zostanie wyświetlony nagłówek. Gdy żądanie zawiera nagłówek Accept, struktura będzie wyliczać typy nośników w nagłówku Accept w kolejności preferencji i spróbuje znaleźć program formatujący, który może wygenerować odpowiedź w jednym z formatów określonych przez nagłówek Accept. W przypadku, gdy nie znaleziono programu formatującego, który może spełnić żądanie klienta, platforma podejmie próbę znalezienia pierwszego elementu formatującego, który może wygenerować odpowiedź (chyba że deweloper skonfigurował opcję w `MvcOptions` celu zwrócenia 406). Jeśli żądanie określa XML, ale nie skonfigurowano programu formatującego XML, zostanie użyty program formatujący JSON. Ogólnie rzecz biorąc, jeśli nie skonfigurowano programu formatującego, który może zapewnić żądany format, zostanie użyty pierwszy program formatujący, który może formatować obiekt. Jeśli nagłówek nie zostanie określony, pierwszy program formatujący, który może obsłużyć zwracany obiekt, zostanie użyty do serializacji odpowiedzi. W takim przypadku nie ma żadnej negocjacji — serwer określa format, który będzie używany.

> [!NOTE]
> Jeśli nagłówek Accept zawiera `*/*`, nagłówek będzie ignorowany, chyba że `RespectBrowserAcceptHeader` `MvcOptions`jest ustawiona na wartość true.

### <a name="browsers-and-content-negotiation"></a>Przeglądarki i negocjacje zawartości

W przeciwieństwie do typowych klientów interfejsu API, przeglądarki sieci `Accept` Web zastępują do dostarczania nagłówków zawierających szeroką gamę formatów, w tym symboli wieloznacznych. Domyślnie, gdy struktura wykryje, że żądanie pochodzi z przeglądarki, zignoruje `Accept` nagłówek i zamiast tego zwróci zawartość w skonfigurowanym formacie domyślnym (JSON, chyba że zostanie skonfigurowany inaczej). Zapewnia to bardziej spójny interfejs, który umożliwia korzystanie z interfejsów API w różnych przeglądarkach.

Jeśli wolisz, aby `RespectBrowserAcceptHeader` `true` aplikacja honorować nagłówki akceptujące w przeglądarce, możesz ją skonfigurować jako część konfiguracji MVC, ustawiając wartość w `ConfigureServices` metodzie w *Startup.cs*.

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a>Konfigurowanie elementów formatujących

Jeśli aplikacja wymaga obsługi dodatkowych formatów poza wartością domyślną JSON, można dodać pakiety NuGet i skonfigurować MVC do ich obsługi. Istnieją osobne elementy formatujące dla danych wejściowych i wyjściowych. Wejściowe elementy formatującego są używane przez [powiązanie modelu](xref:mvc/models/model-binding); wyjściowe elementy formatujące są używane do formatowania odpowiedzi. Możesz również skonfigurować [niestandardowe](xref:web-api/advanced/custom-formatters)elementy formatujące.

::: moniker range=">= aspnetcore-3.0"

### <a name="configure-systemtextjson-based-formatters"></a>Skonfiguruj elementy formatujące system. Text. JSON w oparciu o

Funkcje dla `System.Text.Json`elementów formatujących opartych na programie można skonfigurować `Microsoft.AspNetCore.Mvc.MvcOptions.SerializerOptions`przy użyciu polecenia.

```csharp
services.AddMvc(options =>
{
    options.SerializerOptions.WriterSettings.Indented = true;
});
```

### <a name="add-newtonsoftjson-based-json-format-support"></a>Dodawanie obsługi formatu JSON opartego na Newtonsoft. JSON

Przed ASP.NET Core 3,0, MVC domyślnie korzysta z elementów formatujących JSON wdrożonych przy użyciu `Newtonsoft.Json` pakietu. W ASP.NET Core 3,0 lub nowszych domyślne elementy formatujące JSON są oparte na `System.Text.Json`. Obsługa programów formatującego i funkcji opartych na programie jest dostępna przez zainstalowanie pakietu NuGet [Microsoft. AspNetCore. MVC. NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) i skonfigurowanie go w `Startup.ConfigureServices`programie. `Newtonsoft.Json`

```csharp
services.AddMvc()
    .AddNewtonsoftJson();
```

Niektóre funkcje mogą nie współdziałać `System.Text.Json`z modułami formatującego opartymi na systemie i wymagają `Newtonsoft.Json`odwołania do elementów formatujących opartych na danych dla ASP.NET Core 3,0. Kontynuuj korzystanie z `Newtonsoft.Json`programu formatującego w oparciu o, jeśli Twoja aplikacja ASP.NET Core 3,0 lub nowsza:

* Używa `Newtonsoft.Json` atrybutów (na `[JsonProperty]` przykład lub `[JsonIgnore]`), dostosowuje `Newtonsoft.Json` ustawienia serializacji lub opiera się na udostępnianych funkcjach.
* Konfiguruje `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`. Przed ASP.NET Core 3,0, `JsonResult.SerializerSettings` akceptuje `JsonSerializerSettings` wystąpienie, które jest specyficzne dla `Newtonsoft.Json`.
* Generuje dokumentację [openapi](<xref:tutorials/web-api-help-pages-using-swagger>) .

::: moniker-end

### <a name="add-xml-format-support"></a>Dodawanie obsługi formatu XML

::: moniker range="<= aspnetcore-2.2"

Aby dodać obsługę formatowania XML w ASP.NET Core 2,2 lub starszej, zainstaluj pakiet NuGet [Microsoft. AspNetCore. MVC. formatującegos. XML](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) .

::: moniker-end

Elementy formatujące XML zaimplementowane `System.Xml.Serialization.XmlSerializer` przy użyciu można skonfigurować, <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> wywołując `Startup.ConfigureServices`w:

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

Alternatywnie można skonfigurować elementy formatujące XML `System.Runtime.Serialization.DataContractSerializer` zaimplementowane przy użyciu, wywołując <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlDataContractSerializerFormatters*> w `Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .AddXmlDataContractSerializerFormatters();
```

Po dodaniu obsługi formatowania XML, metody kontrolera powinny zwrócić odpowiedni format na podstawie `Accept` nagłówka żądania, ponieważ przykład programu Fiddler ilustruje:

![Programu Fiddler: Karta RAW dla żądania zawiera wartość nagłówka Accept to Application/XML. Karta RAW odpowiedzi zawiera wartość nagłówka Content-Type dla aplikacji/XML.](formatting/_static/xml-response.png)

Na karcie inspektorzy można zobaczyć, że nieprzetworzone żądanie Get zostało wykonane z `Accept: application/xml` zestawem nagłówków. Okienko odpowiedzi zawiera `Content-Type: application/xml` nagłówek, `Author` a obiekt został Zserializowany do formatu XML.

Użyj karty układacz, aby zmodyfikować żądanie do określenia `application/json` `Accept` w nagłówku. Wykonaj żądanie, a odpowiedź zostanie sformatowana w formacie JSON:

![Programu Fiddler: Karta RAW dla żądania pokazuje, że wartość nagłówka Accept to Application/JSON. Karta RAW odpowiedzi zawiera wartość nagłówka Content-Type dla elementu Application/JSON.](formatting/_static/json-response-fiddler.png)

Na tym zrzucie ekranu można zobaczyć, czy żądanie ustawia nagłówek `Accept: application/json` , a odpowiedź określa takie samo, jak jego. `Content-Type` `Author` Obiekt jest wyświetlany w treści odpowiedzi w formacie JSON.

### <a name="forcing-a-particular-format"></a>Wymuszanie określonego formatu

Jeśli chcesz ograniczyć formaty odpowiedzi dla konkretnej akcji, możesz zastosować `[Produces]` filtr. `[Produces]` Filtr określa formaty odpowiedzi dla określonej akcji (lub kontrolera). Podobnie jak większość [filtrów](xref:mvc/controllers/filters), można to zastosować w ramach akcji, kontrolera lub zakresu globalnego.

```csharp
[Produces("application/json")]
public class AuthorsController
```

Filtr wymusi, że `AuthorsController` wszystkie akcje w programie mają zwracać odpowiedzi w formacie JSON, nawet jeśli dla aplikacji skonfigurowano inne elementy formatujące, `Accept` a klient dostarczył nagłówek z żądaniem innego, dostępnego `[Produces]` Formatowanie. Zobacz [filtry](xref:mvc/controllers/filters) , aby dowiedzieć się więcej, w tym, jak zastosować filtry globalnie.

### <a name="special-case-formatters"></a>Specjalne elementy formatujące Case

Niektóre specjalne przypadki są implementowane przy użyciu wbudowanych elementów formatujących. Domyślnie `string` typy zwracane będą formatowane jako *tekst/zwykły* (*tekst/HTML* , jeśli żądano za `Accept` pośrednictwem nagłówka). To zachowanie można usunąć, usuwając `TextOutputFormatter`. Elementy formatującego można usunąć w `Configure` metodzie w *Startup.cs* (pokazanej poniżej). Akcje, które mają typ zwracany obiektu modelu zwróci 204 braku odpowiedzi zawartości podczas powrotu `null`. To zachowanie można usunąć, usuwając `HttpNoContentOutputFormatter`. Poniższy kod usuwa `TextOutputFormatter` i `HttpNoContentOutputFormatter`.

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

`TextOutputFormatter`Bez zwracanychtypówzwraca406nieakceptowalny`string` , na przykład. Należy pamiętać, że jeśli element formatujący XML istnieje, `string` format zwracanych typów `TextOutputFormatter` , jeśli zostanie usunięty.

Bez obiektów `HttpNoContentOutputFormatter`o wartości null są formatowane przy użyciu skonfigurowanego programu formatującego. Na przykład, program formatujący JSON po prostu zwróci odpowiedź z treścią `null`, podczas gdy program formatujący XML zwróci pusty element XML z zestawem atrybutów. `xsi:nil="true"`

## <a name="response-format-url-mappings"></a>Mapowania adresów URL w formacie odpowiedzi

Klienci mogą zażądać określonego formatu jako części adresu URL, takiego jak ciąg zapytania lub część ścieżki lub przy użyciu rozszerzenia pliku specyficznego dla formatu, takiego jak. XML lub. JSON. Mapowanie ze ścieżki żądania należy określić w marszrucie używanej przez interfejs API. Na przykład:

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

Ta trasa zezwala na określenie żądanego formatu jako opcjonalnego rozszerzenia pliku. Atrybut sprawdza obecność wartości format `RouteData` w i mapuje format odpowiedzi do odpowiedniego programu formatującego podczas tworzenia odpowiedzi. `[FormatFilter]`

|           Szlak            |             EQ              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    Domyślny program formatujący dane wyjściowe    |
| `/products/GetById/5.json` | Program formatujący JSON (jeśli jest skonfigurowany) |
| `/products/GetById/5.xml`  | Program formatujący XML (jeśli jest skonfigurowany)  |
