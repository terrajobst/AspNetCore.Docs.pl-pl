---
title: Razor konwencje tras i aplikacji stron w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak konwencje tras i aplikacji dostawcy modelu pomóc routingu strony kontroli, odnajdywania i przetwarzania.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/17/2018
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: ea4f785dc8a64b430e312fd122a4d3184b61949e
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011865"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>Razor konwencje tras i aplikacji stron w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

Dowiedz się, jak użyć strony [tras i aplikacji modelu Konwencji dostawcy](xref:mvc/controllers/application-model#conventions) do kontrolowania strony routingu, odnajdywania i przetwarzania w aplikacjach stron Razor.

Gdy trzeba skonfigurować trasy niestandardowej strony do poszczególnych stron, należy skonfigurować routing do stron z [Konwencji AddPageRoute](#configure-a-page-route) opisane w dalszej części tego tematu.

Aby określić trasę strony, Dodaj trasę segmentów lub dodać parametry do trasy, przejść do strony `@page` dyrektywy. Aby uzyskać więcej informacji, zobacz [trasy niestandardowe](xref:razor-pages/index#custom-routes).

Brak słów zastrzeżonych, których nie można użyć jako trasy segmentów lub nazw parametrów. Aby uzyskać więcej informacji, zobacz [routingu: zarezerwowanych nazw routingu](xref:fundamentals/routing#reserved-routing-names).

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

::: moniker range="= aspnetcore-2.0"

| Scenariusz | W przykładzie pokazano... |
| -------- | --------------------------- |
| [Konwencje modelu](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | Dodaj szablon trasy i nagłówek do strony aplikacji. |
| [Konwencje akcji trasy strony](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Dodaj szablon trasy do stron w folderze i jednej strony. |
| [Konwencje akcji modelu strony](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (Filtr klasy, wyrażenie lambda lub filtra)</li></ul> | Dodanie nagłówka do stron w folderze, dodanie nagłówka do jednej strony, a następnie skonfiguruj [filtra](xref:mvc/controllers/filters#ifilterfactory) na dodanie nagłówka do strony aplikacji. |
| [Domyślna strona aplikacji modelu dostawcy](#replace-the-default-page-app-model-provider) | Zastąp domyślnego dostawcę modelu strony można zmienić konwencje dla nazwy programów obsługi. |

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

| Scenariusz | W przykładzie pokazano... |
| -------- | --------------------------- |
| [Konwencje modelu](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | Dodaj szablon trasy i nagłówek do strony aplikacji. |
| [Konwencje akcji trasy strony](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Dodaj szablon trasy do stron w folderze i jednej strony. |
| [Konwencje akcji modelu strony](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (Filtr klasy, wyrażenie lambda lub filtra)</li></ul> | Dodanie nagłówka do stron w folderze, dodanie nagłówka do jednej strony, a następnie skonfiguruj [filtra](xref:mvc/controllers/filters#ifilterfactory) na dodanie nagłówka do strony aplikacji. |
| [Domyślna strona aplikacji modelu dostawcy](#replace-the-default-page-app-model-provider) | Zastąp domyślnego dostawcę modelu strony można zmienić konwencje dla nazwy programów obsługi. |

::: moniker-end

Konwencje stron razor są dodawane i skonfigurować za pomocą [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) metodę rozszerzenia, aby [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc) w kolekcji usługi w `Startup` klasy. W poniższych przykładach Konwencji są wyjaśnione w dalszej części tego tematu:

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

| Kolejność            | Zachowanie |
| :--------------: | -------- |
| -1               | Trasy są przetwarzane przed inne trasy są przetwarzane. |
| 0                | Nie określono kolejności (wartość domyślna). Przypisanie nie `Order` (`Order = null`) domyślne trasy `Order` na 0 (zero) do przetworzenia. |
| 1, 2, &hellip; n | Określa kolejność przetwarzania trasy. |

Zgodnie z Konwencją tworzy się przetwarzanie trasy:

* Trasy są przetwarzane w kolejności sekwencyjnej (-1, 0, 1, 2, &hellip; n).
* Kiedy trasy mają taką samą `Order`, maksymalnie określoną trasę jest dopasowywany najpierw następuje mniej określonej trasy.
* Gdy trasy z takimi samymi `Order` i taką samą liczbę parametrów dopasowania w adresie URL żądania, trasy są przetwarzane w kolejności, które są dodawane do <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.

Jeśli to możliwe Unikaj w zależności od ustalonych trasy kolejność przetwarzania. Ogólnie rzecz biorąc routingu wybiera poprawny trasę z pasującymi adresu URL. Jeśli musisz ustawić trasy `Order` właściwości w celu kierowania żądań poprawnie, schemat routingu aplikacji jest prawdopodobnie mylące dla klientów powolnymi i słabymi do zachowania. Wyszukiwanie uprościć routingu schemat aplikacji. Przykładowa aplikacja wymaga jawnego trasę w protokole przetwarzania zamówienia, aby zademonstrować kilku scenariuszy routingu za pomocą pojedynczej aplikacji. Jednakże, należy podjąć w celu uniknięcia rozwiązaniem jest ustawienie trasy `Order` w aplikacjach produkcyjnych.

Strony razor routingu i MVC kontroler routingu udziału wdrożenia. Informacje o kolejność trasy w tematach MVC znajduje się w temacie [Routing do akcji kontrolera: porządkowanie tras atrybutów](xref:mvc/controllers/routing#ordering-attribute-routes).

## <a name="model-conventions"></a>Konwencje modelu

Dodawanie delegatów dla [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) dodać [modelu Konwencji](xref:mvc/controllers/application-model#conventions) które są stosowane do stron Razor.

**Dodawanie modelu Konwencji tras do wszystkich stron**

Użyj [konwencje](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) tworzyć i dodawać [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) do kolekcji [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) wystąpień, które są stosowane podczas modelu trasy strony konstrukcja.

Przykładowa aplikacja dodaje `{globalTemplate?}` szablon trasy do wszystkich stron w aplikacji:

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> ustawiono `1`. Dzięki temu Następująca trasa pasujące do zachowania w przykładowej aplikacji:

* Szablon trasy dla `TheContactPage/{text?}` zostanie dodany w dalszej części tematu. Trasa strony kontaktu ma domyślną kolejność `null` (`Order = 0`), tak, aby odpowiadała przed `{globalTemplate?}` szablon trasy.
* `{aboutTemplate?}` Szablon trasy zostanie dodany w dalszej części tematu. `{aboutTemplate?}` Znajduje się szablon `Order` z `2`. Po żądaniu strony informacje o `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`) i nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) ze względu na ustawienie `Order` właściwości.
* `{otherPagesTemplate?}` Szablon trasy zostanie dodany w dalszej części tematu. `{otherPagesTemplate?}` Znajduje się szablon `Order` z `2`. Po dowolnej stronie *stron/OtherPages* folderu jest żądanego przy użyciu parametru trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`) i nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) ze względu na ustawienie `Order` właściwości.

Wszędzie tam, gdzie to możliwe, nie należy ustawiać `Order`, które powoduje `Order = 0`. Polegaj na routingu, aby wybrać poprawny trasy.

Opcje strony razor, takich jak dodawanie [konwencje](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions), są dodawane, gdy MVC zostanie dodany do kolekcji usługi w `Startup.ConfigureServices`. Aby uzyskać przykład, zobacz [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/sample/).

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet1)]

Żądanie próbki o stronę w `localhost:5000/About/GlobalRouteValue` i sprawdź wynik:

![Za pomocą tras segment GlobalRouteValue żądaniu strony informacje. Renderowanej stronie pokazuje, że wartości danych trasy są przechwytywane w metodzie OnGet strony.](razor-pages-conventions/_static/about-page-global-template.png)

**Dodawanie aplikacji modelu Konwencji do wszystkich stron**

Użyj [konwencje](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) tworzyć i dodawać [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) do kolekcji [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) wystąpień, które są stosowane podczas strony modelu aplikacji konstrukcja.

Aby zademonstrować to i inne konwencje w dalszej części tematu, zawiera przykładową aplikację `AddHeaderAttribute` klasy. Konstruktor klasy akceptuje `name` ciągu i `values` tablicy ciągów. Te wartości są używane w jego `OnResultExecuting` metodę, aby ustawić nagłówek odpowiedzi. Pełne klasy jest wyświetlany w [strony modelu działania Konwencji](#page-model-action-conventions) w dalszej części tematu.

Ta aplikacja używa przykładowych `AddHeaderAttribute` klasy, aby dodać nagłówek `GlobalHeader`, do wszystkich stron w aplikacji:

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet2)]

Żądanie próbki o stronę w `localhost:5000/About` i sprawdzić te nagłówki, aby wyświetlić wynik:

![Nagłówki odpowiedzi strony informacje pokazują, że dodano GlobalHeader.](razor-pages-conventions/_static/about-page-global-header.png)

::: moniker range=">= aspnetcore-2.1"

**Dodaj Konwencji modelu obsługi do wszystkich stron**

Użyj [konwencje](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) tworzyć i dodawać [IPageHandlerModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipagehandlermodelconvention) do kolekcji [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) wystąpień, które są stosowane podczas modelu obsługi strony konstrukcja.

```csharp
public class GlobalPageHandlerModelConvention 
    : IPageHandlerModelConvention
{
    public void Apply(PageHandlerModel model)
    {
        ...
    }
}
```

`Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(new GlobalPageHandlerModelConvention());
        });
```

::: moniker-end

## <a name="page-route-action-conventions"></a>Konwencje akcji trasy strony

Domyślnego dostawcę modelu trasy, która pochodzi od klasy [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) wywołuje konwencje, które są przeznaczone do zapewnienia punkty rozszerzeń do konfigurowania tras strony.

**Folder trasy modelu Konwencji**

Użyj [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) tworzyć i dodawać [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) , wywołuje akcję na [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) dla wszystkich stron w obszarze określony folder.

Ta aplikacja używa przykładowych `AddFolderRouteModelConvention` dodać `{otherPagesTemplate?}` szablon trasy do stron w *OtherPages* folderu:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet3)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> ustawiono `2`. Gwarantuje to, że szablon `{globalTemplate?}` (wcześniej w temacie, aby `1`) otrzymuje priorytet dla pierwszego dane trasy wartość pozycji, gdy została podana wartość jedną trasę. Jeśli na stronie w programie *stron/OtherPages* zażądano folderu z wartością parametru trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`) i nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) ze względu na ustawienie `Order` właściwości.

Wszędzie tam, gdzie to możliwe, nie należy ustawiać `Order`, które powoduje `Order = 0`. Polegaj na routingu, aby wybrać poprawny trasy.

Żądanie strony Strona 1 przykładu w `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` i sprawdź wynik:

![Strona 1 w folderze OtherPages żądanie z segmentem trasy GlobalRouteValue i OtherPagesRouteValue. Renderowanej stronie pokazuje, że wartości danych trasy są przechwytywane w metodzie OnGet strony.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

**Strona trasy modelu Konwencji**

Użyj [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) tworzyć i dodawać [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) , wywołuje akcję na [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) dla strony o określonym Nazwa.

Ta aplikacja używa przykładowych `AddPageRouteModelConvention` dodać `{aboutTemplate?}` szablon trasy do strony informacje:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet4)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> ustawiono `2`. Gwarantuje to, że szablon `{globalTemplate?}` (wcześniej w temacie, aby `1`) otrzymuje priorytet dla pierwszego dane trasy wartość pozycji, gdy została podana wartość jedną trasę. Jeśli wartością parametru trasy w żądaniu strony informacje `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`) i nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) ze względu na ustawienie `Order` właściwości.

Wszędzie tam, gdzie to możliwe, nie należy ustawiać `Order`, które powoduje `Order = 0`. Polegaj na routingu, aby wybrać poprawny trasy.

Żądanie próbki o stronę w `localhost:5000/About/GlobalRouteValue/AboutRouteValue` i sprawdź wynik:

![Informacje o stronie żądania segmentów trasy GlobalRouteValue i AboutRouteValue. Renderowanej stronie pokazuje, że wartości danych trasy są przechwytywane w metodzie OnGet strony.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>Skonfiguruj trasy strony

Użyj [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) do konfigurowania tras do strony w ścieżce określonej strony. Wygenerowany łącza do strony Użyj określonej trasy. `AddPageRoute` używa `AddPageRouteModelConvention` nawiązać trasy.

Przykładowa aplikacja tworzy trasę do `/TheContactPage` dla *Contact.cshtml*:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet5)]

Na stronie kontakt osiągalna na `/Contact` za pomocą tej trasy domyślnej.

Przykładowej aplikacji niestandardowe trasy do strony kontaktu umożliwia opcjonalny `text` trasy segmentu (`{text?}`). Strona zawiera także ten segment opcjonalny w jego `@page` dyrektywy, w przypadku, gdy użytkownik uzyskuje dostęp do strony od jego `/Contact` trasy:

[!code-cshtml[](razor-pages-conventions/sample/Pages/Contact.cshtml?highlight=1)]

Należy zauważyć, że adres URL jest generowane dla **skontaktuj się z pomocą** łącze na renderowanej stronie odzwierciedla zaktualizowany trasy:

![Przykładowy link skontaktuj się z aplikacji na pasku nawigacyjnym](razor-pages-conventions/_static/contact-link.png)

![Inspekcja łącze kontaktu w postaci HTML wskazuje, że odwołanie hipertekstowe jest ustawiona na "/ TheContactPage"](razor-pages-conventions/_static/contact-link-source.png)

Odwiedź stronę skontaktuj się z pomocą pod jedną trasę zwykłych `/Contact`, lub tras niestandardowych `/TheContactPage`. Jeśli podasz dodatkowego `text` segmentu trasy, na stronie znajdują się segment kodowany w formacie HTML, aby podać:

![Przykład przeglądarki Microsoft Edge podawania opcjonalne 'text' segment trasy "Wartość tekstowa" w adresie URL. Renderowanej strony wyświetla wartość segmentu 'text'.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Konwencje akcji modelu strony

Domyślny dostawca modelu strony, który implementuje [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) wywołuje konwencje, które są przeznaczone do Podaj punkty rozszerzeń do skonfigurowania modeli strony. Konwencje te są przydatne podczas tworzenia i modyfikowania strony odnajdowania i scenariusze przetwarzania.

W przykładach w tej sekcji Przykładowa aplikacja używa `AddHeaderAttribute` klasy, która jest [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), która odnosi się nagłówek odpowiedzi:

[!code-csharp[](razor-pages-conventions/sample/Filters/AddHeader.cs?name=snippet1)]

Za pomocą Konwencji, przykład pokazuje, jak zastosować atrybut do wszystkich stron w folderze i jednej strony.

**Folder aplikacji modelu Konwencji**

Użyj [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) tworzyć i dodawać [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) , wywołuje akcję na [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) wystąpień dla wszystkich stron w określonym folderze.

W przykładzie pokazano użycie `AddFolderApplicationModelConvention` przez dodanie nagłówka `OtherPagesHeader`, do stron wewnątrz *OtherPages* folder aplikacji:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet6)]

Żądanie strony Strona 1 przykładu w `localhost:5000/OtherPages/Page1` i sprawdzić te nagłówki, aby wyświetlić wynik:

![Nagłówki odpowiedzi dla strony strona OtherPages/1 pokazują, że dodano OtherPagesHeader.](razor-pages-conventions/_static/page1-otherpages-header.png)

**Strona aplikacji modelu Konwencji**

Użyj [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) tworzyć i dodawać [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) , wywołuje akcję na [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) strony o określonej nazwie.

W przykładzie pokazano użycie `AddPageApplicationModelConvention` przez dodanie nagłówka `AboutHeader`, do strony informacje:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet7)]

Żądanie próbki o stronę w `localhost:5000/About` i sprawdzić te nagłówki, aby wyświetlić wynik:

![Nagłówki odpowiedzi strony informacje pokazują, że dodano AboutHeader.](razor-pages-conventions/_static/about-page-about-header.png)

**Skonfigurować filtr**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) konfiguruje określony filtr do zastosowania. Można zaimplementować Filtr klasy, ale Przykładowa aplikacja pokazuje, jak zaimplementować filtr w wyrażeniu lambda, która jest zaimplementowana w tle, ponieważ fabryka, która zwraca filtru:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet8)]

Model aplikacji strony służy do sprawdzania ścieżkę względną do segmentów, które mogą prowadzić do strony Strona2 *OtherPages* folderu. Jeśli warunek zostanie spełniony, zostanie dodany nagłówek. Jeśli nie, `EmptyFilter` jest stosowany.

`EmptyFilter` jest [filtr akcji](xref:mvc/controllers/filters#action-filters). Ponieważ filtry akcji są ignorowane przez stron Razor `EmptyFilter` żadnych operacji zgodnie z oczekiwaniami, jeśli ścieżka nie zawiera `OtherPages/Page2`.

Żądanie strony Strona2 przykładu w `localhost:5000/OtherPages/Page2` i sprawdzić te nagłówki, aby wyświetlić wynik:

![OtherPagesPage2Header jest dodawany do odpowiedzi Strona2.](razor-pages-conventions/_static/page2-filter-header.png)

**Konfigurowanie fabryki filtru**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) konfiguruje określoną fabrykę do zastosowania [filtry](xref:mvc/controllers/filters) do wszystkich stron Razor.

Przykładowa aplikacja zawiera przykład za pomocą [filtra](xref:mvc/controllers/filters#ifilterfactory) przez dodanie nagłówka `FilterFactoryHeader`, za pomocą dwóch wartości do stron aplikacji:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[](razor-pages-conventions/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Żądanie próbki o stronę w `localhost:5000/About` i sprawdzić te nagłówki, aby wyświetlić wynik:

![Nagłówki odpowiedzi strony informacje pokazują, czy dwa nagłówki FilterFactoryHeader został dodany.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a>Zastąp strony domyślnego dostawcę modelu aplikacji

Strony razor używa `IPageApplicationModelProvider` interfejs do tworzenia [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider). Może dziedziczyć domyślnego dostawcę modelu zapewnienie własnej implementacji logiki obsługi wykrywania i przetwarzania. Domyślna implementacja ([źródło odwołania](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) ustanawia konwencje *nienazwane* i *o nazwie* program obsługi do nadawania nazw, który został opisany poniżej.

**Domyślne metody obsługi bez nazwy**

Metody obsługi zleceń HTTP (metod obsługi "bez nazwy") postępować zgodnie z Konwencją: `On<HTTP verb>[Async]` (dołączanie `Async` jest opcjonalne, ale zalecane do metod asynchronicznych).

| Metody obsługi bez nazwy     | Operacja                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | Zainicjuj stan strony.     |
| `OnPost`/`OnPostAsync`     | Obsługuje żądania POST.          |
| `OnDelete`/`OnDeleteAsync` | Obsługi żądań DELETE&#8224;. |
| `OnPut`/`OnPutAsync`       | Obsługuje żądania PUT&#8224;.    |
| `OnPatch`/`OnPatchAsync`   | Obsługuje żądania PATCH&#8224;.  |

&#8224;Używane na potrzeby wykonywania wywołań interfejsu API do strony.

**Domyślna nazwana metody obsługi**

Metody obsługi dostarczoną przez dewelopera ("o nazwie" metody obsługi) postępować zgodnie z Konwencją podobne. Nazwa procedury obsługi pojawia się po czasownik HTTP lub między czasownik HTTP i `Async`: `On<HTTP verb><handler name>[Async]` (dołączanie `Async` jest opcjonalne, ale zalecane do metod asynchronicznych). Na przykład metody, które przetwarzają komunikatów może potrwać nazewnictwa przedstawiono w poniższej tabeli.

| Przykład o nazwie metody obsługi             | Przykład operacji        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | Uzyskaj wiadomość.        |
| `OnPostMessage`/`OnPostMessageAsync`     | OPUBLIKUJ wiadomość.          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | Usuwanie wiadomości e-mail&#8224;. |
| `OnPutMessage`/`OnPutMessageAsync`       | Umieść komunikat&#8224;.    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | Stosowanie poprawek do komunikatu&#8224;.  |

&#8224;Używane na potrzeby wykonywania wywołań interfejsu API do strony.

**Dostosowywanie nazwy metod obsługi**

Przyjęto założenie, chcesz zmienić sposób, w jaki metody obsługi nazwane i nienazwane są nazwane. Alternatywny schemat nazewnictwa jest unikać uruchamiania nazwy metod za pomocą opcji "Włączone" i użyj pierwszy segment programu word, aby określić zlecenie HTTP. Można wprowadzić inne zmiany, takich jak konwertowanie zlecenia do usunięcia, PUT i PATCH do wpisu. Taki system oferuje nazwy metod przedstawiono w poniższej tabeli.

| Metody obsługi                       | Operacja                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | Zainicjuj stan strony.     |
| `Post`/`PostAsync`                   | Obsługuje żądania POST.          |
| `Delete`/`DeleteAsync`               | Obsługi żądań DELETE&#8224;. |
| `Put`/`PutAsync`                     | Obsługuje żądania PUT&#8224;.    |
| `Patch`/`PatchAsync`                 | Obsługuje żądania PATCH&#8224;.  |
| `GetMessage`                         | Uzyskaj wiadomość.              |
| `PostMessage`/`PostMessageAsync`     | OPUBLIKUJ wiadomość.                |
| `DeleteMessage`/`DeleteMessageAsync` | OPUBLIKUJ wiadomość do usunięcia.      |
| `PutMessage`/`PutMessageAsync`       | OPUBLIKUJ komunikat do umieszczenia.         |
| `PatchMessage`/`PatchMessageAsync`   | OPUBLIKUJ wiadomość poprawki.       |

&#8224;Używane na potrzeby wykonywania wywołań interfejsu API do strony.

Aby ustanowić ten schemat, dziedziczą z `DefaultPageApplicationModelProvider` klasy, a także Przesłoń [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) metody umożliwiają określanie wartości niestandardowej logiki rozpoznawania [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) nazwy programów obsługi. Przykładowa aplikacja pokazuje, jak to zrobić jego `CustomPageApplicationModelProvider` klasy:

[!code-csharp[](razor-pages-conventions/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

Najważniejsze funkcje klasy obejmują:

* Klasa dziedziczy `DefaultPageApplicationModelProvider`.
* `TryParseHandlerMethod` Przetwarza program obsługi do określenia czasownik HTTP (`httpMethod`) i nazwa procedury obsługi o nazwie (`handlerName`) podczas tworzenia `PageHandlerModel`.
  * `Async` Przyrostkowe jest ignorowana, jeśli jest obecny.
  * Wielkość liter w wyrazie służy do analizowania czasownik HTTP na podstawie nazwy metody.
  * Jeśli nazwa metody (bez `Async`) jest taka sama jak nazwa zlecenie HTTP, brak obsługi o nazwie. `handlerName` Ustawiono `null`, a nazwa metody jest `Get`, `Post`, `Delete`, `Put`, lub `Patch`.
  * Gdy nazwa metody (bez `Async`) jest dłuższa niż nazwa zlecenie HTTP, brak obsługi o nazwie. `handlerName` Ustawiono `<method name (less 'Async', if present)>`. Na przykład nazwę procedury obsługi "GetMessage" yield "GetMessage" i "GetMessageAsync".
  * DELETE, PUT i PATCH HTTP zlecenia są konwertowane na WPIS.

Zarejestruj `CustomPageApplicationModelProvider` w `Startup` klasy:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet10)]

Model strony w *Index.cshtml.cs* pokazuje zmian konwencji nazewnictwa metody zwykłej obsługi dla stron w aplikacji. Zwykłych "Włączone" prefiks nazw używany ze stronami Razor jest usuwany. Metoda, która inicjuje stanu strony nosi teraz nazwę `Get`. Możesz zobaczyć tę Konwencję, używane w całej aplikacji, po otwarciu dowolnego modelu strony dla dowolnej strony.

Każdy z pozostałych metod rozpoczynać zlecenie HTTP, który opisuje jego przetwarzania. Te dwie metody, które zaczyna się `Delete` zwykle będzie traktowany jak zleceń DELETE protokołu HTTP, ale logikę `TryParseHandlerMethod` jawnie ustawia czasownik WPIS dla obu programów obsługi.

Należy pamiętać, że `Async` jest opcjonalny między `DeleteAllMessages` i `DeleteMessageAsync`. Są one obu metod asynchronicznych, ale istnieje możliwość użycia `Async` przyrostka, czy nie; firma Microsoft zaleca wykonanie. `DeleteAllMessages` w tym miejscu służy w celach demonstracyjnych, ale zaleca się, czy nazwa jest taka metoda `DeleteAllMessagesAsync`. Nie ma wpływu na przetwarzanie przykład wdrożenia, ale przy użyciu `Async` przyrostkowe wywołań na fakt, że jest metoda asynchroniczna.

[!code-csharp[](razor-pages-conventions/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

Należy pamiętać, nazwy programów obsługi, podany w *Index.cshtml* dopasowania `DeleteAllMessages` i `DeleteMessageAsync` metody obsługi:

[!code-cshtml[](razor-pages-conventions/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

`Async` Nazwa metody obsługi `DeleteMessageAsync` jest brana pod uwagę na zewnątrz według `TryParseHandlerMethod` do obsługi dopasowania żądania POST do metody. `asp-page-handler` Nazwa `DeleteMessage` pasuje do metody obsługi `DeleteMessageAsync`.

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Filtry MVC i filtr strony (IPageFilter)

MVC [filtry akcji](xref:mvc/controllers/filters#action-filters) są ignorowane przez stron Razor, ponieważ stron Razor korzystać z metody obsługi. Inne typy filtrów platformy MVC są dostępne do użycia: [autoryzacji](xref:mvc/controllers/filters#authorization-filters), [wyjątek](xref:mvc/controllers/filters#exception-filters), [zasobów](xref:mvc/controllers/filters#resource-filters), i [wynik](xref:mvc/controllers/filters#result-filters). Aby uzyskać więcej informacji, zobacz [filtry](xref:mvc/controllers/filters) tematu.

Filtr strony ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) jest filtr, który ma zastosowanie do stron Razor. Aby uzyskać więcej informacji, zobacz [metody filtrowania dla stron Razor](xref:razor-pages/filter).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Konwencje autoryzacja stron razor](xref:security/authorization/razor-pages-authorization)
