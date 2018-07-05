---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: Dodawanie kontrolera | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 'Uwaga: Zaktualizowaną wersję w tym samouczku jest dostępna w tym miejscu używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej stosować i pokaz...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 100f5623bafa33548f9c979e98765c92327eb369
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373292"
---
<a name="adding-a-controller"></a><span data-ttu-id="0e658-104">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="0e658-104">Adding a Controller</span></span>
====================
<span data-ttu-id="0e658-105">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="0e658-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="0e658-106">Jest dostępna zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="0e658-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="0e658-107">Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.</span><span class="sxs-lookup"><span data-stu-id="0e658-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="0e658-108">MVC to *model-view-controller*.</span><span class="sxs-lookup"><span data-stu-id="0e658-108">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="0e658-109">MVC to wzorzec do tworzenia aplikacji, które są również wysokiej, zakresie testować i łatwa w obsłudze.</span><span class="sxs-lookup"><span data-stu-id="0e658-109">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="0e658-110">Aplikacje oparte na MVC zawiera:</span><span class="sxs-lookup"><span data-stu-id="0e658-110">MVC-based applications contain:</span></span>

- <span data-ttu-id="0e658-111">**M** odels: klasy reprezentujące dane aplikacji, które korzystają z logikę walidacji do wymuszania reguł biznesowych dla tych danych.</span><span class="sxs-lookup"><span data-stu-id="0e658-111">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="0e658-112">**V** iews: pliki szablonów, których Twoja aplikacja używa do dynamicznego generowania odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="0e658-112">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="0e658-113">**C** ontrollers: klas, które obsługują żądania przychodzące przeglądarki, pobrać modelu danych, a następnie określ Przeglądanie szablonów, które zwracają odpowiedzi do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0e658-113">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="0e658-114">Firma Microsoft będzie obejmujące wszystkie te pojęcia w tej serii samouczków a pokazują, jak ich używać do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0e658-114">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

<span data-ttu-id="0e658-115">Zacznijmy od utworzenia klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0e658-115">Let's begin by creating a controller class.</span></span> <span data-ttu-id="0e658-116">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folder, a następnie wybierz **Dodaj kontroler**.</span><span class="sxs-lookup"><span data-stu-id="0e658-116">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span>

![](adding-a-controller/_static/image1.png)

<span data-ttu-id="0e658-117">Nadaj nazwę nowemu kontrolerowi &quot;HelloWorldController&quot;.</span><span class="sxs-lookup"><span data-stu-id="0e658-117">Name your new controller &quot;HelloWorldController&quot;.</span></span> <span data-ttu-id="0e658-118">Pozostaw domyślny szablon jako **pusty kontroler MVC** i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="0e658-118">Leave the default template as **Empty MVC controller** and click **Add**.</span></span>

![Dodawanie kontrolera](adding-a-controller/_static/image2.png)

<span data-ttu-id="0e658-120">Należy zauważyć w **Eksploratora rozwiązań** czy nowego pliku została utworzona o nazwie *HelloWorldController.cs*.</span><span class="sxs-lookup"><span data-stu-id="0e658-120">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs*.</span></span> <span data-ttu-id="0e658-121">Plik jest otwarty w środowisku IDE.</span><span class="sxs-lookup"><span data-stu-id="0e658-121">The file is open in the IDE.</span></span>

![](adding-a-controller/_static/image3.png)

<span data-ttu-id="0e658-122">Zastąp zawartość pliku następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="0e658-122">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="0e658-123">Metody kontrolera zwróci ciąg HTML jako przykład.</span><span class="sxs-lookup"><span data-stu-id="0e658-123">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="0e658-124">Nosi nazwę kontrolera `HelloWorldController` i nosi nazwę pierwszej metody powyżej `Index`.</span><span class="sxs-lookup"><span data-stu-id="0e658-124">The controller is named `HelloWorldController` and the first method above is named `Index`.</span></span> <span data-ttu-id="0e658-125">Możemy wywołać go z poziomu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0e658-125">Let's invoke it from a browser.</span></span> <span data-ttu-id="0e658-126">Uruchom aplikację (naciśnij klawisz F5 lub Ctrl + F5).</span><span class="sxs-lookup"><span data-stu-id="0e658-126">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="0e658-127">W przeglądarce, dołącz &quot;HelloWorld&quot; do ścieżki, w pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="0e658-127">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="0e658-128">(Na przykład na ilustracji poniżej jego `http://localhost:1234/HelloWorld.`) strony w przeglądarce będzie wyglądać podobnie jak na poniższym zrzucie ekranu.</span><span class="sxs-lookup"><span data-stu-id="0e658-128">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="0e658-129">W powyższej metody kod zwrócony ciąg bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="0e658-129">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="0e658-130">Otrzymaliśmy systemu, aby po prostu zwraca kod HTML i tak!</span><span class="sxs-lookup"><span data-stu-id="0e658-130">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="0e658-131">ASP.NET MVC wywołuje innego kontrolera klasy (i różnych metod akcji w nich), w zależności od przychodzącego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="0e658-131">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="0e658-132">Logiki routingu domyślnego adresu URL używany przez program ASP.NET MVC używa formatu następująco, aby określić, jaki kod do wywołania:</span><span class="sxs-lookup"><span data-stu-id="0e658-132">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="0e658-133">Pierwszą część adresu URL określa klasę kontrolera do wykonania.</span><span class="sxs-lookup"><span data-stu-id="0e658-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="0e658-134">Dlatego */HelloWorld* mapuje `HelloWorldController` klasy.</span><span class="sxs-lookup"><span data-stu-id="0e658-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="0e658-135">Druga część adresu URL określa metody akcji w klasie, do wykonania.</span><span class="sxs-lookup"><span data-stu-id="0e658-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="0e658-136">Dlatego */HelloWorld/indeks* spowodowałoby `Index` metody `HelloWorldController` klasy do wykonania.</span><span class="sxs-lookup"><span data-stu-id="0e658-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="0e658-137">Należy zauważyć, że musimy wskaż */HelloWorld* i `Index` metoda została użyta domyślna.</span><span class="sxs-lookup"><span data-stu-id="0e658-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="0e658-138">Jest to spowodowane metodę o nazwie `Index` jest domyślną metodą, która zostanie wywołana na kontrolerze, jeśli nie jest jawnie określona.</span><span class="sxs-lookup"><span data-stu-id="0e658-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span>

<span data-ttu-id="0e658-139">Przejdź do `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="0e658-139">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="0e658-140">`Welcome` Metoda uruchamia i zwraca ciąg &quot;jest to metoda powitalnej akcji... &quot;.</span><span class="sxs-lookup"><span data-stu-id="0e658-140">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="0e658-141">Domyślne mapowanie MVC to `/[Controller]/[ActionName]/[Parameters]`.</span><span class="sxs-lookup"><span data-stu-id="0e658-141">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="0e658-142">Dla tego adresu URL jest kontroler `HelloWorld` i `Welcome` jest metodą akcji.</span><span class="sxs-lookup"><span data-stu-id="0e658-142">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="0e658-143">Pierwszy raz używasz `[Parameters]` wchodzi w skład jeszcze adresu URL.</span><span class="sxs-lookup"><span data-stu-id="0e658-143">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="0e658-144">Zmodyfikujmy przykład nieco tak, aby przekazać niektóre informacje o parametrach z adresu URL do kontrolera (na przykład */HelloWorld/Witaj? nazwa = Scott&amp;numtimes = 4*).</span><span class="sxs-lookup"><span data-stu-id="0e658-144">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="0e658-145">Zmień swoje `Welcome` metodę, aby uwzględnić dwa parametry, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="0e658-145">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="0e658-146">Należy pamiętać, że kod używa funkcji opcjonalny parametr języka C# z informacją, że `numTimes` parametr powinien domyślnie na 1, jeśli nie przekazano żadnej wartości tego parametru.</span><span class="sxs-lookup"><span data-stu-id="0e658-146">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

<span data-ttu-id="0e658-147">Uruchom aplikację, a następnie przejdź do przykładowego adresu URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`.</span><span class="sxs-lookup"><span data-stu-id="0e658-147">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`.</span></span> <span data-ttu-id="0e658-148">Możesz spróbować różne wartości `name` i `numtimes` w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="0e658-148">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="0e658-149">[System powiązań modelu ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatycznie mapuje parametry nazwane z ciągu zapytania w pasku adresu do parametrów w metodzie.</span><span class="sxs-lookup"><span data-stu-id="0e658-149">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="0e658-150">W obu tych przykładach kontrolera została wykonując &quot;VC&quot; część MVC — oznacza to, widok i kontroler pracy.</span><span class="sxs-lookup"><span data-stu-id="0e658-150">In both these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="0e658-151">Kontroler zwraca HTML bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="0e658-151">The controller is returning HTML directly.</span></span> <span data-ttu-id="0e658-152">Zazwyczaj nie ma kontrolerów bezpośrednio, zwracając HTML, ponieważ staje się bardzo trudne kodu.</span><span class="sxs-lookup"><span data-stu-id="0e658-152">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="0e658-153">Zamiast tego zazwyczaj użyjemy pliku szablonu osobny widok ułatwiający Generowanie odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="0e658-153">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="0e658-154">Przyjrzyjmy się dalej, w jaki sposób możemy to zrobić.</span><span class="sxs-lookup"><span data-stu-id="0e658-154">Let's look next at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0e658-155">[Poprzednie](intro-to-aspnet-mvc-4.md)
> [dalej](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="0e658-155">[Previous](intro-to-aspnet-mvc-4.md)
[Next](adding-a-view.md)</span></span>
