---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
title: Opis filtry akcji (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: "Celem tego samouczka jest wyjaśnienie filtrów akcji. Filtr akcji jest atrybut, który można zastosować do akcji kontrolera — lub całego kontrolera..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: a94e4e81-40c1-47b7-8613-126a1a6cc93d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
msc.type: authoredcontent
ms.openlocfilehash: 86d5d429d9900d4c04391804598626705e6c88b4
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/11/2018
---
<a name="understanding-action-filters-c"></a><span data-ttu-id="43226-104">Opis filtry akcji (C#)</span><span class="sxs-lookup"><span data-stu-id="43226-104">Understanding Action Filters (C#)</span></span>
====================
<span data-ttu-id="43226-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="43226-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="43226-106">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="43226-106">Download PDF</span></span>](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_CS.pdf)

> <span data-ttu-id="43226-107">Celem tego samouczka jest wyjaśnienie filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="43226-107">The goal of this tutorial is to explain action filters.</span></span> <span data-ttu-id="43226-108">Filtr akcji jest atrybut, który można zastosować do akcji kontrolera — lub całego kontrolera — które modyfikuje sposób, w którym akcja jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="43226-108">An action filter is an attribute that you can apply to a controller action -- or an entire controller -- that modifies the way in which the action is executed.</span></span>


## <a name="understanding-action-filters"></a><span data-ttu-id="43226-109">Opis filtry akcji</span><span class="sxs-lookup"><span data-stu-id="43226-109">Understanding Action Filters</span></span>

<span data-ttu-id="43226-110">Celem tego samouczka jest wyjaśnienie filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="43226-110">The goal of this tutorial is to explain action filters.</span></span> <span data-ttu-id="43226-111">Filtr akcji jest atrybut, który można zastosować do akcji kontrolera — lub całego kontrolera — które modyfikuje sposób, w którym akcja jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="43226-111">An action filter is an attribute that you can apply to a controller action -- or an entire controller -- that modifies the way in which the action is executed.</span></span> <span data-ttu-id="43226-112">Platforma ASP.NET MVC zawiera kilka filtrów akcji:</span><span class="sxs-lookup"><span data-stu-id="43226-112">The ASP.NET MVC framework includes several action filters:</span></span>

- <span data-ttu-id="43226-113">OutputCache — ten filtr akcji przechowuje w pamięci podręcznej danych wyjściowych akcji kontrolera dla określonego przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="43226-113">OutputCache – This action filter caches the output of a controller action for a specified amount of time.</span></span>
- <span data-ttu-id="43226-114">HandleError — ten filtr akcji obsługuje błędy wywoływane, gdy wykonuje akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="43226-114">HandleError – This action filter handles errors raised when a controller action executes.</span></span>
- <span data-ttu-id="43226-115">Autoryzowanie — ten filtr akcji pozwala ograniczyć dostęp do określonego użytkownika lub roli.</span><span class="sxs-lookup"><span data-stu-id="43226-115">Authorize – This action filter enables you to restrict access to a particular user or role.</span></span>

<span data-ttu-id="43226-116">Można również tworzyć filtry akcji niestandardowej.</span><span class="sxs-lookup"><span data-stu-id="43226-116">You also can create your own custom action filters.</span></span> <span data-ttu-id="43226-117">Na przykład można utworzyć filtr akcji niestandardowych w celu wdrożenia systemu uwierzytelniania niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="43226-117">For example, you might want to create a custom action filter in order to implement a custom authentication system.</span></span> <span data-ttu-id="43226-118">Można też utworzyć filtr akcji, który modyfikuje widok danych zwróconych przez akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="43226-118">Or, you might want to create an action filter that modifies the view data returned by a controller action.</span></span>

<span data-ttu-id="43226-119">W tym samouczku Dowiedz się jak tworzenie filtr akcji od podstaw.</span><span class="sxs-lookup"><span data-stu-id="43226-119">In this tutorial, you learn how to build an action filter from the ground up.</span></span> <span data-ttu-id="43226-120">Utworzymy filtru akcji dziennika, który rejestruje różne etapy przetwarzania akcji w oknie programu Visual Studio danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="43226-120">We create a Log action filter that logs different stages of the processing of an action to the Visual Studio Output window.</span></span>

### <a name="using-an-action-filter"></a><span data-ttu-id="43226-121">Przy użyciu filtru akcji</span><span class="sxs-lookup"><span data-stu-id="43226-121">Using an Action Filter</span></span>

<span data-ttu-id="43226-122">Filtr akcji jest atrybutem.</span><span class="sxs-lookup"><span data-stu-id="43226-122">An action filter is an attribute.</span></span> <span data-ttu-id="43226-123">Większość filtrów akcji można zastosować do akcji kontrolera poszczególnych lub całego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="43226-123">You can apply most action filters to either an individual controller action or an entire controller.</span></span>

<span data-ttu-id="43226-124">Na przykład kontroler danych w 1 lista przedstawia akcji o nazwie `Index()` zwracającą bieżącego czasu.</span><span class="sxs-lookup"><span data-stu-id="43226-124">For example, the Data controller in Listing 1 exposes an action named `Index()` that returns the current time.</span></span> <span data-ttu-id="43226-125">Ta akcja zostanie nadany `OutputCache` filtru akcji.</span><span class="sxs-lookup"><span data-stu-id="43226-125">This action is decorated with the `OutputCache` action filter.</span></span> <span data-ttu-id="43226-126">Ten filtr powoduje, że wartość zwracana przez akcję pamięci podręcznej przez 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="43226-126">This filter causes the value returned by the action to be cached for 10 seconds.</span></span>

<span data-ttu-id="43226-127">**1 — Lista`Controllers\DataController.cs`**</span><span class="sxs-lookup"><span data-stu-id="43226-127">**Listing 1 – `Controllers\DataController.cs`**</span></span>

[!code-csharp[Main](understanding-action-filters-cs/samples/sample1.cs)]

<span data-ttu-id="43226-128">Jeśli wielokrotnie wywoływać `Index()` akcji, wprowadzając adres URL/danych/indeksem w pasku adresu przeglądarki i naciśnięcie klawisza odświeżania przycisk wiele razy, a następnie pojawi się jednocześnie na 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="43226-128">If you repeatedly invoke the `Index()` action by entering the URL /Data/Index into the address bar of your browser and hitting the Refresh button multiple times, then you will see the same time for 10 seconds.</span></span> <span data-ttu-id="43226-129">Dane wyjściowe `Index()` akcji jest buforowana przez 10 sekund (zobacz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="43226-129">The output of the `Index()` action is cached for 10 seconds (see Figure 1).</span></span>


<span data-ttu-id="43226-130">[![Czas pamięci podręcznej](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="43226-130">[![Cached time](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)</span></span>

<span data-ttu-id="43226-131">**Rysunek 01**: buforowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-action-filters-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="43226-131">**Figure 01**: Cached time ([Click to view full-size image](understanding-action-filters-cs/_static/image3.png))</span></span>


<span data-ttu-id="43226-132">Wyświetlanie 1 filtru jednej akcji — `OutputCache` filtr akcji — jest stosowany do `Index()` metody.</span><span class="sxs-lookup"><span data-stu-id="43226-132">In Listing 1, a single action filter – the `OutputCache` action filter – is applied to the `Index()` method.</span></span> <span data-ttu-id="43226-133">Należy do tego samego działania można zastosować wiele filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="43226-133">If you need, you can apply multiple action filters to the same action.</span></span> <span data-ttu-id="43226-134">Na przykład możesz chcieć zastosowanie zarówno `OutputCache` i `HandleError` filtry akcji do tego samego działania.</span><span class="sxs-lookup"><span data-stu-id="43226-134">For example, you might want to apply both the `OutputCache` and `HandleError` action filters to the same action.</span></span>

<span data-ttu-id="43226-135">Wyświetlanie listy 1 `OutputCache` filtr akcji jest stosowany do `Index()` akcji.</span><span class="sxs-lookup"><span data-stu-id="43226-135">In Listing 1, the `OutputCache` action filter is applied to the `Index()` action.</span></span> <span data-ttu-id="43226-136">Można także zastosować dla tego atrybutu `DataController` samej klasy.</span><span class="sxs-lookup"><span data-stu-id="43226-136">You also could apply this attribute to the `DataController` class itself.</span></span> <span data-ttu-id="43226-137">W takim przypadku wynik zwracany przez działanie udostępnianych przez kontroler będzie buforowana przez 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="43226-137">In that case, the result returned by any action exposed by the controller would be cached for 10 seconds.</span></span>

### <a name="the-different-types-of-filters"></a><span data-ttu-id="43226-138">Różne typy filtrów</span><span class="sxs-lookup"><span data-stu-id="43226-138">The Different Types of Filters</span></span>

<span data-ttu-id="43226-139">Platforma ASP.NET MVC obsługuje cztery różne typy filtrów:</span><span class="sxs-lookup"><span data-stu-id="43226-139">The ASP.NET MVC framework supports four different types of filters:</span></span>

1. <span data-ttu-id="43226-140">Filtry autoryzacji — implementuje `IAuthorizationFilter` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="43226-140">Authorization filters – Implements the `IAuthorizationFilter` attribute.</span></span>
2. <span data-ttu-id="43226-141">Filtry działań — implementuje `IActionFilter` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="43226-141">Action filters – Implements the `IActionFilter` attribute.</span></span>
3. <span data-ttu-id="43226-142">Powoduje filtry — implementuje `IResultFilter` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="43226-142">Result filters – Implements the `IResultFilter` attribute.</span></span>
4. <span data-ttu-id="43226-143">Filtry wyjątków — implementuje `IExceptionFilter` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="43226-143">Exception filters – Implements the `IExceptionFilter` attribute.</span></span>

<span data-ttu-id="43226-144">Filtry są uruchamiane w kolejności podanej powyżej.</span><span class="sxs-lookup"><span data-stu-id="43226-144">Filters are executed in the order listed above.</span></span> <span data-ttu-id="43226-145">Na przykład filtry autoryzacji są zawsze wykonywane przed filtry akcji, a filtry wyjątków są zawsze wykonywane po każdego typu filtru.</span><span class="sxs-lookup"><span data-stu-id="43226-145">For example, authorization filters are always executed before action filters and exception filters are always executed after every other type of filter.</span></span>

<span data-ttu-id="43226-146">Filtry autoryzacji są używane do implementowania uwierzytelniania i autoryzacji dla akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="43226-146">Authorization filters are used to implement authentication and authorization for controller actions.</span></span> <span data-ttu-id="43226-147">Na przykład filtr autoryzacji jest przykładem filtr autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="43226-147">For example, the Authorize filter is an example of an Authorization filter.</span></span>

<span data-ttu-id="43226-148">Filtry akcji zawiera logikę, która jest wykonywana przed i po wykonaniu akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="43226-148">Action filters contain logic that is executed before and after a controller action executes.</span></span> <span data-ttu-id="43226-149">Filtr akcji można użyć na przykład, aby zmodyfikować wyświetlić dane, które zwraca akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="43226-149">You can use an action filter, for instance, to modify the view data that a controller action returns.</span></span>

<span data-ttu-id="43226-150">Filtry wyników zawiera logikę, która jest wykonywana przed i po wykonaniu wyniku widoku.</span><span class="sxs-lookup"><span data-stu-id="43226-150">Result filters contain logic that is executed before and after a view result is executed.</span></span> <span data-ttu-id="43226-151">Na przykład można zmodyfikować wyniku widoku kliknij prawym przyciskiem myszy przed wyświetleniem widok w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="43226-151">For example, you might want to modify a view result right before the view is rendered to the browser.</span></span>

<span data-ttu-id="43226-152">Filtry wyjątków są ostatniego typu filtru.</span><span class="sxs-lookup"><span data-stu-id="43226-152">Exception filters are the last type of filter to run.</span></span> <span data-ttu-id="43226-153">Do obsługi błędów zgłaszane przez akcji kontrolera lub wyników akcji kontrolera, można użyć filtru wyjątków.</span><span class="sxs-lookup"><span data-stu-id="43226-153">You can use an exception filter to handle errors raised by either your controller actions or controller action results.</span></span> <span data-ttu-id="43226-154">Możesz również użyć filtry wyjątków do dziennika błędów.</span><span class="sxs-lookup"><span data-stu-id="43226-154">You also can use exception filters to log errors.</span></span>

<span data-ttu-id="43226-155">Każdy typ filtru jest wykonywany w określonej kolejności.</span><span class="sxs-lookup"><span data-stu-id="43226-155">Each different type of filter is executed in a particular order.</span></span> <span data-ttu-id="43226-156">Jeśli chcesz kontrolować kolejność wykonywania filtrów tego samego typu można ustawić właściwości kolejności filtrów.</span><span class="sxs-lookup"><span data-stu-id="43226-156">If you want to control the order in which filters of the same type are executed then you can set a filter's Order property.</span></span>

<span data-ttu-id="43226-157">Klasa podstawowa dla wszystkich filtrów akcji jest `System.Web.Mvc.FilterAttribute` klasy.</span><span class="sxs-lookup"><span data-stu-id="43226-157">The base class for all action filters is the `System.Web.Mvc.FilterAttribute` class.</span></span> <span data-ttu-id="43226-158">Jeśli chcesz zaimplementować określonego typu filtru, a następnie należy utworzyć klasę, która dziedziczy po klasie podstawowej filtru i implementuje co najmniej jeden `IAuthorizationFilter`, `IActionFilter`, `IResultFilter`, lub `IExceptionFilter` interfejsów.</span><span class="sxs-lookup"><span data-stu-id="43226-158">If you want to implement a particular type of filter, then you need to create a class that inherits from the base Filter class and implements one or more of the `IAuthorizationFilter`, `IActionFilter`, `IResultFilter`, or `IExceptionFilter` interfaces.</span></span>

### <a name="the-base-actionfilterattribute-class"></a><span data-ttu-id="43226-159">Klasa podstawowa ActionFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="43226-159">The Base ActionFilterAttribute Class</span></span>

<span data-ttu-id="43226-160">Aby ułatwić implementuje filtr akcji niestandardowej, platforma ASP.NET MVC zawiera podstawowej `ActionFilterAttribute` klasy.</span><span class="sxs-lookup"><span data-stu-id="43226-160">In order to make it easier for you to implement a custom action filter, the ASP.NET MVC framework includes a base `ActionFilterAttribute` class.</span></span> <span data-ttu-id="43226-161">Ta klasa implementuje zarówno `IActionFilter` i `IResultFilter` i interfejsy dziedziczy `Filter` klasy.</span><span class="sxs-lookup"><span data-stu-id="43226-161">This class implements both the `IActionFilter` and `IResultFilter` interfaces and inherits from the `Filter` class.</span></span>

<span data-ttu-id="43226-162">Terminologia w tym miejscu nie jest całkowicie zgodne.</span><span class="sxs-lookup"><span data-stu-id="43226-162">The terminology here is not entirely consistent.</span></span> <span data-ttu-id="43226-163">Z technicznego punktu widzenia klasy, która dziedziczy po klasie ActionFilterAttribute jest filtr akcji i filtrowania wyników.</span><span class="sxs-lookup"><span data-stu-id="43226-163">Technically, a class that inherits from the ActionFilterAttribute class is both an action filter and a result filter.</span></span> <span data-ttu-id="43226-164">Jednak w tym sensie, utracić, filtr akcji programu word jest używana do odwoływania się do dowolnego typu filtru w platformę ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="43226-164">However, in the loose sense, the word action filter is used to refer to any type of filter in the ASP.NET MVC framework.</span></span>

<span data-ttu-id="43226-165">Podstawowym `ActionFilterAttribute` klasa ma następujące metody, które można zastąpić:</span><span class="sxs-lookup"><span data-stu-id="43226-165">The base `ActionFilterAttribute` class has the following methods that you can override:</span></span>

- <span data-ttu-id="43226-166">OnActionExecuting — ta metoda jest wywoływana przed wykonaniem akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="43226-166">OnActionExecuting – This method is called before a controller action is executed.</span></span>
- <span data-ttu-id="43226-167">OnActionExecuted — ta metoda jest wywoływana po wykonaniu akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="43226-167">OnActionExecuted – This method is called after a controller action is executed.</span></span>
- <span data-ttu-id="43226-168">OnResultExecuting — ta metoda jest wywoływana przed wykonaniem wyniku akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="43226-168">OnResultExecuting – This method is called before a controller action result is executed.</span></span>
- <span data-ttu-id="43226-169">OnResultExecuted — ta metoda jest wywoływana po wykonaniu wyniku akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="43226-169">OnResultExecuted – This method is called after a controller action result is executed.</span></span>

<span data-ttu-id="43226-170">W następnej sekcji przedstawiono będzie implementacji każdej z tych różnych metod.</span><span class="sxs-lookup"><span data-stu-id="43226-170">In the next section, we'll see how you can implement each of these different methods.</span></span>

### <a name="creating-a-log-action-filter"></a><span data-ttu-id="43226-171">Utworzenie filtru akcji dziennika</span><span class="sxs-lookup"><span data-stu-id="43226-171">Creating a Log Action Filter</span></span>

<span data-ttu-id="43226-172">Aby zilustrować, jak można utworzyć filtr akcji niestandardowej, utworzymy filtr akcji niestandardowej, który rejestruje etapów przetwarzania akcji kontrolera, w oknie programu Visual Studio danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="43226-172">In order to illustrate how you can build a custom action filter, we'll create a custom action filter that logs the stages of processing a controller action to the Visual Studio Output window.</span></span> <span data-ttu-id="43226-173">Nasze `LogActionFilter` znajduje się lista 2.</span><span class="sxs-lookup"><span data-stu-id="43226-173">Our `LogActionFilter` is contained in Listing 2.</span></span>

<span data-ttu-id="43226-174">**2 — Lista`ActionFilters\LogActionFilter.cs`**</span><span class="sxs-lookup"><span data-stu-id="43226-174">**Listing 2 – `ActionFilters\LogActionFilter.cs`**</span></span>

[!code-csharp[Main](understanding-action-filters-cs/samples/sample2.cs)]

<span data-ttu-id="43226-175">Wyświetlanie 2 `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, i `OnResultExecuted()` wywołania metody `Log()` metody.</span><span class="sxs-lookup"><span data-stu-id="43226-175">In Listing 2, the `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, and `OnResultExecuted()` methods all call the `Log()` method.</span></span> <span data-ttu-id="43226-176">Nazwa metody i bieżące dane trasy są przekazywane do `Log()` metody.</span><span class="sxs-lookup"><span data-stu-id="43226-176">The name of the method and the current route data is passed to the `Log()` method.</span></span> <span data-ttu-id="43226-177">`Log()` Metody zapisuje komunikat w oknie programu Visual Studio danych wyjściowych (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="43226-177">The `Log()` method writes a message to the Visual Studio Output window (see Figure 2).</span></span>


<span data-ttu-id="43226-178">[![Zapisywanie w oknie programu Visual Studio danych wyjściowych.](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="43226-178">[![Writing to the Visual Studio Output window](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)</span></span>

<span data-ttu-id="43226-179">**Rysunek 02**: zapisywanie w oknie programu Visual Studio danych wyjściowych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-action-filters-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="43226-179">**Figure 02**: Writing to the Visual Studio Output window ([Click to view full-size image](understanding-action-filters-cs/_static/image6.png))</span></span>


<span data-ttu-id="43226-180">Kontrolera głównej w wyświetlania 3 przedstawiono, jak filtr akcji dziennika można stosować do klasy całego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="43226-180">The Home controller in Listing 3 illustrates how you can apply the Log action filter to an entire controller class.</span></span> <span data-ttu-id="43226-181">Zawsze, gdy wszystkie akcje udostępnianych przez kontrolera głównej są wywoływane — albo `Index()` metody lub `About()` metodą — etapy przetwarzania akcji są rejestrowane w oknie programu Visual Studio danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="43226-181">Whenever any of the actions exposed by the Home controller are invoked – either the `Index()` method or the `About()` method – the stages of processing the action are logged to the Visual Studio Output window.</span></span>

<span data-ttu-id="43226-182">**3 — lista`Controllers\HomeController.cs`**</span><span class="sxs-lookup"><span data-stu-id="43226-182">**Listing 3 – `Controllers\HomeController.cs`**</span></span>

[!code-csharp[Main](understanding-action-filters-cs/samples/sample3.cs)]

### <a name="summary"></a><span data-ttu-id="43226-183">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="43226-183">Summary</span></span>

<span data-ttu-id="43226-184">W tym samouczku zostały wprowadzone do platformy ASP.NET MVC filtry akcji.</span><span class="sxs-lookup"><span data-stu-id="43226-184">In this tutorial, you were introduced to ASP.NET MVC action filters.</span></span> <span data-ttu-id="43226-185">Przedstawiono cztery różne typy filtrów: filtry autoryzacji, filtry akcji, filtry wyników i filtry wyjątków.</span><span class="sxs-lookup"><span data-stu-id="43226-185">You learned about the four different types of filters: authorization filters, action filters, result filters, and exception filters.</span></span> <span data-ttu-id="43226-186">Przedstawiono również o podstawowym `ActionFilterAttribute` klasy.</span><span class="sxs-lookup"><span data-stu-id="43226-186">You also learned about the base `ActionFilterAttribute` class.</span></span>

<span data-ttu-id="43226-187">Ponadto przedstawiono sposób wykonania prostego filtru akcji.</span><span class="sxs-lookup"><span data-stu-id="43226-187">Finally, you learned how to implement a simple action filter.</span></span> <span data-ttu-id="43226-188">Utworzyliśmy filtru akcji dziennika, który rejestruje etapów przetwarzania akcji kontrolera, w oknie programu Visual Studio danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="43226-188">We created a Log action filter that logs the stages of processing a controller action to the Visual Studio Output window.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="43226-189">[Poprzednie](asp-net-mvc-routing-overview-cs.md)
[dalej](improving-performance-with-output-caching-cs.md)</span><span class="sxs-lookup"><span data-stu-id="43226-189">[Previous](asp-net-mvc-routing-overview-cs.md)
[Next](improving-performance-with-output-caching-cs.md)</span></span>
