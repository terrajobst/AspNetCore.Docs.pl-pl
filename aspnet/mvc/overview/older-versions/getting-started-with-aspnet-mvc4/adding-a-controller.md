---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: Dodawanie kontrolera | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: "Uwaga: Zaktualizowaną wersję tego samouczka jest dostępnych tutaj używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, znacznie prostsza do wykonania i demonstracją..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 69af91401e51470fbc0b67103345325201b06723
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller"></a><span data-ttu-id="ed4d2-104">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="ed4d2-104">Adding a Controller</span></span>
====================
<span data-ttu-id="ed4d2-105">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="ed4d2-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="ed4d2-106">Dostępna jest zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="ed4d2-107">Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="ed4d2-108">Oznacza MVC *model-view-controller*.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-108">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="ed4d2-109">MVC to wzorzec do tworzenia aplikacji, które są dobrze zaprojektowanej architektury, testować i łatwe w konserwacji.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-109">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="ed4d2-110">Aplikacje oparte na MVC zawiera:</span><span class="sxs-lookup"><span data-stu-id="ed4d2-110">MVC-based applications contain:</span></span>

- <span data-ttu-id="ed4d2-111">**M** odels: klasy reprezentujące dane aplikacji, które korzystają z logikę weryfikacji do wymuszania reguł biznesowych dla tych danych.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-111">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="ed4d2-112">**V** iews: plików szablonów, które Twoja aplikacja używa do dynamicznego generowania odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-112">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="ed4d2-113">**C** ontrollers: klas, które obsługują przychodzące żądania przeglądarki, pobrać modelu danych, a następnie określ szablonów widoków, które zwracają odpowiedzi do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-113">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="ed4d2-114">Firma Microsoft będzie można obejmujące wszystkie te pojęcia w tym samouczku i opisano, jak ich używać do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-114">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

<span data-ttu-id="ed4d2-115">Zacznijmy od utworzenia klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-115">Let's begin by creating a controller class.</span></span> <span data-ttu-id="ed4d2-116">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folder, a następnie wybierz **Dodaj kontroler**.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-116">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span>

![](adding-a-controller/_static/image1.png)

<span data-ttu-id="ed4d2-117">Nowemu kontrolerowi nazwę &quot;HelloWorldController&quot;.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-117">Name your new controller &quot;HelloWorldController&quot;.</span></span> <span data-ttu-id="ed4d2-118">Pozostaw domyślny szablon jako **kontroler MVC pusty** i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-118">Leave the default template as **Empty MVC controller** and click **Add**.</span></span>

![Dodawanie kontrolera](adding-a-controller/_static/image2.png)

<span data-ttu-id="ed4d2-120">Zwróć uwagę, w **Eksploratora rozwiązań** czy nowego pliku została utworzona o nazwie *HelloWorldController.cs*.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-120">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs*.</span></span> <span data-ttu-id="ed4d2-121">Plik jest otwarty w IDE.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-121">The file is open in the IDE.</span></span>

![](adding-a-controller/_static/image3.png)

<span data-ttu-id="ed4d2-122">Zastąp zawartość pliku następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-122">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="ed4d2-123">Jako przykład metod kontrolera zwraca ciąg HTML.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-123">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="ed4d2-124">Nosi nazwę kontrolera `HelloWorldController` i nosi nazwę pierwsza metoda powyżej `Index`.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-124">The controller is named `HelloWorldController` and the first method above is named `Index`.</span></span> <span data-ttu-id="ed4d2-125">Teraz wywołać go w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-125">Let's invoke it from a browser.</span></span> <span data-ttu-id="ed4d2-126">Uruchom aplikację (naciśnij klawisz F5 lub Ctrl + F5).</span><span class="sxs-lookup"><span data-stu-id="ed4d2-126">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="ed4d2-127">W przeglądarce, dołącz &quot;HelloWorld&quot; do ścieżki w pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-127">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="ed4d2-128">(Na przykład na ilustracji poniżej jego `http://localhost:1234/HelloWorld.`) strony w przeglądarce będzie wyglądać podobnie jak na poniższym zrzucie ekranu.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-128">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="ed4d2-129">W powyższej metodzie kod zwrócony ciąg bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-129">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="ed4d2-130">Informację systemu tylko zwrócić kod HTML i jak!</span><span class="sxs-lookup"><span data-stu-id="ed4d2-130">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="ed4d2-131">ASP.NET MVC wywołuje klasy innego kontrolera (i różnych metod akcji w nich), w zależności od przychodzącego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-131">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="ed4d2-132">Logiki routingu domyślny adres URL używany przez program ASP.NET MVC używa formatu podobny do tego, aby określić, jaki kod do wywołania:</span><span class="sxs-lookup"><span data-stu-id="ed4d2-132">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="ed4d2-133">Pierwsza część adresu URL określa klasy kontrolera do wykonania.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="ed4d2-134">Dlatego */HelloWorld* mapuje `HelloWorldController` klasy.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="ed4d2-135">Druga część adresu URL określa metody akcji w klasie, do wykonania.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="ed4d2-136">Dlatego */HelloWorld/indeksu* spowodowałoby `Index` metody `HelloWorldController` klasy do wykonania.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="ed4d2-137">Należy zauważyć, że Musieliśmy aby przejść do */HelloWorld* i `Index` użyto metody domyślnie.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="ed4d2-138">Jest to spowodowane metodę o nazwie `Index` jest domyślna metoda, która zostanie wywołana na kontrolerze, jeśli nie został jawnie określony.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span>

<span data-ttu-id="ed4d2-139">Przejdź do `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-139">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="ed4d2-140">`Welcome` Metoda uruchamia i zwraca ciąg &quot;jest to metoda akcji Zapraszamy... &quot;.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-140">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="ed4d2-141">Domyślne mapowanie MVC jest `/[Controller]/[ActionName]/[Parameters]`.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-141">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="ed4d2-142">Dla tego adresu URL kontrolera jest `HelloWorld` i `Welcome` jest metodą akcji.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-142">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="ed4d2-143">Nie były używane `[Parameters]` część jeszcze adresu URL.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-143">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="ed4d2-144">Załóżmy przykładzie nieco zmodyfikować tak, aby niektóre informacje o parametrach z adresu URL można przekazać do kontrolera (na przykład */HelloWorld/Zapraszamy? nazwa = Scott&amp;numtimes = 4*).</span><span class="sxs-lookup"><span data-stu-id="ed4d2-144">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="ed4d2-145">Zmień Twoje `Welcome` metodę w celu uwzględnienia dwóch parametrów, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-145">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="ed4d2-146">Należy pamiętać, że kod jest używana funkcja opcjonalny parametr C# z informacją, że `numTimes` parametr powinien domyślnie 1, jeśli nie przekazano żadnej wartości tego parametru.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-146">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

<span data-ttu-id="ed4d2-147">Uruchom aplikację i przejdź do adresu URL przykład (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-147">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`.</span></span> <span data-ttu-id="ed4d2-148">Możesz spróbować różnych wartości `name` i `numtimes` w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-148">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="ed4d2-149">[System powiązanie modelu platformy ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatycznie mapuje parametry nazwane z ciągu zapytania w pasku adresu w parametrach w metodę.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-149">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="ed4d2-150">W obu tych przykładach został ten kontroler &quot;VC&quot; część MVC — to znaczy pracy widok i kontroler.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-150">In both these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="ed4d2-151">Kontroler bezpośrednio zwraca HTML.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-151">The controller is returning HTML directly.</span></span> <span data-ttu-id="ed4d2-152">Zwykle nie mają kontrolerów bezpośrednio, zwracanie HTML, ponieważ staje się bardzo trudne kodu.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-152">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="ed4d2-153">Zamiast tego zazwyczaj użyjemy pliku szablonu osobny widok ułatwia generowanie odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-153">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="ed4d2-154">Oto dalej w sposób firma Microsoft można to zrobić.</span><span class="sxs-lookup"><span data-stu-id="ed4d2-154">Let's look next at how we can do this.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ed4d2-155">[Poprzednie](intro-to-aspnet-mvc-4.md)
[dalej](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="ed4d2-155">[Previous](intro-to-aspnet-mvc-4.md)
[Next](adding-a-view.md)</span></span>
