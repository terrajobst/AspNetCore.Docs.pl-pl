---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: "Co nie zrobić w programie ASP.NET i co należy zrobić w zamian | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "W tym temacie opisano kilka typowych pomyłek przez osoby w ramach projektów sieci web ASP.NET. Zapewnia zalecenia dotyczące co należy zrobić, aby uniknąć tych commo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/08/2014
ms.topic: article
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 24c6a35a6b663ebb0f8d0e3e7988322fa5d9018c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="0fb9a-104">Co nie zrobić w programie ASP.NET i co zrobić, zamiast niego</span><span class="sxs-lookup"><span data-stu-id="0fb9a-104">What not to do in ASP.NET, and what to do instead</span></span>
====================
<span data-ttu-id="0fb9a-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0fb9a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0fb9a-106">W tym temacie opisano kilka typowych pomyłek przez osoby w ramach projektów sieci web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-106">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="0fb9a-107">Zapewnia zalecenia dotyczące co należy zrobić, aby uniknąć tych typowych pomyłek.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-107">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="0fb9a-108">Jest on oparty na [prezentacji](http://vimeo.com/68390507) przez **Dyszkiewicz Damianowi** na norweski konferencji deweloperów.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-108">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>


## <a name="disclaimer"></a><span data-ttu-id="0fb9a-109">Zrzeczenie odpowiedzialności</span><span class="sxs-lookup"><span data-stu-id="0fb9a-109">Disclaimer</span></span>

<span data-ttu-id="0fb9a-110">W tym temacie nie ma służyć jako kompletny przewodnik po aby upewnić się, że aplikacja jest bezpieczny i skuteczny.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-110">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="0fb9a-111">Nadal należy stosować najlepsze rozwiązania dotyczące zabezpieczeń i wydajności, które nie zostały opisane w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-111">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="0fb9a-112">Go tylko sugeruje, jak można uniknąć typowych błędów związanych z klasy .NET i procesów.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-112">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="0fb9a-113">Omówienie</span><span class="sxs-lookup"><span data-stu-id="0fb9a-113">Overview</span></span>

<span data-ttu-id="0fb9a-114">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="0fb9a-114">This topic contains the following sections:</span></span>

- [<span data-ttu-id="0fb9a-115">Zgodność ze standardami</span><span class="sxs-lookup"><span data-stu-id="0fb9a-115">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="0fb9a-116">Formantu karty</span><span class="sxs-lookup"><span data-stu-id="0fb9a-116">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="0fb9a-117">Właściwości stylu dla formantów</span><span class="sxs-lookup"><span data-stu-id="0fb9a-117">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="0fb9a-118">Strony i kontrolki wywołań zwrotnych</span><span class="sxs-lookup"><span data-stu-id="0fb9a-118">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="0fb9a-119">Wykrywania możliwości przeglądarki</span><span class="sxs-lookup"><span data-stu-id="0fb9a-119">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="0fb9a-120">Zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="0fb9a-120">Security</span></span>](#security)

    - [<span data-ttu-id="0fb9a-121">Sprawdzanie poprawności żądań</span><span class="sxs-lookup"><span data-stu-id="0fb9a-121">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="0fb9a-122">Uwierzytelnianie formularzy bez plików cookie i sesji</span><span class="sxs-lookup"><span data-stu-id="0fb9a-122">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="0fb9a-123">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="0fb9a-123">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="0fb9a-124">Średnia zaufania</span><span class="sxs-lookup"><span data-stu-id="0fb9a-124">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="0fb9a-125">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="0fb9a-125">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="0fb9a-126">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="0fb9a-126">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="0fb9a-127">Niezawodność i wydajność</span><span class="sxs-lookup"><span data-stu-id="0fb9a-127">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="0fb9a-128">PreSendRequestHeaders i PreSendRequestContext</span><span class="sxs-lookup"><span data-stu-id="0fb9a-128">PreSendRequestHeaders and PreSendRequestContext</span></span>](#presend)
    - [<span data-ttu-id="0fb9a-129">Zdarzenia asynchroniczne strony formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="0fb9a-129">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="0fb9a-130">Fire i zapomnij pracy</span><span class="sxs-lookup"><span data-stu-id="0fb9a-130">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="0fb9a-131">Treści jednostki żądania</span><span class="sxs-lookup"><span data-stu-id="0fb9a-131">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="0fb9a-132">Response.Redirect i Response.End</span><span class="sxs-lookup"><span data-stu-id="0fb9a-132">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="0fb9a-133">EnableViewState i ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="0fb9a-133">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="0fb9a-134">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="0fb9a-134">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="0fb9a-135">Długie żądania uruchamiania (> 110 w sekundach)</span><span class="sxs-lookup"><span data-stu-id="0fb9a-135">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="0fb9a-136">Zgodność ze standardami</span><span class="sxs-lookup"><span data-stu-id="0fb9a-136">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="0fb9a-137">Formantu karty</span><span class="sxs-lookup"><span data-stu-id="0fb9a-137">Control Adapters</span></span>

<span data-ttu-id="0fb9a-138">Zalecenie: Zatrzymaj przy użyciu kart kontrolki do renderowania adaptacyjną, a zamiast tego użyć zapytaniami multimediów CSS i zgodny ze standardami HTML.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-138">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="0fb9a-139">Formanty karty wprowadzono w programie .NET 2.0 do renderowania kodu prezentacji, który został dostosowany do różnych urządzeń i środowiska.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-139">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="0fb9a-140">Teraz ten adaptacyjną renderowania można osiągnąć HTML i CSS.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-140">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="0fb9a-141">Należy zatrzymać za pomocą formantu karty i przekonwertować żadnych istniejących kart CSS i HTML.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-141">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="0fb9a-142">Aby uzyskać więcej informacji, zobacz [zapytaniami multimediów](http://www.w3.org/TR/css3-mediaqueries/) i [jak: Dodaj strony Mobile Your formularzy sieci Web ASP.NET / aplikacji MVC](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="0fb9a-142">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="0fb9a-143">Właściwości stylu dla formantów</span><span class="sxs-lookup"><span data-stu-id="0fb9a-143">Style Properties on Controls</span></span>

<span data-ttu-id="0fb9a-144">Zatrzymaj zalecenie: Ustawianie stylów wartości w znaczniku kontroli, a zamiast tego ustawienia formatowania wartości w arkusze stylów CSS.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-144">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="0fb9a-145">Formanty serwera sieci Web zawiera dziesiątki właściwości, które mogą być używane do ustawiania właściwości stylu w tekście.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-145">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="0fb9a-146">Na przykład właściwości ForeColor Ustawia kolor tekstu dla formantu.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-146">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="0fb9a-147">Można osiągnąć ten sam efekt wydajniej za pośrednictwem arkusze stylów CSS.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-147">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="0fb9a-148">Arkusze stylów umożliwiają scentralizowanie wartości stylu i unikaj ustawiania tych wartości w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-148">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="0fb9a-149">W poniższym przykładzie przedstawiono klasę CSS ustawia tekst na czerwony.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-149">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="0fb9a-150">Kolejnym przykładzie pokazano, jak dynamicznie zastosować klasę CSS.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-150">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="0fb9a-151">Strony i kontrolki wywołań zwrotnych</span><span class="sxs-lookup"><span data-stu-id="0fb9a-151">Page and Control Callbacks</span></span>

<span data-ttu-id="0fb9a-152">Zalecenie: Zatrzymaj przy użyciu wywołania zwrotne strony i kontrolki, a zamiast tego użyć dowolnego z następujących: AJAX, element UpdatePanel, metod akcji MVC, interfejsu API sieci Web lub SignalR.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-152">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="0fb9a-153">We wcześniejszych wersjach programu ASP.NET metody wywołania zwrotnego strony i kontrolki włączone zaktualizować część strony sieci web bez odświeżania całej strony.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-153">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="0fb9a-154">Można teraz wykonać aktualizacje stron częściowych za pośrednictwem [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/en-US/library/bb386454.aspx), [MVC](../../../mvc/index.md), [interfejsu API sieci Web](../../../web-api/index.md) lub [SignalR](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="0fb9a-154">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/en-US/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="0fb9a-155">Należy zatrzymać za pomocą metody wywołania zwrotnego, ponieważ mogą one powodować problemy przyjazne adresy URL i routing.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-155">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="0fb9a-156">Domyślnie przez formanty nie należy włączać metody wywołania zwrotnego, ale włączenie tej funkcji w formancie, należy wyłączyć je.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-156">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="0fb9a-157">Wykrywania możliwości przeglądarki</span><span class="sxs-lookup"><span data-stu-id="0fb9a-157">Browser Capability Detection</span></span>

<span data-ttu-id="0fb9a-158">Zalecenie: Zatrzymaj przy użyciu wykrywania możliwości przeglądarki statyczne i zamiast tego użyć funkcji dynamicznego wykrywania.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-158">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="0fb9a-159">We wcześniejszych wersjach programu ASP.NET obsługiwane funkcje dla każdej przeglądarki były przechowywane w pliku XML.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-159">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="0fb9a-160">Wykrywanie obsługi różnych funkcji za pomocą statycznego wyszukiwania nie jest najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-160">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="0fb9a-161">Teraz można dynamicznie wykryć można funkcji przeglądarki przy użyciu funkcji wykrywania framework, takich jak [Modernizr](http://modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="0fb9a-161">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="0fb9a-162">Funkcja wykrywania określa Obsługa podjęto próbę użycia metody lub właściwości, a następnie sprawdzania, jeśli przeglądarka wyprodukowanych pożądany wynik.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-162">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="0fb9a-163">Domyślnie Modernizr jest zawarte w szablonach aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-163">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="0fb9a-164">Zabezpieczenia</span><span class="sxs-lookup"><span data-stu-id="0fb9a-164">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="0fb9a-165">Sprawdzanie poprawności żądań</span><span class="sxs-lookup"><span data-stu-id="0fb9a-165">Request Validation</span></span>

<span data-ttu-id="0fb9a-166">Zalecenie: Sprawdzanie poprawności danych wejściowych użytkownika, a zakodować dane wyjściowe ze strony użytkowników.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-166">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="0fb9a-167">Weryfikacja żądania jest funkcją programu ASP.NET, która sprawdza każde żądanie i zatrzymuje żądania, jeśli zostanie znaleziony potencjalnych zagrożeń.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-167">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="0fb9a-168">Nie zależą od weryfikacji żądania zabezpieczania aplikacji przed atakami skryptów między witrynami.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-168">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="0fb9a-169">Należy sprawdzić poprawność wszystkich danych wejściowych od użytkowników i kodowania danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-169">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="0fb9a-170">W ograniczonych przypadkach można użyć wyrażeń regularnych do sprawdzania poprawności danych wejściowych, ale w przypadku bardziej skomplikowanych, które należy sprawdzić, czy dane wejściowe użytkownika przy użyciu klasy .NET, które sprawdza, czy wartość jest zgodna dozwolone wartości.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-170">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="0fb9a-171">Poniższy przykład przedstawia użycie metody statycznej klasy identyfikator Uri do ustalenia, czy identyfikator Uri podanego przez użytkownika jest nieprawidłowa.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-171">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="0fb9a-172">Jednak aby wystarczająco sprawdzić identyfikator Uri, należy także sprawdzić aby upewnić się, ponieważ określa on `http` lub `https`.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-172">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="0fb9a-173">W poniższym przykładzie użyto metody wystąpienia, aby sprawdzić poprawność identyfikatora Uri.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-173">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="0fb9a-174">Przed dane wejściowe użytkownika w formacie HTML do renderowania lub dane wejściowe użytkownika w tym w zapytaniu SQL, kodowania wartości do upewnij się, że nie jest dołączana złośliwego kodu.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-174">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="0fb9a-175">Można HTML Koduj wartość w znaczniku z &lt;%: %&gt; składni, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-175">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="0fb9a-176">W składni Razor, można też HTML kodowania z @, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-176">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="0fb9a-177">W następnym przykładzie pokazano sposób do formatu HTML kodowania wartości związane z kodem.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-177">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="0fb9a-178">Aby bezpiecznie Koduj wartość poleceń SQL, użyj parametrów polecenia takiego jak [SqlParameter](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlparameter.aspx).</span><span class="sxs-lookup"><span data-stu-id="0fb9a-178">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="0fb9a-179">Uwierzytelnianie formularzy bez plików cookie i sesji</span><span class="sxs-lookup"><span data-stu-id="0fb9a-179">Cookieless Forms Authentication and Session</span></span>

<span data-ttu-id="0fb9a-180">Zalecenia: Wymagaj plików cookie.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-180">Recommendation: Require cookies.</span></span>

<span data-ttu-id="0fb9a-181">Przekazywanie informacji o uwierzytelnianiu w ciągu zapytania nie jest bezpieczne.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-181">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="0fb9a-182">W związku z tym wymagają plików cookie, jeśli aplikacja zawiera uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-182">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="0fb9a-183">Jeśli Twoje pliki cookie są przechowywane poufne informacje, należy rozważyć wymaganie protokołu SSL dla pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-183">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="0fb9a-184">Poniższy przykład pokazuje, jak można określić w pliku Web.config, że uwierzytelnianie formularzy wymaga pliku cookie, które są przesyłane za pośrednictwem protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-184">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="0fb9a-185">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="0fb9a-185">EnableViewStateMac</span></span>

<span data-ttu-id="0fb9a-186">Zalecenie: Nigdy nie ma wartość false.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-186">Recommendation: Never set to false.</span></span>

<span data-ttu-id="0fb9a-187">Domyślnie EnbableViewStateMac jest ustawiona na true.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-187">By default, EnbableViewStateMac is set to true.</span></span> <span data-ttu-id="0fb9a-188">Nawet jeśli aplikacja nie używa stanu widoku, nie należy ustawiać EnableViewStateMac na wartość false.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-188">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="0fb9a-189">Ustawienie wartości false spowoduje, że aplikacja podatne na wykonywanie skryptów między witrynami.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-189">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="0fb9a-190">Począwszy od platformy ASP.NET 4.5.2, wymusza środowiska uruchomieniowego **EnableViewStateMac = true**.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-190">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="0fb9a-191">Nawet jeśli zostanie ustawiona na wartość false, środowiska uruchomieniowego ignoruje tę wartość i kontynuuje ustaw wartość true.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-191">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="0fb9a-192">Aby uzyskać więcej informacji, zobacz [ASP.NET 4.5.2 i EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="0fb9a-192">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="0fb9a-193">Poniższy przykład pokazuje, jak ustawić EnableViewStateMac na wartość true.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-193">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="0fb9a-194">Nie trzeba faktycznie Ustaw tę wartość na true, ponieważ jest on domyślnie true.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-194">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="0fb9a-195">Jednak jeśli ustawiono go na wartość false na stronach w aplikacji, możesz od razu poprawić tę wartość.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-195">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="0fb9a-196">Średnia zaufania</span><span class="sxs-lookup"><span data-stu-id="0fb9a-196">Medium Trust</span></span>

<span data-ttu-id="0fb9a-197">Zalecenie: Nie zależą od zaufania średni (lub na innym poziomie zaufania) funkcję granicy zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-197">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="0fb9a-198">Częściowej relacji zaufania nie chroni odpowiednio aplikacji i nie powinna być używana.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-198">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="0fb9a-199">Zamiast tego użyj pełnego zaufania i izolowanie niezaufanych aplikacji w osobnych pulach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-199">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="0fb9a-200">Ponadto uruchamiania każdego unikatową tożsamość puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-200">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="0fb9a-201">Aby uzyskać więcej informacji, zobacz [częściowego zaufania programu ASP.NET nie gwarantuje izolacji aplikacji](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="0fb9a-201">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="0fb9a-202">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="0fb9a-202">&lt;appSettings&gt;</span></span>

<span data-ttu-id="0fb9a-203">Zalecenie: Nie należy wyłączać ustawienia zabezpieczeń w &lt;appSettings&gt; elementu.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-203">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="0fb9a-204">AppSettings element zawierającej wiele wartości, które są wymagane dla aktualizacji zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-204">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="0fb9a-205">Nie należy zmieniać ani wyłączyć te wartości.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-205">You should not change or disable these values.</span></span> <span data-ttu-id="0fb9a-206">Jeśli podczas wdrażania aktualizacji, należy wyłączyć te wartości, natychmiast ponownie włączyć po zakończeniu wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-206">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="0fb9a-207">Aby uzyskać więcej informacji, zobacz [ASP.NET appSettings elementu](https://msdn.microsoft.com/en-us/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="0fb9a-207">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/en-us/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="0fb9a-208">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="0fb9a-208">UrlPathEncode</span></span>

<span data-ttu-id="0fb9a-209">Zalecenie: Użyj [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx) zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-209">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="0fb9a-210">Metoda UrlPathEncode został dodany do programu .NET Framework, aby rozwiązać problem ze zgodnością bardzo konkretnej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-210">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="0fb9a-211">Nie można ją było właściwie przeprowadza kodowania adresu URL, a nie chroni aplikację przed skryptów między witrynami.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-211">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="0fb9a-212">Należy nigdy używać w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-212">You should never use it in your application.</span></span> <span data-ttu-id="0fb9a-213">Zamiast tego należy użyć [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="0fb9a-213">Instead, use [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="0fb9a-214">Poniższy przykład przedstawia sposób przekazywania adresu URL zakodowanym jako parametr ciągu zapytania kontrolki hiperlinku.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-214">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="0fb9a-215">Niezawodność i wydajność</span><span class="sxs-lookup"><span data-stu-id="0fb9a-215">Reliability and Performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontext"></a><span data-ttu-id="0fb9a-216">PreSendRequestHeaders i PreSendRequestContext</span><span class="sxs-lookup"><span data-stu-id="0fb9a-216">PreSendRequestHeaders and PreSendRequestContext</span></span>

<span data-ttu-id="0fb9a-217">Zalecenie: Nie należy używać tych zdarzeń z modułów zarządzanych.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-217">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="0fb9a-218">Zamiast tego należy zapisać moduł macierzysty usług IIS w celu wykonania wymaganych zadań.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-218">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="0fb9a-219">Zobacz [tworzenie moduły HTTP kodu natywnego](https://msdn.microsoft.com/en-us/library/ms693629.aspx).</span><span class="sxs-lookup"><span data-stu-id="0fb9a-219">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/en-us/library/ms693629.aspx).</span></span>

<span data-ttu-id="0fb9a-220">Można użyć [PreSendRequestHeaders](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestheaders.aspx) i [PreSendRequestContext](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestcontent.aspx) zdarzenia z modułami macierzystymi usług IIS, ale nie należy używać z modułów zarządzanych, w których zaimplementowano elementu IHttpModule.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-220">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContext](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules, but do not use them with managed modules that implement IHttpModule.</span></span> <span data-ttu-id="0fb9a-221">Ustawienie tych właściwości mogą powodować problemy z żądań asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-221">Setting these properties can cause issues with asynchronous requests.</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="0fb9a-222">Zdarzenia asynchroniczne strony formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="0fb9a-222">Asynchronous Page Events with Web Forms</span></span>

<span data-ttu-id="0fb9a-223">Zalecenie: W formularzach sieci Web unikać pisania async void metody zdarzenia cyklu życia strony, a zamiast tego użyć [Page.RegisterAsyncTask](https://msdn.microsoft.com/en-us/library/system.web.ui.page.registerasynctask.aspx) dla asynchronicznego kodu.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-223">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/en-us/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="0fb9a-224">Po zaznaczeniu zdarzeniem strony z **async** i **void**, nie można określić podczas asynchronicznego kodu zostało zakończone.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-224">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="0fb9a-225">W zamian użyj Page.RegisterAsyncTask, aby uruchomić kod asynchronicznych w taki sposób, który umożliwia śledzenie jego zakończenia.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-225">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="0fb9a-226">W poniższym przykładzie pokazano, a przycisk kliknij program obsługi, który zawiera kod asynchroniczny.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-226">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="0fb9a-227">W tym przykładzie dołączono odczytywania wartości ciągu asynchronicznie, znajdujący się tylko jako uproszczony przykład zadanie asynchroniczne, a nie zalecanym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-227">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="0fb9a-228">Jeśli używasz zadania asynchroniczne, należy ustawić platformę docelową środowiska uruchomieniowego Http 4.5 w pliku Web.config.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-228">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 in the Web.config file.</span></span> <span data-ttu-id="0fb9a-229">Ustawienia platformy docelowej do 4.5 włącza na nowy kontekst synchronizacji został dodany w programie .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-229">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="0fb9a-230">Ta wartość jest ustawiana domyślnie w nowych projektach w programie Visual Studio 2012, ale nie można ustawić podczas pracy z istniejącego projektu.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-230">This value is set by default in new projects in Visual Studio 2012, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="0fb9a-231">Fire i zapomnij pracy</span><span class="sxs-lookup"><span data-stu-id="0fb9a-231">Fire-and-Forget Work</span></span>

<span data-ttu-id="0fb9a-232">Zalecenie: Podczas przetwarzania żądania w programie ASP.NET, należy unikać uruchamiania fire i zapomnij pracy (takie wywołanie metody ThreadPool.QueueUserWorkItem lub tworzenia czasomierza wielokrotnie wywołuje delegata).</span><span class="sxs-lookup"><span data-stu-id="0fb9a-232">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="0fb9a-233">Jeśli aplikacja ma fire i zapomnij pracy uruchamiany w ASP.NET, aplikacja może zsynchronizowane. W dowolnym momencie domena aplikacji mogą zostać zniszczone co oznacza, że Twoje ciągły proces mogą nie odpowiadać bieżący stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-233">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="0fb9a-234">Należy przenieść ten typ pracy poza ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-234">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="0fb9a-235">Można użyć zadania sieci Web, usług systemu Windows lub roli proces roboczy na platformie Azure do wykonywania pracy w toku i uruchomić kod z innego procesu.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-235">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="0fb9a-236">Jeśli konieczne jest przeprowadzenie prac w programie ASP.NET, można dodać pakiet Nuget o nazwie [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) do uruchomienia kodu.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-236">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="0fb9a-237">Treści jednostki żądania</span><span class="sxs-lookup"><span data-stu-id="0fb9a-237">Request Entity Body</span></span>

<span data-ttu-id="0fb9a-238">Zalecenie: Unikaj odczytu Request.Form lub Request.InputStream przed programu obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-238">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="0fb9a-239">Najwcześniejsza którymi należy zapoznać się z kolekcji Request.Form lub Request.InputStream jest podczas obsługi wykonać zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-239">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="0fb9a-240">W nazwie wzorca MVC program obsługi jest kontrolerem i zdarzenie execute jest uruchomienie metody akcji.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-240">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="0fb9a-241">W formularzach sieci Web strona jest programem obsługi i zdarzenie execute jest po zdarzeniu Page.Init.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-241">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="0fb9a-242">Jeśli wcześniej niż zdarzeń execute przeczytanie treści jednostki żądania, zakłócać jest przetwarzania żądania.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-242">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="0fb9a-243">Jeśli zachodzi potrzeba odczytania treści jednostki żądania przed zdarzeniem execute, użyj jednej [Request.GetBufferlessInputStream](https://msdn.microsoft.com/en-us/library/ff406798.aspx) lub [Request.GetBufferedInputStream](https://msdn.microsoft.com/en-us/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="0fb9a-243">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/en-us/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/en-us/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="0fb9a-244">Używasz GetBufferlessInputStream uzyskać raw strumienia z żądania, a na siebie odpowiedzialność za przetwarzanie całego żądania.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-244">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="0fb9a-245">Po wywołaniu metody GetBufferlessInputStream, Request.Form i Request.InputStream są niedostępne, ponieważ nie zostały one wypełnione przez platformę ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-245">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="0fb9a-246">Gdy używasz GetBufferedInputStream otrzymasz kopiowania strumienia z żądania.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-246">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="0fb9a-247">Request.Form i Request.InputStream są nadal dostępne w dalszej części żądania, ponieważ ASP.NET wypełnia pozostałe kopie.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-247">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="0fb9a-248">Response.Redirect i Response.End</span><span class="sxs-lookup"><span data-stu-id="0fb9a-248">Response.Redirect and Response.End</span></span>

<span data-ttu-id="0fb9a-249">Zalecenie: Należy pamiętać o różnice w sposób obsługi wątku po wywołaniu [Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="0fb9a-249">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="0fb9a-250">[Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx) metoda wywołuje metodę Response.End.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-250">The [Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="0fb9a-251">Podczas synchronicznego wywoływania Request.Redirect powoduje, że bieżący wątek natychmiast przerwania.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-251">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="0fb9a-252">Jednak w proces asynchroniczny, wywoływanie metody Response.Redirect nie przerwać bieżącego wątku, więc kontynuuje wykonywanie kodu dla żądania.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-252">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="0fb9a-253">W procesie asynchroniczne musi zwracać zadanie z metody, aby zatrzymać wykonanie kodu.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-253">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="0fb9a-254">W projekcie MVC nie powinny wywoływać Response.Redirect.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-254">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="0fb9a-255">Zamiast tego należy zwracać RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-255">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="0fb9a-256">EnableViewState i ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="0fb9a-256">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="0fb9a-257">Zalecenie: ViewStateMode Użyj zamiast EnableViewState, aby zapewnić kontrolę służącym formanty korzystanie ze stanu widoku.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-257">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="0fb9a-258">Gdy EnableViewState jest ustawiona na wartość false w dyrektywie Page, stan widoku jest wyłączona dla wszystkich kontrolek na stronie i nie można włączyć.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-258">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="0fb9a-259">Jeśli chcesz włączyć tylko niektóre formanty na stronie stanu widoku, wartość ViewStateMode wyłączone dla strony.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-259">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="0fb9a-260">Następnie ustaw ViewStateMode włączone w formantach faktycznie wymagające stan widoku.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-260">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="0fb9a-261">Przez włączenie stanu widoku tylko formanty, które go potrzebują, można zmniejszyć rozmiar stan widoku dla stron sieci web.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-261">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="0fb9a-262">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="0fb9a-262">SqlMembershipProvider</span></span>

<span data-ttu-id="0fb9a-263">Zalecenie: Użyj dostawców uniwersalnych.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-263">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="0fb9a-264">W bieżącym szablony projektów, została zastąpiona SqlMembershipProvider [dostawców uniwersalnych ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.Providers), która jest dostępna jako pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-264">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="0fb9a-265">Jeśli używasz SqlMembershipProvider w projekcie, który został utworzony we wcześniejszej wersji szablonów, należy przełączyć się do dostawców uniwersalnych.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-265">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="0fb9a-266">Dostawców uniwersalnych współpracować z wszystkich baz danych, które są obsługiwane przez program Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-266">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="0fb9a-267">Aby uzyskać więcej informacji, zobacz [wprowadzenie dostawców uniwersalnych ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="0fb9a-267">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="0fb9a-268">Długotrwałe żądania (> 110 w sekundach)</span><span class="sxs-lookup"><span data-stu-id="0fb9a-268">Long-running Requests (>110 seconds)</span></span>

<span data-ttu-id="0fb9a-269">Zalecenie: Użyj [Websocket](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket.aspx) lub [SignalR](../../../signalr/index.md) dla połączonych klientów i użyj asynchronicznej operacji We/Wy.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-269">Recommendation: Use [WebSockets](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="0fb9a-270">Długotrwałe żądania może spowodować nieprzewidywalne skutki i pogorszenie wydajności w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-270">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="0fb9a-271">Domyślne ustawienie limitu czasu dla żądania jest 110 sekund.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-271">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="0fb9a-272">Jeśli używasz stanu sesji z żądaniem długotrwałe, ASP.NET spowoduje zwolnienie blokady obiektu Session 110 sekund.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-272">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="0fb9a-273">Jednak aplikacja może znajdować się w środku operację na obiekcie sesji po zwolnieniu blokady, a operacja nie może zakończyć się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-273">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="0fb9a-274">Drugie żądanie od użytkownika zostało zablokowane podczas pierwszego żądania, drugie żądanie mogą uzyskiwać dostęp do obiektu Session w niespójnym stanie.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-274">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="0fb9a-275">Jeśli aplikacja zawiera blokowania (lub synchroniczne) operacji We/Wy, że aplikacja będzie odpowiadać.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-275">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="0fb9a-276">Aby zwiększyć wydajność, należy użyć asynchronicznej operacji We/Wy w programie .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-276">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="0fb9a-277">Należy także użyć Websocket lub SignalR dla klientów nawiązujących połączenie z serwerem.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-277">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="0fb9a-278">Te funkcje są przeznaczone do efektywnej obsługi żądań długotrwałe.</span><span class="sxs-lookup"><span data-stu-id="0fb9a-278">These features are designed to efficiently handle long-running requests.</span></span>
