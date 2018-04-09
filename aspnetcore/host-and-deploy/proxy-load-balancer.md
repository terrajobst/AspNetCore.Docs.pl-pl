---
title: Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia
author: guardrex
description: Informacje o konfiguracji dla aplikacji hostowanych serwerów proxy i równoważenia obciążenia, które często przesłaniać żądanie ważnych informacji.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: b153a7406ae1b31a2aa453135c6bd0e5ce0b2997
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/30/2018
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a><span data-ttu-id="d38bf-103">Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="d38bf-103">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>

<span data-ttu-id="d38bf-104">Przez [Luke Latham](https://github.com/guardrex) i [Roaming Krzysztof](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="d38bf-104">By [Luke Latham](https://github.com/guardrex) and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="d38bf-105">W zalecanej konfiguracji dla platformy ASP.NET Core aplikacja jest hostowana przy użyciu modułu Core IIS/ASP.NET, Nginx lub Apache.</span><span class="sxs-lookup"><span data-stu-id="d38bf-105">In the recommended configuration for ASP.NET Core, the app is hosted using IIS/ASP.NET Core Module, Nginx, or Apache.</span></span> <span data-ttu-id="d38bf-106">Serwery proxy, usługi równoważenia obciążenia i innych urządzeniach sieciowych często przesłaniać informacje o żądaniu przed jego aplikacji:</span><span class="sxs-lookup"><span data-stu-id="d38bf-106">Proxy servers, load balancers, and other network appliances often obscure information about the request before it reaches the app:</span></span>

* <span data-ttu-id="d38bf-107">Gdy żądań HTTPS są przekazywane przez serwer proxy za pośrednictwem protokołu HTTP, oryginalny schemat (HTTPS) zostaną utracone i muszą być przekazywane w nagłówku.</span><span class="sxs-lookup"><span data-stu-id="d38bf-107">When HTTPS requests are proxied over HTTP, the original scheme (HTTPS) is lost and must be forwarded in a header.</span></span>
* <span data-ttu-id="d38bf-108">Ponieważ aplikacja odbiera żądanie z serwera proxy i jego wartość true źródła w Internecie lub sieci firmowej, źródłowy adres IP klienta, również muszą być przekazywane w nagłówku.</span><span class="sxs-lookup"><span data-stu-id="d38bf-108">Because an app receives a request from the proxy and not its true source on the Internet or corporate network, the originating client IP address must also be forwarded in a header.</span></span>

<span data-ttu-id="d38bf-109">Te informacje mogą być ważne przetwarzania żądania, na przykład w przekierowania uwierzytelniania, generowania łącza, oceny zasad i geoloation klienta.</span><span class="sxs-lookup"><span data-stu-id="d38bf-109">This information may be important in request processing, for example in redirects, authentication, link generation, policy evaluation, and client geoloation.</span></span>

## <a name="forwarded-headers"></a><span data-ttu-id="d38bf-110">Nagłówki przekazany dalej</span><span class="sxs-lookup"><span data-stu-id="d38bf-110">Forwarded headers</span></span>

<span data-ttu-id="d38bf-111">Konwencja serwery proxy przesyła informacje w nagłówkach HTTP.</span><span class="sxs-lookup"><span data-stu-id="d38bf-111">By convention, proxies forward information in HTTP headers.</span></span>

| <span data-ttu-id="d38bf-112">nagłówek</span><span class="sxs-lookup"><span data-stu-id="d38bf-112">Header</span></span> | <span data-ttu-id="d38bf-113">Opis</span><span class="sxs-lookup"><span data-stu-id="d38bf-113">Description</span></span> |
| ------ | ----------- |
| <span data-ttu-id="d38bf-114">X-Forwarded-For</span><span class="sxs-lookup"><span data-stu-id="d38bf-114">X-Forwarded-For</span></span> | <span data-ttu-id="d38bf-115">Przechowuje informacje dotyczące klienta, który zainicjował żądanie i kolejne serwery proxy w łańcuchu serwerów proxy.</span><span class="sxs-lookup"><span data-stu-id="d38bf-115">Holds information about the client that initiated the request and subsequent proxies in a chain of proxies.</span></span> <span data-ttu-id="d38bf-116">Ten parametr może zawierać adresów IP adresy (i, opcjonalnie, numery portów).</span><span class="sxs-lookup"><span data-stu-id="d38bf-116">This parameter may contain IP addresses (and, optionally, port numbers).</span></span> <span data-ttu-id="d38bf-117">Łańcuch serwerów proxy pierwszy parametr wskazuje klienta, w którym najpierw żądanie.</span><span class="sxs-lookup"><span data-stu-id="d38bf-117">In a chain of proxy servers, the first parameter indicates the client where the request was first made.</span></span> <span data-ttu-id="d38bf-118">Wykonaj kolejne proxy identyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="d38bf-118">Subsequent proxy identifiers follow.</span></span> <span data-ttu-id="d38bf-119">Ostatni serwer proxy w łańcuchu nie ma na liście parametrów.</span><span class="sxs-lookup"><span data-stu-id="d38bf-119">The last proxy in the chain isn't in the list of parameters.</span></span> <span data-ttu-id="d38bf-120">Adres IP ostatniego serwera proxy i opcjonalnie numeru portu są dostępne jako zdalny adres IP w przypadku warstwy transportu.</span><span class="sxs-lookup"><span data-stu-id="d38bf-120">The last proxy's IP address, and optionally a port number, are available as the remote IP address at the transport layer.</span></span> |
| <span data-ttu-id="d38bf-121">X-Forwarded-Proto</span><span class="sxs-lookup"><span data-stu-id="d38bf-121">X-Forwarded-Proto</span></span> | <span data-ttu-id="d38bf-122">Wartość źródłowego schemat (HTTP/HTTPS).</span><span class="sxs-lookup"><span data-stu-id="d38bf-122">The value of the originating scheme (HTTP/HTTPS).</span></span> <span data-ttu-id="d38bf-123">Wartość może być również listy programów, jeśli żądanie ma przechodzić wielu serwerów proxy.</span><span class="sxs-lookup"><span data-stu-id="d38bf-123">The value may also be a list of schemes if the request has traversed multiple proxies.</span></span> |
| <span data-ttu-id="d38bf-124">X-Forwarded-Host</span><span class="sxs-lookup"><span data-stu-id="d38bf-124">X-Forwarded-Host</span></span> | <span data-ttu-id="d38bf-125">Oryginalnej wartości pola nagłówka hosta.</span><span class="sxs-lookup"><span data-stu-id="d38bf-125">The original value of the Host header field.</span></span> <span data-ttu-id="d38bf-126">Zwykle nie należy modyfikować proxy nagłówka hosta.</span><span class="sxs-lookup"><span data-stu-id="d38bf-126">Usually, proxies don't modify the Host header.</span></span> <span data-ttu-id="d38bf-127">Zobacz [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) uzyskać informacji dotyczących usterki "podniesienia uprawnień", która dotyczy systemów, gdy serwer proxy nie sprawdzania poprawności lub nagłówków hosta restict znanej dobrej wartości.</span><span class="sxs-lookup"><span data-stu-id="d38bf-127">See [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) for information on an elevation-of-privileges vulnerability that affects systems where the proxy doesn't validate or restict Host headers to known good values.</span></span> |

<span data-ttu-id="d38bf-128">Przekazywane nagłówki oprogramowanie pośredniczące, z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pakietu, odczytuje te nagłówki i wypełnia skojarzonymi polami na [element HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="d38bf-128">The Forwarded Headers Middleware, from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package, reads these headers and fills in the associated fields on [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span> 

<span data-ttu-id="d38bf-129">Aktualizacje oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="d38bf-129">The middleware updates:</span></span>

* <span data-ttu-id="d38bf-130">[HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; ustawić za pomocą `X-Forwarded-For` wartość nagłówka.</span><span class="sxs-lookup"><span data-stu-id="d38bf-130">[HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; Set using the `X-Forwarded-For` header value.</span></span> <span data-ttu-id="d38bf-131">Dodatkowe ustawienia wpływu, jak oprogramowanie pośredniczące ustawia `RemoteIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="d38bf-131">Additional settings influence how the middleware sets `RemoteIpAddress`.</span></span> <span data-ttu-id="d38bf-132">Aby uzyskać więcej informacji, zobacz [opcje przekazane oprogramowania pośredniczącego nagłówki](#forwarded-headers-middleware-options).</span><span class="sxs-lookup"><span data-stu-id="d38bf-132">For details, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>
* <span data-ttu-id="d38bf-133">[HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; ustawić za pomocą `X-Forwarded-Proto` wartość nagłówka.</span><span class="sxs-lookup"><span data-stu-id="d38bf-133">[HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; Set using the `X-Forwarded-Proto` header value.</span></span>
* <span data-ttu-id="d38bf-134">[HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; ustawić za pomocą `X-Forwarded-Host` wartość nagłówka.</span><span class="sxs-lookup"><span data-stu-id="d38bf-134">[HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; Set using the `X-Forwarded-Host` header value.</span></span>

<span data-ttu-id="d38bf-135">Należy pamiętać, że nie wszystkie urządzenia sieciowe dodać `X-Forwarded-For` i `X-Forwarded-Proto` nagłówki bez dodatkowej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="d38bf-135">Note that not all network appliances add the `X-Forwarded-For` and `X-Forwarded-Proto` headers without additional configuration.</span></span> <span data-ttu-id="d38bf-136">Jeśli żądań kierowanych na serwer proxy nie zawierają tych nagłówków, po upływie aplikacji, zapoznaj się wskazówek producenta urządzenia.</span><span class="sxs-lookup"><span data-stu-id="d38bf-136">Consult your appliance manufacturer's guidance if the proxied requests don't contain these headers when they reach the app.</span></span>

<span data-ttu-id="d38bf-137">Nagłówki oprogramowanie pośredniczące przekazane [ustawienia domyślne](#forwarded-headers-middleware-options) można skonfigurować.</span><span class="sxs-lookup"><span data-stu-id="d38bf-137">Forwarded Headers Middleware [default settings](#forwarded-headers-middleware-options) can be configured.</span></span> <span data-ttu-id="d38bf-138">Ustawienia domyślne są:</span><span class="sxs-lookup"><span data-stu-id="d38bf-138">The default settings are:</span></span>

* <span data-ttu-id="d38bf-139">Istnieje tylko *jeden serwer proxy* między źródła żądania i aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d38bf-139">There is only *one proxy* between the app and the source of the requests.</span></span>
* <span data-ttu-id="d38bf-140">Tylko adresy sprzężenia zwrotnego są konfigurowane dla znanych proxy i znane sieci.</span><span class="sxs-lookup"><span data-stu-id="d38bf-140">Only loopback addresses are configured for known proxies and known networks.</span></span>

## <a name="iisiis-express-and-aspnet-core-module"></a><span data-ttu-id="d38bf-141">Usługi IIS/IIS Express i modułów platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d38bf-141">IIS/IIS Express and ASP.NET Core Module</span></span>

<span data-ttu-id="d38bf-142">Przekazany dalej nagłówki oprogramowanie pośredniczące jest domyślnie przez oprogramowanie pośredniczące integracji usług IIS, gdy aplikacja jest uruchamiana za usług IIS i moduł platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d38bf-142">Forwarded Headers Middleware is enabled by default by IIS Integration Middleware when the app is run behind IIS and the ASP.NET Core Module.</span></span> <span data-ttu-id="d38bf-143">Przekazany dalej nagłówki oprogramowanie pośredniczące został aktywowany do uruchomienia pierwszy w potoku oprogramowania pośredniczącego z ograniczeniami konfiguracją określonego modułu platformy ASP.NET Core ze względu na problemy zaufania z nagłówkami przekazany dalej (na przykład [fałszowanie adresu IP](https://www.iplocation.net/ip-spoofing)).</span><span class="sxs-lookup"><span data-stu-id="d38bf-143">Forwarded Headers Middleware is activated to run first in the middleware pipeline with a restricted configuration specific to the ASP.NET Core Module due to trust concerns with forwarded headers (for example, [IP spoofing](https://www.iplocation.net/ip-spoofing)).</span></span> <span data-ttu-id="d38bf-144">Oprogramowanie pośredniczące jest skonfigurowany do przekazywania `X-Forwarded-For` i `X-Forwarded-Proto` nagłówki i jest ograniczone do jednego hosta lokalnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="d38bf-144">The middleware is configured to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers and is restricted to a single localhost proxy.</span></span> <span data-ttu-id="d38bf-145">Jeśli jest wymagana dodatkowa konfiguracja, zobacz [opcje przekazane oprogramowania pośredniczącego nagłówki](#forwarded-headers-middleware-options).</span><span class="sxs-lookup"><span data-stu-id="d38bf-145">If additional configuration is required, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>

## <a name="other-proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="d38bf-146">Inne serwera proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="d38bf-146">Other proxy server and load balancer scenarios</span></span>

<span data-ttu-id="d38bf-147">Poza za pomocą oprogramowania pośredniczącego integracji usług IIS, przekazywane oprogramowanie pośredniczące nagłówki nie jest domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="d38bf-147">Outside of using IIS Integration Middleware, Forwarded Headers Middleware isn't enabled by default.</span></span> <span data-ttu-id="d38bf-148">Przekazany dalej nagłówki oprogramowania pośredniczącego musi być włączony dla aplikacji do procesu przekazywane nagłówków z [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="d38bf-148">Forwarded Headers Middleware must be enabled for an app to process forwarded headers with [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders).</span></span> <span data-ttu-id="d38bf-149">Po włączeniu oprogramowanie pośredniczące, jeśli nie [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) podano do oprogramowania pośredniczącego, domyślnie [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) są [ForwardedHeaders.None ](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="d38bf-149">After enabling the middleware if no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) are [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span>

<span data-ttu-id="d38bf-150">Konfigurowanie oprogramowania pośredniczącego z [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) do przekazywania `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d38bf-150">Configure the middleware with [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d38bf-151">Wywołanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metody `Startup.Configure` przed wywołaniem innych oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="d38bf-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling other middleware:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    
    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = 
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseStaticFiles();
    // In ASP.NET Core 1.x, replace the following line with: app.UseIdentity();
    app.UseAuthentication();
    app.UseMvc();
}
```

> [!NOTE]
> <span data-ttu-id="d38bf-152">Jeśli nie [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) są określone w `Startup.ConfigureServices` lub bezpośrednio do metody rozszerzenia [UseForwardedHeaders (IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), wartość domyślna nagłówki do przekazywania są [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="d38bf-152">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified in `Startup.ConfigureServices` or directly to the extension method with [UseForwardedHeaders(IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), the default headers to forward are [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span> <span data-ttu-id="d38bf-153">[ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) właściwości musi być skonfigurowany z nagłówkami do przesyłania dalej.</span><span class="sxs-lookup"><span data-stu-id="d38bf-153">The [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) property must be configured with the headers to forward.</span></span>

## <a name="forwarded-headers-middleware-options"></a><span data-ttu-id="d38bf-154">Opcje oprogramowania pośredniczącego nagłówki przekazany dalej</span><span class="sxs-lookup"><span data-stu-id="d38bf-154">Forwarded Headers Middleware options</span></span>

<span data-ttu-id="d38bf-155">[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) sterowania zachowaniem oprogramowania pośredniczącego nagłówki przekazywane:</span><span class="sxs-lookup"><span data-stu-id="d38bf-155">[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) control the behavior of the Forwarded Headers Middleware:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

| <span data-ttu-id="d38bf-156">Opcja</span><span class="sxs-lookup"><span data-stu-id="d38bf-156">Option</span></span> | <span data-ttu-id="d38bf-157">Opis</span><span class="sxs-lookup"><span data-stu-id="d38bf-157">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="d38bf-158">ForwardedForHeaderName</span><span class="sxs-lookup"><span data-stu-id="d38bf-158">ForwardedForHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | <span data-ttu-id="d38bf-159">Użyj wartości header określonej przez tę właściwość, zamiast określony [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).</span><span class="sxs-lookup"><span data-stu-id="d38bf-159">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).</span></span><br><br><span data-ttu-id="d38bf-160">Wartość domyślna to `X-Forwarded-For`.</span><span class="sxs-lookup"><span data-stu-id="d38bf-160">The default is `X-Forwarded-For`.</span></span> |
| [<span data-ttu-id="d38bf-161">ForwardedHeaders</span><span class="sxs-lookup"><span data-stu-id="d38bf-161">ForwardedHeaders</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | <span data-ttu-id="d38bf-162">Określa, które usługi przesyłania dalej powinna zostać przetworzona.</span><span class="sxs-lookup"><span data-stu-id="d38bf-162">Identifies which forwarders should be processed.</span></span> <span data-ttu-id="d38bf-163">Zobacz [ForwardedHeaders wyliczenia](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) dla listy pól, które są stosowane.</span><span class="sxs-lookup"><span data-stu-id="d38bf-163">See the [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) for the list of fields that apply.</span></span> <span data-ttu-id="d38bf-164">Typowe wartości przypisane do tej właściwości to <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.</span><span class="sxs-lookup"><span data-stu-id="d38bf-164">Typical values assigned to this property are <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.</span></span><br><br><span data-ttu-id="d38bf-165">Wartość domyślna to [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="d38bf-165">The default value is [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span> |
| [<span data-ttu-id="d38bf-166">ForwardedHostHeaderName</span><span class="sxs-lookup"><span data-stu-id="d38bf-166">ForwardedHostHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | <span data-ttu-id="d38bf-167">Użyj wartości header określonej przez tę właściwość, zamiast określony [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).</span><span class="sxs-lookup"><span data-stu-id="d38bf-167">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).</span></span><br><br><span data-ttu-id="d38bf-168">Wartość domyślna to `X-Forwarded-Host`.</span><span class="sxs-lookup"><span data-stu-id="d38bf-168">The default is `X-Forwarded-Host`.</span></span> |
| [<span data-ttu-id="d38bf-169">ForwardedProtoHeaderName</span><span class="sxs-lookup"><span data-stu-id="d38bf-169">ForwardedProtoHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | <span data-ttu-id="d38bf-170">Użyj wartości header określonej przez tę właściwość, zamiast określony [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).</span><span class="sxs-lookup"><span data-stu-id="d38bf-170">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).</span></span><br><br><span data-ttu-id="d38bf-171">Wartość domyślna to `X-Forwarded-Proto`.</span><span class="sxs-lookup"><span data-stu-id="d38bf-171">The default is `X-Forwarded-Proto`.</span></span> |
| [<span data-ttu-id="d38bf-172">ForwardLimit</span><span class="sxs-lookup"><span data-stu-id="d38bf-172">ForwardLimit</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | <span data-ttu-id="d38bf-173">Ogranicza liczbę wpisów w nagłówkach, które są przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="d38bf-173">Limits the number of entries in the headers that are processed.</span></span> <span data-ttu-id="d38bf-174">Ustaw `null` Aby wyłączyć limit, ale powinna być wykonywana tylko jeśli `KnownProxies` lub `KnownNetworks` są skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="d38bf-174">Set to `null` to disable the limit, but this should only be done if `KnownProxies` or `KnownNetworks` are configured.</span></span><br><br><span data-ttu-id="d38bf-175">Domyślnym ustawieniem jest 1.</span><span class="sxs-lookup"><span data-stu-id="d38bf-175">The default is 1.</span></span> |
| [<span data-ttu-id="d38bf-176">KnownNetworks</span><span class="sxs-lookup"><span data-stu-id="d38bf-176">KnownNetworks</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | <span data-ttu-id="d38bf-177">Zakresy znanych serwerów proxy, aby zaakceptować przekazywane nagłówków z adresów.</span><span class="sxs-lookup"><span data-stu-id="d38bf-177">Address ranges of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="d38bf-178">Podaj zakresów adresów IP za pomocą notacji międzydomenowego routingu CIDR (Classless).</span><span class="sxs-lookup"><span data-stu-id="d38bf-178">Provide IP ranges using Classless Interdomain Routing (CIDR) notation.</span></span><br><br><span data-ttu-id="d38bf-179">Wartość domyślna to [IList](/dotnet/api/system.collections.generic.ilist-1)\<[wspólny](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> zawierający pojedynczy wpis dla `IPAddress.Loopback`.</span><span class="sxs-lookup"><span data-stu-id="d38bf-179">The default is an [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> containing a single entry for `IPAddress.Loopback`.</span></span> |
| [<span data-ttu-id="d38bf-180">KnownProxies</span><span class="sxs-lookup"><span data-stu-id="d38bf-180">KnownProxies</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | <span data-ttu-id="d38bf-181">Adresy znanych serwerów proxy, aby zaakceptować przekazywane nagłówków z.</span><span class="sxs-lookup"><span data-stu-id="d38bf-181">Addresses of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="d38bf-182">Użyj `KnownProxies` do określenia dokładny adres IP jest zgodny.</span><span class="sxs-lookup"><span data-stu-id="d38bf-182">Use `KnownProxies` to specify exact IP address matches.</span></span><br><br><span data-ttu-id="d38bf-183">Wartość domyślna to [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> zawierający pojedynczy wpis dla `IPAddress.IPv6Loopback`.</span><span class="sxs-lookup"><span data-stu-id="d38bf-183">The default is an [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> containing a single entry for `IPAddress.IPv6Loopback`.</span></span> |
| [<span data-ttu-id="d38bf-184">OriginalForHeaderName</span><span class="sxs-lookup"><span data-stu-id="d38bf-184">OriginalForHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | <span data-ttu-id="d38bf-185">Użyj wartości header określonej przez tę właściwość, zamiast określony [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).</span><span class="sxs-lookup"><span data-stu-id="d38bf-185">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).</span></span><br><br><span data-ttu-id="d38bf-186">Wartość domyślna to `X-Original-For`.</span><span class="sxs-lookup"><span data-stu-id="d38bf-186">The default is `X-Original-For`.</span></span> |
| [<span data-ttu-id="d38bf-187">OriginalHostHeaderName</span><span class="sxs-lookup"><span data-stu-id="d38bf-187">OriginalHostHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | <span data-ttu-id="d38bf-188">Użyj wartości header określonej przez tę właściwość, zamiast określony [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).</span><span class="sxs-lookup"><span data-stu-id="d38bf-188">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).</span></span><br><br><span data-ttu-id="d38bf-189">Wartość domyślna to `X-Original-Host`.</span><span class="sxs-lookup"><span data-stu-id="d38bf-189">The default is `X-Original-Host`.</span></span> |
| [<span data-ttu-id="d38bf-190">OriginalProtoHeaderName</span><span class="sxs-lookup"><span data-stu-id="d38bf-190">OriginalProtoHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | <span data-ttu-id="d38bf-191">Użyj wartości header określonej przez tę właściwość, zamiast określony [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).</span><span class="sxs-lookup"><span data-stu-id="d38bf-191">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).</span></span><br><br><span data-ttu-id="d38bf-192">Wartość domyślna to `X-Original-Proto`.</span><span class="sxs-lookup"><span data-stu-id="d38bf-192">The default is `X-Original-Proto`.</span></span> |
| [<span data-ttu-id="d38bf-193">RequireHeaderSymmetry</span><span class="sxs-lookup"><span data-stu-id="d38bf-193">RequireHeaderSymmetry</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | <span data-ttu-id="d38bf-194">Wymagana liczba wartości nagłówka za synchronizację między [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="d38bf-194">Require the number of header values to be in sync between the [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) being processed.</span></span><br><br><span data-ttu-id="d38bf-195">Domyślnie platformy ASP.NET Core 1.x jest `true`.</span><span class="sxs-lookup"><span data-stu-id="d38bf-195">The default in ASP.NET Core 1.x is `true`.</span></span> <span data-ttu-id="d38bf-196">Domyślnie w programie ASP.NET Core 2.0 lub nowszej jest `false`.</span><span class="sxs-lookup"><span data-stu-id="d38bf-196">The default in ASP.NET Core 2.0 or later is `false`.</span></span> |

## <a name="scenarios-and-use-cases"></a><span data-ttu-id="d38bf-197">Scenariusze i przypadki użycia</span><span class="sxs-lookup"><span data-stu-id="d38bf-197">Scenarios and use cases</span></span>

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a><span data-ttu-id="d38bf-198">W przypadku nie można dodać przekazywane nagłówki i wszystkich żądań jest bezpieczne</span><span class="sxs-lookup"><span data-stu-id="d38bf-198">When it isn't possible to add forwarded headers and all requests are secure</span></span>

<span data-ttu-id="d38bf-199">W niektórych przypadkach może być nie można dodać nagłówków przekazywane na żądanie przekazywane przez serwer proxy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d38bf-199">In some cases, it might not be possible to add forwarded headers to the requests proxied to the app.</span></span> <span data-ttu-id="d38bf-200">Jeśli serwer proxy jest wymuszania, czy wszystkie publicznego zewnętrznego żądania HTTPS, schemat można ręcznie ustawić w `Startup.Configure` przed użyciem dowolnego typu oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="d38bf-200">If the proxy is enforcing that all public external requests are HTTPS, the scheme can be manually set in `Startup.Configure` before using any type of middleware:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

<span data-ttu-id="d38bf-201">Ten kod można wyłączyć z inne ustawienia konfiguracji w środowisku przemieszczania lub rozwoju lub zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="d38bf-201">This code can be disabled with an environment variable or other configuration setting in a development or staging environment.</span></span>

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a><span data-ttu-id="d38bf-202">Postępowania w przypadku ścieżki podstawowej i serwerów proxy, które ścieżka żądania zmiany</span><span class="sxs-lookup"><span data-stu-id="d38bf-202">Deal with path base and proxies that change the request path</span></span>

<span data-ttu-id="d38bf-203">Niektóre serwery proxy przekazać ścieżka niezmienione, ale z aplikacją ścieżki podstawowej, która powinna zostać usunięta, dzięki czemu routing działa poprawnie.</span><span class="sxs-lookup"><span data-stu-id="d38bf-203">Some proxies pass the path intact but with an app base path that should be removed so that routing works properly.</span></span> <span data-ttu-id="d38bf-204">[UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) oprogramowanie pośredniczące dzieli ścieżka [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) i ścieżki podstawowej aplikacji do [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).</span><span class="sxs-lookup"><span data-stu-id="d38bf-204">[UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) middleware splits the path into [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) and the app base path into [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).</span></span>

<span data-ttu-id="d38bf-205">Jeśli `/foo` jest ścieżki podstawowej aplikacji dla ścieżki serwera proxy przekazanego jako `/foo/api/1`, ustawia oprogramowanie pośredniczące `Request.PathBase` do `/foo` i `Request.Path` do `/api/1` przy użyciu następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="d38bf-205">If `/foo` is the app base path for a proxy path passed as `/foo/api/1`, the middleware sets `Request.PathBase` to `/foo` and `Request.Path` to `/api/1` with the following command:</span></span>

```csharp
app.UsePathBase("/foo");
```

<span data-ttu-id="d38bf-206">Oryginalna ścieżka i podstawa ścieżki zostaną ponownie zastosowane, gdy oprogramowanie pośredniczące nie zostanie ponownie wywołany odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="d38bf-206">The original path and path base are reapplied when the middleware is called again in reverse.</span></span> <span data-ttu-id="d38bf-207">Aby uzyskać więcej informacji na temat oprogramowania pośredniczącego kolejność przetwarzania zobacz [oprogramowanie pośredniczące](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="d38bf-207">For more information on middleware order processing, see [Middleware](xref:fundamentals/middleware/index).</span></span>

<span data-ttu-id="d38bf-208">Jeśli serwer proxy przycina ścieżki (na przykład przekazywania `/foo/api/1` do `/api/1`), poprawka przekierowuje i łączy przez ustawienie żądania [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) właściwości:</span><span class="sxs-lookup"><span data-stu-id="d38bf-208">If the proxy trims the path (for example, forwarding `/foo/api/1` to `/api/1`), fix redirects and links by setting the request's [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) property:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

<span data-ttu-id="d38bf-209">Jeśli ścieżka danych polega na dodaniu serwera proxy, pozbyć się części ścieżki ustalenie przekierowania i łącza za pomocą [StartsWithSegments (PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) i przypisywanie do [ścieżki](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) właściwości:</span><span class="sxs-lookup"><span data-stu-id="d38bf-209">If the proxy is adding path data, discard part of the path to fix redirects and links by using [StartsWithSegments(PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) and assigning to the [Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) property:</span></span>

```csharp
app.Use((context, next) =>
{
    if (context.Request.Path.StartsWithSegments("/foo", out var remainder))
    {
        context.Request.Path = remainder;
    }

    return next();
});
```

## <a name="troubleshoot"></a><span data-ttu-id="d38bf-210">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="d38bf-210">Troubleshoot</span></span>

<span data-ttu-id="d38bf-211">Gdy nagłówki nie są przekazywane, zgodnie z oczekiwaniami, należy włączyć [rejestrowanie](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="d38bf-211">When headers aren't forwarded as expected, enable [logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="d38bf-212">Jeśli dzienniki nie zawierają wystarczającej ilości informacji do rozwiązania problemu, wyliczanie nagłówków żądania otrzymane przez serwer.</span><span class="sxs-lookup"><span data-stu-id="d38bf-212">If the logs don't provide sufficient information to troubleshoot the problem, enumerate the request headers received by the server.</span></span> <span data-ttu-id="d38bf-213">Można zapisać nagłówków odpowiedzi aplikacji za pomocą oprogramowania pośredniczącego wbudowany:</span><span class="sxs-lookup"><span data-stu-id="d38bf-213">The headers can be written to an app response using inline middleware:</span></span>

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.Run(async (context) =>
    {
        context.Response.ContentType = "text/plain";

        // Request method, scheme, and path
        await context.Response.WriteAsync(
            $"Request Method: {context.Request.Method}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Scheme: {context.Request.Scheme}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Path: {context.Request.Path}{Environment.NewLine}");

        // Headers
        await context.Response.WriteAsync($"Request Headers:{Environment.NewLine}");

        foreach (var header in context.Request.Headers)
        {
            await context.Response.WriteAsync($"{header.Key}: " +
                $"{header.Value}{Environment.NewLine}");
        }

        await context.Response.WriteAsync(Environment.NewLine);

        // Connection: RemoteIp
        await context.Response.WriteAsync(
            $"Request RemoteIp: {context.Connection.RemoteIpAddress}");
    }
}
```

<span data-ttu-id="d38bf-214">Upewnij się, że X - przekazywane-\* nagłówki są odbierane przez serwer z oczekiwanych wartości.</span><span class="sxs-lookup"><span data-stu-id="d38bf-214">Ensure that the X-Forwarded-\* headers are received by the server with the expected values.</span></span> <span data-ttu-id="d38bf-215">Jeśli podany nagłówek jest wiele wartości, weź pod uwagę nagłówków procesów przekazywane oprogramowanie pośredniczące nagłówków w odwrotnej kolejności od prawej do lewej.</span><span class="sxs-lookup"><span data-stu-id="d38bf-215">If there are multiple values in a given header, note Forwarded Headers Middleware processes headers in reverse order from right to left.</span></span>

<span data-ttu-id="d38bf-216">Żądania oryginalnego zdalny adres IP musi być zgodna wpis w `KnownProxies` lub `KnownNetworks` wymieniono przed X przekazywane do przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="d38bf-216">The request's original remote IP must match an entry in the `KnownProxies` or `KnownNetworks` lists before X-Forwarded-For is processed.</span></span> <span data-ttu-id="d38bf-217">To ogranicza nagłówka fałszowania przez nie akceptuje z niezaufanych serwerów proxy usługi przesyłania dalej.</span><span class="sxs-lookup"><span data-stu-id="d38bf-217">This limits header spoofing by not accepting forwarders from untrusted proxies.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d38bf-218">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d38bf-218">Additional resources</span></span>

* [<span data-ttu-id="d38bf-219">Microsoft Security Advisory CVE-2018-0787: Platformy ASP.NET Core zagrożenie podniesieniem poziomu uprawnień</span><span class="sxs-lookup"><span data-stu-id="d38bf-219">Microsoft Security Advisory CVE-2018-0787: ASP.NET Core Elevation Of Privilege Vulnerability</span></span>](https://github.com/aspnet/Announcements/issues/295)
