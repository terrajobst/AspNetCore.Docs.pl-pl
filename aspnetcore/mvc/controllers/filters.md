---
title: Filtry platformy ASP.NET Core
author: ardalis
description: Dowiedz się, jak działają filtry i sposobu ich używania w programie ASP.NET MVC Core.
manager: wpickett
ms.author: riande
ms.date: 4/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/filters
ms.openlocfilehash: 24e754daa68d5247fa444e87ba733891c908d32c
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
---
# <a name="filters-in-aspnet-core"></a>Filtry platformy ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Dykstra Tomasz](https://github.com/tdykstra/), i [Steve Smith](https://ardalis.com/)

*Filtry* na platformie ASP.NET Core MVC pozwala na uruchamianie kodu przed lub po określonym etapy potoku przetwarzania żądań.

> [!IMPORTANT]
> Ten temat jest **nie** dotyczą stron Razor. Podgląd platformy ASP.NET Core 2.1 i nowsze wersje obsługują [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) i [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) dla stron Razor. Aby uzyskać więcej informacji, zobacz [filtrować metod dla stron Razor](xref:mvc/razor-pages/filter).

 Filtry wbudowane realizacji zadań, takich jak:
 
 * Autoryzacji (uniemożliwienia dostępu do zasobów, które użytkownik nie jest autoryzowany do).
 * Zapewnienie, że wszystkie żądania przy użyciu protokołu HTTPS.
 * Odpowiedź buforowania (zwarcie Potok żądań do zwrócenia buforowanej odpowiedzi). 

Filtry niestandardowe można tworzyć do obsługi kompleksowymi problemy. Filtry można uniknąć duplikowania kodu przez akcje. Na przykład błąd obsługi filtra wyjątku można skonsolidować obsługi błędów.

[Wyświetlanie lub pobieranie próbki z usługi GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).

## <a name="how-do-filters-work"></a>Jak działają filtry?

Filtry są uruchamiane w ramach *potok wywołania akcji MVC*, jest czasami nazywany *potoku filtru*.  Potoku filtru jest uruchamiana po MVC wybiera akcję do wykonania.

![Żądanie jest przetwarzane przez inne oprogramowanie pośredniczące, Routing oprogramowania pośredniczącego, wybór akcji i potok MVC wywołania akcji. Przetwarzanie żądań jest nadal powrotem przy użyciu wybór akcji, Routing oprogramowanie pośredniczące i różne inne oprogramowanie pośredniczące aby stać się wysłać do klienta odpowiedź.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>Typy filtrów

Każdego typu filtru jest wykonywana na różnych etapach potoku filtru.

* [Filtry autoryzacji](#authorization-filters) uruchomiony i są używane do ustalenia, czy bieżący użytkownik jest autoryzowany dla bieżącego żądania. Jeśli żądanie jest autoryzowane, ich zwarcia potoku. 

* [Filtry zasobów](#resource-filters) są najpierw do obsługi żądania po autoryzacji.  Mogą uruchamiać kod przed rest potoku filtru, a po zakończeniu pozostałego potoku. Są one przydatne do wdrożenia, buforowanie lub w przeciwnym razie zwarcia potoku filtru ze względu na wydajność. Są uruchamiane przed powiązanie modelu, dlatego może mieć wpływ wiązania modelu.

* [Filtry akcji](#action-filters) można uruchomić kod bezpośrednio przed i po jest wywoływana metoda poszczególnych akcji. Mogą one używane do manipulowania Argumenty przekazane do akcji i wyniku zwracanego z akcji.

* [Filtry wyjątków](#exception-filters) są używane do stosowania zasad globalnych do nieobsługiwanych wyjątków, które występują przed niczego została zapisana treść odpowiedzi.

* [Powoduje filtry](#result-filters) można uruchomić kod bezpośrednio przed i po wykonaniu wyniki poszczególnych akcji. Działają tylko wtedy, gdy metoda akcji została wykonana pomyślnie. Są one przydatne w przypadku logiki, które należy ująć wykonywania widoku lub element formatujący.

Na poniższym diagramie przedstawiono sposób interakcji te typy filtrów w potoku filtru.

![Żądanie jest przetwarzane za pośrednictwem filtry autoryzacji, filtrów zasobów powiązania modelu, filtry akcji, wykonywanie akcji i konwersji wynik akcji, filtry wyjątków, filtry wyników i wykonania wyniku. W drodze limit żądania jest przetwarzany tylko przez filtry wyników i filtry zasobów aby stać się odpowiedzi wysyłane do klienta.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>Implementacja

Filtry obsługuje synchroniczne i asynchroniczne implementacje za pośrednictwem definicji innego interfejsu. 

Synchroniczne filtry, które można uruchomić kod zarówno przed i po ich etap potoku zdefiniować*etap*Executing i na*etap*wykonywane metody. Na przykład `OnActionExecuting` jest wywoływana przed wywołaniem metody akcji i `OnActionExecuted` jest wywoływana po powrocie z metody akcji.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet1)]

Asynchroniczne Filtry definiują jeden na*etap*ExecutionAsync metody. Ta metoda przyjmuje *FilterType*ExecutionDelegate delegata, który wykonuje etap potoku filtru. Na przykład `ActionExecutionDelegate` wywołania metody akcji i może zostać uruchomiony kod przed i po jej wywołaniu.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

Dla wielu etapów filtrów w jednej klasy mogą implementować interfejsów. Na przykład [ActionFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute?view=aspnetcore-2.0) klasa implementuje `IActionFilter`, `IResultFilter`i ich odpowiedniki asynchronicznego.

> [!NOTE]
> Implementowanie **albo** synchronicznego lub wersji asynchronicznego interfejsu filtru, nie oba. Platformę najpierw sprawdza, czy filtr implementuje interfejs asynchronicznych, a jeśli tak, który wywołuje. Jeśli nie wywołuje metody interfejsu synchronicznego. Gdyby implementować interfejsów zarówno na jedną klasę, czy można wywołać tylko metody asynchronicznej. Podczas używania klas abstrakcyjnych, takich jak [ActionFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute?view=aspnetcore-2.0) należy przesłonić metodę metod synchronicznych lub metody asynchronicznej dla każdego typu filtru.

### <a name="ifilterfactory"></a>IFilterFactory

`IFilterFactory` implementuje `IFilter`. W związku z tym `IFilterFactory` wystąpienie może być używane jako `IFilter` wystąpienia dowolne miejsce w potoku filtru. Gdy w ramach przygotowuje się do wywołania filtru, próbuje rzutować go na `IFilterFactory`. Jeśli tego rzutowania zakończy się powodzeniem, `CreateInstance` metoda jest wywoływana w celu utworzenia `IFilter` wystąpienia, która będzie wywołana. Zapewnia to elastycznością, ponieważ potoku filtru dokładne nie trzeba ustawić jawnie po uruchomieniu aplikacji.

Można zaimplementować `IFilterFactory` na własnych implementacje atrybut jako innego podejścia do tworzenia filtrów:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a>Wbudowany filtr atrybutów

Struktura obejmuje wbudowane opartych na atrybutach filtrów, możesz podklasy i dostosowywać. Na przykład następujący filtr wynik dodaje do odpowiedzi nagłówek.

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

Atrybuty zezwalać filtrów, aby akceptować argumenty, jak pokazano w przykładzie powyżej. Czy dodać ten atrybut do kontrolera lub metody akcji i określ nazwę i wartość nagłówka HTTP:

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

Wynik `Index` akcji są wyświetlane poniżej — nagłówki odpowiedzi są wyświetlane w prawym dolnym rogu.

![Developer Tools Microsoft Edge przedstawiający nagłówki odpowiedzi, w tym Steve Smith autora @ardalis](filters/_static/add-header.png)

Kilka interfejsów filtr ma odpowiednie atrybuty, które mogą służyć jako klasy podstawowe dla implementacji niestandardowych.

Atrybuty filtru:

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

`TypeFilterAttribute` i `ServiceFilterAttribute` omówiono [dalszej części tego artykułu](#dependency-injection).

## <a name="filter-scopes-and-order-of-execution"></a>Filtr zakresów i kolejność wykonywania

Filtr można dodać do potoku w jednej z trzech *zakresy*. Można dodać filtr do konkretnej metody akcji lub do klasy kontrolera przy użyciu atrybutu. Lub możesz zarejestrować filtr globalny dla wszystkich kontrolerów i akcji. Filtry są dodawane globalnie, dodając ją do `MvcOptions.Filters` kolekcji w `ConfigureServices`:

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a>Domyślna kolejność wykonywania

Gdy istnieje wiele filtrów do określonego etapu w potoku, zakres Określa domyślną kolejność wykonywania filtrów.  Filtry globalne przestrzenny filtrów klasy, które z kolei przestrzenny filtry metody. Jest to czasami określane do jako zagnieżdżenia "Rosyjski lalki", ponieważ każde zwiększenie zakresu jest otaczający poprzedniej zakresu, takich jak [zagnieżdżenia lalki](https://wikipedia.org/wiki/Matryoshka_doll). Zastępowanie zachowanie jest zwykle uzyskać bez konieczności jawnego określenia kolejności.

W wyniku tego zagnieżdżenia *po* wykonywania kodu filtrów w odwrotnej kolejności *przed* kodu. Sekwencja wygląda następująco:

* *Przed* kodu filtry stosowane globalnie
  * *Przed* kodu filtry stosowane do kontrolerów
    * *Przed* kodu filtry stosowane do metody akcji
    * *Po* kodu filtry stosowane do metody akcji
  * *Po* kodu filtry stosowane do kontrolerów
* *Po* kodu filtry stosowane globalnie
  
Oto przykład ilustrujący kolejności, w którym filtr metody są wywoływane synchroniczne filtrów akcji.

| Sekwencja | Zakresu filtru | Filter — metoda |
|:--------:|:------------:|:-------------:|
| 1 | Global | `OnActionExecuting` |
| 2 | Kontrolera | `OnActionExecuting` |
| 3 | Metoda | `OnActionExecuting` |
| 4 | Metoda | `OnActionExecuted` |
| 5 | Kontrolera | `OnActionExecuted` |
| 6 | Global | `OnActionExecuted` |

Ta sekwencja zawiera:

* Filtr metody jest zagnieżdżony w filtrze kontrolera.
* Filtr kontrolera jest zagnieżdżony w filtrów globalnych. 

Go umieścić inny sposób, jeśli wewnątrz filtr async obiektu*etap*ExecutionAsync, wszystkie filtry z większego zakresu można uruchomić metody zablokowaniu kodu na stosie.

> [!NOTE]
> Każdy kontroler, który dziedziczy z `Controller` klasa podstawowa zawiera `OnActionExecuting` i `OnActionExecuted` metody. Te metody zawijać systemem filtry dla danej akcji: `OnActionExecuting` jest wywoływana przed każdą z filtrów, i `OnActionExecuted` jest wywoływana po wykonaniu wszystkich filtrów.

### <a name="overriding-the-default-order"></a>Zastępowanie domyślna kolejność

Można zastąpić domyślną sekwencją wykonywania zaimplementowanie `IOrderedFilter`. Ten interfejs przedstawia `Order` właściwość, która ma pierwszeństwo przed zakres, aby określić kolejność wykonywania. Filtr o niższej `Order` będzie mieć wartość jego *przed* kod wykonywany przed nim filtr o wyższej wartości `Order`. Filtr o niższej `Order` będzie mieć wartość jego *po* kod wykonywany po filtru o wyższej `Order` wartość. Można ustawić `Order` właściwości przy użyciu parametru konstruktora:

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

Jeśli mają taki sam 3 filtry działania wyświetlane w poprzednim przykładzie, ale zestaw `Order` właściwości kontrolera i globalnych filtrów 1 i 2 odpowiednio, będzie można odwrócić kolejność wykonywania.

| Sekwencja | Zakresu filtru | `Order` Właściwość | Filter — metoda |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1 | Metoda | 0 | `OnActionExecuting` |
| 2 | Kontrolera | 1  | `OnActionExecuting` |
| 3 | Global | 2  | `OnActionExecuting` |
| 4 | Global | 2  | `OnActionExecuted` |
| 5 | Kontrolera | 1  | `OnActionExecuted` |
| 6 | Metoda | 0  | `OnActionExecuted` |

`Order` Atu właściwości zakresu podczas określania kolejność uruchamiania filtrów. Filtry są najpierw posortowane według kolejności, a następnie zakres jest używany do dzielenia ties. Wszystkie filtry wbudowane zaimplementować `IOrderedFilter` i ustaw wartość domyślna `Order` wartość na 0. FIR wbudowanych filtrów zakres określa porządek, chyba że ustawisz `Order` na wartość inną niż zero.

## <a name="cancellation-and-short-circuiting"></a>Anulowanie i krótki circuiting

Potoku filtru w dowolnym momencie można zwarcia przez ustawienie `Result` właściwość `context` parametrów przekazane do metody filtru. Na przykład następujący filtr zasobów uniemożliwia wykonywanie pozostałego potoku.

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

W poniższym kodzie zarówno `ShortCircuitingResourceFilter` i `AddHeader` filtr docelowy `SomeResource` metody akcji. `ShortCircuitingResourceFilter`:

* Uruchamia najpierw, ponieważ jest on filtr zasobów i `AddHeader` jest filtr akcji.
* Short-Circuits pozostałego potoku.

W związku z tym `AddHeader` filtru nigdy nie jest uruchomione `SomeResource` akcji. To zachowanie będzie taki sam, jeśli oba filtry zostały zastosowane na poziomie — metoda akcji, podany `ShortCircuitingResourceFilter` uruchomiono pierwszy. `ShortCircuitingResourceFilter` Jest uruchamiany pierwszy ze względu na jego typ filtru lub przy użyciu jawnych `Order` właściwości.

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>Iniekcji zależności

Filtry można dodać według typu lub wystąpienia. Jeśli dodasz wystąpienia to wystąpienie będzie używany dla każdego żądania. Jeśli dodasz typu będą typu została aktywowana, co oznacza, będzie można utworzyć wystąpienia dla każdego żądania oraz wszelkie zależności konstruktora zostaną wypełnione przez [iniekcji zależności](../../fundamentals/dependency-injection.md) (Podpisane). Dodawanie filtru według typu jest odpowiednikiem `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`.

Filtry, które są zaimplementowane jako atrybuty i dodać bezpośrednio do klasy kontrolera lub metody akcji nie może mieć zależności konstruktora dostarczonych przez [iniekcji zależności](../../fundamentals/dependency-injection.md) (Podpisane). Jest to spowodowane atrybutów musi mieć ich parametrami konstruktora dostarczony, w których są stosowane. Jest to ograniczenie, jak sprawdzić atrybuty.

Jeśli filtry zależności, które chcą korzystać z Podpisane, jest kilka metod obsługiwanych. Filtr można zastosować do klasy lub metody akcji przy użyciu jednej z następujących czynności:

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* `IFilterFactory` na atrybut

> [!NOTE]
> Jeden zależności, które można pobrać z Podpisane jest rejestrator. Należy jednak unikać tworzenia i używania filtrów wyłącznie w celach rejestrowania, ponieważ [funkcji rejestrowania wbudowana struktura](xref:fundamentals/logging/index) już może udostępnić, co jest potrzebne. Jeśli zamierzasz dodać rejestrowania do filtrów, należy skoncentrować się na problemy biznesowe domeny lub zachowanie specyficzne dla filtru, a nie MVC akcje lub inne zdarzenia framework.

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

A `ServiceFilter` pobiera wystąpienia filtru z Podpisane. Dodaj filtr do kontenera w `ConfigureServices`i odwołuje się on w `ServiceFilter` atrybutu

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

Przy użyciu `ServiceFilter` bez rejestrowania wyników filtrowania typ wyjątku:

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

`ServiceFilterAttribute` implementuje `IFilterFactory`. `IFilterFactory` przedstawia `CreateInstance` metodę tworzenia `IFilter` wystąpienia. `CreateInstance` Metody ładuje określonego typu z kontenera usługi (Podpisane).

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute` przypomina `ServiceFilterAttribute`, ale jego typ nie zostanie rozwiązany bezpośrednio z kontenera Podpisane. Tworzy wystąpienie typu przy użyciu `Microsoft.Extensions.DependencyInjection.ObjectFactory`.

Z powodu tej różnicy:

* Typy, które są używane, za pomocą `TypeFilterAttribute` nie trzeba najpierw zarejestrowane z kontenerem.  Mają zależności są spełnione przez kontener. 
* `TypeFilterAttribute` Opcjonalnie mogą akceptować argumenty konstruktora dla typu. 

W poniższym przykładzie pokazano sposób przekazać argumenty do typu przy użyciu `TypeFilterAttribute`:

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

Jeśli masz filtr który:

* Nie wymagają żadnych argumentów.
* Ma zależności konstruktora, które muszą zostać wypełnione przez Podpisane.

Można użyć własnych nazwanego atrybutu na klasy i metody zamiast `[TypeFilter(typeof(FilterType))]`). Następujący filtr pokazuje, jak to można zaimplementować:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

Ten filtr można zastosować do klasy lub metody za pomocą `[SampleActionFilter]` składni, zamiast `[TypeFilter]` lub `[ServiceFilter]`.

## <a name="authorization-filters"></a>Filtry autoryzacji

* Filtry autoryzacji:
* Kontrola dostępu do metody akcji.
* To pierwszy filtry, które ma być wykonywana w potoku filtru. 
* Ma przed metodą, ale nie po metody. 

Należy tylko zapisać filtr autoryzacji niestandardowej Jeśli piszesz własne framework autoryzacji. Preferowane jest konfigurowanie zasad autoryzacji lub zapisywanie niestandardowych zasad autoryzacji przez zapisywanie filtru niestandardowego. Implementacja wbudowany filtr tylko odpowiada za wywołanie systemu autoryzacji.

Nie zgłaszają wyjątki w filtry autoryzacji, ponieważ nic nie będzie obsługiwać wyjątek (filtry wyjątków nie będzie obsługiwać je). Rozważ wystawienie żądanie po wystąpieniu wyjątku.

Dowiedz się więcej o [autoryzacji](../../security/authorization/index.md).

## <a name="resource-filters"></a>Filtry zasobów

* Implementowanie albo `IResourceFilter` lub `IAsyncResourceFilter` interfejsu
* Ich wykonanie powiela większość potoku filtru. 
* Tylko [filtry autoryzacji](#authorization-filters) są uruchamiane przed filtrami zasobów.

Filtry zasobów są przydatne do zwarcia większość pracy, który wykonuje żądanie. Na przykład filtr buforowania można uniknąć pozostałego potoku, jeżeli odpowiedź znajduje się w pamięci podręcznej.

[Krótki circuiting filtru zasobów](#short-circuiting-resource-filter) przedstawiona wcześniej jest jednym z przykładów filtr zasobów. Innym przykładem jest [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):

* Zapobiega wiązania modelu uzyskanie dostępu do danych formularza. 
* Jest przydatne w przypadku wysyłania dużych plików i chcesz uniemożliwić formularzu odczytywania do pamięci.

## <a name="action-filters"></a>Filtry akcji

*Filtry akcji*:

* Implementowanie albo `IActionFilter` lub `IAsyncActionFilter` interfejsu.
* Wykonanie ich wokół wykonywanie metod akcji.

Oto przykładowy filtr akcji:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

[ActionExecutingContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext) zapewnia następujące właściwości:

* `ActionArguments` — umożliwia manipulowanie wejść do akcji.
* `Controller` — umożliwia manipulowanie wystąpienie kontrolera. 
* `Result` — to ustawienie short-circuits wykonywanie metody akcji i filtry akcji kolejne. Zgłaszanie wyjątku powoduje również uniemożliwia wykonanie metody akcji i kolejne filtrów, ale jest traktowana jako błąd zamiast pomyślnego wyniku.

[ActionExecutedContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext) zapewnia `Controller` i `Result` oraz następujące właściwości:

* `Canceled` -będzie mieć wartość true, jeśli zwartym został wykonanie akcji przez inny filtr.
* `Exception` -mieć wartości null, jeśli akcji lub filtr akcji kolejnych zwrócił wyjątek. Ustawienie tej właściwości na wartość null, efektywnie "handles" Wystąpił wyjątek, i `Result` będą wykonywane tak, jakby jego zwykle zwróconych przez metodę akcji.

Aby uzyskać `IAsyncActionFilter`, wywołanie `ActionExecutionDelegate`:

* Wykonuje wszystkie filtry akcji kolejnych i metody akcji.
* Zwraca `ActionExecutedContext`. 

Zwarcie, Przypisz `ActionExecutingContext.Result` niektóre wartości w wyniku wystąpienia i nie wywołuj `ActionExecutionDelegate`.

Abstrakcyjnego zapewnia platformę `ActionFilterAttribute` mogących podklasy. 

Filtr akcji można użyć, aby sprawdzić stan modelu i zwraca błędy, jeśli stan jest nieprawidłowy:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

`OnActionExecuted` Metody działa po metody akcji i umożliwia Zobacz i manipulowania wyniki akcji za pomocą `ActionExecutedContext.Result` właściwości. `ActionExecutedContext.Canceled` zostanie ustawiona na true, jeśli zwartym został wykonanie akcji przez inny filtr. `ActionExecutedContext.Exception` zostanie ustawiona na wartość inną niż null Jeśli akcji lub filtr akcji kolejnych zwrócił wyjątek. Ustawienie `ActionExecutedContext.Exception` null:

* Efektywne "handles" Wystąpił wyjątek.
* `ActionExectedContext.Result` jest wykonywane tak, jakby były zwykle zwrócony przez metodę akcji.

## <a name="exception-filters"></a>Filtry wyjątków

*Filtry wyjątków* implementować albo `IExceptionFilter` lub `IAsyncExceptionFilter` interfejsu. Mogą one używane do implementowania obsługi zasady dla aplikacji typowych błędów. 

Następujący przykładowy filtr wyjątek używa widoku błędów niestandardowych developer Aby wyświetlić szczegóły dotyczące wyjątków, które występują, gdy aplikacja jest rozwijany:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

Filtry wyjątków:

* Nie masz przed i po zdarzenia. 
* Implementowanie `OnException` lub `OnExceptionAsync`. 
* Obsługa nieobsługiwanych wyjątków, które występują w tworzenia kontrolera [modelu powiązania](../models/model-binding.md), filtry akcji lub metody akcji. 
* Nie przechwytuj wyjątków, które występują w filtrów zasobów, filtry wyników lub wykonywania wynik MVC.

Do obsługi wyjątku, ustaw `ExceptionContext.ExceptionHandled` właściwości na wartość true lub zapisu odpowiedzi. Powoduje to zatrzymanie propagacji wyjątku. Filtru wyjątków nie może włączyć wyjątek do "Powodzenie". Filtr akcji można to zrobić.

> [!NOTE]
> W ASP.NET Core 1.1, odpowiedź nie jest wysyłane, jeśli ustawisz `ExceptionHandled` TRUE **i** zapisu odpowiedzi. W tym scenariuszu platformy ASP.NET Core 1.0 wysyłania odpowiedzi i platformy ASP.NET Core 1.1.2 powróci do zachowania 1.0. Aby uzyskać więcej informacji, zobacz [wystawiać #5594](https://github.com/aspnet/Mvc/issues/5594) w repozytorium GitHub. 

Filtry wyjątków:

* Są odpowiednie do generowania pułapek wyjątków, które występują w ramach działań MVC.
* Nie są tak elastyczne jako błąd obsługi oprogramowania pośredniczącego. 

Preferowane jest oprogramowanie pośredniczące do obsługi wyjątków. Za pomocą filtrów wyjątków, tylko gdy należy wykonywać obsługi błędów *inaczej* oparte na Akcja kontrolera MVC, który został wybrany. Na przykład aplikacja może mieć metody akcji dla obu punkty końcowe interfejsu API i widoki/HTML. Punkty końcowe interfejsu API może zwrócić informacji o błędach w formacie JSON, gdy czynności na podstawie widok może zwrócić strony błędu w formacie HTML.

`ExceptionFilterAttribute` Może być podklasą klasy. 

## <a name="result-filters"></a>Filtry wyników

* Implementowanie albo `IResultFilter` lub `IAsyncResultFilter` interfejsu.
* Wykonanie ich wokół wykonywania wyników akcji. 

Oto przykład filtr wynik, który dodaje nagłówek HTTP.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

Typ wyniku wykonywana zależy od danego działania. Akcja kontrolera MVC, zwracając widoku obejmie wszystkie razor przetwarzania jako część `ViewResult` wykonywana. Metody interfejsu API może wykonać niektóre serializacji w ramach wykonania wyniku. Dowiedz się więcej o [wyników akcji](actions.md)

Filtry wyników są wykonywane tylko dla pomyślne wyniki — po akcji lub filtrów akcji dają wyniku akcji. Filtry wyników nie są wykonywane, gdy filtry wyjątków obsługi wyjątku.

`OnResultExecuting` Metody może zwarcia wynik akcji i filtry wyników kolejnych przez ustawienie `ResultExecutingContext.Cancel` na wartość true. Ogólnie należy zapisać obiekt odpowiedzi podczas zwarcie, aby uniknąć generowania pustą odpowiedź. Wyrzucanie wyjątków spowoduje:

* Zapobiec wykonaniu wyniku akcji i kolejne filtrów.
* Traktowane jako błąd zamiast pomyślnego wyniku.

Gdy `OnResultExecuted` metody działa, odpowiedzi prawdopodobnie zostało wysłane do klienta i nie może być więcej (o ile nie wystąpił wyjątek). `ResultExecutedContext.Canceled` zostanie ustawiona na true, jeśli zwartym został wykonywania wynik akcji przez inny filtr.

`ResultExecutedContext.Exception` zostanie ustawiona na wartość inną niż null, jeśli wynik akcji lub filtru wyników kolejnych zwrócił wyjątek. Ustawienie `Exception` do wartości null skutecznie "handles" wyjątku i uniemożliwia wyjątku z zostanie zgłoszony przez MVC później w potoku. Jest obsługi wyjątków w filtrze wynik, nie można zapisać danych do odpowiedzi. Jeśli wynik akcji zgłasza wyjątek w tym za pomocą działania, a nagłówki zostały opróżnione do klienta, nie istnieje mechanizm niezawodnej wysłać kod błędu.

Aby uzyskać `IAsyncResultFilter` wywołanie `await next` na `ResultExecutionDelegate` wykonuje wszystkie filtry wyników kolejnych i wyniku akcji. Aby zwarcia, ustaw `ResultExecutingContext.Cancel` na wartość PRAWDA, a nie wywołuj `ResultExectionDelegate`.

Abstrakcyjnego zapewnia platformę `ResultFilterAttribute` mogących podklasy. [AddHeaderAttribute](#add-header-attribute) klasy przedstawiona wcześniej jest przykładem atrybutów filtru wyniku.

## <a name="using-middleware-in-the-filter-pipeline"></a>Za pomocą oprogramowania pośredniczącego w potoku filtru

Filtry zasobów działają podobnie jak [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) w tym ujęty wykonanie wszystkich elementów, które później w potoku. Jednak filtry różnią się od oprogramowania pośredniczącego, są one częścią MVC, co oznacza, że mają dostęp do kontekstu MVC i konstrukcji.

W ASP.NET Core 1.1 można użyć oprogramowania pośredniczącego w potoku filtru. Można to zrobić, jeśli składnik oprogramowania pośredniczącego, które wymagają dostępu do danych trasy MVC lub taki, który należy uruchamiać tylko dla niektórych kontrolerach ani akcji.

Aby korzystać z oprogramowania pośredniczącego jako filtru, Utwórz typ z `Configure` metodę, która określa oprogramowanie pośredniczące, który chcesz wstawić do potoku filtru. Oto przykład, która używa oprogramowania pośredniczącego lokalizacji do określania bieżącej kultury dla żądania:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

Następnie można użyć `MiddlewareFilterAttribute` do uruchomienia oprogramowania pośredniczącego dla wybranego kontrolera lub akcji lub globalnie:

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

Oprogramowanie pośredniczące filtry są uruchamiane na tym samym etapie potoku filtru jako filtrów zasobów, przed powiązaniem modelu i po pozostałego potoku.

## <a name="next-actions"></a>Kolejne czynności

Do eksperymentów z filtrów, [pobierania, testowania i zmodyfikować próbki](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).
