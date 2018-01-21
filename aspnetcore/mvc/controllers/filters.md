---
title: Filtry
author: ardalis
description: "Dowiedz się, jak *filtry* pracy i sposobu ich używania w programie ASP.NET MVC Core."
ms.author: tdykstra
manager: wpickett
ms.date: 12/12/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/filters
ms.openlocfilehash: db5d6a98d5e6702842e8b036c378ed96aef61b70
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="filters"></a>Filtry

Przez [Dykstra Tomasz](https://github.com/tdykstra/) i [Steve Smith](https://ardalis.com/)

*Filtry* na platformie ASP.NET Core MVC pozwala na uruchamianie kodu przed lub po określonym etapie w potoku przetwarzania żądań.

 Filtry wbudowane dojścia zadań, takich jak Autoryzacja (uniemożliwienia dostępu do zasobów, które użytkownik nie jest autoryzowany dla), zapewnienie, że wszystkie żądania przy użyciu protokołu HTTPS i odpowiedzi buforowania (zwarcie Potok żądań do zwrócenia buforowanej odpowiedzi). 

Można tworzyć filtry niestandardowe do obsługi problemów kompleksowymi aplikacji. W dowolnym momencie chcesz uniknąć duplikowania kodu przez akcje, filtry są rozwiązania. Na przykład można skonsolidować kodu w filtrze wyjątku obsługi błędów.

[Wyświetlanie lub pobieranie próbki z usługi GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).

## <a name="how-do-filters-work"></a>Jak działają filtry?

Filtry są uruchamiane w ramach *potok wywołania akcji MVC*, jest czasami nazywany *potoku filtru*.  Potoku filtru jest uruchamiana po MVC wybiera akcję do wykonania.

![Żądanie jest przetwarzane przez inne oprogramowanie pośredniczące, Routing oprogramowania pośredniczącego, wybór akcji i potok MVC wywołania akcji. Przetwarzanie żądań jest nadal powrotem przy użyciu wybór akcji, Routing oprogramowanie pośredniczące i różne inne oprogramowanie pośredniczące aby stać się wysłać do klienta odpowiedź.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>Typy filtrów

Każdego typu filtru jest wykonywana na różnych etapach potoku filtru.

* [Filtry autoryzacji](#authorization-filters) uruchomiony i są używane do ustalenia, czy bieżący użytkownik jest autoryzowany dla bieżącego żądania. Jeśli żądanie jest autoryzowane, ich zwarcia potoku. 

* [Filtry zasobów](#resource-filters) są najpierw do obsługi żądania po autoryzacji.  Mogą uruchamiać kod przed rest potoku filtru, a po zakończeniu pozostałego potoku. Są one przydatne do wdrożenia, buforowanie lub w przeciwnym razie zwarcia potoku filtru ze względu na wydajność. Ponieważ są uruchamiane przed powiązaniem modelu są przydatne w przypadku wszystkich elementów, które ma wpływ wiązania modelu.

* [Filtry akcji](#action-filters) można uruchomić kod bezpośrednio przed i po jest wywoływana metoda poszczególnych akcji. Mogą one używane do manipulowania Argumenty przekazane do akcji i wyniku zwracanego z akcji.

* [Filtry wyjątków](#exception-filters) są używane do stosowania zasad globalnych do nieobsługiwanych wyjątków, które występują przed niczego została zapisana treść odpowiedzi.

* [Powoduje filtry](#result-filters) można uruchomić kod bezpośrednio przed i po wykonaniu wyniki poszczególnych akcji. Uruchom tylko wtedy, gdy metoda akcji została wykonana pomyślnie, a są przydatne w przypadku należy ująć widoku lub program formatujący wykonywania logiki.

Na poniższym diagramie przedstawiono sposób interakcji te typy filtrów w potoku filtru.

![Żądanie jest przetwarzane za pośrednictwem filtry autoryzacji, filtrów zasobów powiązania modelu, filtry akcji, wykonywanie akcji i konwersji wynik akcji, filtry wyjątków, filtry wyników i wykonania wyniku. W drodze limit żądania jest przetwarzany tylko przez filtry wyników i filtry zasobów aby stać się odpowiedzi wysyłane do klienta.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>Implementacja

Filtry obsługuje synchroniczne i asynchroniczne implementacje za pośrednictwem definicji innego interfejsu. Wybierz wariant synchronizacji lub asynchronicznej w zależności od rodzaju zadania, które należy wykonać. 

Synchroniczne filtry, które można uruchomić kod zarówno przed i po ich etap potoku zdefiniować*etap*Executing i na*etap*wykonywane metody. Na przykład `OnActionExecuting` jest wywoływana przed wywołaniem metody akcji i `OnActionExecuted` jest wywoływana po powrocie z metody akcji.

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?highlight=6,8,13)]

Asynchroniczne Filtry definiują jeden na*etap*ExecutionAsync metody. Ta metoda przyjmuje *FilterType*ExecutionDelegate delegata, który wykonuje etap potoku filtru. Na przykład `ActionExecutionDelegate` wywołania metody akcji i może zostać uruchomiony kod przed i po jej wywołaniu.

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

Dla wielu etapów filtrów w jednej klasy mogą implementować interfejsów. Na przykład [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) abstrakcyjna klasa implementuje zarówno `IActionFilter` i `IResultFilter`, a także ich odpowiedniki asynchronicznego.

> [!NOTE]
> Implementowanie **albo** synchronicznego lub wersji asynchronicznego interfejsu filtru, nie oba. Platformę najpierw sprawdza, czy filtr implementuje interfejs asynchronicznych, a jeśli tak, który wywołuje. Jeśli nie wywołuje metody interfejsu synchronicznego. Gdyby implementować interfejsów zarówno na jedną klasę, czy można wywołać tylko metody asynchronicznej. Podczas używania klas abstrakcyjnych, takich jak [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) należy przesłonić metodę metod synchronicznych lub metody asynchronicznej dla każdego typu filtru.

### <a name="ifilterfactory"></a>IFilterFactory

`IFilterFactory`implementuje `IFilter`. W związku z tym `IFilterFactory` wystąpienie może być używane jako `IFilter` wystąpienia dowolne miejsce w potoku filtru. Gdy w ramach przygotowuje się do wywołania filtru, próbuje rzutować go na `IFilterFactory`. Jeśli tego rzutowania zakończy się powodzeniem, `CreateInstance` metoda jest wywoływana w celu utworzenia `IFilter` wystąpienia, która będzie wywołana. Zapewnia bardzo elastyczne projektu, ponieważ potoku filtru dokładne nie trzeba ustawić jawnie podczas uruchamiania aplikacji.

Można zaimplementować `IFilterFactory` na własnych implementacje atrybut jako innego podejścia do tworzenia filtrów:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a>Wbudowany filtr atrybutów

Struktura obejmuje wbudowane opartych na atrybutach filtrów, możesz podklasy i dostosowywać. Na przykład następujący filtr wynik dodaje do odpowiedzi nagłówek.

<a name="add-header-attribute"></a>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

Atrybuty zezwalać filtrów, aby akceptować argumenty, jak pokazano w przykładzie powyżej. Czy dodać ten atrybut do kontrolera lub metody akcji i określ nazwę i wartość nagłówka HTTP:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

Wynik `Index` akcji są wyświetlane poniżej — nagłówki odpowiedzi są wyświetlane w prawym dolnym rogu.

![Developer Tools Microsoft Edge przedstawiający nagłówki odpowiedzi, w tym Steve Smith autora@ardalis](filters/_static/add-header.png)

Kilka interfejsów filtr ma odpowiednie atrybuty, które mogą służyć jako klasy podstawowe dla implementacji niestandardowych.

Atrybuty filtru:

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

`TypeFilterAttribute`i `ServiceFilterAttribute` omówiono [dalszej części tego artykułu](#dependency-injection).

## <a name="filter-scopes-and-order-of-execution"></a>Filtr zakresów i kolejność wykonywania

Filtr można dodać do potoku w jednej z trzech *zakresy*. Można dodać filtr do konkretnej metody akcji lub do klasy kontrolera przy użyciu atrybutu. Możesz zarejestrować filtr globalny (dla wszystkich kontrolerów i akcji), dodając ją do `MvcOptions.Filters` kolekcji w `ConfigureServices` metoda `Startup` klasy:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

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
| 1 | Globalne | `OnActionExecuting` |
| 2 | Kontrolera | `OnActionExecuting` |
| 3 | Metoda | `OnActionExecuting` |
| 4 | Metoda | `OnActionExecuted` |
| 5 | Kontrolera | `OnActionExecuted` |
| 6 | Globalne | `OnActionExecuted` |

Ta sekwencja zawiera filtr metody jest zagnieżdżony w filtrze kontrolera, czy filtr kontrolera jest zagnieżdżony w filtrów globalnych. Go umieścić inny sposób, jeśli wewnątrz filtr async obiektu*etap*ExecutionAsync, wszystkie filtry z większego zakresu można uruchomić metody zablokowaniu kodu na stosie.

> [!NOTE]
> Każdy kontroler, który dziedziczy z `Controller` klasa podstawowa zawiera `OnActionExecuting` i `OnActionExecuted` metody. Te metody zawijać systemem filtry dla danej akcji: `OnActionExecuting` jest wywoływana przed każdą z filtrów, i `OnActionExecuted` jest wywoływana po wykonaniu wszystkich filtrów.

### <a name="overriding-the-default-order"></a>Zastępowanie domyślna kolejność

Można zastąpić domyślną sekwencją wykonywania zaimplementowanie `IOrderedFilter`. Ten interfejs przedstawia `Order` właściwość, która ma pierwszeństwo przed zakres, aby określić kolejność wykonywania. Filtr o niższej `Order` będzie mieć wartość jego *przed* kod wykonywany przed nim filtr o wyższej wartości `Order`. Filtr o niższej `Order` będzie mieć wartość jego *po* kod wykonywany po filtru o wyższej `Order` wartość. Można ustawić `Order` właściwości przy użyciu parametru konstruktora:

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

Jeśli mają taki sam 3 filtry działania wyświetlane w poprzednim przykładzie, ale zestaw `Order` właściwości kontrolera i globalnych filtrów 1 i 2 odpowiednio, będzie można odwrócić kolejność wykonywania.

| Sekwencja | Zakresu filtru | `Order`Właściwość | Filter — metoda |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1 | Metoda | 0 | `OnActionExecuting` |
| 2 | Kontrolera | 1  | `OnActionExecuting` |
| 3 | Globalne | 2  | `OnActionExecuting` |
| 4 | Globalne | 2  | `OnActionExecuted` |
| 5 | Kontrolera | 1  | `OnActionExecuted` |
| 6 | Metoda | 0  | `OnActionExecuted` |

`Order` Atu właściwości zakresu podczas określania kolejność uruchamiania filtrów. Filtry są najpierw posortowane według kolejności, a następnie zakres jest używany do dzielenia ties. Wszystkie filtry wbudowane zaimplementować `IOrderedFilter` i ustaw wartość domyślna `Order` wartość 0, aby zakres określa porządek, chyba że zostanie ustawiony `Order` na wartość inną niż zero.

## <a name="cancellation-and-short-circuiting"></a>Anulowanie i krótki circuiting

Potoku filtru w dowolnym momencie można zwarcia przez ustawienie `Result` właściwość `context` parametrów przekazane do metody filtru. Na przykład następujący filtr zasobów uniemożliwia wykonywanie pozostałego potoku.

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

W poniższym kodzie zarówno `ShortCircuitingResourceFilter` i `AddHeader` filtr docelowy `SomeResource` metody akcji. Jednak ponieważ `ShortCircuitingResourceFilter` jest uruchamiany pierwszy (ponieważ jest on filtr zasobów i `AddHeader` jest filtr akcji) i short-circuits pozostałego potoku, `AddHeader` filtru nigdy nie jest uruchomione `SomeResource` akcji. To zachowanie będzie taki sam, jeśli oba filtry zostały zastosowane na poziomie — metoda akcji, podany `ShortCircuitingResourceFilter` uruchomiono pierwszy (ze względu na jego typ filtru bezpośredniego lub pośredniego stosowania `Order` właściwości, na przykład).

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>Iniekcji zależności

Filtry można dodać według typu lub wystąpienia. Jeśli dodasz wystąpienia to wystąpienie będzie używany dla każdego żądania. Jeśli dodasz typu będą typu została aktywowana, co oznacza, będzie można utworzyć wystąpienia dla każdego żądania oraz wszelkie zależności konstruktora zostaną wypełnione przez [iniekcji zależności](../../fundamentals/dependency-injection.md) (Podpisane). Dodawanie filtru według typu jest odpowiednikiem `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`.

Filtry, które są zaimplementowane jako atrybuty i dodać bezpośrednio do klasy kontrolera lub metody akcji nie może mieć zależności konstruktora dostarczonych przez [iniekcji zależności](../../fundamentals/dependency-injection.md) (Podpisane). Jest to spowodowane atrybutów musi mieć ich parametrami konstruktora dostarczony, w których są stosowane. Jest to ograniczenie, jak sprawdzić atrybuty.

Jeśli filtry zależności, które chcą korzystać z Podpisane, jest kilka metod obsługiwanych. Filtr można zastosować do klasy lub metody akcji przy użyciu jednej z następujących czynności:

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* `IFilterFactory`na atrybut

> [!NOTE]
> Jeden zależności, które można pobrać z Podpisane jest rejestrator. Należy jednak unikać tworzenia i używania filtrów wyłącznie w celach rejestrowania, ponieważ [funkcji rejestrowania wbudowana struktura](xref:fundamentals/logging/index) już może udostępnić, co jest potrzebne. Jeśli zamierzasz dodać rejestrowania do filtrów, należy skoncentrować się na problemy biznesowe domeny lub zachowanie specyficzne dla filtru, a nie MVC akcje lub inne zdarzenia framework.

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

A `ServiceFilter` pobiera wystąpienia filtru z Podpisane. Dodaj filtr do kontenera w `ConfigureServices`i odwołuje się on w `ServiceFilter` atrybutu

[!code-csharp[Main](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[Main](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

Przy użyciu `ServiceFilter` bez rejestrowania wyników filtrowania typ wyjątku:

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

`ServiceFilterAttribute`implementuje `IFilterFactory`, który opisuje metodę pojedynczego tworzenia `IFilter` wystąpienia. W przypadku liczby `ServiceFilterAttribute`, `IFilterFactory` interfejsu `CreateInstance` zaimplementowano metody można załadować określonego typu z kontenera usługi (Podpisane).

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute`jest bardzo podobny do `ServiceFilterAttribute` (i implementuje również `IFilterFactory`), ale jego typ nie został rozpoznany bezpośrednio z kontenera Podpisane. Zamiast tego tworzy typu przy użyciu `Microsoft.Extensions.DependencyInjection.ObjectFactory`.

Z powodu tej różnicy typy, które są używane, za pomocą `TypeFilterAttribute` nie muszą być najpierw zarejestrowany w usłudze kontenera (ale nadal będzie miał zależności są spełnione przez kontener). Ponadto `TypeFilterAttribute` opcjonalnie mogą akceptować argumenty konstruktora dla danego typu. W poniższym przykładzie pokazano sposób przekazać argumenty do typu przy użyciu `TypeFilterAttribute`:

[!code-csharp[Main](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

Jeśli masz filtr, który nie wymaga żadnych argumentów, ale która zawiera zależności konstruktora, które muszą zostać wypełnione przez Podpisane, można użyć własnego atrybutu nazwanego na klasy i metody zamiast `[TypeFilter(typeof(FilterType))]`). Następujący filtr pokazuje, jak to można zaimplementować:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

Ten filtr można zastosować do klasy lub metody za pomocą `[SampleActionFilter]` składni, zamiast `[TypeFilter]` lub `[ServiceFilter]`.

## <a name="authorization-filters"></a>Filtry autoryzacji

*Filtry autoryzacji* kontroli dostępu do metody akcji i pierwszy filtrów do wykonania w potoku filtru. Mają one tylko przed metody, w przeciwieństwie do większości filtry, które obsługują przed i po metody. Należy tylko zapisać filtr autoryzacji niestandardowej Jeśli piszesz własne framework autoryzacji. Preferowane jest konfigurowanie zasad autoryzacji lub zapisywanie niestandardowych zasad autoryzacji przez zapisywanie filtru niestandardowego. Implementacja wbudowany filtr tylko odpowiada za wywołanie systemu autoryzacji.

Należy pamiętać, należy nie generowanie wyjątków w ramach filtry autoryzacji, ponieważ nic nie będzie obsługiwać wyjątek (filtry wyjątków nie będzie obsługiwać je). Zamiast tego wydania wyzwania lub znaleźć inny sposób.

Dowiedz się więcej o [autoryzacji](../../security/authorization/index.md).

## <a name="resource-filters"></a>Filtry zasobów

*Filtry zasobów* implementować albo `IResourceFilter` lub `IAsyncResourceFilter` interfejs, a ich wykonanie większość potoku filtru opakowuje. (Tylko [filtry autoryzacji](#authorization-filters) uruchamiane przed ich.) Filtry zasobów są szczególnie przydatne, jeśli zachodzi konieczność zwarcia większość pracy, który wykonuje żądanie. Na przykład filtr buforowania można uniknąć pozostałego potoku, jeśli odpowiedź jest już w pamięci podręcznej.

[Krótki circuiting filtru zasobów](#short-circuiting-resource-filter) przedstawiona wcześniej jest jednym z przykładów filtr zasobów. Innym przykładem jest [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs), co uniemożliwia wiązania modelu uzyskanie dostępu do danych formularza. Jest to przydatne, gdy wiesz, czy użytkownik chce otrzymywać wysyłania dużych plików i chcesz uniemożliwić formularzu odczytywania do pamięci.

## <a name="action-filters"></a>Filtry akcji

*Filtry akcji* implementować albo `IActionFilter` lub `IAsyncActionFilter` interfejs, a ich wykonanie otaczający wykonywanie metod akcji.

Oto przykładowy filtr akcji:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

[ActionExecutingContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext) zapewnia następujące właściwości:

* `ActionArguments`— umożliwia manipulowanie wejść do akcji.
* `Controller`— umożliwia manipulowanie wystąpienie kontrolera. 
* `Result`— to ustawienie short-circuits wykonywanie metody akcji i filtry akcji kolejne. Zgłaszanie wyjątku powoduje również uniemożliwia wykonanie metody akcji i kolejne filtrów, ale jest traktowana jako błąd zamiast pomyślnego wyniku.

[ActionExecutedContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext) zapewnia `Controller` i `Result` oraz następujące właściwości:

* `Canceled`-będzie mieć wartość true, jeśli zwartym został wykonanie akcji przez inny filtr.
* `Exception`-mieć wartości null, jeśli akcji lub filtr akcji kolejnych zwrócił wyjątek. Ustawienie tej właściwości na wartość null, efektywnie "handles" Wystąpił wyjątek, i `Result` będą wykonywane tak, jakby jego zwykle zwróconych przez metodę akcji.

Aby uzyskać `IAsyncActionFilter`, wywołanie `ActionExecutionDelegate` wykonuje wszystkie filtry akcji kolejnych i metody akcji, zwracając `ActionExecutedContext`. Zwarcie, Przypisz `ActionExecutingContext.Result` niektóre wartości w wyniku wystąpienia i nie wymagają `ActionExecutionDelegate`.

Abstrakcyjnego zapewnia platformę `ActionFilterAttribute` mogących podklasy. 

Filtr akcji służy do automatycznego sprawdzania poprawności stanu modelu i zwraca błędy, jeśli stan jest nieprawidłowy:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

`OnActionExecuted` Metody działa po metody akcji i umożliwia Zobacz i manipulowania wyniki akcji za pomocą `ActionExecutedContext.Result` właściwości. `ActionExecutedContext.Canceled`zostanie ustawiona na true, jeśli zwartym został wykonanie akcji przez inny filtr. `ActionExecutedContext.Exception`zostanie ustawiona na wartość inną niż null Jeśli akcji lub filtr akcji kolejnych zwrócił wyjątek. Ustawienie `ActionExecutedContext.Exception` równej null, efektywnie "handles" Wystąpił wyjątek i `ActionExectedContext.Result` będą następnie wykonywane tak, jakby jego zwykle zwróconych przez metodę akcji.

## <a name="exception-filters"></a>Filtry wyjątków

*Filtry wyjątków* implementować albo `IExceptionFilter` lub `IAsyncExceptionFilter` interfejsu. Mogą one używane do implementowania obsługi zasady dla aplikacji typowych błędów. 

Następujący przykładowy filtr wyjątek używa widoku błędów niestandardowych developer Aby wyświetlić szczegóły dotyczące wyjątków, które występują, gdy aplikacja jest rozwijany:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

Filtry wyjątków nie mają dwa zdarzenia (dla przed i po) — tylko zaimplementować `OnException` (lub `OnExceptionAsync`). 

Filtry wyjątków obsługi nieobsługiwanych wyjątków, które występują w tworzenia kontrolera [modelu powiązania](../models/model-binding.md), filtry akcji lub metody akcji. Nie będą one catch wyjątków, które występują w filtrów zasobów, filtry wyników lub wykonywania wynik MVC.

Do obsługi wyjątku, ustaw `ExceptionContext.ExceptionHandled` właściwości na wartość true lub zapisu odpowiedzi. Powoduje to zatrzymanie propagacji wyjątku. Należy pamiętać, że filtra wyjątku nie można włączyć wyjątek do "Powodzenie". Filtr akcji można to zrobić.

> [!NOTE]
> W ASP.NET 1.1 odpowiedzi nie są wysyłane po ustawieniu `ExceptionHandled` TRUE **i** zapisu odpowiedzi. W tym scenariuszu platformy ASP.NET Core 1.0 wysyłania odpowiedzi i platformy ASP.NET Core 1.1.2 powróci do zachowania 1.0. Aby uzyskać więcej informacji, zobacz [wystawiać #5594](https://github.com/aspnet/Mvc/issues/5594) w repozytorium GitHub. 

Filtry wyjątków są dobrym zalewania wyjątków, które występują w ramach działań MVC, ale nie są one tak elastyczne jako błąd obsługi oprogramowania pośredniczącego. Preferowane jest oprogramowanie pośredniczące w przypadku ogólnych i za pomocą filtrów, tylko gdy należy wykonywać obsługi błędów *inaczej* oparte na Akcja kontrolera MVC, który został wybrany. Na przykład aplikacja może mieć metody akcji dla obu punkty końcowe interfejsu API i widoki/HTML. Punkty końcowe interfejsu API może zwrócić informacji o błędach w formacie JSON, gdy czynności na podstawie widok może zwrócić strony błędu w formacie HTML.

Abstrakcyjnego zapewnia platformę `ExceptionFilterAttribute` mogących podklasy. 

## <a name="result-filters"></a>Filtry wyników

*Powoduje filtry* implementować albo `IResultFilter` lub `IAsyncResultFilter` interfejs, a ich wykonanie otaczający wykonywania wyników akcji. 

Oto przykład filtr wynik, który dodaje nagłówek HTTP.

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

Typ wyniku wykonywana zależy od danego działania. Akcja kontrolera MVC, zwracając widoku obejmie wszystkie razor przetwarzania jako część `ViewResult` wykonywana. Metody interfejsu API może wykonać niektóre serializacji w ramach wykonania wyniku. Dowiedz się więcej o [wyników akcji](actions.md)

Filtry wyników są wykonywane tylko dla pomyślne wyniki — po akcji lub filtrów akcji dają wyniku akcji. Filtry wyników nie są wykonywane, gdy filtry wyjątków obsługi wyjątku.

`OnResultExecuting` Metody może zwarcia wynik akcji i filtry wyników kolejnych przez ustawienie `ResultExecutingContext.Cancel` na wartość true. Ogólnie należy zapisać obiekt odpowiedzi podczas zwarcie, aby uniknąć generowania pustą odpowiedź. Zgłaszanie wyjątku zostanie również zapobiec wykonaniu wyniku akcji i kolejne filtrów, ale będzie traktowana jako błąd zamiast pomyślnego wyniku.

Gdy `OnResultExecuted` metody działa, odpowiedzi prawdopodobnie zostało wysłane do klienta i nie może być więcej (o ile nie wystąpił wyjątek). `ResultExecutedContext.Canceled`zostanie ustawiona na true, jeśli zwartym został wykonywania wynik akcji przez inny filtr.

`ResultExecutedContext.Exception`zostanie ustawiona na wartość inną niż null, jeśli wynik akcji lub filtru wyników kolejnych zwrócił wyjątek. Ustawienie `Exception` do wartości null skutecznie "handles" wyjątku i uniemożliwia wyjątku z zostanie zgłoszony przez MVC później w potoku. Jest obsługi wyjątków w filtrze wynik, nie można zapisać danych do odpowiedzi. Jeśli wynik akcji zgłasza wyjątek w tym za pomocą działania, a nagłówki zostały opróżnione do klienta, nie istnieje mechanizm niezawodnej wysłać kod błędu.

Aby uzyskać `IAsyncResultFilter` wywołanie `await next()` na `ResultExecutionDelegate` wykonuje wszystkie filtry wyników kolejnych i wyniku akcji. Aby zwarcia, ustaw `ResultExecutingContext.Cancel` na wartość PRAWDA, a nie wywołuj `ResultExectionDelegate`.

Abstrakcyjnego zapewnia platformę `ResultFilterAttribute` mogących podklasy. [AddHeaderAttribute](#add-header-attribute) klasy przedstawiona wcześniej jest przykładem atrybutów filtru wyniku.

## <a name="using-middleware-in-the-filter-pipeline"></a>Za pomocą oprogramowania pośredniczącego w potoku filtru

Filtry zasobów działają podobnie jak [oprogramowanie pośredniczące](../../fundamentals/middleware.md) w tym ujęty wykonanie wszystkich elementów, które później w potoku. Jednak filtry różnią się z oprogramowania pośredniczącego, że są one częścią MVC, co oznacza, że mają dostęp do kontekstu MVC i konstrukcji.

W ASP.NET Core 1.1 można użyć oprogramowania pośredniczącego w potoku filtru. Można to zrobić, jeśli składnik oprogramowania pośredniczącego, które wymagają dostępu do danych trasy MVC lub taki, który należy uruchamiać tylko dla niektórych kontrolerach ani akcji.

Aby korzystać z oprogramowania pośredniczącego jako filtru, Utwórz typ z `Configure` metodę, która określa oprogramowanie pośredniczące, który chcesz wstawić do potoku filtru. Oto przykład, która używa oprogramowania pośredniczącego lokalizacji do określania bieżącej kultury dla żądania:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

Następnie można użyć `MiddlewareFilterAttribute` do uruchomienia oprogramowania pośredniczącego dla wybranego kontrolera lub akcji lub globalnie:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

Oprogramowanie pośredniczące filtry są uruchamiane na tym samym etapie potoku filtru jako filtrów zasobów, przed powiązaniem modelu i po pozostałego potoku.

## <a name="next-actions"></a>Kolejne czynności

Do eksperymentów z filtrów, [pobierania, testowania i zmodyfikować próbki](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).
