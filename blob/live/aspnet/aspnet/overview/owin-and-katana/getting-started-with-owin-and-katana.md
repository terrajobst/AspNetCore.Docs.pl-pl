---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Wprowadzenie do korzystania z OWIN i Katana | Dokumentacja firmy Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/27/2013
ms.topic: article
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 8922aada723da9b149ec111902fcd883c8241dfb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="bef9e-102">Wprowadzenie do korzystania z OWIN i Katana</span><span class="sxs-lookup"><span data-stu-id="bef9e-102">Getting Started with OWIN and Katana</span></span>
====================
<span data-ttu-id="bef9e-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bef9e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="bef9e-104">[Otwórz interfejs sieci Web dla platformy .NET (OWIN)](http://owin.org/) definiuje abstrakcję między serwerami sieci web .NET i aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="bef9e-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="bef9e-105">Dzięki rozdzieleniu serwera sieci web z aplikacji, OWIN ułatwia tworzenie oprogramowania pośredniczącego do tworzenia aplikacji sieci web platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="bef9e-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="bef9e-106">Ponadto OWIN ułatwia portu aplikacji sieci web w innych hostów &#8212; na przykład hostingu samodzielnego usługi systemu Windows lub inny proces.</span><span class="sxs-lookup"><span data-stu-id="bef9e-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="bef9e-107">OWIN jest specyfikacją należących do społeczności, nie implementację.</span><span class="sxs-lookup"><span data-stu-id="bef9e-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="bef9e-108">Projekt Katana jest zestaw składników OWIN open source opracowany przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bef9e-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="bef9e-109">Aby uzyskać ogólne OWIN i Katana, zobacz [Omówienie projektu Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="bef9e-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="bef9e-110">W tym artykule będzie I przejść bezpośrednio do kodu, aby rozpocząć pracę.</span><span class="sxs-lookup"><span data-stu-id="bef9e-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="bef9e-111">W tym samouczku używana [programu Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), ale może również używać programu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="bef9e-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="bef9e-112">Kilka kroków różnią się w programie Visual Studio 2012, który I uwaga poniżej.</span><span class="sxs-lookup"><span data-stu-id="bef9e-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="bef9e-113">Host OWIN w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="bef9e-113">Host OWIN in IIS</span></span>

<span data-ttu-id="bef9e-114">W tej sekcji firma Microsoft będzie udostępniać OWIN w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="bef9e-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="bef9e-115">Ta opcja zapewnia elastyczność i możliwości potoku OWIN wraz z zestawu dojrzałe funkcji usług IIS.</span><span class="sxs-lookup"><span data-stu-id="bef9e-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="bef9e-116">Aplikacja OWIN za pomocą tej opcji, działa w potoku żądania ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bef9e-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="bef9e-117">Najpierw utwórz nowy projekt aplikacji sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bef9e-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="bef9e-118">(W programie Visual Studio 2012, użyj typu projektu pusta aplikacja sieci Web ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="bef9e-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="bef9e-119">W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **pusty** szablonu.</span><span class="sxs-lookup"><span data-stu-id="bef9e-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="bef9e-120">Dodawanie pakietów NuGet</span><span class="sxs-lookup"><span data-stu-id="bef9e-120">Add NuGet Packages</span></span>

<span data-ttu-id="bef9e-121">Następnie dodaj wymagane pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="bef9e-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="bef9e-122">Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="bef9e-122">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="bef9e-123">W oknie Konsola Menedżera pakietów wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="bef9e-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="bef9e-124">Dodaj klasę uruchamiania</span><span class="sxs-lookup"><span data-stu-id="bef9e-124">Add a Startup Class</span></span>

<span data-ttu-id="bef9e-125">Następnie Dodaj klasę początkową OWIN.</span><span class="sxs-lookup"><span data-stu-id="bef9e-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="bef9e-126">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj**, a następnie wybierz pozycję **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="bef9e-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="bef9e-127">W **Dodaj nowy element** okno dialogowe, wybierz opcję **klasy początkowej Owin**.</span><span class="sxs-lookup"><span data-stu-id="bef9e-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="bef9e-128">Aby uzyskać więcej informacji na temat konfigurowania Klasa początkowa, zobacz [OWIN uruchamiania klasy wykrywania](owin-startup-class-detection.md).</span><span class="sxs-lookup"><span data-stu-id="bef9e-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="bef9e-129">Dodaj następujący kod do `Startup1.Configuration` metody:</span><span class="sxs-lookup"><span data-stu-id="bef9e-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="bef9e-130">Ten kod dodaje prosty część oprogramowania pośredniczącego do potoku OWIN zaimplementowane jako funkcja, która odbiera **Microsoft.Owin.IOwinContext** wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="bef9e-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="bef9e-131">Kiedy serwer odbiera żądanie HTTP, potoku OWIN wywołuje oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="bef9e-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="bef9e-132">Oprogramowanie pośredniczące ustawia typ zawartości odpowiedzi i zapisuje treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bef9e-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="bef9e-133">Szablon klasy OWIN uruchomienia jest dostępne w programie Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="bef9e-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="bef9e-134">Jeśli używasz programu Visual Studio 2012, po prostu Dodaj nową klasę pusty o nazwie `Startup1`i wklej poniższy kod:</span><span class="sxs-lookup"><span data-stu-id="bef9e-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="bef9e-135">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="bef9e-135">Run the Application</span></span>

<span data-ttu-id="bef9e-136">Naciśnij klawisz F5, aby rozpocząć debugowanie.</span><span class="sxs-lookup"><span data-stu-id="bef9e-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="bef9e-137">Visual Studio zostanie otwarte okno przeglądarki na `http://localhost:*port*/`.</span><span class="sxs-lookup"><span data-stu-id="bef9e-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="bef9e-138">Strona powinna wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="bef9e-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="bef9e-139">Host samodzielny OWIN w aplikacji konsoli</span><span class="sxs-lookup"><span data-stu-id="bef9e-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="bef9e-140">To proste przekonwertować tę aplikację z hostowanie usług IIS do hostingu samodzielnego w procesie niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="bef9e-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="bef9e-141">Z hostowanie usług IIS, usługi IIS działa jako serwer HTTP, a procesu tego hosta serwera.</span><span class="sxs-lookup"><span data-stu-id="bef9e-141">With IIS hosting, IIS acts as both the HTTP server and as the process that host the sever.</span></span> <span data-ttu-id="bef9e-142">Z samodzielnej obsługi aplikacji tworzy proces i używa **HttpListener** klasy jako serwer HTTP.</span><span class="sxs-lookup"><span data-stu-id="bef9e-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="bef9e-143">W programie Visual Studio Utwórz nową aplikację konsoli.</span><span class="sxs-lookup"><span data-stu-id="bef9e-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="bef9e-144">W oknie Konsola Menedżera pakietów wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="bef9e-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="bef9e-145">Dodaj `Startup1` klasy z tego samouczka, część 1 do projektu.</span><span class="sxs-lookup"><span data-stu-id="bef9e-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="bef9e-146">Nie należy modyfikować tej klasy.</span><span class="sxs-lookup"><span data-stu-id="bef9e-146">You don't need to modify this class.</span></span>

<span data-ttu-id="bef9e-147">Wdrożenie aplikacji `Main` metody w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="bef9e-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="bef9e-148">Po uruchomieniu aplikacji konsoli serwera rozpoczyna nasłuchiwanie `http://localhost:9000`.</span><span class="sxs-lookup"><span data-stu-id="bef9e-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="bef9e-149">Jeśli przejdziesz do tego adresu w przeglądarce sieci web, zostanie wyświetlona strona "Hello world".</span><span class="sxs-lookup"><span data-stu-id="bef9e-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="bef9e-150">Dodaj OWIN Diagnostics</span><span class="sxs-lookup"><span data-stu-id="bef9e-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="bef9e-151">Pakietu Microsoft.Owin.Diagnostics zawiera oprogramowanie pośredniczące, który przechwytuje nieobsługiwanych wyjątków i wyświetlenie strony HTML przy użyciu szczegółów błędu.</span><span class="sxs-lookup"><span data-stu-id="bef9e-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="bef9e-152">Funkcje tej strony podobnie jak strona błędów programu ASP.NET, która jest czasem nazywany "[żółty ekran](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span><span class="sxs-lookup"><span data-stu-id="bef9e-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="bef9e-153">Podobnie jak YSOD stronę błędu Katana jest przydatne podczas programowania, ale jest dobrym rozwiązaniem, aby ją wyłączyć w trybie produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="bef9e-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="bef9e-154">Aby zainstalować pakiet diagnostyki w projekcie, wpisz następujące polecenie w oknie konsoli Menedżera pakietów:</span><span class="sxs-lookup"><span data-stu-id="bef9e-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="bef9e-155">Zmień kod w Twojej `Startup1.Configuration` metody w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bef9e-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="bef9e-156">Teraz używać CTRL + F5, aby uruchomić aplikację bez debugowania, tak, aby nie będę powodować utraty na wyjątek programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bef9e-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="bef9e-157">Aplikacja działa tak samo jak wcześniej, dopóki przejdź do `http://localhost/fail`, w którym aplikacja zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="bef9e-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="bef9e-158">Oprogramowanie pośredniczące strony błędu będzie catch wyjątku i wyświetlenia strony HTML z informacjami o tym błędzie.</span><span class="sxs-lookup"><span data-stu-id="bef9e-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="bef9e-159">Możesz kliknąć odpowiednie karty, aby zobaczyć stos, ciąg zapytania, plików cookie, nagłówek żądania i zmiennych środowiskowych OWIN.</span><span class="sxs-lookup"><span data-stu-id="bef9e-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="bef9e-160">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="bef9e-160">Next Steps</span></span>

- [<span data-ttu-id="bef9e-161">Wykrywanie klasy uruchamiania OWIN</span><span class="sxs-lookup"><span data-stu-id="bef9e-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="bef9e-162">Umożliwia hosta samodzielnego ASP.NET Web API OWIN</span><span class="sxs-lookup"><span data-stu-id="bef9e-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="bef9e-163">Host samodzielny SignalR za pomocą OWIN</span><span class="sxs-lookup"><span data-stu-id="bef9e-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
