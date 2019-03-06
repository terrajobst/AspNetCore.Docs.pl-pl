---
title: Razor konwencje tras i aplikacji stron w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak konwencje tras i aplikacji dostawcy modelu pomóc routingu strony kontroli, odnajdywania i przetwarzania.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 02/27/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: 5cfcae5cffd5d9484ca64c3885b838ae0a2b4a0d
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346518"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>Razor konwencje tras i aplikacji stron w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

Dowiedz się, jak użyć strony [tras i aplikacji modelu Konwencji dostawcy](xref:mvc/controllers/application-model#conventions) do kontrolowania strony routingu, odnajdywania i przetwarzania w aplikacjach stron Razor.

Gdy trzeba skonfigurować trasy niestandardowej strony do poszczególnych stron, należy skonfigurować routing do stron z [Konwencji AddPageRoute](#configure-a-page-route) opisane w dalszej części tego tematu.

Aby określić trasę strony, Dodaj trasę segmentów lub dodać parametry do trasy, przejść do strony `@page` dyrektywy. Aby uzyskać więcej informacji, zobacz [trasy niestandardowe](xref:razor-pages/index#custom-routes).

Brak słów zastrzeżonych, których nie można użyć jako trasy segmentów lub nazw parametrów. Aby uzyskać więcej informacji, zobacz [routingu: Zastrzeżone nazwy routingu](xref:fundamentals/routing#reserved-routing-names).

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

| Scenariusz | W przykładzie pokazano... |
| -------- | --------------------------- |
| [Konwencje modelu](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | Dodaj szablon trasy i nagłówek do strony aplikacji. |
| [Konwencje akcji trasy strony](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Dodaj szablon trasy do stron w folderze i jednej strony. |
| [Konwencje akcji modelu strony](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (Filtr klasy, wyrażenie lambda lub filtra)</li></ul> | Dodanie nagłówka do stron w folderze, dodanie nagłówka do jednej strony, a następnie skonfiguruj [filtra](xref:mvc/controllers/filters#ifilterfactory) na dodanie nagłówka do strony aplikacji. |

Konwencje stron razor są dodawane i skonfigurować za pomocą <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> metodę rozszerzenia, aby <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> w kolekcji usługi w `Startup` klasy. W poniższych przykładach Konwencji są wyjaśnione w dalszej części tego tematu:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add( ... );
                options.Conventions.AddFolderRouteModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageRouteModelConvention("/About", model => { ... });
                options.Conventions.AddPageRoute("/Contact", "TheContactPage/{text?}");
                options.Conventions.AddFolderApplicationModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageApplicationModelConvention("/About", model => { ... });
                options.Conventions.ConfigureFilter(model => { ... });
                options.Conventions.ConfigureFilter( ... );
            });
}
```

## <a name="route-order"></a>Kolejność trasy

Określanie tras <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> przetwarzania (dopasowanie trasy).

| Zamówienie            | Zachowanie |
| :--------------: | -------- |
| -1               | Trasy są przetwarzane przed inne trasy są przetwarzane. |
| 0                | Nie określono kolejności (wartość domyślna). Przypisanie nie `Order` (`Order = null`) domyślne trasy `Order` na 0 (zero) do przetworzenia. |
| 1, 2, &hellip; n | Określa kolejność przetwarzania trasy. |

Zgodnie z Konwencją tworzy się przetwarzanie trasy:

* Trasy są przetwarzane w kolejności sekwencyjnej (-1, 0, 1, 2, &hellip; n).
* Kiedy trasy mają taką samą `Order`, maksymalnie określoną trasę jest dopasowywany najpierw następuje mniej określonej trasy.
* Gdy trasy z takimi samymi `Order` i taką samą liczbę parametrów dopasowania w adresie URL żądania, trasy są przetwarzane w kolejności, które są dodawane do <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.

Jeśli to możliwe Unikaj w zależności od ustalonych trasy kolejność przetwarzania. Ogólnie rzecz biorąc routingu wybiera poprawny trasę z pasującymi adresu URL. Jeśli musisz ustawić trasy `Order` właściwości w celu kierowania żądań poprawnie, schemat routingu aplikacji jest prawdopodobnie mylące dla klientów powolnymi i słabymi do zachowania. Wyszukiwanie uprościć routingu schemat aplikacji. Przykładowa aplikacja wymaga jawnego trasę w protokole przetwarzania zamówienia, aby zademonstrować kilku scenariuszy routingu za pomocą pojedynczej aplikacji. Jednakże, należy podjąć w celu uniknięcia rozwiązaniem jest ustawienie trasy `Order` w aplikacjach produkcyjnych.

Strony razor routingu i MVC kontroler routingu udziału wdrożenia. Informacje o kolejność trasy w tematach MVC znajduje się w temacie [Routing do akcji kontrolera: Kolejność trasy w atrybucie](xref:mvc/controllers/routing#ordering-attribute-routes).

## <a name="model-conventions"></a>Konwencje modelu

Dodawanie delegatów dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> dodać [modelu Konwencji](xref:mvc/controllers/application-model#conventions) które są stosowane do stron Razor.

### <a name="add-a-route-model-convention-to-all-pages"></a>Dodawanie modelu Konwencji tras do wszystkich stron

Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> tworzyć i dodawać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> do kolekcji <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> wystąpień, które są stosowane podczas trasy strony modelu konstrukcji.

Przykładowa aplikacja dodaje `{globalTemplate?}` szablon trasy do wszystkich stron w aplikacji:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> ustawiono `1`. Dzięki temu Następująca trasa pasujące do zachowania w przykładowej aplikacji:

* Szablon trasy dla `TheContactPage/{text?}` zostanie dodany w dalszej części tematu. Trasa strony kontaktu ma domyślną kolejność `null` (`Order = 0`), tak, aby odpowiadała przed `{globalTemplate?}` szablon trasy.
* `{aboutTemplate?}` Szablon trasy zostanie dodany w dalszej części tematu. `{aboutTemplate?}` Znajduje się szablon `Order` z `2`. Po żądaniu strony informacje o `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`) i nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) ze względu na ustawienie `Order` właściwości.
* `{otherPagesTemplate?}` Szablon trasy zostanie dodany w dalszej części tematu. `{otherPagesTemplate?}` Znajduje się szablon `Order` z `2`. Po dowolnej stronie *stron/OtherPages* folderu jest żądanego przy użyciu parametru trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`) i nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) ze względu na ustawienie `Order` właściwości.

Wszędzie tam, gdzie to możliwe, nie należy ustawiać `Order`, które powoduje `Order = 0`. Polegaj na routingu, aby wybrać poprawny trasy.

Opcje strony razor, takich jak dodawanie <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, są dodawane, gdy MVC zostanie dodany do kolekcji usługi w `Startup.ConfigureServices`. Aby uzyskać przykład, zobacz [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

Żądanie próbki o stronę w `localhost:5000/About/GlobalRouteValue` i sprawdź wynik:

![Za pomocą tras segment GlobalRouteValue żądaniu strony informacje. Renderowanej stronie pokazuje, że wartości danych trasy są przechwytywane w metodzie OnGet strony.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a>Dodawanie aplikacji modelu Konwencji do wszystkich stron

Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> tworzyć i dodawać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> do kolekcji <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> wystąpień, które są stosowane podczas stronie konstruowania modelu aplikacji.

Aby zademonstrować to i inne konwencje w dalszej części tematu, zawiera przykładową aplikację `AddHeaderAttribute` klasy. Konstruktor klasy akceptuje `name` ciągu i `values` tablicy ciągów. Te wartości są używane w jego `OnResultExecuting` metodę, aby ustawić nagłówek odpowiedzi. Pełne klasy jest wyświetlany w [strony modelu działania Konwencji](#page-model-action-conventions) w dalszej części tematu.

Ta aplikacja używa przykładowych `AddHeaderAttribute` klasy, aby dodać nagłówek `GlobalHeader`, do wszystkich stron w aplikacji:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

Żądanie próbki o stronę w `localhost:5000/About` i sprawdzić te nagłówki, aby wyświetlić wynik:

![Nagłówki odpowiedzi strony informacje pokazują, że dodano GlobalHeader.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a>Dodaj Konwencji modelu obsługi do wszystkich stron

Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> tworzyć i dodawać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> do kolekcji <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> wystąpień, które są stosowane podczas stronie konstruowania modelu obsługi.

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a>Konwencje akcji trasy strony

Domyślnego dostawcę modelu trasy, która pochodzi od klasy <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> wywołuje konwencje, które są przeznaczone do zapewnienia punkty rozszerzeń do konfigurowania tras strony.

### <a name="folder-route-model-convention"></a>Folder trasy modelu Konwencji

Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> tworzyć i dodawać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> , wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> dla wszystkich stron w określonym folderze.

Ta aplikacja używa przykładowych <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> dodać `{otherPagesTemplate?}` szablon trasy do stron w *OtherPages* folderu:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> ustawiono `2`. Gwarantuje to, że szablon `{globalTemplate?}` (wcześniej w temacie, aby `1`) otrzymuje priorytet dla pierwszego dane trasy wartość pozycji, gdy została podana wartość jedną trasę. Jeśli na stronie w programie *stron/OtherPages* zażądano folderu z wartością parametru trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`) i nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) ze względu na ustawienie `Order` właściwości.

Wszędzie tam, gdzie to możliwe, nie należy ustawiać `Order`, które powoduje `Order = 0`. Polegaj na routingu, aby wybrać poprawny trasy.

Żądanie strony Strona 1 przykładu w `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` i sprawdź wynik:

![Strona 1 w folderze OtherPages żądanie z segmentem trasy GlobalRouteValue i OtherPagesRouteValue. Renderowanej stronie pokazuje, że wartości danych trasy są przechwytywane w metodzie OnGet strony.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a>Strona trasy modelu Konwencji

Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> tworzyć i dodawać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> , wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> dla strony o określonej nazwie.

Ta aplikacja używa przykładowych `AddPageRouteModelConvention` dodać `{aboutTemplate?}` szablon trasy do strony informacje:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> ustawiono `2`. Gwarantuje to, że szablon `{globalTemplate?}` (wcześniej w temacie, aby `1`) otrzymuje priorytet dla pierwszego dane trasy wartość pozycji, gdy została podana wartość jedną trasę. Jeśli wartością parametru trasy w żądaniu strony informacje `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`) i nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) ze względu na ustawienie `Order` właściwości.

Wszędzie tam, gdzie to możliwe, nie należy ustawiać `Order`, które powoduje `Order = 0`. Polegaj na routingu, aby wybrać poprawny trasy.

Żądanie próbki o stronę w `localhost:5000/About/GlobalRouteValue/AboutRouteValue` i sprawdź wynik:

![Informacje o stronie żądania segmentów trasy GlobalRouteValue i AboutRouteValue. Renderowanej stronie pokazuje, że wartości danych trasy są przechwytywane w metodzie OnGet strony.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a>Użyj transformatora parametru, aby dostosować stronę trasy

Używanie transformatora parametru można dostosować trasy strony, wygenerowane przez platformy ASP.NET Core. Transformer parametr implementuje `IOutboundParameterTransformer` i przekształca wartości parametrów. Na przykład niestandardowy `SlugifyParameterTransformer` zmiany transformatora parametrów `SubscriptionManagement` trasy wartość `subscription-management`.

`PageRouteTransformerConvention` Strony trasy modelu Konwencją transformatora parametr do segmentów nazwę folderu i pliku tras automatycznie wygenerowanych stron w aplikacji. Na przykład pliku stron Razor */Pages/SubscriptionManagement/ViewAll.cshtml* miałby trasę przepisany z `/SubscriptionManagement/ViewAll` do `/subscription-management/view-all`.

`PageRouteTransformerConvention` tylko przy użyciu automatycznie generowanego segmenty trasy strony ze stron Razor folder i nazwę pliku. Go nie przekształci segmentów trasa dodana z `@page` dyrektywy. Konwencja również nie przekształci trasy dodane przez <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.

`PageRouteTransformerConvention` Jest zarejestrowany jako opcja w `Startup.ConfigureServices`:

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

## <a name="configure-a-page-route"></a>Skonfiguruj trasy strony

Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> do konfigurowania tras do strony w ścieżce określonej strony. Wygenerowany łącza do strony Użyj określonej trasy. `AddPageRoute` używa `AddPageRouteModelConvention` nawiązać trasy.

Przykładowa aplikacja tworzy trasę do `/TheContactPage` dla *Contact.cshtml*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

Na stronie kontakt osiągalna na `/Contact` za pomocą tej trasy domyślnej.

Przykładowej aplikacji niestandardowe trasy do strony kontaktu umożliwia opcjonalny `text` trasy segmentu (`{text?}`). Strona zawiera także ten segment opcjonalny w jego `@page` dyrektywy, w przypadku, gdy użytkownik uzyskuje dostęp do strony od jego `/Contact` trasy:

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

Należy zauważyć, że adres URL jest generowane dla **skontaktuj się z pomocą** łącze na renderowanej stronie odzwierciedla zaktualizowany trasy:

![Przykładowy link skontaktuj się z aplikacji na pasku nawigacyjnym](razor-pages-conventions/_static/contact-link.png)

![Inspekcja łącze kontaktu w postaci HTML wskazuje, że odwołanie hipertekstowe jest ustawiona na "/ TheContactPage"](razor-pages-conventions/_static/contact-link-source.png)

Odwiedź stronę skontaktuj się z pomocą pod jedną trasę zwykłych `/Contact`, lub tras niestandardowych `/TheContactPage`. Jeśli podasz dodatkowego `text` segmentu trasy, na stronie znajdują się segment kodowany w formacie HTML, aby podać:

![Przykład przeglądarki Microsoft Edge podawania opcjonalne 'text' segment trasy "Wartość tekstowa" w adresie URL. Renderowanej strony wyświetla wartość segmentu 'text'.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Konwencje akcji modelu strony

Domyślny dostawca modelu strony, który implementuje <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> wywołuje konwencje, które są przeznaczone do Podaj punkty rozszerzeń do skonfigurowania modeli strony. Konwencje te są przydatne podczas tworzenia i modyfikowania strony odnajdowania i scenariusze przetwarzania.

W przykładach w tej sekcji Przykładowa aplikacja używa `AddHeaderAttribute` klasy, która jest <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, która odnosi się nagłówek odpowiedzi:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

Za pomocą Konwencji, przykład pokazuje, jak zastosować atrybut do wszystkich stron w folderze i jednej strony.

**Folder aplikacji modelu Konwencji**

Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> tworzyć i dodawać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> , wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> wystąpienia dla wszystkich stron w określonym folderze.

W przykładzie pokazano użycie `AddFolderApplicationModelConvention` przez dodanie nagłówka `OtherPagesHeader`, do stron wewnątrz *OtherPages* folder aplikacji:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

Żądanie strony Strona 1 przykładu w `localhost:5000/OtherPages/Page1` i sprawdzić te nagłówki, aby wyświetlić wynik:

![Nagłówki odpowiedzi dla strony strona OtherPages/1 pokazują, że dodano OtherPagesHeader.](razor-pages-conventions/_static/page1-otherpages-header.png)

**Strona aplikacji modelu Konwencji**

Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> tworzyć i dodawać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> , wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> dla strony o określonej nazwie.

W przykładzie pokazano użycie `AddPageApplicationModelConvention` przez dodanie nagłówka `AboutHeader`, do strony informacje:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

Żądanie próbki o stronę w `localhost:5000/About` i sprawdzić te nagłówki, aby wyświetlić wynik:

![Nagłówki odpowiedzi strony informacje pokazują, że dodano AboutHeader.](razor-pages-conventions/_static/about-page-about-header.png)

**Skonfigurować filtr**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> Konfiguruje określony filtr do zastosowania. Można zaimplementować Filtr klasy, ale Przykładowa aplikacja pokazuje, jak zaimplementować filtr w wyrażeniu lambda, która jest zaimplementowana w tle, ponieważ fabryka, która zwraca filtru:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

Model aplikacji strony służy do sprawdzania ścieżkę względną do segmentów, które mogą prowadzić do strony Strona2 *OtherPages* folderu. Jeśli warunek zostanie spełniony, zostanie dodany nagłówek. Jeśli nie, `EmptyFilter` jest stosowany.

`EmptyFilter` jest [filtr akcji](xref:mvc/controllers/filters#action-filters). Ponieważ filtry akcji są ignorowane przez stron Razor `EmptyFilter` żadnych operacji zgodnie z oczekiwaniami, jeśli ścieżka nie zawiera `OtherPages/Page2`.

Żądanie strony Strona2 przykładu w `localhost:5000/OtherPages/Page2` i sprawdzić te nagłówki, aby wyświetlić wynik:

![OtherPagesPage2Header jest dodawany do odpowiedzi Strona2.](razor-pages-conventions/_static/page2-filter-header.png)

**Konfigurowanie fabryki filtru**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> Umożliwia skonfigurowanie określonej fabryki, aby zastosować [filtry](xref:mvc/controllers/filters) do wszystkich stron Razor.

Przykładowa aplikacja zawiera przykład za pomocą [filtra](xref:mvc/controllers/filters#ifilterfactory) przez dodanie nagłówka `FilterFactoryHeader`, za pomocą dwóch wartości do stron aplikacji:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Żądanie próbki o stronę w `localhost:5000/About` i sprawdzić te nagłówki, aby wyświetlić wynik:

![Nagłówki odpowiedzi strony informacje pokazują, czy dwa nagłówki FilterFactoryHeader został dodany.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Filtry MVC i filtr strony (IPageFilter)

MVC [filtry akcji](xref:mvc/controllers/filters#action-filters) są ignorowane przez stron Razor, ponieważ stron Razor korzystać z metody obsługi. Inne typy filtrów platformy MVC są dostępne do użycia: [Autoryzacja](xref:mvc/controllers/filters#authorization-filters), [wyjątek](xref:mvc/controllers/filters#exception-filters), [zasobów](xref:mvc/controllers/filters#resource-filters), i [wynik](xref:mvc/controllers/filters#result-filters). Aby uzyskać więcej informacji, zobacz [filtry](xref:mvc/controllers/filters) tematu.

Filtr strony (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) jest filtr, który ma zastosowanie do stron Razor. Aby uzyskać więcej informacji, zobacz [metody filtrowania dla stron Razor](xref:razor-pages/filter).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Konwencje autoryzacji Razor Pages](xref:security/authorization/razor-pages-authorization)
