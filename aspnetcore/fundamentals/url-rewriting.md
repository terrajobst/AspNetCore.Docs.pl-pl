---
title: "Adres URL ponowne zapisanie oprogramowania pośredniczącego w platformy ASP.NET Core"
author: guardrex
description: "Więcej informacji na temat adresu URL ponowne zapisywanie i przekierowywania z pośredniczącym ponowne zapisywanie adresów URL w aplikacji platformy ASP.NET Core."
keywords: "Platformy ASP.NET Core ponowne zapisywanie ponowne zapisywanie adresów URL, przekierowywanie adresu URL, adres URL przekierowania, oprogramowanie pośredniczące, apache_mod adresu URL"
ms.author: riande
manager: wpickett
ms.date: 08/17/2017
ms.topic: article
ms.assetid: e6130638-c410-4161-9921-b658ce988bd1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/url-rewriting
ms.openlocfilehash: e07634a6d7ad97bf8735029b5c28d6935b71eb52
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2018
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="854d2-104">Adres URL ponowne zapisanie oprogramowania pośredniczącego w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="854d2-104">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="854d2-105">Przez [Luke Latham](https://github.com/guardrex) i [Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="854d2-105">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="854d2-106">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="854d2-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="854d2-107">Ponowne zapisywanie adresów URL jest czynnością modyfikowania żądania, których adresy URL na podstawie co najmniej jeden wstępnie zdefiniowanych reguł.</span><span class="sxs-lookup"><span data-stu-id="854d2-107">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="854d2-108">Ponowne zapisywanie adresów URL powoduje abstrakcję między lokalizacje zasobów i ich adresów, tak aby lokalizacje i adresy nie są ściśle powiązane.</span><span class="sxs-lookup"><span data-stu-id="854d2-108">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="854d2-109">Istnieje kilka scenariuszy, w których jest przydatna ponowne zapisywanie adresów URL:</span><span class="sxs-lookup"><span data-stu-id="854d2-109">There are several scenarios where URL rewriting is valuable:</span></span>
* <span data-ttu-id="854d2-110">Przenoszenie lub zastępowanie tymczasowo lub trwale zasoby serwera przy zachowaniu stabilna lokalizatorów dla tych zasobów</span><span class="sxs-lookup"><span data-stu-id="854d2-110">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources</span></span>
* <span data-ttu-id="854d2-111">Dzielenie żądania przetwarzania różnych aplikacji lub obszarów jednej aplikacji</span><span class="sxs-lookup"><span data-stu-id="854d2-111">Splitting request processing across different apps or across areas of one app</span></span>
* <span data-ttu-id="854d2-112">Usuwanie, dodając lub reorganizacja segmenty adresu URL na przychodzące żądania</span><span class="sxs-lookup"><span data-stu-id="854d2-112">Removing, adding, or reorganizing URL segments on incoming requests</span></span>
* <span data-ttu-id="854d2-113">Optymalizacja publiczne adresy URL do optymalizacji aparatu wyszukiwania (SEO)</span><span class="sxs-lookup"><span data-stu-id="854d2-113">Optimizing public URLs for Search Engine Optimization (SEO)</span></span>
* <span data-ttu-id="854d2-114">Pozwalające na używanie przyjaznych adresów URL publiczne, ułatwiające osobom prognozowania zawartość, którą znajdą, wykonując następujące łącze</span><span class="sxs-lookup"><span data-stu-id="854d2-114">Permitting the use of friendly public URLs to help people predict the content they will find by following a link</span></span>
* <span data-ttu-id="854d2-115">Przekierowywanie żądań niezabezpieczonego do bezpiecznego punkty końcowe</span><span class="sxs-lookup"><span data-stu-id="854d2-115">Redirecting insecure requests to secure endpoints</span></span>
* <span data-ttu-id="854d2-116">Zapobieganie hotlinking obrazu</span><span class="sxs-lookup"><span data-stu-id="854d2-116">Preventing image hotlinking</span></span>

<span data-ttu-id="854d2-117">Można zdefiniować reguły zmiana adresu URL na kilka sposobów, łącznie z wyrażenia regularnego, Apache mod_rewrite modułu zasad, zasady przepisywania moduł usług IIS i przy użyciu reguły niestandardowej logiki.</span><span class="sxs-lookup"><span data-stu-id="854d2-117">You can define rules for changing the URL in several ways, including regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="854d2-118">Ten dokument zawiera instrukcje dotyczące sposobu używania pośredniczącym ponowne zapisywanie adresów URL w aplikacji platformy ASP.NET Core ponowne zapisywanie adresów URL.</span><span class="sxs-lookup"><span data-stu-id="854d2-118">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="854d2-119">Ponowne zapisywanie adresów URL może zmniejszyć wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="854d2-119">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="854d2-120">W przypadku, gdy jest to możliwe, należy ograniczyć liczba i złożoność reguł.</span><span class="sxs-lookup"><span data-stu-id="854d2-120">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="854d2-121">Ponowne zapisywanie adresów URL przekierowania i adres URL</span><span class="sxs-lookup"><span data-stu-id="854d2-121">URL redirect and URL rewrite</span></span>
<span data-ttu-id="854d2-122">Różnica w treść między *adres URL przekierowania* i *ponowne zapisywanie adresów URL* może wydawać się niewielkie w pierwszym, ale ma istotny wpływ na zapewnianie zasobów na klientach.</span><span class="sxs-lookup"><span data-stu-id="854d2-122">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="854d2-123">Ponowne zapisywanie adresów URL platformy ASP.NET Core w oprogramowaniu pośredniczącym jest w stanie spełniających potrzeby dla obu.</span><span class="sxs-lookup"><span data-stu-id="854d2-123">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="854d2-124">A *adres URL przekierowania* jest operacją po stronie klienta, w którym klient otrzymuje instrukcję do uzyskania dostępu do zasobu na inny adres.</span><span class="sxs-lookup"><span data-stu-id="854d2-124">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="854d2-125">To wymaga przesłania danych do serwera, a adres URL przekierowania do klienta zwracany jest wyświetlany w pasku adresu przeglądarki, gdy klient wysyła żądanie nowego zasobu.</span><span class="sxs-lookup"><span data-stu-id="854d2-125">This requires a round-trip to the server, and the redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> <span data-ttu-id="854d2-126">Jeśli `/resource` jest *przekierowanie* do `/different-resource`, żądań klientów `/resource`, a serwer odpowiada, że klient Zażądaj zasobu pod adresem `/different-resource` z stan kodu wskazujący, że przekierowanie tymczasowych lub trwałych.</span><span class="sxs-lookup"><span data-stu-id="854d2-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`, and the server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="854d2-127">Klient wykonuje żądanie nowego zasobu pod adresem URL przekierowania.</span><span class="sxs-lookup"><span data-stu-id="854d2-127">The client executes a new request for the resource at the redirect URL.</span></span>

![Punkt końcowy usługi WebAPI tymczasowo zmieniono z wersji 1 (wersja 1) w wersji 2 (v2) na serwerze.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="854d2-133">Przekierowywanie żądań do innego adresu URL, wskazuje, czy przekierowanie stałych lub tymczasowych.</span><span class="sxs-lookup"><span data-stu-id="854d2-133">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="854d2-134">301 (trwale przeniesiona) kod stanu jest używany gdzie zasób ma adres URL nowego, stałe i chcesz nakazać klienta, że wszystkie przyszłe żądania dla zasobu powinien używać nowego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="854d2-134">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="854d2-135">*Po odebraniu kod stanu 301, klient może buforować odpowiedzi.*</span><span class="sxs-lookup"><span data-stu-id="854d2-135">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="854d2-136">Kod stanu 302 (Found) jest używany gdzie przekierowania jest tymczasowe lub ogólnie podmiotu można zmienić w taki sposób, że klient nie należy przechowywać i ponowne użycie adres URL przekierowania w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="854d2-136">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="854d2-137">Aby uzyskać więcej informacji, zobacz [RFC 2616: definicje kod stanu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="854d2-137">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="854d2-138">A *ponowne zapisywanie adresów URL* jest operacją po stronie serwera w celu zapewnienia zasobów z adresu innego zasobu.</span><span class="sxs-lookup"><span data-stu-id="854d2-138">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="854d2-139">Ponowne zapisywanie adresów URL nie wymaga przesłania danych do serwera.</span><span class="sxs-lookup"><span data-stu-id="854d2-139">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="854d2-140">Ponownie zapisane adres URL nie jest zwracana do klienta i nie będą wyświetlane na pasku adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="854d2-140">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="854d2-141">Gdy `/resource` jest *ulegną* do `/different-resource`, żądań klientów `/resource`, a serwerem *wewnętrznie* pobiera zasobu pod adresem `/different-resource`.</span><span class="sxs-lookup"><span data-stu-id="854d2-141">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="854d2-142">Chociaż przez klienta może być w stanie pobrać zasobu pod adresem URL ponownie zapisane, klient nie będzie informacja, czy zasób istnieje pod adresem URL ponownie zapisane wysyła żądania i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="854d2-142">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Punkt końcowy usługi WebAPI zmianie z wersji 1 (wersja 1) w wersji 2 (v2) na serwerze.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="854d2-147">Adres URL przebudowywania przykładowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="854d2-147">URL rewriting sample app</span></span>
<span data-ttu-id="854d2-148">Można eksplorować funkcje pośredniczącym ponowne zapisywanie adresów URL z [adres URL przebudowywania Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span><span class="sxs-lookup"><span data-stu-id="854d2-148">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span></span> <span data-ttu-id="854d2-149">Aplikacja stosuje ponownego zapisywania i Przekierowanie zasady i przedstawia nowych lub przekierowany adres URL.</span><span class="sxs-lookup"><span data-stu-id="854d2-149">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="854d2-150">Kiedy należy używać pośredniczącym ponowne zapisywanie adresów URL</span><span class="sxs-lookup"><span data-stu-id="854d2-150">When to use URL Rewriting Middleware</span></span>
<span data-ttu-id="854d2-151">Użyj ponowne zapisywanie adresów URL w oprogramowaniu pośredniczącym, gdy nie można użyć [moduł ponowne zapisywanie adresów URL](https://www.iis.net/downloads/microsoft/url-rewrite) z usługami IIS w systemie Windows Server [modułu mod_rewrite Apache](https://httpd.apache.org/docs/2.4/rewrite/) na serwerze Apache [na NginxponownezapisywanieadresówURL](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), lub aplikacji znajduje się na [serwer HTTP.sys](xref:fundamentals/servers/httpsys) (wcześniej nazywanych [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="854d2-151">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="854d2-152">Główne powody do użycia na serwerze adresu URL przebudowywania technologii w usługach IIS, Apache lub Nginx są czy oprogramowanie pośredniczące nie obsługuje wszystkich funkcji tych modułów i wydajności oprogramowania pośredniczącego prawdopodobnie nie będzie zgodny z modułów.</span><span class="sxs-lookup"><span data-stu-id="854d2-152">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="854d2-153">Istnieją jednak niektóre funkcje modułów serwera, które nie działają z projektów platformy ASP.NET Core, takich jak `IsFile` i `IsDirectory` ograniczeń modułu ponownego zapisywania usług IIS.</span><span class="sxs-lookup"><span data-stu-id="854d2-153">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="854d2-154">W tych scenariuszach w zamian użyj oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="854d2-154">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="854d2-155">Package</span><span class="sxs-lookup"><span data-stu-id="854d2-155">Package</span></span>
<span data-ttu-id="854d2-156">Aby dołączyć oprogramowanie pośredniczące w projekcie, należy dodać odwołanie do [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="854d2-156">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="854d2-157">Ta funkcja jest dostępna dla aplikacji przeznaczonych dla platformy ASP.NET Core 1.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="854d2-157">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="854d2-158">Opcje i rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="854d2-158">Extension and options</span></span>
<span data-ttu-id="854d2-159">Ustanowić Twojej ponowne zapisywanie adresów URL i Przekierowanie reguł przez utworzenie wystąpienia `RewriteOptions` klasy z metody rozszerzenia dla poszczególnych reguł.</span><span class="sxs-lookup"><span data-stu-id="854d2-159">Establish your URL rewrite and redirect rules by creating an instance of the `RewriteOptions` class with extension methods for each of your rules.</span></span> <span data-ttu-id="854d2-160">Łańcucha wielu reguł w kolejności, że chcesz je przetworzyć.</span><span class="sxs-lookup"><span data-stu-id="854d2-160">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="854d2-161">`RewriteOptions` Są przekazywane do oprogramowania pośredniczącego ponowne zapisywanie adresów URL, który jest dodawany do potoku żądania z `app.UseRewriter(options);`.</span><span class="sxs-lookup"><span data-stu-id="854d2-161">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="854d2-162">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="854d2-162">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="854d2-163">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="854d2-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]

---

### <a name="url-redirect"></a><span data-ttu-id="854d2-164">Adres URL przekierowania</span><span class="sxs-lookup"><span data-stu-id="854d2-164">URL redirect</span></span>
<span data-ttu-id="854d2-165">Użyj `AddRedirect` Przekierowywanie żądań.</span><span class="sxs-lookup"><span data-stu-id="854d2-165">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="854d2-166">Pierwszy parametr zawiera z wyrażenia regularnego do dopasowania w ścieżce przychodzącego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="854d2-166">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="854d2-167">Drugi parametr jest ciąg zastępczy.</span><span class="sxs-lookup"><span data-stu-id="854d2-167">The second parameter is the replacement string.</span></span> <span data-ttu-id="854d2-168">Trzeci parametr, jeśli jest obecny, określa kod stanu.</span><span class="sxs-lookup"><span data-stu-id="854d2-168">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="854d2-169">Jeśli kod stanu nie jest określony, domyślnie 302 (Found), który wskazuje, że zasób jest tymczasowo przeniesiony lub zastąpione.</span><span class="sxs-lookup"><span data-stu-id="854d2-169">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="854d2-170">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="854d2-170">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="854d2-171">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="854d2-171">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]

---

<span data-ttu-id="854d2-172">W przeglądarce za pomocą narzędzia dla deweloperów włączone, Wyślij żądanie do przykładowej aplikacji ze ścieżką `/redirect-rule/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="854d2-172">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="854d2-173">Wyrażenia regularnego zgodna ze ścieżką żądania na `redirect-rule/(.*)`, a ścieżka została zastąpiona `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="854d2-173">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="854d2-174">Przekieruj adres URL jest wysyłany do klienta z kodem stanu 302 (Found).</span><span class="sxs-lookup"><span data-stu-id="854d2-174">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="854d2-175">Przeglądarka sprawia, że nowe żądanie pod adresem URL przekierowania, który jest wyświetlany na pasku adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="854d2-175">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="854d2-176">Ponieważ brak reguł w przykładowej aplikacji odpowiada na adres URL przekierowania, drugie żądanie odbiera odpowiedź 200 (OK) z aplikacji i treść odpowiedzi zawiera adres URL przekierowania.</span><span class="sxs-lookup"><span data-stu-id="854d2-176">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="854d2-177">Obie strony jest kierowane do serwera, gdy adres URL jest *przekierowanie*.</span><span class="sxs-lookup"><span data-stu-id="854d2-177">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="854d2-178">Należy zachować ostrożność podczas ustanawiania reguł przekierowania.</span><span class="sxs-lookup"><span data-stu-id="854d2-178">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="854d2-179">Reguły przekierowania są oceniane na każde żądanie do aplikacji, w tym nastąpiło przekierowanie.</span><span class="sxs-lookup"><span data-stu-id="854d2-179">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="854d2-180">Łatwo jest przypadkowo pętlę nieskończoną przekierowania.</span><span class="sxs-lookup"><span data-stu-id="854d2-180">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="854d2-181">Oryginalne żądanie:`/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="854d2-181">Original Request: `/redirect-rule/1234/5678`</span></span>

![Okno przeglądarki z narzędzi deweloperskich śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="854d2-183">Część wyrażenia umieszczone w nawiasach jest nazywana *grupy przechwytywania*.</span><span class="sxs-lookup"><span data-stu-id="854d2-183">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="854d2-184">Kropka (`.`) wyrażenia oznacza *dopasowuje dowolny znak*.</span><span class="sxs-lookup"><span data-stu-id="854d2-184">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="854d2-185">Gwiazdka (`*`) wskazuje *Dopasowuje poprzedni znak zero lub więcej razy*.</span><span class="sxs-lookup"><span data-stu-id="854d2-185">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="854d2-186">W związku z tym segmenty dwa ostatnie ścieżki adresu URL, `1234/5678`, są przechwytywane przez grupę przechwytywania `(.*)`.</span><span class="sxs-lookup"><span data-stu-id="854d2-186">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="854d2-187">Wartości podane w adresie URL żądania po `redirect-rule/` są przechwytywane przez tę grupę pojedynczego przechwytywania.</span><span class="sxs-lookup"><span data-stu-id="854d2-187">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="854d2-188">W ciągu zastępowania przechwyconej grupy są wstrzykiwane do ciągu z znak dolara (`$`) wraz z numerem sekwencji przechwytywania.</span><span class="sxs-lookup"><span data-stu-id="854d2-188">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="854d2-189">Pierwsza wartość grupy przechwytywania są uzyskiwane z `$1`, druga z `$2`, i kontynuują w sekwencji dla grup przechwytywania w Twojej wyrażenia regularnego.</span><span class="sxs-lookup"><span data-stu-id="854d2-189">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="854d2-190">Tylko jedna grupa przechwyconych regex reguły przekierowania w jest przykładowej aplikacji, tak aby tylko jedna grupa wprowadzony w ciągu zastępowania, która jest `$1`.</span><span class="sxs-lookup"><span data-stu-id="854d2-190">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="854d2-191">Reguła jest stosowana, adres URL staje się `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="854d2-191">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

<a name="url-redirect-to-secure-endpoint"></a>
### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="854d2-192">Adres URL przekierowania do bezpiecznego punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="854d2-192">URL redirect to a secure endpoint</span></span>
<span data-ttu-id="854d2-193">Użyj `AddRedirectToHttps` przekierowywania żądań HTTP do tego samego hosta i ścieżkę przy użyciu protokołu HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="854d2-193">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="854d2-194">Jeśli kod stanu nie jest podany, oprogramowanie pośredniczące domyślnie 302 (Found).</span><span class="sxs-lookup"><span data-stu-id="854d2-194">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="854d2-195">Jeśli port nie jest podany, oprogramowanie pośredniczące domyślnie `null`, co oznacza, że protokół zmienia się na `https://` i klient uzyskuje dostęp do zasobów na porcie 443.</span><span class="sxs-lookup"><span data-stu-id="854d2-195">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="854d2-196">W przykładzie przedstawiono sposób ustawić kod stanu 301 (trwale przeniesiona) i zmień numer portu na 5001.</span><span class="sxs-lookup"><span data-stu-id="854d2-196">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>
```csharp
var options = new RewriteOptions()
    .AddRedirectToHttps(301, 5001);

app.UseRewriter(options);
```
<span data-ttu-id="854d2-197">Użyj `AddRedirectToHttpsPermanent` przekierowywania żądań niezabezpieczonych do tego samego hosta i ścieżki z bezpiecznego protokołu HTTPS (`https://` na porcie 443).</span><span class="sxs-lookup"><span data-stu-id="854d2-197">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="854d2-198">Oprogramowanie pośredniczące ustawia kod stanu 301 (trwale przeniesiona).</span><span class="sxs-lookup"><span data-stu-id="854d2-198">The middleware sets the status code to 301 (Moved Permanently).</span></span>

<span data-ttu-id="854d2-199">Przykładowa aplikacja jest w stanie pokazuje sposób użycia `AddRedirectToHttps` lub `AddRedirectToHttpsPermanent`.</span><span class="sxs-lookup"><span data-stu-id="854d2-199">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="854d2-200">Dodaj metodę rozszerzenie do `RewriteOptions`.</span><span class="sxs-lookup"><span data-stu-id="854d2-200">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="854d2-201">Wprowadź niezabezpieczonego żądania do aplikacji na dowolny adres URL.</span><span class="sxs-lookup"><span data-stu-id="854d2-201">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="854d2-202">Zignoruj ostrzeżenie niezaufany certyfikat z podpisem własnym zabezpieczeń przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="854d2-202">Dismiss the browser security warning that the self-signed certificate is untrusted.</span></span>

<span data-ttu-id="854d2-203">Oryginalnego żądania przy użyciu `AddRedirectToHttps(301, 5001)`:`/secure`</span><span class="sxs-lookup"><span data-stu-id="854d2-203">Original Request using `AddRedirectToHttps(301, 5001)`: `/secure`</span></span>

![Okno przeglądarki z narzędzi deweloperskich śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="854d2-205">Oryginalnego żądania przy użyciu `AddRedirectToHttpsPermanent`:`/secure`</span><span class="sxs-lookup"><span data-stu-id="854d2-205">Original Request using `AddRedirectToHttpsPermanent`: `/secure`</span></span>

![Okno przeglądarki z narzędzi deweloperskich śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="854d2-207">Ponowne zapisywanie adresów URL</span><span class="sxs-lookup"><span data-stu-id="854d2-207">URL rewrite</span></span>
<span data-ttu-id="854d2-208">Użyj `AddRewrite` Aby utworzyć regułę dla ponowne zapisywanie adresów URL.</span><span class="sxs-lookup"><span data-stu-id="854d2-208">Use `AddRewrite` to create a rule for rewriting URLs.</span></span> <span data-ttu-id="854d2-209">Pierwszy parametr zawiera z wyrażenia regularnego do dopasowania na ścieżkę przychodzącego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="854d2-209">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="854d2-210">Drugi parametr jest ciąg zastępczy.</span><span class="sxs-lookup"><span data-stu-id="854d2-210">The second parameter is the replacement string.</span></span> <span data-ttu-id="854d2-211">Trzeci parametr `skipRemainingRules: {true|false}`, wskazuje, aby oprogramowanie pośredniczące umożliwia określenie, czy pominąć reguły ponownego zapisywania dodatkowe, jeśli zastosowano bieżącej regule.</span><span class="sxs-lookup"><span data-stu-id="854d2-211">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="854d2-212">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="854d2-212">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="854d2-213">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="854d2-213">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

<span data-ttu-id="854d2-214">Oryginalne żądanie:`/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="854d2-214">Original Request: `/rewrite-rule/1234/5678`</span></span>

![Okno przeglądarki z narzędzi deweloperskich śledzenia żądań i odpowiedzi](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="854d2-216">Pierwszą rzeczą, którą można zauważyć, że w wyrażenia regularnego jest karatach (`^`) na początku wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="854d2-216">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="854d2-217">Oznacza to, że pasujące zaczyna się na początku ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="854d2-217">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="854d2-218">W przypadku poprzedniego przykładu z regułą przekierowania `redirect-rule/(.*)`, na początku wyrażenia regularnego nie istnieją żadne karatach — w związku z tym może poprzedzać wszystkie znaki `redirect-rule/` w ścieżce dla pomyślnego dopasowania.</span><span class="sxs-lookup"><span data-stu-id="854d2-218">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="854d2-219">Ścieżka</span><span class="sxs-lookup"><span data-stu-id="854d2-219">Path</span></span>                               | <span data-ttu-id="854d2-220">Dopasowanie</span><span class="sxs-lookup"><span data-stu-id="854d2-220">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="854d2-221">Tak</span><span class="sxs-lookup"><span data-stu-id="854d2-221">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="854d2-222">Tak</span><span class="sxs-lookup"><span data-stu-id="854d2-222">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="854d2-223">Tak</span><span class="sxs-lookup"><span data-stu-id="854d2-223">Yes</span></span>   |

<span data-ttu-id="854d2-224">Reguła ponownego zapisywania `^rewrite-rule/(\d+)/(\d+)`, ścieżki zgodny tylko, jeśli zaczynają `rewrite-rule/`.</span><span class="sxs-lookup"><span data-stu-id="854d2-224">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="854d2-225">Zwróć uwagę, różnica dopasowywania między poniżej reguły ponownego zapisywania i zasadę przekierowania powyżej.</span><span class="sxs-lookup"><span data-stu-id="854d2-225">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="854d2-226">Ścieżka</span><span class="sxs-lookup"><span data-stu-id="854d2-226">Path</span></span>                              | <span data-ttu-id="854d2-227">Dopasowanie</span><span class="sxs-lookup"><span data-stu-id="854d2-227">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="854d2-228">Tak</span><span class="sxs-lookup"><span data-stu-id="854d2-228">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="854d2-229">Nie</span><span class="sxs-lookup"><span data-stu-id="854d2-229">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="854d2-230">Nie</span><span class="sxs-lookup"><span data-stu-id="854d2-230">No</span></span>    |

<span data-ttu-id="854d2-231">Po `^rewrite-rule/` część wyrażenia, istnieją dwie grupy przechwytywania, `(\d+)/(\d+)`.</span><span class="sxs-lookup"><span data-stu-id="854d2-231">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="854d2-232">`\d` Oznacza *odpowiada cyfrę (numer)*.</span><span class="sxs-lookup"><span data-stu-id="854d2-232">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="854d2-233">Znak plus (`+`) oznacza *zgodne z co najmniej jednego znaku poprzedzającego*.</span><span class="sxs-lookup"><span data-stu-id="854d2-233">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="854d2-234">W związku z tym adres URL musi zawierać liczbę następuje następuje inny numer kreskami ukośnymi.</span><span class="sxs-lookup"><span data-stu-id="854d2-234">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="854d2-235">Te przechwytywania grup są wstrzykiwane do nowych adresu URL jako `$1` i `$2`.</span><span class="sxs-lookup"><span data-stu-id="854d2-235">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="854d2-236">Ciąg zastępczy reguły ponownego zapisywania umieszczenie grup przechwyconych w ciąg zapytania.</span><span class="sxs-lookup"><span data-stu-id="854d2-236">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="854d2-237">Żądana ścieżka `/rewrite-rule/1234/5678` jest napisany od nowa można uzyskać zasobu pod adresem `/rewritten?var1=1234&var2=5678`.</span><span class="sxs-lookup"><span data-stu-id="854d2-237">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="854d2-238">Jeśli ciąg zapytania jest obecna na oryginalne żądanie, jest to zachowane, gdy adres URL jest napisany od nowa.</span><span class="sxs-lookup"><span data-stu-id="854d2-238">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="854d2-239">Nie ma żadnych obie strony do serwera w celu uzyskania zasobu.</span><span class="sxs-lookup"><span data-stu-id="854d2-239">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="854d2-240">Jeśli zasób istnieje, ma pobrane i zwracany do klienta z kodem 200 stanu (OK).</span><span class="sxs-lookup"><span data-stu-id="854d2-240">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="854d2-241">Ponieważ klient nie jest przekierowany, nie powoduje zmiany adresu URL na pasku adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="854d2-241">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="854d2-242">Ile dotyczy to klient, wystąpił nigdy nie operacji ponowne zapisywanie adresów URL.</span><span class="sxs-lookup"><span data-stu-id="854d2-242">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="854d2-243">Użyj `skipRemainingRules: true` zawsze, gdy jest to możliwe, ponieważ reguł dopasowywania jest procesem kosztowne i skraca czas odpowiedzi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="854d2-243">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="854d2-244">Najszybszym odpowiedzi aplikacji:</span><span class="sxs-lookup"><span data-stu-id="854d2-244">For the fastest app response:</span></span>
> * <span data-ttu-id="854d2-245">Kolejność reguły ponownego zapisywania z reguły najczęściej dopasowane do najmniej często dopasowane reguły.</span><span class="sxs-lookup"><span data-stu-id="854d2-245">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="854d2-246">Pomiń przetwarzanie pozostałych reguł, gdy pasują do i nie dodatkowe reguły przetwarzania jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="854d2-246">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="854d2-247">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="854d2-247">Apache mod_rewrite</span></span>
<span data-ttu-id="854d2-248">Zastosuj reguły mod_rewrite Apache z `AddApacheModRewrite`.</span><span class="sxs-lookup"><span data-stu-id="854d2-248">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="854d2-249">Upewnij się, że plik reguł jest wdrażany z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="854d2-249">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="854d2-250">Aby uzyskać więcej informacji i przykłady reguł mod_rewrite, zobacz [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span><span class="sxs-lookup"><span data-stu-id="854d2-250">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="854d2-251">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="854d2-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="854d2-252">A `StreamReader` jest używany do odczytu reguły z *ApacheModRewrite.txt* pliku reguł.</span><span class="sxs-lookup"><span data-stu-id="854d2-252">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="854d2-253">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="854d2-253">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="854d2-254">Pierwszy parametr przyjmuje `IFileProvider`, które zostaną przekazane za pośrednictwem [iniekcji zależności](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="854d2-254">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="854d2-255">`IHostingEnvironment` Jest wprowadzonym w celu zapewnienia `ContentRootFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="854d2-255">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="854d2-256">Drugi parametr jest ścieżka do pliku reguł, który jest *ApacheModRewrite.txt* w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="854d2-256">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]

---

<span data-ttu-id="854d2-257">Przykładowa aplikacja przekierowuje żądania z `/apache-mod-rules-redirect/(.\*)` do `/redirected?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="854d2-257">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="854d2-258">Kod stanu odpowiedzi jest 302 (Found).</span><span class="sxs-lookup"><span data-stu-id="854d2-258">The response status code is 302 (Found).</span></span>

[!code[Main](url-rewriting/samples/2.x/ApacheModRewrite.txt)]

<span data-ttu-id="854d2-259">Oryginalne żądanie:`/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="854d2-259">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![Okno przeglądarki z narzędzi deweloperskich śledzenia żądań i odpowiedzi](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a><span data-ttu-id="854d2-261">Zmienne serwera obsługiwanych</span><span class="sxs-lookup"><span data-stu-id="854d2-261">Supported server variables</span></span>
<span data-ttu-id="854d2-262">Oprogramowanie pośredniczące obsługuje następujące zmienne serwera Apache mod_rewrite:</span><span class="sxs-lookup"><span data-stu-id="854d2-262">The middleware supports the following Apache mod_rewrite server variables:</span></span>
* <span data-ttu-id="854d2-263">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="854d2-263">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="854d2-264">SPECJALNY RODZAJ</span><span class="sxs-lookup"><span data-stu-id="854d2-264">HTTP_ACCEPT</span></span>
* <span data-ttu-id="854d2-265">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="854d2-265">HTTP_CONNECTION</span></span>
* <span data-ttu-id="854d2-266">MOŻNA TAKŻE WYLICZYĆ WSZYSTKIE</span><span class="sxs-lookup"><span data-stu-id="854d2-266">HTTP_COOKIE</span></span>
* <span data-ttu-id="854d2-267">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="854d2-267">HTTP_FORWARDED</span></span>
* <span data-ttu-id="854d2-268">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="854d2-268">HTTP_HOST</span></span>
* <span data-ttu-id="854d2-269">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="854d2-269">HTTP_REFERER</span></span>
* <span data-ttu-id="854d2-270">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="854d2-270">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="854d2-271">HTTPS</span><span class="sxs-lookup"><span data-stu-id="854d2-271">HTTPS</span></span>
* <span data-ttu-id="854d2-272">PROTOKÓŁ IPV6</span><span class="sxs-lookup"><span data-stu-id="854d2-272">IPV6</span></span>
* <span data-ttu-id="854d2-273">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="854d2-273">QUERY_STRING</span></span>
* <span data-ttu-id="854d2-274">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="854d2-274">REMOTE_ADDR</span></span>
* <span data-ttu-id="854d2-275">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="854d2-275">REMOTE_PORT</span></span>
* <span data-ttu-id="854d2-276">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="854d2-276">REQUEST_FILENAME</span></span>
* <span data-ttu-id="854d2-277">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="854d2-277">REQUEST_METHOD</span></span>
* <span data-ttu-id="854d2-278">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="854d2-278">REQUEST_SCHEME</span></span>
* <span data-ttu-id="854d2-279">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="854d2-279">REQUEST_URI</span></span>
* <span data-ttu-id="854d2-280">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="854d2-280">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="854d2-281">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="854d2-281">SERVER_ADDR</span></span>
* <span data-ttu-id="854d2-282">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="854d2-282">SERVER_PORT</span></span>
* <span data-ttu-id="854d2-283">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="854d2-283">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="854d2-284">CZAS</span><span class="sxs-lookup"><span data-stu-id="854d2-284">TIME</span></span>
* <span data-ttu-id="854d2-285">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="854d2-285">TIME_DAY</span></span>
* <span data-ttu-id="854d2-286">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="854d2-286">TIME_HOUR</span></span>
* <span data-ttu-id="854d2-287">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="854d2-287">TIME_MIN</span></span>
* <span data-ttu-id="854d2-288">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="854d2-288">TIME_MON</span></span>
* <span data-ttu-id="854d2-289">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="854d2-289">TIME_SEC</span></span>
* <span data-ttu-id="854d2-290">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="854d2-290">TIME_WDAY</span></span>
* <span data-ttu-id="854d2-291">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="854d2-291">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="854d2-292">Reguły moduł ponowne zapisywanie adresów URL usług IIS</span><span class="sxs-lookup"><span data-stu-id="854d2-292">IIS URL Rewrite Module rules</span></span>
<span data-ttu-id="854d2-293">Aby użyć reguł, które dotyczą moduł ponowne zapisywanie adresów URL usług IIS, użyj `AddIISUrlRewrite`.</span><span class="sxs-lookup"><span data-stu-id="854d2-293">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="854d2-294">Upewnij się, że plik reguł jest wdrażany z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="854d2-294">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="854d2-295">Nie bezpośrednie oprogramowaniu pośredniczącym, aby korzystać z *web.config* pliku podczas uruchamiania w systemie Windows Server IIS.</span><span class="sxs-lookup"><span data-stu-id="854d2-295">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="854d2-296">Z programem IIS, zasady te powinny być przechowywane poza Twojej *web.config* w celu uniknięcia konfliktów z modułem ponownego zapisywania usług IIS.</span><span class="sxs-lookup"><span data-stu-id="854d2-296">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="854d2-297">Aby uzyskać więcej informacji i przykłady reguł moduł ponowne zapisywanie adresów URL usług IIS, zobacz [przy użyciu adresu Url przepisywania moduł 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) i [odwołania konfiguracji modułu ponowne zapisywanie adresów URL](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span><span class="sxs-lookup"><span data-stu-id="854d2-297">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="854d2-298">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="854d2-298">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="854d2-299">A `StreamReader` jest używany do odczytu reguły z *IISUrlRewrite.xml* pliku reguł.</span><span class="sxs-lookup"><span data-stu-id="854d2-299">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="854d2-300">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="854d2-300">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="854d2-301">Pierwszy parametr przyjmuje `IFileProvider`, a drugi parametr jest ścieżka do pliku reguł XML, który jest *IISUrlRewrite.xml* w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="854d2-301">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]

---

<span data-ttu-id="854d2-302">Przykładowa aplikacja ponownie zapisuje żądań z `/iis-rules-rewrite/(.*)` do `/rewritten?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="854d2-302">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="854d2-303">Odpowiedź jest wysyłana do klienta z kodem stanu 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="854d2-303">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]

<span data-ttu-id="854d2-304">Oryginalne żądanie:`/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="854d2-304">Original Request: `/iis-rules-rewrite/1234`</span></span>

![Okno przeglądarki z narzędzi deweloperskich śledzenia żądań i odpowiedzi](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="854d2-306">Jeśli masz aktywnego modułu ponownego zapisywania usług IIS skonfigurowanych reguł poziomu serwera, które będzie mieć wpływ aplikacji w sposób niepożądanych można wyłączyć moduł ponowne zapisywanie adresów usług IIS dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="854d2-306">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="854d2-307">Aby uzyskać więcej informacji, zobacz [moduły IIS wyłączenie](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="854d2-307">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="854d2-308">Nieobsługiwane funkcje</span><span class="sxs-lookup"><span data-stu-id="854d2-308">Unsupported features</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="854d2-309">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="854d2-309">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="854d2-310">Oprogramowanie pośredniczące zwolnione z platformy ASP.NET Core 2.x nie obsługuje następujące funkcje moduł ponowne zapisywanie adresów URL usług IIS:</span><span class="sxs-lookup"><span data-stu-id="854d2-310">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="854d2-311">Reguły ruchu wychodzącego</span><span class="sxs-lookup"><span data-stu-id="854d2-311">Outbound Rules</span></span>
* <span data-ttu-id="854d2-312">Zmienne serwera niestandardowego</span><span class="sxs-lookup"><span data-stu-id="854d2-312">Custom Server Variables</span></span>
* <span data-ttu-id="854d2-313">Symbole wieloznaczne</span><span class="sxs-lookup"><span data-stu-id="854d2-313">Wildcards</span></span>
* <span data-ttu-id="854d2-314">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="854d2-314">LogRewrittenUrl</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="854d2-315">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="854d2-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="854d2-316">Oprogramowanie pośredniczące zwolnione z platformy ASP.NET Core 1.x nie obsługuje następujące funkcje moduł ponowne zapisywanie adresów URL usług IIS:</span><span class="sxs-lookup"><span data-stu-id="854d2-316">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="854d2-317">Globalne zasady</span><span class="sxs-lookup"><span data-stu-id="854d2-317">Global Rules</span></span>
* <span data-ttu-id="854d2-318">Reguły ruchu wychodzącego</span><span class="sxs-lookup"><span data-stu-id="854d2-318">Outbound Rules</span></span>
* <span data-ttu-id="854d2-319">Ponownego zapisywania map</span><span class="sxs-lookup"><span data-stu-id="854d2-319">Rewrite Maps</span></span>
* <span data-ttu-id="854d2-320">Akcja CustomResponse</span><span class="sxs-lookup"><span data-stu-id="854d2-320">CustomResponse Action</span></span>
* <span data-ttu-id="854d2-321">Zmienne serwera niestandardowego</span><span class="sxs-lookup"><span data-stu-id="854d2-321">Custom Server Variables</span></span>
* <span data-ttu-id="854d2-322">Symbole wieloznaczne</span><span class="sxs-lookup"><span data-stu-id="854d2-322">Wildcards</span></span>
* <span data-ttu-id="854d2-323">Akcja: CustomResponse</span><span class="sxs-lookup"><span data-stu-id="854d2-323">Action:CustomResponse</span></span>
* <span data-ttu-id="854d2-324">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="854d2-324">LogRewrittenUrl</span></span>

---

#### <a name="supported-server-variables"></a><span data-ttu-id="854d2-325">Zmienne serwera obsługiwanych</span><span class="sxs-lookup"><span data-stu-id="854d2-325">Supported server variables</span></span>
<span data-ttu-id="854d2-326">Oprogramowanie pośredniczące obsługuje następujące zmienne serwera moduł ponowne zapisywanie adresów URL usług IIS:</span><span class="sxs-lookup"><span data-stu-id="854d2-326">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>
* <span data-ttu-id="854d2-327">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="854d2-327">CONTENT_LENGTH</span></span>
* <span data-ttu-id="854d2-328">TYP_ZAWARTOŚCI</span><span class="sxs-lookup"><span data-stu-id="854d2-328">CONTENT_TYPE</span></span>
* <span data-ttu-id="854d2-329">SPECJALNY RODZAJ</span><span class="sxs-lookup"><span data-stu-id="854d2-329">HTTP_ACCEPT</span></span>
* <span data-ttu-id="854d2-330">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="854d2-330">HTTP_CONNECTION</span></span>
* <span data-ttu-id="854d2-331">MOŻNA TAKŻE WYLICZYĆ WSZYSTKIE</span><span class="sxs-lookup"><span data-stu-id="854d2-331">HTTP_COOKIE</span></span>
* <span data-ttu-id="854d2-332">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="854d2-332">HTTP_HOST</span></span>
* <span data-ttu-id="854d2-333">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="854d2-333">HTTP_REFERER</span></span>
* <span data-ttu-id="854d2-334">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="854d2-334">HTTP_URL</span></span>
* <span data-ttu-id="854d2-335">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="854d2-335">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="854d2-336">HTTPS</span><span class="sxs-lookup"><span data-stu-id="854d2-336">HTTPS</span></span>
* <span data-ttu-id="854d2-337">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="854d2-337">LOCAL_ADDR</span></span>
* <span data-ttu-id="854d2-338">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="854d2-338">QUERY_STRING</span></span>
* <span data-ttu-id="854d2-339">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="854d2-339">REMOTE_ADDR</span></span>
* <span data-ttu-id="854d2-340">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="854d2-340">REMOTE_PORT</span></span>
* <span data-ttu-id="854d2-341">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="854d2-341">REQUEST_FILENAME</span></span>
* <span data-ttu-id="854d2-342">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="854d2-342">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="854d2-343">Możesz również uzyskać `IFileProvider` za pośrednictwem `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="854d2-343">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="854d2-344">Takie podejście może udostępnić większą elastyczność lokalizacji Twojej ponownego zapisywania plików reguł.</span><span class="sxs-lookup"><span data-stu-id="854d2-344">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="854d2-345">Upewnij się, że reguły ponownego zapisywania plików są wdrażane do serwera w ścieżce podane przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="854d2-345">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="854d2-346">Reguły na podstawie — metoda</span><span class="sxs-lookup"><span data-stu-id="854d2-346">Method-based rule</span></span>
<span data-ttu-id="854d2-347">Użyj `Add(Action<RewriteContext> applyRule)` implementacji logiki reguły w metodzie.</span><span class="sxs-lookup"><span data-stu-id="854d2-347">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="854d2-348">`RewriteContext` Przedstawia `HttpContext` do użycia w metodę.</span><span class="sxs-lookup"><span data-stu-id="854d2-348">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="854d2-349">`context.Result` Określa, jak dodatkowe potoku przetwarzania jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="854d2-349">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="854d2-350">kontekst. Wynik</span><span class="sxs-lookup"><span data-stu-id="854d2-350">context.Result</span></span>                       | <span data-ttu-id="854d2-351">Akcja</span><span class="sxs-lookup"><span data-stu-id="854d2-351">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="854d2-352">`RuleResult.ContinueRules`(ustawienie domyślne)</span><span class="sxs-lookup"><span data-stu-id="854d2-352">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="854d2-353">Kontynuować stosowanie reguł</span><span class="sxs-lookup"><span data-stu-id="854d2-353">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="854d2-354">Zaprzestanie stosowania reguły i wysyłania odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="854d2-354">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="854d2-355">Zaprzestanie stosowania reguły i wysyłać kontekstu do następnego oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="854d2-355">Stop applying rules and send the context to the next middleware</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="854d2-356">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="854d2-356">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="854d2-357">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="854d2-357">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]

---

<span data-ttu-id="854d2-358">Przykładowa aplikacja przedstawiono metodę, która przekierowuje żądania dla ścieżek czy kończą się *.xml*.</span><span class="sxs-lookup"><span data-stu-id="854d2-358">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="854d2-359">Jeśli żądanie `/file.xml`, nastąpi przekierowanie do `/xmlfiles/file.xml`.</span><span class="sxs-lookup"><span data-stu-id="854d2-359">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="854d2-360">Kod stanu jest ustawiona na 301 (trwale przeniesiona).</span><span class="sxs-lookup"><span data-stu-id="854d2-360">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="854d2-361">Dla przekierowania Musisz jawnie ustawić kod stanu odpowiedzi; w przeciwnym razie zwracany jest kod stanu 200 (OK) i Przekierowanie nie występują na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="854d2-361">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="854d2-362">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="854d2-362">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="854d2-363">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="854d2-363">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]

---

<span data-ttu-id="854d2-364">Oryginalne żądanie:`/file.xml`</span><span class="sxs-lookup"><span data-stu-id="854d2-364">Original Request: `/file.xml`</span></span>

![Okno przeglądarki z śledzenia żądań i odpowiedzi dla file.xml narzędzia dla deweloperów](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="854d2-366">Reguły na podstawie IRule</span><span class="sxs-lookup"><span data-stu-id="854d2-366">IRule-based rule</span></span>
<span data-ttu-id="854d2-367">Użyj `Add(IRule)` implementacji logiki reguły w klasie, która jest pochodną `IRule`.</span><span class="sxs-lookup"><span data-stu-id="854d2-367">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="854d2-368">Przy użyciu `IRule` zapewnia większą elastyczność w przypadku metody oparte na metodzie reguły.</span><span class="sxs-lookup"><span data-stu-id="854d2-368">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="854d2-369">Klasy pochodne mogą obejmować konstruktora, w którym można przekazać w parametrach `ApplyRule` metody.</span><span class="sxs-lookup"><span data-stu-id="854d2-369">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="854d2-370">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="854d2-370">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="854d2-371">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="854d2-371">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]

---

<span data-ttu-id="854d2-372">Wartości parametrów w Przykładowa aplikacja dla `extension` i `newPath` są sprawdzane w celu spełnienia kilku warunków.</span><span class="sxs-lookup"><span data-stu-id="854d2-372">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="854d2-373">`extension` Musi zawierać wartość, a wartość musi być *.png*, *.jpg*, lub *.gif*.</span><span class="sxs-lookup"><span data-stu-id="854d2-373">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="854d2-374">Jeśli `newPath` nie jest prawidłowy, `ArgumentException` jest generowany.</span><span class="sxs-lookup"><span data-stu-id="854d2-374">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="854d2-375">Jeśli żądanie *image.png*, nastąpi przekierowanie do `/png-images/image.png`.</span><span class="sxs-lookup"><span data-stu-id="854d2-375">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="854d2-376">Jeśli żądanie *image.jpg*, nastąpi przekierowanie do `/jpg-images/image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="854d2-376">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="854d2-377">Kod stanu ma ustawioną wartość 301 (przenieść trwale) i `context.Result` ustawiono Zatrzymaj przetwarzanie reguł i wysyłania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="854d2-377">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="854d2-378">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="854d2-378">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="854d2-379">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="854d2-379">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]

---

<span data-ttu-id="854d2-380">Oryginalne żądanie:`/image.png`</span><span class="sxs-lookup"><span data-stu-id="854d2-380">Original Request: `/image.png`</span></span>

![Okno przeglądarki z śledzenia żądań i odpowiedzi dla image.png narzędzia dla deweloperów](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="854d2-382">Oryginalne żądanie:`/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="854d2-382">Original Request: `/image.jpg`</span></span>

![Okno przeglądarki z śledzenia żądań i odpowiedzi dla image.jpg narzędzia dla deweloperów](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="854d2-384">Przykłady wyrażeń regularnych</span><span class="sxs-lookup"><span data-stu-id="854d2-384">Regex examples</span></span>

| <span data-ttu-id="854d2-385">Cel</span><span class="sxs-lookup"><span data-stu-id="854d2-385">Goal</span></span> | <span data-ttu-id="854d2-386">Ciąg wyrażenia regularnego &</span><span class="sxs-lookup"><span data-stu-id="854d2-386">Regex String &</span></span><br><span data-ttu-id="854d2-387">Przykład dopasowania</span><span class="sxs-lookup"><span data-stu-id="854d2-387">Match Example</span></span> | <span data-ttu-id="854d2-388">Ciąg zastępczy &</span><span class="sxs-lookup"><span data-stu-id="854d2-388">Replacement String &</span></span><br><span data-ttu-id="854d2-389">Przykład danych wyjściowych</span><span class="sxs-lookup"><span data-stu-id="854d2-389">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="854d2-390">Ścieżka Napisz ponownie w ciągu kwerendy</span><span class="sxs-lookup"><span data-stu-id="854d2-390">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="854d2-391">Usuwanie ukośnika</span><span class="sxs-lookup"><span data-stu-id="854d2-391">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="854d2-392">Wymuszanie ukośnika</span><span class="sxs-lookup"><span data-stu-id="854d2-392">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="854d2-393">Unikaj ponowne zapisywanie określone żądania</span><span class="sxs-lookup"><span data-stu-id="854d2-393">Avoid rewriting specific requests</span></span> | `(.*[^(\.axd)])$`<br><span data-ttu-id="854d2-394">Tak:`/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="854d2-394">Yes: `/resource.htm`</span></span><br><span data-ttu-id="854d2-395">Nie:`/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="854d2-395">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="854d2-396">Rozmieszczanie segmenty adresu URL</span><span class="sxs-lookup"><span data-stu-id="854d2-396">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="854d2-397">Zastąp segment adresu URL</span><span class="sxs-lookup"><span data-stu-id="854d2-397">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="854d2-398">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="854d2-398">Additional resources</span></span>
* [<span data-ttu-id="854d2-399">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="854d2-399">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="854d2-400">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="854d2-400">Middleware</span></span>](middleware.md)
* [<span data-ttu-id="854d2-401">Wyrażenia regularne w .NET</span><span class="sxs-lookup"><span data-stu-id="854d2-401">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="854d2-402">Język wyrażeń regularnych — podręczny wykaz</span><span class="sxs-lookup"><span data-stu-id="854d2-402">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="854d2-403">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="854d2-403">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="854d2-404">Za pomocą modułu ponowne zapisywanie adresów Url 2.0 (dla usług IIS)</span><span class="sxs-lookup"><span data-stu-id="854d2-404">Using Url Rewrite Module 2.0 (for IIS)</span></span>](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="854d2-405">Odwołania do konfiguracji modułu ponowne zapisywanie adresów URL</span><span class="sxs-lookup"><span data-stu-id="854d2-405">URL Rewrite Module Configuration Reference</span></span>](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="854d2-406">Forum moduł ponowne zapisywanie adresów URL usług IIS</span><span class="sxs-lookup"><span data-stu-id="854d2-406">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="854d2-407">Zachowaj prostą strukturę adresu URL</span><span class="sxs-lookup"><span data-stu-id="854d2-407">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="854d2-408">Ponowne zapisywanie adresów URL 10 porady i wskazówki</span><span class="sxs-lookup"><span data-stu-id="854d2-408">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="854d2-409">Ukośnika lub ukośnika</span><span class="sxs-lookup"><span data-stu-id="854d2-409">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
