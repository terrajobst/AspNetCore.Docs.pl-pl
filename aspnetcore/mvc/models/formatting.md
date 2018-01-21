---
title: Formatowanie danych odpowiedzi na platformie ASP.NET Core MVC
author: ardalis
description: "Dowiedz się, jak sformatować dane odpowiedzi na platformie ASP.NET Core MVC."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 85398928164e75ec27c91870f03ee1c81725e080
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a>Wprowadzenie do formatowania danych odpowiedzi na platformie ASP.NET Core MVC

Przez [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC ma wbudowaną obsługę formatowania danych odpowiedzi, za pomocą stałych formatów lub w odpowiedzi do klienta specyfikacji.

[Wyświetlanie lub pobieranie próbki z usługi GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).

## <a name="format-specific-action-results"></a>Wyniki akcji specyficzne dla formatu

Niektóre typy wyników akcji są specyficzne dla określonego formatu, takie jak `JsonResult` i `ContentResult`. Akcje może zwrócić określone wyniki, które są zawsze formatowane w określonym czasie. Na przykład zwracanie `JsonResult` zwróci dane w formacie JSON, niezależnie od preferencje dotyczące klienta. Podobnie, zwracając `ContentResult` zwraca ciąg w formacie tekstu zwykłego danych (podobnie jak po prostu zwraca ciąg).

> [!NOTE]
> Akcja nie jest wymagane, aby zwrócić określonego typu; MVC obsługuje żadnej wartości zwracanych obiektów. Jeśli działanie zwróci `IActionResult` implementacji i kontrolera dziedziczy `Controller`, deweloperzy mają wiele metody pomocnicze odpowiednie do wielu opcji. Wyniki z akcji, które zwracają obiekty, które nie są `IActionResult` typy będzie można zserializować przy użyciu odpowiednich `IOutputFormatter` implementacji.

Aby zwrócić dane w określonym formacie z kontrolerem, która dziedziczy `Controller` klasy podstawowej, należy użyć metody pomocniczej wbudowanych `Json` do zwrócenia JSON i `Content` na zwykły tekst. Twoje metoda akcji powinna zwrócić typ określony wynik (na przykład `JsonResult`) lub `IActionResult`.

Zwraca dane w formacie JSON:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

Przykładowa odpowiedź z tej akcji:

![Karta sieci narzędzi dla deweloperów w programie Microsoft Edge przedstawiający typ zawartości odpowiedzi to application/json](formatting/_static/json-response.png)

Należy pamiętać, że typ zawartości odpowiedzi jest `application/json`, pokazano zarówno na liście żądania sieci, jak i w sekcji nagłówków odpowiedzi. Należy również zauważyć listę opcji dostępnych w przeglądarce (w tym przypadku Microsoft Edge) w nagłówku Accept w sekcji nagłówki żądania. Bieżąca metoda ignoruje ten nagłówek; Poniżej omówiono obeying go.

Aby zwrócić dane w formacie zwykłego tekstu, należy użyć `ContentResult` i `Content` pomocy:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

Odpowiedź z tej akcji:

![Karta Sieć narzędzi dla deweloperów w programie Microsoft Edge przedstawiający typ zawartości odpowiedzi to zwykły tekst](formatting/_static/text-response.png)

W takim przypadku należy zwrócić uwagę `Content-Type` zwracane jest `text/plain`. Można również uzyskać to samo przy użyciu tylko ciąg typu odpowiedzi:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> Dla nietrywialnej akcji z wieloma zwracać typów lub opcji (na przykład różnych kodów stanu HTTP na podstawie wyniku operacji wykonywanych) tak, `IActionResult` jako typ zwracany.

## <a name="content-negotiation"></a>Negocjowanie zawartości

Negocjowanie zawartości (*conneg* skrócie) występuje, gdy klient określa [nagłówek Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html). Domyślny format używany przez program ASP.NET Core MVC jest JSON. Negocjacje zawartości jest implementowany przez `ObjectResult`. Również jest wbudowana kod stanu wyników określonych akcji zwrócony z metody pomocnicze (wszystkie oparte na `ObjectResult`). Można także wrócić typu modelu (klasa została zdefiniowana jako typ transferu danych) i ramach będzie automatycznie zawijany w `ObjectResult` dla Ciebie.

Używa następującej metody akcji `Ok` i `NotFound` metody pomocnicze:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

Odpowiedź w formacie JSON zostaną zwrócone, chyba że zażądano innego formatu i serwer może zwrócić żądany format. Można użyć narzędzia, takiego jak [Fiddler](http://www.telerik.com/fiddler) Aby utworzyć żądanie zawiera nagłówek Accept i określić innego formatu. W takim przypadku, jeśli serwer ma *program formatujący* które powodują uzyskanie odpowiedzi w formacie żądanego, wynik zostanie zwrócony w formacie preferowanego klienta.

![Fiddler Konsola pokazująca, że ręcznie utworzonych pobrać żądania o wartości nagłówka Accept application/xml](formatting/_static/fiddler-composer.png)

Na zrzucie ekranu powyżej, Fiddler Composer został użyty do wygenerowania żądania, określając `Accept: application/xml`. Domyślnie, ASP.NET Core MVC obsługuje tylko JSON, dlatego nawet jeśli nie określono innego formatu, zwracana jest nadal formacie JSON. Jak dodać dodatkowe elementy formatujące w następnej sekcji zostanie wyświetlone.

Akcji kontrolera może zwrócić POCOs (zwykły stare obiekty CLR), w którym to przypadku automatycznie utworzy ASP.NET MVC `ObjectResult` dla Ciebie, który opakowuje obiektu. Klient pobierze sformatowany Zserializowany obiekt (JSON format jest domyślnie; można skonfigurować XML lub inne formaty). Jeśli obiekt zwracane jest `null`, a następnie zwraca platformę `204 No Content` odpowiedzi.

Zwraca typ obiektu:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

W przykładzie żądania dla aliasu autora otrzyma odpowiedź 200 OK z danymi autora. Żądanie nieprawidłowy alias otrzyma 204 odpowiedzi bez zawartości. Poniżej przedstawiono zrzuty ekranu przedstawiający odpowiedzi w formacie XML i JSON.

### <a name="content-negotiation-process"></a>Proces negocjacje zawartości

Zawartości *negocjacji* tylko ma miejsce, jeśli `Accept` nagłówek pojawia się w żądaniu. Jeśli żądanie zawiera nagłówek accept, platformę będzie wyliczyć typów nośników w nagłówku accept w kolejności preferencji i wyszukiwany element formatujący, który może utworzyć odpowiedź w jednym z formatów określony przez nagłówek accept. W przypadku, gdy element formatujący nie zostanie znaleziony, który można spełnić żądania klienta, platformę spróbuje znaleźć pierwszy element formatujący, który może utworzyć odpowiedź (chyba że deweloper została skonfigurowana opcja w `MvcOptions` do zwrócenia 406 nie do przyjęcia zamiast niego). Jeśli żądanie określa XML, ale nie skonfigurowano element formatujący XML, JSON program formatujący będzie używany. Ogólnie rzecz biorąc Jeśli element formatujący nie skonfigurowano które zapewniają żądany format, a następnie pierwszy element formatujący, który można sformatować obiekt jest używany. Jeśli nie zostanie podany bez nagłówka, pierwszy element formatujący, który może obsługiwać obiektu, który ma zostać zwrócona będzie używany do serializacji w odpowiedzi. W takim przypadku nie ma żadnych negocjacji miejsce — serwer jest określenie format zostanie użyty.

> [!NOTE]
> Jeśli zawiera nagłówek Accept `*/*`, nagłówek będą ignorowane, chyba że `RespectBrowserAcceptHeader` ma ustawioną wartość true na `MvcOptions`.

### <a name="browsers-and-content-negotiation"></a>Przeglądarki i negocjacje zawartości

W odróżnieniu od typowych klientów interfejsu API, przeglądarki sieci web mają tendencję do dostarczania `Accept` nagłówków zawierających szerokiej gamy formatów, w tym symboli wieloznacznych. Domyślnie, gdy w ramach wykrywa, czy żądanie pochodzi z przeglądarki, jego zignoruje `Accept` nagłówka i zamiast tego zwracany zawartości w aplikacji skonfigurowana domyślny format (JSON, chyba że skonfigurowane w inny sposób). Zapewnia bardziej spójne środowisko, korzystając z różnych przeglądarek użycie interfejsów API.

Jeśli wolisz nagłówków accept przeglądarkę honoruj aplikacji, można skonfigurować to w ramach konfiguracji MVC przez ustawienie `RespectBrowserAcceptHeader` do `true` w `ConfigureServices` metody w *Startup.cs*.

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a>Konfigurowanie programów formatujących

Jeśli aplikacja wymaga do obsługi dodatkowych formatach ponad wartość domyślną JSON, można dodać pakiety NuGet i skonfigurować MVC do ich obsługi. Istnieją oddzielne elementy formatujące dla danych wejściowych i wyjściowych. Wejściowy elementy formatujące są używane przez [powiązanie modelu](model-binding.md); elementy formatujące dane wyjściowe są używane do formatu odpowiedzi. Można również skonfigurować [niestandardowe elementy formatujące](../advanced/custom-formatters.md).

### <a name="adding-xml-format-support"></a>Dodawanie obsługi formatu XML

Aby dodać obsługę formatowania XML, zainstaluj `Microsoft.AspNetCore.Mvc.Formatters.Xml` pakietu NuGet.

Dodaj XmlSerializerFormatters do konfiguracji MVC w *Startup.cs*:

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

Alternatywnie można dodać tylko element formatujący danych wyjściowych:

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

Te dwie metody będzie serializować wyników za pomocą `System.Xml.Serialization.XmlSerializer`. Jeśli wolisz, możesz użyć `System.Runtime.Serialization.DataContractSerializer` przez dodanie jego skojarzony element formatujący:

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

Po dodaniu obsługę formatowania XML metody kontroler powinien zwrócić odpowiedni format na podstawie żądania `Accept` nagłówka, jak ta Fiddler przykładzie pokazano:

![Konsola fiddler: Raw kartę dla żądania zawiera wartość nagłówka Accept jest aplikacja/xml. Nieprzetworzone kartę odpowiedzi zawiera wartość nagłówka typu zawartości application/XML.](formatting/_static/xml-response.png)

Można zobaczyć w karcie inspektorzy zgłaszającego żądanie Raw GET z `Accept: application/xml` zestaw nagłówka. Pokazuje okienko odpowiedzi `Content-Type: application/xml` nagłówka i `Author` do pliku XML ma została wykonana serializacja obiektu.

Karta Composer służy do modyfikowania żądania, aby określić `application/json` w `Accept` nagłówka. Wykonanie żądania i odpowiedzi będą formatowane jako elementu JSON:

![Konsola fiddler: Raw kartę dla żądania zawiera wartość nagłówka Accept jest application/json. Nieprzetworzone kartę odpowiedzi zawiera wartość nagłówka typu zawartości application/json.](formatting/_static/json-response-fiddler.png)

Tego zrzutu ekranu widać żądania ustawia nagłówek z `Accept: application/json` i określa odpowiedź, taka sama jak jego `Content-Type`. `Author` Obiektu jest wyświetlany w treści odpowiedzi w formacie JSON.

### <a name="forcing-a-particular-format"></a>Wymuszanie w określonym formacie

Jeśli chcesz ograniczyć formatów odpowiedzi dla określonej akcji można wykonywać następujące czynności, możesz zastosować `[Produces]` filtru. `[Produces]` Filtru określa format odpowiedzi dla danego działania (lub kontrolera). Większość, takich jak [filtry](../controllers/filters.md), może to być zastosowane na działania, kontrolera lub zakresie globalnym.

```csharp
[Produces("application/json")]
public class AuthorsController
```

`[Produces]` Filtru wymusi wszystkich akcji w ramach `AuthorsController` do zwracania odpowiedzi w formacie JSON, nawet jeśli inne elementy formatujące zostały skonfigurowane dla aplikacji i klient dostarczył `Accept` nagłówka żądania różnych, dostępne Format. Zobacz [filtry](../controllers/filters.md) Aby dowiedzieć się więcej, łącznie ze sposobem stosowania filtrów globalnie.

### <a name="special-case-formatters"></a>Specjalne elementy formatujące Case

Niektóre specjalne przypadki są implementowane za pomocą wbudowanych elementy formatujące. Domyślnie `string` zwracane typy będą formatowane jako *zwykły tekst* (*tekst i html* żądanie za pośrednictwem `Accept` nagłówka). To zachowanie można usunąć przez usunięcie `TextOutputFormatter`. Usuń elementy formatujące w `Configure` metody w *Startup.cs* (pokazana poniżej). Akcje, które mają obiektu modelu zwracany typ zwróci 204 zawartości odpowiedzi wracając `null`. To zachowanie można usunąć przez usunięcie `HttpNoContentOutputFormatter`. Poniższy kod usuwa `TextOutputFormatter` i `HttpNoContentOutputFormatter`.

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

Bez `TextOutputFormatter`, `string` zwracać typów zwracać 406 nie do przyjęcia, na przykład. Należy pamiętać, że jeśli element formatujący XML istnieje, jej sformatuje `string` typy zwracane, jeżeli `TextOutputFormatter` zostanie usunięta.

Bez `HttpNoContentOutputFormatter`, obiektów null są sformatowane przy użyciu skonfigurowanego elementu formatującego. Na przykład program formatujący JSON po prostu zwróci odpowiedzi jednostki `null`, gdy element formatujący XML zwróci pustego elementu XML z atrybutem `xsi:nil="true"` ustawiony.

## <a name="response-format-url-mappings"></a>Mapowania adresów URL Format odpowiedzi

Klienci mogą żądać określony format jako część adresu URL, takie jak w ciągu zapytania lub część ścieżki lub przy użyciu rozszerzenia nazwy pliku specyficzne dla formatu takiego jak XML lub JSON. Powinny być określone mapowanie ścieżki żądania w trasy, który używa interfejsu API. Na przykład:

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

Żądany format można określić jako opcjonalny plik rozszerzenie umożliwia tej trasy. `[FormatFilter]` Atrybutu sprawdza istnienie wartości formatu `RouteData` i przypisze format odpowiedzi do odpowiedniego obiektu formatującego po utworzeniu odpowiedzi.

| Trasy                      | Program formatujący                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | Domyślny element formatujący danych wyjściowych       |
| `/products/GetById/5.json` | Program formatujący JSON (jeśli jest skonfigurowane) |
| `/products/GetById/5.xml`  | Element formatujący XML (jeśli jest skonfigurowane)  |
