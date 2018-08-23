---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profilowanie i debugowanie aplikacji ASP.NET MVC za pomocą pakietów Glimpse | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Możliwość wypróbowania to prężnie działającą rośnie z rodziny pakietów NuGet typu open source zapewniająca wydajność szczegółowe, debugowania i informacji diagnostycznych dla platformy ASP.NET...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: f5d174ff6823d654a24dcb2c90f10a3cbd24f1e7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755874"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="32e8e-103">Profilowanie i debugowanie aplikacji ASP.NET MVC za pomocą pakietów Glimpse</span><span class="sxs-lookup"><span data-stu-id="32e8e-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="32e8e-104">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="32e8e-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="32e8e-105">Możliwość wypróbowania to prężnie działającą i rośnie z rodziny pakietów NuGet typu open source zapewniająca wydajność szczegółowe, debugowania i informacje diagnostyczne dla aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="32e8e-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="32e8e-106">Jest proste do zainstalowania, lekki, niezwykle szybkie uzyskiwanie i przedstawia kluczowe metryki wydajności u dołu każdej strony.</span><span class="sxs-lookup"><span data-stu-id="32e8e-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="32e8e-107">Umożliwia przechodzenie do aplikacji, gdy potrzebujesz dowiedzieć się, co się dzieje na serwerze.</span><span class="sxs-lookup"><span data-stu-id="32e8e-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="32e8e-108">Możliwość wypróbowania zapewnia znacznie cenne informacje, zaleca się używania go w całym cyklu tworzenia oprogramowania, w tym środowisku testowym platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="32e8e-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="32e8e-109">Podczas [Fiddler](http://www.telerik.com/fiddler) i [narzędzia programistyczne F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) zapewniają po stronie klienta widok, możliwość wypróbowania zawiera szczegółowy widok z serwera.</span><span class="sxs-lookup"><span data-stu-id="32e8e-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="32e8e-110">Ten samouczek koncentruje się na przy użyciu platformy ASP.NET MVC możliwość wypróbowania i EF pakietów, ale dostępnych jest wiele innych pakietów.</span><span class="sxs-lookup"><span data-stu-id="32e8e-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="32e8e-111">Jeśli jest to możliwe I połączy się z odpowiednią [Poznaj docs](http://getglimpse.com/Docs/) co mogę pomóc zachować.</span><span class="sxs-lookup"><span data-stu-id="32e8e-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="32e8e-112">Możliwość wypróbowania to projekt typu open source, zbyt można współtworzyć kod źródłowy i dokumentacja.</span><span class="sxs-lookup"><span data-stu-id="32e8e-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="32e8e-113">Instalowanie możliwość wypróbowania</span><span class="sxs-lookup"><span data-stu-id="32e8e-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="32e8e-114">Włącz możliwość wypróbowania hosta lokalnego</span><span class="sxs-lookup"><span data-stu-id="32e8e-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="32e8e-115">Na karcie osi czasu</span><span class="sxs-lookup"><span data-stu-id="32e8e-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="32e8e-116">Wiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="32e8e-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="32e8e-117">Trasy</span><span class="sxs-lookup"><span data-stu-id="32e8e-117">Routes</span></span>](#route)
- [<span data-ttu-id="32e8e-118">Używanie możliwość wypróbowania na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="32e8e-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="32e8e-119">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="32e8e-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="32e8e-120">Instalowanie możliwość wypróbowania</span><span class="sxs-lookup"><span data-stu-id="32e8e-120">Installing Glimpse</span></span>

<span data-ttu-id="32e8e-121">Możliwość wypróbowania można zainstalować z konsoli Menedżera pakietów NuGet lub **Zarządzaj pakietami NuGet** konsoli.</span><span class="sxs-lookup"><span data-stu-id="32e8e-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="32e8e-122">Dla tej wersji demonstracyjnej dokonam instalacji pakietów Mvc5 i EF6:</span><span class="sxs-lookup"><span data-stu-id="32e8e-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![Zainstaluj możliwość wypróbowania z NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="32e8e-124">Wyszukaj *Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="32e8e-124">Search for *Glimpse.EF*</span></span>

![Glimpse.EF z NuGet install dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="32e8e-126">Wybierając **zainstalowanych pakietów**, zobaczysz możliwość wypróbowania modułów zależnych zainstalowane:</span><span class="sxs-lookup"><span data-stu-id="32e8e-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Zainstalowane pakiety możliwość wypróbowania z DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="32e8e-128">Poniższe polecenia instalują MVC5 możliwość wypróbowania i EF6 modułów z konsoli Menedżera pakietów:</span><span class="sxs-lookup"><span data-stu-id="32e8e-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="32e8e-129">Włącz możliwość wypróbowania hosta lokalnego</span><span class="sxs-lookup"><span data-stu-id="32e8e-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="32e8e-130">Przejdź do http://localhost:&lt; port #&gt;/glimpse.axd, a następnie kliknij przycisk <strong>Włącz możliwość wypróbowania</strong> przycisku.</span><span class="sxs-lookup"><span data-stu-id="32e8e-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the <strong>Turn Glimpse On</strong> button.</span></span>

![Możliwość wypróbowania axd strony](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="32e8e-132">W przypadku paska ulubionych wyświetlane możesz przeciągnij i upuść przyciski możliwość wypróbowania i dodać je jako bookmarklets:</span><span class="sxs-lookup"><span data-stu-id="32e8e-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![Programu Internet Explorer przy użyciu boookmarklets możliwość wypróbowania](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="32e8e-134">Możesz teraz przechodzić swojej aplikacji, a **głowic się wyświetlanie** (HUD) są widoczne u dołu strony.</span><span class="sxs-lookup"><span data-stu-id="32e8e-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Strona Contact Manager z HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="32e8e-136">[Strony HUD możliwość wypróbowania](http://getglimpse.com/Docs/Heads-up-Display) zawiera szczegółowe informacje o chronometrażu przedstawionych powyżej.</span><span class="sxs-lookup"><span data-stu-id="32e8e-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="32e8e-137">Wyświetla dane HUD wydajności dyskretny kod może generować powiadomienia o problemie natychmiast — przed zagłębieniem się w cyklu testów.</span><span class="sxs-lookup"><span data-stu-id="32e8e-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="32e8e-138">Kliknięcie &quot;g&quot; w prawym dolnym rogu powoduje wyświetlenie panelu możliwość wypróbowania:</span><span class="sxs-lookup"><span data-stu-id="32e8e-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Możliwość wypróbowania panelu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="32e8e-140">Na ilustracji powyżej [karcie wykonywanie](http://getglimpse.com/Docs/Execution-Tab) jest zaznaczone, które wyświetla szczegóły dotyczące działania i filtry w potoku.</span><span class="sxs-lookup"><span data-stu-id="32e8e-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="32e8e-141">Możesz zobaczyć Moje [czasomierza filtr zatrzymać Obejrzyj](http://www.nuget.org/packages/StopWatch/) rozpoczynają się od 6 Etap potoku.</span><span class="sxs-lookup"><span data-stu-id="32e8e-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="32e8e-142">Gdy mój czasomierza lekkie może zapewnić przydatne profilu/chronometrażu danych, jej chybień cały czas poświęcony na autoryzacji i renderowania widoku.</span><span class="sxs-lookup"><span data-stu-id="32e8e-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="32e8e-143">Informacje o mojej zegara [profilowania i czas aplikacji ASP.NET MVC do usługi Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="32e8e-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="32e8e-144">[Karty](http://getglimpse.com/Docs/Tabs) strona zawiera linki do szczegółowych informacji na poszczególnych kartach.</span><span class="sxs-lookup"><span data-stu-id="32e8e-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="32e8e-145">Na karcie osi czasu</span><span class="sxs-lookup"><span data-stu-id="32e8e-145">The Timeline tab</span></span>

<span data-ttu-id="32e8e-146">Po zmodyfikowaniu Tom Dykstra oczekujących [EF 6/MVC 5 samouczka](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) następującym kodem zmienić kontroler Instruktorzy:</span><span class="sxs-lookup"><span data-stu-id="32e8e-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="32e8e-147">Powyższy kod pozwala mi przekazać w ciągu zapytania (`eager`) do formantu eager lub jawne ładowanie danych.</span><span class="sxs-lookup"><span data-stu-id="32e8e-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="32e8e-148">Na poniższej ilustracji, używane jest jawne ładowanie i strona chronometrażu pokazuje każdej rejestracji załadowany w `Index` metody akcji:</span><span class="sxs-lookup"><span data-stu-id="32e8e-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![jawne ładowanie](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="32e8e-150">W poniższym kodzie określono eager i każdej rejestracji jest pobierana po `Index` nosi nazwę widoku:</span><span class="sxs-lookup"><span data-stu-id="32e8e-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![określono eager](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="32e8e-152">Możesz umieścić kursor segment czasu, aby uzyskać szczegółowe informacje o czasie:</span><span class="sxs-lookup"><span data-stu-id="32e8e-152">You can hover over a time segment to get detailed timing information:</span></span>

![Po wskazaniu wskaźnikiem, aby wyświetlić szczegółowe informacje o czasach](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="32e8e-154">Wiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="32e8e-154">Model Binding</span></span>

<span data-ttu-id="32e8e-155">[Kartę powiązanie modelu](http://getglimpse.com/Docs/Model-Binding-Tab) zawiera mnóstwo informacji, które pomagają zrozumieć, jak zmiennych formularza są powiązane i dlaczego niektóre nie są powiązane zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="32e8e-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="32e8e-156">Obraz poniżej przedstawia **?**</span><span class="sxs-lookup"><span data-stu-id="32e8e-156">The image below shows the **?**</span></span> <span data-ttu-id="32e8e-157">ikona, który można kliknąć, aby wyświetlić stronę pomocy możliwość wypróbowania tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="32e8e-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![Poznaj widok wiązanie modelu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="32e8e-159">Trasy</span><span class="sxs-lookup"><span data-stu-id="32e8e-159">Routes</span></span>

 <span data-ttu-id="32e8e-160">Na karcie możliwość wypróbowania trasy można pomoże Ci debugowania i poznanie routingu.</span><span class="sxs-lookup"><span data-stu-id="32e8e-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="32e8e-161">Na poniższej ilustracji wybrano trasy produktu (i pokazuje, w kolorze zielonym, Konwencja możliwość wypróbowania).</span><span class="sxs-lookup"><span data-stu-id="32e8e-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="32e8e-162">![Wybrana nazwa produktu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) tokeny ograniczenia, obszary i dane trasy są również wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="32e8e-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="32e8e-163">Zobacz [trasy możliwość wypróbowania](http://getglimpse.com/Docs/Routes-Tab) i [trasowanie atrybutów w programie ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="32e8e-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="32e8e-164">Używanie możliwość wypróbowania na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="32e8e-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="32e8e-165">Możliwość wypróbowania domyślne zasady zabezpieczeń zezwala tylko możliwość wypróbowania dane mają być wyświetlane z hosta lokalnego.</span><span class="sxs-lookup"><span data-stu-id="32e8e-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="32e8e-166">Możesz zmienić te zasady zabezpieczeń, aby można było wyświetlić te dane na serwerze zdalnym (np. aplikacji sieci web na platformie Azure).</span><span class="sxs-lookup"><span data-stu-id="32e8e-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="32e8e-167">Dla środowisk testowych na platformie Azure, Dodaj wyróżnione znacznika do końca *web.confg* plik, aby włączyć możliwość wypróbowania:</span><span class="sxs-lookup"><span data-stu-id="32e8e-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="32e8e-168">Dzięki tej zmianie samodzielnie każdemu użytkownikowi możliwość wyświetlenia danych możliwość wypróbowania w lokalizacji zdalnej.</span><span class="sxs-lookup"><span data-stu-id="32e8e-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="32e8e-169">Rozważ dodanie znaczników powyżej profil publikowania, dzięki czemu jest ona wdrożona tylko stosowane podczas korzystania z tego profilu publikowania (na przykład Twoja Azure test proifle.) Aby ograniczyć możliwość wypróbowania dane, dodamy `canViewGlimpseData` roli oraz Zezwalanie tylko na użytkowników w tej roli, aby wyświetlić dane możliwość wypróbowania.</span><span class="sxs-lookup"><span data-stu-id="32e8e-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="32e8e-170">Usuń komentarze z *GlimpseSecurityPolicy.cs* pliku, a następnie zmień [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) wywołać z `Administrator` do `canViewGlimpseData` roli:</span><span class="sxs-lookup"><span data-stu-id="32e8e-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="32e8e-171">Zabezpieczenia — zaawansowanych danych dostarczanych przez możliwość wypróbowania może narazić bezpieczeństwo aplikacji.</span><span class="sxs-lookup"><span data-stu-id="32e8e-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="32e8e-172">Nie przeprowadzanych przez firmę Microsoft o możliwość wypróbowania inspekcji zabezpieczeń do użycia w aplikacji w produkcji.</span><span class="sxs-lookup"><span data-stu-id="32e8e-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="32e8e-173">Aby uzyskać informacje na temat dodawania ról, zobacz mój [wdrażanie aplikacji sieci web zabezpieczenia programu ASP.NET MVC 5 z członkostwa, uwierzytelnianiem OAuth i bazą danych SQL na platformie Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) samouczka.</span><span class="sxs-lookup"><span data-stu-id="32e8e-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="32e8e-174">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="32e8e-174">Additional Resources</span></span>

- [<span data-ttu-id="32e8e-175">Wdrażanie bezpiecznej aplikacji ASP.NET MVC 5 z członkostwa, uwierzytelnianiem OAuth i bazą danych SQL na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="32e8e-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="32e8e-176">[Poznaj konfiguracji](http://getglimpse.com/Docs/Configuration) -stronie dokumentu na temat konfigurowania kart, zasad wykonywania, rejestrowania i innych.</span><span class="sxs-lookup"><span data-stu-id="32e8e-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
