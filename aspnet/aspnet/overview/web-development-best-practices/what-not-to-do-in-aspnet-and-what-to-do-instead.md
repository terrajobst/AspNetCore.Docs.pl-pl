---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Czego nie robić na platformie ASP.NET i co zrobić zamiast tego | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym temacie opisano kilka typowych pomyłek, których wiele osób wprowadza w projektach programu ASP.NET w sieci web. Zawiera on zalecenia dotyczące co należy zrobić, aby uniknąć tych commo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/08/2014
ms.topic: article
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
ms.technology: ''
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: bf46d0b4997d9816071df20fb1884dd76dce8903
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371885"
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="364ca-104">Czego nie robić na platformie ASP.NET i co zrobić zamiast tego</span><span class="sxs-lookup"><span data-stu-id="364ca-104">What not to do in ASP.NET, and what to do instead</span></span>
====================
<span data-ttu-id="364ca-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="364ca-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="364ca-106">W tym temacie opisano kilka typowych pomyłek, których wiele osób wprowadza w projektach programu ASP.NET w sieci web.</span><span class="sxs-lookup"><span data-stu-id="364ca-106">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="364ca-107">Zawiera on zalecenia dotyczące co należy zrobić, aby uniknąć tych typowych pomyłek.</span><span class="sxs-lookup"><span data-stu-id="364ca-107">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="364ca-108">Jest on oparty na [prezentacji](http://vimeo.com/68390507) przez **Damianem Edwardsem** na norweskiej konferencji deweloperów.</span><span class="sxs-lookup"><span data-stu-id="364ca-108">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>


## <a name="disclaimer"></a><span data-ttu-id="364ca-109">Zrzeczenie odpowiedzialności</span><span class="sxs-lookup"><span data-stu-id="364ca-109">Disclaimer</span></span>

<span data-ttu-id="364ca-110">W tym temacie nie ma służyć jako kompletny przewodnik, aby upewnić się, że aplikacja jest bezpieczny i skuteczny.</span><span class="sxs-lookup"><span data-stu-id="364ca-110">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="364ca-111">Nadal należy stosować najlepsze rozwiązania dotyczące zabezpieczeń i wydajności, które nie zostały opisane w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="364ca-111">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="364ca-112">Sugerują one tylko sposoby unikania typowych błędów związanych z klas .NET i procesów.</span><span class="sxs-lookup"><span data-stu-id="364ca-112">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="364ca-113">Omówienie</span><span class="sxs-lookup"><span data-stu-id="364ca-113">Overview</span></span>

<span data-ttu-id="364ca-114">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="364ca-114">This topic contains the following sections:</span></span>

- [<span data-ttu-id="364ca-115">Zgodność ze standardami</span><span class="sxs-lookup"><span data-stu-id="364ca-115">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="364ca-116">Kontrolki karty</span><span class="sxs-lookup"><span data-stu-id="364ca-116">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="364ca-117">Właściwości stylu dla formantów</span><span class="sxs-lookup"><span data-stu-id="364ca-117">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="364ca-118">Strony i wywołań zwrotnych kontroli</span><span class="sxs-lookup"><span data-stu-id="364ca-118">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="364ca-119">Wykrywanie możliwości przeglądarki</span><span class="sxs-lookup"><span data-stu-id="364ca-119">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="364ca-120">Zabezpieczenia</span><span class="sxs-lookup"><span data-stu-id="364ca-120">Security</span></span>](#security)

    - [<span data-ttu-id="364ca-121">Żądanie weryfikacji</span><span class="sxs-lookup"><span data-stu-id="364ca-121">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="364ca-122">Uwierzytelnianie formularzy cookieless i sesji</span><span class="sxs-lookup"><span data-stu-id="364ca-122">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="364ca-123">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="364ca-123">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="364ca-124">Trybie średniego zaufania</span><span class="sxs-lookup"><span data-stu-id="364ca-124">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="364ca-125">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="364ca-125">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="364ca-126">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="364ca-126">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="364ca-127">Niezawodność i wydajność</span><span class="sxs-lookup"><span data-stu-id="364ca-127">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="364ca-128">PreSendRequestHeaders i PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="364ca-128">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="364ca-129">Zdarzenia asynchroniczne strony za pomocą formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="364ca-129">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="364ca-130">Ogień i zapominać pracy</span><span class="sxs-lookup"><span data-stu-id="364ca-130">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="364ca-131">Treść jednostki żądania</span><span class="sxs-lookup"><span data-stu-id="364ca-131">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="364ca-132">Response.Redirect i Response.End</span><span class="sxs-lookup"><span data-stu-id="364ca-132">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="364ca-133">EnableViewState i ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="364ca-133">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="364ca-134">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="364ca-134">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="364ca-135">Długie żądania uruchomione (> 110 w sekundach)</span><span class="sxs-lookup"><span data-stu-id="364ca-135">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="364ca-136">Zgodność ze standardami</span><span class="sxs-lookup"><span data-stu-id="364ca-136">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="364ca-137">Kontrolki karty</span><span class="sxs-lookup"><span data-stu-id="364ca-137">Control Adapters</span></span>

<span data-ttu-id="364ca-138">Zalecenie: Zatrzymaj renderowanie adaptacyjne za pomocą adapterów kontrolek, a zamiast tego użyć zapytaniami multimediów CSS i HTML zgodnych ze standardami.</span><span class="sxs-lookup"><span data-stu-id="364ca-138">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="364ca-139">Formanty karty zostały wprowadzone w .NET 2.0 do renderowania kodu prezentacji, który został dostosowany do różnych urządzeń i środowisk.</span><span class="sxs-lookup"><span data-stu-id="364ca-139">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="364ca-140">Teraz można osiągnąć ten adaptacyjne renderowania przy użyciu CSS i HTML.</span><span class="sxs-lookup"><span data-stu-id="364ca-140">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="364ca-141">Należy zaprzestać korzystania z adapterów kontrolek i przekonwertować żadnych istniejących kart CSS i HTML.</span><span class="sxs-lookup"><span data-stu-id="364ca-141">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="364ca-142">Aby uzyskać więcej informacji, zobacz [zapytaniami multimediów](http://www.w3.org/TR/css3-mediaqueries/) i [instrukcje: Dodawanie stron Mobile Your wzorca ASP.NET Web Forms / aplikacji MVC](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="364ca-142">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="364ca-143">Właściwości stylu dla formantów</span><span class="sxs-lookup"><span data-stu-id="364ca-143">Style Properties on Controls</span></span>

<span data-ttu-id="364ca-144">Zalecenie: Zatrzymaj, ustawienie wartości stylu w znacznikach kontroli, a zamiast tego ustawienia formatowania wartości w arkusze stylów CSS.</span><span class="sxs-lookup"><span data-stu-id="364ca-144">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="364ca-145">Formanty serwera sieci Web zawierają dziesiątek, jak właściwości, które mogą być używane do ustawiania właściwości stylu w tekście.</span><span class="sxs-lookup"><span data-stu-id="364ca-145">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="364ca-146">Na przykład ForeColor właściwość ustawia kolor tekstu dla formantu.</span><span class="sxs-lookup"><span data-stu-id="364ca-146">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="364ca-147">Można osiągnąć ten sam efekt, bardziej wydajnie za pomocą arkuszy stylów CSS.</span><span class="sxs-lookup"><span data-stu-id="364ca-147">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="364ca-148">Arkusze stylów umożliwiają scentralizowanie wartości stylu i zapobiec ustawianiu tych wartości w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="364ca-148">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="364ca-149">Poniższy przykład przedstawia klasę CSS tekstu zestawy na czerwony.</span><span class="sxs-lookup"><span data-stu-id="364ca-149">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="364ca-150">Następny przykład pokazuje, jak dynamicznie zastosować klasę CSS.</span><span class="sxs-lookup"><span data-stu-id="364ca-150">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="364ca-151">Strony i wywołań zwrotnych kontroli</span><span class="sxs-lookup"><span data-stu-id="364ca-151">Page and Control Callbacks</span></span>

<span data-ttu-id="364ca-152">Zalecenie: Zatrzymaj, za pomocą wywołania zwrotne strony i kontrolki, a zamiast tego użyć dowolnej z następujących czynności: AJAX, UpdatePanel, metod akcji MVC, interfejs API sieci Web lub SignalR.</span><span class="sxs-lookup"><span data-stu-id="364ca-152">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="364ca-153">We wcześniejszych wersjach programu ASP.NET metody wywołania zwrotnego strony i kontrolki włączone aktualizację części strony sieci web bez odświeżania całej strony.</span><span class="sxs-lookup"><span data-stu-id="364ca-153">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="364ca-154">Teraz można wykonać aktualizacji stron częściowych przy użyciu [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [interfejsu API sieci Web](../../../web-api/index.md) lub [SignalR](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="364ca-154">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="364ca-155">Należy zatrzymać przy użyciu metod wywołania zwrotnego, ponieważ mogą one spowodować problemy z przyjaznymi adresami URL i routing.</span><span class="sxs-lookup"><span data-stu-id="364ca-155">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="364ca-156">Domyślnie formanty, nie należy włączać metody wywołania zwrotnego, ale jeśli włączono tę funkcję w kontrolce, należy wyłączyć je.</span><span class="sxs-lookup"><span data-stu-id="364ca-156">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="364ca-157">Wykrywanie możliwości przeglądarki</span><span class="sxs-lookup"><span data-stu-id="364ca-157">Browser Capability Detection</span></span>

<span data-ttu-id="364ca-158">Zalecenie: Zatrzymaj, przy użyciu przeglądarki statyczne możliwości wykrywania, a zamiast tego użyj funkcji dynamicznej wykrywania.</span><span class="sxs-lookup"><span data-stu-id="364ca-158">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="364ca-159">We wcześniejszych wersjach programu ASP.NET obsługiwane funkcje w każdej przeglądarce są przechowywane w pliku XML.</span><span class="sxs-lookup"><span data-stu-id="364ca-159">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="364ca-160">Wykrywanie obsługi różnych funkcji za pomocą statycznych wyszukiwania nie jest najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="364ca-160">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="364ca-161">Teraz można dynamicznie wykryć przeglądarki użytkownika obsługiwane funkcje przy użyciu funkcji wykrywania struktury, takich jak [Modernizr](http://modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="364ca-161">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="364ca-162">Funkcja wykrywania określa pomocy technicznej podjęto próbę użycia metody lub właściwości, a następnie zaznaczając, aby zobaczyć, jeśli przeglądarki generowane oczekiwany rezultat.</span><span class="sxs-lookup"><span data-stu-id="364ca-162">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="364ca-163">Domyślnie Modernizr znajduje się w szablonach aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="364ca-163">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="364ca-164">Zabezpieczenia</span><span class="sxs-lookup"><span data-stu-id="364ca-164">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="364ca-165">Żądanie weryfikacji</span><span class="sxs-lookup"><span data-stu-id="364ca-165">Request Validation</span></span>

<span data-ttu-id="364ca-166">Zalecenie: Sprawdzanie poprawności danych wejściowych użytkownika i kodowanie danych wyjściowych z użytkowników.</span><span class="sxs-lookup"><span data-stu-id="364ca-166">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="364ca-167">Weryfikacja żądania jest funkcją programu ASP.NET, która sprawdza każde żądanie i zatrzymuje żądania, jeśli zostanie znaleziony potencjalnych zagrożeń.</span><span class="sxs-lookup"><span data-stu-id="364ca-167">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="364ca-168">Nie są zależne od weryfikację żądań dla zabezpieczania aplikacji przed atakami skryptów między witrynami.</span><span class="sxs-lookup"><span data-stu-id="364ca-168">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="364ca-169">Zamiast tego należy sprawdzić poprawność wszystkich danych wejściowych od użytkowników i kodowanie danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="364ca-169">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="364ca-170">W niektórych przypadkach ograniczonych wyrażeń regularnych można użyć, aby sprawdzić poprawność danych wejściowych, ale w przypadku bardziej skomplikowane, które należy sprawdzić, czy dane wejściowe użytkownika za pomocą klas platformy .NET, które sprawdza, czy wartość jest zgodna, dozwolone wartości.</span><span class="sxs-lookup"><span data-stu-id="364ca-170">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="364ca-171">Poniższy przykład przedstawia sposób użycia metody statycznej w klasie identyfikatora Uri do ustalenia, czy identyfikator Uri, podane przez użytkownika jest nieprawidłowa.</span><span class="sxs-lookup"><span data-stu-id="364ca-171">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="364ca-172">Jednak aby wystarczająco sprawdzić identyfikator Uri, należy także sprawdzić się upewnić, że Określa ona `http` lub `https`.</span><span class="sxs-lookup"><span data-stu-id="364ca-172">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="364ca-173">W poniższym przykładzie użyto metody wystąpienia, aby sprawdzić, czy identyfikator Uri jest prawidłowa.</span><span class="sxs-lookup"><span data-stu-id="364ca-173">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="364ca-174">Przed Renderowanie danych wejściowych użytkownika w formacie HTML, lub danych wejściowych użytkownika, np. w zapytaniu SQL, należy zakodować wartości, aby upewnić się, że złośliwy kod nie jest uwzględniony.</span><span class="sxs-lookup"><span data-stu-id="364ca-174">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="364ca-175">Możesz HTML Koduj wartość w znacznikach z &lt;%: %&gt; składni, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="364ca-175">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="364ca-176">Lub, w składni Razor HTML kodowanie za pomocą @, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="364ca-176">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="364ca-177">W następnym przykładzie pokazano sposób w formacie HTML zakodować wartości w związanym z kodem.</span><span class="sxs-lookup"><span data-stu-id="364ca-177">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="364ca-178">Bezpiecznie zakodować wartości poleceń SQL, użyj parametrów polecenia takie jak [parametr SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span><span class="sxs-lookup"><span data-stu-id="364ca-178">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="364ca-179">Uwierzytelnianie formularzy cookieless i sesji</span><span class="sxs-lookup"><span data-stu-id="364ca-179">Cookieless Forms Authentication and Session</span></span>

<span data-ttu-id="364ca-180">Zalecenie: Wymagaj plików cookie.</span><span class="sxs-lookup"><span data-stu-id="364ca-180">Recommendation: Require cookies.</span></span>

<span data-ttu-id="364ca-181">Przekazywanie informacji uwierzytelniania w ciągu zapytania nie jest bezpieczne.</span><span class="sxs-lookup"><span data-stu-id="364ca-181">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="364ca-182">Dlatego wymaga plików cookie, jeśli aplikacja zawiera uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="364ca-182">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="364ca-183">Jeśli Twojego pliku cookie są przechowywane poufne informacje, należy rozważyć wymaganie protokołu SSL dla pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="364ca-183">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="364ca-184">Poniższy przykład pokazuje, jak określić w pliku Web.config, że uwierzytelnianie formularzy wymaga pliku cookie, które są przesyłane za pośrednictwem protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="364ca-184">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="364ca-185">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="364ca-185">EnableViewStateMac</span></span>

<span data-ttu-id="364ca-186">Zalecenie: Nigdy nie ma wartość false.</span><span class="sxs-lookup"><span data-stu-id="364ca-186">Recommendation: Never set to false.</span></span>

<span data-ttu-id="364ca-187">Domyślnie EnbableViewStateMac jest ustawiona na wartość true.</span><span class="sxs-lookup"><span data-stu-id="364ca-187">By default, EnbableViewStateMac is set to true.</span></span> <span data-ttu-id="364ca-188">Nawet wtedy, gdy aplikacja nie używa stanu widoku, nie należy ustawiać EnableViewStateMac na wartość false.</span><span class="sxs-lookup"><span data-stu-id="364ca-188">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="364ca-189">Ustawienie wartości FALSE spowoduje, że aplikacja narażone na wykonywanie skryptów między witrynami.</span><span class="sxs-lookup"><span data-stu-id="364ca-189">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="364ca-190">Począwszy od platformy ASP.NET 4.5.2, środowisko wykonawcze wymusza **EnableViewStateMac = true**.</span><span class="sxs-lookup"><span data-stu-id="364ca-190">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="364ca-191">Nawet wtedy, gdy zostanie ustawiona na wartość false, środowisko uruchomieniowe ignoruje tę wartość i będzie kontynuowane z ustawioną wartość true.</span><span class="sxs-lookup"><span data-stu-id="364ca-191">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="364ca-192">Aby uzyskać więcej informacji, zobacz [ASP.NET 4.5.2 i EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="364ca-192">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="364ca-193">Poniższy przykład pokazuje, jak ustawić EnableViewStateMac na wartość true.</span><span class="sxs-lookup"><span data-stu-id="364ca-193">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="364ca-194">Nie trzeba faktycznie Ustaw tę wartość na wartość true, ponieważ jest prawdą, domyślnie.</span><span class="sxs-lookup"><span data-stu-id="364ca-194">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="364ca-195">Jednakże jeśli ustawiono go na wartość false, na dowolnej stronie w aplikacji, należy poprawić natychmiast tę wartość.</span><span class="sxs-lookup"><span data-stu-id="364ca-195">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="364ca-196">Trybie średniego zaufania</span><span class="sxs-lookup"><span data-stu-id="364ca-196">Medium Trust</span></span>

<span data-ttu-id="364ca-197">Zalecenie: Nie są zależne od zaufania Medium (lub inne poziom zaufania) pełnią funkcję granicy zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="364ca-197">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="364ca-198">Częściowej relacji zaufania nie chronią odpowiednio aplikacji i nie powinna być używana.</span><span class="sxs-lookup"><span data-stu-id="364ca-198">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="364ca-199">Zamiast tego należy używać pełnego zaufania i izolowania niezaufanych aplikacji w osobnych pulach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="364ca-199">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="364ca-200">Ponadto należy uruchomić każdy unikatową tożsamość puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="364ca-200">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="364ca-201">Aby uzyskać więcej informacji, zobacz [częściowego zaufania programu ASP.NET nie gwarantuje izolacji aplikacji](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="364ca-201">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="364ca-202">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="364ca-202">&lt;appSettings&gt;</span></span>

<span data-ttu-id="364ca-203">Zalecenie: Nie należy wyłączać ustawienia zabezpieczeń w &lt;appSettings&gt; elementu.</span><span class="sxs-lookup"><span data-stu-id="364ca-203">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="364ca-204">AppSettings element zawiera wiele wartości, które są wymagane dla aktualizacji zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="364ca-204">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="364ca-205">Nie należy zmienić lub wyłączyć te wartości.</span><span class="sxs-lookup"><span data-stu-id="364ca-205">You should not change or disable these values.</span></span> <span data-ttu-id="364ca-206">Jeśli konieczne jest wyłączenie tych wartości podczas wdrażania aktualizacji, natychmiast ponownie włączyć po ukończeniu wdrażania.</span><span class="sxs-lookup"><span data-stu-id="364ca-206">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="364ca-207">Aby uzyskać więcej informacji, zobacz [Element appSettings platformy ASP.NET](https://msdn.microsoft.com/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="364ca-207">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="364ca-208">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="364ca-208">UrlPathEncode</span></span>

<span data-ttu-id="364ca-209">Zalecenie: Użyj [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="364ca-209">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="364ca-210">Metoda UrlPathEncode został dodany do programu .NET Framework, aby rozwiązać problem ze zgodnością bardzo przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="364ca-210">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="364ca-211">Odpowiednio nie przeprowadza kodowania adresu URL, a nie chroni aplikację przed skryptów między witrynami.</span><span class="sxs-lookup"><span data-stu-id="364ca-211">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="364ca-212">Powinno nigdy nie używaj go w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="364ca-212">You should never use it in your application.</span></span> <span data-ttu-id="364ca-213">Zamiast tego należy użyć [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="364ca-213">Instead, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="364ca-214">Poniższy przykład pokazuje, jak przekazać adres URL zakodowany jako parametr ciągu zapytania kontrolki hiperlinku.</span><span class="sxs-lookup"><span data-stu-id="364ca-214">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="364ca-215">Niezawodność i wydajność</span><span class="sxs-lookup"><span data-stu-id="364ca-215">Reliability and Performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="364ca-216">PreSendRequestHeaders i PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="364ca-216">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="364ca-217">Zalecenie: Nie należy używać tych zdarzeń z modułów zarządzanych.</span><span class="sxs-lookup"><span data-stu-id="364ca-217">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="364ca-218">Zamiast tego należy zapisać moduł macierzysty usług IIS w celu wykonania wymaganych zadań.</span><span class="sxs-lookup"><span data-stu-id="364ca-218">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="364ca-219">Zobacz [tworzenie moduły HTTP kodu natywnego](https://msdn.microsoft.com/library/ms693629.aspx).</span><span class="sxs-lookup"><span data-stu-id="364ca-219">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/library/ms693629.aspx).</span></span>

<span data-ttu-id="364ca-220">Możesz użyć [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) i [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) zdarzeń z modułów natywnych usług IIS.</span><span class="sxs-lookup"><span data-stu-id="364ca-220">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="364ca-221">Nie używaj `PreSendRequestHeaders` i `PreSendRequestContent` z modułów zarządzanych, które implementują `IHttpModule`.</span><span class="sxs-lookup"><span data-stu-id="364ca-221">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="364ca-222">Ustawienie tych właściwości mogą powodować problemy z żądań asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="364ca-222">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="364ca-223">Kombinacja żądane Routing aplikacji (ARR) i technologia websockets może prowadzić do wyjątki naruszenie zasad dostępu, które może spowodować, że w3wp ulega awarii.</span><span class="sxs-lookup"><span data-stu-id="364ca-223">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="364ca-224">Na przykład iiscore! W3_CONTEXT_BASE::GetIsLastNotification + 68 w iiscore.dll spowodowała wyjątek naruszenie zasad dostępu (0xC0000005).</span><span class="sxs-lookup"><span data-stu-id="364ca-224">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="364ca-225">Zdarzenia asynchroniczne strony za pomocą formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="364ca-225">Asynchronous Page Events with Web Forms</span></span>

<span data-ttu-id="364ca-226">Zalecenie: W formularzach sieci Web należy unikać pisania asynchronicznej metody void zdarzenia cyklu życia strony, a zamiast tego użyć [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) dla kodu asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="364ca-226">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="364ca-227">Po oznaczeniu zdarzeń strony, przy użyciu **async** i **void**, nie można określić, kiedy kod asynchroniczny zostało zakończone.</span><span class="sxs-lookup"><span data-stu-id="364ca-227">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="364ca-228">Zamiast tego należy użyć Page.RegisterAsyncTask do uruchomienia kodu asynchronicznego w sposób, który pozwala na śledzenie jego zakończenia.</span><span class="sxs-lookup"><span data-stu-id="364ca-228">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="364ca-229">W poniższym przykładzie pokazano, a przycisk kliknij program obsługi, który zawiera kod asynchroniczny.</span><span class="sxs-lookup"><span data-stu-id="364ca-229">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="364ca-230">W tym przykładzie zawiera wartość ciągu do czytania asynchronicznie, inaczej niż tylko jako uproszczony przykład asynchroniczne zadanie, a nie zalecanym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="364ca-230">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="364ca-231">Jeśli używasz zadań asynchronicznych ustawić platformę docelową środowiska wykonawczego protokołu Http do wersji 4.5 w pliku Web.config.</span><span class="sxs-lookup"><span data-stu-id="364ca-231">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 in the Web.config file.</span></span> <span data-ttu-id="364ca-232">Ustawienia platformy docelowej do 4.5 włącza na nowy kontekst synchronizacji został dodany w .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="364ca-232">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="364ca-233">Ta wartość jest ustawiana domyślnie w nowych projektach programu Visual Studio 2012, ale nie można ustawić, jeśli pracujesz z istniejącego projektu.</span><span class="sxs-lookup"><span data-stu-id="364ca-233">This value is set by default in new projects in Visual Studio 2012, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="364ca-234">Ogień i zapominać pracy</span><span class="sxs-lookup"><span data-stu-id="364ca-234">Fire-and-Forget Work</span></span>

<span data-ttu-id="364ca-235">Zalecenie: Podczas obsługi żądania w programie ASP.NET, należy unikać uruchamiania pożarowego i zapominać pracy (takie wywołanie metody ThreadPool.QueueUserWorkItem lub tworzenia czasomierza, który wielokrotnie wywołuje delegata).</span><span class="sxs-lookup"><span data-stu-id="364ca-235">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="364ca-236">Jeśli aplikacja ma pożarowego i zapominać pracy jest uruchamiany w ramach programu ASP.NET, aplikację można uzyskać zsynchronizowane. W dowolnym momencie domeny aplikacji może zostać zniszczone co oznacza, że proces ciągłego mogą nie odpowiadać bieżący stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="364ca-236">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="364ca-237">Przeniesienie tego typu pracy poza programem ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="364ca-237">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="364ca-238">Możesz użyć zadania Web Job, usługa Windows lub roli procesu roboczego na platformie Azure do wykonywania pracy w toku i uruchomić kod z innego procesu.</span><span class="sxs-lookup"><span data-stu-id="364ca-238">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="364ca-239">Jeśli konieczne jest wykonanie tej pracy w programie ASP.NET, można dodać pakietu Nuget o nazwie [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) do uruchomienia kodu.</span><span class="sxs-lookup"><span data-stu-id="364ca-239">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="364ca-240">Treść jednostki żądania</span><span class="sxs-lookup"><span data-stu-id="364ca-240">Request Entity Body</span></span>

<span data-ttu-id="364ca-241">Zalecenie: Unikaj czytania Request.Form Request.InputStream przed programu obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="364ca-241">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="364ca-242">Najwcześniejsza którymi należy zapoznać się z Request.Form lub Request.InputStream jest podczas obsługi wykonywania zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="364ca-242">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="364ca-243">W przypadku platformy MVC kontroler jest programem obsługi i zdarzenie wykonania jest po uruchomieniu metody akcji.</span><span class="sxs-lookup"><span data-stu-id="364ca-243">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="364ca-244">W formularzach sieci Web strona jest program obsługi, a zdarzenie wykonania jest gdy zostanie wyzwolony zdarzeń Page.Init.</span><span class="sxs-lookup"><span data-stu-id="364ca-244">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="364ca-245">Jeśli treść jednostki żądania możesz odczytać starszych niż zdarzenie wykonania, kolidować z przetwarzaniem żądania.</span><span class="sxs-lookup"><span data-stu-id="364ca-245">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="364ca-246">Jeśli zachodzi potrzeba odczytania treści jednostki żądania przed zdarzeniem execute, użyj jednej [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) lub [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="364ca-246">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="364ca-247">Użycie opcji GetBufferlessInputStream, uzyskiwanie pierwotne strumienia żądania, a na siebie odpowiedzialność za przetwarzanie całego żądania.</span><span class="sxs-lookup"><span data-stu-id="364ca-247">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="364ca-248">Po wywołaniu GetBufferlessInputStream, Request.Form i Request.InputStream są niedostępne, ponieważ nie ma została wypełniona przez platformę ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="364ca-248">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="364ca-249">Gdy używasz GetBufferedInputStream, otrzymasz kopię strumienia z żądania.</span><span class="sxs-lookup"><span data-stu-id="364ca-249">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="364ca-250">Request.Form i Request.InputStream będą nadal dostępne w dalszej części żądania, ponieważ ASP.NET wypełnia drugi egzemplarz.</span><span class="sxs-lookup"><span data-stu-id="364ca-250">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="364ca-251">Response.Redirect i Response.End</span><span class="sxs-lookup"><span data-stu-id="364ca-251">Response.Redirect and Response.End</span></span>

<span data-ttu-id="364ca-252">Zalecenie: Należy pamiętać o różnice w sposób obsługi wątków po wywołaniu [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="364ca-252">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="364ca-253">[Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) metoda wywołuje metodę Response.End.</span><span class="sxs-lookup"><span data-stu-id="364ca-253">The [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="364ca-254">W procesie synchronicznym wywołanie Request.Redirect powoduje, że bieżący wątek natychmiastowe przerwanie.</span><span class="sxs-lookup"><span data-stu-id="364ca-254">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="364ca-255">Jednak w procesie asynchronicznym, wywołanie Response.Redirect nie przerwać bieżącego wątku, więc kontynuuje wykonywanie kodu dla żądania.</span><span class="sxs-lookup"><span data-stu-id="364ca-255">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="364ca-256">W procesie asynchronicznym musi zwracać zadanie z metody, aby zatrzymać wykonywanie kodu.</span><span class="sxs-lookup"><span data-stu-id="364ca-256">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="364ca-257">W projekcie MVC nie powinien wywoływać Response.Redirect.</span><span class="sxs-lookup"><span data-stu-id="364ca-257">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="364ca-258">Zamiast tego zwracają RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="364ca-258">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="364ca-259">EnableViewState i ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="364ca-259">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="364ca-260">Zalecenie: ViewStateMode Użyj zamiast EnableViewState, aby zapewnić kontrolę nad tym, którzy kontrolki używać stan widoku.</span><span class="sxs-lookup"><span data-stu-id="364ca-260">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="364ca-261">Gdy EnableViewState jest ustawiona na wartość false w dyrektywie Page, stan widoku jest wyłączona dla wszystkich kontrolek w obrębie strony i nie można włączyć.</span><span class="sxs-lookup"><span data-stu-id="364ca-261">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="364ca-262">Jeśli chcesz włączyć tylko niektóre formanty na stronie stanu widoku, ustaw ViewStateMode wyłączone dla strony.</span><span class="sxs-lookup"><span data-stu-id="364ca-262">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="364ca-263">Następnie ustaw ViewStateMode włączone na tylko formanty, które jest potrzebna stan widoku.</span><span class="sxs-lookup"><span data-stu-id="364ca-263">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="364ca-264">Włączając stan widoku dla formantów, które go potrzebują, można zmniejszyć rozmiar danych stanu widoku dla stron sieci web.</span><span class="sxs-lookup"><span data-stu-id="364ca-264">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="364ca-265">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="364ca-265">SqlMembershipProvider</span></span>

<span data-ttu-id="364ca-266">Zalecenie: Korzystanie z dostawców uniwersalnych.</span><span class="sxs-lookup"><span data-stu-id="364ca-266">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="364ca-267">W bieżącym szablony projektu została zastąpiona SqlMembershipProvider [dostawców uniwersalnych ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.Providers), która jest dostępna jako pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="364ca-267">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="364ca-268">Jeśli używasz SqlMembershipProvider w projekcie, który został zbudowany przy użyciu starszej wersji szablony, powinien Przełącz się do dostawców uniwersalnych.</span><span class="sxs-lookup"><span data-stu-id="364ca-268">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="364ca-269">Dostawców uniwersalnych współpracować z wszystkich baz danych, które są obsługiwane przez program Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="364ca-269">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="364ca-270">Aby uzyskać więcej informacji, zobacz [wprowadzenie do dostawców uniwersalnych ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="364ca-270">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="364ca-271">Długotrwałych żądań (> 110 w sekundach)</span><span class="sxs-lookup"><span data-stu-id="364ca-271">Long-running Requests (>110 seconds)</span></span>

<span data-ttu-id="364ca-272">Zalecenie: Użyj [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) lub [SignalR](../../../signalr/index.md) dla połączonych klientów i użyj asynchroniczne operacje We/Wy.</span><span class="sxs-lookup"><span data-stu-id="364ca-272">Recommendation: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="364ca-273">Długotrwałych żądań może spowodować nieprzewidywalne skutki i niską wydajnością w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="364ca-273">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="364ca-274">Domyślne ustawienie limitu czasu żądania wynosi 110 sekund.</span><span class="sxs-lookup"><span data-stu-id="364ca-274">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="364ca-275">Jeśli używasz stanu sesji przy użyciu żądania długotrwałych, ASP.NET zwolni blokadę obiektu sesji po 110 sekundach.</span><span class="sxs-lookup"><span data-stu-id="364ca-275">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="364ca-276">Jednak aplikacja może być w trakcie wykonywania operacji na obiekcie sesji, blokada jest zwalniana, gdy operacja może zakończyć się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="364ca-276">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="364ca-277">Jeśli drugie żądanie od użytkownika jest zablokowane podczas pierwszego żądania, drugie żądanie mogą uzyskiwać dostęp do obiektu Session w niespójnym stanie.</span><span class="sxs-lookup"><span data-stu-id="364ca-277">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="364ca-278">Jeśli aplikacja zawiera operacje We/Wy blokowania (lub synchronicznego), aplikacja będzie odpowiadać.</span><span class="sxs-lookup"><span data-stu-id="364ca-278">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="364ca-279">Aby zwiększyć wydajność, należy użyć operacji asynchronicznych operacji We/Wy w .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="364ca-279">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="364ca-280">Ponadto na użytek funkcji WebSockets lub SignalR klientów nawiązujących połączenie z serwerem.</span><span class="sxs-lookup"><span data-stu-id="364ca-280">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="364ca-281">Te funkcje są przeznaczone do efektywnej obsługi długotrwałych żądań.</span><span class="sxs-lookup"><span data-stu-id="364ca-281">These features are designed to efficiently handle long-running requests.</span></span>
