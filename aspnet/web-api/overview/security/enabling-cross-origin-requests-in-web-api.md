---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Włączanie żądań Cross-Origin we wzorcu ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: Pokazuje sposób obsługi udostępniania zasobów między źródłami (CORS) w interfejsie API sieci Web platformy ASP.NET.
ms.author: riande
ms.date: 07/15/2014
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: dc95c39af0821c2f456f5a312de5532c5aeb3c10
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912205"
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="16432-103">Włączanie żądań Cross-Origin we wzorcu ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="16432-103">Enabling Cross-Origin Requests in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="16432-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="16432-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="16432-105">Poziom zabezpieczeń przeglądarki uniemożliwia strony sieci web wprowadzanie wysyłanie żądań AJAX do innej domeny.</span><span class="sxs-lookup"><span data-stu-id="16432-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="16432-106">To ograniczenie jest nazywany *zasadami tego samego źródła*i zapobiega złośliwych witryn odczytywanie poufnych danych z innej lokacji.</span><span class="sxs-lookup"><span data-stu-id="16432-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="16432-107">Jednak czasami możesz chcieć umożliwić innych witryn, wywołania interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="16432-107">However, sometimes you might want to let other sites call your web API.</span></span>
> 
> <span data-ttu-id="16432-108">[Krzyżowe współużytkowanie zasobów](http://www.w3.org/TR/cors/) (między źródłami CORS) jest to standard W3C, dzięki któremu serwer może Poluzować zasady tego samego źródła.</span><span class="sxs-lookup"><span data-stu-id="16432-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="16432-109">Przy użyciu mechanizmu CORS, serwer można jawnie zezwolić na niektórych żądań cross-origin jednocześnie odrzucając inne.</span><span class="sxs-lookup"><span data-stu-id="16432-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="16432-110">CORS to bezpieczniejsze i bardziej elastyczne niż wcześniej techniki takie jak [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="16432-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="16432-111">W tym samouczku pokazano, jak włączyć mechanizm CORS w aplikacji interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="16432-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="16432-112">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="16432-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="16432-113">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="16432-113">Visual Studio 2013 Update 2</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="16432-114">Składnik Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="16432-114">Web API 2.2</span></span>


<a id="intro"></a>
## <a name="introduction"></a><span data-ttu-id="16432-115">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="16432-115">Introduction</span></span>

<span data-ttu-id="16432-116">Ten samouczek pokazuje, że obsługa mechanizmu CORS w Web API platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="16432-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="16432-117">Zaczniemy od utworzenia dwóch projektów platformy ASP.NET — jedną o nazwie "Usługa sieci Web", który hostuje kontrolera interfejsu API sieci Web, a inne o nazwie "Usługa WebClient", która wywołuje usługę sieci Web.</span><span class="sxs-lookup"><span data-stu-id="16432-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="16432-118">Ponieważ dwie aplikacje są przechowywane w różnych domenach, żądanie AJAX z WebClient Usługa sieci Web jest żądaniem cross-origin.</span><span class="sxs-lookup"><span data-stu-id="16432-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="16432-119">Co to jest "Tego samego Origin"?</span><span class="sxs-lookup"><span data-stu-id="16432-119">What is "Same Origin"?</span></span>

<span data-ttu-id="16432-120">Dwa adresy URL mają tego samego źródła, jeśli mają identyczne schematy, hosty i portów.</span><span class="sxs-lookup"><span data-stu-id="16432-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="16432-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="16432-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="16432-122">Te dwa adresy URL są tego samego źródła:</span><span class="sxs-lookup"><span data-stu-id="16432-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="16432-123">Te adresy URL są źródła innego niż poprzednie dwa:</span><span class="sxs-lookup"><span data-stu-id="16432-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="16432-124">`http://example.net` -Innej domeny</span><span class="sxs-lookup"><span data-stu-id="16432-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="16432-125">`http://example.com:9000/foo.html` -Innego portu</span><span class="sxs-lookup"><span data-stu-id="16432-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="16432-126">`https://example.com/foo.html` -Innego schematu</span><span class="sxs-lookup"><span data-stu-id="16432-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="16432-127">`http://www.example.com/foo.html` — Różne domeny podrzędnej</span><span class="sxs-lookup"><span data-stu-id="16432-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="16432-128">Program Internet Explorer nie należy wziąć pod uwagę portu podczas porównywania źródeł.</span><span class="sxs-lookup"><span data-stu-id="16432-128">Internet Explorer does not consider the port when comparing origins.</span></span>


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a><span data-ttu-id="16432-129">Utwórz projekt usługi sieci Web</span><span class="sxs-lookup"><span data-stu-id="16432-129">Create the WebService Project</span></span>

> [!NOTE]
> <span data-ttu-id="16432-130">W tej sekcji założono, że użytkownik wie już, jak tworzyć projekty interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="16432-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="16432-131">Jeśli nie, zobacz [wprowadzenie do interfejsu API sieci Web platformy ASP.NET](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="16432-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>


<span data-ttu-id="16432-132">Uruchom program Visual Studio i Utwórz nowy **aplikacji sieci Web ASP.NET** projektu.</span><span class="sxs-lookup"><span data-stu-id="16432-132">Start Visual Studio and create a new **ASP.NET Web Application** project.</span></span> <span data-ttu-id="16432-133">Wybierz **pusty** szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="16432-133">Select the **Empty** project template.</span></span> <span data-ttu-id="16432-134">W obszarze "Dodaj foldery i podstawowe odwołania dla" Wybierz **interfejsu API sieci Web** pola wyboru.</span><span class="sxs-lookup"><span data-stu-id="16432-134">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="16432-135">Opcjonalnie wybierz opcję "Hostuj w chmurze", aby wdrożyć aplikację pakietu Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="16432-135">Optionally, select the "Host in Cloud" option to deploy the app to Mircosoft Azure.</span></span> <span data-ttu-id="16432-136">Firma Microsoft oferuje bezpłatny internetowy hostowanie do 10 witryn sieci Web w [bezpłatne konto wersji próbnej platformy Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="16432-136">Microsoft offers free web hosting for up to 10 websites in a [free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

<span data-ttu-id="16432-137">Dodawanie kontrolera interfejsu API sieci Web o nazwie `TestController` następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="16432-137">Add a Web API controller named `TestController` with the following code:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

<span data-ttu-id="16432-138">Można uruchomić aplikację lokalnie, lub Wdróż na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="16432-138">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="16432-139">(Zrzuty ekranu, w tym samouczku, czy wdrożenie w usłudze Azure App Service Web Apps.) Aby sprawdzić, czy działa interfejs API sieci web, przejdź do `http://hostname/api/test/`, gdzie *hostname* jest domeną, w których wdrożono aplikację.</span><span class="sxs-lookup"><span data-stu-id="16432-139">(For the screenshots in this tutorial, I deployed to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="16432-140">Powinien zostać wyświetlony tekst odpowiedzi &quot;Uzyskaj: wiadomość testową&quot;.</span><span class="sxs-lookup"><span data-stu-id="16432-140">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a><span data-ttu-id="16432-141">Utwórz projekt WebClient</span><span class="sxs-lookup"><span data-stu-id="16432-141">Create the WebClient Project</span></span>

<span data-ttu-id="16432-142">Utwórz inny projekt aplikacji sieci Web ASP.NET i wybierz **MVC** szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="16432-142">Create another ASP.NET Web Application project and select the **MVC** project template.</span></span> <span data-ttu-id="16432-143">Opcjonalnie można zaznaczyć **Zmień uwierzytelnianie** > **bez uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="16432-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="16432-144">Nie musisz uwierzytelniania na potrzeby tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="16432-144">You don't need authentication for this tutorial.</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

<span data-ttu-id="16432-145">W Eksploratorze rozwiązań Otwórz plik Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="16432-145">In Solution Explorer, open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="16432-146">Zastąp kod w tym pliku zgodnie z poniższym przykładem:</span><span class="sxs-lookup"><span data-stu-id="16432-146">Replace the code in this file with the following:</span></span>

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

<span data-ttu-id="16432-147">Aby uzyskać *serviceUrl* zmiennej, użyj identyfikatora URI aplikacji usługi sieci Web.</span><span class="sxs-lookup"><span data-stu-id="16432-147">For the *serviceUrl* variable, use the URI of the WebService app.</span></span> <span data-ttu-id="16432-148">Teraz uruchom aplikację modułu WebClient lokalnie lub opublikować ją do innej witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="16432-148">Now run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="16432-149">Kliknięcie przycisku "Try It" przesyła żądanie AJAX do aplikacji usługi sieci Web, za pomocą metody HTTP wymienionych w polu listy rozwijanej (GET, POST lub PUT).</span><span class="sxs-lookup"><span data-stu-id="16432-149">Clicking the "Try It" button submits an AJAX request to the WebService app, using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="16432-150">Pozwala to sprawdzić różne żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="16432-150">This lets us examine different cross-origin requests.</span></span> <span data-ttu-id="16432-151">W tej chwili, aplikacja usługi sieci Web nie obsługuje mechanizm CORS, więc jeśli klikniesz przycisk, otrzymają komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="16432-151">Right now, the WebService app does not support CORS, so if you click the button, you will get an error.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="16432-152">Jeśli możesz obejrzeć ruch HTTP w narzędziu [Fiddler](http://www.telerik.com/fiddler), zostanie wyświetlony w przeglądarce wysyłania żądania GET i żądanie zakończy się powodzeniem, że wywołanie AJAX zwraca błąd.</span><span class="sxs-lookup"><span data-stu-id="16432-152">If you watch the HTTP traffic in a tool like [Fiddler](http://www.telerik.com/fiddler), you will see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="16432-153">Jest ważne zrozumieć, zasadami jednego źródła, nie uniemożliwia przeglądarki z *wysyłania* żądania.</span><span class="sxs-lookup"><span data-stu-id="16432-153">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="16432-154">Zamiast tego należy go zapobiega aplikację widzisz *odpowiedzi*.</span><span class="sxs-lookup"><span data-stu-id="16432-154">Instead, it prevents the application from seeing the *response*.</span></span>


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a><span data-ttu-id="16432-155">Włączanie mechanizmu CORS</span><span class="sxs-lookup"><span data-stu-id="16432-155">Enable CORS</span></span>

<span data-ttu-id="16432-156">Teraz możemy włączyć mechanizm CORS w aplikacji usługi sieci Web.</span><span class="sxs-lookup"><span data-stu-id="16432-156">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="16432-157">Najpierw Dodaj pakiet NuGet mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="16432-157">First, add the CORS NuGet package.</span></span> <span data-ttu-id="16432-158">W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet**, a następnie wybierz **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="16432-158">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="16432-159">W oknie Konsola Menedżera pakietów wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="16432-159">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="16432-160">To polecenie instaluje najnowszy pakiet i aktualizuje wszystkie zależności, w tym podstawowe biblioteki interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="16432-160">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="16432-161">Użytkownik flagi wersji, pod kątem określonej wersji.</span><span class="sxs-lookup"><span data-stu-id="16432-161">User the -Version flag to target a specific version.</span></span> <span data-ttu-id="16432-162">Pakiet CORS wymaga internetowego interfejsu API w wersji 2.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="16432-162">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="16432-163">Otwórz plik App\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="16432-163">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="16432-164">Dodaj następujący kod do **WebApiConfig.Register** metody.</span><span class="sxs-lookup"><span data-stu-id="16432-164">Add the following code to the **WebApiConfig.Register** method.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="16432-165">Następnie dodaj **[EnableCors]** atrybutu `TestController` klasy:</span><span class="sxs-lookup"><span data-stu-id="16432-165">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="16432-166">Aby uzyskać *źródła* parametru, użyj identyfikatora URI, do której została wdrożona aplikacja klient sieci Web.</span><span class="sxs-lookup"><span data-stu-id="16432-166">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="16432-167">Dzięki temu żądań cross-origin z WebClient, podczas nadal nie można przydzielać wszystkich innych żądaniach między domenami.</span><span class="sxs-lookup"><span data-stu-id="16432-167">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="16432-168">Później będziesz opisano parametry **[EnableCors]** bardziej szczegółowo.</span><span class="sxs-lookup"><span data-stu-id="16432-168">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="16432-169">Nie dołączaj ukośnika na końcu *źródła* adresu URL.</span><span class="sxs-lookup"><span data-stu-id="16432-169">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="16432-170">Ponownie wdróż zaktualizowaną aplikację usługi sieci Web.</span><span class="sxs-lookup"><span data-stu-id="16432-170">Redeploy the updated WebService application.</span></span> <span data-ttu-id="16432-171">Nie musisz zaktualizować WebClient.</span><span class="sxs-lookup"><span data-stu-id="16432-171">You don't need to update WebClient.</span></span> <span data-ttu-id="16432-172">Teraz żądanie AJAX z WebClient powinna zakończyć się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="16432-172">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="16432-173">Metody GET, PUT i POST wszystkie dozwolone.</span><span class="sxs-lookup"><span data-stu-id="16432-173">The GET, PUT, and POST methods are all allowed.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a><span data-ttu-id="16432-174">Jak działa mechanizmu CORS</span><span class="sxs-lookup"><span data-stu-id="16432-174">How CORS Works</span></span>

<span data-ttu-id="16432-175">W tej sekcji opisano, co się dzieje w żądaniu CORS, na poziomie wiadomości HTTP.</span><span class="sxs-lookup"><span data-stu-id="16432-175">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="16432-176">Jest ważne dowiedzieć się, jak działa mechanizm CORS, dzięki czemu można skonfigurować **[EnableCors]** atrybutu prawidłowo i rozwiązywanie problemów, jeśli elementy nie działają zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="16432-176">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly, and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="16432-177">Specyfikacja CORS wprowadza kilka nowych nagłówki HTTP, które Włączanie żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="16432-177">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="16432-178">Jeśli przeglądarka obsługuje mechanizm CORS, ustawia nagłówki te automatycznie dla żądań cross-origin; nie trzeba wykonywać żadnych specjalnych w kodzie JavaScript czynności.</span><span class="sxs-lookup"><span data-stu-id="16432-178">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="16432-179">Oto przykład żądania między źródłami.</span><span class="sxs-lookup"><span data-stu-id="16432-179">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="16432-180">Nagłówek "Origin" zapewnia domeny lokacji, który wysłał żądanie.</span><span class="sxs-lookup"><span data-stu-id="16432-180">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="16432-181">Jeśli serwer zezwala na żądanie, ustawia dla nagłówka Access-Control-Allow-Origin.</span><span class="sxs-lookup"><span data-stu-id="16432-181">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="16432-182">Wartość tego nagłówka stanowi odpowiada nagłówek źródła albo ma wartość symbolu wieloznacznego "\*", co oznacza, że wszystkie źródła jest dozwolone.</span><span class="sxs-lookup"><span data-stu-id="16432-182">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="16432-183">Odpowiedź nie zawiera nagłówka Access-Control-Allow-Origin, żądanie AJAX nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="16432-183">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="16432-184">W szczególności przeglądarki nie zezwalają na żądanie.</span><span class="sxs-lookup"><span data-stu-id="16432-184">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="16432-185">Nawet jeśli serwer zwraca odpowiedź oznaczająca Powodzenie, przeglądarka nie udostępnić odpowiedź do aplikacji klienckiej.</span><span class="sxs-lookup"><span data-stu-id="16432-185">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="16432-186">**Żądania wstępnego**</span><span class="sxs-lookup"><span data-stu-id="16432-186">**Preflight Requests**</span></span>

<span data-ttu-id="16432-187">W przypadku niektórych żądań CORPS przeglądarki przesyła wysłanie dodatkowego żądania o nazwie "żądania wstępnego", przed wysłaniem rzeczywistego żądania dla zasobu.</span><span class="sxs-lookup"><span data-stu-id="16432-187">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="16432-188">Przeglądarka może pominąć żądania wstępnego, jeśli są spełnione następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="16432-188">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="16432-189">Metoda żądania jest GET, HEAD lub WPIS, *i*</span><span class="sxs-lookup"><span data-stu-id="16432-189">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="16432-190">Aplikacja nie ustawia wszystkie nagłówki żądania innego niż zawartości Accept, Accept-Language-Language, Content-Type lub ostatni-Event-ID *i*</span><span class="sxs-lookup"><span data-stu-id="16432-190">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="16432-191">Nagłówek Content-Type (jeśli ustawić) jest jednym z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="16432-191">The Content-Type header (if set) is one of the following:</span></span> 

    - <span data-ttu-id="16432-192">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="16432-192">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="16432-193">multipart/formularza data</span><span class="sxs-lookup"><span data-stu-id="16432-193">multipart/form-data</span></span>
    - <span data-ttu-id="16432-194">zwykły tekst</span><span class="sxs-lookup"><span data-stu-id="16432-194">text/plain</span></span>

<span data-ttu-id="16432-195">Reguła o nagłówków żądań ma zastosowanie do nagłówków, które aplikacja ustawia przez wywołanie metody **setRequestHeader** na **XMLHttpRequest** obiektu.</span><span class="sxs-lookup"><span data-stu-id="16432-195">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="16432-196">(Specyfikacji CORS wywołuje te "Autor nagłówki żądania"). Reguła nie ma zastosowania do nagłówków *przeglądarki* można ustawić, np. Agent użytkownika, Host lub Content-Length.</span><span class="sxs-lookup"><span data-stu-id="16432-196">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="16432-197">Oto przykład żądania wstępnego:</span><span class="sxs-lookup"><span data-stu-id="16432-197">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="16432-198">Żądanie krótkiej wykorzystuje metodę OPTIONS protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="16432-198">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="16432-199">Zawiera ona dwie specjalnych nagłówków:</span><span class="sxs-lookup"><span data-stu-id="16432-199">It includes two special headers:</span></span>

- <span data-ttu-id="16432-200">Access-Control-Request-Method: Metoda HTTP, która będzie służyć do rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="16432-200">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="16432-201">Access-Control-Request-Headers: Listę nagłówków żądań, *aplikacji* nastavit rzeczywistego żądania.</span><span class="sxs-lookup"><span data-stu-id="16432-201">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="16432-202">(Ponownie, to nie zawiera nagłówki, które ustawia przeglądarki.)</span><span class="sxs-lookup"><span data-stu-id="16432-202">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="16432-203">Poniżej przedstawiono przykładową odpowiedź, przy założeniu, że serwer zezwala na żądanie:</span><span class="sxs-lookup"><span data-stu-id="16432-203">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="16432-204">Odpowiedź zawiera nagłówek dostępu — kontrola-Allow-Methods, zawierającego dozwolone metody i, opcjonalnie nagłówka Access-Control-Zezwalaj-Headers, który zawiera listę dozwolonych nagłówków.</span><span class="sxs-lookup"><span data-stu-id="16432-204">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="16432-205">Jeśli żądania wstępnego zakończy się powodzeniem, przeglądarka wysyła rzeczywistego żądania, zgodnie z wcześniejszym opisem.</span><span class="sxs-lookup"><span data-stu-id="16432-205">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="16432-206">Zakres reguły [EnableCors]</span><span class="sxs-lookup"><span data-stu-id="16432-206">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="16432-207">W aplikacji, można włączyć mechanizm CORS każdej akcji, każdy kontroler lub globalnie dla wszystkich kontrolerów składnika Web API.</span><span class="sxs-lookup"><span data-stu-id="16432-207">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="16432-208">**Za akcję**</span><span class="sxs-lookup"><span data-stu-id="16432-208">**Per Action**</span></span>

<span data-ttu-id="16432-209">Aby włączyć mechanizm CORS dla jednej akcji, należy ustawić **[EnableCors]** atrybutu metody akcji.</span><span class="sxs-lookup"><span data-stu-id="16432-209">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="16432-210">Poniższy przykład umożliwia włączenie mechanizmu CORS dla `GetItem` tylko metody.</span><span class="sxs-lookup"><span data-stu-id="16432-210">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="16432-211">**Każdy kontroler**</span><span class="sxs-lookup"><span data-stu-id="16432-211">**Per Controller**</span></span>

<span data-ttu-id="16432-212">Jeśli ustawisz **[EnableCors]** klasy kontrolera ma zastosowanie do wszystkich akcji w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="16432-212">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="16432-213">Aby wyłączyć CORS dla akcji, należy dodać **[DisableCors]** atrybutu akcji.</span><span class="sxs-lookup"><span data-stu-id="16432-213">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="16432-214">Poniższy przykład umożliwia CORS dla każdej metody, z wyjątkiem `PutItem`.</span><span class="sxs-lookup"><span data-stu-id="16432-214">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="16432-215">**Globalnie**</span><span class="sxs-lookup"><span data-stu-id="16432-215">**Globally**</span></span>

<span data-ttu-id="16432-216">Aby włączyć mechanizm CORS dla wszystkich kontrolerów składnika Web API w aplikacji, należy przekazać **EnableCorsAttribute** wystąpienia do **EnableCors** metody:</span><span class="sxs-lookup"><span data-stu-id="16432-216">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="16432-217">Jeśli ten atrybut zostanie ustawiony na więcej niż jednego zakresu, jest kolejność według priorytetu:</span><span class="sxs-lookup"><span data-stu-id="16432-217">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="16432-218">Akcja</span><span class="sxs-lookup"><span data-stu-id="16432-218">Action</span></span>
2. <span data-ttu-id="16432-219">Kontroler</span><span class="sxs-lookup"><span data-stu-id="16432-219">Controller</span></span>
3. <span data-ttu-id="16432-220">Global</span><span class="sxs-lookup"><span data-stu-id="16432-220">Global</span></span>

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a><span data-ttu-id="16432-221">Ustaw dozwolone źródła</span><span class="sxs-lookup"><span data-stu-id="16432-221">Set the Allowed Origins</span></span>

<span data-ttu-id="16432-222">*Źródła* parametru **[EnableCors]** atrybut określa, które źródła są dozwolone dostępu do zasobu.</span><span class="sxs-lookup"><span data-stu-id="16432-222">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="16432-223">Wartość jest rozdzielaną przecinkami listę dozwolonych źródeł.</span><span class="sxs-lookup"><span data-stu-id="16432-223">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="16432-224">Możesz także użyć wartości symbolu wieloznacznego "\*" Aby zezwolić na żądania z dowolnego źródła.</span><span class="sxs-lookup"><span data-stu-id="16432-224">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="16432-225">Starannie rozważ, przed zezwoleniem na żądania z dowolnego źródła.</span><span class="sxs-lookup"><span data-stu-id="16432-225">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="16432-226">Oznacza to, że dosłownie w dowolnej witrynie sieci Web może wykonywać wywołania AJAX, do interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="16432-226">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="16432-227">Ustaw metody HTTP dozwolonych</span><span class="sxs-lookup"><span data-stu-id="16432-227">Set the Allowed HTTP Methods</span></span>

<span data-ttu-id="16432-228">*Metody* parametru **[EnableCors]** atrybut określa metody HTTP, które mogą uzyskać dostępu do zasobu.</span><span class="sxs-lookup"><span data-stu-id="16432-228">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="16432-229">Aby zezwolić na wszystkie metody, należy użyć wartości symbolu wieloznacznego "\*".</span><span class="sxs-lookup"><span data-stu-id="16432-229">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="16432-230">Poniższy przykład umożliwia tylko żądania GET i POST.</span><span class="sxs-lookup"><span data-stu-id="16432-230">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="16432-231">Ustawia nagłówki żądania dozwolone</span><span class="sxs-lookup"><span data-stu-id="16432-231">Set the Allowed Request Headers</span></span>

<span data-ttu-id="16432-232">Wcześniej I opisano sposób żądania wstępnego mogą obejmować nagłówka Access-Control-Request-Headers lista nagłówków HTTP, ustawiane przez aplikację (tak zwane "Autor nagłówki żądania").</span><span class="sxs-lookup"><span data-stu-id="16432-232">Earlier I described how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="16432-233">*Nagłówki* parametru **[EnableCors]** atrybut określa, które autor nagłówki żądania są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="16432-233">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="16432-234">Aby zezwolić na wszystkie nagłówki, *nagłówki* do "\*".</span><span class="sxs-lookup"><span data-stu-id="16432-234">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="16432-235">Aby nagłówki określonej listy dozwolonych adresów, należy ustawić *nagłówki* do listy dozwolone nagłówki oddzielone przecinkami:</span><span class="sxs-lookup"><span data-stu-id="16432-235">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="16432-236">Jednak przeglądarki nie są całkowicie zgodne, w jaki ustawiają Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="16432-236">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="16432-237">Na przykład w przeglądarce Chrome zawiera obecnie "origin"; gdy FireFox nie ma standardowych nagłówków, takie jak "Akceptuj", nawet wtedy, gdy aplikacja ustawia je w skrypcie.</span><span class="sxs-lookup"><span data-stu-id="16432-237">For example, Chrome currently includes "origin"; while FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="16432-238">Jeśli ustawisz *nagłówki* do żadnego elementu innego niż "\*", użytkownik powinien zawierać co najmniej "Zaakceptuj", "content-type" i "origin", a także niestandardowe nagłówki, które mają być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="16432-238">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="16432-239">Ustaw dozwolonych nagłówków odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="16432-239">Set the Allowed Response Headers</span></span>

<span data-ttu-id="16432-240">Domyślnie przeglądarka nie uwidacznia wszystkie nagłówki odpowiedzi do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="16432-240">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="16432-241">Nagłówki odpowiedzi, które są domyślnie dostępne są następujące:</span><span class="sxs-lookup"><span data-stu-id="16432-241">The response headers that are available by default are:</span></span>

- <span data-ttu-id="16432-242">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="16432-242">Cache-Control</span></span>
- <span data-ttu-id="16432-243">Język zawartości</span><span class="sxs-lookup"><span data-stu-id="16432-243">Content-Language</span></span>
- <span data-ttu-id="16432-244">Typ zawartości</span><span class="sxs-lookup"><span data-stu-id="16432-244">Content-Type</span></span>
- <span data-ttu-id="16432-245">Wygasa</span><span class="sxs-lookup"><span data-stu-id="16432-245">Expires</span></span>
- <span data-ttu-id="16432-246">Last-Modified</span><span class="sxs-lookup"><span data-stu-id="16432-246">Last-Modified</span></span>
- <span data-ttu-id="16432-247">Dyrektywy pragma</span><span class="sxs-lookup"><span data-stu-id="16432-247">Pragma</span></span>

<span data-ttu-id="16432-248">Specyfikacja CORS wywołuje te [nagłówki odpowiedzi proste](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="16432-248">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="16432-249">Aby udostępnić innych nagłówków do aplikacji, należy ustawić *exposedHeaders* parametru **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="16432-249">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="16432-250">W poniższym przykładzie, kontroler firmy `Get` metoda ustawia niestandardowy nagłówek o nazwie "X-Custom-Header".</span><span class="sxs-lookup"><span data-stu-id="16432-250">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="16432-251">Domyślnie przeglądarka nie udostępni tego nagłówka w żądaniu cross-origin.</span><span class="sxs-lookup"><span data-stu-id="16432-251">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="16432-252">Aby udostępnić nagłówka, należy dołączyć "X-Custom-Header" *exposedHeaders*.</span><span class="sxs-lookup"><span data-stu-id="16432-252">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a><span data-ttu-id="16432-253">Przekazywanie poświadczeń w żądań Cross-Origin</span><span class="sxs-lookup"><span data-stu-id="16432-253">Passing Credentials in Cross-Origin Requests</span></span>

<span data-ttu-id="16432-254">Poświadczenia wymagają specjalnej obsługi żądania CORS.</span><span class="sxs-lookup"><span data-stu-id="16432-254">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="16432-255">Domyślnie przeglądarka nie wysyła żadnych poświadczeń z żądaniem cross-origin.</span><span class="sxs-lookup"><span data-stu-id="16432-255">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="16432-256">Poświadczenia obejmują pliki cookie, a także schematów uwierzytelniania HTTP.</span><span class="sxs-lookup"><span data-stu-id="16432-256">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="16432-257">Do wysyłania poświadczeń z żądaniem cross-origin, klient musi być ustawiona **XMLHttpRequest.withCredentials** na wartość true.</span><span class="sxs-lookup"><span data-stu-id="16432-257">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="16432-258">Za pomocą **XMLHttpRequest** bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="16432-258">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="16432-259">W technologii jQuery:</span><span class="sxs-lookup"><span data-stu-id="16432-259">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="16432-260">Ponadto serwer musi zezwalać na poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="16432-260">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="16432-261">Aby zezwolić na poświadczenia cross-origin w interfejsie API sieci Web, należy ustawić **SupportsCredentials** właściwości na wartość TRUE **[EnableCors]** atrybutu:</span><span class="sxs-lookup"><span data-stu-id="16432-261">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="16432-262">Jeśli ta właściwość ma wartość true, odpowiedź HTTP będzie zawierać nagłówek dostępu — kontrola-Allow-Credentials.</span><span class="sxs-lookup"><span data-stu-id="16432-262">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="16432-263">Ten nagłówek informuje przeglądarkę o serwer zezwala na poświadczenia dla żądań cross-origin.</span><span class="sxs-lookup"><span data-stu-id="16432-263">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="16432-264">Jeśli przeglądarka wysyła poświadczenia, ale odpowiedź nie zawiera prawidłowego nagłówka Access-kontroli-Allow-Credentials, przeglądarka nie będzie ujawniać odpowiedź do aplikacji, a żądanie AJAX nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="16432-264">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="16432-265">Należy zachować ostrożność bardzo ustawienie **SupportsCredentials** na wartość true, ponieważ oznacza witryny sieci Web w innej domenie może wysyłać poświadczeń zalogowanego użytkownika do internetowego interfejsu API w imieniu użytkownika, bez angażowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="16432-265">Be very careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="16432-266">Specyfikacja CORS również stany to ustawienie *źródła* do &quot; \* &quot; jest nieprawidłowa Jeśli **SupportsCredentials** ma wartość true.</span><span class="sxs-lookup"><span data-stu-id="16432-266">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a><span data-ttu-id="16432-267">Dostawcy zasad niestandardowego mechanizmu CORS</span><span class="sxs-lookup"><span data-stu-id="16432-267">Custom CORS Policy Providers</span></span>

<span data-ttu-id="16432-268">**[EnableCors]** atrybutu implementuje **ICorsPolicyProvider** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="16432-268">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="16432-269">Możesz podać Twojej własnej implementacji, tworząc klasę, która pochodzi od klasy **atrybut** i implementuje **ICorsProlicyProvider**.</span><span class="sxs-lookup"><span data-stu-id="16432-269">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsProlicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="16432-270">Teraz można zastosować atrybut, w dowolnym miejscu, że możesz umieścić **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="16432-270">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="16432-271">Na przykład niestandardowy Dostawca zasad CORS można odczytać ustawień z pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="16432-271">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="16432-272">Jako alternatywa przy użyciu atrybutów, możesz zarejestrować się **ICorsPolicyProviderFactory** obiektu, który tworzy **ICorsPolicyProvider** obiektów.</span><span class="sxs-lookup"><span data-stu-id="16432-272">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="16432-273">Aby ustawić **ICorsPolicyProviderFactory**, wywołania **SetCorsPolicyProviderFactory** — metoda rozszerzenia przy uruchamianiu w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="16432-273">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a><span data-ttu-id="16432-274">Obsługa przeglądarek</span><span class="sxs-lookup"><span data-stu-id="16432-274">Browser Support</span></span>

<span data-ttu-id="16432-275">Pakietów mechanizmu CORS interfejsu API sieci Web to technologia po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="16432-275">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="16432-276">Przeglądarka musi także obsługę mechanizmu CORS.</span><span class="sxs-lookup"><span data-stu-id="16432-276">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="16432-277">Na szczęście zawierają aktualne wersje wszystkie popularne przeglądarki [Obsługa mechanizmu CORS](http://caniuse.com/cors).</span><span class="sxs-lookup"><span data-stu-id="16432-277">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>

<span data-ttu-id="16432-278">Program Internet Explorer 8 oraz Internet Explorer 9 mają częściowa Obsługa mechanizmu CORS za pomocą starszej wersji obiektu XDomainRequest zamiast XMLHttpRequest.</span><span class="sxs-lookup"><span data-stu-id="16432-278">Internet Explorer 8 and Internet Explorer 9 have partial support for CORS, using the legacy XDomainRequest object instead of XMLHttpRequest.</span></span> <span data-ttu-id="16432-279">Aby uzyskać więcej informacji, zobacz [XDomainRequest — ograniczenia, ograniczeń i obejścia](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span><span class="sxs-lookup"><span data-stu-id="16432-279">For more information, see [XDomainRequest - Restrictions, Limitations and Workarounds](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span></span>
