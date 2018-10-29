---
title: Adres URL ponowne napisanie oprogramowania pośredniczącego w programie ASP.NET Core
author: guardrex
description: Zapoznaj się z adresem URL ponownego zapisywania adresów i przekierowywania z oprogramowanie pośredniczące ponownego zapisywania adresów URL w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 08/17/2017
uid: fundamentals/url-rewriting
ms.openlocfilehash: 5a1891c838436467fb49ff6288587fab08201179
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207189"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="e6ae8-103">Adres URL ponowne napisanie oprogramowania pośredniczącego w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6ae8-103">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="e6ae8-104">Przez [Luke Latham](https://github.com/guardrex) i [Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="e6ae8-104">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="e6ae8-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e6ae8-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e6ae8-106">Ponownego zapisywania adresów URL jest działaniem zmiany żądania, których adresy URL na podstawie co najmniej jeden wstępnie zdefiniowanych reguł.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-106">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="e6ae8-107">Ponownego zapisywania adresów URL tworzy abstrakcję między lokalizacje zasobów i ich adresów, tak, aby lokalizacje i adresy nie są ściśle powiązane.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-107">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="e6ae8-108">Istnieje kilka scenariuszy, w których jest przydatna ponownego zapisywania adresów URL:</span><span class="sxs-lookup"><span data-stu-id="e6ae8-108">There are several scenarios where URL rewriting is valuable:</span></span>

* <span data-ttu-id="e6ae8-109">Przenoszenie lub zastępując tymczasowo lub trwale zasoby serwera przy zachowaniu stabilne lokalizatorów dla tych zasobów.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-109">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources.</span></span>
* <span data-ttu-id="e6ae8-110">Podział żądań w różnych aplikacjach lub w wielu obszarach jednej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-110">Splitting request processing across different apps or across areas of one app.</span></span>
* <span data-ttu-id="e6ae8-111">Usuwanie, dodawanie lub reorganizacja segmenty adresu URL na przychodzące żądania.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-111">Removing, adding, or reorganizing URL segments on incoming requests.</span></span>
* <span data-ttu-id="e6ae8-112">Optymalizacja publiczne adresy URL do optymalizacji aparatu wyszukiwania (SEO).</span><span class="sxs-lookup"><span data-stu-id="e6ae8-112">Optimizing public URLs for Search Engine Optimization (SEO).</span></span>
* <span data-ttu-id="e6ae8-113">Umożliwiający użycie przyjaznych adresów URL publicznej, aby pomóc użytkownikom przewidzieć zawartość, którą znajdą, wykonując poniższe łącze.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-113">Permitting the use of friendly public URLs to help people predict the content they will find by following a link.</span></span>
* <span data-ttu-id="e6ae8-114">Przekierowywanie żądań niezabezpieczone bezpieczne punkty końcowe.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-114">Redirecting insecure requests to secure endpoints.</span></span>
* <span data-ttu-id="e6ae8-115">Zapobieganie hotlinking obrazu.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-115">Preventing image hotlinking.</span></span>

<span data-ttu-id="e6ae8-116">Można zdefiniować reguły zmiana adresu URL na kilka sposobów, łącznie z wyrażeniem regularnym Apache mod_rewrite modułu zasad, zasady moduł ponowne zapisywanie adresów usług IIS i przy użyciu reguły niestandardowej logiki.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-116">You can define rules for changing the URL in several ways, including Regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="e6ae8-117">Ten dokument wprowadza instrukcje dotyczące sposobu używania oprogramowanie pośredniczące ponownego zapisywania adresów URL w aplikacji platformy ASP.NET Core ponownego zapisywania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-117">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="e6ae8-118">Ponownego zapisywania adresów URL może zmniejszyć wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-118">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="e6ae8-119">Jeśli jest to możliwe, należy ograniczyć liczbę i złożoność reguł.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-119">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="e6ae8-120">Ponowne zapisywanie adresów URL przekierowania i adres URL</span><span class="sxs-lookup"><span data-stu-id="e6ae8-120">URL redirect and URL rewrite</span></span>

<span data-ttu-id="e6ae8-121">Różnica w treść między *adres URL przekierowania* i *ponowne zapisywanie adresów URL* może wydawać się subtelne na pierwszego, ale ma istotny wpływ na zapewniania zasobów dla klientów.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-121">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="e6ae8-122">Oprogramowanie pośredniczące ponownego zapisywania adresów URL platformy ASP.NET Core jest w stanie spełniające potrzeby dla obu.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-122">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="e6ae8-123">A *adres URL przekierowania* jest operacją po stronie klienta, w których klient jest zobowiązany do uzyskania dostępu do zasobu na inny adres.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-123">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="e6ae8-124">Ta migracja wymaga przesłania danych do serwera.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-124">This requires a round-trip to the server.</span></span> <span data-ttu-id="e6ae8-125">Adres URL przekierowania zwracana do klienta pojawia się w pasku adresu przeglądarki, gdy klient wysyła nowe żądanie dla zasobu.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-125">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> 

<span data-ttu-id="e6ae8-126">Jeśli `/resource` jest *przekierowanie* do `/different-resource`, żądań klientów `/resource`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`.</span></span> <span data-ttu-id="e6ae8-127">Następnie serwer odpowiada, że klient powinien uzyskać zasób w `/different-resource` z kodem stanu wskazującym, przekierowania, który jest tymczasowy lub stały.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-127">The server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="e6ae8-128">Klient wykonuje żądanie nowego zasobu pod adresem URL przekierowania.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-128">The client executes a new request for the resource at the redirect URL.</span></span>

![Punkt końcowy usługi WebAPI tymczasowo zmieniono z wersją 1 (v1) do wersji 2 (v2) na serwerze.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="e6ae8-134">Przekierowywanie żądań do innego adresu URL, wskazują, czy przekierowania, który jest stałych lub tymczasowych.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-134">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="e6ae8-135">301 (trwale przeniesiona) kod stanu służy gdzie zasób ma nowy, stały adres URL i chcesz w celu poinstruowania klienta o tym, że wszystkie przyszłe żądania dotyczące zasobów powinien używać nowego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-135">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="e6ae8-136">*Klient może buforować odpowiedzi, po odebraniu kodu 301 stanu.*</span><span class="sxs-lookup"><span data-stu-id="e6ae8-136">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="e6ae8-137">302 kod stanu (Found) jest używana, gdzie przekierowania jest tymczasowy lub ogólnie podmiotu można zmienić w taki sposób, że klient nie należy przechowywać i ponowne użycie adres URL przekierowania w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-137">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="e6ae8-138">Aby uzyskać więcej informacji, zobacz [dokumencie RFC 2616: definicje kodów stanu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="e6ae8-138">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="e6ae8-139">A *ponowne zapisywanie adresów URL* jest operacją po stronie serwera w celu zapewnienia zasobów z adresu innego zasobu.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-139">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="e6ae8-140">Ponownego zapisywania adresów URL nie wymaga przesłania danych do serwera.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-140">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="e6ae8-141">Nowych adres URL nie jest zwracana do klienta i nie będzie wyświetlane na pasku adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-141">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="e6ae8-142">Gdy `/resource` jest *przepisany* do `/different-resource`, żądań klientów `/resource`, a serwerem *wewnętrznie* pobiera zasób o `/different-resource`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-142">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="e6ae8-143">Mimo, że klient może być możliwe do pobrania zasobu pod adresem URL nowych, klient nie będzie informacja, że zasób istnieje pod adresem URL nowych sprawia, że jego żądanie i odbiera odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-143">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Punkt końcowy usługi WebAPI została zmieniona z wersją 1 (v1) do wersji 2 (v2) na serwerze.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="e6ae8-148">Przykładowa aplikacja ponownego zapisywania adresów URL</span><span class="sxs-lookup"><span data-stu-id="e6ae8-148">URL rewriting sample app</span></span>

<span data-ttu-id="e6ae8-149">Możesz eksplorować funkcje pośredniczącym ponownego zapisywania adresów URL przy użyciu [ponownego zapisywania adresów URL przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span><span class="sxs-lookup"><span data-stu-id="e6ae8-149">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span></span> <span data-ttu-id="e6ae8-150">Aplikacja stosuje ponownego zapisywania i Przekierowanie reguł i zawiera adres URL nowych lub przekierowanego.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-150">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="e6ae8-151">Kiedy należy używać oprogramowanie pośredniczące ponownego zapisywania adresów URL</span><span class="sxs-lookup"><span data-stu-id="e6ae8-151">When to use URL Rewriting Middleware</span></span>

<span data-ttu-id="e6ae8-152">Użyj oprogramowanie pośredniczące ponownego zapisywania adresów URL, gdy nie można używać [moduł ponowne zapisywanie adresów URL](https://www.iis.net/downloads/microsoft/url-rewrite) z usługami IIS w systemie Windows Server [modułu mod_rewrite Apache](https://httpd.apache.org/docs/2.4/rewrite/) na serwerze Apache [na NginxponownegozapisywaniaadresówURL](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), lub aplikacja jest hostowana na [serwera HTTP.sys](xref:fundamentals/servers/httpsys) (wcześniej noszącą nazwę [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="e6ae8-152">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="e6ae8-153">Główne powody korzystania na serwerze adresu URL przebudowywania technologii w IIS, Apache i Nginx są, oprogramowanie pośredniczące nie obsługuje funkcji pełnego tych modułów i wydajność oprogramowania pośredniczącego prawdopodobnie nie będzie zgodne z modułów.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-153">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="e6ae8-154">Istnieją jednak pewne funkcje modułów serwera, która nie działa z projektami ASP.NET Core, takich jak `IsFile` i `IsDirectory` ograniczenia moduł ponowne zapisywanie adresów IIS.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-154">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="e6ae8-155">W tych scenariuszach Użyj oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-155">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="e6ae8-156">Package</span><span class="sxs-lookup"><span data-stu-id="e6ae8-156">Package</span></span>

<span data-ttu-id="e6ae8-157">Aby dołączyć oprogramowanie pośredniczące w projekcie, należy dodać odwołanie do [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-157">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="e6ae8-158">Ta funkcja jest dostępna dla aplikacji przeznaczonych dla platformy ASP.NET Core 1.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-158">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="e6ae8-159">Opcje i rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="e6ae8-159">Extension and options</span></span>

<span data-ttu-id="e6ae8-160">Ustanowienia sieci ponowne zapisywanie adresów URL i przekierować reguły przez utworzenie wystąpienia [RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) klasy przy użyciu metody rozszerzenia dla poszczególnych reguł.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-160">Establish your URL rewrite and redirect rules by creating an instance of the [RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) class with extension methods for each of your rules.</span></span> <span data-ttu-id="e6ae8-161">Utworzyć łańcuch wielu reguł w kolejności, że chcesz je przetworzyć.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-161">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="e6ae8-162">`RewriteOptions` Są przekazywane do oprogramowanie pośredniczące ponownego zapisywania adresów URL jest dodawany do potoku żądania za pomocą `app.UseRewriter(options);`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-162">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1")
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true)
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")
        .Add(RedirectXMLRequests)
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="redirect-non-www-to-www"></a><span data-ttu-id="e6ae8-163">Przekierowanie nie www do www</span><span class="sxs-lookup"><span data-stu-id="e6ae8-163">Redirect non-www to www</span></span>

<span data-ttu-id="e6ae8-164">Trzy opcje umożliwiają aplikacji, aby przekierować non -`www` żądania `www`:</span><span class="sxs-lookup"><span data-stu-id="e6ae8-164">Three options permit the app to redirect non-`www` requests to `www`:</span></span>

* <span data-ttu-id="e6ae8-165">[AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; trwałe przekierowanie żądania do `www` poddomeny, jeśli żądanie ma wartość inną niż`www`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-165">[AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; Permanently redirect the request to the `www` subdomain if the request is non-`www`.</span></span> <span data-ttu-id="e6ae8-166">Przekierowuje z [Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect) kod stanu.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-166">Redirects with a [Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect) status code.</span></span>
* <span data-ttu-id="e6ae8-167">[AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; przekierować żądania `www` poddomeny, jeśli żądanie przychodzące ma wartość inną niż`www`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-167">[AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="e6ae8-168">Przekierowuje z [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) kod stanu.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-168">Redirects with a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) status code.</span></span>
* <span data-ttu-id="e6ae8-169">[AddRedirectToWww (RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; przekierować żądania `www` poddomeny, jeśli żądanie przychodzące ma wartość inną niż`www`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-169">[AddRedirectToWww(RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; Redirect the request to the `www` subdomain if the incoming request is non-`www`.</span></span> <span data-ttu-id="e6ae8-170">Umożliwia podanie kodu stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-170">Allows you to provide the status code for the response.</span></span> <span data-ttu-id="e6ae8-171">Użyj pola [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) klasy do przypisania do `AddRedirectToWww`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-171">Use the fields of the [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) class for assignments to `AddRedirectToWww`.</span></span>

::: moniker-end

### <a name="url-redirect"></a><span data-ttu-id="e6ae8-172">Adres URL przekierowania</span><span class="sxs-lookup"><span data-stu-id="e6ae8-172">URL redirect</span></span>

<span data-ttu-id="e6ae8-173">Użyj `AddRedirect` Przekierowywanie żądań.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-173">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="e6ae8-174">Pierwszy parametr zawiera Twoje wyrażenia regularnego do dopasowania w ścieżce przychodzącego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-174">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="e6ae8-175">Drugi parametr jest ciąg zastępujący.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-175">The second parameter is the replacement string.</span></span> <span data-ttu-id="e6ae8-176">Trzeci parametr, jeśli jest obecny, określa kod stanu.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-176">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="e6ae8-177">Jeśli nie określisz kod stanu, domyślnie 302 (Found), która wskazuje, że zasób tymczasowo przenieść lub zastąpiony.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-177">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="e6ae8-178">W przeglądarce za pomocą narzędzi dla deweloperów, włączone, należy wysłać żądanie do przykładowej aplikacji ze ścieżką `/redirect-rule/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-178">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="e6ae8-179">Wyrażenie regularne dopasowuje ścieżki żądania w `redirect-rule/(.*)`, a ścieżka została zastąpiona `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-179">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="e6ae8-180">Przekieruj adres URL jest wysyłane z powrotem do klienta z kodem stanu 302 (Found).</span><span class="sxs-lookup"><span data-stu-id="e6ae8-180">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="e6ae8-181">Przeglądarka sprawia, że nowe wezwanie pod adresem URL przekierowania, który jest wyświetlany na pasku adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-181">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="e6ae8-182">Ponieważ żadne reguły w przykładowej aplikacji odpowiada na adres URL przekierowania, drugie żądanie odbiera odpowiedź 200 (OK) z poziomu aplikacji, a treść odpowiedzi zawiera adres URL przekierowania.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-182">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="e6ae8-183">Komunikacja dwukierunkowa jest wysyłane do serwera, gdy adres URL jest *przekierowanie*.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-183">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="e6ae8-184">Należy zachować ostrożność podczas ustanawiania reguł przekierowania.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-184">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="e6ae8-185">Twoje zasady przekierowania są obliczane na każde żądanie do aplikacji, nawet po przekierowania.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-185">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="e6ae8-186">Łatwo przypadkowo spowodować powstanie pętli nieskończonej przekierowania.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-186">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="e6ae8-187">Oryginalne żądanie: `/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="e6ae8-187">Original Request: `/redirect-rule/1234/5678`</span></span>

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="e6ae8-189">Część numerowanie wyrażenia zawartgoe w nawiasach jest nazywany *grupa przechwytywania*.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-189">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="e6ae8-190">Kropka (`.`) wyrażenie oznacza, że *dopasowuje dowolny znak*.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-190">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="e6ae8-191">Gwiazdka (`*`) wskazuje *Dopasuj poprzedni znak zero lub więcej razy*.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-191">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="e6ae8-192">W związku z tym, segmenty ostatnie dwie ścieżki adresu URL, `1234/5678`, są przechwytywane przez grupę przechwytywania `(.*)`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-192">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="e6ae8-193">Dowolna wartość należy podać w adresie URL żądania po `redirect-rule/` są przechwytywane przez tę grupę przechwytywania jednego.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-193">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="e6ae8-194">W ciągu zamiennym przechwyconych grupach są wstrzykiwane do ciągu znakiem dolara (`$`) wraz z numerem sekwencji przechwytywania.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-194">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="e6ae8-195">Wartość pierwszej grupy przechwytywania są uzyskiwane z `$1`, druga z `$2`, i kontynuują w sekwencji dla grup przechwytywania w swojej wyrażenia regularnego.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-195">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="e6ae8-196">Tylko jedna grupa przechwycone z wyrażeniem regularnym reguła przekierowania w jest przykładowej aplikacji, więc tylko jedna grupa wprowadzonego w ciągu zamiennym, który jest `$1`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-196">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="e6ae8-197">Po zastosowaniu reguły staje się adres URL `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-197">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="e6ae8-198">Adres URL przekierowania do bezpiecznego punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="e6ae8-198">URL redirect to a secure endpoint</span></span>

<span data-ttu-id="e6ae8-199">Użyj `AddRedirectToHttps` Przekierowywanie żądań HTTP do tego samego hosta i ścieżkę przy użyciu protokołu HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="e6ae8-199">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="e6ae8-200">Jeśli nie został dostarczony kod stanu, oprogramowanie pośredniczące domyślnie 302 (Found).</span><span class="sxs-lookup"><span data-stu-id="e6ae8-200">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="e6ae8-201">Jeśli port nie jest podany, oprogramowanie pośredniczące, wartość domyślna to `null`, co oznacza, że protokół zmieni się na `https://` i klient uzyskuje dostęp do zasobów na porcie 443.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-201">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="e6ae8-202">W przykładzie pokazano, jak ustawić kod stanu 301 (trwale przeniesiona) i zmień numer portu na 5001.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-202">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="e6ae8-203">Użyj `AddRedirectToHttpsPermanent` Aby przekierować niezabezpieczone żądania do tego samego hosta i ścieżkę z bezpiecznego protokołu HTTPS (`https://` na porcie 443).</span><span class="sxs-lookup"><span data-stu-id="e6ae8-203">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="e6ae8-204">Oprogramowanie pośredniczące ustawia kod stanu 301 (trwale przeniesiona).</span><span class="sxs-lookup"><span data-stu-id="e6ae8-204">The middleware sets the status code to 301 (Moved Permanently).</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> <span data-ttu-id="e6ae8-205">Podczas przekierowywania HTTPS bez potrzeby przekierowania dodatkowe reguły, zaleca się za pomocą oprogramowania pośredniczącego przekierowania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-205">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware.</span></span> <span data-ttu-id="e6ae8-206">Aby uzyskać więcej informacji, zobacz [Wymuszanie protokołu HTTPS](xref:security/enforcing-ssl#require-https) tematu.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-206">For more information, see the [Enforce HTTPS](xref:security/enforcing-ssl#require-https) topic.</span></span>

<span data-ttu-id="e6ae8-207">Przykładowa aplikacja jest w stanie pokazuje sposób użycia studia `AddRedirectToHttps` lub `AddRedirectToHttpsPermanent`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-207">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="e6ae8-208">Dodaj metodę rozszerzenia, aby `RewriteOptions`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-208">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="e6ae8-209">Przesyłania niezabezpieczone żądania do aplikacji na dowolny adres URL.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-209">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="e6ae8-210">Odrzuć zabezpieczeń przeglądarki, ostrzeżenie, że nie jest zaufany certyfikat z podpisem własnym, lub Utwórz wyjątek, aby ufać certyfikatowi.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-210">Dismiss the browser security warning that the self-signed certificate is untrusted or create an exception to trust the certificate.</span></span>

<span data-ttu-id="e6ae8-211">Oryginalne żądanie, używając `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="e6ae8-211">Original Request using `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span></span>

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="e6ae8-213">Oryginalne żądanie, używając `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="e6ae8-213">Original Request using `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span></span>

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="e6ae8-215">Ponowne zapisywanie adresów URL</span><span class="sxs-lookup"><span data-stu-id="e6ae8-215">URL rewrite</span></span>

<span data-ttu-id="e6ae8-216">Użyj `AddRewrite` można utworzyć regułę ponownego zapisywania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-216">Use `AddRewrite` to create a rule for rewriting URLs.</span></span> <span data-ttu-id="e6ae8-217">Pierwszy parametr zawiera Twoje wyrażenia regularnego do dopasowania na przychodzące Ścieżka adresu URL.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-217">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="e6ae8-218">Drugi parametr jest ciąg zastępujący.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-218">The second parameter is the replacement string.</span></span> <span data-ttu-id="e6ae8-219">Trzeci parametr `skipRemainingRules: {true|false}`, wskazuje, aby oprogramowanie pośredniczące umożliwia określenie, czy pominąć reguły ponownego zapisywania dodatkowe, jeśli zastosowano bieżącej reguły.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-219">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="e6ae8-220">Oryginalne żądanie: `/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="e6ae8-220">Original Request: `/rewrite-rule/1234/5678`</span></span>

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="e6ae8-222">Pierwszą rzeczą, którą można zauważyć, wyrażenie regularne jest pojawia się znak daszka (`^`) na początku wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-222">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="e6ae8-223">Oznacza to, że dopasowanie rozpoczyna się od początku ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-223">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="e6ae8-224">We wcześniejszym przykładzie z regułą przekierowania `redirect-rule/(.*)`, nie ma żadnych daszka, na początku wyrażenia regularnego; w związku z tym, mogą poprzedzać wszystkie znaki `redirect-rule/` w ścieżce dla pomyślnego dopasowania.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-224">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="e6ae8-225">Ścieżka</span><span class="sxs-lookup"><span data-stu-id="e6ae8-225">Path</span></span>                               | <span data-ttu-id="e6ae8-226">Dopasowanie</span><span class="sxs-lookup"><span data-stu-id="e6ae8-226">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="e6ae8-227">Tak</span><span class="sxs-lookup"><span data-stu-id="e6ae8-227">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="e6ae8-228">Tak</span><span class="sxs-lookup"><span data-stu-id="e6ae8-228">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="e6ae8-229">Tak</span><span class="sxs-lookup"><span data-stu-id="e6ae8-229">Yes</span></span>   |

<span data-ttu-id="e6ae8-230">Reguły ponownego pisania `^rewrite-rule/(\d+)/(\d+)`, tylko dopasowuje ścieżek, jeśli zaczyna `rewrite-rule/`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-230">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="e6ae8-231">Należy zauważyć różnicę w dopasowywania między poniższe reguły ponownego pisania i reguła przekierowania powyżej.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-231">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="e6ae8-232">Ścieżka</span><span class="sxs-lookup"><span data-stu-id="e6ae8-232">Path</span></span>                              | <span data-ttu-id="e6ae8-233">Dopasowanie</span><span class="sxs-lookup"><span data-stu-id="e6ae8-233">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="e6ae8-234">Tak</span><span class="sxs-lookup"><span data-stu-id="e6ae8-234">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="e6ae8-235">Nie</span><span class="sxs-lookup"><span data-stu-id="e6ae8-235">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="e6ae8-236">Nie</span><span class="sxs-lookup"><span data-stu-id="e6ae8-236">No</span></span>    |

<span data-ttu-id="e6ae8-237">Następujące `^rewrite-rule/` część wyrażenia, istnieją dwie grupy przechwytywania, `(\d+)/(\d+)`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-237">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="e6ae8-238">`\d` Oznacza *dopasowania cyfrę (numer)*.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-238">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="e6ae8-239">Znak plus (`+`) oznacza, że *zgodne z co najmniej jeden znak poprzedzający*.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-239">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="e6ae8-240">W związku z tym adres URL musi zawierać liczbę następuje ukośnikiem do przodu następuje inny numer.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-240">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="e6ae8-241">Przechwytywanie, te grupy są wstrzykiwane do nowych adresu URL jako `$1` i `$2`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-241">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="e6ae8-242">Ciąg zastępujący reguły ponownego zapisywania umieszcza przechwyconych grupach w zmiennej querystring.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-242">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="e6ae8-243">Żądana ścieżka `/rewrite-rule/1234/5678` jest przepisany można uzyskać zasobu w `/rewritten?var1=1234&var2=5678`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-243">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="e6ae8-244">Jeśli ciąg zapytania jest obecna na oryginalne żądanie, są zachowywane, gdy adres URL jest przepisany.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-244">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="e6ae8-245">Nie ma żadnych obie strony do serwera można uzyskać zasobu.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-245">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="e6ae8-246">Jeśli zasób istnieje, ma pobrać i zwracany do klienta z kodem stanu 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="e6ae8-246">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="e6ae8-247">Ponieważ klient nie jest przekierowany, nie powoduje zmiany adresu URL w pasku adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-247">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="e6ae8-248">O ile dotyczy to klient, nigdy nie operacji ponownego zapisywania adresu URL wystąpił.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-248">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="e6ae8-249">Użyj `skipRemainingRules: true` zawsze, gdy jest to możliwe, ponieważ reguł dopasowania jest procesem kosztowne i skraca czas odpowiedzi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-249">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="e6ae8-250">Najszybszy odpowiedzi aplikacji:</span><span class="sxs-lookup"><span data-stu-id="e6ae8-250">For the fastest app response:</span></span>
> * <span data-ttu-id="e6ae8-251">Kolejność reguły ponownego zapisywania z najczęściej dopasowane reguły do najmniej często dopasowane reguły.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-251">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="e6ae8-252">Pomiń przetwarzanie pozostałych reguł, gdy pasują do przetwarzania nie dodatkowe reguły jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-252">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="e6ae8-253">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="e6ae8-253">Apache mod_rewrite</span></span>

<span data-ttu-id="e6ae8-254">Zastosuj reguły mod_rewrite Apache z `AddApacheModRewrite`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-254">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="e6ae8-255">Upewnij się, że plik reguł jest wdrażany z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-255">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="e6ae8-256">Aby uzyskać więcej informacji i przykłady reguł mod_rewrite, zobacz [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span><span class="sxs-lookup"><span data-stu-id="e6ae8-256">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e6ae8-257">A `StreamReader` służy do odczytywania reguły z *ApacheModRewrite.txt* pliku reguł.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-257">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e6ae8-258">Pierwszy parametr przyjmuje `IFileProvider`, która jest oferowana w ramach [wstrzykiwanie zależności](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="e6ae8-258">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="e6ae8-259">`IHostingEnvironment` Są wstrzykiwane do zapewnienia `ContentRootFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-259">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="e6ae8-260">Drugi parametr jest ścieżka do pliku reguł, który jest *ApacheModRewrite.txt* w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-260">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="e6ae8-261">Przykładowa aplikacja przekierowuje żądania z `/apache-mod-rules-redirect/(.\*)` do `/redirected?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-261">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="e6ae8-262">Kod stanu odpowiedzi to 302 (Found).</span><span class="sxs-lookup"><span data-stu-id="e6ae8-262">The response status code is 302 (Found).</span></span>

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

<span data-ttu-id="e6ae8-263">Oryginalne żądanie: `/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="e6ae8-263">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi](url-rewriting/_static/add_apache_mod_redirect.png)

<span data-ttu-id="e6ae8-265">Oprogramowanie pośredniczące obsługuje następujące zmienne serwera Apache mod_rewrite:</span><span class="sxs-lookup"><span data-stu-id="e6ae8-265">The middleware supports the following Apache mod_rewrite server variables:</span></span>

* <span data-ttu-id="e6ae8-266">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="e6ae8-266">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="e6ae8-267">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="e6ae8-267">HTTP_ACCEPT</span></span>
* <span data-ttu-id="e6ae8-268">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="e6ae8-268">HTTP_CONNECTION</span></span>
* <span data-ttu-id="e6ae8-269">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="e6ae8-269">HTTP_COOKIE</span></span>
* <span data-ttu-id="e6ae8-270">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="e6ae8-270">HTTP_FORWARDED</span></span>
* <span data-ttu-id="e6ae8-271">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="e6ae8-271">HTTP_HOST</span></span>
* <span data-ttu-id="e6ae8-272">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="e6ae8-272">HTTP_REFERER</span></span>
* <span data-ttu-id="e6ae8-273">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="e6ae8-273">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="e6ae8-274">HTTPS</span><span class="sxs-lookup"><span data-stu-id="e6ae8-274">HTTPS</span></span>
* <span data-ttu-id="e6ae8-275">PROTOKÓŁ IPV6</span><span class="sxs-lookup"><span data-stu-id="e6ae8-275">IPV6</span></span>
* <span data-ttu-id="e6ae8-276">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="e6ae8-276">QUERY_STRING</span></span>
* <span data-ttu-id="e6ae8-277">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="e6ae8-277">REMOTE_ADDR</span></span>
* <span data-ttu-id="e6ae8-278">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="e6ae8-278">REMOTE_PORT</span></span>
* <span data-ttu-id="e6ae8-279">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="e6ae8-279">REQUEST_FILENAME</span></span>
* <span data-ttu-id="e6ae8-280">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="e6ae8-280">REQUEST_METHOD</span></span>
* <span data-ttu-id="e6ae8-281">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="e6ae8-281">REQUEST_SCHEME</span></span>
* <span data-ttu-id="e6ae8-282">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="e6ae8-282">REQUEST_URI</span></span>
* <span data-ttu-id="e6ae8-283">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="e6ae8-283">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="e6ae8-284">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="e6ae8-284">SERVER_ADDR</span></span>
* <span data-ttu-id="e6ae8-285">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="e6ae8-285">SERVER_PORT</span></span>
* <span data-ttu-id="e6ae8-286">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="e6ae8-286">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="e6ae8-287">CZAS</span><span class="sxs-lookup"><span data-stu-id="e6ae8-287">TIME</span></span>
* <span data-ttu-id="e6ae8-288">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="e6ae8-288">TIME_DAY</span></span>
* <span data-ttu-id="e6ae8-289">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="e6ae8-289">TIME_HOUR</span></span>
* <span data-ttu-id="e6ae8-290">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="e6ae8-290">TIME_MIN</span></span>
* <span data-ttu-id="e6ae8-291">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="e6ae8-291">TIME_MON</span></span>
* <span data-ttu-id="e6ae8-292">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="e6ae8-292">TIME_SEC</span></span>
* <span data-ttu-id="e6ae8-293">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="e6ae8-293">TIME_WDAY</span></span>
* <span data-ttu-id="e6ae8-294">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="e6ae8-294">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="e6ae8-295">Reguły moduł ponowne zapisywanie adresów URL usług IIS</span><span class="sxs-lookup"><span data-stu-id="e6ae8-295">IIS URL Rewrite Module rules</span></span>

<span data-ttu-id="e6ae8-296">Aby użyć reguł, które dotyczą moduł ponowne zapisywanie adresów URL usług IIS, należy użyć `AddIISUrlRewrite`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-296">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="e6ae8-297">Upewnij się, że plik reguł jest wdrażany z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-297">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="e6ae8-298">Nie bezpośrednie oprogramowania pośredniczącego do zastosowania usługi *web.config* pliku podczas uruchamiania w systemie Windows Server w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-298">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="e6ae8-299">Za pomocą programu IIS, reguły te powinny być przechowywane poza swojej *web.config* w celu uniknięcia konfliktów z modułem ponownego zapisywania usług IIS.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-299">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="e6ae8-300">Aby uzyskać więcej informacji i przykłady reguł moduł ponowne zapisywanie adresów URL usług IIS, zobacz [przy użyciu adresu Url Nadpisz modułu 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) i [odwołanie konfiguracji modułu ponowne zapisywanie adresów URL](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span><span class="sxs-lookup"><span data-stu-id="e6ae8-300">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e6ae8-301">A `StreamReader` służy do odczytywania reguły z *IISUrlRewrite.xml* pliku reguł.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-301">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e6ae8-302">Pierwszy parametr przyjmuje `IFileProvider`, podczas gdy drugi parametr jest ścieżka do pliku zasady XML, który jest *IISUrlRewrite.xml* w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-302">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="e6ae8-303">Przykładowa aplikacja ponownie zapisuje żądań z `/iis-rules-rewrite/(.*)` do `/rewritten?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-303">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="e6ae8-304">Odpowiedź jest wysyłana do klienta z kodem stanu 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="e6ae8-304">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

<span data-ttu-id="e6ae8-305">Oryginalne żądanie: `/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="e6ae8-305">Original Request: `/iis-rules-rewrite/1234`</span></span>

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="e6ae8-307">Jeśli masz aktywne moduł ponowne zapisywanie adresów usług IIS przy użyciu skonfigurowanych reguł poziom serwera, które mogło mieć wpływ na aplikację w sposób niepożądane, możesz wyłączyć moduł ponowne zapisywanie adresów usług IIS dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-307">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="e6ae8-308">Aby uzyskać więcej informacji, zobacz [moduły IIS wyłączenie](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="e6ae8-308">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="e6ae8-309">Nieobsługiwane funkcje</span><span class="sxs-lookup"><span data-stu-id="e6ae8-309">Unsupported features</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e6ae8-310">Oprogramowanie pośredniczące zwolnione z platformą ASP.NET Core 2.x nie obsługuje następujące funkcje moduł ponowne zapisywanie adresów URL usług IIS:</span><span class="sxs-lookup"><span data-stu-id="e6ae8-310">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="e6ae8-311">Reguły ruchu wychodzącego</span><span class="sxs-lookup"><span data-stu-id="e6ae8-311">Outbound Rules</span></span>
* <span data-ttu-id="e6ae8-312">Zmienne serwera niestandardowego</span><span class="sxs-lookup"><span data-stu-id="e6ae8-312">Custom Server Variables</span></span>
* <span data-ttu-id="e6ae8-313">Symboli wieloznacznych</span><span class="sxs-lookup"><span data-stu-id="e6ae8-313">Wildcards</span></span>
* <span data-ttu-id="e6ae8-314">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="e6ae8-314">LogRewrittenUrl</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="e6ae8-315">Oprogramowanie pośredniczące zwolnione z platformą ASP.NET Core 1.x nie obsługuje następujące funkcje moduł ponowne zapisywanie adresów URL usług IIS:</span><span class="sxs-lookup"><span data-stu-id="e6ae8-315">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="e6ae8-316">Globalne reguły</span><span class="sxs-lookup"><span data-stu-id="e6ae8-316">Global Rules</span></span>
* <span data-ttu-id="e6ae8-317">Reguły ruchu wychodzącego</span><span class="sxs-lookup"><span data-stu-id="e6ae8-317">Outbound Rules</span></span>
* <span data-ttu-id="e6ae8-318">Map ponownego zapisywania</span><span class="sxs-lookup"><span data-stu-id="e6ae8-318">Rewrite Maps</span></span>
* <span data-ttu-id="e6ae8-319">Akcja CustomResponse</span><span class="sxs-lookup"><span data-stu-id="e6ae8-319">CustomResponse Action</span></span>
* <span data-ttu-id="e6ae8-320">Zmienne serwera niestandardowego</span><span class="sxs-lookup"><span data-stu-id="e6ae8-320">Custom Server Variables</span></span>
* <span data-ttu-id="e6ae8-321">Symboli wieloznacznych</span><span class="sxs-lookup"><span data-stu-id="e6ae8-321">Wildcards</span></span>
* <span data-ttu-id="e6ae8-322">Akcja: CustomResponse</span><span class="sxs-lookup"><span data-stu-id="e6ae8-322">Action:CustomResponse</span></span>
* <span data-ttu-id="e6ae8-323">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="e6ae8-323">LogRewrittenUrl</span></span>

::: moniker-end

#### <a name="supported-server-variables"></a><span data-ttu-id="e6ae8-324">Zmienne serwera obsługiwane</span><span class="sxs-lookup"><span data-stu-id="e6ae8-324">Supported server variables</span></span>

<span data-ttu-id="e6ae8-325">Oprogramowanie pośredniczące obsługuje następujące zmienne serwera moduł ponowne zapisywanie adresów URL usług IIS:</span><span class="sxs-lookup"><span data-stu-id="e6ae8-325">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>

* <span data-ttu-id="e6ae8-326">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="e6ae8-326">CONTENT_LENGTH</span></span>
* <span data-ttu-id="e6ae8-327">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="e6ae8-327">CONTENT_TYPE</span></span>
* <span data-ttu-id="e6ae8-328">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="e6ae8-328">HTTP_ACCEPT</span></span>
* <span data-ttu-id="e6ae8-329">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="e6ae8-329">HTTP_CONNECTION</span></span>
* <span data-ttu-id="e6ae8-330">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="e6ae8-330">HTTP_COOKIE</span></span>
* <span data-ttu-id="e6ae8-331">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="e6ae8-331">HTTP_HOST</span></span>
* <span data-ttu-id="e6ae8-332">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="e6ae8-332">HTTP_REFERER</span></span>
* <span data-ttu-id="e6ae8-333">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="e6ae8-333">HTTP_URL</span></span>
* <span data-ttu-id="e6ae8-334">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="e6ae8-334">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="e6ae8-335">HTTPS</span><span class="sxs-lookup"><span data-stu-id="e6ae8-335">HTTPS</span></span>
* <span data-ttu-id="e6ae8-336">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="e6ae8-336">LOCAL_ADDR</span></span>
* <span data-ttu-id="e6ae8-337">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="e6ae8-337">QUERY_STRING</span></span>
* <span data-ttu-id="e6ae8-338">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="e6ae8-338">REMOTE_ADDR</span></span>
* <span data-ttu-id="e6ae8-339">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="e6ae8-339">REMOTE_PORT</span></span>
* <span data-ttu-id="e6ae8-340">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="e6ae8-340">REQUEST_FILENAME</span></span>
* <span data-ttu-id="e6ae8-341">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="e6ae8-341">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="e6ae8-342">Możesz również uzyskać `IFileProvider` za pośrednictwem `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-342">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="e6ae8-343">Takie podejście może dostarczyć większą elastyczność dla lokalizacji usługi ponownego zapisywania plików reguły.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-343">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="e6ae8-344">Upewnij się, że pliki reguły ponownego zapisywania są wdrażane do serwera w ścieżce, których udzielasz.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-344">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="e6ae8-345">Reguły oparte na metodzie</span><span class="sxs-lookup"><span data-stu-id="e6ae8-345">Method-based rule</span></span>

<span data-ttu-id="e6ae8-346">Użyj `Add(Action<RewriteContext> applyRule)` do zaimplementowania własnej logiki reguły w metodzie.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-346">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="e6ae8-347">`RewriteContext` Udostępnia `HttpContext` do użycia w metodzie.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-347">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="e6ae8-348">`RewriteContext.Result` Określa, jak dodatkowe potok przetwarzania jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-348">The `RewriteContext.Result` determines how additional pipeline processing is handled.</span></span>

| `RewriteContext.Result`              | <span data-ttu-id="e6ae8-349">Akcja</span><span class="sxs-lookup"><span data-stu-id="e6ae8-349">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="e6ae8-350">`RuleResult.ContinueRules` (ustawienie domyślne)</span><span class="sxs-lookup"><span data-stu-id="e6ae8-350">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="e6ae8-351">Kontynuować stosowanie reguł</span><span class="sxs-lookup"><span data-stu-id="e6ae8-351">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="e6ae8-352">Zatrzymaj stosowanie reguł i wysłania odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="e6ae8-352">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="e6ae8-353">Zaprzestanie stosowania reguły i wysyłać kontekstu do następnego oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="e6ae8-353">Stop applying rules and send the context to the next middleware</span></span> |

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="e6ae8-354">Przykładowa aplikacja pokazuje metody, która przekierowuje żądania dla ścieżek, które kończy się *.xml*.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-354">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="e6ae8-355">Jeśli wykonasz żądanie typu `/file.xml`, nastąpi przekierowanie do `/xmlfiles/file.xml`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-355">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="e6ae8-356">Kod stanu jest równa 301 (trwale przeniesiona).</span><span class="sxs-lookup"><span data-stu-id="e6ae8-356">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="e6ae8-357">Dla przekierowania Musisz jawnie ustawić kod stanu odpowiedzi; w przeciwnym razie zwracany jest kod stanu 200 (OK) i Przekierowanie nie zostanie wykonana na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-357">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

<span data-ttu-id="e6ae8-358">Oryginalne żądanie: `/file.xml`</span><span class="sxs-lookup"><span data-stu-id="e6ae8-358">Original Request: `/file.xml`</span></span>

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi dla file.xml](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="e6ae8-360">Na podstawie irule na podstawie reguł</span><span class="sxs-lookup"><span data-stu-id="e6ae8-360">IRule-based rule</span></span>

<span data-ttu-id="e6ae8-361">Użyj `Add(IRule)` do hermetyzacji własną logiką reguły określoną w klasę, która implementuje `IRule` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-361">Use `Add(IRule)` to encapsulate your own rule logic in a class that implements the `IRule` interface.</span></span> <span data-ttu-id="e6ae8-362">Za pomocą `IRule` zapewnia większą elastyczność na podejście oparte na metodzie reguły.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-362">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="e6ae8-363">Klasa doprowadzić mogą obejmować konstruktora, w którym możesz przekazać parametry `ApplyRule` metody.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-363">Your implementaion class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

<span data-ttu-id="e6ae8-364">Wartości parametrów w przykładowej aplikacji dla `extension` i `newPath` są sprawdzane w celu spełnienia kilku warunków.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-364">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="e6ae8-365">`extension` Musi zawierać wartość, a wartość musi być *.png*, *.jpg*, lub *.gif*.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-365">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="e6ae8-366">Jeśli `newPath` nie jest prawidłowy, `ArgumentException` zgłaszany.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-366">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="e6ae8-367">Jeśli wykonasz żądanie typu *image.png*, nastąpi przekierowanie do `/png-images/image.png`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-367">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="e6ae8-368">Jeśli wykonasz żądanie typu *image.jpg*, nastąpi przekierowanie do `/jpg-images/image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-368">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="e6ae8-369">Kod stanu jest równa 301 (przeniesione trwale), a `context.Result` jest ustawiona, aby zatrzymać przetwarzanie reguł i wysłania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-369">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

<span data-ttu-id="e6ae8-370">Oryginalne żądanie: `/image.png`</span><span class="sxs-lookup"><span data-stu-id="e6ae8-370">Original Request: `/image.png`</span></span>

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi dla image.png](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="e6ae8-372">Oryginalne żądanie: `/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="e6ae8-372">Original Request: `/image.jpg`</span></span>

![Okno przeglądarki z narzędzi deweloperskich do śledzenia żądań i odpowiedzi dla image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="e6ae8-374">Przykłady wyrażeń regularnych</span><span class="sxs-lookup"><span data-stu-id="e6ae8-374">Regex examples</span></span>

| <span data-ttu-id="e6ae8-375">Cel</span><span class="sxs-lookup"><span data-stu-id="e6ae8-375">Goal</span></span> | <span data-ttu-id="e6ae8-376">Wyrażenie regularne ciągu &</span><span class="sxs-lookup"><span data-stu-id="e6ae8-376">Regex String &</span></span><br><span data-ttu-id="e6ae8-377">Przykład dopasowania</span><span class="sxs-lookup"><span data-stu-id="e6ae8-377">Match Example</span></span> | <span data-ttu-id="e6ae8-378">Ciąg zastępujący &</span><span class="sxs-lookup"><span data-stu-id="e6ae8-378">Replacement String &</span></span><br><span data-ttu-id="e6ae8-379">Przykład danych wyjściowych</span><span class="sxs-lookup"><span data-stu-id="e6ae8-379">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="e6ae8-380">Zmodyfikuj ścieżkę do querystring</span><span class="sxs-lookup"><span data-stu-id="e6ae8-380">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="e6ae8-381">Usuń ukośnika.</span><span class="sxs-lookup"><span data-stu-id="e6ae8-381">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="e6ae8-382">Wymuszanie ukośnika</span><span class="sxs-lookup"><span data-stu-id="e6ae8-382">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="e6ae8-383">Należy unikać ponownego zapisywania adresów określone żądania</span><span class="sxs-lookup"><span data-stu-id="e6ae8-383">Avoid rewriting specific requests</span></span> | <span data-ttu-id="e6ae8-384">`^(.*)(?<!\.axd)$` lub `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="e6ae8-384">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="e6ae8-385">Tak: `/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="e6ae8-385">Yes: `/resource.htm`</span></span><br><span data-ttu-id="e6ae8-386">Nie: `/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="e6ae8-386">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="e6ae8-387">Rozmieszczanie segmenty adresu URL</span><span class="sxs-lookup"><span data-stu-id="e6ae8-387">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="e6ae8-388">Zastąp segment adresu URL</span><span class="sxs-lookup"><span data-stu-id="e6ae8-388">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="e6ae8-389">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e6ae8-389">Additional resources</span></span>

* [<span data-ttu-id="e6ae8-390">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="e6ae8-390">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="e6ae8-391">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="e6ae8-391">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="e6ae8-392">Wyrażenia regularne w .NET</span><span class="sxs-lookup"><span data-stu-id="e6ae8-392">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="e6ae8-393">Język wyrażeń regularnych — podręczny wykaz</span><span class="sxs-lookup"><span data-stu-id="e6ae8-393">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="e6ae8-394">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="e6ae8-394">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="e6ae8-395">Za pomocą moduł ponowne zapisywanie adresów Url w wersji 2.0 (dla usług IIS)</span><span class="sxs-lookup"><span data-stu-id="e6ae8-395">Using Url Rewrite Module 2.0 (for IIS)</span></span>](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="e6ae8-396">Odwołania do konfiguracji modułu ponowne zapisywanie adresów URL</span><span class="sxs-lookup"><span data-stu-id="e6ae8-396">URL Rewrite Module Configuration Reference</span></span>](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="e6ae8-397">Forum moduł ponowne zapisywanie adresów URL usług IIS</span><span class="sxs-lookup"><span data-stu-id="e6ae8-397">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="e6ae8-398">Zachowaj proste Struktura adresu URL</span><span class="sxs-lookup"><span data-stu-id="e6ae8-398">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="e6ae8-399">10 ponownego zapisywania adresów URL porady i wskazówki</span><span class="sxs-lookup"><span data-stu-id="e6ae8-399">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="e6ae8-400">Slash — lub nie ukośnika</span><span class="sxs-lookup"><span data-stu-id="e6ae8-400">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
