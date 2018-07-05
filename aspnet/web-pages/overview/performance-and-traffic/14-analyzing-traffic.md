---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Śledzenie informacji odwiedzający (analiza) for an ASP.NET Web Pages (Razor) lokacji | Dokumentacja firmy Microsoft
author: tfitzmac
description: Po trafiła do Ciebie witryny sieci Web, pracę, możesz chcieć analizowanie ruchu witryny sieci Web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 48782606083b4aa1e32adf6163bcb3f2d9828bc3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387140"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="b6d92-103">Obiekt odwiedzający informacji (analiza) witrynie ASP.NET Web Pages (Razor) ze śledzenia</span><span class="sxs-lookup"><span data-stu-id="b6d92-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="b6d92-104">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b6d92-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b6d92-105">W tym artykule opisano, jak dodać analizy witryn sieci Web do stron w witrynie internetowej ASP.NET Web Pages (Razor) za pomocą pomocnika.</span><span class="sxs-lookup"><span data-stu-id="b6d92-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="b6d92-106">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="b6d92-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="b6d92-107">Jak wysyłać informacje o ruchu witryny sieci Web do dostawcy analiz.</span><span class="sxs-lookup"><span data-stu-id="b6d92-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="b6d92-108">Poniżej przedstawiono funkcje wprowadzone w artykule programowania programu ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="b6d92-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="b6d92-109">`Analytics` Pomocnika.</span><span class="sxs-lookup"><span data-stu-id="b6d92-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b6d92-110">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="b6d92-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b6d92-111">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="b6d92-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="b6d92-112">Biblioteka pomocników sieci Web platformy ASP.NET (pakiet NuGet)</span><span class="sxs-lookup"><span data-stu-id="b6d92-112">ASP.NET Web Helpers Library (NuGet package)</span></span>


<span data-ttu-id="b6d92-113">Analiza jest ogólnym terminem dla technologii, która mierzy ruch w witrynie sieci Web, aby zrozumieć, jak użytkownicy korzystają z witryny.</span><span class="sxs-lookup"><span data-stu-id="b6d92-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="b6d92-114">Wiele usług analizy są dostępne, łącznie z usługami Google, Yahoo, StatCounter i innych.</span><span class="sxs-lookup"><span data-stu-id="b6d92-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="b6d92-115">Analiza sposób, którego działa, można założyć konto za pomocą dostawcy analiz, w której możesz zarejestrować lokację, mają być śledzone. Dostawca wysyła fragment kodu JavaScript, która zawiera identyfikator lub kod konta śledzenia.</span><span class="sxs-lookup"><span data-stu-id="b6d92-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="b6d92-116">Dodaj fragment kodu JavaScript do stron sieci web w lokacji, który chcesz śledzić. (Zazwyczaj dodajesz fragment analytics stopki lub układ strony lub innych znaczników HTML, który pojawia się na każdej stronie w witrynie.) Gdy użytkownicy żądają strony, która zawiera jeden z tych fragmentów kodu JavaScript, fragment kodu wysyła informacje o bieżącej strony do dostawcy analiz, który rejestruje szczegółowe informacje o stronie.</span><span class="sxs-lookup"><span data-stu-id="b6d92-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="b6d92-117">Chcesz się jej przyjrzeć statystyk lokacji logujesz się do witryny sieci Web dostawcy analiz.</span><span class="sxs-lookup"><span data-stu-id="b6d92-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="b6d92-118">Można wyświetlić różne rodzaje raportów o witrynie, takich jak:</span><span class="sxs-lookup"><span data-stu-id="b6d92-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="b6d92-119">Liczba wyświetleń stron dla poszczególnych stron.</span><span class="sxs-lookup"><span data-stu-id="b6d92-119">The number of page views for individual pages.</span></span> <span data-ttu-id="b6d92-120">Oznacza to, (około), jak wiele osób odwiedzających witrynę, a które strony w witrynie są najbardziej popularne.</span><span class="sxs-lookup"><span data-stu-id="b6d92-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="b6d92-121">Ile osób możesz wydać na określone strony.</span><span class="sxs-lookup"><span data-stu-id="b6d92-121">How long people spend on specific pages.</span></span> <span data-ttu-id="b6d92-122">To może określić, np. czy strona główna może być utrzymywanie zainteresowania osób.</span><span class="sxs-lookup"><span data-stu-id="b6d92-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="b6d92-123">Jakie witryn osób były na przed ich odwiedzane witryny.</span><span class="sxs-lookup"><span data-stu-id="b6d92-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="b6d92-124">Pomaga to zrozumieć, czy ruch sieciowy pochodzi z łącza z wyszukiwania i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="b6d92-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="b6d92-125">Gdy ludzie odwiedzić witrynę i jak długo pozostają.</span><span class="sxs-lookup"><span data-stu-id="b6d92-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="b6d92-126">Jakich krajach odwiedzających pochodzą z.</span><span class="sxs-lookup"><span data-stu-id="b6d92-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="b6d92-127">Jakie przeglądarki i systemy operacyjne korzystają z odwiedzających.</span><span class="sxs-lookup"><span data-stu-id="b6d92-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="b6d92-129">Dodawanie analizy do strony przy użyciu Pomocnika</span><span class="sxs-lookup"><span data-stu-id="b6d92-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="b6d92-130">ASP.NET Web Pages obejmuje kilka pomocników analytics (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, i `Analytics.GetStatCounterHtml`) ułatwia zarządzanie fragmenty kodu JavaScript, używane do analizy.</span><span class="sxs-lookup"><span data-stu-id="b6d92-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="b6d92-131">Zamiast ustalenie, jak i gdzie umieścić kod JavaScript wszystko, co należy zrobić, to dodanie pomocnika do strony.</span><span class="sxs-lookup"><span data-stu-id="b6d92-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="b6d92-132">Wszystkie informacje potrzebne do zapewnienia jest nazwa konta, identyfikator lub kod śledzenia.</span><span class="sxs-lookup"><span data-stu-id="b6d92-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="b6d92-133">(Dla StatCounter, również należy podać kilka dodatkowych wartości.)</span><span class="sxs-lookup"><span data-stu-id="b6d92-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="b6d92-134">W tej procedurze utworzysz stronę układu, który używa `GetGoogleHtml` pomocnika.</span><span class="sxs-lookup"><span data-stu-id="b6d92-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="b6d92-135">Jeśli masz już konto, przy użyciu jednego z innych dostawców analytics, możesz zamiast tego użyj tego konta i wprowadzić niewielkie korekty, zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="b6d92-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="b6d92-136">Podczas tworzenia konta usługi analytics, możesz zarejestrować adres URL witryny, która ma być śledzenia.</span><span class="sxs-lookup"><span data-stu-id="b6d92-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="b6d92-137">Jeśli testujesz wszystko na komputerze lokalnym, użytkownik nie będzie śledzenia rzeczywisty ruch (tylko ruch jest użytkownik), dzięki czemu nie będzie mógł rejestrowanie i wyświetlanie statystyk witryny.</span><span class="sxs-lookup"><span data-stu-id="b6d92-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="b6d92-138">Ale w tej procedurze pokazano, jak dodać pomocnika analytics do strony.</span><span class="sxs-lookup"><span data-stu-id="b6d92-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="b6d92-139">Podczas publikowania witryny działającej witryny będzie wysyłać informacje do dostawcy analiz.</span><span class="sxs-lookup"><span data-stu-id="b6d92-139">When you publish your site, the live site will send information to your analytics provider.</span></span>


1. <span data-ttu-id="b6d92-140">Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [instalowanie obiekty pomocnicze w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze nie został dodany.</span><span class="sxs-lookup"><span data-stu-id="b6d92-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="b6d92-141">Utwórz konto za pomocą usługi Google Analytics i zapisać jego nazwę konta.</span><span class="sxs-lookup"><span data-stu-id="b6d92-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="b6d92-142">Tworzenie układu strony o nazwie *Analytics.cshtml* i Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="b6d92-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="b6d92-143">Należy umieścić wywołanie `Analytics` pomocnika w treści strony sieci web (przed `</body>` tagu).</span><span class="sxs-lookup"><span data-stu-id="b6d92-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="b6d92-144">W przeciwnym razie przeglądarki nie uruchomi skrypt.</span><span class="sxs-lookup"><span data-stu-id="b6d92-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="b6d92-145">Jeśli używasz dostawcy analizy różnych użyć jednej z następujących pomocników:</span><span class="sxs-lookup"><span data-stu-id="b6d92-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="b6d92-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="b6d92-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="b6d92-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="b6d92-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="b6d92-148">Zastąp `myaccount` o nazwie konto, identyfikator lub kod śledzenia, który został utworzony w kroku 1.</span><span class="sxs-lookup"><span data-stu-id="b6d92-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="b6d92-149">Uruchom stronę w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="b6d92-149">Run the page in the browser.</span></span> <span data-ttu-id="b6d92-150">(Upewnij się, że strona jest zaznaczona w **pliki** obszaru roboczego przed jej uruchomieniem.)</span><span class="sxs-lookup"><span data-stu-id="b6d92-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="b6d92-151">W przeglądarce Wyświetl źródło strony.</span><span class="sxs-lookup"><span data-stu-id="b6d92-151">In the browser, view the page source.</span></span> <span data-ttu-id="b6d92-152">Będzie można wyświetlić kod renderowanych analytics:</span><span class="sxs-lookup"><span data-stu-id="b6d92-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="b6d92-153">Zaloguj się w witrynie Google Analytics i zbadaj statystyki dla danej witryny.</span><span class="sxs-lookup"><span data-stu-id="b6d92-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="b6d92-154">Jeśli korzystasz z stronę w witrynie na żywo, zostanie wyświetlony wpis, który rejestruje odwiedziny na stronę.</span><span class="sxs-lookup"><span data-stu-id="b6d92-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="b6d92-155">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b6d92-155">Additional Resources</span></span>

- [<span data-ttu-id="b6d92-156">Witryna usługi Google Analytics</span><span class="sxs-lookup"><span data-stu-id="b6d92-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="b6d92-157">Yahoo! Analiza witrynę sieci Web</span><span class="sxs-lookup"><span data-stu-id="b6d92-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="b6d92-158">StatCounter lokacji</span><span class="sxs-lookup"><span data-stu-id="b6d92-158">StatCounter site</span></span>](http://statcounter.com/)
