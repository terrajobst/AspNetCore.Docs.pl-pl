---
title: Filtry w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak działa filtr i sposobu ich używania w programie ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 5/08/2019
uid: mvc/controllers/filters
ms.openlocfilehash: cdf121b97396cb23103d49cd141b9ef19b8c0cc6
ms.sourcegitcommit: e1623d8279b27ff83d8ad67a1e7ef439259decdf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/25/2019
ms.locfileid: "66223023"
---
# <a name="filters-in-aspnet-core"></a>Filtry w programie ASP.NET Core

Przez [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), i [Steve Smith](https://ardalis.com/)

*Filtry* w programie ASP.NET Core umożliwia kodu do uruchomienia przed lub po określonych etapów w potoku przetwarzania żądań.

Wbudowanych filtrów obsługi zadań, takich jak:

* Autoryzacja (blokowanie dostępu do zasobów, których użytkownik nie jest autoryzowany do).
* Odpowiedź buforowania (zwarcie Potok żądań do zwracania odpowiedzi pamięci podręcznej).

Filtry niestandardowe mogą być tworzone do obsługi odciąż przekrojowe zagadnienia. Przykładami odciąż przekrojowe zagadnienia obsługi błędów, buforowanie, konfiguracji, autoryzacji i rejestrowania.  Filtry uniknąć duplikowania kodu. Na przykład błąd obsługi filtra wyjątku można skonsolidować obsługi błędów.

Ten dokument ma zastosowanie do stron Razor, kontrolery interfejsu API i kontrolery, za pomocą widoków.

[Wyświetlanie lub pobieranie przykładowych](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample)).

## <a name="how-filters-work"></a>Jak działa filtr

Filtry są uruchamiane w ramach *potok wywołania akcji programu ASP.NET Core*nazywanych czasami *potoku filtru*.  Potok filtru jest uruchamiany po platformy ASP.NET Core wybiera akcję do wykonania.

![Żądanie jest przetwarzane przez inne oprogramowanie pośredniczące, Routing oprogramowanie pośredniczące, wybór akcji i potoku wywołania akcji programu ASP.NET Core. Wstecz przez wybór akcji, Routing oprogramowanie pośredniczące i różne inne oprogramowanie pośredniczące kontynuuje przetwarzanie żądania, aby stać się odpowiedzi wysyłane do klienta.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>Filtruj typy

Każdy typ filtru jest wykonywane na różnych etapach potoku filtru:

* [Filtry autoryzacji](#authorization-filters) są uruchamiane jako pierwsze i są używane do ustalenia, czy użytkownik jest autoryzowany dla żądania. Filtry autoryzacji zwarcie potoku, jeśli żądanie jest nieautoryzowane.

* [Filtry zasobów](#resource-filters):

  * Uruchom po autoryzacji.  
  * <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> można uruchomić kod przed pozostałego potoku filtru. Na przykład `OnResourceExecuting` można uruchomić kod przed powiązaniem modelu.
  * <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> można uruchomić kod po ukończeniu pozostałego potoku.

* [Filtry akcji](#action-filters) może uruchamiać kod bezpośrednio przed i po jest wywoływana z metody akcji. One może służyć do manipulowania Argumenty przekazane do akcji i wyniku zwracanego z akcji. Filtry akcji są **nie** obsługiwane w przypadku stron Razor.

* [Filtry wyjątków](#exception-filters) jest używana do stosowania zasad globalnych do nieobsługiwanych wyjątków występujące przed niczego zostały zapisane w treści odpowiedzi.

* [Wynik filtry](#result-filters) może uruchamiać kod bezpośrednio przed i po wykonaniu wyniki poszczególnych akcji. Działają one tylko wtedy, gdy metoda akcji zostało wykonane pomyślnie. Są one przydatne dla logiki, które należy otoczyć wykonywanie widoku lub elementu formatującego.

Na poniższym diagramie przedstawiono sposób interakcji typów filtrów w potoku filtru.

![Żądanie jest przetwarzane przez filtry autoryzacji, filtrów zasobów, powiązanie modelu, filtry akcji, wykonywanie akcji i konwersji wynik akcji, filtry wyjątków, filtry wyników i wynik wykonywania. Na jej poziomie żądania jest przetwarzany tylko przez filtry wyników i filtrów zasobów przed staje się odpowiedzi wysyłane do klienta.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>Implementacja

Filtry obsługuje synchroniczne i asynchroniczne implementacji za pośrednictwem interfejsu w różnych definicjach.

Synchroniczne filtrów można uruchomić kod przed (`On-Stage-Executing`) oraz po (`On-Stage-Executed`) ich etap potoku. Na przykład `OnActionExecuting` jest wywoływana przed wywołaniem metody akcji. `OnActionExecuted` jest wywoływana po powrocie z metody akcji.

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

Zdefiniuj filtry asynchroniczne `On-Stage-ExecutionAsync` metody:

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

W poprzednim kodzie `SampleAsyncActionFilter` ma <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`), który jest wykonywany metody akcji.  Każdy z `On-Stage-ExecutionAsync` metody przyjmują `FilterType-ExecutionDelegate` , który jest wykonywany etap potoku filtru.

### <a name="multiple-filter-stages"></a>Wiele etapów filtru

Interfejsy na wiele etapów filtr można zaimplementować w jednej klasie. Na przykład <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> klasy implementuje `IActionFilter`, `IResultFilter`i ich odpowiedniki async.

Implementowanie **albo** synchronicznej lub asynchronicznej wersję interfejsu filtru, **nie** jednocześnie. Środowisko uruchomieniowe sprawdza najpierw Jeśli filtr implementuje interfejs asynchroniczne, a jeśli tak jest, wywołuje metodę. W przeciwnym razie wywołuje metody synchronicznej interfejsu. Jeśli synchronicznego i asynchronicznego interfejsy są implementowane w jedną klasę, jest wywoływana tylko metody asynchronicznej. Podczas używania klas abstrakcyjnych, takich jak <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> przesłonić tylko synchroniczne metody lub metody asynchronicznej dla każdego typu filtru.

### <a name="built-in-filter-attributes"></a>Atrybuty filtru wbudowane

ASP.NET Core obejmuje wbudowane filtry oparte na atrybutach, które należy do podklasy i dostosowane. Na przykład następujący filtr wyników dodaje do odpowiedzi nagłówek:

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

Atrybuty zezwalać na filtry przyjmowały argumenty, jak pokazano w powyższym przykładzie. Zastosuj `AddHeaderAttribute` do kontrolera lub metody akcji i określ nazwę i wartość nagłówka HTTP:

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

Kilka interfejsów filtr ma odpowiednie atrybuty, które może służyć jako klay bazowe dla niestandardowych implementacji.

Atrybuty filtru:

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a>Filtr zakresów i kolejność wykonywania

Filtr można dodać do potoku w jednej z trzech *zakresy*:

* Za pomocą atrybutu w akcji.
* Za pomocą atrybutu na kontrolerze.
* Globalnie dla wszystkich kontrolerów i akcji, jak pokazano w poniższym kodzie:

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

Poprzedzający kod dodaje trzy filtry zasięg globalny dzięki [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) kolekcji.

### <a name="default-order-of-execution"></a>Domyślna kolejność wykonywania

Gdy istnieje wiele filtrów dla danego etapu potoku, zakres Określa domyślną kolejność wykonywania filtrów.  Filtry globalne Otocz filtrów klasy, które z kolei Otocz metoda filtrów.

W wyniku zagnieżdżanie filtru *po* kod filtrów, który jest uruchamiany w odwrotnej kolejności *przed* kodu. Sekwencja filtru:

* *Przed* kodu filtrów globalnych.
  * *Przed* kodu filtrów kontrolera.
    * *Przed* kodu filtrów metody akcji.
    * *Po* kodu filtrów metody akcji.
  * *Po* kodu filtrów kontrolera.
* *Po* kodu filtrów globalnych.
  
Poniższy przykład ilustruje zamówienia w filtrze, które metody są wywoływane dla filtrów akcji synchroniczne.

| Sequence | Zakres filtru | Filter — metoda |
|:--------:|:------------:|:-------------:|
| 1 | Global | `OnActionExecuting` |
| 2 | Kontroler | `OnActionExecuting` |
| 3 | Metoda | `OnActionExecuting` |
| 4 | Metoda | `OnActionExecuted` |
| 5 | Kontroler | `OnActionExecuted` |
| 6 | Global | `OnActionExecuted` |

Ta sekwencja zawiera:

* Filtr metody jest zagnieżdżona w filtrze kontrolera.
* Filtr kontrolera jest zagnieżdżony w filtrów globalnych.

### <a name="controller-and-razor-page-level-filters"></a>Filtry na poziomie kontrolera i strona Razor

Każdy kontroler, który dziedziczy z <xref:Microsoft.AspNetCore.Mvc.Controller> zawiera klasę bazową [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), i [ Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*) 
 `OnActionExecuted` metody. Te metody:

* Otoczenie systemem filtry dla danej akcji.
* `OnActionExecuting` jest wywoływana przed każdą filtrów akcji.
* `OnActionExecuted` jest wywoływana po wszystkie filtry akcji.
* `OnActionExecutionAsync` jest wywoływana przed każdą filtrów akcji. Kod w filtrze po `next` jest uruchamiany po metody akcji.

Na przykład, w tym przykładzie pobierania `MySampleActionFilter` jest stosowane globalnie w uruchamiania.

`TestController`:

* Stosuje `SampleActionFilterAttribute` (`[SampleActionFilter]`) do `FilterTest2` akcji:
* Zastępuje `OnActionExecuting` i `OnActionExecuted`.

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

Przejdź do `https://localhost:5001/Test/FilterTest2` uruchomieniu następujący kod:

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

Dla stron Razor, zobacz [filtry stron Razor Implementowanie poprzez zastąpienie metody filtr](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).

### <a name="overriding-the-default-order"></a>Zastępowanie domyślna kolejność

Domyślną sekwencją wykonanie może być zastąpiona przez Implementowanie <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>. `IOrderedFilter` udostępnia <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> właściwość, która ma pierwszeństwo przed zakresu, aby określić kolejność wykonywania. Filtr o niższych `Order` wartość:

* Przebiegi *przed* kodu wcześniej filtr o wyższej wartości `Order`.
* Przebiegi *po* kodzie następnie filtr o wyższej `Order` wartość.

`Order` Właściwość można ustawić za pomocą parametru konstruktora:

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

Należy wziąć pod uwagę tych samych filtrów akcji 3 pokazano w powyższym przykładzie. Jeśli `Order` właściwość kontrolera i filtrów globalnych jest ustawiona na 1 i 2, odpowiednio, kolejność wykonywania została odwrócona.

| Sequence | Zakres filtru | `Order` Właściwość | Filter — metoda |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1 | Metoda | 0 | `OnActionExecuting` |
| 2 | Kontroler | 1  | `OnActionExecuting` |
| 3 | Global | 2  | `OnActionExecuting` |
| 4 | Global | 2  | `OnActionExecuted` |
| 5 | Kontroler | 1  | `OnActionExecuted` |
| 6 | Metoda | 0  | `OnActionExecuted` |

`Order` Właściwość zastępuje zakres podczas ustalania kolejności, w której filtry są uruchamiane. Filtry są najpierw posortowane według kolejności, a następnie do przerwania ties jest używany zakres. Spośród filtrów wbudowanych implementują `IOrderedFilter` i Ustaw domyślną `Order` wartość 0. Wbudowane filtry, zakres Określa kolejność, chyba że `Order` jest ustawiona na wartość inną niż zero.

## <a name="cancellation-and-short-circuiting"></a>Anulowanie i zwarcie

Może być short-circuited potoku filtru, ustawiając <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> właściwość <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parametr do metody filtru. Na przykład następujący filtr zasobu uniemożliwia wykonywanie pozostałego potoku:

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

W poniższym kodzie zarówno `ShortCircuitingResourceFilter` i `AddHeader` filtr docelowy `SomeResource` metody akcji. `ShortCircuitingResourceFilter`:

* Jest uruchamiany po pierwsze, ponieważ filtr zasobów i `AddHeader` jest filtrem akcji.
* Short-Circuits pozostałego potoku.

W związku z tym `AddHeader` filtr nigdy nie będzie uruchamiany dla `SomeResource` akcji. To zachowanie będzie taki sam w przypadku obu filtrów były stosowane na poziomie metody akcji, podany `ShortCircuitingResourceFilter` uruchomiono pierwszy. `ShortCircuitingResourceFilter` Uruchamia pierwszy ze względu na jej typ filtru lub przez jawne użycie `Order` właściwości.

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>Wstrzykiwanie zależności

Filtry można dodać według typu lub wystąpienia. Jeśli wystąpienie zostanie dodany, że wystąpienie jest używane dla każdego żądania. Jeśli typ jest dodawany, jest typ aktywowany. Oznacza typ aktywowany filtru:

* Wystąpienie jest tworzone dla każdego żądania.
* Wszelkie zależności konstruktora są wypełniane przez [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) (DI).

Filtry, które są zaimplementowane jako atrybuty i dodawane bezpośrednio do klasy kontrolera lub metody akcji nie może mieć konstruktora zależności, dostarczone przez [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) (DI). Nie można podać zależności Konstruktor przez DI, ponieważ:

* Atrybuty musi mieć ich parametry konstruktora dostarczane, gdy są one stosowane. 
* Jest to ograniczenie o współdziałaniu atrybutów.

Poniższe filtry obsługują zależności Konstruktor udostępnionych w serwisie DI:

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> zaimplementowane w atrybucie.

Poprzedni filtrów można zastosować do kontrolera lub metody akcji:

Rejestratory są dostępne z DI. Jednak należy unikać tworzenia i używania filtrów służyć wyłącznie do celów rejestrowania. [Wbudowana struktura rejestrowania](xref:fundamentals/logging/index) zwykle zapewnia, co jest potrzebne do rejestrowania. Rejestrowanie dodane do filtrów:

* Należy skoncentrować się na potencjalne problemy biznesowe domeny lub zachowania specyficzne dla filtru.
* Należy **nie** dziennika akcji lub inne zdarzenia framework. Wbudowane filtry dziennika akcji i zdarzenia framework.

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

Typy wdrożenia filtrów usługi są zarejestrowane w usłudze `ConfigureServices`. A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> pobiera wystąpienia filtru z DI.

Poniższy kod przedstawia `AddHeaderResultServiceFilter`:

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

W poniższym kodzie `AddHeaderResultServiceFilter` zostanie dodany do kontenera DI:

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

W poniższym kodzie `ServiceFilter` atrybut pobiera wystąpienia `AddHeaderResultServiceFilter` filtr z DI:

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

[ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):

* Stanowi wskazówkę wystąpienie filtru *może* być ponownie używane poza zakres żądania został utworzony w ciągu. Środowisko uruchomieniowe programu ASP.NET Core nie gwarantuje:

  * Czy zostaną utworzone pojedyncze wystąpienie filtru.
  * Filtr nie będzie ponowne żądanie z kontenera DI w pewnym momencie później.

* Nie można używać z filtrem, który zależy od usługi z okresem istnienia innego niż pojedyncze.

 <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>. `IFilterFactory` udostępnia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> metodę tworzenia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> wystąpienia. `CreateInstance` Ładuje określony typ z DI.

### <a name="typefilterattribute"></a>TypeFilterAttribute

<xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> jest podobny do <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, ale jego typ nie zostanie rozwiązany bezpośrednio w kontenerze DI. Tworzy wystąpienie typu przy użyciu <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.

Ponieważ `TypeFilterAttribute` typy nie są rozwiązane bezpośrednio w kontenerze DI:

* Typy, które są wywoływane przy użyciu `TypeFilterAttribute` nie muszą być zarejestrowane przy użyciu kontenera DI.  Mają zależności są spełnione przez kontener DI.
* `TypeFilterAttribute` Opcjonalnie można zaakceptować argumentów konstruktora dla typu.

Korzystając z `TypeFilterAttribute`, ustawiając `IsReusable` jest to wskazówka, wystąpienie filtru *może* być ponownie używane poza zakres żądania został utworzony w ciągu. Środowisko uruchomieniowe programu ASP.NET Core zapewnia żadnej gwarancji, które zostaną utworzone pojedyncze wystąpienie filtru. `IsReusable` Nie można używać z filtrem, który zależy od usługi z okresem istnienia innego niż pojedyncze.

Poniższy przykład pokazuje, jak przekazywać argumenty do typu przy użyciu `TypeFilterAttribute`:

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a>Filtry autoryzacji

Filtry autoryzacji:

* Czy pierwszy filtry są uruchamiane w potoku filtru.
* Kontrola dostępu do metody akcji.
* Masz przed metodą, ale nie po metodzie.

Filtry autoryzacji niestandardowe wymagają framework autoryzacja niestandardowa. Preferuj Konfigurowanie zasad autoryzacji lub zapisu niestandardowych zasad autoryzacji za pośrednictwem pisania niestandardowego filtru. Filtr autoryzacji wbudowane:

* Wywołuje system autoryzacji.
* Nie zezwalają na żądania.

Czy **nie** zgłaszają wyjątki w ramach filtry autoryzacji:

* Wyjątek nie będzie obsługiwana.
* Filtry wyjątków nie będzie obsługiwać wyjątek.

Należy wziąć pod uwagę, wydawanie wyzwanie w przypadku, gdy wystąpi wyjątek w filtru autoryzacji.

Dowiedz się więcej o [autoryzacji](xref:security/authorization/introduction).

## <a name="resource-filters"></a>Filtry zasobów

Filtry zasobów:

* Implementowanie albo <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interfejsu.
* Wykonanie opakowuje większość potoku filtru.
* Tylko [filtry autoryzacji](#authorization-filters) uruchamiane przed filtrami zasobów.

Filtry zasobów są przydatne do zwarcie większość potoku. Na przykład filtr pamięci podręcznej można uniknąć pozostałego potoku na trafień pamięci podręcznej.

Przykłady filtrów zasobów:

* [Filtr zasobu short-circuiting](#short-circuiting-resource-filter) przedstawionego wcześniej.
* [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):

  * Zapobiega wiązania modelu dostępu do danych formularza.
  * Używane przekazywania plików o dużym rozmiarze, aby zapobiec wczytywana pamięci danych formularza.

## <a name="action-filters"></a>Filtry akcji

> [!IMPORTANT]
> Filtry akcji wykonaj **nie** dotyczą stron Razor. Strony razor obsługuje <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> . Aby uzyskać więcej informacji, zobacz [metody filtrowania dla stron Razor](xref:razor-pages/filter).

Filtry akcji:

* Implementowanie albo <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interfejsu.
* Ich wykonanie otacza wykonywania metody akcji.

Poniższy kod przedstawia przykładowy filtr akcji:

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> Zawiera następujące właściwości:

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> — Umożliwia odczytanie danych wejściowych do metody akcji.
* <xref:Microsoft.AspNetCore.Mvc.Controller> — umożliwia manipulowanie wystąpienie kontrolera.
* <xref:System.Web.Mvc.ActionExecutingContext.Result> -ustawienie `Result` short-circuits wykonywania metody akcji i filtry kolejnych działań.

Zostanie zgłoszony wyjątek w metodzie akcji:

* Uniemożliwia uruchamianie kolejne filtry.
* W przeciwieństwie do ustawienia `Result`, jest traktowana jako błąd zamiast pomyślnego wyniku.

<xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> Zapewnia `Controller` i `Result` oraz następujące właściwości:

* <xref:System.Web.Mvc.ActionExecutedContext.Canceled> — Wartość true, jeśli zwartym został wykonanie akcji przez inny filtr.
* <xref:System.Web.Mvc.ActionExecutedContext.Exception> Innych niż null w przypadku akcji lub filtru akcji wcześniej przebiegu zgłosiła wyjątek. Ustawienie tej właściwości na wartość null:

  * Efektywnie obsługuje wyjątek.
  * `Result` jest wykonywane tak, jakby został zwrócony przez metodę akcji.

Aby uzyskać `IAsyncActionFilter`, wywołanie <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:

* Wykonuje wszelkie filtry kolejnej akcji i metody akcji.
* Zwraca `ActionExecutedContext`.

Aby zwarcie, należy przypisać <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> w wyniku wystąpienia i nie wywołuj `next` ( `ActionExecutionDelegate`).

Struktura dostarcza abstrakcyjną <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> , może być podklasą klasy.

`OnActionExecuting` Filtr akcji można używać do:

* Sprawdź stan modelu.
* Zwraca błąd, jeśli stan jest nieprawidłowy.

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

`OnActionExecuted` Metoda uruchamia się po metody akcji:

* Można wyświetlić i manipulowania wynikami akcji za pomocą <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> właściwości.
* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> ustawiono wartość true, jeśli jest to zwartym został wykonanie akcji przez inny filtr.
* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> jest ustawiona na wartość inną niż null, jeśli akcji lub filtru akcji kolejnych zgłosiła wyjątek. Ustawienie `Exception` null:

  * Efektywnie obsługuje wyjątek.
  * `ActionExecutedContext.Result` jest wykonywane tak, jakby były zwracane normalnie przez metodę akcji.

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a>Filtry wyjątków

Filtry wyjątków:

* Implementowanie <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>. 
* Może służyć do implementowania obsługi zasad typowych błędów.

Następujący filtr wyjątku przykładowych użyto widoku błędów niestandardowych, aby wyświetlić szczegóły dotyczące wyjątków, które występują, gdy aplikacja jest w trakcie opracowywania:

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=16-19)]

Filtry wyjątków:

* Nie masz, przed i po nim zdarzeń.
* Implementowanie <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.
* Obsługa nieobsługiwanych wyjątków, które występują w stronę Razor lub tworzenia kontrolera [wiązanie modelu](xref:mvc/models/model-binding), filtry akcji lub metody akcji.
* Czy **nie** przechwytywać wyjątków, które występują w filtrów zasobów, filtry wyników lub MVC wynik wykonywania.

Aby obsłużyć wyjątek, należy ustawić <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> właściwości `true` lub zapisu odpowiedzi. Spowoduje to zatrzymanie propagacji wyjątku. Filtra wyjątku nie może włączyć wyjątek do "Powodzenie". Filtr akcji można to zrobić.

Filtry wyjątków:

* Dla zastosowań dobre są wyjątki wyłapywanie, które występują w ramach akcji.
* Nie są tak elastyczne, oprogramowanie pośredniczące obsługi błędów.

Preferuj oprogramowanie pośredniczące do obsługi wyjątków. Użyj wyjątek filtruje tylko wtedy, gdy obsługa błędów *różni się* oparte na jakiej metody akcji jest wywoływana. Na przykład aplikacja może mieć metody akcji dla obu punktów końcowych interfejsu API i widoki/HTML. Punkty końcowe interfejsu API może zwrócić informacje o błędach w formacie JSON, natomiast akcje na podstawie widoku może zwrócić strony błędu w formacie HTML.

## <a name="result-filters"></a>Filtry wyników

Filtry wyników:

* Implementuj interfejs z:
  * <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>
  * <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter>
* Ich wykonanie otacza wykonywania wyników akcji.

### <a name="iresultfilter-and-iasyncresultfilter"></a>IResultFilter i IAsyncResultFilter

Poniższy kod pokazuje filtr wynik, który dodaje nagłówek HTTP:

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

Typ wyniku wykonywania zależy od akcji. Zwracanie Widok akcji obejmuje wszystkie razor przetwarzania w ramach <xref:Microsoft.AspNetCore.Mvc.ViewResult> wykonywana. Metoda interfejsu API może wykonywać niektóre serializacji jako część wykonania wyniku. Dowiedz się więcej o [wyników akcji](xref:mvc/controllers/actions)

Filtry wyników są wykonywane tylko na pomyślne wyniki — gdy akcji lub filtry akcji dają wynik akcji. Filtry wyników nie są wykonywane, gdy filtry wyjątków obsługi wyjątku.

<xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> Metoda może zwarcie wykonywania wynik akcji i filtry kolejnych wyników, ustawiając <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> do `true`. Zapisywana w obiekt odpowiedzi zwarcie, aby uniknąć generowania pustą odpowiedź. Zostanie zgłoszony wyjątek `IResultFilter.OnResultExecuting` będzie:

* Zapobiec wykonaniu wyniku akcji i kolejne filtry.
* Traktowane jako błąd zamiast pomyślnego wyniku.

Gdy <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> uruchamia metody:

* Odpowiedź, prawdopodobnie została wysłana do klienta i nie można jej zmienić.
* Jeśli wystąpił wyjątek, treść odpowiedzi nie są wysyłane.

<!-- Review preceding "If an exception was thrown: Original 
When the OnResultExecuted method runs, the response has likely been sent to the client and cannot be changed further (unless an exception was thrown).

SHould that be , 
If an exception was thrown **IN THE RESULT FILTER**, the response body is not sent.

 -->

`ResultExecutedContext.Canceled` ustawiono `true` Jeśli zwartym został wykonanie wynik akcji przez inny filtr.

`ResultExecutedContext.Exception` jest ustawiona na wartość inną niż null, jeśli wynik akcji lub filtru kolejnych wyników zgłosiła wyjątek. Ustawienie `Exception` do wartości null skutecznie obsługuje wyjątek i uniemożliwia wyjątek z jest zgłaszany ponownie przez platformy ASP.NET Core w dalszej części procesu. Brak niezawodnego sposobu zapisywania danych do odpowiedzi podczas obsługi wyjątku w filtrze wynik. Jeśli ma został opróżniony nagłówki do klienta, gdy wynik akcji zgłasza wyjątek, nie istnieje mechanizm niezawodne wysłać kod błędu.

Aby uzyskać <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, wywołanie `await next` na <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> wykonuje wszystkie filtry kolejnych wyników i wyniku akcji. Aby zwarcie, ustaw [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) do `true` i nie wywołuj `ResultExecutionDelegate`:

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

Struktura dostarcza abstrakcyjną `ResultFilterAttribute` , może być podklasą klasy. [AddHeaderAttribute](#add-header-attribute) klasy wyświetlane wcześniej jest przykładem atrybutów filtru wyniku.

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a>IAlwaysRunResultFilter i IAsyncAlwaysRunResultFilter

<xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> zadeklarować interfejsów <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementację, która jest uruchamiana dla wszystkich wyników akcji. Filtr jest stosowany do wszystkich wyników akcji, chyba że:

* <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> Lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> ma zastosowanie i short-circuits odpowiedzi.
* Filtra wyjątku obsługuje wyjątek, przedstawiając wynik akcji.

Filtry w innych niż `IExceptionFilter` i `IAuthorizationFilter` nie powodują pominięcie `IAlwaysRunResultFilter` i `IAsyncAlwaysRunResultFilter`.

Na przykład następujący filtr zawsze uruchamia i ustawia wynik akcji (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) za pomocą *422 brakuje jednostki* kod stanu niepowodzenia negocjacji zawartości:

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a>IFilterFactory

<xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>. W związku z tym `IFilterFactory` wystąpienia mogą być używane jako `IFilterMetadata` wystąpienia w dowolnym miejscu w potoku filtru. Gdy środowisko uruchomieniowe przygotowuje się do wywołania z filtrem, próbuje rzutować go na `IFilterFactory`. Jeśli tego rzutowania zakończy się powodzeniem, <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> metoda jest wywoływana, aby utworzyć `IFilterMetadata` wystąpienia, które jest wywoływane. Dzięki temu elastycznością, ponieważ potoku filtru dokładne nie muszą być ustawiony w sposób jawny po uruchomieniu aplikacji.

`IFilterFactory` można zaimplementować przy użyciu atrybutów niestandardowych implementacji jako innego podejścia do tworzenia filtrów:

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

Powyższy kod mogą być testowane przez uruchomienie [Pobierz przykładowe](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):

* Wywoływanie narzędzia programistyczne F12.
* Przejdź do `https://localhost:5001/Sample/HeaderWithFactory`

Narzędzia programistyczne F12 wyświetlić następujące nagłówki odpowiedzi dodany przez kod przykładowy:

* **Autor:** `Joe Smith`
* **globaladdheader:** `Result filter added to MvcOptions.Filters`
* **Wewnętrzny:** `My header`

Powyższy kod tworzy **wewnętrzny:** `My header` nagłówka odpowiedzi.

### <a name="ifilterfactory-implemented-on-an-attribute"></a>IFilterFactory implementowane w atrybucie

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

Filtry, które implementują `IFilterFactory` są przydatne w przypadku filtrów, które:

* Nie wymagają przekazywanie parametrów.
* Mają zależności konstruktora, które muszą zostać wypełnione przez DI.

<xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>. `IFilterFactory` udostępnia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> metodę tworzenia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> wystąpienia. `CreateInstance` Ładuje określony typ z kontenera usług (DI).

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

Poniższy kod pokazuje trzy sposoby stosowania `[SampleActionFilter]`:

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

W poprzednim kodzie urządzanie metody `[SampleActionFilter]` jest preferowanym sposobem stosowania `SampleActionFilter`.

## <a name="using-middleware-in-the-filter-pipeline"></a>Za pomocą oprogramowania pośredniczącego w potoku filtru

Filtry zasobów działają podobnie jak [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) w tym, że ujęty wykonanie wszystkich elementów, których można użyć później w potoku. Jednak filtrów różnią się od oprogramowania pośredniczącego, w tym, że są one częścią środowiska uruchomieniowego platformy ASP.NET Core, co oznacza, że mają dostęp do kontekstu platformy ASP.NET Core i konstrukcji.

Aby korzystać z oprogramowania pośredniczącego jako filtru, Utwórz typ z `Configure` metody, która określa oprogramowania pośredniczącego, aby wstawić do potoku filtru. W poniższym przykładzie użyto oprogramowania pośredniczącego lokalizacji ustalenie bieżącej kultury na żądanie:

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

Użyj <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> do uruchamiania oprogramowania pośredniczącego:

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

Oprogramowanie pośredniczące filtry są uruchamiane na tym samym etapie potoku filtru jako zasób filtry, przed powiązaniem modelu i po nim pozostałego potoku.

## <a name="next-actions"></a>Następne akcje

* Zobacz [metody filtrowania dla stron Razor](xref:razor-pages/filter)
* Aby poeksperymentować z filtrami, [pobierania, testowanie i zmodyfikować przykładowe GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).
