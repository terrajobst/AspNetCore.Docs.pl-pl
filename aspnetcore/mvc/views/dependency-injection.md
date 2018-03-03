---
title: "Iniekcji zależności do widoków"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/dependency-injection
ms.openlocfilehash: e5703bd5b3b2340b4ef170a81550237ffe2c3e3d
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="dependency-injection-into-views"></a><span data-ttu-id="b7be7-102">Iniekcji zależności do widoków</span><span class="sxs-lookup"><span data-stu-id="b7be7-102">Dependency injection into views</span></span>

<span data-ttu-id="b7be7-103">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="b7be7-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="b7be7-104">Obsługuje platformy ASP.NET Core [iniekcji zależności](xref:fundamentals/dependency-injection) do widoków.</span><span class="sxs-lookup"><span data-stu-id="b7be7-104">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="b7be7-105">Może to być przydatne w przypadku usług specyficzne dla widoku, takich jak lokalizacja lub wymagane tylko w przypadku wypełnianie elementy widoku danych.</span><span class="sxs-lookup"><span data-stu-id="b7be7-105">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="b7be7-106">Staraj się zachować [separacji](http://deviq.com/separation-of-concerns/) między widoków i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="b7be7-106">You should try to maintain [separation of concerns](http://deviq.com/separation-of-concerns/) between your controllers and views.</span></span> <span data-ttu-id="b7be7-107">Większość danych, które widoków wyświetlić powinien zostać przekazany w z kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b7be7-107">Most of the data your views display should be passed in from the controller.</span></span>

<span data-ttu-id="b7be7-108">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b7be7-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="a-simple-example"></a><span data-ttu-id="b7be7-109">Prosty przykład</span><span class="sxs-lookup"><span data-stu-id="b7be7-109">A Simple Example</span></span>

<span data-ttu-id="b7be7-110">Usługi można wstawić do widoku przy użyciu `@inject` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="b7be7-110">You can inject a service into a view using the `@inject` directive.</span></span> <span data-ttu-id="b7be7-111">Można potraktować `@inject` jako Dodawanie właściwości do widoku i wypełniania właściwości, używając Podpisane.</span><span class="sxs-lookup"><span data-stu-id="b7be7-111">You can think of `@inject` as adding a property to your view, and populating the property using DI.</span></span>

<span data-ttu-id="b7be7-112">Składnia `@inject`: `@inject <type> <name>`</span><span class="sxs-lookup"><span data-stu-id="b7be7-112">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="b7be7-113">Przykład `@inject` w akcji:</span><span class="sxs-lookup"><span data-stu-id="b7be7-113">An example of `@inject` in action:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

<span data-ttu-id="b7be7-114">Ten widok przedstawia listę `ToDoItem` wystąpień, wraz z podsumowaniem przedstawiający ogólne statystyki.</span><span class="sxs-lookup"><span data-stu-id="b7be7-114">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="b7be7-115">Podsumowanie jest wypełniana z wprowadzonym `StatisticsService`.</span><span class="sxs-lookup"><span data-stu-id="b7be7-115">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="b7be7-116">Ta usługa jest zarejestrowana iniekcji zależności w `ConfigureServices` w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b7be7-116">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

<span data-ttu-id="b7be7-117">`StatisticsService` Wykonuje obliczenia na zbiór `ToDoItem` wystąpienia, które uzyskuje dostęp do za pośrednictwem repozytorium:</span><span class="sxs-lookup"><span data-stu-id="b7be7-117">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

<span data-ttu-id="b7be7-118">Repozytorium przykładowej korzysta z kolekcji w pamięci.</span><span class="sxs-lookup"><span data-stu-id="b7be7-118">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="b7be7-119">Implementacja pokazanym powyżej (który działa na wszystkich danych w pamięci) nie jest zalecane w przypadku dużych, zdalny dostęp do zestawów danych.</span><span class="sxs-lookup"><span data-stu-id="b7be7-119">The implementation shown above (which operates on all of the data in memory) isn't recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="b7be7-120">Próbki są wyświetlane dane z modelu, powiązany z widoku i usługi do widoku:</span><span class="sxs-lookup"><span data-stu-id="b7be7-120">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![Aby wyświetlić listę całkowita liczba elementów ukończone elementy, średni priorytet i Lista zadań z ich poziomy priorytetu i wskazujący ukończenia wartości logiczne.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="b7be7-122">Wypełnianie wyszukiwanie danych</span><span class="sxs-lookup"><span data-stu-id="b7be7-122">Populating Lookup Data</span></span>

<span data-ttu-id="b7be7-123">Widok iniekcji mogą być przydatne do wypełniania opcji w elementów interfejsu użytkownika, takich jak list rozwijanych.</span><span class="sxs-lookup"><span data-stu-id="b7be7-123">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="b7be7-124">Należy wziąć pod uwagę formularz profilu użytkownika, która obejmuje opcje określenie płci, stanu i inne preferencje.</span><span class="sxs-lookup"><span data-stu-id="b7be7-124">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="b7be7-125">Renderowanie formularza przy użyciu standardowej metody MVC wymaga kontrolera w celu żądania usługi dostępu do danych dla każdego z tych zestawów opcje, a następnie wypełnij modelu lub `ViewBag` każdy zestaw opcji może być powiązane.</span><span class="sxs-lookup"><span data-stu-id="b7be7-125">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="b7be7-126">Informacje o innym podejściu injects usług bezpośrednio w widoku, aby uzyskać odpowiednie opcje.</span><span class="sxs-lookup"><span data-stu-id="b7be7-126">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="b7be7-127">Pozwala to zmniejszyć ilość kodu wymagane przez administratora, przeniesienie tego widoku elementu konstrukcji logiki do samego widoku.</span><span class="sxs-lookup"><span data-stu-id="b7be7-127">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="b7be7-128">Akcji kontrolera do wyświetlania formularza edycji profilu musi tylko Przekaż wystąpienie profilu formularza:</span><span class="sxs-lookup"><span data-stu-id="b7be7-128">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

<span data-ttu-id="b7be7-129">Formularz HTML używane do aktualizowania tych preferencji zawiera list rozwijanych trzy właściwości:</span><span class="sxs-lookup"><span data-stu-id="b7be7-129">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![Aktualizowanie widoku profilu dla formularza, umożliwiając wpis nazwy, płci, stanu i kolor ulubionych.](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="b7be7-131">Te listy są wypełniane przez usługę, które zostały dodane do widoku:</span><span class="sxs-lookup"><span data-stu-id="b7be7-131">These lists are populated by a service that has been injected into the view:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

<span data-ttu-id="b7be7-132">`ProfileOptionsService` Jest usługą interfejsu użytkownika na poziomie zapewnia tylko te dane, które są wymagane dla tego formularza:</span><span class="sxs-lookup"><span data-stu-id="b7be7-132">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> <span data-ttu-id="b7be7-133">Nie zapomnij zarejestrować typy będzie żądać za pomocą iniekcji zależności w `ConfigureServices` metody w *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="b7be7-133">Don't forget to register types you will request through dependency injection in the  `ConfigureServices` method in *Startup.cs*.</span></span>

## <a name="overriding-services"></a><span data-ttu-id="b7be7-134">Zastępowanie usług</span><span class="sxs-lookup"><span data-stu-id="b7be7-134">Overriding Services</span></span>

<span data-ttu-id="b7be7-135">Oprócz wstrzyknięcie nowych usług, ta metoda mogą służyć do zastępowania poprzednio wprowadzony usług na stronie.</span><span class="sxs-lookup"><span data-stu-id="b7be7-135">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="b7be7-136">Na poniższej ilustracji przedstawiono wszystkie pola, które są dostępne na stronie używany w pierwszym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="b7be7-136">The figure below shows all of the fields available on the page used in the first example:</span></span>

![Menu kontekstowe IntelliSense na typu wyświetlania pola Html, składnik StatsService i adres Url symbol @](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="b7be7-138">Jak widać, domyślne pola zawierają `Html`, `Component`, i `Url` (a także `StatsService` który możemy wprowadzić).</span><span class="sxs-lookup"><span data-stu-id="b7be7-138">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="b7be7-139">Jeśli na przykład chcesz zastąpić domyślne pomocników HTML własnym, możesz łatwo to zrobić przy użyciu `@inject`:</span><span class="sxs-lookup"><span data-stu-id="b7be7-139">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

<span data-ttu-id="b7be7-140">Jeśli chcesz rozszerzyć istniejących usług, po prostu służy ta technika podczas dziedziczenia z lub zawijania istniejącej implementacji własnymi.</span><span class="sxs-lookup"><span data-stu-id="b7be7-140">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="b7be7-141">Zobacz też</span><span class="sxs-lookup"><span data-stu-id="b7be7-141">See Also</span></span>

* <span data-ttu-id="b7be7-142">Blog Timms Simona: [pobierania danych odnośników do widoku](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span><span class="sxs-lookup"><span data-stu-id="b7be7-142">Simon Timms Blog: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>
