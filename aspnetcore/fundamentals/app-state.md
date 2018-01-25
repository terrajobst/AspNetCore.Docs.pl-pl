---
title: Stan sesji i aplikacji w ASP.NET Core
author: rick-anderson
description: "Podejścia do zachowania aplikacji i stanu użytkowników (sesja) między żądaniami."
ms.author: riande
manager: wpickett
ms.date: 11/27/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e00960370fbe87ac0f81f8455526221fa992decd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a><span data-ttu-id="a23a6-103">Wprowadzenie do stanu sesji oraz aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a23a6-103">Introduction to session and application state in ASP.NET Core</span></span>

<span data-ttu-id="a23a6-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), i [Diana LaRose](https://github.com/DianaLaRose)</span><span class="sxs-lookup"><span data-stu-id="a23a6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Diana LaRose](https://github.com/DianaLaRose)</span></span>

<span data-ttu-id="a23a6-105">HTTP jest protokołem bezstanowe.</span><span class="sxs-lookup"><span data-stu-id="a23a6-105">HTTP is a stateless protocol.</span></span> <span data-ttu-id="a23a6-106">Serwer sieci web traktuje każde żądanie HTTP jako żądanie niezależne i nie zachowują wartości użytkownika z poprzedniego żądania.</span><span class="sxs-lookup"><span data-stu-id="a23a6-106">A web server treats each HTTP request as an independent request and doesn't retain user values from previous requests.</span></span> <span data-ttu-id="a23a6-107">W tym artykule opisano różne sposoby, aby zachować aplikacji i stanu sesji między żądaniami.</span><span class="sxs-lookup"><span data-stu-id="a23a6-107">This article discusses different ways to preserve application and session state between requests.</span></span> 

## <a name="session-state"></a><span data-ttu-id="a23a6-108">Stan sesji</span><span class="sxs-lookup"><span data-stu-id="a23a6-108">Session state</span></span>

<span data-ttu-id="a23a6-109">Stan sesji jest funkcją platformy ASP.NET Core, który służy do zapisywania i przechowywania danych użytkownika, gdy użytkownik będzie przeglądać aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="a23a6-109">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span> <span data-ttu-id="a23a6-110">Składające się tabeli słownika lub skrótu na serwerze, stanu sesji utrzymuje dane żądań w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="a23a6-110">Consisting of a dictionary or hash table on the server, session state persists data across requests from a browser.</span></span> <span data-ttu-id="a23a6-111">Dane sesji jest przechowywana w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="a23a6-111">The session data is backed by a cache.</span></span>

<span data-ttu-id="a23a6-112">Platformy ASP.NET Core zachowuje swój stan sesji, zapewniając klienta pliku cookie, który zawiera identyfikator sesji, który jest wysyłany na serwer z każdym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="a23a6-112">ASP.NET Core maintains session state by giving the client a cookie that contains the session ID, which is sent to the server with each request.</span></span> <span data-ttu-id="a23a6-113">Identyfikator sesji są używane do pobierania danych sesji.</span><span class="sxs-lookup"><span data-stu-id="a23a6-113">The server uses the session ID to fetch the session data.</span></span> <span data-ttu-id="a23a6-114">Ponieważ plik cookie sesji jest specyficzna dla przeglądarki, nie mogą współużytkować sesji w różnych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="a23a6-114">Because the session cookie is specific to the browser, you cannot share sessions across browsers.</span></span> <span data-ttu-id="a23a6-115">Pliki cookie dotyczące sesji są usuwane tylko wtedy, gdy kończy się sesji przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a23a6-115">Session cookies are deleted only when the browser session ends.</span></span> <span data-ttu-id="a23a6-116">Jeśli plik cookie zostanie odebrana dla wygasłych sesji, utworzeniu nowej sesji, która używa tego samego pliku cookie sesji.</span><span class="sxs-lookup"><span data-stu-id="a23a6-116">If a cookie is received for an expired session, a new session that uses the same session cookie  is created.</span></span> 

<span data-ttu-id="a23a6-117">Serwer zachowuje sesję przez ograniczony czas, po zgłoszeniu ostatniego żądania.</span><span class="sxs-lookup"><span data-stu-id="a23a6-117">The server retains a session for a limited time after the last request.</span></span> <span data-ttu-id="a23a6-118">Można ustawić limitu czasu sesji lub użyj wartości domyślnej 20 minut.</span><span class="sxs-lookup"><span data-stu-id="a23a6-118">You can either set the session timeout or use the default value of 20 minutes.</span></span> <span data-ttu-id="a23a6-119">Stan sesji jest idealny dla przechowywania danych użytkownika, która jest specyficzna dla konkretnej sesji, ale nie musi zostać utrwalony trwale.</span><span class="sxs-lookup"><span data-stu-id="a23a6-119">Session state is ideal for storing user data that's specific to a particular session but doesn’t need to be persisted permanently.</span></span> <span data-ttu-id="a23a6-120">Dane są usuwane z magazynu zapasowego albo po wywołaniu `Session.Clear` lub utraty ważności sesji w magazynie danych.</span><span class="sxs-lookup"><span data-stu-id="a23a6-120">Data is deleted from the backing store either when you call `Session.Clear` or when the session expires in the data store.</span></span> <span data-ttu-id="a23a6-121">Serwer nie może ustalić zamknięcia przeglądarki lub usunięcia pliku cookie sesji.</span><span class="sxs-lookup"><span data-stu-id="a23a6-121">The server doesn't know when the browser is closed or when the session cookie is deleted.</span></span>

> [!WARNING]
> <span data-ttu-id="a23a6-122">Nie należy przechowywać poufnych danych w sesji.</span><span class="sxs-lookup"><span data-stu-id="a23a6-122">Don't store sensitive data in session.</span></span> <span data-ttu-id="a23a6-123">Klient nie może być Zamknij przeglądarkę i wyczyść pliku cookie sesji (i w niektórych przeglądarkach podtrzymywania plików cookie sesji w systemie windows).</span><span class="sxs-lookup"><span data-stu-id="a23a6-123">The client might not close the browser and clear the session cookie (and some browsers keep session cookies alive across windows).</span></span> <span data-ttu-id="a23a6-124">Ponadto sesji nie może być ograniczony do jednego użytkownika; Następny użytkownik może kontynuować tej samej sesji.</span><span class="sxs-lookup"><span data-stu-id="a23a6-124">Also, a session might not be restricted to a single user; the next user might continue with the same session.</span></span>

<span data-ttu-id="a23a6-125">Dostawca sesji w pamięci są przechowywane dane sesji na serwerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="a23a6-125">The in-memory session provider stores session data on the local server.</span></span> <span data-ttu-id="a23a6-126">Jeśli planujesz uruchamianie aplikacji sieci web w farmie serwerów, należy użyć trwałe sesje powiązać każdej sesji do określonego serwera.</span><span class="sxs-lookup"><span data-stu-id="a23a6-126">If you plan to run your web app on a server farm, you must use sticky sessions to tie each session to a specific server.</span></span> <span data-ttu-id="a23a6-127">Platforma Windows Azure Web Sites domyślnie trwałe sesje (Routing żądań aplikacji lub ARR).</span><span class="sxs-lookup"><span data-stu-id="a23a6-127">The Windows Azure Web Sites platform defaults to sticky sessions (Application Request Routing or ARR).</span></span> <span data-ttu-id="a23a6-128">Trwałe sesje mogą jednak mieć wpływ na skalowalność i skomplikować aktualizacji aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="a23a6-128">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="a23a6-129">Lepszym rozwiązaniem jest użycie pamięci podręcznej Redis, lub buforować rozproszonych programu SQL Server, które nie wymagają trwałe sesje.</span><span class="sxs-lookup"><span data-stu-id="a23a6-129">A better option is to use the Redis or SQL Server distributed caches, which don't require sticky sessions.</span></span> <span data-ttu-id="a23a6-130">Aby uzyskać więcej informacji, zobacz [Praca z rozproszonej pamięci podręcznej](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="a23a6-130">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="a23a6-131">Aby uzyskać więcej informacji na temat konfigurowania dostawcy usług, zobacz [Konfigurowanie sesji](#configuring-session) dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="a23a6-131">For details on setting up service providers, see [Configuring Session](#configuring-session) later in this article.</span></span>

<a name="temp"></a>
## <a name="tempdata"></a><span data-ttu-id="a23a6-132">TempData</span><span class="sxs-lookup"><span data-stu-id="a23a6-132">TempData</span></span>

<span data-ttu-id="a23a6-133">Przedstawia platformy ASP.NET Core MVC [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) właściwość [kontrolera](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="a23a6-133">ASP.NET Core MVC exposes the [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span></span> <span data-ttu-id="a23a6-134">Ta właściwość przechowuje dane, dopóki nie jest do odczytu.</span><span class="sxs-lookup"><span data-stu-id="a23a6-134">This property stores data until it's read.</span></span> <span data-ttu-id="a23a6-135">`Keep` i `Peek` metod można użyć do sprawdzenia danych bez usuwania.</span><span class="sxs-lookup"><span data-stu-id="a23a6-135">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="a23a6-136">`TempData`jest szczególnie przydatne podczas przekierowania, gdy dane są potrzebne dla więcej niż jednego żądania.</span><span class="sxs-lookup"><span data-stu-id="a23a6-136">`TempData` is particularly useful for redirection, when data is needed for more than a single request.</span></span> <span data-ttu-id="a23a6-137">`TempData`jest implementowany przez dostawców TempData, na przykład za pomocą plików cookie lub stanu sesji.</span><span class="sxs-lookup"><span data-stu-id="a23a6-137">`TempData` is implemented by TempData providers, for example, using either cookies or session state.</span></span>

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a><span data-ttu-id="a23a6-138">TempData dostawców</span><span class="sxs-lookup"><span data-stu-id="a23a6-138">TempData providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a23a6-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a23a6-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a23a6-140">W programie ASP.NET Core 2.0 lub nowszego oraz dostawcy TempData na podstawie plików cookie jest używany domyślnie do przechowywania TempData w plikach cookie.</span><span class="sxs-lookup"><span data-stu-id="a23a6-140">In ASP.NET Core 2.0 and later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="a23a6-141">Dane pliku cookie jest zakodowane za pomocą [Base64UrlTextEncoder](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="a23a6-141">The cookie data is encoded with the [Base64UrlTextEncoder](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span></span> <span data-ttu-id="a23a6-142">Ponieważ plik cookie jest zaszyfrowany i fragmentaryczne, pojedynczy plik cookie rozmiar limit w ASP.NET Core 1.x nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="a23a6-142">Because the cookie is encrypted and chunked, the single cookie size limit found in ASP.NET Core 1.x does not apply.</span></span> <span data-ttu-id="a23a6-143">Ponieważ kompresja zaszyfrowanych danych może prowadzić do problemów z bezpieczeństwem takich jak dane pliku cookie nie jest skompresowany [ataki CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) i [naruszenia](https://wikipedia.org/wiki/BREACH_(security_exploit)) ataków.</span><span class="sxs-lookup"><span data-stu-id="a23a6-143">The cookie data is not compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="a23a6-144">Aby uzyskać więcej informacji o dostawcy TempData na podstawie plików cookie, zobacz [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span><span class="sxs-lookup"><span data-stu-id="a23a6-144">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a23a6-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a23a6-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a23a6-146">Program ASP.NET Core 1.0 i 1.1 dostawca TempData stanu sesji jest ustawieniem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="a23a6-146">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default.</span></span>

--------------

<a name="choose-temp"></a>
### <a name="choosing-a-tempdata-provider"></a><span data-ttu-id="a23a6-147">Wybieranie dostawcy TempData</span><span class="sxs-lookup"><span data-stu-id="a23a6-147">Choosing a TempData provider</span></span>

<span data-ttu-id="a23a6-148">Wybieranie dostawcy TempData obejmuje kilka kwestii, takich jak:</span><span class="sxs-lookup"><span data-stu-id="a23a6-148">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="a23a6-149">Aplikacja już używa stanu sesji dla innych celów?</span><span class="sxs-lookup"><span data-stu-id="a23a6-149">Does the application already use session state for other purposes?</span></span> <span data-ttu-id="a23a6-150">Jeśli jest to tak, przy użyciu dostawcy TempData stan sesji nie ma żadnych dodatkowych kosztów do aplikacji (jako uzupełnienie rozmiar danych).</span><span class="sxs-lookup"><span data-stu-id="a23a6-150">If so, using the session state TempData provider has no additional cost to the application (aside from the size of the data).</span></span>
2. <span data-ttu-id="a23a6-151">Aplikacja używa TempData tylko oszczędnie przypadku względnie niewielkich ilości danych (maksymalnie 500 bajtów)?</span><span class="sxs-lookup"><span data-stu-id="a23a6-151">Does the application use TempData only sparingly, for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="a23a6-152">Jeśli tak, dostawcy TempData pliku cookie na każde żądanie, który przenosi TempData doda małych kosztów.</span><span class="sxs-lookup"><span data-stu-id="a23a6-152">If so, the cookie TempData provider will add a small cost to each request that carries TempData.</span></span> <span data-ttu-id="a23a6-153">Jeśli nie, dostawca TempData stanu sesji korzystne może okazać się uniknąć dwustronną komunikację dużej ilości danych do wszystkich żądań do momentu TempData jest używany.</span><span class="sxs-lookup"><span data-stu-id="a23a6-153">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="a23a6-154">Czy aplikacja działa w kolektywie serwerów sieci web (wiele serwerów)?</span><span class="sxs-lookup"><span data-stu-id="a23a6-154">Does the application run in a web farm (multiple servers)?</span></span> <span data-ttu-id="a23a6-155">Jeśli tak, nie istnieje żadne dodatkowe czynności konfiguracyjne niezbędne do używania dostawcy TempData pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="a23a6-155">If so, there's no additional configuration needed to use the cookie TempData provider.</span></span>

> [!NOTE]
> <span data-ttu-id="a23a6-156">Większość klientów w sieci web (na przykład przeglądarki sieci web) wymuszać ograniczenia dotyczące maksymalny rozmiar każdego pliku cookie i całkowita liczba plików cookie.</span><span class="sxs-lookup"><span data-stu-id="a23a6-156">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="a23a6-157">W związku z tym podczas korzystania z dostawcy TempData pliku cookie, sprawdź, czy aplikacja nie przekroczy te ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="a23a6-157">Therefore, when using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="a23a6-158">Należy wziąć pod uwagę całkowity rozmiar danych, ewidencjonowanie aktywności traktowane jako zapasy szyfrowania i podziału.</span><span class="sxs-lookup"><span data-stu-id="a23a6-158">Consider the total size of the data, accounting for the overheads of encryption and chunking.</span></span>

<a name="config-temp"></a>
### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="a23a6-159">Konfigurowanie dostawcy TempData</span><span class="sxs-lookup"><span data-stu-id="a23a6-159">Configure the TempData provider</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a23a6-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a23a6-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a23a6-161">Dostawca TempData na podstawie plików cookie jest domyślnie włączona.</span><span class="sxs-lookup"><span data-stu-id="a23a6-161">The cookie-based TempData provider is enabled by default.</span></span> <span data-ttu-id="a23a6-162">Następujące `Startup` kod klasy konfiguruje dostawcy TempData opartymi na sesji:</span><span class="sxs-lookup"><span data-stu-id="a23a6-162">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a23a6-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a23a6-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a23a6-164">Następujące `Startup` kod klasy konfiguruje dostawcy TempData opartymi na sesji:</span><span class="sxs-lookup"><span data-stu-id="a23a6-164">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

<span data-ttu-id="a23a6-165">Kolejność jest kluczowa dla składników oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="a23a6-165">Ordering is critical for middleware components.</span></span> <span data-ttu-id="a23a6-166">W powyższym przykładzie wyjątek typu `InvalidOperationException` występuje, gdy `UseSession` jest wywoływana po `UseMvcWithDefaultRoute`.</span><span class="sxs-lookup"><span data-stu-id="a23a6-166">In the preceding example, an exception of type `InvalidOperationException` occurs when `UseSession` is invoked after `UseMvcWithDefaultRoute`.</span></span> <span data-ttu-id="a23a6-167">Zobacz [kolejność oprogramowania pośredniczącego](xref:fundamentals/middleware#ordering) uzyskać więcej szczegółowych informacji.</span><span class="sxs-lookup"><span data-stu-id="a23a6-167">See [Middleware Ordering](xref:fundamentals/middleware#ordering) for more detail.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a23a6-168">Jeśli przeznaczonych dla platformy .NET Framework i przy użyciu dostawcy opartymi na sesji, Dodaj [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) pakiet NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="a23a6-168">If targeting .NET Framework and using the session-based provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet package to your project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="a23a6-169">Ciągi zapytań</span><span class="sxs-lookup"><span data-stu-id="a23a6-169">Query strings</span></span>

<span data-ttu-id="a23a6-170">Można przekazać ograniczoną ilość danych z jednego żądania do innego przez dodanie go do nowego żądania ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="a23a6-170">You can pass a limited amount of data from one request to another by adding it to the new request’s query string.</span></span> <span data-ttu-id="a23a6-171">Jest to przydatne w przypadku przechwytywania stanu w sposób ciągły, umożliwiający łącza osadzonego stanu do udostępnienia za pośrednictwem poczty e-mail lub sieci społecznościowych.</span><span class="sxs-lookup"><span data-stu-id="a23a6-171">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="a23a6-172">Jednak z tego powodu należy nigdy używać ciągów zapytania dla danych poufnych.</span><span class="sxs-lookup"><span data-stu-id="a23a6-172">However, for this reason, you should never use query strings for sensitive data.</span></span> <span data-ttu-id="a23a6-173">Oprócz łatwo udostępnianiem, włącznie z danymi w ciągów zapytania można tworzyć możliwości [sfałszowaniem żądań Cross-Site (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) ataków, których można wymuszać użytkowników do odwiedzenia złośliwych witryn podczas uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="a23a6-173">In addition to being easily shared, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="a23a6-174">Osoby atakujące można następnie kradzieży danych użytkownika z aplikacji lub podjąć złośliwe akcje w imieniu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a23a6-174">Attackers can then steal user data from your app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="a23a6-175">Każdy zachowanego stan application lub session należy chronić przed atakami CSRF.</span><span class="sxs-lookup"><span data-stu-id="a23a6-175">Any preserved application or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="a23a6-176">Aby uzyskać więcej informacji dotyczących ataków CSRF, zobacz [zapobieganie Cross-Site żądania (XSRF/CSRF) Fałszerstwie w ASP.NET Core](../security/anti-request-forgery.md).</span><span class="sxs-lookup"><span data-stu-id="a23a6-176">For more information on CSRF attacks, see [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](../security/anti-request-forgery.md).</span></span>

## <a name="post-data-and-hidden-fields"></a><span data-ttu-id="a23a6-177">Dane POST i ukryte pola</span><span class="sxs-lookup"><span data-stu-id="a23a6-177">Post data and hidden fields</span></span>

<span data-ttu-id="a23a6-178">Dane można zapisane w ukrytym pól i opublikować ponownie przy kolejnym żądaniu.</span><span class="sxs-lookup"><span data-stu-id="a23a6-178">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="a23a6-179">To jest typowe w formularzach wiele stron.</span><span class="sxs-lookup"><span data-stu-id="a23a6-179">This is common in multi-page forms.</span></span> <span data-ttu-id="a23a6-180">Jednak ponieważ klienta może potencjalnie manipulować danymi, serwer musi zawsze ponownie sprawdź poprawność go.</span><span class="sxs-lookup"><span data-stu-id="a23a6-180">However, because the client can potentially tamper with the data, the server must always revalidate it.</span></span> 

## <a name="cookies"></a><span data-ttu-id="a23a6-181">Pliki cookie</span><span class="sxs-lookup"><span data-stu-id="a23a6-181">Cookies</span></span>

<span data-ttu-id="a23a6-182">Pliki cookie umożliwiają przechowywanie danych użytkownika w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="a23a6-182">Cookies provide a way to store user-specific data in web applications.</span></span> <span data-ttu-id="a23a6-183">Ponieważ pliki cookie są wysyłane z każdym żądaniem, ich rozmiar powinny być ograniczone do minimum.</span><span class="sxs-lookup"><span data-stu-id="a23a6-183">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="a23a6-184">Najlepiej, jeśli tylko identyfikator powinny być przechowywane w pliku cookie z rzeczywistymi danymi przechowywanymi na serwerze.</span><span class="sxs-lookup"><span data-stu-id="a23a6-184">Ideally, only an identifier should be stored in a cookie with the actual data stored on the server.</span></span> <span data-ttu-id="a23a6-185">W większości przeglądarek ograniczyć plików cookie do 4096 bajtów.</span><span class="sxs-lookup"><span data-stu-id="a23a6-185">Most browsers restrict cookies to 4096 bytes.</span></span> <span data-ttu-id="a23a6-186">Ponadto ograniczonej liczby plików cookie, są dostępne dla każdej domeny.</span><span class="sxs-lookup"><span data-stu-id="a23a6-186">In addition, only a limited number of cookies are available for each domain.</span></span>  

<span data-ttu-id="a23a6-187">Ponieważ pliki cookie podlegają naruszeniu, musi zostać zweryfikowany na serwerze.</span><span class="sxs-lookup"><span data-stu-id="a23a6-187">Because cookies are subject to tampering, they must be validated on the server.</span></span> <span data-ttu-id="a23a6-188">Trwałość pliku cookie na komputerze klienckim podlega interwencji użytkownika i wygaśnięcia, ale zazwyczaj są one najbardziej niezawodna formę trwałości danych na kliencie.</span><span class="sxs-lookup"><span data-stu-id="a23a6-188">Although the durability of the cookie on a client is subject to user intervention and expiration, they're generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="a23a6-189">Pliki cookie są często używane na potrzeby personalizacji, gdy zawartość jest dostosowany do znanego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a23a6-189">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="a23a6-190">Ponieważ użytkownik tylko zidentyfikowane i nie jest uwierzytelniony w większości przypadków, zwykle można zabezpieczyć plik cookie przez zapisanie nazwę użytkownika, nazwę konta lub unikatowe Identyfikatory (na przykład identyfikator GUID) w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="a23a6-190">Because the user is only identified and not authenticated in most cases, you can typically secure a cookie by storing the user name, account name, or a unique user ID (such as a GUID) in the cookie.</span></span> <span data-ttu-id="a23a6-191">Plik cookie można następnie użyć uzyskiwać dostęp do infrastruktury personalizacji użytkownika witryny.</span><span class="sxs-lookup"><span data-stu-id="a23a6-191">You can then use the cookie to access the user personalization infrastructure of a site.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="a23a6-192">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="a23a6-192">HttpContext.Items</span></span>

<span data-ttu-id="a23a6-193">`Items` Kolekcja jest dobrym miejscem do przechowywania danych, które ma potrzebne tylko podczas przetwarzania jednego określonego żądania.</span><span class="sxs-lookup"><span data-stu-id="a23a6-193">The `Items` collection is a good location to store data that's needed only while processing one particular request.</span></span> <span data-ttu-id="a23a6-194">Zawartość kolekcji zostaną odrzucone po każdym żądaniu.</span><span class="sxs-lookup"><span data-stu-id="a23a6-194">The collection's contents are discarded after each request.</span></span> <span data-ttu-id="a23a6-195">`Items` Kolekcja najlepiej jest używana jako sposób składniki lub oprogramowanie pośredniczące do komunikacji, gdy działają w różnych punktach w czasie żądania i nie może bezpośrednio do przekazania parametrów.</span><span class="sxs-lookup"><span data-stu-id="a23a6-195">The `Items` collection is best used as a way for components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span> <span data-ttu-id="a23a6-196">Aby uzyskać więcej informacji, zobacz [Praca z HttpContext.Items](#working-with-httpcontextitems)w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="a23a6-196">For more information, see [Working with HttpContext.Items](#working-with-httpcontextitems), later in this article.</span></span>

## <a name="cache"></a><span data-ttu-id="a23a6-197">Pamięć podręczna</span><span class="sxs-lookup"><span data-stu-id="a23a6-197">Cache</span></span>

<span data-ttu-id="a23a6-198">Buforowanie jest wydajny sposób przechowywania i pobierania danych.</span><span class="sxs-lookup"><span data-stu-id="a23a6-198">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="a23a6-199">Można kontrolować okres istnienia pamięci podręcznej elementów na podstawie czasu i innych kwestii.</span><span class="sxs-lookup"><span data-stu-id="a23a6-199">You can control the lifetime of cached items based on time and other considerations.</span></span> <span data-ttu-id="a23a6-200">Dowiedz się więcej o [buforowanie](../performance/caching/index.md).</span><span class="sxs-lookup"><span data-stu-id="a23a6-200">Learn more about [Caching](../performance/caching/index.md).</span></span>

<a name="session"></a>
## <a name="working-with-session-state"></a><span data-ttu-id="a23a6-201">Praca z stanu sesji</span><span class="sxs-lookup"><span data-stu-id="a23a6-201">Working with Session State</span></span>

### <a name="configuring-session"></a><span data-ttu-id="a23a6-202">Konfigurowanie sesji</span><span class="sxs-lookup"><span data-stu-id="a23a6-202">Configuring Session</span></span>

<span data-ttu-id="a23a6-203">`Microsoft.AspNetCore.Session` Pakietu udostępnia oprogramowanie pośredniczące do zarządzania stanem sesji.</span><span class="sxs-lookup"><span data-stu-id="a23a6-203">The `Microsoft.AspNetCore.Session` package provides middleware for managing session state.</span></span> <span data-ttu-id="a23a6-204">Aby włączyć sesji oprogramowanie pośredniczące, `Startup` musi zawierać:</span><span class="sxs-lookup"><span data-stu-id="a23a6-204">To enable the session middleware, `Startup` must contain:</span></span>

- <span data-ttu-id="a23a6-205">Żadnego z [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) pamięci podręcznej pamięci.</span><span class="sxs-lookup"><span data-stu-id="a23a6-205">Any of the [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="a23a6-206">`IDistributedCache` Implementacji jest używany jako magazynu zapasowego dla sesji.</span><span class="sxs-lookup"><span data-stu-id="a23a6-206">The `IDistributedCache` implementation is used as a backing store for session.</span></span>
- <span data-ttu-id="a23a6-207">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) wywołać, co wymaga pakietu NuGet "Microsoft.AspNetCore.Session".</span><span class="sxs-lookup"><span data-stu-id="a23a6-207">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) call, which requires NuGet package "Microsoft.AspNetCore.Session".</span></span>
- <span data-ttu-id="a23a6-208">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) wywołania.</span><span class="sxs-lookup"><span data-stu-id="a23a6-208">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) call.</span></span>

<span data-ttu-id="a23a6-209">Poniższy kod przedstawia, jak skonfigurować dostawcę sesji w pamięci.</span><span class="sxs-lookup"><span data-stu-id="a23a6-209">The following code shows how to set up the in-memory session provider.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a23a6-210">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a23a6-210">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a23a6-211">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a23a6-211">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

<span data-ttu-id="a23a6-212">Możesz odwoływać się do sesji z `HttpContext` po zostanie on zainstalowany i skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="a23a6-212">You can reference Session from `HttpContext` once it's installed and configured.</span></span>

<span data-ttu-id="a23a6-213">Jeśli użytkownik próbuje uzyskać dostęp `Session` przed `UseSession` została wywołana, wyjątek `InvalidOperationException: Session has not been configured for this application or request` jest generowany.</span><span class="sxs-lookup"><span data-stu-id="a23a6-213">If you try to access `Session` before `UseSession` has been called, the exception `InvalidOperationException: Session has not been configured for this application or request` is thrown.</span></span>

<span data-ttu-id="a23a6-214">Jeśli próbujesz utworzyć nowy `Session` (to znaczy, że pliki cookie sesji nie została utworzona) po już Rozpoczęto zapisywanie `Response` strumienia wyjątek `InvalidOperationException: The session cannot be established after the response has started` jest generowany.</span><span class="sxs-lookup"><span data-stu-id="a23a6-214">If you try to create a new `Session` (that is, no session cookie has been created) after you have already begun writing to the `Response` stream, the exception `InvalidOperationException: The session cannot be established after the response has started` is thrown.</span></span> <span data-ttu-id="a23a6-215">Wyjątek można znaleźć w dzienniku serwera sieci web; nie będzie wyświetlana w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="a23a6-215">The exception can be found in the web server log; it will not be displayed in the browser.</span></span>

### <a name="loading-session-asynchronously"></a><span data-ttu-id="a23a6-216">Ładowany asynchronicznie sesji</span><span class="sxs-lookup"><span data-stu-id="a23a6-216">Loading Session asynchronously</span></span> 

<span data-ttu-id="a23a6-217">Domyślny dostawca sesji w ASP.NET Core ładuje rekordu sesji z podstawową [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) magazynu asynchronicznie tylko wtedy, gdy [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) metoda jawnie jest wywoływana przed  `TryGetValue`, `Set`, lub `Remove` metody.</span><span class="sxs-lookup"><span data-stu-id="a23a6-217">The default session provider in ASP.NET Core loads the session record from the underlying [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) store asynchronously only if the [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) method is explicitly called before  the `TryGetValue`, `Set`, or `Remove` methods.</span></span> <span data-ttu-id="a23a6-218">Jeśli `LoadAsync` nie jest wywoływany jako pierwszy, odpowiadającego rekordu sesji jest ładowany synchronicznie, które mogą potencjalnie wpłynąć na możliwość skalowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a23a6-218">If `LoadAsync` isn't called first, the underlying session record is loaded synchronously, which could potentially impact the ability of the app to scale.</span></span>

<span data-ttu-id="a23a6-219">Aby wymusić ten wzorzec aplikacji, zawijać [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) i [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementacje wersje zgłosić wyjątek, jeśli `LoadAsync` — metoda nie jest wywoływana przed `TryGetValue`, `Set`, lub `Remove`.</span><span class="sxs-lookup"><span data-stu-id="a23a6-219">To have applications enforce this pattern, wrap the [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method isn't called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="a23a6-220">Zarejestruj opakowana wersje w kontenerze usług.</span><span class="sxs-lookup"><span data-stu-id="a23a6-220">Register the wrapped versions in the services container.</span></span>

### <a name="implementation-details"></a><span data-ttu-id="a23a6-221">Szczegóły implementacji</span><span class="sxs-lookup"><span data-stu-id="a23a6-221">Implementation Details</span></span>

<span data-ttu-id="a23a6-222">Sesja używa pliku cookie do śledzenia i zidentyfikować żądań z jednej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a23a6-222">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="a23a6-223">Domyślnie ten plik cookie o nazwie ". AspNet.Session"i używa ścieżką"/".</span><span class="sxs-lookup"><span data-stu-id="a23a6-223">By default, this cookie is named ".AspNet.Session", and it uses a path of "/".</span></span> <span data-ttu-id="a23a6-224">Ponieważ domyślny plik cookie nie określa domeny, nie zostało to zrobione dla skryptu po stronie klienta na stronie (ponieważ `CookieHttpOnly` domyślnie `true`).</span><span class="sxs-lookup"><span data-stu-id="a23a6-224">Because the cookie default doesn't specify a domain, it's not made available to the client-side script on the page (because `CookieHttpOnly` defaults to `true`).</span></span>

<span data-ttu-id="a23a6-225">Aby zastąpić wartości domyślne sesji, użyj `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="a23a6-225">To override session defaults, use `SessionOptions`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a23a6-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a23a6-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a23a6-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a23a6-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

<span data-ttu-id="a23a6-228">Serwer używa `IdleTimeout` właściwości w celu określenia, jak długo sesji może być bezczynne, zanim zostały porzucone jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="a23a6-228">The server uses the `IdleTimeout` property to determine how long a session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="a23a6-229">Ta właściwość jest niezależna od datę ważności pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="a23a6-229">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="a23a6-230">Każde żądanie, który przechodzi przez oprogramowanie pośredniczące sesji (odczytywane lub zapisywane) resetuje limit czasu.</span><span class="sxs-lookup"><span data-stu-id="a23a6-230">Each request that passes through the Session middleware (read from or written to) resets the timeout.</span></span>

<span data-ttu-id="a23a6-231">Ponieważ `Session` jest *— blokowanie*, jeśli dwa żądania zarówno próbę zmodyfikowania zawartości sesji, ostatnią przesłania pierwszego.</span><span class="sxs-lookup"><span data-stu-id="a23a6-231">Because `Session` is *non-locking*, if two requests both attempt to modify the contents of session, the last one overrides the first.</span></span> <span data-ttu-id="a23a6-232">`Session`jest zaimplementowany jako *spójnego sesji*, co oznacza, że cała zawartość są przechowywane razem.</span><span class="sxs-lookup"><span data-stu-id="a23a6-232">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="a23a6-233">Dwa żądania, które modyfikowania różnych części sesji (różne klucze) nadal może mieć wpływ na siebie.</span><span class="sxs-lookup"><span data-stu-id="a23a6-233">Two requests that are modifying different parts of the session (different keys) might still impact each other.</span></span>

### <a name="setting-and-getting-session-values"></a><span data-ttu-id="a23a6-234">Ustawianie i pobieranie wartości sesji</span><span class="sxs-lookup"><span data-stu-id="a23a6-234">Setting and getting Session values</span></span>

<span data-ttu-id="a23a6-235">Sesja jest dostępny za pośrednictwem `Session` właściwość `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="a23a6-235">Session is accessed through the `Session` property on `HttpContext`.</span></span> <span data-ttu-id="a23a6-236">Ta właściwość jest [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementacji.</span><span class="sxs-lookup"><span data-stu-id="a23a6-236">This property is an [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

<span data-ttu-id="a23a6-237">W poniższym przykładzie pokazano, ustawiania i pobierania int i ciąg:</span><span class="sxs-lookup"><span data-stu-id="a23a6-237">The following example shows setting and getting an int and a string:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

<span data-ttu-id="a23a6-238">Jeśli dodasz następujące metody rozszerzenia, należy Ustawianie i pobieranie obiekty możliwe do serializacji do sesji:</span><span class="sxs-lookup"><span data-stu-id="a23a6-238">If you add the following extension methods, you can set and get serializable objects to Session:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

<span data-ttu-id="a23a6-239">Poniższy przykład przedstawia sposób Ustawianie i pobieranie obiektu podlegającego serializacji:</span><span class="sxs-lookup"><span data-stu-id="a23a6-239">The following sample shows how to set and get a serializable object:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a><span data-ttu-id="a23a6-240">Praca z HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="a23a6-240">Working with HttpContext.Items</span></span>

<span data-ttu-id="a23a6-241">`HttpContext` Abstrakcji zapewnia obsługę kolekcji słownika typu `IDictionary<object, object>`o nazwie `Items`.</span><span class="sxs-lookup"><span data-stu-id="a23a6-241">The `HttpContext` abstraction provides support for a dictionary collection of type `IDictionary<object, object>`, called `Items`.</span></span> <span data-ttu-id="a23a6-242">Ta kolekcja jest dostępny od początku *HttpRequest* i zostaną odrzucone pod koniec każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="a23a6-242">This collection is available from the start of an *HttpRequest* and is discarded at the end of each request.</span></span> <span data-ttu-id="a23a6-243">Można do niego dostęp, przypisując wartość wpisu kluczem lub przez zażądanie wartość dla określonego klucza.</span><span class="sxs-lookup"><span data-stu-id="a23a6-243">You can access it by  assigning a value to a keyed entry, or by requesting the value for a particular key.</span></span>

<span data-ttu-id="a23a6-244">W przykładzie poniżej [oprogramowanie pośredniczące](middleware.md) dodaje `isVerified` do `Items` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="a23a6-244">In the sample below, [Middleware](middleware.md) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="a23a6-245">Później w potoku innego oprogramowania pośredniczącego można do niego dostęp:</span><span class="sxs-lookup"><span data-stu-id="a23a6-245">Later in the pipeline, another middleware could access it:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

<span data-ttu-id="a23a6-246">Dla oprogramowania pośredniczącego, która będzie używana tylko w jednej aplikacji `string` klucze są akceptowane.</span><span class="sxs-lookup"><span data-stu-id="a23a6-246">For middleware that will only be used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="a23a6-247">Jednak aby uniknąć wysłanej kolizji kluczy obiekt unikatowy kluczy należy używać oprogramowania pośredniczącego, które będą udostępniane między aplikacjami.</span><span class="sxs-lookup"><span data-stu-id="a23a6-247">However, middleware that will be shared between applications should use unique object keys to avoid any chance of key collisions.</span></span> <span data-ttu-id="a23a6-248">Jeśli tworzysz oprogramowania pośredniczącego, które muszą działać dla wielu aplikacji, należy użyć klucza unikatowym obiektem zdefiniowany w klasie oprogramowanie pośredniczące, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="a23a6-248">If you are developing middleware that must work across multiple applications, use a unique object key defined in your middleware class as shown below:</span></span>

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

<span data-ttu-id="a23a6-249">Inny kod, mogą uzyskiwać dostęp do wartości przechowywanej w `HttpContext.Items` przy użyciu klucza udostępniane przez oprogramowanie pośredniczące klasy:</span><span class="sxs-lookup"><span data-stu-id="a23a6-249">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

<span data-ttu-id="a23a6-250">Takie podejście charakteryzuje się również zaletą wyeliminowanie powtórzenia "magic ciągi" w kilku miejscach w kodzie.</span><span class="sxs-lookup"><span data-stu-id="a23a6-250">This approach also has the advantage of eliminating repetition of "magic strings" in multiple places in the code.</span></span>

<a name="appstate-errors"></a>

## <a name="application-state-data"></a><span data-ttu-id="a23a6-251">Dane o stanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="a23a6-251">Application state data</span></span>

<span data-ttu-id="a23a6-252">Użyj [iniekcji zależności](xref:fundamentals/dependency-injection) udostępnić dane dla wszystkich użytkowników:</span><span class="sxs-lookup"><span data-stu-id="a23a6-252">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="a23a6-253">Zdefiniuj usługą zawierającego dane (na przykład klasa o nazwie `MyAppData`).</span><span class="sxs-lookup"><span data-stu-id="a23a6-253">Define a service containing the data (for example, a class named `MyAppData`).</span></span>

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. <span data-ttu-id="a23a6-254">Dodawanie klasy usługi do `ConfigureServices` (na przykład `services.AddSingleton<MyAppData>();`).</span><span class="sxs-lookup"><span data-stu-id="a23a6-254">Add the service class to `ConfigureServices` (for example `services.AddSingleton<MyAppData>();`).</span></span>
3. <span data-ttu-id="a23a6-255">Korzystanie z klasy usługi danych w każdym kontrolerze:</span><span class="sxs-lookup"><span data-stu-id="a23a6-255">Consume the data service class in each controller:</span></span>

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

## <a name="common-errors-when-working-with-session"></a><span data-ttu-id="a23a6-256">Typowe błędy podczas pracy z sesji</span><span class="sxs-lookup"><span data-stu-id="a23a6-256">Common errors when working with session</span></span>

* <span data-ttu-id="a23a6-257">"Nie można rozpoznać usługi dla typu"Microsoft.Extensions.Caching.Distributed.IDistributedCache"podczas próby aktywowania"Microsoft.AspNetCore.Session.DistributedSessionStore"."</span><span class="sxs-lookup"><span data-stu-id="a23a6-257">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="a23a6-258">Jest to zazwyczaj spowodowane nie powiodło się skonfigurowanie co najmniej jednego `IDistributedCache` implementacji.</span><span class="sxs-lookup"><span data-stu-id="a23a6-258">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="a23a6-259">Aby uzyskać więcej informacji, zobacz [Praca z rozproszonej pamięci podręcznej](xref:performance/caching/distributed) i [w ramach buforowania pamięci](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="a23a6-259">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed) and [In memory caching](xref:performance/caching/memory).</span></span>

* <span data-ttu-id="a23a6-260">W przypadku który sesji oprogramowanie pośredniczące nie powiedzie się, aby utrwalić sesji (na przykład: Jeśli bazy danych nie jest dostępna), rejestruje wyjątek i swallows go.</span><span class="sxs-lookup"><span data-stu-id="a23a6-260">In the event that the session middleware fails to persist a session (for example: if the database isn't available), it logs the exception and swallows it.</span></span> <span data-ttu-id="a23a6-261">Żądanie będzie się nadal normalnie, która prowadzi do bardzo nieprzewidywalne zachowanie.</span><span class="sxs-lookup"><span data-stu-id="a23a6-261">The request will then continue normally, which leads to very unpredictable behavior.</span></span>

<span data-ttu-id="a23a6-262">Typowym przykładem:</span><span class="sxs-lookup"><span data-stu-id="a23a6-262">A typical example:</span></span>

<span data-ttu-id="a23a6-263">Ktoś przechowuje koszyka zakupów w sesji.</span><span class="sxs-lookup"><span data-stu-id="a23a6-263">Someone stores a shopping basket in session.</span></span> <span data-ttu-id="a23a6-264">Użytkownik dodaje elementu, ale zatwierdzenia kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="a23a6-264">The user adds an item but the commit fails.</span></span> <span data-ttu-id="a23a6-265">Aplikacja nie może ustalić o awarii, zgłasza komunikat "element został dodany", który nie jest spełniony.</span><span class="sxs-lookup"><span data-stu-id="a23a6-265">The app doesn't know about the failure so it reports the message "The item has been added", which isn't true.</span></span>

<span data-ttu-id="a23a6-266">Zalecanym sposobem Sprawdź, czy błędy takie jest wywołać `await feature.Session.CommitAsync();` z kodu aplikacji, gdy wszystko będzie gotowe zapisywania do sesji.</span><span class="sxs-lookup"><span data-stu-id="a23a6-266">The recommended way to check for such errors is to call `await feature.Session.CommitAsync();` from app code when you're done writing to the session.</span></span> <span data-ttu-id="a23a6-267">Następnie możesz zrobić, takich jak z powodu błędu.</span><span class="sxs-lookup"><span data-stu-id="a23a6-267">Then you can do what you like with the error.</span></span> <span data-ttu-id="a23a6-268">Działa tak samo, podczas wywoływania metody `LoadAsync`.</span><span class="sxs-lookup"><span data-stu-id="a23a6-268">It works the same way when calling `LoadAsync`.</span></span>


### <a name="additional-resources"></a><span data-ttu-id="a23a6-269">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a23a6-269">Additional Resources</span></span>


* [<span data-ttu-id="a23a6-270">Platformy ASP.NET Core 1.x: Przykładowy kod używany w tym dokumencie</span><span class="sxs-lookup"><span data-stu-id="a23a6-270">ASP.NET Core 1.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [<span data-ttu-id="a23a6-271">Platformy ASP.NET Core 2.x: Przykładowy kod używany w tym dokumencie</span><span class="sxs-lookup"><span data-stu-id="a23a6-271">ASP.NET Core 2.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
