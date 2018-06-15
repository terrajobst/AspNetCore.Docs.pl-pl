---
title: Iniekcji zależności do widoków w ASP.NET Core
author: ardalis
description: Dowiedz się, jak platformy ASP.NET Core obsługuje iniekcji zależności do widoków MVC.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/dependency-injection
ms.openlocfilehash: d33f0253fc7c1329e8bab400ace4c4ce8d10d792
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/14/2018
ms.locfileid: "35613063"
---
# <a name="dependency-injection-into-views-in-aspnet-core"></a><span data-ttu-id="0e774-103">Iniekcji zależności do widoków w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0e774-103">Dependency injection into views in ASP.NET Core</span></span>

<span data-ttu-id="0e774-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="0e774-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="0e774-105">Obsługuje platformy ASP.NET Core [iniekcji zależności](xref:fundamentals/dependency-injection) do widoków.</span><span class="sxs-lookup"><span data-stu-id="0e774-105">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="0e774-106">Może to być przydatne w przypadku usług specyficzne dla widoku, takich jak lokalizacja lub wymagane tylko w przypadku wypełnianie elementy widoku danych.</span><span class="sxs-lookup"><span data-stu-id="0e774-106">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="0e774-107">Staraj się zachować [separacji](http://deviq.com/separation-of-concerns/) między widoków i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="0e774-107">You should try to maintain [separation of concerns](http://deviq.com/separation-of-concerns/) between your controllers and views.</span></span> <span data-ttu-id="0e774-108">Większość danych, które widoków wyświetlić powinien zostać przekazany w z kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0e774-108">Most of the data your views display should be passed in from the controller.</span></span>

<span data-ttu-id="0e774-109">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0e774-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="a-simple-example"></a><span data-ttu-id="0e774-110">Prosty przykład</span><span class="sxs-lookup"><span data-stu-id="0e774-110">A Simple Example</span></span>

<span data-ttu-id="0e774-111">Usługi można wstawić do widoku przy użyciu `@inject` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="0e774-111">You can inject a service into a view using the `@inject` directive.</span></span> <span data-ttu-id="0e774-112">Można potraktować `@inject` jako Dodawanie właściwości do widoku i wypełniania właściwości, używając Podpisane.</span><span class="sxs-lookup"><span data-stu-id="0e774-112">You can think of `@inject` as adding a property to your view, and populating the property using DI.</span></span>

<span data-ttu-id="0e774-113">Składnia `@inject`: `@inject <type> <name>`</span><span class="sxs-lookup"><span data-stu-id="0e774-113">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="0e774-114">Przykład `@inject` w akcji:</span><span class="sxs-lookup"><span data-stu-id="0e774-114">An example of `@inject` in action:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

<span data-ttu-id="0e774-115">Ten widok przedstawia listę `ToDoItem` wystąpień, wraz z podsumowaniem przedstawiający ogólne statystyki.</span><span class="sxs-lookup"><span data-stu-id="0e774-115">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="0e774-116">Podsumowanie jest wypełniana z wprowadzonym `StatisticsService`.</span><span class="sxs-lookup"><span data-stu-id="0e774-116">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="0e774-117">Ta usługa jest zarejestrowana iniekcji zależności w `ConfigureServices` w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0e774-117">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

<span data-ttu-id="0e774-118">`StatisticsService` Wykonuje obliczenia na zbiór `ToDoItem` wystąpienia, które uzyskuje dostęp do za pośrednictwem repozytorium:</span><span class="sxs-lookup"><span data-stu-id="0e774-118">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,25)]

<span data-ttu-id="0e774-119">Repozytorium przykładowej korzysta z kolekcji w pamięci.</span><span class="sxs-lookup"><span data-stu-id="0e774-119">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="0e774-120">Implementacja pokazanym powyżej (który działa na wszystkich danych w pamięci) nie jest zalecane w przypadku dużych, zdalny dostęp do zestawów danych.</span><span class="sxs-lookup"><span data-stu-id="0e774-120">The implementation shown above (which operates on all of the data in memory) isn't recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="0e774-121">Próbki są wyświetlane dane z modelu, powiązany z widoku i usługi do widoku:</span><span class="sxs-lookup"><span data-stu-id="0e774-121">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![Aby wyświetlić listę całkowita liczba elementów ukończone elementy, średni priorytet i Lista zadań z ich poziomy priorytetu i wskazujący ukończenia wartości logiczne.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="0e774-123">Wypełnianie wyszukiwanie danych</span><span class="sxs-lookup"><span data-stu-id="0e774-123">Populating Lookup Data</span></span>

<span data-ttu-id="0e774-124">Widok iniekcji mogą być przydatne do wypełniania opcji w elementów interfejsu użytkownika, takich jak list rozwijanych.</span><span class="sxs-lookup"><span data-stu-id="0e774-124">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="0e774-125">Należy wziąć pod uwagę formularz profilu użytkownika, która obejmuje opcje określenie płci, stanu i inne preferencje.</span><span class="sxs-lookup"><span data-stu-id="0e774-125">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="0e774-126">Renderowanie formularza przy użyciu standardowej metody MVC wymaga kontrolera w celu żądania usługi dostępu do danych dla każdego z tych zestawów opcje, a następnie wypełnij modelu lub `ViewBag` każdy zestaw opcji może być powiązane.</span><span class="sxs-lookup"><span data-stu-id="0e774-126">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="0e774-127">Informacje o innym podejściu injects usług bezpośrednio w widoku, aby uzyskać odpowiednie opcje.</span><span class="sxs-lookup"><span data-stu-id="0e774-127">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="0e774-128">Pozwala to zmniejszyć ilość kodu wymagane przez administratora, przeniesienie tego widoku elementu konstrukcji logiki do samego widoku.</span><span class="sxs-lookup"><span data-stu-id="0e774-128">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="0e774-129">Akcji kontrolera do wyświetlania formularza edycji profilu musi tylko Przekaż wystąpienie profilu formularza:</span><span class="sxs-lookup"><span data-stu-id="0e774-129">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

<span data-ttu-id="0e774-130">Formularz HTML używane do aktualizowania tych preferencji zawiera list rozwijanych trzy właściwości:</span><span class="sxs-lookup"><span data-stu-id="0e774-130">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![Aktualizowanie widoku profilu dla formularza, umożliwiając wpis nazwy, płci, stanu i kolor ulubionych.](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="0e774-132">Te listy są wypełniane przez usługę, które zostały dodane do widoku:</span><span class="sxs-lookup"><span data-stu-id="0e774-132">These lists are populated by a service that has been injected into the view:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

<span data-ttu-id="0e774-133">`ProfileOptionsService` Jest usługą interfejsu użytkownika na poziomie zapewnia tylko te dane, które są wymagane dla tego formularza:</span><span class="sxs-lookup"><span data-stu-id="0e774-133">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

> [!IMPORTANT]
> <span data-ttu-id="0e774-134">Nie zapomnij zarejestrować typy żądań za pomocą iniekcji zależności w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0e774-134">Don't forget to register types you request through dependency injection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0e774-135">Typem wyrejestrować zgłasza wyjątek w czasie wykonywania, ponieważ dostawca usług wewnętrznie wysyłane jest [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice).</span><span class="sxs-lookup"><span data-stu-id="0e774-135">An unregistered type throws an exception at runtime because the service provider is internally queried via [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice).</span></span>

## <a name="overriding-services"></a><span data-ttu-id="0e774-136">Zastępowanie usług</span><span class="sxs-lookup"><span data-stu-id="0e774-136">Overriding Services</span></span>

<span data-ttu-id="0e774-137">Oprócz wstrzyknięcie nowych usług, ta metoda mogą służyć do zastępowania poprzednio wprowadzony usług na stronie.</span><span class="sxs-lookup"><span data-stu-id="0e774-137">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="0e774-138">Na poniższej ilustracji przedstawiono wszystkie pola, które są dostępne na stronie używany w pierwszym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="0e774-138">The figure below shows all of the fields available on the page used in the first example:</span></span>

![Menu kontekstowe IntelliSense na typu wyświetlania pola Html, składnik StatsService i adres Url symbol @](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="0e774-140">Jak widać, domyślne pola zawierają `Html`, `Component`, i `Url` (a także `StatsService` który możemy wprowadzić).</span><span class="sxs-lookup"><span data-stu-id="0e774-140">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="0e774-141">Jeśli na przykład chcesz zastąpić domyślne pomocników HTML własnym, możesz łatwo to zrobić przy użyciu `@inject`:</span><span class="sxs-lookup"><span data-stu-id="0e774-141">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

<span data-ttu-id="0e774-142">Jeśli chcesz rozszerzyć istniejących usług, po prostu służy ta technika podczas dziedziczenia z lub zawijania istniejącej implementacji własnymi.</span><span class="sxs-lookup"><span data-stu-id="0e774-142">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="0e774-143">Zobacz też</span><span class="sxs-lookup"><span data-stu-id="0e774-143">See Also</span></span>

* <span data-ttu-id="0e774-144">Blog Timms Simona: [pobierania danych odnośników do widoku](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span><span class="sxs-lookup"><span data-stu-id="0e774-144">Simon Timms Blog: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>
