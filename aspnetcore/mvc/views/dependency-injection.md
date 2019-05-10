---
title: Wstrzykiwanie zależności do widoków w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak platforma ASP.NET Core obsługuje wstrzykiwanie zależności do widoków MVC.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/dependency-injection
ms.openlocfilehash: b411b164bfea81f82c5c9fc1052e0ecfe65f0bc2
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2019
ms.locfileid: "65517051"
---
# <a name="dependency-injection-into-views-in-aspnet-core"></a><span data-ttu-id="f5e16-103">Wstrzykiwanie zależności do widoków w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f5e16-103">Dependency injection into views in ASP.NET Core</span></span>

<span data-ttu-id="f5e16-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="f5e16-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f5e16-105">Obsługuje platformy ASP.NET Core [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) do widoków.</span><span class="sxs-lookup"><span data-stu-id="f5e16-105">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="f5e16-106">Może to być przydatne w przypadku usługi specyficzne dla widoku, takie jak lokalizacja lub wymagane tylko w przypadku wypełnianie elementy widoku danych.</span><span class="sxs-lookup"><span data-stu-id="f5e16-106">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="f5e16-107">Należy starać się utrzymać [separacji](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) między widoków i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="f5e16-107">You should try to maintain [separation of concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) between your controllers and views.</span></span> <span data-ttu-id="f5e16-108">Większość danych, których wyświetlanie widoków powinien być przekazywany w z kontrolera.</span><span class="sxs-lookup"><span data-stu-id="f5e16-108">Most of the data your views display should be passed in from the controller.</span></span>

<span data-ttu-id="f5e16-109">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f5e16-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="configuration-injection"></a><span data-ttu-id="f5e16-110">Iniekcja konfiguracji</span><span class="sxs-lookup"><span data-stu-id="f5e16-110">Configuration injection</span></span>

<span data-ttu-id="f5e16-111">*appSettings.JSON* wartości może wprowadzone bezpośrednio w widoku.</span><span class="sxs-lookup"><span data-stu-id="f5e16-111">*appsettings.json* values can be injected directly into a view.</span></span>

<span data-ttu-id="f5e16-112">Przykład *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="f5e16-112">Example of an *appsettings.json* file:</span></span>

```json
{
   "root": {
      "parent": {
         "child": "myvalue"
      }
   }
}
```

<span data-ttu-id="f5e16-113">Składnia `@inject`: `@inject <type> <name>`</span><span class="sxs-lookup"><span data-stu-id="f5e16-113">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="f5e16-114">Przykłady dotyczące używania `@inject`:</span><span class="sxs-lookup"><span data-stu-id="f5e16-114">An example using `@inject`:</span></span>

```csharp
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration
@{
   string myValue = Configuration["root:parent:child"];
   ...
}
```

## <a name="service-injection"></a><span data-ttu-id="f5e16-115">Uruchomienie usługi</span><span class="sxs-lookup"><span data-stu-id="f5e16-115">Service injection</span></span>

<span data-ttu-id="f5e16-116">Usługa może zostać wprowadzona w widoku, używając `@inject` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="f5e16-116">A service can be injected into a view using the `@inject` directive.</span></span> <span data-ttu-id="f5e16-117">Można potraktować `@inject` Dodawanie właściwości do widoku i wypełnienie właściwości, używając DI.</span><span class="sxs-lookup"><span data-stu-id="f5e16-117">You can think of `@inject` as adding a property to the view, and populating the property using DI.</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

<span data-ttu-id="f5e16-118">Ten widok przedstawia listę `ToDoItem` wystąpień, wraz z podsumowaniem przedstawiająca ogólne statystyki.</span><span class="sxs-lookup"><span data-stu-id="f5e16-118">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="f5e16-119">Podsumowanie jest wypełniana od wprowadzonego `StatisticsService`.</span><span class="sxs-lookup"><span data-stu-id="f5e16-119">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="f5e16-120">Ta usługa jest zarejestrowana dla wstrzykiwanie zależności w `ConfigureServices` w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f5e16-120">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

<span data-ttu-id="f5e16-121">`StatisticsService` Wykonywania niektórych obliczeń dla zestawu `ToDoItem` wystąpień, które uzyskuje dostęp za pośrednictwem repozytorium:</span><span class="sxs-lookup"><span data-stu-id="f5e16-121">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,25)]

<span data-ttu-id="f5e16-122">Przykładowe repozytorium korzysta z kolekcji w pamięci.</span><span class="sxs-lookup"><span data-stu-id="f5e16-122">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="f5e16-123">Implementacja powyżej (który działa na wszystkich danych w pamięci) nie jest zalecane w przypadku dużych, zdalny dostęp do zestawów danych.</span><span class="sxs-lookup"><span data-stu-id="f5e16-123">The implementation shown above (which operates on all of the data in memory) isn't recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="f5e16-124">Przykład wyświetla dane z modelu powiązany z widoku i usługi, które są wstrzykiwane do widoku:</span><span class="sxs-lookup"><span data-stu-id="f5e16-124">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![Aby wyświetlić listę łączna liczba elementów ukończone elementy, średni priorytet i Lista zadań wraz z ich poziomy priorytetów i wartości logiczne, wskazując ukończenia.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="f5e16-126">Wypełnianie danych wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="f5e16-126">Populating Lookup Data</span></span>

<span data-ttu-id="f5e16-127">Iniekcja widok może być przydatne do wypełniania opcje w elementach interfejsu użytkownika, takich jak listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="f5e16-127">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="f5e16-128">Należy wziąć pod uwagę formularz profilu użytkownika, która obejmuje opcje określania płeć, stan i inne preferencje.</span><span class="sxs-lookup"><span data-stu-id="f5e16-128">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="f5e16-129">Renderowanie formularza przy użyciu standardowego podejścia MVC wymagałoby kontrolera w celu żądania usługi dostępu do danych dla każdego z tych zestawów opcji, a następnie wypełnij modelu lub `ViewBag` z każdym zestawem opcji, aby powiązać.</span><span class="sxs-lookup"><span data-stu-id="f5e16-129">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="f5e16-130">Alternatywne podejście wprowadza services bezpośrednio w widoku, aby uzyskać opcje.</span><span class="sxs-lookup"><span data-stu-id="f5e16-130">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="f5e16-131">Zmniejsza to ilość kodu wymaganą przez kontroler przeniesienie tę logikę budowy elementu widoku do samego widoku.</span><span class="sxs-lookup"><span data-stu-id="f5e16-131">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="f5e16-132">Akcji kontrolera, aby wyświetlić formularz edycji profilu wystarczy przekazać formularz wystąpienia profilu:</span><span class="sxs-lookup"><span data-stu-id="f5e16-132">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

<span data-ttu-id="f5e16-133">Formularza HTML, używane do aktualizowania tych preferencji zawiera listy rozwijane dla trzech właściwości:</span><span class="sxs-lookup"><span data-stu-id="f5e16-133">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![Zaktualizuj widoku profilu użytkownika z formularzem, umożliwiając wprowadzanie nazwy, płeć, stanu i ulubionych kolorów.](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="f5e16-135">Te listy są wypełniane przez usługę, która ma został wprowadzony w widoku:</span><span class="sxs-lookup"><span data-stu-id="f5e16-135">These lists are populated by a service that has been injected into the view:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

<span data-ttu-id="f5e16-136">`ProfileOptionsService` Jest usługą poziomu interfejsu użytkownika, zaprojektowana w celu zapewnienia tylko dane, które są wymagane dla tego formularza:</span><span class="sxs-lookup"><span data-stu-id="f5e16-136">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

> [!IMPORTANT]
> <span data-ttu-id="f5e16-137">Nie należy zapominać zarejestrować typy żądań za pośrednictwem wstrzykiwanie zależności w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f5e16-137">Don't forget to register types you request through dependency injection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="f5e16-138">Niezarejestrowany typ zgłasza wyjątek w czasie wykonywania, ponieważ usługodawcy wewnętrznie otrzymaniu kwerendy za pośrednictwem [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice).</span><span class="sxs-lookup"><span data-stu-id="f5e16-138">An unregistered type throws an exception at runtime because the service provider is internally queried via [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice).</span></span>

## <a name="overriding-services"></a><span data-ttu-id="f5e16-139">Zastępowanie usług</span><span class="sxs-lookup"><span data-stu-id="f5e16-139">Overriding Services</span></span>

<span data-ttu-id="f5e16-140">Oprócz wprowadza nowe usługi, ta technika może również zastąpić uprzednio wprowadzony usług na stronie.</span><span class="sxs-lookup"><span data-stu-id="f5e16-140">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="f5e16-141">Na poniższym rysunku przedstawiono wszystkie pola, które są dostępne na stronie używany w pierwszym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="f5e16-141">The figure below shows all of the fields available on the page used in the first example:</span></span>

![Menu kontekstowe funkcji IntelliSense na typizowaną listę pól Html, składnika, StatsService i adres Url symbol @](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="f5e16-143">Jak widać, pól domyślnych obejmują `Html`, `Component`, i `Url` (także `StatsService` , firma Microsoft wprowadzony).</span><span class="sxs-lookup"><span data-stu-id="f5e16-143">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="f5e16-144">Jeśli na przykład chcesz zastąpić domyślny pomocników HTML swoją własną, użytkownik może łatwo to zrobić za pomocą `@inject`:</span><span class="sxs-lookup"><span data-stu-id="f5e16-144">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

<span data-ttu-id="f5e16-145">Jeśli chcesz rozszerzyć istniejące usługi, możesz po prostu użyć tej techniki podczas dziedziczących lub zawijania istniejącego wdrożenia za pomocą własnych.</span><span class="sxs-lookup"><span data-stu-id="f5e16-145">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="f5e16-146">Zobacz też</span><span class="sxs-lookup"><span data-stu-id="f5e16-146">See Also</span></span>

* <span data-ttu-id="f5e16-147">Simon Timms Blog: [Pobieranie danych wyszukiwania do widoku](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span><span class="sxs-lookup"><span data-stu-id="f5e16-147">Simon Timms Blog: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>
