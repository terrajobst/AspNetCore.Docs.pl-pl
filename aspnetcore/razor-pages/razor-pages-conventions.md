---
title: Razor Pages Konwencji tras i aplikacji w ASP.NET Core
author: guardrex
description: Dowiedz się, w jaki sposób Konwencja i konwencje dostawcy modelu aplikacji ułatwiają kontrolowanie routingu, odnajdywania i przetwarzania stron.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/22/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: a0a1eda69da34d1865fd11ef464c3697bcd01eff
ms.sourcegitcommit: 810d5831169770ee240d03207d6671dabea2486e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/22/2019
ms.locfileid: "72779214"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>Razor Pages Konwencji tras i aplikacji w ASP.NET Core

Autor [Luke Latham](https://github.com/guardrex)

Dowiedz się, w jaki sposób używać [konwencji dotyczących trasy strony i dostawcy modelu aplikacji](xref:mvc/controllers/application-model#conventions) do kontrolowania routingu, odnajdywania i przetwarzania stron w aplikacjach Razor Pages.

Jeśli trzeba skonfigurować niestandardowe trasy stron dla poszczególnych stron, skonfiguruj Routing do stron z [Konwencją AddPageRoute](#configure-a-page-route) opisanej w dalszej części tego tematu.

Aby określić trasę strony, dodać segmenty tras lub dodać parametry do trasy, użyj dyrektywy `@page` strony. Aby uzyskać więcej informacji, zobacz [niestandardowe trasy](xref:razor-pages/index#custom-routes).

Istnieją słowa zastrzeżone, których nie można używać jako segmentów tras ani nazw parametrów. Aby uzyskać więcej informacji, zobacz [Routing: zastrzeżone nazwy routingu](xref:fundamentals/routing#reserved-routing-names).

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))

| Scenariusz | Przykład ilustruje... |
| -------- | --------------------------- |
| [Konwencje modelu](#model-conventions)<br><br>Konwencje. Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | Dodaj szablon trasy i nagłówek do stron aplikacji. |
| [Konwencje akcji trasy strony](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Dodaj szablon trasy do stron w folderze i do pojedynczej strony. |
| [Konwencje akcji modelu strony](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (Klasa filtru, wyrażenie lambda lub fabryka filtrów)</li></ul> | Dodaj nagłówek do stron w folderze, Dodaj nagłówek do jednej strony i skonfiguruj [fabrykę filtrów](xref:mvc/controllers/filters#ifilterfactory) , aby dodać nagłówek do stron aplikacji. |

Konwencje Razor Pages są dodawane i konfigurowane przy użyciu metody rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> do <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> w kolekcji usług w klasie `Startup`. Poniższe przykłady Konwencji zostały omówione w dalszej części tego tematu:

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add( ... );
            options.Conventions.AddFolderRouteModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageRouteModelConvention(
                "/About", model => { ... });
            options.Conventions.AddPageRoute(
                "/Contact", "TheContactPage/{text?}");
            options.Conventions.AddFolderApplicationModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageApplicationModelConvention(
                "/About", model => { ... });
            options.Conventions.ConfigureFilter(model => { ... });
            options.Conventions.ConfigureFilter( ... );
        });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add( ... );
            options.Conventions.AddFolderRouteModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageRouteModelConvention(
                "/About", model => { ... });
            options.Conventions.AddPageRoute(
                "/Contact", "TheContactPage/{text?}");
            options.Conventions.AddFolderApplicationModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageApplicationModelConvention(
                "/About", model => { ... });
            options.Conventions.ConfigureFilter(model => { ... });
            options.Conventions.ConfigureFilter( ... );
        });
}
```

::: moniker-end

## <a name="route-order"></a>Kolejność tras

Trasy określają <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> do przetwarzania (dopasowanie trasy).

| Porządek            | Zachowanie |
| :--------------: | -------- |
| -1               | Trasa jest przetwarzana przed przetworzeniem innych tras. |
| 0                | Nie określono kolejności (wartość domyślna). Przypisanie `Order` (`Order = null`) domyślnie trasy `Order` do 0 (zero) do przetworzenia. |
| 1, 2, &hellip; n | Określa kolejność przetwarzania trasy. |

Przetwarzanie trasy zostało ustanowione według Konwencji:

* Trasy są przetwarzane w kolejności sekwencyjnej (-1, 0, 1, 2, &hellip; n).
* Gdy trasy mają takie same `Order`, najpierw pasuje do najbardziej określonej trasy, a następnie mniej konkretnych tras.
* Gdy trasy o tej samej `Order` i tej samej liczbie parametrów pasują do adresu URL żądania, trasy są przetwarzane w kolejności, w jakiej są dodawane do <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.

Jeśli to możliwe, unikaj w zależności od ustalonej kolejności przetwarzania trasy. Ogólnie rzecz biorąc, routing wybiera prawidłową trasę z dopasowywaniem adresów URL. Jeśli musisz ustawić właściwości `Order` trasy, aby poprawnie kierować żądania, schemat routingu aplikacji jest prawdopodobnie mylący dla klientów i jest nierozsądny do utrzymania. Postaraj się, aby uprościć schemat routingu aplikacji. Przykładowa aplikacja wymaga jawnej kolejności przetwarzania trasy, aby przedstawić kilka scenariuszy routingu przy użyciu jednej aplikacji. Należy jednak podjąć próbę uniknięcia praktycznego ustawienia `Order` tras w aplikacjach produkcyjnych.

Razor Pages Routing i kontroler MVC współdzielą implementację. Informacje o zamówieniu trasy w tematach MVC są dostępne w obszarze [routing do akcji kontrolera: porządkowanie tras atrybutów](xref:mvc/controllers/routing#ordering-attribute-routes).

## <a name="model-conventions"></a>Konwencje modelu

Dodaj delegata dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, aby dodać [konwencje modelu](xref:mvc/controllers/application-model#conventions) , które mają zastosowanie do Razor Pages.

### <a name="add-a-route-model-convention-to-all-pages"></a>Dodawanie Konwencji modelu trasy do wszystkich stron

Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu trasy strony.

Przykładowa aplikacja dodaje szablon `{globalTemplate?}` trasy do wszystkich stron w aplikacji:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `1`. Zapewnia to zachowanie dopasowania trasy w przykładowej aplikacji:

* Szablon trasy dla `TheContactPage/{text?}` został dodany w dalszej części tematu. Trasa strony kontaktowej ma domyślną kolejność `null` (`Order = 0`), więc pasuje przed szablonem `{globalTemplate?}` trasy.
* Szablon trasy `{aboutTemplate?}` zostanie dodany w dalszej części tematu. Szablon `{aboutTemplate?}` jest przyznany `Order` `2`. Gdy strona informacje jest wymagana w `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.
* Szablon trasy `{otherPagesTemplate?}` zostanie dodany w dalszej części tematu. Szablon `{otherPagesTemplate?}` jest przyznany `Order` `2`. Gdy zażądana jest jakakolwiek strona w folderze *Pages/OtherPages* z parametrem trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.

Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`. Należy polegać na routingu w celu wybrania odpowiedniej trasy.

Opcje Razor Pages, takie jak dodawanie <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, są dodawane po dodaniu MVC do kolekcji usług w `Startup.ConfigureServices`. Aby zapoznać się z przykładem, zobacz przykładową [aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

Zażądaj przykładowej strony o `localhost:5000/About/GlobalRouteValue` i sprawdź wynik:

![Strona informacje jest wymagana z segmentem trasy GlobalRouteValue. Wyrenderowana strona pokazuje, że wartość danych trasy jest przechwytywana w metodzie OnGet strony.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a>Dodawanie Konwencji modelu aplikacji do wszystkich stron

Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu aplikacji.

Aby przedstawić te i inne konwencje w dalszej części tematu, przykładowa aplikacja zawiera klasę `AddHeaderAttribute`. Konstruktor klasy akceptuje `name` ciąg i `values` tablicę ciągów. Te wartości są używane w `OnResultExecuting` metodzie do ustawiania nagłówka odpowiedzi. Pełna Klasa jest wyświetlana w sekcji [konwencje akcji modelu strony](#page-model-action-conventions) w dalszej części tematu.

Aplikacja Przykładowa używa klasy `AddHeaderAttribute` do dodawania nagłówka, `GlobalHeader` do wszystkich stron w aplikacji:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

*Startup.cs*:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:

![Nagłówki odpowiedzi na stronie informacje pokazują, że GlobalHeader został dodany.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a>Dodawanie Konwencji modelu programu obsługi do wszystkich stron

Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu obsługi stron.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

*Startup.cs*:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

## <a name="page-route-action-conventions"></a>Konwencje akcji trasy strony

Domyślny dostawca modelu trasy pochodzący z <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> wywołuje konwencje, które zostały zaprojektowane w celu zapewnienia punktów rozszerzalności do konfigurowania tras stron.

### <a name="folder-route-model-convention"></a>Konwencja modelu trasy folderu

Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> dla wszystkich stron w określonym folderze.

Przykładowa aplikacja używa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> do dodawania szablonu trasy `{otherPagesTemplate?}` do stron w folderze *OtherPages* :

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `2`. Dzięki temu szablon dla `{globalTemplate?}` (ustawiony wcześniej w temacie do `1`) ma priorytet dla pierwszej pozycji wartości danych trasy, gdy zostanie podana wartość pojedynczej trasy. Jeśli zażądano strony w folderze *Pages/OtherPages* z wartością parametru trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.

Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`. Należy polegać na routingu w celu wybrania odpowiedniej trasy.

Zażądaj przykładowej strony w `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` i sprawdź wynik:

![Żądanie Strona1 w folderze OtherPages z segmentem trasy GlobalRouteValue i OtherPagesRouteValue. Wyrenderowana strona pokazuje, że wartości danych trasy są przechwytywane w metodzie OnGet strony.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a>Konwencja modelu trasy strony

Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> dla strony o określonej nazwie.

Przykładowa aplikacja używa `AddPageRouteModelConvention` do dodawania szablonu trasy `{aboutTemplate?}` do strony informacje:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `2`. Dzięki temu szablon dla `{globalTemplate?}` (ustawiony wcześniej w temacie do `1`) ma priorytet dla pierwszej pozycji wartości danych trasy, gdy zostanie podana wartość pojedynczej trasy. Jeśli na stronie informacje zażądano wartości parametru trasy w `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.

Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`. Należy polegać na routingu w celu wybrania odpowiedniej trasy.

Zażądaj przykładowej strony o `localhost:5000/About/GlobalRouteValue/AboutRouteValue` i sprawdź wynik:

![Żądanie strony about z segmentami trasy dla GlobalRouteValue i AboutRouteValue. Wyrenderowana strona pokazuje, że wartości danych trasy są przechwytywane w metodzie OnGet strony.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a>Dostosowywanie tras stron przy użyciu transformatora parametrów

Trasy stron generowane przez ASP.NET Core mogą być dostosowywane przy użyciu transformatora parametrów. Transformator parametrów implementuje `IOutboundParameterTransformer` i przekształca wartość parametrów. Na przykład niestandardowy `SlugifyParameterTransformer` przekształcania parametrów zmienia wartość trasy `SubscriptionManagement` na `subscription-management`.

Konwencja modelu trasy strony `PageRouteTransformerConvention` stosuje transformator parametrów do segmentów nazw folderów i plików w przypadku automatycznie generowanych tras stron w aplikacji. Na przykład plik Razor Pages w */Pages/SubscriptionManagement/ViewAll.cshtml* będzie miał swoją trasę ponownie zapisany z `/SubscriptionManagement/ViewAll` do `/subscription-management/view-all`.

`PageRouteTransformerConvention` przekształca tylko automatycznie generowane segmenty trasy strony, która pochodzi z folderu Razor Pages i nazwy pliku. Nie przekształca segmentów tras dodanych z dyrektywą `@page`. Konwencja nie przetwarza również tras dodanych przez <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.

@No__t_0 jest zarejestrowana jako opcja w `Startup.ConfigureServices`:

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(
                new PageRouteTransformerConvention(
                    new SlugifyParameterTransformer()));
        });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(
                new PageRouteTransformerConvention(
                    new SlugifyParameterTransformer()));
        });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end

## <a name="configure-a-page-route"></a>Konfigurowanie trasy strony

Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>, aby skonfigurować trasę do strony pod określoną ścieżką strony. Wygenerowane linki do strony używają określonej trasy. `AddPageRoute` używa `AddPageRouteModelConvention` do ustanowienia trasy.

Przykładowa aplikacja tworzy trasę do `/TheContactPage` dla *Contact. cshtml*:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

Na stronie kontakt można także uzyskać dostęp do `/Contact` za pośrednictwem swojej trasy domyślnej.

Niestandardowa trasa przykładowej aplikacji do strony kontaktowej umożliwia określenie opcjonalnego segmentu trasy `text` (`{text?}`). Strona zawiera również ten opcjonalny segment w swojej dyrektywie `@page` na wypadek, gdy odwiedzający uzyskuje dostęp do strony przy użyciu trasy `/Contact`:

::: moniker range=">= aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

Należy pamiętać, że adres URL wygenerowany dla linku **kontaktowego** na renderowanej stronie odzwierciedla zaktualizowaną trasę:

![Link do kontaktu z przykładową aplikacją na pasku nawigacyjnym](razor-pages-conventions/_static/contact-link.png)

![Sprawdzanie linku kontaktu w renderowanym kodzie HTML wskazuje, że odwołanie href jest ustawione na wartość "/TheContactPage"](razor-pages-conventions/_static/contact-link-source.png)

Odwiedź stronę kontaktu na swojej zwykłej trasie, `/Contact` lub trasy niestandardowej, `/TheContactPage`. Jeśli podasz dodatkowy segment trasy `text`, na stronie zostanie wyświetlony segment zakodowany w formacie HTML:

![Przykładowa przeglądarka brzegowa dostarczająca opcjonalny segment trasy "text" elementu "TextValue" w adresie URL. Na renderowanej stronie zostanie wyświetlona wartość segmentu "text".](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Konwencje akcji modelu strony

Domyślny dostawca modelu strony, który implementuje <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> wywołuje konwencje, które są zaprojektowane w celu zapewnienia punktów rozszerzalności do konfigurowania modeli stron. Konwencje te są przydatne podczas kompilowania i modyfikowania scenariuszy przetwarzania i odnajdywania stron.

W przykładach w tej sekcji Przykładowa aplikacja używa klasy `AddHeaderAttribute`, która jest <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, która ma zastosowanie do nagłówka odpowiedzi:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

Przy użyciu konwencji, przykład pokazuje, jak zastosować atrybut do wszystkich stron w folderze i do pojedynczej strony.

**Konwencja modelu aplikacji folderu**

Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>, który wywołuje akcję w wystąpieniach <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> dla wszystkich stron w określonym folderze.

Przykład ilustruje użycie `AddFolderApplicationModelConvention` przez dodanie nagłówka, `OtherPagesHeader` do stron w folderze *OtherPages* aplikacji:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

Zażądaj przykładowej strony w `localhost:5000/OtherPages/Page1` i sprawdź nagłówki, aby wyświetlić wyniki:

![Nagłówki odpowiedzi strony OtherPages/Strona1 pokazują, że OtherPagesHeader został dodany.](razor-pages-conventions/_static/page1-otherpages-header.png)

**Konwencja modelu aplikacji strony**

Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> dla strony o określonej nazwie.

Przykład ilustruje użycie `AddPageApplicationModelConvention` przez dodanie nagłówka, `AboutHeader` do strony informacje:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:

![Nagłówki odpowiedzi na stronie informacje pokazują, że AboutHeader został dodany.](razor-pages-conventions/_static/about-page-about-header.png)

**Konfigurowanie filtru**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> konfiguruje określony filtr, aby zastosować. Można zaimplementować klasę filtru, ale Przykładowa aplikacja pokazuje, jak zaimplementować filtr w wyrażeniu lambda, które jest zaimplementowane w tle jako fabryka, która zwraca filtr:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

Model aplikacji strony służy do sprawdzania ścieżki względnej dla segmentów, które prowadzą do strony PAGE2 w folderze *OtherPages* . Jeśli warunek zostanie spełniony, zostanie dodany nagłówek. W przeciwnym razie zostanie zastosowana `EmptyFilter`.

`EmptyFilter` jest [filtrem akcji](xref:mvc/controllers/filters#action-filters). Ponieważ filtry akcji są ignorowane przez Razor Pages, `EmptyFilter` nie ma wpływu zgodnie z oczekiwaniami, jeśli ścieżka nie zawiera `OtherPages/Page2`.

Zażądaj strony PAGE2 próbki w `localhost:5000/OtherPages/Page2` i sprawdź nagłówki, aby wyświetlić wyniki:

![OtherPagesPage2Header jest dodawany do odpowiedzi dla PAGE2.](razor-pages-conventions/_static/page2-filter-header.png)

**Konfigurowanie fabryki filtrów**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> konfiguruje określoną fabrykę, aby zastosować [filtry](xref:mvc/controllers/filters) do wszystkich Razor Pages.

Przykładowa aplikacja zawiera przykładowe użycie [fabryki filtrów](xref:mvc/controllers/filters#ifilterfactory) przez dodanie nagłówka, `FilterFactoryHeader`, z dwiema wartościami na stronach aplikacji:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

*AddHeaderWithFactory.cs*:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:

![Nagłówki odpowiedzi na stronie informacje pokazują, że dodano dwa nagłówki FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Filtry MVC i filtr strony (IPageFilter)

[Filtry akcji](xref:mvc/controllers/filters#action-filters) MVC są ignorowane przez Razor Pages, ponieważ Razor Pages używają metod obsługi. Dostępne są inne typy filtrów MVC: [autoryzacja](xref:mvc/controllers/filters#authorization-filters), [wyjątek](xref:mvc/controllers/filters#exception-filters), [zasób](xref:mvc/controllers/filters#resource-filters)i [wynik](xref:mvc/controllers/filters#result-filters). Aby uzyskać więcej informacji, zobacz temat [filtry](xref:mvc/controllers/filters) .

Filtr strony (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) jest filtrem, który ma zastosowanie do Razor Pages. Aby uzyskać więcej informacji, zobacz [metody filtrowania dla Razor Pages](xref:razor-pages/filter).

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
