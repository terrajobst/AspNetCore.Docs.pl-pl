---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: "Profile i debugowania aplikacji ASP.NET MVC za pomocą Glimpse | Dokumentacja firmy Microsoft"
author: Rick-Anderson
description: "Glimpse jest thriving i powiększania rodziny pakietów NuGet typu open source, który zapewnia szczegółowe wydajność, debugowanie i informacji diagnostycznych dla platformy ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 9cfdced21251b482ca527dda9c3a698de77cc8ca
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="2aea3-103">Profile i debugowania aplikacji ASP.NET MVC za pomocą Glimpse</span><span class="sxs-lookup"><span data-stu-id="2aea3-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="2aea3-104">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="2aea3-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="2aea3-105">Glimpse jest thriving i powiększania rodziny pakietów NuGet typu open source, który zapewnia szczegółowe wydajność, debugowanie i informacje diagnostyczne dotyczące aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2aea3-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="2aea3-106">Jest prosta do zainstalowania, lekkie, niezwykle szybka i przedstawia kluczowe metryki wydajności u dołu każdej strony.</span><span class="sxs-lookup"><span data-stu-id="2aea3-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="2aea3-107">Umożliwia przechodzenie w swojej aplikacji, gdy chcesz dowiedzieć się, co dzieje się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="2aea3-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="2aea3-108">Glimpse zapewnia dużo cenne informacje, które firma Microsoft zaleca się, że używasz przez cały cykl programowania, w tym środowiska testowego Azure.</span><span class="sxs-lookup"><span data-stu-id="2aea3-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="2aea3-109">Podczas [Fiddler](http://www.telerik.com/fiddler) i [narzędzi programistycznych F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) Podaj po stronie klienta widoku Glimpse zawiera widok szczegółowy z serwera.</span><span class="sxs-lookup"><span data-stu-id="2aea3-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="2aea3-110">Ten samouczek koncentruje się na użyciu Glimpse ASP.NET MVC i EF pakietów, ale dostępnych jest wiele innych pakietów.</span><span class="sxs-lookup"><span data-stu-id="2aea3-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="2aea3-111">W miarę możliwości I połączy się z odpowiednim [Glimpse docs](http://getglimpse.com/Docs/) którego pomoc I Obsługa.</span><span class="sxs-lookup"><span data-stu-id="2aea3-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="2aea3-112">Glimpse jest projekt typu open source, zbyt można współtworzyć kod źródłowy i dokumentacja.</span><span class="sxs-lookup"><span data-stu-id="2aea3-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="2aea3-113">Instalowanie Glimpse</span><span class="sxs-lookup"><span data-stu-id="2aea3-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="2aea3-114">Włącz Glimpse hosta lokalnego</span><span class="sxs-lookup"><span data-stu-id="2aea3-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="2aea3-115">Na karcie osi czasu</span><span class="sxs-lookup"><span data-stu-id="2aea3-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="2aea3-116">Wiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="2aea3-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="2aea3-117">Trasy</span><span class="sxs-lookup"><span data-stu-id="2aea3-117">Routes</span></span>](#route)
- [<span data-ttu-id="2aea3-118">Przy użyciu Glimpse na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="2aea3-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="2aea3-119">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2aea3-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="2aea3-120">Instalowanie Glimpse</span><span class="sxs-lookup"><span data-stu-id="2aea3-120">Installing Glimpse</span></span>

<span data-ttu-id="2aea3-121">Glimpse można zainstalować z konsoli Menedżera pakietów NuGet lub **Zarządzaj pakietami NuGet** konsoli.</span><span class="sxs-lookup"><span data-stu-id="2aea3-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="2aea3-122">Dla tego pokazu I Zainstaluj pakiety Mvc5 i EF6:</span><span class="sxs-lookup"><span data-stu-id="2aea3-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![Zainstaluj Glimpse z okno NuGet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="2aea3-124">Wyszukaj *Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="2aea3-124">Search for *Glimpse.EF*</span></span>

![Glimpse.EF z NuGet install dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="2aea3-126">Wybierając **zainstalowane pakiety**, widzą modułów zależnych Glimpse zainstalowane:</span><span class="sxs-lookup"><span data-stu-id="2aea3-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Zainstalowane pakiety Glimpse z okno dialogowe](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="2aea3-128">Następujące polecenia zainstalować moduły Glimpse MVC5 i EF6 z konsoli Menedżera pakietów:</span><span class="sxs-lookup"><span data-stu-id="2aea3-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="2aea3-129">Włącz Glimpse hosta lokalnego</span><span class="sxs-lookup"><span data-stu-id="2aea3-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="2aea3-130">Przejdź do http://localhost:&lt;portu #&gt;/glimpse.axd, a następnie kliknij przycisk **Włącz Glimpse** przycisku.</span><span class="sxs-lookup"><span data-stu-id="2aea3-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the **Turn Glimpse On** button.</span></span>

![Strona axd glimpse](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="2aea3-132">Masz paska ulubionych wyświetlane można przeciągnij i upuść przycisków Glimpse i dodać je jako bookmarklets:</span><span class="sxs-lookup"><span data-stu-id="2aea3-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![IE z Glimpse boookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="2aea3-134">Teraz można przechodzić aplikacji oraz **głowic zapasowej wyświetlania** (HUD) jest wyświetlany w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="2aea3-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Strona Menedżer kontakt z HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="2aea3-136">[Glimpse HUD strony](http://getglimpse.com/Docs/Heads-up-Display) szczegóły informacji przedstawionych powyżej.</span><span class="sxs-lookup"><span data-stu-id="2aea3-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="2aea3-137">Wyświetla dane HUD wydajności dyskretny kod może powiadomić o problemie natychmiast — przed uzyskanie cyklu testu.</span><span class="sxs-lookup"><span data-stu-id="2aea3-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="2aea3-138">Kliknięcie &quot;g&quot; w prawym dolnym rogu wyświetlenie panelu Glimpse:</span><span class="sxs-lookup"><span data-stu-id="2aea3-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Glimpse panel](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="2aea3-140">Na ilustracji powyżej [karcie wykonywanie](http://getglimpse.com/Docs/Execution-Tab) jest zaznaczone, który zawiera szczegóły dotyczące działań i filtry w potoku.</span><span class="sxs-lookup"><span data-stu-id="2aea3-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="2aea3-141">Widać Mój [czujki zatrzymać czasomierza filtru](http://www.nuget.org/packages/StopWatch/) start na etapie 6 potoku.</span><span class="sxs-lookup"><span data-stu-id="2aea3-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="2aea3-142">Gdy mój czasomierza lekkie zapewniają przydatne profilu/chronometrażu danych, jej chybień cały czas spędzony w autoryzacji i renderowania widoku.</span><span class="sxs-lookup"><span data-stu-id="2aea3-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="2aea3-143">Informacje o Moje czasomierza po [profilu i czas aplikacji ASP.NET MVC do Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="2aea3-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="2aea3-144">[Karty](http://getglimpse.com/Docs/Tabs) strona zawiera linki do szczegółowych informacji na poszczególnych kartach.</span><span class="sxs-lookup"><span data-stu-id="2aea3-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="2aea3-145">Na karcie osi czasu</span><span class="sxs-lookup"><span data-stu-id="2aea3-145">The Timeline tab</span></span>

<span data-ttu-id="2aea3-146">Po zmodyfikowaniu Tomasz Dykstra oczekujących [EF 6/MVC 5 samouczka](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) następującym kodem zmienić kontroler instruktorów:</span><span class="sxs-lookup"><span data-stu-id="2aea3-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="2aea3-147">Powyższy kod umożliwia mi do przekazania w ciągu zapytania (`eager`) do formantu wczesny lub jawnego ładowania danych.</span><span class="sxs-lookup"><span data-stu-id="2aea3-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="2aea3-148">Na poniższej ilustracji, jest używany podczas jawnego ładowania i strona chronometrażu przedstawia każdego rejestrowania załadowany w `Index` metody akcji:</span><span class="sxs-lookup"><span data-stu-id="2aea3-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![Ładowanie jawne](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="2aea3-150">W poniższym kodzie określono eager i każdej rejestracji jest pobierana po `Index` nosi nazwę widoku:</span><span class="sxs-lookup"><span data-stu-id="2aea3-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![określono eager](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="2aea3-152">Można umieść kursor nad segment czasu można uzyskać informacji o chronometrażu:</span><span class="sxs-lookup"><span data-stu-id="2aea3-152">You can hover over a time segment to get detailed timing information:</span></span>

![aktywowany, aby zobaczyć chronometrażu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="2aea3-154">Wiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="2aea3-154">Model Binding</span></span>

<span data-ttu-id="2aea3-155">[Kartę powiązanie modelu](http://getglimpse.com/Docs/Model-Binding-Tab) zawiera szereg informacji, aby lepiej zrozumieć, jak są powiązane zmiennych formularza i dlaczego niektóre nie są powiązane zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="2aea3-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="2aea3-156">Obraz poniżej przedstawia **?**</span><span class="sxs-lookup"><span data-stu-id="2aea3-156">The image below shows the **?**</span></span> <span data-ttu-id="2aea3-157">ikona, które można kliknąć można wyświetlić strony pomocy glimpse tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="2aea3-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![glimpse widoku powiązanie modelu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="2aea3-159">Trasy</span><span class="sxs-lookup"><span data-stu-id="2aea3-159">Routes</span></span>

 <span data-ttu-id="2aea3-160">Na karcie Glimpse trasy można pomoże Ci debugowania i zrozumieć ich routingu.</span><span class="sxs-lookup"><span data-stu-id="2aea3-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="2aea3-161">Na poniższej ilustracji wybór trasy produktu (i będzie wyświetlana na zielono, Konwencji Glimpse).</span><span class="sxs-lookup"><span data-stu-id="2aea3-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="2aea3-162">![Wybrana nazwa produktu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) wyświetlana jest również tokeny ograniczenia, obszarów i dane trasy.</span><span class="sxs-lookup"><span data-stu-id="2aea3-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="2aea3-163">Zobacz [tras Glimpse](http://getglimpse.com/Docs/Routes-Tab) i [atrybutu routingu na platformie ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="2aea3-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="2aea3-164">Przy użyciu Glimpse na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="2aea3-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="2aea3-165">Domyślnych zasad zabezpieczeń Glimpse umożliwia tylko Glimpse danych mają być wyświetlane z hosta lokalnego.</span><span class="sxs-lookup"><span data-stu-id="2aea3-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="2aea3-166">Ta zasada zabezpieczeń można zmienić, aby wyświetlić te dane na serwerze zdalnym (np. aplikacji sieci web na platformie Azure).</span><span class="sxs-lookup"><span data-stu-id="2aea3-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="2aea3-167">Dla środowisk testowych na platformie Azure, Dodaj wyróżnione znacznika do końca *web.confg* plik w celu włączenia Glimpse:</span><span class="sxs-lookup"><span data-stu-id="2aea3-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="2aea3-168">Z tej samej zmiany dowolnego użytkownika można wyświetlić danych Glimpse lokacji zdalnej.</span><span class="sxs-lookup"><span data-stu-id="2aea3-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="2aea3-169">Rozważ dodanie znaczników powyżej profil publikowania, więc ona wdrożona tylko stosowane podczas korzystania z tego profilu publikowania (na przykład sieci Azure testu proifle.) Aby ograniczyć dane Glimpse, dodamy `canViewGlimpseData` roli i umożliwić tylko przez użytkowników w tej roli, aby wyświetlić dane Glimpse.</span><span class="sxs-lookup"><span data-stu-id="2aea3-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="2aea3-170">Usuń komentarze z *GlimpseSecurityPolicy.cs* plików i zmień [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) wywoływać z `Administrator` do `canViewGlimpseData` roli:</span><span class="sxs-lookup"><span data-stu-id="2aea3-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="2aea3-171">Zabezpieczenia — obszerne dane przez Glimpse może spowodować narażenie zabezpieczeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2aea3-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="2aea3-172">Firma Microsoft nie przeprowadziła inspekcji zabezpieczeń programu Glimpse do użycia w produkcji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2aea3-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="2aea3-173">Aby uzyskać informacje na temat dodawania ról, zobacz Moje [wdrażanie aplikacji sieci web zabezpieczenia programu ASP.NET MVC 5 z członkostwa, OAuth i bazy danych SQL Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) samouczka.</span><span class="sxs-lookup"><span data-stu-id="2aea3-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="2aea3-174">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2aea3-174">Additional Resources</span></span>

- [<span data-ttu-id="2aea3-175">Wdrażanie aplikacji bezpiecznego platformy ASP.NET MVC 5 z członkostwa, OAuth i bazy danych SQL Azure</span><span class="sxs-lookup"><span data-stu-id="2aea3-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="2aea3-176">[Glimpse konfiguracji](http://getglimpse.com/Docs/Configuration) -Doc strony na temat konfigurowania kart, zasad wykonywania, rejestrowania i inne.</span><span class="sxs-lookup"><span data-stu-id="2aea3-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
