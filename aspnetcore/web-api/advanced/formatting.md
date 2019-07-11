---
title: Formatowanie danych odpowiedzi w interfejsie API sieci Web platformy ASP.NET Core
author: ardalis
description: Dowiedz się, jak sformatować dane odpowiedzi w interfejsie API sieci Web platformy ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 05/29/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: b050011aa38743353fb2a7d133abcdca0b8c6d33
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814816"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a>Formatowanie danych odpowiedzi w interfejsie API sieci Web platformy ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

Platforma ASP.NET Core MVC ma wbudowaną funkcję formatowanie danych odpowiedzi, przy użyciu formatów stałej lub w odpowiedzi do klienta specyfikacji.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="format-specific-action-results"></a>Wyniki specyficzne dla formatu akcji

Niektóre typy wyników akcji są specyficzne dla danego formatu, takie jak `JsonResult` i `ContentResult`. Akcje może zwrócić określone wyniki, które są zawsze formatowane w szczególności sposób. Na przykład, zwracając `JsonResult` zwróci dane w formacie JSON, niezależnie od preferencji klienta. Podobnie, zwracając `ContentResult` zwróci dane ciągu w formacie tekstu zwykłego (tak jak będzie po prostu zwracanie ciągu).

> [!NOTE]
> Akcja nie jest wymagane do zwrócenia określonego typu; MVC obsługuje dowolną wartość zwracaną obiektu. Jeśli działanie zwróci `IActionResult` implementacji i kontroler dziedziczy `Controller`, deweloperzy mają wiele metod pomocniczych odpowiadający wiele opcji. Wyniki z akcji, które zwracają obiekty, które nie są `IActionResult` typy będzie serializowana za pomocą odpowiedniego `IOutputFormatter` implementacji.

Aby zwrócić dane w określonym formacie z kontrolerem, która dziedziczy `Controller` klasy bazowej, należy użyć metody wbudowanej Pomocy `Json` do zwracają kod JSON i `Content` zwykłego tekstu. Metoda akcji powinna zwrócić typ określony wynik (na przykład `JsonResult`) lub `IActionResult`.

Zwracanie danych w formacie JSON:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

Przykładowa odpowiedź z tej akcji:

![Karta Sieć narzędzi dla deweloperów w programie Microsoft Edge wyświetlanie typ zawartości odpowiedzi to application/json](formatting/_static/json-response.png)

Należy zauważyć, że typ zawartości odpowiedzi `application/json`, jak pokazano na liście żądań sieciowych i w sekcji nagłówków odpowiedzi. Należy również zauważyć listę opcji dostępnych w przeglądarce (w tym przypadku Microsoft Edge) w nagłówku Accept, w sekcji nagłówkami żądań. Bieżącą metodę ignoruje tego pliku nagłówkowego; obeying go omówiono poniżej.

Aby zwrócić dane w formacie zwykłego tekstu, należy użyć `ContentResult` i `Content` pomocy:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

Odpowiedź z tej akcji:

![Karta Sieć narzędzi dla deweloperów w programie Microsoft Edge wyświetlanie typ zawartości odpowiedzi to zwykły tekst](formatting/_static/text-response.png)

Należy pamiętać, w tym przypadku `Content-Type` zwracany jest `text/plain`. Możesz również uzyskać to takie samo zachowanie za pomocą tylko parametry typu odpowiedzi:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> Nietrywialnymi akcji z wieloma zwraca typy lub opcje (na przykład różne kody stanu HTTP na podstawie wyniku operacji wykonywanych) tak, `IActionResult` jako typ zwracany.

## <a name="content-negotiation"></a>Negocjowanie zawartości

Negocjowanie zawartości (*conneg* w skrócie) występuje, gdy klient określa [nagłówek Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html). Domyślny format używany przez program ASP.NET Core MVC jest JSON. Negocjowanie zawartości jest implementowany przez `ObjectResult`. Również ma wbudowaną kod stanu określonej akcji wyniki zwrócone z metody pomocnicze (wszystkie opartych na `ObjectResult`). Może również zwracać typu modelu (klasy została zdefiniowana jako typ transferu danych) i struktura będzie automatycznie zawijany w `ObjectResult` dla Ciebie.

Używa następującej metody akcji `Ok` i `NotFound` metody pomocnicze:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

Odpowiedź w formacie JSON zostaną zwrócone, chyba że zażądano inny format, a serwer może zwrócić żądany format. Można użyć narzędzia, takiego jak [Fiddler](https://www.telerik.com/fiddler) utworzyć żądanie zawiera nagłówek Accept, i określ inny format. W tym przypadku, jeśli serwer ma *program formatujący* , może utworzyć odpowiedź w formacie żądanego, zostanie zwrócony wynik w formacie preferowanego klienta.

![Konsola programu fiddler pokazująca, że utworzone ręcznie pobrać żądania o wartości nagłówka Accept application/xml](formatting/_static/fiddler-composer.png)

Na powyższym zrzucie ekranu Fiddler Composer został użyty do wygenerowania żądania, określając `Accept: application/xml`. Domyślnie program ASP.NET Core MVC obsługuje tylko JSON, nawet jeśli inny format jest określony, zwracana jest nadal — w formacie JSON. Pokazano, jak dodać dodatkowe programy formatujące w następnej sekcji.

Akcji kontrolera może zwrócić POCOs (zwykłe stare obiekty CLR), w którym to przypadku ASP.NET Core MVC automatycznie tworzy `ObjectResult` dla Ciebie, która otacza obiekt. Klient otrzyma sformatowane Zserializowany obiekt (JSON format jest ustawieniem domyślnym; można skonfigurować, XML lub innych formatów). Jeśli jest obiekt zwrócony `null`, a następnie zwróci ramach `204 No Content` odpowiedzi.

Zwraca typ obiektu:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

W tym przykładzie żądania dla aliasu autora otrzyma odpowiedź 200 OK danymi autora. Żądanie dotyczące nieprawidłowy alias otrzyma 204, Brak zawartości odpowiedzi. Poniżej przedstawiono zrzuty ekranu pokazujące odpowiedzi w formatach XML i JSON.

### <a name="content-negotiation-process"></a>Proces negocjowania zawartości

Zawartość *negocjacji* tylko ma miejsce, jeśli `Accept` nagłówek pojawia się w żądaniu. Jeśli żądanie zawiera nagłówek accept, struktura zostanie wyliczyć typów nośników w nagłówku accept w kolejności preferencji i spróbuje znaleźć element formatujący, który może utworzyć odpowiedź w jednym z formatów określony w nagłówku accept. W przypadku, gdy element formatujący nie zostanie znaleziony, która może spełnić żądanie klienta, struktura spróbuje znaleźć pierwszy element formatujący, który może utworzyć odpowiedź (chyba że deweloper została skonfigurowana opcja w `MvcOptions` do zwrócenia 406 nie do przyjęcia zamiast). Jeśli żądanie określa XML, ale nie skonfigurowano program formatujący kod XML, JSON program formatujący będą używane. Ogólnie rzecz biorąc Jeśli element formatujący nie jest skonfigurowany, które oferują żądany format, a następnie pierwszy element formatujący, który można sformatować obiekt jest używany. Jeśli podano bez nagłówka, pierwszy element formatujący, który może obsłużyć obiekt, który ma zostać zwrócona będzie służyć do serializacji w odpowiedzi. W takim przypadku nie ma żadnych miejsce negocjacji — serwer jest określenie jakiego formatu, zostanie użyty.

> [!NOTE]
> Jeśli zawiera nagłówek Accept `*/*`, nagłówka zostanie zignorowany, chyba że `RespectBrowserAcceptHeader` jest ustawiona na wartość true na `MvcOptions`.

### <a name="browsers-and-content-negotiation"></a>Przeglądarki i negocjacji zawartości

W odróżnieniu od typowych interfejsów API klienci, przeglądarki sieci web mają tendencję do dostarczania `Accept` nagłówków zawierających szerokiej gamy formatów, łącznie z symbolami wieloznacznymi. Domyślnie, gdy struktura wykryje, czy żądanie pochodzi z przeglądarki, jego zignoruje `Accept` nagłówka i zamiast tego zwracany zawartości w aplikacji skonfigurowany przez domyślny format (JSON, chyba że skonfigurowano inaczej). Zapewnia bardziej spójne środowisko, korzystając z różnych przeglądarek korzystanie z interfejsów API.

Jeśli wolisz nagłówków accept przeglądarce honor aplikacji można skonfigurować to w ramach konfiguracji MVC, ustawiając `RespectBrowserAcceptHeader` do `true` w `ConfigureServices` method in Class metoda *Startup.cs*.

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a>Konfigurowanie elementy formatujące

Jeśli aplikacja wymaga do obsługi dodatkowych formatach poza domyślnym formacie JSON, można dodać pakietów NuGet i skonfigurować MVC do ich obsługi. Istnieją osobne elementy formatujące dane wejściowe i wyjściowe. Elementy formatujące wejściowe są używane przez [powiązań modelu](xref:mvc/models/model-binding); elementy formatujące dane wyjściowe są używane do formatowania odpowiedzi. Można również skonfigurować [niestandardowe elementy formatujące](xref:web-api/advanced/custom-formatters).

::: moniker range=">= aspnetcore-3.0"

### <a name="configure-systemtextjson-based-formatters"></a>Skonfiguruj na podstawie System.Text.Json elementy formatujące

Funkcje dla `System.Text.Json`— oparte na elementy formatujące można skonfigurować za pomocą `Microsoft.AspNetCore.Mvc.MvcOptions.SerializerOptions`.

```csharp
services.AddMvc(options =>
{
    options.SerializerOptions.WriterSettings.Indented = true;
});
```

### <a name="add-newtonsoftjson-based-json-format-support"></a>Dodanie obsługi formatu JSON na podstawie Newtonsoft.Json

Przed platformy ASP.NET Core 3.0 to MVC ustawiana domyślnie przy użyciu elementów formatujących JSON implementowane przy użyciu `Newtonsoft.Json` pakietu. W programie ASP.NET Core 3.0 lub nowszej, elementy formatujące JSON domyślne są oparte na `System.Text.Json`. Obsługa `Newtonsoft.Json`— funkcje i elementy formatujące zależności jest dostępna po zainstalowaniu [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet pakietu i skonfigurowanie go w `Startup.ConfigureServices`.

```csharp
services.AddMvc()
    .AddNewtonsoftJson();
```

Niektóre funkcje mogą nie działać poprawnie z `System.Text.Json`— na podstawie elementy formatujące i wymagają odwołania do `Newtonsoft.Json`— na podstawie elementy formatujące dla wersji platformy ASP.NET Core 3.0. Nadal korzystać z `Newtonsoft.Json`— na podstawie elementów formatujących, jeśli sieci Web platformy ASP.NET Core 3.0 lub nowszej aplikacji:

* Używa `Newtonsoft.Json` atrybutów (na przykład `[JsonProperty]` lub `[JsonIgnore]`), dostosowuje ustawienia serializacji lub opiera się na funkcji `Newtonsoft.Json` udostępnia.
* Konfiguruje `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`. Przed platformy ASP.NET Core 3.0 to `JsonResult.SerializerSettings` akceptuje wystąpienie `JsonSerializerSettings` specyficznego `Newtonsoft.Json`.
* Generuje [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) dokumentacji.

::: moniker-end

### <a name="add-xml-format-support"></a>Dodanie obsługi formatu XML

::: moniker range="<= aspnetcore-2.2"

Aby dodać kod XML, formatowaniem obsługę platformy ASP.NET Core 2.2 lub wcześniej, należy zainstalować [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) pakietu NuGet.

::: moniker-end

Programy formatujące XML implementowane przy użyciu `System.Xml.Serialization.XmlSerializer` mogą być konfigurowane przez wywołanie <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> w `Startup.ConfigureServices`:

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

Alternatywnie XML elementy formatujące implementowane przy użyciu `System.Runtime.Serialization.DataContractSerializer` mogą być konfigurowane przez wywołanie <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlDataContractSerializerFormatters*> w `Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .AddXmlDataContractSerializerFormatters();
```

Po dodaniu obsługi formatowania XML metody kontroler powinien zwrócić odpowiedni format na podstawie żądania `Accept` nagłówka, jak Fiddler, ten przykład pokazuje:

![Konsola programu fiddler: Nieprzetworzone kartę dla żądania pokazuje wartość nagłówka Accept jest aplikacja/xml. Nieprzetworzone kartę odpowiedzi zawiera wartość nagłówka Content-Type application/XML.](formatting/_static/xml-response.png)

Widoczne na karcie inspektorzy, która pierwotne Pobierz żądanie zostało wystosowane z `Accept: application/xml` zestaw nagłówka. Przedstawia okienko odpowiedzi `Content-Type: application/xml` nagłówka, a `Author` obiekt został serializowany do XML.

Użyj karty Composer (kompozytor), aby zmodyfikować żądanie, aby określić `application/json` w `Accept` nagłówka. Wykonanie żądania i odpowiedzi będą formatowane jako dane JSON:

![Konsola programu fiddler: Nieprzetworzone kartę dla żądania pokazuje wartość nagłówka Accept jest application/json. Nieprzetworzone kartę odpowiedzi zawiera wartość nagłówka Content-Type application/JSON.](formatting/_static/json-response-fiddler.png)

W tym zrzucie ekranu widać, żądanie ustawia nagłówek `Accept: application/json` i określa odpowiedź, taka sama jak jego `Content-Type`. `Author` Obiektu jest wyświetlany w treści odpowiedzi w formacie JSON.

### <a name="forcing-a-particular-format"></a>Wymuszanie określonym formacie

Jeśli chcesz ograniczyć formaty odpowiedzi dla określonej akcji można, można zastosować `[Produces]` filtru. `[Produces]` Filtr Określa format odpowiedzi dla określonej akcji (lub kontroler). Większość, takich jak [filtry](xref:mvc/controllers/filters), to można zastosować na działania, kontrolera lub zakresie globalnym.

```csharp
[Produces("application/json")]
public class AuthorsController
```

`[Produces]` Filtru spowoduje to wymuszenie wszystkich akcji w ramach `AuthorsController` do zwrócenia odpowiedzi w formacie JSON, nawet jeśli inne programy formatujące zostały skonfigurowane na potrzeby aplikacji i klienta, pod warunkiem `Accept` nagłówka żądania różne, dostępne Format. Zobacz [filtry](xref:mvc/controllers/filters) Aby uzyskać więcej informacji, w tym sposobu stosowania filtrów globalnie.

### <a name="special-case-formatters"></a>Specjalnych przypadków elementy formatujące

Niektóre szczególne przypadki są implementowane za pomocą wbudowanych elementy formatujące. Domyślnie `string` zwracane typy, które będą formatowane jako *text/plain* (*text/html* na żądanie za pośrednictwem `Accept` nagłówek). To zachowanie można usunąć przez usunięcie `TextOutputFormatter`. Usuń elementy formatujące w `Configure` method in Class metoda *Startup.cs* (pokazana poniżej). Akcje, które mają obiektu modelu zwracać typ zwróci 204 zawartości odpowiedzi podczas zwracania `null`. To zachowanie można usunąć przez usunięcie `HttpNoContentOutputFormatter`. Poniższy kod usuwa `TextOutputFormatter` i `HttpNoContentOutputFormatter`.

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

Bez `TextOutputFormatter`, `string` zwraca typy zwracają 406 nie do przyjęcia, na przykład. Należy pamiętać, że jeśli element formatujący XML istnieje, jej sformatuje `string` typy zwracane, jeśli `TextOutputFormatter` zostanie usunięty.

Bez `HttpNoContentOutputFormatter`, obiektów o wartości null są formatowane przy użyciu skonfigurowanego elementu formatującego. Na przykład program formatujący JSON po prostu zwróci odpowiedź z treści pewnego `null`, podczas gdy program formatujący kod XML zwróci puste XML element z atrybutem `xsi:nil="true"` zestawu.

## <a name="response-format-url-mappings"></a>Mapowania adresów URL Format odpowiedzi

Klienci mogą żądać określonym formacie jako część adresu URL, takich jak w ciągu zapytania lub część ścieżki lub przy użyciu rozszerzenia specyficzne dla formatu plików, takich jak XML lub JSON. Powinny być określone mapowanie ścieżki żądania w trasy, która używa interfejsu API. Na przykład:

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

Ta trasa pozwoliłoby żądany format można określić jako rozszerzenie pliku opcjonalne. `[FormatFilter]` Atrybut sprawdza istnienie wartości formatu `RouteData` i będzie zmapowana format odpowiedzi do odpowiedniego obiektu formatującego, po utworzeniu odpowiedzi.

|           trasy            |             Element formatujący              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    Domyślny element formatujący dane wyjściowe    |
| `/products/GetById/5.json` | Element formatujący JSON (jeśli jest skonfigurowane) |
| `/products/GetById/5.xml`  | Element formatujący XML (jeśli jest skonfigurowane)  |
