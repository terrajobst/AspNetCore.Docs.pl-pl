---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Dodawanie kontrolera | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: d5cc9db7b1eec139a37afb6427fd761342fcc1f5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751956"
---
<a name="adding-a-controller"></a><span data-ttu-id="687e5-102">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="687e5-102">Adding a Controller</span></span>
====================
<span data-ttu-id="687e5-103">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="687e5-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="687e5-104">MVC to *model-view-controller*.</span><span class="sxs-lookup"><span data-stu-id="687e5-104">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="687e5-105">MVC to wzorzec do tworzenia aplikacji, które są również wysokiej, zakresie testować i łatwa w obsłudze.</span><span class="sxs-lookup"><span data-stu-id="687e5-105">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="687e5-106">Aplikacje oparte na MVC zawiera:</span><span class="sxs-lookup"><span data-stu-id="687e5-106">MVC-based applications contain:</span></span>

- <span data-ttu-id="687e5-107">**M** odels: klasy reprezentujące dane aplikacji, które korzystają z logikę walidacji do wymuszania reguł biznesowych dla tych danych.</span><span class="sxs-lookup"><span data-stu-id="687e5-107">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="687e5-108">**V** iews: pliki szablonów, których Twoja aplikacja używa do dynamicznego generowania odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="687e5-108">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="687e5-109">**C** ontrollers: klas, które obsługują żądania przychodzące przeglądarki, pobrać modelu danych, a następnie określ Przeglądanie szablonów, które zwracają odpowiedzi do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="687e5-109">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="687e5-110">Firma Microsoft będzie obejmujące wszystkie te pojęcia w tej serii samouczków a pokazują, jak ich używać do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="687e5-110">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

> [!NOTE]
> <span data-ttu-id="687e5-111">W poprzednim kroku MVC domyślny szablon został wybrany.</span><span class="sxs-lookup"><span data-stu-id="687e5-111">In the previous step the Default MVC template was selected.</span></span> <span data-ttu-id="687e5-112">Spowoduje to utworzenie domu, konta i Zarządzanie kontrolerami domyślnie.</span><span class="sxs-lookup"><span data-stu-id="687e5-112">This creates Home, Account and Manage Controllers by default.</span></span>

<span data-ttu-id="687e5-113">Zacznijmy od utworzenia klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="687e5-113">Let's begin by creating a controller class.</span></span> <span data-ttu-id="687e5-114">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folder, a następnie kliknij przycisk **Dodaj**, następnie **kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="687e5-114">In **Solution Explorer**, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>


![](adding-a-controller/_static/image1.png)

<span data-ttu-id="687e5-115">W **Dodawanie szkieletu** okno dialogowe, kliknij przycisk **kontroler MVC 5 — pusty**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="687e5-115">In the **Add Scaffold** dialog box, click **MVC 5 Controller - Empty**, and then click **Add**.</span></span>

![](adding-a-controller/_static/image2.png)  
 

<span data-ttu-id="687e5-116">Nadaj nowemu kontrolerowi nazwę "HelloWorldController", a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="687e5-116">Name your new controller "HelloWorldController" and click **Add**.</span></span>

![Dodawanie kontrolera](adding-a-controller/_static/image3.png)

<span data-ttu-id="687e5-118">Należy zauważyć w **Eksploratora rozwiązań** czy nowego pliku została utworzona o nazwie *HelloWorldController.cs* i nowy folder *Views\HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="687e5-118">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs* and a new folder *Views\HelloWorld*.</span></span> <span data-ttu-id="687e5-119">Kontroler jest otwarty w środowisku IDE.</span><span class="sxs-lookup"><span data-stu-id="687e5-119">The controller is open in the IDE.</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="687e5-120">Zastąp zawartość pliku następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="687e5-120">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="687e5-121">Metody kontrolera zwróci ciąg HTML jako przykład.</span><span class="sxs-lookup"><span data-stu-id="687e5-121">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="687e5-122">Nosi nazwę kontrolera `HelloWorldController` i nosi nazwę pierwszej metody `Index`.</span><span class="sxs-lookup"><span data-stu-id="687e5-122">The controller is named `HelloWorldController` and the first method is named `Index`.</span></span> <span data-ttu-id="687e5-123">Możemy wywołać go z poziomu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="687e5-123">Let's invoke it from a browser.</span></span> <span data-ttu-id="687e5-124">Uruchom aplikację (naciśnij klawisz F5 lub Ctrl + F5).</span><span class="sxs-lookup"><span data-stu-id="687e5-124">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="687e5-125">W przeglądarce, dołącz &quot;HelloWorld&quot; do ścieżki, w pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="687e5-125">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="687e5-126">(Na przykład na ilustracji poniżej jego `http://localhost:1234/HelloWorld.`) strony w przeglądarce będzie wyglądać podobnie jak na poniższym zrzucie ekranu.</span><span class="sxs-lookup"><span data-stu-id="687e5-126">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="687e5-127">W powyższej metody kod zwrócony ciąg bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="687e5-127">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="687e5-128">Otrzymaliśmy systemu, aby po prostu zwraca kod HTML i tak!</span><span class="sxs-lookup"><span data-stu-id="687e5-128">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="687e5-129">ASP.NET MVC wywołuje innego kontrolera klasy (i różnych metod akcji w nich), w zależności od przychodzącego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="687e5-129">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="687e5-130">Logiki routingu domyślnego adresu URL używany przez program ASP.NET MVC używa formatu następująco, aby określić, jaki kod do wywołania:</span><span class="sxs-lookup"><span data-stu-id="687e5-130">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="687e5-131">Ustaw format routingu w *aplikacji\_Start/RouteConfig.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="687e5-131">You set the format for routing in the *App\_Start/RouteConfig.cs* file.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

<span data-ttu-id="687e5-132">Podczas uruchamiania aplikacji, a nie podasz żadnych segmentów adresu URL, jego wartość domyślna to "Domowy" kontrolera i metody akcji "Index" określone w sekcji Ustawienia domyślne powyżej kodu.</span><span class="sxs-lookup"><span data-stu-id="687e5-132">When you run the application and don't supply any URL segments, it defaults to the "Home" controller and the "Index" action method specified in the defaults section of the code above.</span></span>

<span data-ttu-id="687e5-133">Pierwszą część adresu URL określa klasę kontrolera do wykonania.</span><span class="sxs-lookup"><span data-stu-id="687e5-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="687e5-134">Dlatego */HelloWorld* mapuje `HelloWorldController` klasy.</span><span class="sxs-lookup"><span data-stu-id="687e5-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="687e5-135">Druga część adresu URL określa metody akcji w klasie, do wykonania.</span><span class="sxs-lookup"><span data-stu-id="687e5-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="687e5-136">Dlatego */HelloWorld/indeks* spowodowałoby `Index` metody `HelloWorldController` klasy do wykonania.</span><span class="sxs-lookup"><span data-stu-id="687e5-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="687e5-137">Należy zauważyć, że musimy wskaż */HelloWorld* i `Index` metoda została użyta domyślna.</span><span class="sxs-lookup"><span data-stu-id="687e5-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="687e5-138">Jest to spowodowane metodę o nazwie `Index` jest domyślną metodą, która zostanie wywołana na kontrolerze, jeśli nie jest jawnie określona.</span><span class="sxs-lookup"><span data-stu-id="687e5-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span> <span data-ttu-id="687e5-139">Trzecia część segment adresu URL ( `Parameters`) dla danych trasy.</span><span class="sxs-lookup"><span data-stu-id="687e5-139">The third part of the URL segment ( `Parameters`) is for route data.</span></span> <span data-ttu-id="687e5-140">Zobaczymy danych trasy w późniejszym czasie na w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="687e5-140">We'll see route data later on in this tutorial.</span></span>

<span data-ttu-id="687e5-141">Przejdź do `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="687e5-141">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="687e5-142">`Welcome` Metoda uruchamia i zwraca ciąg &quot;jest to metoda powitalnej akcji... &quot;.</span><span class="sxs-lookup"><span data-stu-id="687e5-142">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="687e5-143">Domyślne mapowanie MVC to `/[Controller]/[ActionName]/[Parameters]`.</span><span class="sxs-lookup"><span data-stu-id="687e5-143">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="687e5-144">Dla tego adresu URL jest kontroler `HelloWorld` i `Welcome` jest metodą akcji.</span><span class="sxs-lookup"><span data-stu-id="687e5-144">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="687e5-145">Pierwszy raz używasz `[Parameters]` wchodzi w skład jeszcze adresu URL.</span><span class="sxs-lookup"><span data-stu-id="687e5-145">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="687e5-146">Zmodyfikujmy przykład nieco tak, aby przekazać niektóre informacje o parametrach z adresu URL do kontrolera (na przykład */HelloWorld/Witaj? nazwa = Scott&amp;numtimes = 4*).</span><span class="sxs-lookup"><span data-stu-id="687e5-146">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="687e5-147">Zmień swoje `Welcome` metodę, aby uwzględnić dwa parametry, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="687e5-147">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="687e5-148">Należy pamiętać, że kod używa funkcji opcjonalny parametr języka C# z informacją, że `numTimes` parametr powinien domyślnie na 1, jeśli nie przekazano żadnej wartości tego parametru.</span><span class="sxs-lookup"><span data-stu-id="687e5-148">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="687e5-149">Uwaga dotycząca zabezpieczeń: Powyższy kod używa [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) chronić aplikację przed złośliwe dane wejściowe (to znaczy JavaScript).</span><span class="sxs-lookup"><span data-stu-id="687e5-149">Security Note: The code above uses [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) to protect the application from malicious input (namely JavaScript).</span></span> <span data-ttu-id="687e5-150">Aby uzyskać więcej informacji, zobacz [porady: ochrona względem skryptu luki w aplikacji sieci Web, stosując kodowanie HTML na ciągi](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="687e5-150">For more information see [How to: Protect Against Script Exploits in a Web Application by Applying HTML Encoding to Strings](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span></span>


 <span data-ttu-id="687e5-151">Uruchom aplikację, a następnie przejdź do przykładowego adresu URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span><span class="sxs-lookup"><span data-stu-id="687e5-151">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span></span> <span data-ttu-id="687e5-152">Możesz spróbować różne wartości `name` i `numtimes` w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="687e5-152">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="687e5-153">[System powiązań modelu ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatycznie mapuje parametry nazwane z ciągu zapytania w pasku adresu do parametrów w metodzie.</span><span class="sxs-lookup"><span data-stu-id="687e5-153">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="687e5-154">W przykładzie powyżej segment adresu URL ( `Parameters`) nie jest używany, `name` i `numTimes` parametry są przekazywane jako [ciągów zapytania](http://en.wikipedia.org/wiki/Query_string).</span><span class="sxs-lookup"><span data-stu-id="687e5-154">In the sample above, the URL segment ( `Parameters`) is not used, the `name` and `numTimes` parameters are passed as [query strings](http://en.wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="687e5-155">?</span><span class="sxs-lookup"><span data-stu-id="687e5-155">The ?</span></span> <span data-ttu-id="687e5-156">(znak zapytania) w powyższym adresie URL jest separatorem, a następnie wykonaj ciągi zapytań.</span><span class="sxs-lookup"><span data-stu-id="687e5-156">(question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="687e5-157">&amp; Rozdziela ciągi zapytań.</span><span class="sxs-lookup"><span data-stu-id="687e5-157">The &amp; character separates query strings.</span></span>

<span data-ttu-id="687e5-158">Zastąp metodę powitalnej następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="687e5-158">Replace the Welcome method with the following code:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

<span data-ttu-id="687e5-159">Uruchom aplikację, a następnie wprowadź następujący adres URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span><span class="sxs-lookup"><span data-stu-id="687e5-159">Run the application and enter the following URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span></span>

![](adding-a-controller/_static/image8.png)

<span data-ttu-id="687e5-160">Tym razem trzeci segment adresu URL dopasowane parametru trasy `ID.` `Welcome` metody akcji zawiera parametr (`ID`) pasujących specyfikacji adresu URL w `RegisterRoutes` metody.</span><span class="sxs-lookup"><span data-stu-id="687e5-160">This time the third URL segment matched the route parameter `ID.` The `Welcome` action method contains a parameter (`ID`) that matched the URL specification in the `RegisterRoutes` method.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

<span data-ttu-id="687e5-161">W aplikacjach ASP.NET MVC jest bardziej typowego, aby przekazać parametry trasy danych (takich jak zrobiliśmy o identyfikatorze powyżej) od przekazywania ich jako ciągi zapytań.</span><span class="sxs-lookup"><span data-stu-id="687e5-161">In ASP.NET MVC applications, it's more typical to pass in parameters as route data (like we did with ID above) than passing them as query strings.</span></span> <span data-ttu-id="687e5-162">Można również dodać trasę do przekazywania zarówno `name` i `numtimes` w parametrach jako dane trasy w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="687e5-162">You could also add a route to pass both the `name` and `numtimes` in parameters as route data in the URL.</span></span> <span data-ttu-id="687e5-163">W *aplikacji\_Start\RouteConfig.cs* plików, Dodawanie trasy "Hello":</span><span class="sxs-lookup"><span data-stu-id="687e5-163">In the *App\_Start\RouteConfig.cs* file, add the "Hello" route:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

<span data-ttu-id="687e5-164">Uruchom aplikację, a następnie przejdź do `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span><span class="sxs-lookup"><span data-stu-id="687e5-164">Run the application and browse to `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span></span>

![](adding-a-controller/_static/image9.png)

<span data-ttu-id="687e5-165">W przypadku wielu aplikacji MVC trasy domyślnej działa prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="687e5-165">For many MVC applications, the default route works fine.</span></span> <span data-ttu-id="687e5-166">Dowiesz się w dalszej części tego samouczka, aby przekazać dane za pomocą integratora modelu, a nie będziesz mieć zmodyfikować domyślną trasę dla tego.</span><span class="sxs-lookup"><span data-stu-id="687e5-166">You'll learn later in this tutorial to pass data using the model binder, and you won't have to modify the default route for that.</span></span>

<span data-ttu-id="687e5-167">W tych przykładach kontrolera została wykonując &quot;VC&quot; część MVC — oznacza to, widok i kontroler pracy.</span><span class="sxs-lookup"><span data-stu-id="687e5-167">In these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="687e5-168">Kontroler zwraca HTML bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="687e5-168">The controller is returning HTML directly.</span></span> <span data-ttu-id="687e5-169">Zazwyczaj nie ma kontrolerów bezpośrednio, zwracając HTML, ponieważ staje się bardzo trudne kodu.</span><span class="sxs-lookup"><span data-stu-id="687e5-169">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="687e5-170">Zamiast tego zazwyczaj użyjemy pliku szablonu osobny widok ułatwiający Generowanie odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="687e5-170">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="687e5-171">Przyjrzyjmy się dalej, w jaki sposób możemy to zrobić.</span><span class="sxs-lookup"><span data-stu-id="687e5-171">Let's look next at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="687e5-172">[Poprzednie](getting-started.md)
> [dalej](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="687e5-172">[Previous](getting-started.md)
[Next](adding-a-view.md)</span></span>
