---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: "Śledzenie informacji odwiedzający (Analytics) dla strony sieci Web ASP.NET (Razor) lokacji | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "Po ich zaakceptujesz przechodzi do witryny sieci Web, można przeanalizować ruchu witryny sieci Web."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 9a381ebaed30325fdfa5f0f558910d3002c61559
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="93d62-103">Śledzenie informacji odwiedzający (Analytics) dla lokacji (Razor) stron sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="93d62-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="93d62-104">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="93d62-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="93d62-105">W tym artykule opisano, jak dodawać witryny sieci Web analytics do stron w witrynie sieci Web platformy ASP.NET Web Pages (Razor) przy użyciu pomocnika.</span><span class="sxs-lookup"><span data-stu-id="93d62-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="93d62-106">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="93d62-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="93d62-107">Jak wysyłać informacje o ruchu witryny sieci Web do dostawcy analiz.</span><span class="sxs-lookup"><span data-stu-id="93d62-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="93d62-108">Są to programowania funkcje dodane w artykule programu ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="93d62-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="93d62-109">`Analytics` Pomocnika.</span><span class="sxs-lookup"><span data-stu-id="93d62-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="93d62-110">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="93d62-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="93d62-111">Strony sieci Web platformy ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="93d62-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="93d62-112">Biblioteka pomocników sieci Web platformy ASP.NET (pakiet NuGet)</span><span class="sxs-lookup"><span data-stu-id="93d62-112">ASP.NET Web Helpers Library (NuGet package)</span></span>


<span data-ttu-id="93d62-113">Analytics to ogólny termin technologii, która ruchu w witrynie sieci Web, można zrozumieć, jak użytkownicy korzystają z witryny.</span><span class="sxs-lookup"><span data-stu-id="93d62-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="93d62-114">Wiele usług analizy są dostępne, łącznie z Google, Yahoo StatCounter i innych usług.</span><span class="sxs-lookup"><span data-stu-id="93d62-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="93d62-115">Analiza sposób, działa to, czy utworzysz konto z dostawcy analiz, w którym rejestrowane są lokacji które mają być śledzone. Dostawca wysyła fragment kodu JavaScript, która zawiera identyfikator lub śledzenia kodu dla Twojego konta.</span><span class="sxs-lookup"><span data-stu-id="93d62-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="93d62-116">Możesz dodać fragment kodu JavaScript do stron sieci web w lokacji, które mają być śledzone. (Zazwyczaj są dodawane fragment analytics stopki lub układ strony lub innych kod znaczników HTML, który pojawia się na każdej stronie w witrynie.) Gdy użytkownik zażąda strony, która zawiera jeden z tych fragmentów kodu JavaScript, fragment wysyła informacje o bieżącej strony do dostawcy analiz, który rejestruje szczegółowe informacje o stronie.</span><span class="sxs-lookup"><span data-stu-id="93d62-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="93d62-117">Chcesz przyjrzeć statystyk lokacji logujesz się do witryny sieci Web dostawcy analiz.</span><span class="sxs-lookup"><span data-stu-id="93d62-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="93d62-118">Można wyświetlić szerokiej gamy raportów o witrynie, takich jak:</span><span class="sxs-lookup"><span data-stu-id="93d62-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="93d62-119">Liczba wyświetleń strony dla poszczególnych stron.</span><span class="sxs-lookup"><span data-stu-id="93d62-119">The number of page views for individual pages.</span></span> <span data-ttu-id="93d62-120">Oznacza to, (około) jak wiele osób odwiedzających witrynę, a które strony w witrynie są najbardziej popularnych.</span><span class="sxs-lookup"><span data-stu-id="93d62-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="93d62-121">Jak długo osób wydatkami określone strony.</span><span class="sxs-lookup"><span data-stu-id="93d62-121">How long people spend on specific pages.</span></span> <span data-ttu-id="93d62-122">To może określić, np. czy może być utrzymywanie zainteresowania ludzi strony głównej.</span><span class="sxs-lookup"><span data-stu-id="93d62-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="93d62-123">Jakie osób lokacji były na przed ich odwiedzający witrynę.</span><span class="sxs-lookup"><span data-stu-id="93d62-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="93d62-124">Pomaga to zrozumieć, czy ruchu pochodzi z łącza z wyszukiwanie i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="93d62-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="93d62-125">Gdy osoby odwiedzające witrynę witryny i jak długo pozostają.</span><span class="sxs-lookup"><span data-stu-id="93d62-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="93d62-126">Jakie krajach odwiedzających pochodzą z.</span><span class="sxs-lookup"><span data-stu-id="93d62-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="93d62-127">Jakie przeglądarek i systemów operacyjnych odwiedzających jest używany.</span><span class="sxs-lookup"><span data-stu-id="93d62-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="93d62-129">Dodawanie Analytics do strony przy użyciu Pomocnika</span><span class="sxs-lookup"><span data-stu-id="93d62-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="93d62-130">Strony sieci Web platformy ASP.NET zawiera kilka wątków analizy (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, i `Analytics.GetStatCounterHtml`) ułatwia zarządzanie przeznaczony analytics fragmenty kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="93d62-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="93d62-131">Zamiast ustaleniem, jak i gdzie do umieść kod JavaScript, musisz wykonać jest dodawanie do strony elementu pomocniczego.</span><span class="sxs-lookup"><span data-stu-id="93d62-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="93d62-132">Wszystkie informacje potrzebne do zapewnienia jest nazwa konta, identyfikator lub kod śledzenia.</span><span class="sxs-lookup"><span data-stu-id="93d62-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="93d62-133">(Dla StatCounter, również należy podać kilka dodatkowych wartości.)</span><span class="sxs-lookup"><span data-stu-id="93d62-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="93d62-134">W tej procedurze utworzysz stronę układu, która używa `GetGoogleHtml` pomocnika.</span><span class="sxs-lookup"><span data-stu-id="93d62-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="93d62-135">Jeśli masz już konto, z jednym z innych dostawców analytics, możesz użyć tego konta i dostosować nieznaczne zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="93d62-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="93d62-136">Podczas tworzenia konta usługi analytics, możesz zarejestrować adres URL witryny, który ma być śledzenia.</span><span class="sxs-lookup"><span data-stu-id="93d62-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="93d62-137">Jeśli testujesz wszystko, co na komputerze lokalnym, możesz nie śledzenia ruch rzeczywisty (tylko ruch jest możesz), więc nie będzie mógł rekordu i wyświetlanie statystyk witryny.</span><span class="sxs-lookup"><span data-stu-id="93d62-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="93d62-138">Jednak w tej procedurze pokazano, jak dodać pomocnika analytics do strony.</span><span class="sxs-lookup"><span data-stu-id="93d62-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="93d62-139">Podczas publikowania witryny witryny na żywo będzie wysyłać informacje do dostawcy usługi analytics.</span><span class="sxs-lookup"><span data-stu-id="93d62-139">When you publish your site, the live site will send information to your analytics provider.</span></span>


1. <span data-ttu-id="93d62-140">Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze nie zostało dodane.</span><span class="sxs-lookup"><span data-stu-id="93d62-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="93d62-141">Utworzyć konto w serwisie Google Analytics i zarejestrować nazwę konta.</span><span class="sxs-lookup"><span data-stu-id="93d62-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="93d62-142">Utwórz stronę układu o nazwie *Analytics.cshtml* i Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="93d62-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="93d62-143">Należy umieścić wywołanie `Analytics` pomocnika w treści strony sieci web (przed `</body>` tag).</span><span class="sxs-lookup"><span data-stu-id="93d62-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="93d62-144">W przeciwnym razie przeglądarki nie można uruchomić skryptu.</span><span class="sxs-lookup"><span data-stu-id="93d62-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="93d62-145">Jeśli używasz dostawcy innego analityka, użyj jednej z następujących pomocników:</span><span class="sxs-lookup"><span data-stu-id="93d62-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="93d62-146">(Yahoo)`@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="93d62-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="93d62-147">(StatCounter)`@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="93d62-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="93d62-148">Zastąp `myaccount` o nazwie konto, identyfikator lub kod śledzenia, który został utworzony w kroku 1.</span><span class="sxs-lookup"><span data-stu-id="93d62-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="93d62-149">Uruchom strony w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="93d62-149">Run the page in the browser.</span></span> <span data-ttu-id="93d62-150">(Upewnij się, że strona jest zaznaczona w **pliki** obszar roboczy przed jej uruchomieniem.)</span><span class="sxs-lookup"><span data-stu-id="93d62-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="93d62-151">W przeglądarce Wyświetl źródło strony.</span><span class="sxs-lookup"><span data-stu-id="93d62-151">In the browser, view the page source.</span></span> <span data-ttu-id="93d62-152">Można wyświetlić kod renderowanym analityka:</span><span class="sxs-lookup"><span data-stu-id="93d62-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="93d62-153">Zaloguj się do witryny Google Analytics i sprawdź, czy statystyki dla witryny.</span><span class="sxs-lookup"><span data-stu-id="93d62-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="93d62-154">Jeśli używasz strony w witrynie na żywo, zobaczysz wpis, który rejestruje wizycie na stronie.</span><span class="sxs-lookup"><span data-stu-id="93d62-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="93d62-155">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="93d62-155">Additional Resources</span></span>

- [<span data-ttu-id="93d62-156">Witryny Google Analytics</span><span class="sxs-lookup"><span data-stu-id="93d62-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="93d62-157">Yahoo! Witryna sieci Web Analytics</span><span class="sxs-lookup"><span data-stu-id="93d62-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="93d62-158">StatCounter lokacji</span><span class="sxs-lookup"><span data-stu-id="93d62-158">StatCounter site</span></span>](http://statcounter.com/)
