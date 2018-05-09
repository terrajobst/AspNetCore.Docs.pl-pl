---
title: Razor strony trasy i aplikacji w Konwencji platformy ASP.NET Core
author: guardrex
description: Odkryj, jak trasy i aplikacji konwencje dostawcy modelu pomaga formantu strony routingu, odnajdywania i przetwarzania.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 04/12/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/razor-pages/razor-pages-conventions
ms.openlocfilehash: 15bb0687ffef777b82ea9374fdc3b92f3af7818b
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/08/2018
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>Razor strony trasy i aplikacji w Konwencji platformy ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

Dowiedz się, jak używać strony [trasy i aplikacjami modelu Konwencji dostawcy](xref:mvc/controllers/application-model#conventions) do kontrolowania strony routingu, odnajdywania i przetwarzania w aplikacjach stron Razor. Gdy trzeba skonfigurować niestandardową stronę tras dla poszczególnych stron, Konfigurowanie routingu do stron z [Konwencji AddPageRoute](#configure-a-page-route) opisane w dalszej części tego tematu.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-conventions/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

::: moniker range="= aspnetcore-2.0"
| Scenariusz | W przykładzie pokazano... |
| -------- | --------------------------- |
| [Konwencje modelu](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | Dodaj szablon trasy i nagłówek do strony aplikacji. |
| [Konwencje akcji trasy strony](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Dodaj szablon trasy do stron w folderze i do jednej strony. |
| [Konwencje akcji modelu strony](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (klasa filtru, wyrażenia lambda lub filtra)</li></ul> | Dodanie nagłówka do stron w folderze, dodanie nagłówka do jednej strony, a następnie skonfiguruj [filtra](xref:mvc/controllers/filters#ifilterfactory) można dodać nagłówka do strony aplikacji. |
| [Domyślnego dostawcę modelu aplikacji strony](#replace-the-default-page-app-model-provider) | Zastąp domyślnego dostawcę modelu strony można zmienić konwencje nazw programu obsługi. |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| Scenariusz | W przykładzie pokazano... |
| -------- | --------------------------- |
| [Konwencje modelu](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | Dodaj szablon trasy i nagłówek do strony aplikacji. |
| [Konwencje akcji trasy strony](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Dodaj szablon trasy do stron w folderze i do jednej strony. |
| [Konwencje akcji modelu strony](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (klasa filtru, wyrażenia lambda lub filtra)</li></ul> | Dodanie nagłówka do stron w folderze, dodanie nagłówka do jednej strony, a następnie skonfiguruj [filtra](xref:mvc/controllers/filters#ifilterfactory) można dodać nagłówka do strony aplikacji. |
| [Domyślnego dostawcę modelu aplikacji strony](#replace-the-default-page-app-model-provider) | Zastąp domyślnego dostawcę modelu strony można zmienić konwencje nazw programu obsługi. |
::: moniker-end

Konwencje stron razor są dodawane i skonfigurować za pomocą [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) metodę rozszerzenie [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc) w kolekcji usługi w `Startup` klasy. Poniższe przykłady Konwencji opisano szczegółowo w dalszej części tego tematu:

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

## <a name="model-conventions"></a>Konwencje modelu

Dodaj obiekt delegowany dla [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) można dodać [modelu Konwencji](xref:mvc/controllers/application-model#conventions) przeznaczonych do stron Razor.

**Dodawanie Konwencji modelu trasy do wszystkich stron**

Użyj [konwencje](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) utworzyć i dodać [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) do kolekcji [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) wystąpień, które są stosowane podczas modelu trasy strony konstrukcja.

Przykładowa aplikacja dodaje `{globalTemplate?}` szablon trasy do wszystkich stron w aplikacji:

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> `Order` Właściwość `AttributeRouteModel` ma ustawioną wartość `-1`. Dzięki temu, ten szablon jest nadawana priorytet pierwszą pozycję wartości danych trasy, jeśli podano wartość trasa, a także że będzie mają pierwszeństwo przed automatycznie generowanych trasy stron Razor. Na przykład dodaje próbki `{aboutTemplate?}` szablon trasy w dalszej części tematu. `{aboutTemplate?}` Podano szablon `Order` z `1`. Po zażądaniu strony informacje w `/About/RouteDataValue`, "RouteDataValue" zostanie załadowana do `RouteData.Values["globalTemplate"]` (`Order = -1`) i nie `RouteData.Values["aboutTemplate"]` (`Order = 1`) z powodu ustawienia `Order` właściwości.

Opcje stron razor, takie jak dodawanie [konwencje](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions), zostaną dodane, gdy MVC zostanie dodany do kolekcji usługi w `Startup.ConfigureServices`. Na przykład zobacz [Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-conventions/sample/).

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet1)]

Żądanie przykładową stronę o w `localhost:5000/About/GlobalRouteValue` i sprawdzić wynik:

![Strona informacje jest pobierana z segment trasy GlobalRouteValue. Renderowanej strony pokazuje, że wartości danych trasy są przechwytywane w metodzie OnGet strony.](razor-pages-conventions/_static/about-page-global-template.png)

**Dodawanie aplikacji modelu Konwencji do wszystkich stron**

Użyj [konwencje](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) utworzyć i dodać [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) do kolekcji [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) wystąpień, które są stosowane podczas modelu aplikacji strony konstrukcja.

Aby zademonstrować to i inne konwencje w dalszej części tematu, zawiera przykładową aplikację `AddHeaderAttribute` klasy. Konstruktor klasy akceptuje `name` ciągu i `values` tablicy ciągów. Te wartości są używane w jego `OnResultExecuting` metodę, aby ustawić nagłówka odpowiedzi. Pełna klasy jest wyświetlany w obszarze [strony konwencje akcji modelu](#page-model-action-conventions) dalszej części tematu.

Przykładowe zastosowania aplikacji `AddHeaderAttribute` klasy można dodać nagłówka `GlobalHeader`, do wszystkich stron w aplikacji:

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet2)]

Żądanie przykładową stronę o w `localhost:5000/About` i sprawdzić nagłówków, aby wyświetlić wyniki:

![Nagłówki odpowiedzi strony informacje zawierają GlobalHeader został dodany.](razor-pages-conventions/_static/about-page-global-header.png)

::: moniker range=">= aspnetcore-2.1"
**Dodawanie obsługi modelu Konwencji do wszystkich stron**



Użyj [konwencje](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) utworzyć i dodać [IPageHandlerModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipagehandlermodelconvention) do kolekcji [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) wystąpień, które są stosowane podczas modelu obsługi strony konstrukcja.

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

Domyślny dostawca modelu trasy, która pochodzi z [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) wywołuje Konwencji, których celem jest punkty rozszerzeń do konfigurowania tras strony.

**Folder trasy modelu Konwencji**

Użyj [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) utworzyć i dodać [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) który wywołuje akcję na [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) dla wszystkich stron w obszarze określony folder.

Przykładowe zastosowania aplikacji `AddFolderRouteModelConvention` można dodać `{otherPagesTemplate?}` szablon trasy do stron w *OtherPages* folderu:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> `Order` Właściwość `AttributeRouteModel` ma ustawioną wartość `1`. Gwarantuje to, że szablon `{globalTemplate?}` (zestaw we wcześniejszej części tematu) jest priorytet dla pierwsze dane trasy wartości pozycji, gdy została podana wartość jedną trasę. Jeśli żądanie strony Strona 1 na `/OtherPages/Page1/RouteDataValue`, "RouteDataValue" zostanie załadowana do `RouteData.Values["globalTemplate"]` (`Order = -1`) i nie `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) z powodu ustawienia `Order` właściwości.

Przykładowe strony Strona 1 na `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` i sprawdzić wynik:

![Strona 1 w folderze OtherPages jest wymagany z segmentem trasy GlobalRouteValue i OtherPagesRouteValue. Renderowanej strony pokazuje, że wartości danych trasy są przechwytywane w metodzie OnGet strony.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

**Strona trasy modelu Konwencji**

Użyj [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) utworzyć i dodać [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) który wywołuje akcję na [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) strony z określonym Nazwa.

Przykładowe zastosowania aplikacji `AddPageRouteModelConvention` można dodać `{aboutTemplate?}` szablon trasy do strony informacje:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> `Order` Właściwość `AttributeRouteModel` ma ustawioną wartość `1`. Gwarantuje to, że szablon `{globalTemplate?}` (zestaw we wcześniejszej części tematu) jest priorytet dla pierwsze dane trasy wartości pozycji, gdy została podana wartość jedną trasę. Jeśli żądanie na stronie informacje `/About/RouteDataValue`, "RouteDataValue" zostanie załadowana do `RouteData.Values["globalTemplate"]` (`Order = -1`) i nie `RouteData.Values["aboutTemplate"]` (`Order = 1`) z powodu ustawienia `Order` właściwości.

Żądanie przykładową stronę o w `localhost:5000/About/GlobalRouteValue/AboutRouteValue` i sprawdzić wynik:

![Strona informacje jest wymagany z segmentami trasy dla GlobalRouteValue i AboutRouteValue. Renderowanej strony pokazuje, że wartości danych trasy są przechwytywane w metodzie OnGet strony.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>Konfiguruj trasy strony

Użyj [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) do konfigurowania trasy do strony w ścieżce określonej strony. Wygenerowany łącza do strony korzystać z określoną trasę. `AddPageRoute` używa `AddPageRouteModelConvention` ustanowienie trasy.

Przykładowa aplikacja tworzy trasę do `/TheContactPage` dla *Contact.cshtml*:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet5)]

Strony kontaktu można również można uzyskać pod adresem `/Contact` za pomocą tej trasy domyślnej.

Przykładowa aplikacja tras niestandardowych na stronie kontakt umożliwia opcjonalny `text` trasy segmentu (`{text?}`). Strona zawiera również ten segment opcjonalne w jego `@page` dyrektywy w przypadku, gdy użytkownik uzyskuje dostęp do strony o jego `/Contact` trasy:

[!code-cshtml[](razor-pages-conventions/sample/Pages/Contact.cshtml?highlight=1)]

Należy pamiętać, że wygenerowany adres URL dla **skontaktuj się z** łącza na renderowanej stronie odzwierciedla zaktualizowane trasy:

![Przykładowy link skontaktuj się z aplikacji na pasku nawigacyjnym](razor-pages-conventions/_static/contact-link.png)

![Zapoznanie się łącze kontaktu w kodzie HTML renderowanych wskazuje, że odwołanie jest ustawiona na "/ TheContactPage"](razor-pages-conventions/_static/contact-link-source.png)

Odwiedź stronę kontaktu w albo jego zwykłej trasy `/Contact`, lub tras niestandardowych `/TheContactPage`. Jeśli podasz dodatkowe `text` segmentu trasy, na stronie znajdują się segmentu kodowany w formacie HTML podanie:

![Przykład przeglądarki Microsoft Edge podawania opcjonalne 'text' segment trasy "Wartość tekstowa" w adresie URL. Renderowanej strony zawiera wartość segmentu 'text'.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Konwencje akcji modelu strony

Domyślny dostawca modelu strony implementujący [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) wywołuje Konwencji, których celem jest punkty rozszerzeń do konfigurowania strony modeli. Konwencje te są przydatne w przypadku tworzenia i modyfikowania strony odnajdywania i scenariusze przetwarzania.

Przykłady w tej sekcji, przykładowa aplikacja używa `AddHeaderAttribute` klasy, która jest [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), który dotyczy nagłówek odpowiedzi:

[!code-csharp[](razor-pages-conventions/sample/Filters/AddHeader.cs?name=snippet1)]

Za pomocą Konwencji, przykładzie pokazano, jak do zastosowania atrybutu do wszystkich stron w folderze i do jednej strony.

**Folder aplikacji modelu Konwencji**

Użyj [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) utworzyć i dodać [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) który wywołuje akcję na [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) wystąpienia dla wszystkich stron w określonym folderze.

W przykładzie pokazano użycie `AddFolderApplicationModelConvention` przez dodanie nagłówka `OtherPagesHeader`, do stron wewnątrz *OtherPages* folderu aplikacji:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet6)]

Przykładowe strony Strona 1 na `localhost:5000/OtherPages/Page1` i sprawdzić nagłówków, aby wyświetlić wyniki:

![Nagłówki odpowiedzi strony strona OtherPages/1 pokazują, że dodano OtherPagesHeader.](razor-pages-conventions/_static/page1-otherpages-header.png)

**Strona aplikacji modelu Konwencji**

Użyj [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) utworzyć i dodać [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) który wywołuje akcję na [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) strony o nazwie speciifed.

W przykładzie pokazano użycie `AddPageApplicationModelConvention` przez dodanie nagłówka `AboutHeader`, do strony informacje:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet7)]

Żądanie przykładową stronę o w `localhost:5000/About` i sprawdzić nagłówków, aby wyświetlić wyniki:

![Nagłówki odpowiedzi strony informacje zawierają AboutHeader został dodany.](razor-pages-conventions/_static/about-page-about-header.png)

**Konfiguruj filtr**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) konfiguruje określony filtr do zastosowania. Można zaimplementować klasy filtru, ale Przykładowa aplikacja pokazano, jak implementuje filtr w wyrażeniu lambda, które zostało zaimplementowane jako fabryka, która zwraca filtr wewnętrznych:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet8)]

Model aplikacji strony służy do sprawdzania ścieżki względnej segmentów, które mogą prowadzić do strony Strona2 *OtherPages* folderu. Jeśli warunek zakończy się pomyślnie, jest dodawana nagłówka. Jeśli nie, `EmptyFilter` została zastosowana.

`EmptyFilter` jest [filtr akcji](xref:mvc/controllers/filters#action-filters). Ponieważ filtry akcji są ignorowane przez stron Razor `EmptyFilter` ops nie zgodnie z oczekiwaniami, jeśli ścieżka nie zawiera `OtherPages/Page2`.

Przykładowe strony Strona2 w `localhost:5000/OtherPages/Page2` i sprawdzić nagłówków, aby wyświetlić wyniki:

![OtherPagesPage2Header jest dodawany do odpowiedzi do Strona2.](razor-pages-conventions/_static/page2-filter-header.png)

**Skonfiguruj fabryki filtru**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) konfiguruje określony fabryka do zastosowania [filtry](xref:mvc/controllers/filters) do wszystkich stron Razor.

Przykładowa aplikacja zawiera przykład za pomocą [filtra](xref:mvc/controllers/filters#ifilterfactory) przez dodanie nagłówka `FilterFactoryHeader`, z dwóch wartości do stron aplikacji:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[](razor-pages-conventions/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Żądanie przykładową stronę o w `localhost:5000/About` i sprawdzić nagłówków, aby wyświetlić wyniki:

![Nagłówki odpowiedzi strony informacje zawierają dodano dwa nagłówki FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a>Zastąp domyślnego dostawcę modelu aplikacji strony

Używa stron razor `IPageApplicationModelProvider` interfejs do tworzenia [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider). Może dziedziczyć domyślnego dostawcy modelu w celu zapewnienia logika implementacji programu obsługi wykrywania i przetwarzania. Domyślna implementacja ([źródło odwołania](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) ustanawia konwencjach *nienazwane* i *o nazwie* obsługi nazw, które jest opisane poniżej.

**Domyślne metody obsługi bez nazwy**

Metody obsługi zleceń HTTP ("bez nazwy" obsługi metody) wykonaj Konwencji: `On<HTTP verb>[Async]` (dołączanie `Async` jest opcjonalne, ale zalecane do metod asynchronicznych).

| Metoda obsługi bez nazwy     | Operacja                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | Inicjowanie stan strony.     |
| `OnPost`/`OnPostAsync`     | Obsługuje żądania POST.          |
| `OnDelete`/`OnDeleteAsync` | Obsługi żądań DELETE&#8224;. |
| `OnPut`/`OnPutAsync`       | Obsługuje żądania PUT&#8224;.    |
| `OnPatch`/`OnPatchAsync`   | Obsługuje żądania PATCH&#8224;.  |

&#8224;Używany dla wywołań interfejsu API do strony.

**Domyślną nazwę metody procedury obsługi**

Metody obsługi udostępniane przez deweloperów ("o nazwie" metod obsługi) wykonaj podobne Konwencji. Nazwa programu obsługi pojawia się po zlecenie HTTP lub między zlecenie HTTP i `Async`: `On<HTTP verb><handler name>[Async]` (dołączanie `Async` jest opcjonalne, ale zalecane do metod asynchronicznych). Na przykład metod, które przetwarzają wiadomości może potrwać nazewnictwa przedstawiono w poniższej tabeli.

| Przykład o nazwie obsługi — metoda             | Przykład działania        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | Uzyskiwania wiadomości.        |
| `OnPostMessage`/`OnPostMessageAsync`     | OPUBLIKOWANIE wiadomości.          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | Usuwanie wiadomości&#8224;. |
| `OnPutMessage`/`OnPutMessageAsync`       | Umieść komunikat&#8224;.    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | POPRAWKA komunikat&#8224;.  |

&#8224;Używany dla wywołań interfejsu API do strony.

**Dostosowywanie obsługi nazw — metoda**

Przykładowa wolisz zmienić sposób metody nazwane i nienazwane procedury obsługi są nazywane. Alternatywne schemat nazewnictwa jest uniknąć uruchamiania nazwy metody z "On" i użyj pierwszy segment word, aby określić zlecenie HTTP. Można wprowadzić inne zmiany, takie jak konwertowanie zleceń do usunięcia, PUT i poprawka POST. Taki system zawiera nazwy metod przedstawiono w poniższej tabeli.

| Metoda obsługi                       | Operacja                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | Inicjowanie stan strony.     |
| `Post`/`PostAsync`                   | Obsługuje żądania POST.          |
| `Delete`/`DeleteAsync`               | Obsługi żądań DELETE&#8224;. |
| `Put`/`PutAsync`                     | Obsługuje żądania PUT&#8224;.    |
| `Patch`/`PatchAsync`                 | Obsługuje żądania PATCH&#8224;.  |
| `GetMessage`                         | Uzyskiwania wiadomości.              |
| `PostMessage`/`PostMessageAsync`     | OPUBLIKOWANIE wiadomości.                |
| `DeleteMessage`/`DeleteMessageAsync` | OPUBLIKOWANIE wiadomości do usunięcia.      |
| `PutMessage`/`PutMessageAsync`       | Komunikat, aby umieścić POST.         |
| `PatchMessage`/`PatchMessageAsync`   | OPUBLIKOWANIE wiadomości do poprawki.       |

&#8224;Używany dla wywołań interfejsu API do strony.

Ustanowienie ten schemat dziedziczyć `DefaultPageApplicationModelProvider` klasy i zastąpienia [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) metodę, aby podać niestandardową logikę na potrzeby rozpoznawania [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) nazwy programu obsługi. Przykładowa aplikacja pokazuje, jak to zrobić jego `CustomPageApplicationModelProvider` klasy:

[!code-csharp[](razor-pages-conventions/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

Najważniejsze funkcje klasy obejmują:

* Klasa dziedziczy `DefaultPageApplicationModelProvider`.
* `TryParseHandlerMethod` Przetwarza program obsługi w celu określenia zlecenie HTTP (`httpMethod`) i nazwa obsługi o nazwie (`handlerName`) podczas tworzenia `PageHandlerModel`.
  * `Async` Przyrostek jest ignorowana, jeśli jest obecny.
  * Wielkość liter jest używany do analizy zlecenie HTTP z nazwę metody.
  * Gdy nazwa metody (bez `Async`) jest taka sama jak nazwa zlecenie HTTP, braku nazwanego obsługi. `handlerName` Ustawiono `null`, a nazwa metody jest `Get`, `Post`, `Delete`, `Put`, lub `Patch`.
  * Gdy nazwa metody (bez `Async`) jest dłuższy niż nazwa zlecenie HTTP, nazwany program obsługi. `handlerName` Ma ustawioną wartość `<method name (less 'Async', if present)>`. Na przykład zarówno "GetMessage" i "GetMessageAsync" yield "GetMessage" Nazwa programu obsługi.
  * DELETE, PUT i PATCH HTTP zlecenia są konwertowane na POST.

Zarejestruj `CustomPageApplicationModelProvider` w `Startup` klasy:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet10)]

Model strony w *Index.cshtml.cs* pokazuje zmian konwencji nazewnictwa metody zwykłej obsługi dla stron w aplikacji. Zwykły "On" nazewnictwa prefiks używany ze stronami Razor zostaną usunięte. Teraz nosi nazwę metody, która inicjuje stanu strony `Get`. Widać tę Konwencję używane w całej aplikacji po otwarciu dowolnego modelu strony dla dowolnej strony.

Każda z innych metod zaczynać się zlecenie HTTP opisujące jego przetwarzanie. Te dwie metody, które zaczynają się `Delete` zazwyczaj będzie traktowane jako zleceń HTTP, usuwanie, ale logikę `TryParseHandlerMethod` jawnie ustawia zlecenie POST dla obu programów obsługi.

Należy pamiętać, że `Async` jest opcjonalny między `DeleteAllMessages` i `DeleteMessageAsync`. Są one obu metod asynchronicznych, ale mogą być używane `Async` przyrostka lub nie; zaleca się wykonanie. `DeleteAllMessages` jest tu używany do celów demonstracyjnych, ale zaleca się, że nazwa taka metoda `DeleteAllMessagesAsync`. Nie ma wpływu na przetwarzanie próbki implementacji, ale przy użyciu `Async` przyrostka wywołania na fakt, że jest metody asynchronicznej.

[!code-csharp[](razor-pages-conventions/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

Zanotuj nazwy obsługi w *Index.cshtml* odpowiada `DeleteAllMessages` i `DeleteMessageAsync` metod obsługi:

[!code-cshtml[](razor-pages-conventions/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

`Async` w nazwie metody obsługi `DeleteMessageAsync` jest brana pod uwagę limit przez `TryParseHandlerMethod` do obsługi dopasowania żądania POST do metody. `asp-page-handler` Nazwa `DeleteMessage` jest dopasowywany do metody obsługi `DeleteMessageAsync`.

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Filtry MVC i filtr strony (IPageFilter)

MVC [filtry akcji](xref:mvc/controllers/filters#action-filters) są ignorowane przez Razor strony, ponieważ stron Razor użyć metod obsługi. Inne rodzaje MVC filtry są dostępne do użycia: [autoryzacji](xref:mvc/controllers/filters#authorization-filters), [wyjątek](xref:mvc/controllers/filters#exception-filters), [zasobów](xref:mvc/controllers/filters#resource-filters), i [wynik](xref:mvc/controllers/filters#result-filters). Aby uzyskać więcej informacji, zobacz [filtry](xref:mvc/controllers/filters) tematu.

Filtr strony ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) jest filtr, który ma zastosowanie do stron Razor. Aby uzyskać więcej informacji, zobacz [filtrować metod dla stron Razor](xref:mvc/razor-pages/filter).

## <a name="see-also"></a>Zobacz także

* [Konwencje autoryzacji stron razor](xref:security/authorization/razor-pages-authorization)
