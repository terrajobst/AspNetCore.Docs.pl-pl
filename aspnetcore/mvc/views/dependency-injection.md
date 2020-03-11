---
title: Wstrzykiwanie zależności do widoków w ASP.NET Core
author: ardalis
description: Dowiedz się, jak ASP.NET Core obsługuje iniekcję zależności w widokach MVC.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/dependency-injection
ms.openlocfilehash: 6241bb8e262f64e2e30721bc5fe6f8f1be84b60d
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656101"
---
# <a name="dependency-injection-into-views-in-aspnet-core"></a><span data-ttu-id="9d093-103">Wstrzykiwanie zależności do widoków w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d093-103">Dependency injection into views in ASP.NET Core</span></span>

<span data-ttu-id="9d093-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="9d093-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="9d093-105">ASP.NET Core obsługuje [iniekcję zależności](xref:fundamentals/dependency-injection) w widokach.</span><span class="sxs-lookup"><span data-stu-id="9d093-105">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="9d093-106">Może to być przydatne w przypadku usług specyficznych dla widoku, takich jak lokalizacja lub dane wymagane tylko do wypełniania elementów widoku.</span><span class="sxs-lookup"><span data-stu-id="9d093-106">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="9d093-107">Należy spróbować zachować [rozdzielenie problemów](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) między kontrolerami i widokami.</span><span class="sxs-lookup"><span data-stu-id="9d093-107">You should try to maintain [separation of concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) between your controllers and views.</span></span> <span data-ttu-id="9d093-108">Większość danych wyświetlanych w widokach powinna zostać przeniesiona z kontrolera.</span><span class="sxs-lookup"><span data-stu-id="9d093-108">Most of the data your views display should be passed in from the controller.</span></span>

<span data-ttu-id="9d093-109">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9d093-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="configuration-injection"></a><span data-ttu-id="9d093-110">Iniekcja konfiguracji</span><span class="sxs-lookup"><span data-stu-id="9d093-110">Configuration injection</span></span>

<span data-ttu-id="9d093-111">wartości *appSettings. JSON* można wstrzyknąć bezpośrednio do widoku.</span><span class="sxs-lookup"><span data-stu-id="9d093-111">*appsettings.json* values can be injected directly into a view.</span></span>

<span data-ttu-id="9d093-112">Przykład pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="9d093-112">Example of an *appsettings.json* file:</span></span>

```json
{
   "root": {
      "parent": {
         "child": "myvalue"
      }
   }
}
```

<span data-ttu-id="9d093-113">Składnia `@inject`: `@inject <type> <name>`</span><span class="sxs-lookup"><span data-stu-id="9d093-113">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="9d093-114">Przykład przy użyciu `@inject`:</span><span class="sxs-lookup"><span data-stu-id="9d093-114">An example using `@inject`:</span></span>

```csharp
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration
@{
   string myValue = Configuration["root:parent:child"];
   ...
}
```

## <a name="service-injection"></a><span data-ttu-id="9d093-115">Wstrzykiwanie usługi</span><span class="sxs-lookup"><span data-stu-id="9d093-115">Service injection</span></span>

<span data-ttu-id="9d093-116">Usługę można wstrzyknąć do widoku przy użyciu dyrektywy `@inject`.</span><span class="sxs-lookup"><span data-stu-id="9d093-116">A service can be injected into a view using the `@inject` directive.</span></span> <span data-ttu-id="9d093-117">Można traktować `@inject` jak dodawanie właściwości do widoku i wypełnianie właściwości przy użyciu DI.</span><span class="sxs-lookup"><span data-stu-id="9d093-117">You can think of `@inject` as adding a property to the view, and populating the property using DI.</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

<span data-ttu-id="9d093-118">Ten widok przedstawia listę wystąpień `ToDoItem` wraz z podsumowaniem pokazującym ogólną statystykę.</span><span class="sxs-lookup"><span data-stu-id="9d093-118">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="9d093-119">Podsumowanie zostanie wypełnione na podstawie wstrzykiwanych `StatisticsService`.</span><span class="sxs-lookup"><span data-stu-id="9d093-119">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="9d093-120">Ta usługa jest zarejestrowana na potrzeby iniekcji zależności w `ConfigureServices` w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9d093-120">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

<span data-ttu-id="9d093-121">`StatisticsService` wykonuje pewne obliczenia dotyczące zestawu `ToDoItem` wystąpień, do którego uzyskuje dostęp za pośrednictwem repozytorium:</span><span class="sxs-lookup"><span data-stu-id="9d093-121">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,25)]

<span data-ttu-id="9d093-122">Przykładowe repozytorium używa kolekcji w pamięci.</span><span class="sxs-lookup"><span data-stu-id="9d093-122">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="9d093-123">Implementacja pokazana powyżej (która operuje na wszystkich danych w pamięci) nie jest zalecana w przypadku dużych, zdalnych dostępnych zestawów danych.</span><span class="sxs-lookup"><span data-stu-id="9d093-123">The implementation shown above (which operates on all of the data in memory) isn't recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="9d093-124">Przykład wyświetla dane z modelu powiązanego z widokiem, a usługa została wprowadzona do widoku:</span><span class="sxs-lookup"><span data-stu-id="9d093-124">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![Aby wyświetlić listę wszystkich elementów, elementów zakończonych, średni priorytet i listę zadań z poziomami priorytetów i wartościami logicznymi wskazującymi na zakończenie.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="9d093-126">Wypełnianie danych wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="9d093-126">Populating Lookup Data</span></span>

<span data-ttu-id="9d093-127">Iniekcja widoku może być przydatna do wypełniania opcji elementów interfejsu użytkownika, takich jak listy rozwijane.</span><span class="sxs-lookup"><span data-stu-id="9d093-127">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="9d093-128">Rozważ użycie formularza profilu użytkownika, który zawiera opcje określania płci, stanu i innych preferencji.</span><span class="sxs-lookup"><span data-stu-id="9d093-128">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="9d093-129">Renderowanie takiego formularza przy użyciu standardowego podejścia MVC wymagało, aby kontroler zażądał usług dostępu do danych dla każdego z tych zestawów opcji, a następnie wypełnia model lub `ViewBag` przy użyciu każdego zestawu opcji, które mają być powiązane.</span><span class="sxs-lookup"><span data-stu-id="9d093-129">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="9d093-130">Alternatywna metoda powoduje wstrzyknięcie usług bezpośrednio do widoku w celu uzyskania opcji.</span><span class="sxs-lookup"><span data-stu-id="9d093-130">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="9d093-131">Pozwala to zminimalizować ilość kodu wymaganego przez kontroler, przenosząc tę logikę konstrukcyjną tego elementu widoku do samego widoku.</span><span class="sxs-lookup"><span data-stu-id="9d093-131">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="9d093-132">Akcja kontrolera, aby wyświetlić formularz edycji profilu, musi jedynie przekazać formularz wystąpienia profilu:</span><span class="sxs-lookup"><span data-stu-id="9d093-132">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

<span data-ttu-id="9d093-133">Formularz HTML służący do aktualizowania tych preferencji zawiera listę rozwijaną dla trzech właściwości:</span><span class="sxs-lookup"><span data-stu-id="9d093-133">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![Aktualizacja widoku profilu z formularzem umożliwiającym wprowadzanie nazwy, płci, stanu i koloru ulubionego.](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="9d093-135">Te listy są wypełniane przez usługę, która została wprowadzona do widoku:</span><span class="sxs-lookup"><span data-stu-id="9d093-135">These lists are populated by a service that has been injected into the view:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

<span data-ttu-id="9d093-136">`ProfileOptionsService` to usługa poziomu interfejsu użytkownika zaprojektowana w celu zapewnienia tylko danych wymaganych dla tego formularza:</span><span class="sxs-lookup"><span data-stu-id="9d093-136">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

> [!IMPORTANT]
> <span data-ttu-id="9d093-137">Nie zapomnij zarejestrować typów, które zażądasz poprzez iniekcję zależności w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9d093-137">Don't forget to register types you request through dependency injection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9d093-138">Wyrejestrowanie typu zgłasza wyjątek w czasie wykonywania, ponieważ dostawca usług jest wewnętrznie zapytań za pośrednictwem [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice).</span><span class="sxs-lookup"><span data-stu-id="9d093-138">An unregistered type throws an exception at runtime because the service provider is internally queried via [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice).</span></span>

## <a name="overriding-services"></a><span data-ttu-id="9d093-139">Zastępowanie usług</span><span class="sxs-lookup"><span data-stu-id="9d093-139">Overriding Services</span></span>

<span data-ttu-id="9d093-140">Oprócz wstrzykiwania nowych usług ta technika może być również używana do przesłonięcia wcześniej wprowadzonych usług na stronie.</span><span class="sxs-lookup"><span data-stu-id="9d093-140">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="9d093-141">Na poniższej ilustracji przedstawiono wszystkie pola dostępne na stronie, które są używane w pierwszym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="9d093-141">The figure below shows all of the fields available on the page used in the first example:</span></span>

![Menu kontekstowe IntelliSense dla wpisanego znaku @ symbol z listą pól HTML, składników, StatsService i adresów URL](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="9d093-143">Jak widać, pola domyślne obejmują `Html`, `Component`i `Url` (a także `StatsService`, które zostały dodane).</span><span class="sxs-lookup"><span data-stu-id="9d093-143">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="9d093-144">Jeśli na przykład chcesz zastąpić domyślne pomocników HTML własnymi, możesz łatwo to zrobić przy użyciu `@inject`:</span><span class="sxs-lookup"><span data-stu-id="9d093-144">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

<span data-ttu-id="9d093-145">Jeśli chcesz rozłożyć istniejące usługi, możesz po prostu użyć tej techniki podczas dziedziczenia lub otaczania istniejącej implementacji własnymi.</span><span class="sxs-lookup"><span data-stu-id="9d093-145">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="9d093-146">Zobacz też</span><span class="sxs-lookup"><span data-stu-id="9d093-146">See Also</span></span>

* <span data-ttu-id="9d093-147">Blog Simon Timms: [pobieranie danych wyszukiwania do widoku](https://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span><span class="sxs-lookup"><span data-stu-id="9d093-147">Simon Timms Blog: [Getting Lookup Data Into Your View](https://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>
