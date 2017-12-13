---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: "Omówienie kontrolera ASP.NET MVC (VB) | Dokumentacja firmy Microsoft"
author: StephenWalther
description: "W tym samouczku Stephen Walther przedstawiono kontrolery ASP.NET MVC. Jak utworzyć nowe kontrolery i zwracanie różnych typów akcji res..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 452e2cf771e8b1f298ce9693ec2a707e7c1d4dd1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-controller-overview-vb"></a><span data-ttu-id="02de0-104">Omówienie kontrolera ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="02de0-104">ASP.NET MVC Controller Overview (VB)</span></span>
====================
<span data-ttu-id="02de0-105">przez [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="02de0-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="02de0-106">W tym samouczku Stephen Walther przedstawiono kontrolery ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="02de0-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="02de0-107">Jak utworzyć nowe kontrolery i zwracanie różnych typów wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="02de0-107">You learn how to create new controllers and return different types of action results.</span></span>


<span data-ttu-id="02de0-108">W tym samouczku Eksploruje temat kontrolery ASP.NET MVC, akcji kontrolera i wyniki akcji.</span><span class="sxs-lookup"><span data-stu-id="02de0-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="02de0-109">Po ukończeniu tego samouczka zostanie zrozumieć, jak kontrolery są używane do kontrolować sposób którą użytkownik wchodzi w interakcję z witryny sieci Web platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="02de0-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="02de0-110">Opis kontrolerów</span><span class="sxs-lookup"><span data-stu-id="02de0-110">Understanding Controllers</span></span>

<span data-ttu-id="02de0-111">Kontrolerów MVC jest odpowiedzialny za odpowiada na żądania wysyłane przed witryny sieci Web platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="02de0-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="02de0-112">Każde żądanie przeglądarki jest mapowane na określony kontroler.</span><span class="sxs-lookup"><span data-stu-id="02de0-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="02de0-113">Załóżmy na przykład, wprowadź następujący adres URL na pasku adresu przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="02de0-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="02de0-114">W takim przypadku kontrolerze o nazwie ProductController jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="02de0-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="02de0-115">ProductController jest odpowiedzialny za Generowanie odpowiedzi na żądanie.</span><span class="sxs-lookup"><span data-stu-id="02de0-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="02de0-116">Na przykład kontroler może wrócić określonego widoku do przeglądarki lub kontroler może przekierować użytkownika do innego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="02de0-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="02de0-117">Wyświetlanie listy 1 zawiera proste kontrolerze o nazwie ProductController.</span><span class="sxs-lookup"><span data-stu-id="02de0-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="02de0-118">**Listing1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="02de0-118">**Listing1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

<span data-ttu-id="02de0-119">Jak widać z zakresu od 1 do wyświetlania, kontrolera jest właśnie klasą (Visual Basic .NET lub C#).</span><span class="sxs-lookup"><span data-stu-id="02de0-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="02de0-120">Kontroler jest klasą pochodzącą od klasy podstawowej System.Web.Mvc.Controller.</span><span class="sxs-lookup"><span data-stu-id="02de0-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="02de0-121">Ponieważ kontrolera dziedziczy po tej klasie podstawowej, kontrolera bezpłatnie dziedziczy kilka metod przydatne (omówiono te metody później).</span><span class="sxs-lookup"><span data-stu-id="02de0-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="02de0-122">Opis akcji kontrolera</span><span class="sxs-lookup"><span data-stu-id="02de0-122">Understanding Controller Actions</span></span>

<span data-ttu-id="02de0-123">Kontroler przedstawia akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="02de0-123">A controller exposes controller actions.</span></span> <span data-ttu-id="02de0-124">Akcja jest metoda na kontrolerze, która jest wywoływana po wprowadzeniu określonego adresu URL na pasku adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="02de0-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="02de0-125">Załóżmy na przykład, musisz wysłać żądanie dla następującego adresu URL:</span><span class="sxs-lookup"><span data-stu-id="02de0-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="02de0-126">W takim przypadku klasa ProductController wywoływana jest metoda indeks().</span><span class="sxs-lookup"><span data-stu-id="02de0-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="02de0-127">Metoda indeks() jest przykładem akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="02de0-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="02de0-128">Akcja kontrolera musi być publiczną metodę klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="02de0-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="02de0-129">Visual języku, domyślnie przedstawiono publicznej metody.</span><span class="sxs-lookup"><span data-stu-id="02de0-129">Visual Basic.NET methods, by default, are public methods.</span></span> <span data-ttu-id="02de0-130">Należy pamiętać, że metoda publiczna, które dodajesz do klasy kontrolera jest ujawniona jako akcji kontrolera automatycznie (należy zachować ostrożność na ten temat ponieważ akcji kontrolera może być wywoływany dla wszystkich użytkowników na całym świecie, po prostu, wpisując adres URL prawego w pasku adresu przeglądarki).</span><span class="sxs-lookup"><span data-stu-id="02de0-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="02de0-131">Istnieją pewne dodatkowe wymagania, które muszą być spełnione przez akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="02de0-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="02de0-132">Metoda używana jako akcji kontrolera nie może być przeciążony.</span><span class="sxs-lookup"><span data-stu-id="02de0-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="02de0-133">Ponadto akcji kontrolera nie może być metodą statyczną.</span><span class="sxs-lookup"><span data-stu-id="02de0-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="02de0-134">Inną niż można użyć dowolnego — metoda akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="02de0-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="02de0-135">Opis wyników akcji</span><span class="sxs-lookup"><span data-stu-id="02de0-135">Understanding Action Results</span></span>

<span data-ttu-id="02de0-136">Akcja kontrolera zwraca coś o nazwie *wynik akcji*.</span><span class="sxs-lookup"><span data-stu-id="02de0-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="02de0-137">Wynik akcji jest akcją kontrolera zwraca w odpowiedzi na żądanie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="02de0-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="02de0-138">Platforma ASP.NET MVC obsługuje kilka typów wyników akcji w tym:</span><span class="sxs-lookup"><span data-stu-id="02de0-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="02de0-139">ViewResult - reprezentuje HTML i znaczników.</span><span class="sxs-lookup"><span data-stu-id="02de0-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="02de0-140">EmptyResult - reprezentuje żadnego wyniku.</span><span class="sxs-lookup"><span data-stu-id="02de0-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="02de0-141">RedirectResult - reprezentuje przekierowanie do nowego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="02de0-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="02de0-142">JsonResult — przedstawia wynik JavaScript Object Notation, które mogą być używane w aplikacji AJAX.</span><span class="sxs-lookup"><span data-stu-id="02de0-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="02de0-143">JavaScriptResult - reprezentuje skryptu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="02de0-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="02de0-144">ContentResult - reprezentuje wynik tekstu.</span><span class="sxs-lookup"><span data-stu-id="02de0-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="02de0-145">FileContentResult - reprezentuje do pobrania pliku (z zawartość binarną).</span><span class="sxs-lookup"><span data-stu-id="02de0-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="02de0-146">FilePathResult - reprezentuje plik do pobrania (ze ścieżką).</span><span class="sxs-lookup"><span data-stu-id="02de0-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="02de0-147">FileStreamResult - reprezentuje plik do pobrania (za pomocą strumienia pliku).</span><span class="sxs-lookup"><span data-stu-id="02de0-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="02de0-148">Wszystkie te wyniki akcji dziedziczyć po klasie ActionResult podstawowej.</span><span class="sxs-lookup"><span data-stu-id="02de0-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="02de0-149">W większości przypadków akcji kontrolera zwraca ViewResult.</span><span class="sxs-lookup"><span data-stu-id="02de0-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="02de0-150">Na przykład w akcji kontrolera indeks wyświetlania 2 zwraca ViewResult.</span><span class="sxs-lookup"><span data-stu-id="02de0-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="02de0-151">**Wyświetlanie listy 2 - Controllers\BookController.vb**</span><span class="sxs-lookup"><span data-stu-id="02de0-151">**Listing 2 - Controllers\BookController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

<span data-ttu-id="02de0-152">Po powrocie z operacji ViewResult HTML jest zwracany do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="02de0-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="02de0-153">Metoda indeks() wyświetlania 2 zwraca widok o nazwie indeksu w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="02de0-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="02de0-154">Zwróć uwagę, w akcji indeks() wyświetlania 2 nie zwraca ViewResult().</span><span class="sxs-lookup"><span data-stu-id="02de0-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="02de0-155">Zamiast tego jest wywoływana metoda View() klasy podstawowej kontrolera.</span><span class="sxs-lookup"><span data-stu-id="02de0-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="02de0-156">Zwykle użytkownik nie zwrócenia wyniku akcji bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="02de0-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="02de0-157">Zamiast tego wywołania jest jedną z następujących metod klasy podstawowej kontrolera:</span><span class="sxs-lookup"><span data-stu-id="02de0-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="02de0-158">Wyświetl — zwraca wynik akcji ViewResult.</span><span class="sxs-lookup"><span data-stu-id="02de0-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="02de0-159">Przekieruj — zwraca wynik akcji RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="02de0-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="02de0-160">RedirectToAction — zwraca wynik akcji RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="02de0-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="02de0-161">Metody RedirectToRoute — zwraca wynik akcji RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="02de0-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="02de0-162">JSON — zwraca wynik akcji JsonResult.</span><span class="sxs-lookup"><span data-stu-id="02de0-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="02de0-163">JavaScriptResult — zwraca JavaScriptResult.</span><span class="sxs-lookup"><span data-stu-id="02de0-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="02de0-164">Zawartości — zwraca wynik akcji ContentResult.</span><span class="sxs-lookup"><span data-stu-id="02de0-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="02de0-165">Plik — zwraca FileContentResult, FilePathResult lub FileStreamResult w zależności od parametry przekazane do metody.</span><span class="sxs-lookup"><span data-stu-id="02de0-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="02de0-166">Tak aby zwrócić widok do przeglądarki, należy wywołać metodę View().</span><span class="sxs-lookup"><span data-stu-id="02de0-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="02de0-167">Jeśli chcesz przekierowywać użytkowników z jednego kontrolera akcji do innego, należy wywołać metodę RedirectToAction().</span><span class="sxs-lookup"><span data-stu-id="02de0-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="02de0-168">Na przykład w akcji Details() wyświetlania 3 przedstawia widok albo przekierowuje użytkownika do akcji indeks() w zależności od tego, czy parametr Id ma wartość.</span><span class="sxs-lookup"><span data-stu-id="02de0-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="02de0-169">**Wyświetlanie listy 3 - CustomerController.vb**</span><span class="sxs-lookup"><span data-stu-id="02de0-169">**Listing 3 - CustomerController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

<span data-ttu-id="02de0-170">Wynik akcji ContentResult jest specjalne.</span><span class="sxs-lookup"><span data-stu-id="02de0-170">The ContentResult action result is special.</span></span> <span data-ttu-id="02de0-171">Wynik akcji ContentResult służy do zwrócenia wyniku akcji jako zwykły tekst.</span><span class="sxs-lookup"><span data-stu-id="02de0-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="02de0-172">Na przykład metoda indeks() listę 4 zwraca komunikat jako zwykły tekst, a nie w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="02de0-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="02de0-173">**Wyświetlanie listy 4 - Controllers\StatusController.vb**</span><span class="sxs-lookup"><span data-stu-id="02de0-173">**Listing 4 - Controllers\StatusController.vb**</span></span>

> <span data-ttu-id="02de0-174">StatusController</span><span class="sxs-lookup"><span data-stu-id="02de0-174">StatusController</span></span>


> <span data-ttu-id="02de0-175">System.Web.Mvc.Controller</span><span class="sxs-lookup"><span data-stu-id="02de0-175">System.Web.Mvc.Controller</span></span>


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

<span data-ttu-id="02de0-176">Po wywołaniu akcji StatusController.Index() widoku nie są zwracane.</span><span class="sxs-lookup"><span data-stu-id="02de0-176">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="02de0-177">Zamiast tego nieprzetworzony tekst "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="02de0-177">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="02de0-178">jest zwracany do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="02de0-178">is returned to the browser.</span></span>

<span data-ttu-id="02de0-179">Jeśli akcja kontrolera zwraca wynik nie wynik akcji — na przykład datę lub liczbę całkowitą — następnie wynik jest ujęte w ContentResult automatycznie.</span><span class="sxs-lookup"><span data-stu-id="02de0-179">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="02de0-180">Na przykład po wywołaniu akcji indeks() WorkController listę 5 daty jest zwracana jako ContentResult automatycznie.</span><span class="sxs-lookup"><span data-stu-id="02de0-180">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="02de0-181">**Wyświetlanie listy 5 - WorkController.vb**</span><span class="sxs-lookup"><span data-stu-id="02de0-181">**Listing 5 - WorkController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

<span data-ttu-id="02de0-182">W akcji indeks() listę 5 zwraca obiekt DateTime.</span><span class="sxs-lookup"><span data-stu-id="02de0-182">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="02de0-183">Platforma ASP.NET MVC Konwertuje obiekt daty i godziny do ciągu i opakowuje wartość daty i godziny w ContentResult automatycznie.</span><span class="sxs-lookup"><span data-stu-id="02de0-183">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="02de0-184">Przeglądarka odbiera datę i godzinę jako zwykły tekst.</span><span class="sxs-lookup"><span data-stu-id="02de0-184">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="02de0-185">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="02de0-185">Summary</span></span>

<span data-ttu-id="02de0-186">Celem tego samouczka było przedstawiono podstawowe pojęcia kontrolery ASP.NET MVC, akcji kontrolera i wyniki akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="02de0-186">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="02de0-187">W pierwszej sekcji przedstawiono sposób dodawania nowych kontrolerów do projektu programu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="02de0-187">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="02de0-188">Następnie przedstawiono sposób publicznej metody kontrolera są widoczne dla całość jako akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="02de0-188">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="02de0-189">Ponadto omówiono różne typy wyników akcji, które mogą zostać zwrócone z akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="02de0-189">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="02de0-190">W szczególności omówiono sposób zwracania ViewResult, RedirectToActionResult i ContentResult z akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="02de0-190">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="02de0-191">[Poprzednie](creating-a-custom-route-constraint-cs.md)
[dalej](creating-custom-routes-vb.md)</span><span class="sxs-lookup"><span data-stu-id="02de0-191">[Previous](creating-a-custom-route-constraint-cs.md)
[Next](creating-custom-routes-vb.md)</span></span>
