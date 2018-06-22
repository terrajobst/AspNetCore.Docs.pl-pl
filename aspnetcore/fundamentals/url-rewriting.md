---
title: Adres URL ponowne zapisanie oprogramowania pośredniczącego w platformy ASP.NET Core
author: guardrex
description: Więcej informacji na temat adresu URL ponowne zapisywanie i przekierowywania z pośredniczącym ponowne zapisywanie adresów URL w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 08/17/2017
uid: fundamentals/url-rewriting
ms.openlocfilehash: d3484e222c4412a427d086c1b71a12b81095ba72
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276350"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="33c5f-103">Adres URL ponowne zapisanie oprogramowania pośredniczącego w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="33c5f-103">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="33c5f-104">Przez [Luke Latham](https://github.com/guardrex) i [Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="33c5f-104">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="33c5f-105">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="33c5f-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="33c5f-106">Ponowne zapisywanie adresów URL jest czynnością modyfikowania żądania, których adresy URL na podstawie co najmniej jeden wstępnie zdefiniowanych reguł.</span><span class="sxs-lookup"><span data-stu-id="33c5f-106">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="33c5f-107">Ponowne zapisywanie adresów URL powoduje abstrakcję między lokalizacje zasobów i ich adresów, tak aby lokalizacje i adresy nie są ściśle powiązane.</span><span class="sxs-lookup"><span data-stu-id="33c5f-107">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="33c5f-108">Istnieje kilka scenariuszy, w których jest przydatna ponowne zapisywanie adresów URL:</span><span class="sxs-lookup"><span data-stu-id="33c5f-108">There are several scenarios where URL rewriting is valuable:</span></span>

* <span data-ttu-id="33c5f-109">Przenoszenie lub zastępowanie tymczasowo lub trwale zasoby serwera przy zachowaniu stabilna lokalizatorów dla tych zasobów.</span><span class="sxs-lookup"><span data-stu-id="33c5f-109">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources.</span></span>
* <span data-ttu-id="33c5f-110">Dzielenie żądania przetwarzania różnych aplikacji lub obszarów jedną aplikację.</span><span class="sxs-lookup"><span data-stu-id="33c5f-110">Splitting request processing across different apps or across areas of one app.</span></span>
* <span data-ttu-id="33c5f-111">Usuwanie, dodając lub reorganizacja segmenty adresu URL na żądań przychodzących.</span><span class="sxs-lookup"><span data-stu-id="33c5f-111">Removing, adding, or reorganizing URL segments on incoming requests.</span></span>
* <span data-ttu-id="33c5f-112">Optymalizacja publiczne adresy URL do optymalizacji aparatu wyszukiwania (SEO).</span><span class="sxs-lookup"><span data-stu-id="33c5f-112">Optimizing public URLs for Search Engine Optimization (SEO).</span></span>
* <span data-ttu-id="33c5f-113">Pozwalające na używanie przyjaznych adresów URL publiczne, ułatwiające osobom prognozowania zawartość, którą znajdą, wykonując następujące łącze.</span><span class="sxs-lookup"><span data-stu-id="33c5f-113">Permitting the use of friendly public URLs to help people predict the content they will find by following a link.</span></span>
* <span data-ttu-id="33c5f-114">Przekierowywanie żądań niezabezpieczonego do bezpiecznego punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="33c5f-114">Redirecting insecure requests to secure endpoints.</span></span>
* <span data-ttu-id="33c5f-115">Zapobieganie hotlinking obrazu.</span><span class="sxs-lookup"><span data-stu-id="33c5f-115">Preventing image hotlinking.</span></span>

<span data-ttu-id="33c5f-116">Można zdefiniować reguły zmiana adresu URL na kilka sposobów, łącznie z wyrażenia regularnego Apache mod_rewrite modułu zasad, zasady przepisywania moduł usług IIS i przy użyciu reguły niestandardowej logiki.</span><span class="sxs-lookup"><span data-stu-id="33c5f-116">You can define rules for changing the URL in several ways, including Regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="33c5f-117">Ten dokument zawiera instrukcje dotyczące sposobu używania pośredniczącym ponowne zapisywanie adresów URL w aplikacji platformy ASP.NET Core ponowne zapisywanie adresów URL.</span><span class="sxs-lookup"><span data-stu-id="33c5f-117">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="33c5f-118">Ponowne zapisywanie adresów URL może zmniejszyć wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33c5f-118">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="33c5f-119">W przypadku, gdy jest to możliwe, należy ograniczyć liczba i złożoność reguł.</span><span class="sxs-lookup"><span data-stu-id="33c5f-119">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="33c5f-120">Ponowne zapisywanie adresów URL przekierowania i adres URL</span><span class="sxs-lookup"><span data-stu-id="33c5f-120">URL redirect and URL rewrite</span></span>

<span data-ttu-id="33c5f-121">Różnica w treść między *adres URL przekierowania* i *ponowne zapisywanie adresów URL* może wydawać się niewielkie w pierwszym, ale ma istotny wpływ na zapewnianie zasobów na klientach.</span><span class="sxs-lookup"><span data-stu-id="33c5f-121">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="33c5f-122">Ponowne zapisywanie adresów URL platformy ASP.NET Core w oprogramowaniu pośredniczącym jest w stanie spełniających potrzeby dla obu.</span><span class="sxs-lookup"><span data-stu-id="33c5f-122">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="33c5f-123">A *adres URL przekierowania* jest operacją po stronie klienta, w którym klient otrzymuje instrukcję do uzyskania dostępu do zasobu na inny adres.</span><span class="sxs-lookup"><span data-stu-id="33c5f-123">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="33c5f-124">To wymaga przesłania danych do serwera.</span><span class="sxs-lookup"><span data-stu-id="33c5f-124">This requires a round-trip to the server.</span></span> <span data-ttu-id="33c5f-125">Adres URL przekierowania do klienta zwracany jest wyświetlany w pasku adresu przeglądarki, gdy klient wysyła żądanie nowego zasobu.</span><span class="sxs-lookup"><span data-stu-id="33c5f-125">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> 

<span data-ttu-id="33c5f-126">Jeśli `/resource` jest *przekierowanie* do `/different-resource`, żądań klientów `/resource`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`.</span></span> <span data-ttu-id="33c5f-127">Serwer odpowiada, że klient Zażądaj zasobu pod adresem `/different-resource` z stan kodu wskazujący, że przekierowanie tymczasowych lub trwałych.</span><span class="sxs-lookup"><span data-stu-id="33c5f-127">The server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="33c5f-128">Klient wykonuje żądanie nowego zasobu pod adresem URL przekierowania.</span><span class="sxs-lookup"><span data-stu-id="33c5f-128">The client executes a new request for the resource at the redirect URL.</span></span>

![Punkt końcowy usługi WebAPI tymczasowo zmieniono z wersji 1 (wersja 1) w wersji 2 (v2) na serwerze.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="33c5f-134">Przekierowywanie żądań do innego adresu URL, wskazuje, czy przekierowanie stałych lub tymczasowych.</span><span class="sxs-lookup"><span data-stu-id="33c5f-134">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="33c5f-135">301 (trwale przeniesiona) kod stanu jest używany gdzie zasób ma adres URL nowego, stałe i chcesz nakazać klienta, że wszystkie przyszłe żądania dla zasobu powinien używać nowego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33c5f-135">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="33c5f-136">*Po odebraniu kod stanu 301, klient może buforować odpowiedzi.*</span><span class="sxs-lookup"><span data-stu-id="33c5f-136">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="33c5f-137">Kod stanu 302 (Found) jest używany gdzie przekierowania jest tymczasowe lub ogólnie podmiotu można zmienić w taki sposób, że klient nie należy przechowywać i ponowne użycie adres URL przekierowania w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="33c5f-137">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="33c5f-138">Aby uzyskać więcej informacji, zobacz [RFC 2616: definicje kod stanu](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="33c5f-138">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="33c5f-139">A *ponowne zapisywanie adresów URL* jest operacją po stronie serwera w celu zapewnienia zasobów z adresu innego zasobu.</span><span class="sxs-lookup"><span data-stu-id="33c5f-139">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="33c5f-140">Ponowne zapisywanie adresów URL nie wymaga przesłania danych do serwera.</span><span class="sxs-lookup"><span data-stu-id="33c5f-140">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="33c5f-141">Ponownie zapisane adres URL nie jest zwracana do klienta i nie będą wyświetlane na pasku adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="33c5f-141">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="33c5f-142">Gdy `/resource` jest *ulegną* do `/different-resource`, żądań klientów `/resource`, a serwerem *wewnętrznie* pobiera zasobu pod adresem `/different-resource`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-142">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="33c5f-143">Chociaż przez klienta może być w stanie pobrać zasobu pod adresem URL ponownie zapisane, klient nie będzie informacja, czy zasób istnieje pod adresem URL ponownie zapisane wysyła żądania i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="33c5f-143">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Punkt końcowy usługi WebAPI zmianie z wersji 1 (wersja 1) w wersji 2 (v2) na serwerze.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="33c5f-148">Adres URL przebudowywania przykładowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="33c5f-148">URL rewriting sample app</span></span>

<span data-ttu-id="33c5f-149">Można eksplorować funkcje pośredniczącym ponowne zapisywanie adresów URL z [adres URL przebudowywania Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span><span class="sxs-lookup"><span data-stu-id="33c5f-149">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span></span> <span data-ttu-id="33c5f-150">Aplikacja stosuje ponownego zapisywania i Przekierowanie zasady i przedstawia nowych lub przekierowany adres URL.</span><span class="sxs-lookup"><span data-stu-id="33c5f-150">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="33c5f-151">Kiedy należy używać pośredniczącym ponowne zapisywanie adresów URL</span><span class="sxs-lookup"><span data-stu-id="33c5f-151">When to use URL Rewriting Middleware</span></span>

<span data-ttu-id="33c5f-152">Użyj ponowne zapisywanie adresów URL w oprogramowaniu pośredniczącym, gdy nie można użyć [moduł ponowne zapisywanie adresów URL](https://www.iis.net/downloads/microsoft/url-rewrite) z usługami IIS w systemie Windows Server [modułu mod_rewrite Apache](https://httpd.apache.org/docs/2.4/rewrite/) na serwerze Apache [na NginxponownezapisywanieadresówURL](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), lub aplikacji znajduje się na [serwer HTTP.sys](xref:fundamentals/servers/httpsys) (wcześniej nazywanych [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="33c5f-152">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="33c5f-153">Główne powody do użycia na serwerze adresu URL przebudowywania technologii w usługach IIS, Apache lub Nginx są czy oprogramowanie pośredniczące nie obsługuje wszystkich funkcji tych modułów i wydajności oprogramowania pośredniczącego prawdopodobnie nie będzie zgodny z modułów.</span><span class="sxs-lookup"><span data-stu-id="33c5f-153">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="33c5f-154">Istnieją jednak niektóre funkcje modułów serwera, które nie działają z projektów platformy ASP.NET Core, takich jak `IsFile` i `IsDirectory` ograniczeń modułu ponownego zapisywania usług IIS.</span><span class="sxs-lookup"><span data-stu-id="33c5f-154">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="33c5f-155">W tych scenariuszach w zamian użyj oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="33c5f-155">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="33c5f-156">Package</span><span class="sxs-lookup"><span data-stu-id="33c5f-156">Package</span></span>

<span data-ttu-id="33c5f-157">Aby dołączyć oprogramowanie pośredniczące w projekcie, należy dodać odwołanie do [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="33c5f-157">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="33c5f-158">Ta funkcja jest dostępna dla aplikacji przeznaczonych dla platformy ASP.NET Core 1.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="33c5f-158">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="33c5f-159">Opcje i rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="33c5f-159">Extension and options</span></span>

<span data-ttu-id="33c5f-160">Ustanowić Twojej ponowne zapisywanie adresów URL i Przekierowanie reguł przez utworzenie wystąpienia `RewriteOptions` klasy z metody rozszerzenia dla poszczególnych reguł.</span><span class="sxs-lookup"><span data-stu-id="33c5f-160">Establish your URL rewrite and redirect rules by creating an instance of the `RewriteOptions` class with extension methods for each of your rules.</span></span> <span data-ttu-id="33c5f-161">Łańcucha wielu reguł w kolejności, że chcesz je przetworzyć.</span><span class="sxs-lookup"><span data-stu-id="33c5f-161">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="33c5f-162">`RewriteOptions` Są przekazywane do oprogramowania pośredniczącego ponowne zapisywanie adresów URL, który jest dodawany do potoku żądania z `app.UseRewriter(options);`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-162">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="33c5f-163">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="33c5f-163">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="33c5f-164">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="33c5f-164">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

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

---

### <a name="url-redirect"></a><span data-ttu-id="33c5f-165">Adres URL przekierowania</span><span class="sxs-lookup"><span data-stu-id="33c5f-165">URL redirect</span></span>

<span data-ttu-id="33c5f-166">Użyj `AddRedirect` Przekierowywanie żądań.</span><span class="sxs-lookup"><span data-stu-id="33c5f-166">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="33c5f-167">Pierwszy parametr zawiera z wyrażenia regularnego do dopasowania w ścieżce przychodzącego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33c5f-167">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="33c5f-168">Drugi parametr jest ciąg zastępczy.</span><span class="sxs-lookup"><span data-stu-id="33c5f-168">The second parameter is the replacement string.</span></span> <span data-ttu-id="33c5f-169">Trzeci parametr, jeśli jest obecny, określa kod stanu.</span><span class="sxs-lookup"><span data-stu-id="33c5f-169">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="33c5f-170">Jeśli kod stanu nie jest określony, domyślnie 302 (Found), który wskazuje, że zasób jest tymczasowo przeniesiony lub zastąpione.</span><span class="sxs-lookup"><span data-stu-id="33c5f-170">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="33c5f-171">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="33c5f-171">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="33c5f-172">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="33c5f-172">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="33c5f-173">W przeglądarce za pomocą narzędzia dla deweloperów włączone, Wyślij żądanie do przykładowej aplikacji ze ścieżką `/redirect-rule/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-173">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="33c5f-174">Wyrażenia regularnego zgodna ze ścieżką żądania na `redirect-rule/(.*)`, a ścieżka została zastąpiona `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-174">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="33c5f-175">Przekieruj adres URL jest wysyłany do klienta z kodem stanu 302 (Found).</span><span class="sxs-lookup"><span data-stu-id="33c5f-175">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="33c5f-176">Przeglądarka sprawia, że nowe żądanie pod adresem URL przekierowania, który jest wyświetlany na pasku adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="33c5f-176">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="33c5f-177">Ponieważ brak reguł w przykładowej aplikacji odpowiada na adres URL przekierowania, drugie żądanie odbiera odpowiedź 200 (OK) z aplikacji i treść odpowiedzi zawiera adres URL przekierowania.</span><span class="sxs-lookup"><span data-stu-id="33c5f-177">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="33c5f-178">Obie strony jest kierowane do serwera, gdy adres URL jest *przekierowanie*.</span><span class="sxs-lookup"><span data-stu-id="33c5f-178">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="33c5f-179">Należy zachować ostrożność podczas ustanawiania reguł przekierowania.</span><span class="sxs-lookup"><span data-stu-id="33c5f-179">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="33c5f-180">Reguły przekierowania są oceniane na każde żądanie do aplikacji, w tym nastąpiło przekierowanie.</span><span class="sxs-lookup"><span data-stu-id="33c5f-180">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="33c5f-181">Łatwo jest przypadkowo pętlę nieskończoną przekierowania.</span><span class="sxs-lookup"><span data-stu-id="33c5f-181">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="33c5f-182">Oryginalne żądanie: `/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="33c5f-182">Original Request: `/redirect-rule/1234/5678`</span></span>

![Okno przeglądarki z narzędzi deweloperskich śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="33c5f-184">Część wyrażenia umieszczone w nawiasach jest nazywana *grupy przechwytywania*.</span><span class="sxs-lookup"><span data-stu-id="33c5f-184">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="33c5f-185">Kropka (`.`) wyrażenia oznacza *dopasowuje dowolny znak*.</span><span class="sxs-lookup"><span data-stu-id="33c5f-185">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="33c5f-186">Gwiazdka (`*`) wskazuje *Dopasowuje poprzedni znak zero lub więcej razy*.</span><span class="sxs-lookup"><span data-stu-id="33c5f-186">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="33c5f-187">W związku z tym segmenty dwa ostatnie ścieżki adresu URL, `1234/5678`, są przechwytywane przez grupę przechwytywania `(.*)`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-187">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="33c5f-188">Wartości podane w adresie URL żądania po `redirect-rule/` są przechwytywane przez tę grupę pojedynczego przechwytywania.</span><span class="sxs-lookup"><span data-stu-id="33c5f-188">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="33c5f-189">W ciągu zastępowania przechwyconej grupy są wstrzykiwane do ciągu z znak dolara (`$`) wraz z numerem sekwencji przechwytywania.</span><span class="sxs-lookup"><span data-stu-id="33c5f-189">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="33c5f-190">Pierwsza wartość grupy przechwytywania są uzyskiwane z `$1`, druga z `$2`, i kontynuują w sekwencji dla grup przechwytywania w Twojej wyrażenia regularnego.</span><span class="sxs-lookup"><span data-stu-id="33c5f-190">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="33c5f-191">Tylko jedna grupa przechwyconych regex reguły przekierowania w jest przykładowej aplikacji, tak aby tylko jedna grupa wprowadzony w ciągu zastępowania, która jest `$1`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-191">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="33c5f-192">Reguła jest stosowana, adres URL staje się `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-192">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="33c5f-193">Adres URL przekierowania do bezpiecznego punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="33c5f-193">URL redirect to a secure endpoint</span></span>

<span data-ttu-id="33c5f-194">Użyj `AddRedirectToHttps` przekierowywania żądań HTTP do tego samego hosta i ścieżkę przy użyciu protokołu HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="33c5f-194">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="33c5f-195">Jeśli kod stanu nie jest podany, oprogramowanie pośredniczące domyślnie 302 (Found).</span><span class="sxs-lookup"><span data-stu-id="33c5f-195">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="33c5f-196">Jeśli port nie jest podany, oprogramowanie pośredniczące domyślnie `null`, co oznacza, że protokół zmienia się na `https://` i klient uzyskuje dostęp do zasobów na porcie 443.</span><span class="sxs-lookup"><span data-stu-id="33c5f-196">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="33c5f-197">W przykładzie przedstawiono sposób ustawić kod stanu 301 (trwale przeniesiona) i zmień numer portu na 5001.</span><span class="sxs-lookup"><span data-stu-id="33c5f-197">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="33c5f-198">Użyj `AddRedirectToHttpsPermanent` przekierowywania żądań niezabezpieczonych do tego samego hosta i ścieżki z bezpiecznego protokołu HTTPS (`https://` na porcie 443).</span><span class="sxs-lookup"><span data-stu-id="33c5f-198">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="33c5f-199">Oprogramowanie pośredniczące ustawia kod stanu 301 (trwale przeniesiona).</span><span class="sxs-lookup"><span data-stu-id="33c5f-199">The middleware sets the status code to 301 (Moved Permanently).</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> <span data-ttu-id="33c5f-200">Podczas przekierowywania HTTPS bez potrzeby przekierowania dodatkowe reguły, firma Microsoft zaleca używanie oprogramowania pośredniczącego przekierowania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="33c5f-200">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware.</span></span> <span data-ttu-id="33c5f-201">Aby uzyskać więcej informacji, zobacz [wymusić HTTPS](xref:security/enforcing-ssl#require-https) tematu.</span><span class="sxs-lookup"><span data-stu-id="33c5f-201">For more information, see the [Enforce HTTPS](xref:security/enforcing-ssl#require-https) topic.</span></span>

<span data-ttu-id="33c5f-202">Przykładowa aplikacja jest w stanie pokazuje sposób użycia `AddRedirectToHttps` lub `AddRedirectToHttpsPermanent`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-202">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="33c5f-203">Dodaj metodę rozszerzenie do `RewriteOptions`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-203">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="33c5f-204">Wprowadź niezabezpieczonego żądania do aplikacji na dowolny adres URL.</span><span class="sxs-lookup"><span data-stu-id="33c5f-204">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="33c5f-205">Odrzucić zabezpieczeń przeglądarki ostrzeżenie niezaufany certyfikat z podpisem własnym lub utworzyć wyjątek dotyczący ufać certyfikatowi.</span><span class="sxs-lookup"><span data-stu-id="33c5f-205">Dismiss the browser security warning that the self-signed certificate is untrusted or create an exception to trust the certificate.</span></span>

<span data-ttu-id="33c5f-206">Oryginalnego żądania przy użyciu `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="33c5f-206">Original Request using `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span></span>

![Okno przeglądarki z narzędzi deweloperskich śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="33c5f-208">Oryginalnego żądania przy użyciu `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="33c5f-208">Original Request using `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span></span>

![Okno przeglądarki z narzędzi deweloperskich śledzenia żądań i odpowiedzi](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="33c5f-210">Ponowne zapisywanie adresów URL</span><span class="sxs-lookup"><span data-stu-id="33c5f-210">URL rewrite</span></span>

<span data-ttu-id="33c5f-211">Użyj `AddRewrite` Aby utworzyć regułę dla ponowne zapisywanie adresów URL.</span><span class="sxs-lookup"><span data-stu-id="33c5f-211">Use `AddRewrite` to create a rule for rewriting URLs.</span></span> <span data-ttu-id="33c5f-212">Pierwszy parametr zawiera z wyrażenia regularnego do dopasowania na ścieżkę przychodzącego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33c5f-212">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="33c5f-213">Drugi parametr jest ciąg zastępczy.</span><span class="sxs-lookup"><span data-stu-id="33c5f-213">The second parameter is the replacement string.</span></span> <span data-ttu-id="33c5f-214">Trzeci parametr `skipRemainingRules: {true|false}`, wskazuje, aby oprogramowanie pośredniczące umożliwia określenie, czy pominąć reguły ponownego zapisywania dodatkowe, jeśli zastosowano bieżącej regule.</span><span class="sxs-lookup"><span data-stu-id="33c5f-214">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="33c5f-215">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="33c5f-215">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="33c5f-216">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="33c5f-216">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="33c5f-217">Oryginalne żądanie: `/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="33c5f-217">Original Request: `/rewrite-rule/1234/5678`</span></span>

![Okno przeglądarki z narzędzi deweloperskich śledzenia żądań i odpowiedzi](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="33c5f-219">Pierwszą rzeczą, którą można zauważyć, że w wyrażenia regularnego jest karatach (`^`) na początku wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="33c5f-219">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="33c5f-220">Oznacza to, że pasujące zaczyna się na początku ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33c5f-220">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="33c5f-221">W przypadku poprzedniego przykładu z regułą przekierowania `redirect-rule/(.*)`, na początku wyrażenia regularnego nie istnieją żadne karatach — w związku z tym może poprzedzać wszystkie znaki `redirect-rule/` w ścieżce dla pomyślnego dopasowania.</span><span class="sxs-lookup"><span data-stu-id="33c5f-221">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="33c5f-222">Ścieżka</span><span class="sxs-lookup"><span data-stu-id="33c5f-222">Path</span></span>                               | <span data-ttu-id="33c5f-223">Dopasowanie</span><span class="sxs-lookup"><span data-stu-id="33c5f-223">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="33c5f-224">Tak</span><span class="sxs-lookup"><span data-stu-id="33c5f-224">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="33c5f-225">Tak</span><span class="sxs-lookup"><span data-stu-id="33c5f-225">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="33c5f-226">Tak</span><span class="sxs-lookup"><span data-stu-id="33c5f-226">Yes</span></span>   |

<span data-ttu-id="33c5f-227">Reguła ponownego zapisywania `^rewrite-rule/(\d+)/(\d+)`, ścieżki zgodny tylko, jeśli zaczynają `rewrite-rule/`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-227">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="33c5f-228">Zwróć uwagę, różnica dopasowywania między poniżej reguły ponownego zapisywania i zasadę przekierowania powyżej.</span><span class="sxs-lookup"><span data-stu-id="33c5f-228">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="33c5f-229">Ścieżka</span><span class="sxs-lookup"><span data-stu-id="33c5f-229">Path</span></span>                              | <span data-ttu-id="33c5f-230">Dopasowanie</span><span class="sxs-lookup"><span data-stu-id="33c5f-230">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="33c5f-231">Tak</span><span class="sxs-lookup"><span data-stu-id="33c5f-231">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="33c5f-232">Nie</span><span class="sxs-lookup"><span data-stu-id="33c5f-232">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="33c5f-233">Nie</span><span class="sxs-lookup"><span data-stu-id="33c5f-233">No</span></span>    |

<span data-ttu-id="33c5f-234">Po `^rewrite-rule/` część wyrażenia, istnieją dwie grupy przechwytywania, `(\d+)/(\d+)`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-234">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="33c5f-235">`\d` Oznacza *odpowiada cyfrę (numer)*.</span><span class="sxs-lookup"><span data-stu-id="33c5f-235">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="33c5f-236">Znak plus (`+`) oznacza *zgodne z co najmniej jednego znaku poprzedzającego*.</span><span class="sxs-lookup"><span data-stu-id="33c5f-236">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="33c5f-237">W związku z tym adres URL musi zawierać liczbę następuje następuje inny numer kreskami ukośnymi.</span><span class="sxs-lookup"><span data-stu-id="33c5f-237">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="33c5f-238">Te przechwytywania grup są wstrzykiwane do nowych adresu URL jako `$1` i `$2`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-238">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="33c5f-239">Ciąg zastępczy reguły ponownego zapisywania umieszczenie grup przechwyconych w ciąg zapytania.</span><span class="sxs-lookup"><span data-stu-id="33c5f-239">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="33c5f-240">Żądana ścieżka `/rewrite-rule/1234/5678` jest napisany od nowa można uzyskać zasobu pod adresem `/rewritten?var1=1234&var2=5678`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-240">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="33c5f-241">Jeśli ciąg zapytania jest obecna na oryginalne żądanie, jest to zachowane, gdy adres URL jest napisany od nowa.</span><span class="sxs-lookup"><span data-stu-id="33c5f-241">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="33c5f-242">Nie ma żadnych obie strony do serwera w celu uzyskania zasobu.</span><span class="sxs-lookup"><span data-stu-id="33c5f-242">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="33c5f-243">Jeśli zasób istnieje, ma pobrane i zwracany do klienta z kodem 200 stanu (OK).</span><span class="sxs-lookup"><span data-stu-id="33c5f-243">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="33c5f-244">Ponieważ klient nie jest przekierowany, nie powoduje zmiany adresu URL na pasku adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="33c5f-244">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="33c5f-245">Ile dotyczy to klient, wystąpił nigdy nie operacji ponowne zapisywanie adresów URL.</span><span class="sxs-lookup"><span data-stu-id="33c5f-245">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="33c5f-246">Użyj `skipRemainingRules: true` zawsze, gdy jest to możliwe, ponieważ reguł dopasowywania jest procesem kosztowne i skraca czas odpowiedzi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33c5f-246">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="33c5f-247">Najszybszym odpowiedzi aplikacji:</span><span class="sxs-lookup"><span data-stu-id="33c5f-247">For the fastest app response:</span></span>
> * <span data-ttu-id="33c5f-248">Kolejność reguły ponownego zapisywania z reguły najczęściej dopasowane do najmniej często dopasowane reguły.</span><span class="sxs-lookup"><span data-stu-id="33c5f-248">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="33c5f-249">Pomiń przetwarzanie pozostałych reguł, gdy pasują do i nie dodatkowe reguły przetwarzania jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="33c5f-249">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="33c5f-250">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="33c5f-250">Apache mod_rewrite</span></span>

<span data-ttu-id="33c5f-251">Zastosuj reguły mod_rewrite Apache z `AddApacheModRewrite`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-251">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="33c5f-252">Upewnij się, że plik reguł jest wdrażany z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="33c5f-252">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="33c5f-253">Aby uzyskać więcej informacji i przykłady reguł mod_rewrite, zobacz [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span><span class="sxs-lookup"><span data-stu-id="33c5f-253">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="33c5f-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="33c5f-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="33c5f-255">A `StreamReader` jest używany do odczytu reguły z *ApacheModRewrite.txt* pliku reguł.</span><span class="sxs-lookup"><span data-stu-id="33c5f-255">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="33c5f-256">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="33c5f-256">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="33c5f-257">Pierwszy parametr przyjmuje `IFileProvider`, które zostaną przekazane za pośrednictwem [iniekcji zależności](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="33c5f-257">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="33c5f-258">`IHostingEnvironment` Jest wprowadzonym w celu zapewnienia `ContentRootFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-258">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="33c5f-259">Drugi parametr jest ścieżka do pliku reguł, który jest *ApacheModRewrite.txt* w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33c5f-259">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="33c5f-260">Przykładowa aplikacja przekierowuje żądania z `/apache-mod-rules-redirect/(.\*)` do `/redirected?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-260">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="33c5f-261">Kod stanu odpowiedzi jest 302 (Found).</span><span class="sxs-lookup"><span data-stu-id="33c5f-261">The response status code is 302 (Found).</span></span>

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

<span data-ttu-id="33c5f-262">Oryginalne żądanie: `/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="33c5f-262">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![Okno przeglądarki z narzędzi deweloperskich śledzenia żądań i odpowiedzi](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a><span data-ttu-id="33c5f-264">Zmienne serwera obsługiwanych</span><span class="sxs-lookup"><span data-stu-id="33c5f-264">Supported server variables</span></span>

<span data-ttu-id="33c5f-265">Oprogramowanie pośredniczące obsługuje następujące zmienne serwera Apache mod_rewrite:</span><span class="sxs-lookup"><span data-stu-id="33c5f-265">The middleware supports the following Apache mod_rewrite server variables:</span></span>

* <span data-ttu-id="33c5f-266">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="33c5f-266">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="33c5f-267">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="33c5f-267">HTTP_ACCEPT</span></span>
* <span data-ttu-id="33c5f-268">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="33c5f-268">HTTP_CONNECTION</span></span>
* <span data-ttu-id="33c5f-269">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="33c5f-269">HTTP_COOKIE</span></span>
* <span data-ttu-id="33c5f-270">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="33c5f-270">HTTP_FORWARDED</span></span>
* <span data-ttu-id="33c5f-271">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="33c5f-271">HTTP_HOST</span></span>
* <span data-ttu-id="33c5f-272">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="33c5f-272">HTTP_REFERER</span></span>
* <span data-ttu-id="33c5f-273">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="33c5f-273">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="33c5f-274">HTTPS</span><span class="sxs-lookup"><span data-stu-id="33c5f-274">HTTPS</span></span>
* <span data-ttu-id="33c5f-275">PROTOKÓŁ IPV6</span><span class="sxs-lookup"><span data-stu-id="33c5f-275">IPV6</span></span>
* <span data-ttu-id="33c5f-276">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="33c5f-276">QUERY_STRING</span></span>
* <span data-ttu-id="33c5f-277">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="33c5f-277">REMOTE_ADDR</span></span>
* <span data-ttu-id="33c5f-278">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="33c5f-278">REMOTE_PORT</span></span>
* <span data-ttu-id="33c5f-279">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="33c5f-279">REQUEST_FILENAME</span></span>
* <span data-ttu-id="33c5f-280">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="33c5f-280">REQUEST_METHOD</span></span>
* <span data-ttu-id="33c5f-281">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="33c5f-281">REQUEST_SCHEME</span></span>
* <span data-ttu-id="33c5f-282">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="33c5f-282">REQUEST_URI</span></span>
* <span data-ttu-id="33c5f-283">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="33c5f-283">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="33c5f-284">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="33c5f-284">SERVER_ADDR</span></span>
* <span data-ttu-id="33c5f-285">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="33c5f-285">SERVER_PORT</span></span>
* <span data-ttu-id="33c5f-286">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="33c5f-286">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="33c5f-287">CZAS</span><span class="sxs-lookup"><span data-stu-id="33c5f-287">TIME</span></span>
* <span data-ttu-id="33c5f-288">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="33c5f-288">TIME_DAY</span></span>
* <span data-ttu-id="33c5f-289">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="33c5f-289">TIME_HOUR</span></span>
* <span data-ttu-id="33c5f-290">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="33c5f-290">TIME_MIN</span></span>
* <span data-ttu-id="33c5f-291">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="33c5f-291">TIME_MON</span></span>
* <span data-ttu-id="33c5f-292">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="33c5f-292">TIME_SEC</span></span>
* <span data-ttu-id="33c5f-293">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="33c5f-293">TIME_WDAY</span></span>
* <span data-ttu-id="33c5f-294">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="33c5f-294">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="33c5f-295">Reguły moduł ponowne zapisywanie adresów URL usług IIS</span><span class="sxs-lookup"><span data-stu-id="33c5f-295">IIS URL Rewrite Module rules</span></span>

<span data-ttu-id="33c5f-296">Aby użyć reguł, które dotyczą moduł ponowne zapisywanie adresów URL usług IIS, użyj `AddIISUrlRewrite`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-296">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="33c5f-297">Upewnij się, że plik reguł jest wdrażany z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="33c5f-297">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="33c5f-298">Nie bezpośrednie oprogramowaniu pośredniczącym, aby korzystać z *web.config* pliku podczas uruchamiania w systemie Windows Server IIS.</span><span class="sxs-lookup"><span data-stu-id="33c5f-298">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="33c5f-299">Z programem IIS, zasady te powinny być przechowywane poza Twojej *web.config* w celu uniknięcia konfliktów z modułem ponownego zapisywania usług IIS.</span><span class="sxs-lookup"><span data-stu-id="33c5f-299">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="33c5f-300">Aby uzyskać więcej informacji i przykłady reguł moduł ponowne zapisywanie adresów URL usług IIS, zobacz [przy użyciu adresu Url przepisywania moduł 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) i [odwołania konfiguracji modułu ponowne zapisywanie adresów URL](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span><span class="sxs-lookup"><span data-stu-id="33c5f-300">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="33c5f-301">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="33c5f-301">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="33c5f-302">A `StreamReader` jest używany do odczytu reguły z *IISUrlRewrite.xml* pliku reguł.</span><span class="sxs-lookup"><span data-stu-id="33c5f-302">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="33c5f-303">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="33c5f-303">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="33c5f-304">Pierwszy parametr przyjmuje `IFileProvider`, a drugi parametr jest ścieżka do pliku reguł XML, który jest *IISUrlRewrite.xml* w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33c5f-304">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="33c5f-305">Przykładowa aplikacja ponownie zapisuje żądań z `/iis-rules-rewrite/(.*)` do `/rewritten?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-305">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="33c5f-306">Odpowiedź jest wysyłana do klienta z kodem stanu 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="33c5f-306">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

<span data-ttu-id="33c5f-307">Oryginalne żądanie: `/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="33c5f-307">Original Request: `/iis-rules-rewrite/1234`</span></span>

![Okno przeglądarki z narzędzi deweloperskich śledzenia żądań i odpowiedzi](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="33c5f-309">Jeśli masz aktywnego modułu ponownego zapisywania usług IIS skonfigurowanych reguł poziomu serwera, które będzie mieć wpływ aplikacji w sposób niepożądanych można wyłączyć moduł ponowne zapisywanie adresów usług IIS dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33c5f-309">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="33c5f-310">Aby uzyskać więcej informacji, zobacz [moduły IIS wyłączenie](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="33c5f-310">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="33c5f-311">Nieobsługiwane funkcje</span><span class="sxs-lookup"><span data-stu-id="33c5f-311">Unsupported features</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="33c5f-312">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="33c5f-312">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="33c5f-313">Oprogramowanie pośredniczące zwolnione z platformy ASP.NET Core 2.x nie obsługuje następujące funkcje moduł ponowne zapisywanie adresów URL usług IIS:</span><span class="sxs-lookup"><span data-stu-id="33c5f-313">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="33c5f-314">Reguły ruchu wychodzącego</span><span class="sxs-lookup"><span data-stu-id="33c5f-314">Outbound Rules</span></span>
* <span data-ttu-id="33c5f-315">Zmienne serwera niestandardowego</span><span class="sxs-lookup"><span data-stu-id="33c5f-315">Custom Server Variables</span></span>
* <span data-ttu-id="33c5f-316">Symbole wieloznaczne</span><span class="sxs-lookup"><span data-stu-id="33c5f-316">Wildcards</span></span>
* <span data-ttu-id="33c5f-317">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="33c5f-317">LogRewrittenUrl</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="33c5f-318">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="33c5f-318">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="33c5f-319">Oprogramowanie pośredniczące zwolnione z platformy ASP.NET Core 1.x nie obsługuje następujące funkcje moduł ponowne zapisywanie adresów URL usług IIS:</span><span class="sxs-lookup"><span data-stu-id="33c5f-319">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="33c5f-320">Globalne zasady</span><span class="sxs-lookup"><span data-stu-id="33c5f-320">Global Rules</span></span>
* <span data-ttu-id="33c5f-321">Reguły ruchu wychodzącego</span><span class="sxs-lookup"><span data-stu-id="33c5f-321">Outbound Rules</span></span>
* <span data-ttu-id="33c5f-322">Ponownego zapisywania map</span><span class="sxs-lookup"><span data-stu-id="33c5f-322">Rewrite Maps</span></span>
* <span data-ttu-id="33c5f-323">Akcja CustomResponse</span><span class="sxs-lookup"><span data-stu-id="33c5f-323">CustomResponse Action</span></span>
* <span data-ttu-id="33c5f-324">Zmienne serwera niestandardowego</span><span class="sxs-lookup"><span data-stu-id="33c5f-324">Custom Server Variables</span></span>
* <span data-ttu-id="33c5f-325">Symbole wieloznaczne</span><span class="sxs-lookup"><span data-stu-id="33c5f-325">Wildcards</span></span>
* <span data-ttu-id="33c5f-326">Akcja: CustomResponse</span><span class="sxs-lookup"><span data-stu-id="33c5f-326">Action:CustomResponse</span></span>
* <span data-ttu-id="33c5f-327">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="33c5f-327">LogRewrittenUrl</span></span>

---

#### <a name="supported-server-variables"></a><span data-ttu-id="33c5f-328">Zmienne serwera obsługiwanych</span><span class="sxs-lookup"><span data-stu-id="33c5f-328">Supported server variables</span></span>

<span data-ttu-id="33c5f-329">Oprogramowanie pośredniczące obsługuje następujące zmienne serwera moduł ponowne zapisywanie adresów URL usług IIS:</span><span class="sxs-lookup"><span data-stu-id="33c5f-329">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>

* <span data-ttu-id="33c5f-330">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="33c5f-330">CONTENT_LENGTH</span></span>
* <span data-ttu-id="33c5f-331">TYP_ZAWARTOŚCI</span><span class="sxs-lookup"><span data-stu-id="33c5f-331">CONTENT_TYPE</span></span>
* <span data-ttu-id="33c5f-332">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="33c5f-332">HTTP_ACCEPT</span></span>
* <span data-ttu-id="33c5f-333">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="33c5f-333">HTTP_CONNECTION</span></span>
* <span data-ttu-id="33c5f-334">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="33c5f-334">HTTP_COOKIE</span></span>
* <span data-ttu-id="33c5f-335">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="33c5f-335">HTTP_HOST</span></span>
* <span data-ttu-id="33c5f-336">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="33c5f-336">HTTP_REFERER</span></span>
* <span data-ttu-id="33c5f-337">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="33c5f-337">HTTP_URL</span></span>
* <span data-ttu-id="33c5f-338">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="33c5f-338">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="33c5f-339">HTTPS</span><span class="sxs-lookup"><span data-stu-id="33c5f-339">HTTPS</span></span>
* <span data-ttu-id="33c5f-340">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="33c5f-340">LOCAL_ADDR</span></span>
* <span data-ttu-id="33c5f-341">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="33c5f-341">QUERY_STRING</span></span>
* <span data-ttu-id="33c5f-342">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="33c5f-342">REMOTE_ADDR</span></span>
* <span data-ttu-id="33c5f-343">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="33c5f-343">REMOTE_PORT</span></span>
* <span data-ttu-id="33c5f-344">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="33c5f-344">REQUEST_FILENAME</span></span>
* <span data-ttu-id="33c5f-345">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="33c5f-345">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="33c5f-346">Możesz również uzyskać `IFileProvider` za pośrednictwem `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-346">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="33c5f-347">Takie podejście może udostępnić większą elastyczność lokalizacji Twojej ponownego zapisywania plików reguł.</span><span class="sxs-lookup"><span data-stu-id="33c5f-347">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="33c5f-348">Upewnij się, że reguły ponownego zapisywania plików są wdrażane do serwera w ścieżce podane przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="33c5f-348">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="33c5f-349">Reguły na podstawie — metoda</span><span class="sxs-lookup"><span data-stu-id="33c5f-349">Method-based rule</span></span>

<span data-ttu-id="33c5f-350">Użyj `Add(Action<RewriteContext> applyRule)` implementacji logiki reguły w metodzie.</span><span class="sxs-lookup"><span data-stu-id="33c5f-350">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="33c5f-351">`RewriteContext` Przedstawia `HttpContext` do użycia w metodę.</span><span class="sxs-lookup"><span data-stu-id="33c5f-351">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="33c5f-352">`context.Result` Określa, jak dodatkowe potoku przetwarzania jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="33c5f-352">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="33c5f-353">context.Result</span><span class="sxs-lookup"><span data-stu-id="33c5f-353">context.Result</span></span>                       | <span data-ttu-id="33c5f-354">Akcja</span><span class="sxs-lookup"><span data-stu-id="33c5f-354">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="33c5f-355">`RuleResult.ContinueRules` (ustawienie domyślne)</span><span class="sxs-lookup"><span data-stu-id="33c5f-355">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="33c5f-356">Kontynuować stosowanie reguł</span><span class="sxs-lookup"><span data-stu-id="33c5f-356">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="33c5f-357">Zaprzestanie stosowania reguły i wysyłania odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="33c5f-357">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="33c5f-358">Zaprzestanie stosowania reguły i wysyłać kontekstu do następnego oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="33c5f-358">Stop applying rules and send the context to the next middleware</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="33c5f-359">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="33c5f-359">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="33c5f-360">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="33c5f-360">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="33c5f-361">Przykładowa aplikacja przedstawiono metodę, która przekierowuje żądania dla ścieżek czy kończą się *.xml*.</span><span class="sxs-lookup"><span data-stu-id="33c5f-361">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="33c5f-362">Jeśli żądanie `/file.xml`, nastąpi przekierowanie do `/xmlfiles/file.xml`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-362">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="33c5f-363">Kod stanu jest ustawiona na 301 (trwale przeniesiona).</span><span class="sxs-lookup"><span data-stu-id="33c5f-363">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="33c5f-364">Dla przekierowania Musisz jawnie ustawić kod stanu odpowiedzi; w przeciwnym razie zwracany jest kod stanu 200 (OK) i Przekierowanie nie występują na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="33c5f-364">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

<span data-ttu-id="33c5f-365">Oryginalne żądanie: `/file.xml`</span><span class="sxs-lookup"><span data-stu-id="33c5f-365">Original Request: `/file.xml`</span></span>

![Okno przeglądarki z śledzenia żądań i odpowiedzi dla file.xml narzędzia dla deweloperów](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="33c5f-367">Reguły na podstawie IRule</span><span class="sxs-lookup"><span data-stu-id="33c5f-367">IRule-based rule</span></span>

<span data-ttu-id="33c5f-368">Użyj `Add(IRule)` implementacji logiki reguły w klasie, która jest pochodną `IRule`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-368">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="33c5f-369">Przy użyciu `IRule` zapewnia większą elastyczność w przypadku metody oparte na metodzie reguły.</span><span class="sxs-lookup"><span data-stu-id="33c5f-369">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="33c5f-370">Klasy pochodne mogą obejmować konstruktora, w którym można przekazać w parametrach `ApplyRule` metody.</span><span class="sxs-lookup"><span data-stu-id="33c5f-370">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="33c5f-371">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="33c5f-371">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="33c5f-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="33c5f-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="33c5f-373">Wartości parametrów w Przykładowa aplikacja dla `extension` i `newPath` są sprawdzane w celu spełnienia kilku warunków.</span><span class="sxs-lookup"><span data-stu-id="33c5f-373">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="33c5f-374">`extension` Musi zawierać wartość, a wartość musi być *.png*, *.jpg*, lub *.gif*.</span><span class="sxs-lookup"><span data-stu-id="33c5f-374">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="33c5f-375">Jeśli `newPath` nie jest prawidłowy, `ArgumentException` jest generowany.</span><span class="sxs-lookup"><span data-stu-id="33c5f-375">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="33c5f-376">Jeśli żądanie *image.png*, nastąpi przekierowanie do `/png-images/image.png`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-376">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="33c5f-377">Jeśli żądanie *image.jpg*, nastąpi przekierowanie do `/jpg-images/image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="33c5f-377">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="33c5f-378">Kod stanu ma ustawioną wartość 301 (przenieść trwale) i `context.Result` ustawiono Zatrzymaj przetwarzanie reguł i wysyłania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="33c5f-378">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

<span data-ttu-id="33c5f-379">Oryginalne żądanie: `/image.png`</span><span class="sxs-lookup"><span data-stu-id="33c5f-379">Original Request: `/image.png`</span></span>

![Okno przeglądarki z śledzenia żądań i odpowiedzi dla image.png narzędzia dla deweloperów](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="33c5f-381">Oryginalne żądanie: `/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="33c5f-381">Original Request: `/image.jpg`</span></span>

![Okno przeglądarki z śledzenia żądań i odpowiedzi dla image.jpg narzędzia dla deweloperów](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="33c5f-383">Przykłady wyrażeń regularnych</span><span class="sxs-lookup"><span data-stu-id="33c5f-383">Regex examples</span></span>

| <span data-ttu-id="33c5f-384">Cel</span><span class="sxs-lookup"><span data-stu-id="33c5f-384">Goal</span></span> | <span data-ttu-id="33c5f-385">Ciąg wyrażenia regularnego &</span><span class="sxs-lookup"><span data-stu-id="33c5f-385">Regex String &</span></span><br><span data-ttu-id="33c5f-386">Przykład dopasowania</span><span class="sxs-lookup"><span data-stu-id="33c5f-386">Match Example</span></span> | <span data-ttu-id="33c5f-387">Ciąg zastępczy &</span><span class="sxs-lookup"><span data-stu-id="33c5f-387">Replacement String &</span></span><br><span data-ttu-id="33c5f-388">Przykład danych wyjściowych</span><span class="sxs-lookup"><span data-stu-id="33c5f-388">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="33c5f-389">Ścieżka Napisz ponownie w ciągu kwerendy</span><span class="sxs-lookup"><span data-stu-id="33c5f-389">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="33c5f-390">Usuwanie ukośnika</span><span class="sxs-lookup"><span data-stu-id="33c5f-390">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="33c5f-391">Wymuszanie ukośnika</span><span class="sxs-lookup"><span data-stu-id="33c5f-391">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="33c5f-392">Unikaj ponowne zapisywanie określone żądania</span><span class="sxs-lookup"><span data-stu-id="33c5f-392">Avoid rewriting specific requests</span></span> | <span data-ttu-id="33c5f-393">`^(.*)(?<!\.axd)$` lub `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="33c5f-393">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="33c5f-394">Tak: `/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="33c5f-394">Yes: `/resource.htm`</span></span><br><span data-ttu-id="33c5f-395">Nie: `/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="33c5f-395">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="33c5f-396">Rozmieszczanie segmenty adresu URL</span><span class="sxs-lookup"><span data-stu-id="33c5f-396">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="33c5f-397">Zastąp segment adresu URL</span><span class="sxs-lookup"><span data-stu-id="33c5f-397">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="33c5f-398">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="33c5f-398">Additional resources</span></span>

* [<span data-ttu-id="33c5f-399">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="33c5f-399">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="33c5f-400">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="33c5f-400">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="33c5f-401">Wyrażenia regularne w .NET</span><span class="sxs-lookup"><span data-stu-id="33c5f-401">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="33c5f-402">Język wyrażeń regularnych — podręczny wykaz</span><span class="sxs-lookup"><span data-stu-id="33c5f-402">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="33c5f-403">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="33c5f-403">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="33c5f-404">Za pomocą modułu ponowne zapisywanie adresów Url 2.0 (dla usług IIS)</span><span class="sxs-lookup"><span data-stu-id="33c5f-404">Using Url Rewrite Module 2.0 (for IIS)</span></span>](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="33c5f-405">Odwołania do konfiguracji modułu ponowne zapisywanie adresów URL</span><span class="sxs-lookup"><span data-stu-id="33c5f-405">URL Rewrite Module Configuration Reference</span></span>](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="33c5f-406">Forum moduł ponowne zapisywanie adresów URL usług IIS</span><span class="sxs-lookup"><span data-stu-id="33c5f-406">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="33c5f-407">Zachowaj prostą strukturę adresu URL</span><span class="sxs-lookup"><span data-stu-id="33c5f-407">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="33c5f-408">Ponowne zapisywanie adresów URL 10 porady i wskazówki</span><span class="sxs-lookup"><span data-stu-id="33c5f-408">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="33c5f-409">Ukośnika lub ukośnika</span><span class="sxs-lookup"><span data-stu-id="33c5f-409">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
