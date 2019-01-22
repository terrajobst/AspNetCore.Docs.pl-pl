---
title: Filtry w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak działa filtr i sposobu ich używania w programie ASP.NET Core MVC.
ms.author: riande
ms.custom: mvc
ms.date: 1/15/2019
uid: mvc/controllers/filters
ms.openlocfilehash: fe3082481b51c968fd361dbcc9553c4e35a36f2a
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444353"
---
# <a name="filters-in-aspnet-core"></a>Filtry w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), i [Steve Smith](https://ardalis.com/)

*Filtry* we wzorcu ASP.NET Core MVC pozwala na uruchamianie kodu przed lub po określonych etapów w potoku przetwarzania żądań.

 Wbudowanych filtrów obsługi zadań, takich jak:

 * Autoryzacja (blokowanie dostępu do zasobów, których użytkownik nie jest autoryzowany do).
 * Zapewnienie, że wszystkie żądania przy użyciu protokołu HTTPS.
 * Odpowiedź buforowania (zwarcie Potok żądań do zwracania odpowiedzi pamięci podręcznej). 

Filtry niestandardowe mogą być tworzone do obsługi odciąż przekrojowe zagadnienia. Filtry można uniknąć duplikowania kodu w akcji. Na przykład błąd obsługi filtra wyjątku można skonsolidować obsługi błędów.

[Wyświetlanie lub pobieranie przykładowy z serwisu GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).

## <a name="how-filters-work"></a>Jak działa filtr

Filtry są uruchamiane w ramach *potok wywołania Akcja MVC*nazywanych czasami *potoku filtru*.  Potok filtru jest uruchamiany po MVC wybiera akcję do wykonania.

![Żądanie jest przetwarzane przez inne oprogramowanie pośredniczące, Routing oprogramowanie pośredniczące, wybór akcji i potok wywołania Akcja MVC. Wstecz przez wybór akcji, Routing oprogramowanie pośredniczące i różne inne oprogramowanie pośredniczące kontynuuje przetwarzanie żądania, aby stać się odpowiedzi wysyłane do klienta.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>Filtruj typy

Każdy typ filtru jest wykonywane na różnych etapach potoku filtru.

* [Filtry autoryzacji](#authorization-filters) są uruchamiane jako pierwsze i są używane do ustalenia, czy bieżący użytkownik jest autoryzowany dla bieżącego żądania. Jeśli żądanie jest autoryzowane, ich zwarcie potoku. 

* [Filtry zasobów](#resource-filters) obsługi żądania po autoryzacji jako pierwsze.  Mogą uruchamiać kod przed rest z potoku filtru, a po ukończeniu pozostałego potoku. Są one przydatne implementuje się buforowanie lub w przeciwnym razie zwarcie potoku filtru ze względu na wydajność. Działają one przed powiązanie modelu, dzięki czemu mogą mieć wpływ na powiązania modelu.

* [Filtry akcji](#action-filters) może uruchamiać kod bezpośrednio przed i po jest wywoływana z metody akcji. One może służyć do manipulowania Argumenty przekazane do akcji i wyniku zwracanego z akcji. Filtry akcji nie są obsługiwane w przypadku stron Razor.

* [Filtry wyjątków](#exception-filters) jest używana do stosowania zasad globalnych do nieobsługiwanych wyjątków występujące przed niczego zostały zapisane w treści odpowiedzi.

* [Wynik filtry](#result-filters) może uruchamiać kod bezpośrednio przed i po wykonaniu wyniki poszczególnych akcji. Działają one tylko wtedy, gdy metoda akcji zostało wykonane pomyślnie. Są one przydatne dla logiki, które należy otoczyć wykonywanie widoku lub elementu formatującego.

Na poniższym diagramie przedstawiono, jak te typy filtrów wchodzić w interakcje w potoku filtru.

![Żądanie jest przetwarzane przez filtry autoryzacji, filtrów zasobów, powiązanie modelu, filtry akcji, wykonywanie akcji i konwersji wynik akcji, filtry wyjątków, filtry wyników i wynik wykonywania. Na jej poziomie żądania jest przetwarzany tylko przez filtry wyników i filtrów zasobów przed staje się odpowiedzi wysyłane do klienta.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>Implementacja

Filtry obsługuje synchroniczne i asynchroniczne implementacji za pośrednictwem interfejsu w różnych definicjach. 

Synchroniczne filtry, które można uruchomić kod, zarówno przed i po ich etap potoku, zdefiniuj na*etapu*Executing i*etapu*wykonywane metody. Na przykład `OnActionExecuting` jest wywoływana przed wywołaniem metody akcji i `OnActionExecuted` jest wywoływana po powrocie z metody akcji.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet1)]

Asynchroniczne Filtry definiują jeden na*etapu*ExecutionAsync metody. Ta metoda przyjmuje *FilterType*ExecutionDelegate delegata, która wykonuje etap potoku filtru. Na przykład `ActionExecutionDelegate` wywołania metody akcji lub następnego filtru akcji, na które może wykonać kod przed i po jej wywołaniu.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

Na wiele etapów filtru w jednej klasie mogą implementować interfejsy. Na przykład <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> klasy implementuje `IActionFilter`, `IResultFilter`i ich odpowiedniki async.

> [!NOTE]
> Implementowanie **albo** synchronicznej lub asynchronicznej wersję interfejsu filtru, nie obydwa. Struktura najpierw sprawdza, czy filtr implementuje interfejs asynchroniczne, a jeśli tak jest, wywołuje metodę. W przeciwnym razie wywołuje metody synchronicznej interfejsu. Gdyby zaimplementować obu interfejsów na jedną klasę, czy można wywołać tylko metody asynchronicznej. Podczas używania klas abstrakcyjnych, takich jak <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> należy przesłonić metodę tylko synchroniczne metody lub metody asynchronicznej dla każdego typu filtru.

### <a name="ifilterfactory"></a>IFilterFactory
[IFilterFactory](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifilterfactory) implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>. W związku z tym `IFilterFactory` wystąpienia mogą być używane jako `IFilterMetadata` wystąpienia w dowolnym miejscu w potoku filtru. Gdy w ramach przygotowuje się do wywołania filtr, próbuje rzutować go na `IFilterFactory`. Jeśli tego rzutowania zakończy się powodzeniem, [CreateInstance —](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifilterfactory.createinstance) metoda jest wywoływana, aby utworzyć `IFilterMetadata` wystąpienia, która będzie wywołana. Dzięki temu elastycznością, ponieważ potoku filtru dokładne nie muszą być ustawiony w sposób jawny po uruchomieniu aplikacji.

Możesz zaimplementować `IFilterFactory` na własnych implementacji atrybutu jako innego podejścia do tworzenia filtrów:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a>Atrybuty filtru wbudowane

Środowisko zawiera wbudowane filtry oparte na atrybutach można podklasy i dostosowywania. Na przykład następujący filtr wyników dodaje do odpowiedzi nagłówek.

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

Atrybuty zezwalać na filtry przyjmowały argumenty, jak pokazano w powyższym przykładzie. Czy dodać ten atrybut do kontrolera lub metody akcji i określ nazwę i wartość nagłówka HTTP:

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

Wynik `Index` akcja jest pokazana poniżej — nagłówki odpowiedzi są wyświetlane w prawym dolnym rogu.

![Deweloper narzędzia Microsoft Edge wyświetlanie nagłówki odpowiedzi, w tym Steve Smith autora @ardalis](filters/_static/add-header.png)

Kilka interfejsów filtr ma odpowiednie atrybuty, które może służyć jako klay bazowe dla niestandardowych implementacji.

Atrybuty filtru:

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

`TypeFilterAttribute` i `ServiceFilterAttribute` zostały wyjaśnione [w dalszej części tego artykułu](#dependency-injection).

## <a name="filter-scopes-and-order-of-execution"></a>Filtr zakresów i kolejność wykonywania

Filtr można dodać do potoku w jednej z trzech *zakresy*. Można dodać filtr do konkretnej metody akcji lub Klasa kontrolera, używając atrybutu. Możesz także zarejestrować filtr globalny dla wszystkich kontrolerów i akcji. Filtry są dodawane globalnie przez dodanie jej do `MvcOptions.Filters` kolekcji w `ConfigureServices`:

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a>Domyślna kolejność wykonywania

Gdy istnieje wiele filtrów dla danego etapu potoku, zakres Określa domyślną kolejność wykonywania filtrów.  Filtry globalne Otocz filtrów klasy, które z kolei Otocz metoda filtrów. To jest czasami określane jako zagnieżdżanie "Rosyjski lalki", ponieważ każde zwiększenie zakresu jest otaczający zakres poprzedniej, takich jak [zagnieżdżenia lalki](https://wikipedia.org/wiki/Matryoshka_doll). Uzyskasz ogólnie żądane zachowanie nadrzędnych, bez konieczności jawnego określenia kolejności.

W wyniku tego zagnieżdżanie *po* kod filtrów, który jest uruchamiany w odwrotnej kolejności *przed* kodu. Sekwencja wygląda następująco:

* *Przed* kodu filtry zastosowane globalnie
  * *Przed* kodu filtrów stosowanych do kontrolerów
    * *Przed* kodu filtrów stosowanych do metody akcji
    * *Po* kodu filtrów stosowanych do metody akcji
  * *Po* kodu filtrów stosowanych do kontrolerów
* *Po* kodu filtry zastosowane globalnie
  
Oto przykład ilustrujący zamówienia w filtrze, które metody są wywoływane dla synchroniczne filtrów akcji.

| Sekwencja | Zakres filtru | Filter — metoda |
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

Umieść go inny sposób, w przypadku wewnątrz asynchronicznej filtr użytkownika w*etapu*metoda ExecutionAsync wszystkie filtry z większego zakresu są uruchamiane podczas, gdy kod jest na stosie.

> [!NOTE]
> Każdy kontroler, który dziedziczy z `Controller` zawiera klasę bazową `OnActionExecuting` i `OnActionExecuted` metody. Te metody opakować systemem filtry dla danej akcji: `OnActionExecuting` jest wywoływane przed każdym filtrem, i `OnActionExecuted` jest wywoływana po wszystkie filtry.

### <a name="overriding-the-default-order"></a>Zastępowanie domyślna kolejność

Można zastąpić domyślną sekwencją wykonywania przez zaimplementowanie `IOrderedFilter`. Ten interfejs udostępnia `Order` właściwość, która ma pierwszeństwo przed zakresu, aby określić kolejność wykonywania. Filtr o niższych `Order` będzie miał wartość jego *przed* kod wykonywany wcześniej filtr o wyższej wartości `Order`. Filtr o niższych `Order` będzie miał wartość jego *po* kod wykonywany po tym filtr z większym `Order` wartość. Możesz ustawić `Order` właściwości przy użyciu parametru konstruktora:

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

Jeśli mają taką samą 3 filtry akcji pokazano w poprzednim przykładzie, ale zestaw `Order` właściwości kontrolera i globalne filtry 1 i 2, odpowiednio, będzie można odwrócić kolejność wykonywania.

| Sekwencja | Zakres filtru | `Order` Właściwość | Filter — metoda |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1 | Metoda | 0 | `OnActionExecuting` |
| 2 | Kontroler | 1  | `OnActionExecuting` |
| 3 | Global | 2  | `OnActionExecuting` |
| 4 | Global | 2  | `OnActionExecuted` |
| 5 | Kontroler | 1  | `OnActionExecuted` |
| 6 | Metoda | 0  | `OnActionExecuted` |

`Order` Atu właściwości zakresu podczas określania kolejność uruchamiania filtrów. Filtry są najpierw posortowane według kolejności, a następnie do przerwania ties jest używany zakres. Spośród filtrów wbudowanych implementują `IOrderedFilter` i Ustaw domyślną `Order` wartość 0. Wbudowane filtry, zakres Określa kolejność, chyba że `Order` na wartość inną niż zero.

## <a name="cancellation-and-short-circuiting"></a>Anulowanie i krótki circuiting

Powodują pominięcie potoku filtru w dowolnym momencie przez ustawienie `Result` właściwość `context` parametr do metody filtru. Na przykład następujący filtr zasobu uniemożliwia wykonywanie pozostałego potoku.

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

W poniższym kodzie zarówno `ShortCircuitingResourceFilter` i `AddHeader` filtr docelowy `SomeResource` metody akcji. `ShortCircuitingResourceFilter`:

* Jest uruchamiany po pierwsze, ponieważ filtr zasobów i `AddHeader` jest filtrem akcji.
* Short-Circuits pozostałego potoku.

W związku z tym `AddHeader` filtr nigdy nie będzie uruchamiany dla `SomeResource` akcji. To zachowanie będzie taki sam w przypadku obu filtrów były stosowane na poziomie metody akcji, podany `ShortCircuitingResourceFilter` uruchomiono pierwszy. `ShortCircuitingResourceFilter` Uruchamia pierwszy ze względu na jej typ filtru lub przez jawne użycie `Order` właściwości.

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>Wstrzykiwanie zależności

Filtry można dodać według typu lub wystąpienia. Jeśli dodasz wystąpienia, że wystąpienie zostanie użyte dla każdego żądania. Jeśli dodasz typu, będzie ona typu została aktywowana, co oznacza, będzie można utworzyć wystąpienia dla każdego żądania oraz wszelkie zależności Konstruktor zostanie wypełniony przez [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md) (DI). Dodawanie filtru według typu jest odpowiednikiem `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`.

Filtry, które są zaimplementowane jako atrybuty i dodawane bezpośrednio do klasy kontrolera lub metody akcji nie może mieć konstruktora zależności, dostarczone przez [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md) (DI). Jest to spowodowane atrybuty musi mieć ich parametry konstruktora dostarczane, gdy są one stosowane. Jest to ograniczenie o współdziałaniu atrybutów.

Jeśli filtry mają zależności, które wymagają dostępu z DI, jest kilka metod obsługiwanych. Filtr można zastosować do klasy lub metody akcji przy użyciu jednej z następujących czynności:

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* `IFilterFactory` zaimplementowany z atrybutu

> [!NOTE]
> Jeden zależności, które można pobrać z DI jest rejestrator. Należy jednak unikać tworzenia i używania filtrów wyłącznie do celów rejestrowania, ponieważ [funkcji rejestrowania wbudowana struktura](xref:fundamentals/logging/index) już może dostarczyć, co jest potrzebne. Jeśli zamierzasz dodać rejestrowania do filtry, należy skoncentrować się na potencjalne problemy biznesowe domeny lub zachowania specyficzne dla filtru, a nie akcji MVC lub inne zdarzenia framework.

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

Typy wdrożenia filtrów usługi są zarejestrowane w usłudze DI. A `ServiceFilterAttribute` pobiera wystąpienia filtru z DI. Dodaj `ServiceFilterAttribute` do kontenera w `Startup.ConfigureServices`i odwoływać się do niego w `[ServiceFilter]` atrybutu:

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

Korzystając z `ServiceFilterAttribute`, ustawiając `IsReusable` jest to wskazówka, wystąpienie filtru *może* być ponownie używane poza zakres żądania został utworzony w ciągu. Struktura zapewnia żadnej gwarancji, że pojedyncze wystąpienie filtru, który zostanie utworzony lub filtr nie nastąpi ponowne żądanie z kontenera DI w pewnym momencie później. Unikaj używania `IsReusable` przy użyciu filtru, który jest zależna od usługi z okresem istnienia innego niż pojedyncze.

Za pomocą `ServiceFilterAttribute` bez rejestrowania wyników filtrowania typ wyjątku:

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

`ServiceFilterAttribute` implementuje `IFilterFactory`. `IFilterFactory` udostępnia `CreateInstance` metodę tworzenia `IFilterMetadata` wystąpienia. `CreateInstance` Metoda ładuje określonego typu z kontenera usługi (DI).

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute` jest podobny do `ServiceFilterAttribute`, ale jego typ nie zostanie rozwiązany bezpośrednio w kontenerze DI. Tworzy wystąpienie typu przy użyciu `Microsoft.Extensions.DependencyInjection.ObjectFactory`.

Z powodu tej różnicy:

* Typy, które są wywoływane przy użyciu `TypeFilterAttribute` trzeba najpierw zarejestrowane z kontenerem.  Mają zależności są spełnione przez kontener. 
* `TypeFilterAttribute` Opcjonalnie można zaakceptować argumentów konstruktora dla typu.

Korzystając z `TypeFilterAttribute`, ustawiając `IsReusable` jest to wskazówka, wystąpienie filtru *może* być ponownie używane poza zakres żądania został utworzony w ciągu. Struktura zapewnia żadnej gwarancji, które zostaną utworzone pojedyncze wystąpienie filtru. Unikaj używania `IsReusable` przy użyciu filtru, który jest zależna od usługi z okresem istnienia innego niż pojedyncze.

W poniższym przykładzie pokazano sposób przekazywania argumentów do typu przy użyciu `TypeFilterAttribute`:

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

### <a name="ifilterfactory-implemented-on-your-attribute"></a>IFilterFactory na atrybut

Jeśli masz filtr który:

* Nie wymagają żadnych argumentów.
* Ma zależności konstruktora, które muszą zostać wypełnione przez DI.

Możesz użyć własnego atrybutu nazwanego na klasy i metody zamiast `[TypeFilter(typeof(FilterType))]`). Następujący filtr pokazuje, jak można to zaimplementować:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

Ten filtr można zastosować do klasy lub metody za pomocą `[SampleActionFilter]` składni, zamiast `[TypeFilter]` lub `[ServiceFilter]`.

## <a name="authorization-filters"></a>Filtry autoryzacji

*Filtry autoryzacji*:

* Kontrola dostępu do metody akcji.
* To pierwszy filtry, które mają być wykonane w ramach potoku filtru. 
* Masz przed metodą, ale nie po metodzie. 

Należy tylko wpisać filtr autoryzacji niestandardowej Jeśli piszesz własnego ramy autoryzacji. Preferuj Konfigurowanie zasad autoryzacji lub zapisu niestandardowych zasad autoryzacji za pośrednictwem pisania niestandardowego filtru. Implementacja wbudowany filtr jest po prostu odpowiedzialny za wywołanie systemu autoryzacji.

Nie należy zgłaszać wyjątki, w ramach filtry autoryzacji, ponieważ nic nie będzie obsługiwać wyjątek (filtry wyjątków nie będzie obsługiwać je). Należy wziąć pod uwagę, wydawanie wyzwanie w przypadku, gdy wystąpi wyjątek.

Dowiedz się więcej o [autoryzacji](xref:security/authorization/introduction).

## <a name="resource-filters"></a>Filtry zasobów

* Implementowanie albo `IResourceFilter` lub `IAsyncResourceFilter` interfejsu
* Ich wykonanie opakowuje większość potoku filtru. 
* Tylko [filtry autoryzacji](#authorization-filters) uruchamiane przed filtrami zasobów.

Filtry zasobów są przydatne do zwarcie większość pracy, który wykonuje żądanie. Na przykład filtr pamięci podręcznej można uniknąć pozostałego potoku, jeśli odpowiedź znajduje się w pamięci podręcznej.

[Krótki circuiting filtr zasobu](#short-circuiting-resource-filter) przedstawionej wcześniej jest jednym z przykładów filtr zasobów. Innym przykładem jest [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):

* Uniemożliwia ona powiązanie modelu, uzyskanie dostępu do danych formularza. 
* Jest przydatne w przypadku plików o dużym rozmiarze przekazywanie i aby zapobiec wczytywana pamięci w formularzu.

## <a name="action-filters"></a>Filtry akcji

> [!IMPORTANT]
> Filtry akcji wykonaj **nie** dotyczą stron Razor. Strony razor obsługuje <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> . Aby uzyskać więcej informacji, zobacz [metody filtrowania dla stron Razor](xref:razor-pages/filter).

*Filtry akcji*:

* Implementowanie albo `IActionFilter` lub `IAsyncActionFilter` interfejsu.
* Ich wykonanie otacza wykonywania metody akcji.

Poniżej przedstawiono przykładowy filtr akcji:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

<xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> Zawiera następujące właściwości:

* `ActionArguments` — umożliwia manipulowanie dane wejściowe akcji.
* `Controller` — umożliwia manipulowanie wystąpienie kontrolera. 
* `Result` -Ustawienie short-circuits wykonywania metody akcji i filtry kolejnych działań. Zostanie zgłoszony wyjątek powoduje także uniemożliwia wykonanie metody akcji i kolejne filtry, ale jest traktowana jako błąd zamiast pomyślnego wyniku.

<xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> Zapewnia `Controller` i `Result` oraz następujące właściwości:

* `Canceled` -będzie mieć wartość true, jeśli zwartym został wykonanie akcji przez inny filtr.
* `Exception` -będzie inna niż null, jeśli akcji lub filtru akcji kolejnych zgłosiła wyjątek. Ustawienie tej właściwości na wartość null skutecznie "handles" wyjątek, i `Result` będą wykonywane tak, jakby on zazwyczaj zwróconych przez metodę akcji.

Aby uzyskać `IAsyncActionFilter`, wywołanie `ActionExecutionDelegate`:

* Wykonuje wszelkie filtry kolejnej akcji i metody akcji.
* Zwraca `ActionExecutedContext`. 

Aby zwarcie, należy przypisać `ActionExecutingContext.Result` niektóre wartości w wyniku wystąpienia i nie wywołuj `ActionExecutionDelegate`.

Struktura dostarcza abstrakcyjną `ActionFilterAttribute` można podklasę. 

Filtr akcji można użyć, aby zweryfikować stan modelu i zwraca wszystkie błędy, jeśli stan jest nieprawidłowy:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

`OnActionExecuted` Uruchamia metodę po metody akcji i może zobaczyć i manipulowania wynikami akcji za pomocą `ActionExecutedContext.Result` właściwości. `ActionExecutedContext.Canceled` będzie można ustawić wartość true, jeśli jest to zwartym został wykonanie akcji przez inny filtr. `ActionExecutedContext.Exception` zostaną ustawione na wartość inną niż null w przypadku akcji lub filtru akcji kolejnych zgłosiła wyjątek. Ustawienie `ActionExecutedContext.Exception` null:

* Skutecznie "handles" wyjątek.
* `ActionExectedContext.Result` jest wykonywane tak, jakby były zwracane normalnie przez metodę akcji.

## <a name="exception-filters"></a>Filtry wyjątków

*Filtry wyjątków* implementować albo `IExceptionFilter` lub `IAsyncExceptionFilter` interfejsu. One może służyć do implementowania obsługi zasad dla aplikacji typowych błędów. 

Następujący filtr wyjątku przykładowych użyto widoku błędów niestandardowych dla deweloperów, aby wyświetlić szczegóły dotyczące wyjątków, które występują, gdy aplikacja jest w trakcie opracowywania:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

Filtry wyjątków:

* Nie masz, przed i po nim zdarzeń. 
* Implementowanie `OnException` lub `OnExceptionAsync`. 
* Obsługa nieobsługiwane wyjątki występujące podczas tworzenia kontrolera [wiązanie modelu](../models/model-binding.md), filtry akcji lub metody akcji. 
* Nie przechwytuj wyjątków, które występują w filtrów zasobów, filtry wyników lub wykonywania wynik MVC.

Aby obsłużyć wyjątek, należy ustawić `ExceptionContext.ExceptionHandled` właściwości do wartości true lub zapisu odpowiedzi. Spowoduje to zatrzymanie propagacji wyjątku. Filtra wyjątku nie może włączyć wyjątek do "Powodzenie". Filtr akcji można to zrobić.

> [!NOTE]
> W programie ASP.NET Core 1.1 odpowiedź nie jest wysyłana, jeśli ustawisz `ExceptionHandled` TRUE **i** zapisu odpowiedzi. W tym scenariuszu programu ASP.NET Core 1.0 wysłania odpowiedzi i ASP.NET Core 1.1.2 nastąpi powrót do zachowania 1.0. Aby uzyskać więcej informacji, zobacz [wystawiać #5594](https://github.com/aspnet/Mvc/issues/5594) w repozytorium GitHub. 

Filtry wyjątków:

* Dla zastosowań dobre są wyjątki wyłapywanie, które występują w ramach działań platformy MVC.
* Nie są tak elastyczne, oprogramowanie pośredniczące obsługi błędów. 

Preferuj oprogramowanie pośredniczące do obsługi wyjątków. Użyć filtry wyjątków, tylko gdy potrzebujesz obsługi błędów *inaczej* oparte na akcję MVC, która została wybrana. Na przykład aplikacja może mieć metody akcji dla obu punktów końcowych interfejsu API i widoki/HTML. Punkty końcowe interfejsu API może zwrócić informacje o błędach w formacie JSON, natomiast akcje na podstawie widoku może zwrócić strony błędu w formacie HTML.

`ExceptionFilterAttribute` Może być podklasą klasy. 

## <a name="result-filters"></a>Filtry wyników

* Implementowanie albo `IResultFilter` lub `IAsyncResultFilter` interfejsu.
* Ich wykonanie otacza wykonywania wyników akcji. 

Oto przykład filtr wynik, który dodaje nagłówek HTTP.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

Typ wyniku wykonywania zależy od danego działania. Akcja kontrolera MVC, zwracając widoku obejmuje wszystkie razor przetwarzania w ramach `ViewResult` wykonywana. Metoda interfejsu API może wykonywać niektóre serializacji jako część wykonania wyniku. Dowiedz się więcej o [wyników akcji](actions.md)

Filtry wyników są wykonywane tylko na pomyślne wyniki — gdy akcji lub filtry akcji dają wynik akcji. Filtry wyników nie są wykonywane, gdy filtry wyjątków obsługi wyjątku.

`OnResultExecuting` Metoda może zwarcie wykonywania wynik akcji i filtry kolejnych wyników, ustawiając `ResultExecutingContext.Cancel` na wartość true. Ogólnie należy zapisać do obiektu odpowiedzi podczas zwarcie, aby uniknąć generowania pustą odpowiedź. Zostanie zgłoszony zostanie wyjątek:

* Zapobiec wykonaniu wyniku akcji i kolejne filtry.
* Traktowane jako błąd zamiast pomyślnego wyniku.

Gdy `OnResultExecuted` metoda przebiegów, odpowiedź prawdopodobnie została wysłana do klienta i nie może być dodatkowo (o ile nie wystąpił wyjątek). `ResultExecutedContext.Canceled` będzie można ustawić wartość true, jeśli jest to zwartym został wykonanie wynik akcji przez inny filtr.

`ResultExecutedContext.Exception` zostaną ustawione na wartość inną niż null w przypadku wyniku akcji lub filtru kolejnych wyników zgłosiła wyjątek. Ustawienie `Exception` do wartości null skutecznie obsługuje wyjątek i uniemożliwia wyjątek z jest zgłaszany ponownie przez MVC później w potoku. Podczas one obsługi wyjątków w filtrze wynik, nie można zapisywać wszystkie dane na potrzeby odpowiedzi. Jeśli wynik akcji zatrzymuje zgłasza za pośrednictwem jej wykonanie, a już zostać wyczyszczona nagłówki do klienta, nie istnieje mechanizm niezawodne wysłać kod błędu.

Aby uzyskać `IAsyncResultFilter` wywołanie `await next` na `ResultExecutionDelegate` wykonuje wszystkie filtry kolejnych wyników i wyniku akcji. Aby zwarcie, ustaw `ResultExecutingContext.Cancel` na wartość true, a nie wywołuj `ResultExectionDelegate`.

Struktura dostarcza abstrakcyjną `ResultFilterAttribute` można podklasę. [AddHeaderAttribute](#add-header-attribute) klasy wyświetlane wcześniej jest przykładem atrybutów filtru wyniku.

## <a name="using-middleware-in-the-filter-pipeline"></a>Za pomocą oprogramowania pośredniczącego w potoku filtru

Filtry zasobów działają podobnie jak [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) w tym, że ujęty wykonanie wszystkich elementów, których można użyć później w potoku. Jednak filtrów różnią się od oprogramowania pośredniczącego, w tym, że są one częścią MVC, co oznacza, że mają dostęp do kontekstu MVC i konstrukcji.

W programie ASP.NET Core 1.1 można użyć oprogramowania pośredniczącego w potoku filtru. Można to zrobić, jeśli masz składnik oprogramowania pośredniczącego, które wymagają dostępu do danych trasy MVC lub taki, który należy uruchomić tylko w przypadku niektórych kontrolerów i akcji.

Aby korzystać z oprogramowania pośredniczącego jako filtru, Utwórz typ z `Configure` metody, która określa oprogramowania pośredniczącego, które chcesz wstawić do potoku filtru. Oto przykład, który używa oprogramowania pośredniczącego lokalizacji w celu ustanowienia bieżącej kultury na żądanie:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

Następnie można użyć `MiddlewareFilterAttribute` do uruchamiania oprogramowania pośredniczącego dla wybranego kontrolera lub akcji lub globalnie:

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

Oprogramowanie pośredniczące filtry są uruchamiane na tym samym etapie potoku filtru jako zasób filtry, przed powiązaniem modelu i po nim pozostałego potoku.

## <a name="next-actions"></a>Następne akcje

* Zobacz [metody filtrowania dla stron Razor](xref:razor-pages/filter)
* Aby poeksperymentować z filtrami, [pobierania, testowania i zmodyfikować przykładowe Github](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).
