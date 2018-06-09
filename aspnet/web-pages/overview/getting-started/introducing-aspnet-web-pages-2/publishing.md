---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: Wprowadzenie do składnika ASP.NET Web Pages — publikowania lokacji za pomocą programu WebMatrix | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym samouczku jest ostatnim rat w zestawie Samouczek wprowadzający stron ASP.NET Web Pages i programu Microsoft WebMatrix. Zawarto informacje, jak opublikować witrynę t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: 7b9bffac5cc72e1bea3f1b211cc03be2ccb8e499
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "30899593"
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a><span data-ttu-id="bbe9d-104">Wprowadzenie do strony sieci Web ASP.NET - publikowania lokacji za pomocą programu WebMatrix</span><span class="sxs-lookup"><span data-stu-id="bbe9d-104">Introducing ASP.NET Web Pages - Publishing a Site by Using WebMatrix</span></span>
====================
<span data-ttu-id="bbe9d-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="bbe9d-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="bbe9d-106">W tym samouczku jest ostatnim rat w zestawie Samouczek wprowadzający stron ASP.NET Web Pages i programu Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-106">This tutorial is the final installment in the tutorial set that introduces ASP.NET Web Pages and Microsoft WebMatrix.</span></span> <span data-ttu-id="bbe9d-107">Zawarto informacje, jak opublikowanie witryny z Internetem, tak aby inne osoby mogą pracować z nim.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-107">It discusses how to publish your site to the Internet so that others can work with it.</span></span> <span data-ttu-id="bbe9d-108">Przyjęto założenie, że zostały wykonane serii za pomocą [tworzenie spójny wygląd witryn stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251585).</span><span class="sxs-lookup"><span data-stu-id="bbe9d-108">It assumes you have completed the series through [Creating a Consistent Look for ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=251585).</span></span>
> 
> <span data-ttu-id="bbe9d-109">Dowiesz się, jak opublikować swoją witrynę przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="bbe9d-109">You'll learn how to publish your site using:</span></span>
> 
> - <span data-ttu-id="bbe9d-110">Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="bbe9d-110">Microsoft Azure</span></span>
> - <span data-ttu-id="bbe9d-111">Firmy hostingu sieci Web</span><span class="sxs-lookup"><span data-stu-id="bbe9d-111">Web Hosting Company</span></span>


## <a name="about-publishing-your-site"></a><span data-ttu-id="bbe9d-112">Publikowanie witryny — informacje</span><span class="sxs-lookup"><span data-stu-id="bbe9d-112">About Publishing Your Site</span></span>

<span data-ttu-id="bbe9d-113">Do chwili wykonaniu całą pracę na komputerze lokalnym, łącznie z testowaniem stron.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-113">Up to now, you've done all your work on a local computer, including testing your pages.</span></span> <span data-ttu-id="bbe9d-114">Do uruchomienia programu<em>.cshtml</em> stron, użytych serwera sieci web, wbudowany w program WebMatrix, to znaczy usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-114">To run your<em>.cshtml</em> pages, you've used the web server that's built into WebMatrix, namely IIS Express.</span></span> <span data-ttu-id="bbe9d-115">Jednak oczywiście nie zobaczyć, witryny, do której został utworzony, chyba że użytkownik.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-115">But of course no one can see the site you've created except you.</span></span> <span data-ttu-id="bbe9d-116">Aby inni pracować z witryny, należy opublikować go w Internecie.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-116">To let others work with your site, you have to publish it to the Internet.</span></span>

<span data-ttu-id="bbe9d-117">Jeśli nie masz już dostępu do serwera sieci web publiczne, publikowanie oznacza, że konto z *platformy w chmurze* lub *dostawcy hostingu*.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-117">Unless you have access to a public web server already, publishing means that you have to have an account with a *cloud platform* or a *hosting provider*.</span></span> <span data-ttu-id="bbe9d-118">Platformy w chmurze, takich jak Microsoft Azure oferuje infrastrukturę na żądanie do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-118">A cloud platform, such as Microsoft Azure, provides on-demand infrastructure for your applications.</span></span> <span data-ttu-id="bbe9d-119">Dostawca usług hostingowych jest firmy, który jest właścicielem serwerów sieci web publicznie i który będzie można wynajmować miejsce dla witryny.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-119">A hosting provider is a company that owns publicly accessible web servers and that will rent you space for your site.</span></span> <span data-ttu-id="bbe9d-120">Hosting planów wykonywania z kilku kwoty miesięcznie (lub nawet wolnego) dla małych witryn do wielu tysięcy dolarów miesięcznie dla dużych komercyjnych witryn sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-120">Hosting plans run from a few dollars a month (or even free) for small sites to many hundreds of dollars a month for high-volume commercial websites.</span></span>

> [!NOTE]
> <span data-ttu-id="bbe9d-121">Może mieć dostęp do serwera sieci web publicznych za pośrednictwem usługodawcy internetowego (ISP) używanego do pobrania w domu usługi internet.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-121">You might have access to a public web server via the internet service provider (ISP) that you use to get internet service at home.</span></span> <span data-ttu-id="bbe9d-122">Jednak Twój dostawca hostingu musi obsługiwać stron ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-122">However, your hosting provider must support ASP.NET Web Pages.</span></span> <span data-ttu-id="bbe9d-123">Nie wielu usługodawców internetowych, ale warto zawsze sprawdzania.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-123">Many ISPs don't, but it's always worth checking.</span></span>


<span data-ttu-id="bbe9d-124">W tym samouczku przedstawimy omówienie sposobu publikowania.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-124">In this tutorial, we'll give you an overview of how to publish.</span></span> <span data-ttu-id="bbe9d-125">Nie jest praktyczne podanie szczegółowymi wszystko, ponieważ proces różni się nieco dla każdego dostawcy hostingu.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-125">It's not practical to provide exact details for everything, because the process differs a bit for every hosting provider.</span></span> <span data-ttu-id="bbe9d-126">Ale otrzymasz dobrze działania procesu.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-126">But you'll get a good idea of how the process works.</span></span>

<span data-ttu-id="bbe9d-127">Ten samouczek zawiera cztery sekcje:</span><span class="sxs-lookup"><span data-stu-id="bbe9d-127">This tutorial contains four sections:</span></span>

1. [<span data-ttu-id="bbe9d-128">Konfigurowanie domyślnej strony</span><span class="sxs-lookup"><span data-stu-id="bbe9d-128">Setting up the default page</span></span>](#defaultpage)
2. <span data-ttu-id="bbe9d-129">Publikowanie (wybierz jedną z następujących)</span><span class="sxs-lookup"><span data-stu-id="bbe9d-129">Publishing (choose one of the following)</span></span>  
 <span data-ttu-id="bbe9d-130">a.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-130">a.</span></span> [<span data-ttu-id="bbe9d-131">Publikowanie witryny Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="bbe9d-131">Publishing Your Site to Microsoft Azure</span></span>](#azure)  
 <span data-ttu-id="bbe9d-132">b.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-132">b.</span></span> [<span data-ttu-id="bbe9d-133">Publikowanie witryny firmy hostingu w sieci Web</span><span class="sxs-lookup"><span data-stu-id="bbe9d-133">Publishing Your Site to a Web Hosting Company</span></span>](#host)
3. [<span data-ttu-id="bbe9d-134">Aktualizowanie na żywo witryny: ponowne publikowanie</span><span class="sxs-lookup"><span data-stu-id="bbe9d-134">Updating the Live Site: Republishing</span></span>](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a><span data-ttu-id="bbe9d-135">Konfigurowanie domyślnej strony</span><span class="sxs-lookup"><span data-stu-id="bbe9d-135">Setting up the default page</span></span>

<span data-ttu-id="bbe9d-136">Gdy użytkownik przechodzi do podstawowego adresu witryny sieci web, domyślnej strony w witrynie jest wyświetlany użytkownikowi.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-136">When a user navigates to the base address for your web site, the default page for your site is displayed to the user.</span></span> <span data-ttu-id="bbe9d-137">Na przykład gdy Default.htm jest ustawiona jako stronę domyślną witryny w www.contoso.com, następnie przejść do obszaru <strong>www.contoso.com</strong> jest taka sama jak przejść do obszaru <strong>www.contoso.com/Default.htm</strong>.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-137">For example, when Default.htm is set as the default page for the site at www.contoso.com, then navigating to <strong>www.contoso.com</strong> is the same as navigating to <strong>www.contoso.com/Default.htm</strong>.</span></span>

<span data-ttu-id="bbe9d-138">Obecnie Twoja witryna wymaga **Default.cshtml** jako domyślnej strony.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-138">Currently, your site uses **Default.cshtml** as the default page.</span></span> <span data-ttu-id="bbe9d-139">Ta strona jest poprawnie domyślnej strony, ale w tym samouczku nie dodano żadnej zawartości do tej strony, będzie wyświetlany w pustej strony.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-139">This page is fine for your default page, but in this tutorial you have not added any content to that page so it would display a blank page.</span></span> <span data-ttu-id="bbe9d-140">Otwórz Default.cshtml i Zastąp zawartość następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-140">Open Default.cshtml and replace the content with the following code.</span></span>

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

<span data-ttu-id="bbe9d-141">Teraz ta lokacja jest gotowa do opublikowania.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-141">Now your site is ready for publication.</span></span> <span data-ttu-id="bbe9d-142">Po pierwsze zobaczysz, jak wdrożyć witrynę Azure, a następnie wdrożyć go hostingu firmy w sieci web.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-142">First, you will see how to deploy the site to Azure, and then how to deploy it to a web hosting company.</span></span> <span data-ttu-id="bbe9d-143">Każda opcja działa dla witryny sieci web, a tylko konieczne wykonaj jedną z opcji wdrażania.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-143">Either option works for your web site, and you only need to follow one of the deployment options.</span></span>

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a><span data-ttu-id="bbe9d-144">Publikowanie witryny Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="bbe9d-144">Publishing Your Site to Microsoft Azure</span></span>

<span data-ttu-id="bbe9d-145">W tym samouczku najpierw opisano sposób wdrażania witryny Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-145">This tutorial will first show you how to deploy your site to Microsoft Azure.</span></span> <span data-ttu-id="bbe9d-146">Logując się za pomocą konta Microsoft, można utworzyć maksymalnie 10 bezpłatnych witryn na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-146">By signing in with a Microsoft account, you can create up to 10 free sites on Azure.</span></span> <span data-ttu-id="bbe9d-147">Witryny wolnego te zapewniają wygodny sposób testowania witryny.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-147">These free sites provide a convenient way to test your sites.</span></span> <span data-ttu-id="bbe9d-148">Ten przykład witryny później, aby uniknąć używania wszystkich witryn wolnego zawsze można usunąć.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-148">You can always delete this example site later to avoid using all of your free sites.</span></span> <span data-ttu-id="bbe9d-149">Można utworzyć bezpłatne konto próbne w zaledwie kilka minut.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-149">You can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="bbe9d-150">Aby uzyskać więcej informacji, zobacz [bezpłatnej wersji próbnej Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="bbe9d-150">For details, see [Azure Free Trial](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

<span data-ttu-id="bbe9d-151">Na wstążce programu WebMatrix, kliknij przycisk **publikowania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-151">In the WebMatrix ribbon, click the **Publish** button.</span></span>

![Przycisk "Publikuj" wstążce programu WebMatrix](publishing/_static/image1.png)

<span data-ttu-id="bbe9d-153">**Publikowania witryny** zostanie wyświetlone okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-153">The **Publish Your Site** dialog box is displayed.</span></span> <span data-ttu-id="bbe9d-154">Jeśli użytkownik nie ma zalogowany do konta Microsoft, okno dialogowe będzie zawierać **Rozpoczynanie pracy z platformą Azure** łącza.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-154">If you have not signed in to your Microsoft account, the dialog box will contain a **Get started with Azure** link.</span></span> <span data-ttu-id="bbe9d-155">Kliknięcie tego łącza.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-155">Click this link.</span></span>

![Publikowanie witryny](publishing/_static/image2.png)

<span data-ttu-id="bbe9d-157">Jeśli użytkownik nie ma zalogowany do konta Microsoft, podane są ponownie podpisać.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-157">If you have not signed in to a Microsoft account, you are again given the opportunity to sign in.</span></span> <span data-ttu-id="bbe9d-158">Możesz zalogować się do konta Microsoft do publikowania witryny na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-158">You must sign in to a Microsoft account to publish your site on Azure.</span></span>

![Rejestrowanie](publishing/_static/image3.png)

<span data-ttu-id="bbe9d-160">Po zalogowaniu się do swojego konta Microsoft, okno dialogowe zawiera łącza do tworzenia nowej witryny na platformie Azure lub połączyć się z jedną z istniejących witryn na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-160">After signing in to your Microsoft account, the dialog box contains links to create a new site on Azure or connect to one of your existing sites on Azure.</span></span>

![Utwórz nową witrynę](publishing/_static/image4.png)

<span data-ttu-id="bbe9d-162">Wybierz **Utwórz nową witrynę**.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-162">Select **Create a new site**.</span></span>

<span data-ttu-id="bbe9d-163">Jeśli nazwa projektu jest **WebPagesMovies**, będzie domyślna nazwa witryny **webpagesmovies.azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-163">If you named your project **WebPagesMovies**, the default name for your site will be **webpagesmovies.azurewebsites.net**.</span></span> <span data-ttu-id="bbe9d-164">Ta nazwa domyślna to najprawdopodobniej nie jest dostępny, wskazywany przez czerwony wykrzyknik.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-164">This default name is most likely not available, as indicated by the red exclamation mark.</span></span>

![Domyślna nazwa witryny sieci Web](publishing/_static/image5.png)

<span data-ttu-id="bbe9d-166">Zmień nazwę lokacji do zasobu, który jest dostępny i wybierz lokalizację, która znajduje się w pobliżu lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-166">Change the site name to something that is available, and select a location that is close to your location.</span></span>

![Nazwa witryny zmienione](publishing/_static/image6.png)

<span data-ttu-id="bbe9d-168">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-168">Click **OK**.</span></span>

<span data-ttu-id="bbe9d-169">Program WebMatrix performss test, aby określić, czy serwer jest zgodny z witryną.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-169">WebMatrix performss a test to determine if the server is compatible with your site.</span></span>

![test zgodności](publishing/_static/image7.png)

<span data-ttu-id="bbe9d-171">Wybierz **kontynuować**.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-171">Select **Continue**.</span></span>

<span data-ttu-id="bbe9d-172">Wyniki testowania zgodności są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-172">The results of the compatibility test are displayed.</span></span>

![wynik zgodności](publishing/_static/image8.png)

<span data-ttu-id="bbe9d-174">Wybierz **kontynuować**.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-174">Select **Continue**.</span></span>

<span data-ttu-id="bbe9d-175">Program WebMatrix Wyświetla plików i baz danych, które zostaną opublikowane w witrynie.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-175">WebMatrix displays the files and databases that will be published to the site.</span></span> <span data-ttu-id="bbe9d-176">Ponieważ jest to w przypadku publikowania lokacji po raz pierwszy, wszystkie pliki są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-176">Since this is the first time you are publishing the site, all of the files are listed.</span></span> <span data-ttu-id="bbe9d-177">Można usunąć zaznaczenie pola wyboru pliku, który nie jest gotowy do opublikowania.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-177">You can uncheck a file that is not ready to be published.</span></span> <span data-ttu-id="bbe9d-178">W kolejnych publikacji będą wyświetlane tylko te pliki, które zostały zmienione.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-178">In the subsequent publications, only the files that have changed will be displayed.</span></span> <span data-ttu-id="bbe9d-179">Zobacz [aktualizowanie na żywo witryny: ponowne publikowanie](#update).</span><span class="sxs-lookup"><span data-stu-id="bbe9d-179">See [Updating the Live Site: Republishing](#update).</span></span>

![wyświetlić podglądu publikowania](publishing/_static/image9.png)

<span data-ttu-id="bbe9d-181">Wybierz **kontynuować**.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-181">Select **Continue**.</span></span>

<span data-ttu-id="bbe9d-182">Po wdrożeniu witryny na platformie Azure, zostanie wyświetlony komunikat, który wskazuje, że wdrożenie zostało ukończone.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-182">After the site has been deployed to Azure, a message is displayed that indicates the deployment has completed.</span></span>

![Publikowanie ukończone](publishing/_static/image10.png)

<span data-ttu-id="bbe9d-184">Z lokacji i bazą danych została opublikowana na platformie Azure i są teraz dostępne do publicznego.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-184">Your site and database have been published to Azure, and are now available to the public.</span></span> <span data-ttu-id="bbe9d-185">Kliknij link w komunikacie wskazujący publikowania zostało ukończone i znajduje się teraz wdrożonej witryny.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-185">Click the link in the message indicating publishing has completed, and you will now see your deployed site.</span></span> <span data-ttu-id="bbe9d-186">Ani osób z dostępem do Internetu, można dodać lub zmodyfikować rekordy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-186">You or anyone with Internet access can add or modify records in the database.</span></span>

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a><span data-ttu-id="bbe9d-187">Publikowanie witryny firmy hostingu w sieci Web</span><span class="sxs-lookup"><span data-stu-id="bbe9d-187">Publishing Your Site to a Web Hosting Company</span></span>

<span data-ttu-id="bbe9d-188">Jeśli zdecydujesz się nie publikowanie na platformie Azure, zamiast tego można opublikować witryny firmy hostingu w sieci web.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-188">If you decide to not publish to Azure, you can instead publish your site to a web hosting company.</span></span>

<span data-ttu-id="bbe9d-189">Kliknij przycisk **znaleźć usługi hostingu sieci web** łącza.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-189">Click the **Find web hosting** link.</span></span>

![Przycisk "Znajdź hostingu sieci web" w oknie Ustawienia publikowania](publishing/_static/image12.png)

<span data-ttu-id="bbe9d-191">Przejdź do strony w witrynie firmy Microsoft, który zawiera listę dostawców hostingu, które obsługują aplikacje ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-191">You go to a page on the Microsoft site that lists hosting providers that support ASP.NET.</span></span>

![W witrynie firmy Microsoft, który wyświetla listę dostawców usług hostingu](publishing/_static/image13.png)

<span data-ttu-id="bbe9d-193">Oczywiście może być trudne teraz wiedzieć dokładnie funkcje hostingu mogą wymagać w dłuższym okresie.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-193">Obviously, it can be difficult to know now exactly what hosting features you might require over the long term.</span></span> <span data-ttu-id="bbe9d-194">Oto kilka rzeczy, które należy wziąć pod uwagę:</span><span class="sxs-lookup"><span data-stu-id="bbe9d-194">Here are a couple of things to consider:</span></span>

- <span data-ttu-id="bbe9d-195">Do celów WebPagesMovies lokacji nie trzeba mieć osobne dodatek dla programu SQL Server, który często koszty dodatkowe.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-195">For purposes of the WebPagesMovies site, you don't have to have a separate add-on for SQL Server, which often costs extra.</span></span> <span data-ttu-id="bbe9d-196">W witrynie używasz programu SQL Server Compact Edition, która jest niezależna.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-196">In your site, you're using SQL Server Compact Edition, which is self-contained.</span></span> <span data-ttu-id="bbe9d-197">Jednak może być konieczne dostęp do serwera SQL dla niektórych pracy przyszłych witryny sieci Web, które należy wykonać.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-197">However, you might need SQL Server access for some future website work you do.</span></span> <span data-ttu-id="bbe9d-198">Jeśli uważasz, że możesz, upewnij się, że możliwości programu SQL Server można dodać później.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-198">If you think you might, make sure that you can add SQL Server capability later.</span></span>
- <span data-ttu-id="bbe9d-199">Sprawdź, czy dostawca hostingu obsługuje protokół publikowania narzędzia Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-199">Check whether the hosting provider supports the Web Deploy publishing protocol.</span></span> <span data-ttu-id="bbe9d-200">Można opublikować za pomocą protokołu FTP, ale jest bardziej wygodne użycie narzędzia Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-200">You can publish by using FTP protocol, but it's more convenient to use Web Deploy.</span></span>

<span data-ttu-id="bbe9d-201">Niektóre witryny oferują bezpłatny okres próbny.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-201">Some sites offer a free trial period.</span></span> <span data-ttu-id="bbe9d-202">Bezpłatna wersja próbna jest dobrym sposobem spróbuj publikowania i hosting czasie jest nadal eksperymentowanie z programu WebMatrix i stron ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-202">A free trial is a good way to try publishing and hosting while you're still experimenting with WebMatrix and ASP.NET Web Pages.</span></span>

<span data-ttu-id="bbe9d-203">Klucz, który chcesz wybrać.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-203">Pick one that you like.</span></span> <span data-ttu-id="bbe9d-204">W tym samouczku wybrano DiscountASP.NET, ponieważ podczas możemy zostały tworzenia tego samouczka, przedsiębiorstwo podwyższania poziomu, które umożliwiają użytkownikom hosta lokację przez kilka miesięcy.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-204">For this tutorial, we selected DiscountASP.NET, because while we were creating the tutorial, that company had a promotion that let people host a site free for a few months.</span></span>

> [!NOTE]
> <span data-ttu-id="bbe9d-205">Nasz wybór dostawcy hostingu, w tym samouczku nie powinny być rozumiane jako poręczenia tej firmy za pośrednictwem innych.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-205">Our choice of a hosting provider for this tutorial shouldn't be interpreted as an endorsement of that company over any other.</span></span> <span data-ttu-id="bbe9d-206">Ale było konieczne pobranie jednej ilustracyjną i DiscountASP.NET jest jednym z wielu firm, które obsługuje stron sieci Web ASP.NET i protokołu Web Deploy do opublikowania.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-206">But we had to pick one for illustration, and DiscountASP.NET is one of the many companies that supports ASP.NET Web Pages and the Web Deploy protocol for publishing.</span></span>


<span data-ttu-id="bbe9d-207">Zazwyczaj po zarejestrowaniu się z dostawcą hostingu, przedsiębiorstwo wysyła wiadomość e-mail, która zawiera nazwę użytkownika i hasło, adres URL serwera sieci web i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-207">Typically, after you've signed up with the hosting provider, the company sends you an email that contains a user name and password, the URL of the web server, and so on.</span></span> <span data-ttu-id="bbe9d-208">Jeśli firma obsługuje protokołu Web Deploy, może wysyłać należy plik, który zawiera ustawienia publikowania lub pozwalają na pobranie jednej.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-208">If the hosting company supports Web Deploy protocol, they might send you a file that contains publish settings, or let you download one.</span></span> <span data-ttu-id="bbe9d-209">Plik ustawień publikowania upraszcza proces dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-209">A publish settings file simplifies the process for you.</span></span>

<span data-ttu-id="bbe9d-210">Gdy jest zapisany i jest gotowy do opublikowania, kliknij przycisk **publikowania** przycisk na wstążce programu WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-210">When you've signed up and are ready to publish, click the **Publish** button in the WebMatrix ribbon.</span></span> <span data-ttu-id="bbe9d-211">**Ustawień publikowania** zostanie wyświetlone okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-211">The **Publish Settings** dialog box is displayed.</span></span>

<span data-ttu-id="bbe9d-212">Jeśli dostawca hostingu wysyłane plik ustawień publikowania, kliknij przycisk **importowanie ustawień publikowania** link i zaimportuj plik.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-212">If the hosting provider sent you a publish settings file, click the **Import publish settings** link and import the file.</span></span> <span data-ttu-id="bbe9d-213">Jeśli nie masz plik ustawień publikowania, wypełnij pola przy użyciu wartości, które hostingu firmy wysyłane w wiadomościach e-mail.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-213">If you don't have a publish settings file, fill in the fields by using the values that the hosting company sent you in email.</span></span> <span data-ttu-id="bbe9d-214">Oto, co **ustawień publikowania** okno dialogowe może wyglądać po zakończeniu:</span><span class="sxs-lookup"><span data-stu-id="bbe9d-214">Here's what the **Publish Settings** dialog box might look like when you're done:</span></span>

![Ustawienia publikowania wprowadzone w oknie dialogowym Ustawienia publikowania](publishing/_static/image14.png)

<span data-ttu-id="bbe9d-216">Kliknij przycisk **Weryfikacja połączenia z**.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-216">Click **Validate Connection**.</span></span> <span data-ttu-id="bbe9d-217">Jeśli wszystko jest prawidłowy, okno dialogowe Raporty **Połączono pomyślnie**, co oznacza, że może komunikować się z serwerem dostawcy hostingu.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-217">If everything is ok, the dialog box reports **Connected successfully**, which means it can communicate with the hosting provider's server.</span></span>

![Powodzenie komunikatów, jeśli publikowanie ustawienia są prawidłowe](publishing/_static/image15.png)

<span data-ttu-id="bbe9d-219">W przypadku problemu program WebMatrix zapewnia największą informujące o tym, co to jest problem:</span><span class="sxs-lookup"><span data-stu-id="bbe9d-219">If there's a problem, WebMatrix does its best to tell you what the problem is:</span></span>

![Komunikat o błędzie w przypadku problemu z ustawienia publikowania](publishing/_static/image16.png)

<span data-ttu-id="bbe9d-221">Kliknij przycisk **zapisać** Aby zapisać ustawienia.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-221">Click **Save** to save your settings.</span></span> <span data-ttu-id="bbe9d-222">Program WebMatrix udostępnia wykonać test, aby upewnić się, że może komunikować się poprawnie w witrynie hostingu:</span><span class="sxs-lookup"><span data-stu-id="bbe9d-222">WebMatrix offers to perform a test to make sure that it can communicate correctly with the hosting site:</span></span>

![Komunikat oferty wykonać test proces publikowania](publishing/_static/image17.png)

<span data-ttu-id="bbe9d-224">Kliknij przycisk **Tak**.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-224">Click **Yes**.</span></span> <span data-ttu-id="bbe9d-225">Program WebMatrix przekazywać niektóre przykładowe pliki do dostawcy hostingu.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-225">WebMatrix uploads some sample files to the hosting provider.</span></span> <span data-ttu-id="bbe9d-226">Po zakończeniu testowania zgodności programu WebMatrix raportuje wyniki:</span><span class="sxs-lookup"><span data-stu-id="bbe9d-226">When the compatibility test is done, WebMatrix reports the results:</span></span>

![Wyniki testu publikowania](publishing/_static/image18.png)

<span data-ttu-id="bbe9d-228">Jeśli wszystko jest gotowe, przejdź dalej i kliknij przycisk **Kontynuuj** rzeczywistym uruchomić proces publikowania.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-228">If you're ready to go, go ahead and click **Continue** to start the publish process for real.</span></span> <span data-ttu-id="bbe9d-229">Program WebMatrix danych liczbowych co pliki znajdują się w witrynie i znajdują się już na serwerze hosta (od razu, none) i Podgląd proces publikowania:</span><span class="sxs-lookup"><span data-stu-id="bbe9d-229">WebMatrix figures out what files are in your site and are already on the host server (right now, none) and gives you a preview of the publish process:</span></span>

![Jakie pliki, które przekaże proces publikowania w wersji zapoznawczej](publishing/_static/image19.png)

<span data-ttu-id="bbe9d-231">Lista plików do opublikowania zawiera stron sieci web, które po utworzeniu, takich jak *Movies.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-231">The list of files to publish includes the web pages that you've created like *Movies.cshtml*.</span></span> <span data-ttu-id="bbe9d-232">Lista zawiera także pliki dla wątków, które po zainstalowaniu, pliki, aby uruchomić bazy danych programu SQL Server Compact Edition i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-232">The list also includes files for helpers that you've installed, the files to run SQL Server Compact Edition for your database, and so on.</span></span> <span data-ttu-id="bbe9d-233">W związku z tym początkowej publikowania procesu może być istotne.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-233">As a result, the initial publish process can be substantial.</span></span>

<span data-ttu-id="bbe9d-234">Kliknij przycisk **Kontynuuj**.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-234">Click **Continue**.</span></span> <span data-ttu-id="bbe9d-235">Program WebMatrix kopiuje pliki na serwer dostawcy hostingu.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-235">WebMatrix copies your files to the hosting provider's server.</span></span> <span data-ttu-id="bbe9d-236">Po zakończeniu, wyniki są zgłaszane w pasku stanu:</span><span class="sxs-lookup"><span data-stu-id="bbe9d-236">When it's done, the results are reported in the status bar:</span></span>

![Komunikat paska stanu, gdy proces publikowania zakończy się pomyślnie](publishing/_static/image20.png)

<span data-ttu-id="bbe9d-238">Aby wyświetlić witryny na żywo, kliknij łącze w pasku stanu.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-238">To see your live site, click the link in the status bar.</span></span> <span data-ttu-id="bbe9d-239">Dodaj *filmy* do adresu URL, i zobaczysz *Movies.cshtml* utworzony plik:</span><span class="sxs-lookup"><span data-stu-id="bbe9d-239">Add *Movies* to the URL, and you'll see the *Movies.cshtml* file that you created:</span></span>

![Działającą witrynę przedstawiający stronę filmy](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a><span data-ttu-id="bbe9d-241">Aktualizowanie na żywo witryny: ponowne publikowanie</span><span class="sxs-lookup"><span data-stu-id="bbe9d-241">Updating the Live Site: Republishing</span></span>

<span data-ttu-id="bbe9d-242">Po opublikowaniu witryny (do platformy Azure lub hostingu sieci web), są dwie kopie &mdash; wersja na komputerze oraz wersja dostawcy usługi.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-242">Once you've published your site (to either Azure or a web hosting company), there are two copies of it &mdash; the version on your computer and the version on the service provider.</span></span> <span data-ttu-id="bbe9d-243">Prawdopodobnie należy kontynuować tworzenie lokacji (jeśli nic, jako część zestawu samouczek dalej).</span><span class="sxs-lookup"><span data-stu-id="bbe9d-243">You'll probably want to continue developing the site (if nothing else, as part of the next tutorial set).</span></span> <span data-ttu-id="bbe9d-244">Po wykonaniu czynności należy ponownie opublikować witrynę, aby umożliwić kopiowanie zmian z komputera z dostawcą usług.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-244">When you do, you have to republish your site in order to copy changes from your computer to the service provider.</span></span> <span data-ttu-id="bbe9d-245">Proces publikowania w programie WebMatrix można określić, co pliki zostały zmienione w witrynie i publikowanie tylko tych plików.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-245">The publish process in WebMatrix can determine what files have changed on your site and publish just those files.</span></span>

<span data-ttu-id="bbe9d-246">Aby zobaczyć, jak działa ponownego publikowania, otwórz *Movies.cshtml* lokacji, niektóre zmiany małe, a następnie zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-246">To see how republishing works, open the *Movies.cshtml* site, make some small change, and then save the file.</span></span> <span data-ttu-id="bbe9d-247">Na przykład zmień tytuł, aby `Movies - Updated`.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-247">For example, change the title to `Movies - Updated`.</span></span>

<span data-ttu-id="bbe9d-248">Kliknij przycisk **publikowania** przycisk na Wstążce.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-248">Click the **Publish** button in the ribbon.</span></span> <span data-ttu-id="bbe9d-249">Program WebMatrix Określa, co zostało zmienione i pokazuje podgląd plików, które będą go opublikować.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-249">WebMatrix determines what's changed and shows you a preview of the files it will publish.</span></span>

![Okno dialogowe "Publikuj" przedstawiający zmienione pliki gotowe do ponownego publikowania](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> <span data-ttu-id="bbe9d-251">Domyślnie program WebMatrix publikowanie bazy danych (*sdf* pliku) tylko po raz pierwszy, należy opublikować witrynę.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-251">By default, WebMatrix publishes your database (*.sdf* file) only the first time you publish the site.</span></span> <span data-ttu-id="bbe9d-252">Po opublikowaniu lokacji i osób pracuje z witryny sieci Web, bazy danych lokacji na żywo zwykle ma prawdziwe dane z lokacji.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-252">Once your site is published and people are interacting with the website, the database on the live site typically has the site's real data.</span></span> <span data-ttu-id="bbe9d-253">Masz bardzo nie można więc zastąpienia bazy danych na żywo z *sdf* pliku znajdującego się na tym komputerze, który zwykle zawiera tylko dane testowe.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-253">You have to be very careful not to overwrite the live database with the *.sdf* file that's on your computer, which usually contains only test data.</span></span> <span data-ttu-id="bbe9d-254">Dlatego zostanie wyświetlone ostrzeżenie **publikowania spowoduje zastąpienie wszelkich zdalnymi bazami danych**, oraz Dlaczego pole wyboru dla *WebPagesMovies.sdf* jest domyślnie wyczyszczone.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-254">That's why you see the warning **Publishing will overwrite any remote databases**, and why the check box for *WebPagesMovies.sdf* is cleared by default.</span></span>


<span data-ttu-id="bbe9d-255">Kliknij przycisk **Kontynuuj**.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-255">Click **Continue**.</span></span> <span data-ttu-id="bbe9d-256">Program WebMatrix publikuje zmienione pliki i zawiera komunikat z potwierdzeniem, tak, jak został opublikowany po raz pierwszy.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-256">WebMatrix publishes the changed files and shows you a success message, like it did the first time you published.</span></span>

<span data-ttu-id="bbe9d-257">Przejdź do witryny na żywo (można kliknąć link w komunikacie Powodzenie Jeśli nadal jest wyświetlany) i upewnij się, że ta zmiana została opublikowana.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-257">Go to the live site (you can click the link in the success message if it's still showing) and verify that your change has been published.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="bbe9d-258">**Zdalne edytowanie plików**</span><span class="sxs-lookup"><span data-stu-id="bbe9d-258">**Editing Files Remotely**</span></span>
> 
> <span data-ttu-id="bbe9d-259">Alternatywą wobec zmiana witryny i ponowne opublikowanie można edytować pliki zdalne bezpośrednio w programie WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-259">As an alternative to changing your site and then republishing, you can edit remote files directly in WebMatrix.</span></span> <span data-ttu-id="bbe9d-260">W tym scenariuszu Otwórz plik, który znajduje się w dostawcy usług, a program WebMatrix pobierze kopii do edycji.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-260">In this scenario, you open a file that's on the service provider, and WebMatrix downloads a copy of it for you to edit.</span></span> <span data-ttu-id="bbe9d-261">Za każdym razem, gdy zapiszesz plik programu WebMatrix przesyła zmiany do lokacji.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-261">Every time you save the file, WebMatrix sends the changes to the site.</span></span>
> 
> <span data-ttu-id="bbe9d-262">Zdalne edytowanie jest prosty sposób, aby wprowadzić zmiany dotyczące witryny na żywo.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-262">Remote editing is an easy way to make changes to your live site.</span></span> <span data-ttu-id="bbe9d-263">Jednak zmiany wprowadzone w ten sposób nie są zsynchronizowane z plikami w lokalnej witrynie.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-263">However, the changes you make this way aren't synchronized with the files in your local site.</span></span> <span data-ttu-id="bbe9d-264">Aby zsynchronizować lokalnych plików z lokacji zdalnej, możesz pobrać pliki zdalne.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-264">To synchronize the local files with the remote site, you can download the remote files.</span></span> <span data-ttu-id="bbe9d-265">Ten proces działa podobne jak w przypadku publikowania, z wyjątkiem odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-265">This process works much like publishing, except in reverse.</span></span>
> 
> <span data-ttu-id="bbe9d-266">Nie możemy opisano więcej o edycji zdalnego i zdalnego pobierania funkcji programu WebMatrix w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-266">We won't describe more about the remote-editing and remote-download facilities of WebMatrix here.</span></span> <span data-ttu-id="bbe9d-267">Są one bardzo przydatne, jeśli wiele osób muszą pracować na tym samym miejscu na różnych komputerach.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-267">They're quite useful if multiple people have to work on the same site on different computers.</span></span> <span data-ttu-id="bbe9d-268">Aby uzyskać więcej informacji, zobacz [publikowania i edytować lokacją zdalną z wersji Beta 2 programu WebMatrix](https://go.microsoft.com/fwlink/?LinkId=251591).</span><span class="sxs-lookup"><span data-stu-id="bbe9d-268">For more information, see [Publish and Edit a Remote Site with WebMatrix 2 Beta](https://go.microsoft.com/fwlink/?LinkId=251591).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="bbe9d-269">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="bbe9d-269">Additional Resources</span></span>

- <span data-ttu-id="bbe9d-270">[Forum programu WebMatrix ASP.NET Web Pages platformy ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), doskonałym miejscem do zadawania pytań i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bbe9d-270">[ASP.NET WebMatrix ASP.NET Web Pages forum](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), a great place to post questions and get answers.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bbe9d-271">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="bbe9d-271">Previous</span></span>](layouts.md)
