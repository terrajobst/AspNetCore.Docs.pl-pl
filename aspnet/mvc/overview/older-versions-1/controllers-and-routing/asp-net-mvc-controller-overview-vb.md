---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: Omówienie kontrolera ASP.NET MVC (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'W tym samouczku Walther Autor: Stephen przedstawiono kontrolery ASP.NET MVC. Dowiesz się, jak utworzyć nowe kontrolery i zwracać różne typy akcji res...'
ms.author: aspnetcontent
ms.date: 02/16/2008
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: ac5e9242f494b8472e582bc76a6f4805db2f770f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809226"
---
<a name="aspnet-mvc-controller-overview-vb"></a><span data-ttu-id="c7e93-104">Omówienie kontrolera ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="c7e93-104">ASP.NET MVC Controller Overview (VB)</span></span>
====================
<span data-ttu-id="c7e93-105">przez [Walther Autor: Stephen](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="c7e93-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="c7e93-106">W tym samouczku Walther Autor: Stephen przedstawiono kontrolery ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c7e93-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="c7e93-107">Dowiesz się, jak utworzyć nowe kontrolery i zwracania różnych typów wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="c7e93-107">You learn how to create new controllers and return different types of action results.</span></span>


<span data-ttu-id="c7e93-108">W tym samouczku przedstawiono temat kontrolerów, akcji kontrolera ASP.NET MVC i wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="c7e93-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="c7e93-109">Po ukończeniu tego samouczka należy zrozumieć, jak kontrolery są używane do kontrolowania sposobu, w których użytkownik wchodzi w interakcję z witryny sieci Web platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c7e93-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="c7e93-110">Opis kontrolerów</span><span class="sxs-lookup"><span data-stu-id="c7e93-110">Understanding Controllers</span></span>

<span data-ttu-id="c7e93-111">Kontrolerów MVC są odpowiedzialne za odpowiada na żądania skierowanego do witryny sieci Web platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c7e93-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="c7e93-112">Każde żądanie przeglądarki jest mapowane na określony kontroler.</span><span class="sxs-lookup"><span data-stu-id="c7e93-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="c7e93-113">Załóżmy na przykład, wprowadź następujący adres URL w pasku adresu przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="c7e93-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="c7e93-114">W tym przypadku kontroler o nazwie ProductController jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="c7e93-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="c7e93-115">ProductController jest odpowiedzialny za Generowanie odpowiedzi na żądanie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c7e93-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="c7e93-116">Na przykład kontroler może zwrócić określonego widoku, wróć do przeglądarki lub kontroler może być przekierowanie użytkownika do innego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c7e93-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="c7e93-117">Wyświetlanie listy 1 zawiera proste o nazwie ProductController kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c7e93-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="c7e93-118">**Listing1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="c7e93-118">**Listing1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

<span data-ttu-id="c7e93-119">Jak widać z zakresu od 1 do wyświetlania listy kontrolera jest po prostu klasą (Visual Basic .NET lub C#).</span><span class="sxs-lookup"><span data-stu-id="c7e93-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="c7e93-120">Kontroler jest klasą pochodzącą z klasy bazowej System.Web.Mvc.Controller.</span><span class="sxs-lookup"><span data-stu-id="c7e93-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="c7e93-121">Ponieważ kontroler dziedziczy z tej klasy bazowej, kontrolerem dziedziczy kilka użytecznych metod bezpłatnie (omawiane metody te za chwilę).</span><span class="sxs-lookup"><span data-stu-id="c7e93-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="c7e93-122">Opis akcji kontrolera</span><span class="sxs-lookup"><span data-stu-id="c7e93-122">Understanding Controller Actions</span></span>

<span data-ttu-id="c7e93-123">Kontroler udostępnia akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c7e93-123">A controller exposes controller actions.</span></span> <span data-ttu-id="c7e93-124">Akcja jest metoda na kontrolerze, która jest wywoływana po wprowadzeniu określonego adresu URL w pasku adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c7e93-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="c7e93-125">Załóżmy na przykład, przesyłania żądania do następującego adresu URL:</span><span class="sxs-lookup"><span data-stu-id="c7e93-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="c7e93-126">W takim przypadku klasa ProductController wywoływana jest metoda indeks().</span><span class="sxs-lookup"><span data-stu-id="c7e93-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="c7e93-127">Metoda indeks() jest przykładem akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c7e93-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="c7e93-128">Akcja kontrolera musi być publiczna metoda klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c7e93-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="c7e93-129">Wizualne metod języku, domyślnie są metody publiczne.</span><span class="sxs-lookup"><span data-stu-id="c7e93-129">Visual Basic.NET methods, by default, are public methods.</span></span> <span data-ttu-id="c7e93-130">Należy pamiętać, że metoda publiczna, dodawana do klasy kontrolera jest automatycznie widoczne jako akcji kontrolera (należy zachować ostrożność na ten temat ponieważ akcji kontrolera może być wywoływany przez wszystkich użytkowników w środowisku, po prostu wpisując polecenie właściwego adresu URL w pasku adresu przeglądarki).</span><span class="sxs-lookup"><span data-stu-id="c7e93-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="c7e93-131">Istnieją pewne wymagania dodatkowe, które muszą zostać spełnione przez akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c7e93-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="c7e93-132">Nie można przeciążyć metodę akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c7e93-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="c7e93-133">Ponadto akcji kontrolera nie może być metodą statyczną.</span><span class="sxs-lookup"><span data-stu-id="c7e93-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="c7e93-134">Innych niż można użyć niemal dowolnej metody akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c7e93-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="c7e93-135">Opis wyników akcji</span><span class="sxs-lookup"><span data-stu-id="c7e93-135">Understanding Action Results</span></span>

<span data-ttu-id="c7e93-136">Akcja kontrolera zwraca coś, co jest nazywane *wynik akcji*.</span><span class="sxs-lookup"><span data-stu-id="c7e93-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="c7e93-137">Wynik akcji jest akcją kontroler zwraca w odpowiedzi na żądanie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c7e93-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="c7e93-138">Platforma ASP.NET MVC obsługuje kilka typów wyników akcji, w tym:</span><span class="sxs-lookup"><span data-stu-id="c7e93-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="c7e93-139">ViewResult — HTML reprezentuje i znaczników.</span><span class="sxs-lookup"><span data-stu-id="c7e93-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="c7e93-140">EmptyResult — reprezentuje żadnego wyniku.</span><span class="sxs-lookup"><span data-stu-id="c7e93-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="c7e93-141">RedirectResult — reprezentuje przekierowanie do nowego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="c7e93-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="c7e93-142">JsonResult — reprezentuje wynik JavaScript Object Notation używanym w aplikacji interfejsu AJAX.</span><span class="sxs-lookup"><span data-stu-id="c7e93-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="c7e93-143">JavaScriptResult — reprezentuje skryptu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c7e93-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="c7e93-144">ContentResult — reprezentuje rezultat tekstu.</span><span class="sxs-lookup"><span data-stu-id="c7e93-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="c7e93-145">FileContentResult — reprezentuje plik do pobrania (z zawartości binarnej).</span><span class="sxs-lookup"><span data-stu-id="c7e93-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="c7e93-146">FilePathResult — reprezentuje plik do pobrania (przy użyciu ścieżki).</span><span class="sxs-lookup"><span data-stu-id="c7e93-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="c7e93-147">FileStreamResult — reprezentuje plik do pobrania (z strumienia pliku).</span><span class="sxs-lookup"><span data-stu-id="c7e93-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="c7e93-148">Wszystkie te wyniki akcji dziedziczą z klasy bazowej ActionResult.</span><span class="sxs-lookup"><span data-stu-id="c7e93-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="c7e93-149">W większości przypadków akcji kontrolera zwraca ViewResult.</span><span class="sxs-lookup"><span data-stu-id="c7e93-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="c7e93-150">Na przykład akcji kontrolera indeksu w ofercie 2 zwraca ViewResult.</span><span class="sxs-lookup"><span data-stu-id="c7e93-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="c7e93-151">**Wyświetlanie listy 2 - Controllers\BookController.vb**</span><span class="sxs-lookup"><span data-stu-id="c7e93-151">**Listing 2 - Controllers\BookController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

<span data-ttu-id="c7e93-152">Po powrocie z akcji ViewResult HTML jest zwracany do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c7e93-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="c7e93-153">Metoda indeks() w ofercie 2 zwraca widok o nazwie indeksu w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="c7e93-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="c7e93-154">Zwróć uwagę, czy akcja indeks() w ofercie 2 nie może zwracać ViewResult().</span><span class="sxs-lookup"><span data-stu-id="c7e93-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="c7e93-155">Zamiast tego zostanie wywołana metoda View() klasy bazowej kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c7e93-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="c7e93-156">Zazwyczaj użytkownik nie zwracają wynik akcji bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="c7e93-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="c7e93-157">Zamiast tego można wywołać jedną z następujących metod klasy bazowej kontrolera:</span><span class="sxs-lookup"><span data-stu-id="c7e93-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="c7e93-158">Wyświetl — zwraca wynik akcji ViewResult.</span><span class="sxs-lookup"><span data-stu-id="c7e93-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="c7e93-159">Przekieruj — zwraca wynik akcji RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="c7e93-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="c7e93-160">RedirectToAction — zwraca wynik akcji RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="c7e93-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="c7e93-161">RedirectToRoute — zwraca wynik akcji RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="c7e93-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="c7e93-162">JSON — zwraca wynik akcji JsonResult.</span><span class="sxs-lookup"><span data-stu-id="c7e93-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="c7e93-163">JavaScriptResult — zwraca JavaScriptResult.</span><span class="sxs-lookup"><span data-stu-id="c7e93-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="c7e93-164">Zawartości — zwraca wynik akcji ContentResult.</span><span class="sxs-lookup"><span data-stu-id="c7e93-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="c7e93-165">Plik — zwraca FileContentResult, FilePathResult lub FileStreamResult w zależności od parametrów przekazywany do metody.</span><span class="sxs-lookup"><span data-stu-id="c7e93-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="c7e93-166">Tak Jeśli chcesz przywrócić widok w przeglądarce, należy wywołać metodę View().</span><span class="sxs-lookup"><span data-stu-id="c7e93-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="c7e93-167">Jeśli chcesz przekierować użytkownika z jednego kontrolera akcji do innego, należy wywołać metodę RedirectToAction().</span><span class="sxs-lookup"><span data-stu-id="c7e93-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="c7e93-168">Na przykład akcja Details() w ofercie 3 przedstawia widok lub przekierowuje użytkownika do akcji indeks(), w zależności od tego, czy parametr Id ma wartość.</span><span class="sxs-lookup"><span data-stu-id="c7e93-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="c7e93-169">**Wyświetlanie listy 3 - CustomerController.vb**</span><span class="sxs-lookup"><span data-stu-id="c7e93-169">**Listing 3 - CustomerController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

<span data-ttu-id="c7e93-170">Wynik akcji ContentResult to specjalne.</span><span class="sxs-lookup"><span data-stu-id="c7e93-170">The ContentResult action result is special.</span></span> <span data-ttu-id="c7e93-171">Można użyć ContentResult wyniku akcji do zwrócenia wyniku akcji jako zwykły tekst.</span><span class="sxs-lookup"><span data-stu-id="c7e93-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="c7e93-172">Na przykład metoda indeks() w ofercie 4 zwraca komunikat jako zwykły tekst, a nie kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="c7e93-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="c7e93-173">**Wyświetlanie listy 4 - Controllers\StatusController.vb**</span><span class="sxs-lookup"><span data-stu-id="c7e93-173">**Listing 4 - Controllers\StatusController.vb**</span></span>

> <span data-ttu-id="c7e93-174">StatusController</span><span class="sxs-lookup"><span data-stu-id="c7e93-174">StatusController</span></span>
> 
> 
> <span data-ttu-id="c7e93-175">System.Web.Mvc.Controller</span><span class="sxs-lookup"><span data-stu-id="c7e93-175">System.Web.Mvc.Controller</span></span>


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

<span data-ttu-id="c7e93-176">Wywołana Akcja StatusController.Index() widoku nie są zwracane.</span><span class="sxs-lookup"><span data-stu-id="c7e93-176">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="c7e93-177">Zamiast tego nieprzetworzony tekst "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="c7e93-177">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="c7e93-178">jest zwracany do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c7e93-178">is returned to the browser.</span></span>

<span data-ttu-id="c7e93-179">Akcja kontrolera zwraca wynik nie wynik akcji — na przykład datę lub liczbę całkowitą — następnie wynik jest otoczona ContentResult automatycznie.</span><span class="sxs-lookup"><span data-stu-id="c7e93-179">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="c7e93-180">Na przykład podczas wywoływania akcji indeks() WorkController w ofercie 5 Data jest zwracana jako ContentResult automatycznie.</span><span class="sxs-lookup"><span data-stu-id="c7e93-180">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="c7e93-181">**Wyświetlanie listy 5 - WorkController.vb**</span><span class="sxs-lookup"><span data-stu-id="c7e93-181">**Listing 5 - WorkController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

<span data-ttu-id="c7e93-182">Akcja indeks() w ofercie 5 zwraca obiekt daty/godziny.</span><span class="sxs-lookup"><span data-stu-id="c7e93-182">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="c7e93-183">Platforma ASP.NET MVC Konwertuje obiekt DateTime na ciąg i otacza wartość daty/godziny w ContentResult automatycznie.</span><span class="sxs-lookup"><span data-stu-id="c7e93-183">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="c7e93-184">Przeglądarka odbiera datę i godzinę w postaci zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="c7e93-184">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="c7e93-185">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="c7e93-185">Summary</span></span>

<span data-ttu-id="c7e93-186">Celem tego samouczka było wprowadzenie do koncepcji kontrolerów, akcji kontrolera i wyników akcji kontrolera ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c7e93-186">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="c7e93-187">W pierwszej sekcji przedstawiono sposób dodawania nowych kontrolerów do projektu programu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c7e93-187">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="c7e93-188">Następnie przedstawiono metod jak publiczny kontrolera są narażone na wszechświat jako akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c7e93-188">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="c7e93-189">Na koniec Omówiliśmy różne typy wyników akcji, które mogą zostać zwrócone z akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c7e93-189">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="c7e93-190">W szczególności omówiono sposób zwracania ViewResult, RedirectToActionResult i ContentResult z akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c7e93-190">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c7e93-191">[Poprzednie](creating-a-custom-route-constraint-cs.md)
> [dalej](creating-custom-routes-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c7e93-191">[Previous](creating-a-custom-route-constraint-cs.md)
[Next](creating-custom-routes-vb.md)</span></span>
