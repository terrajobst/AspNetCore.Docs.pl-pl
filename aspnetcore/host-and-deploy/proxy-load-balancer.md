---
title: Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia
author: guardrex
description: Więcej informacji o konfiguracji dla aplikacji hostowanych za serwery proxy i moduły równoważenia obciążenia, które często zasłaniać żądanie ważnych informacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/12/2019
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: 4f04e6cae120ee88734855252542e2bfc2f194a0
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67856168"
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a><span data-ttu-id="d4b5f-103">Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="d4b5f-103">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>

<span data-ttu-id="d4b5f-104">Przez [Luke Latham](https://github.com/guardrex) i [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="d4b5f-104">By [Luke Latham](https://github.com/guardrex) and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="d4b5f-105">W zalecanej konfiguracji dla platformy ASP.NET Core aplikacja jest hostowana przy użyciu modułu Core IIS/ASP.NET, Nginx lub Apache.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-105">In the recommended configuration for ASP.NET Core, the app is hosted using IIS/ASP.NET Core Module, Nginx, or Apache.</span></span> <span data-ttu-id="d4b5f-106">Serwery proxy, moduły równoważenia obciążenia i innych urządzeniach sieciowych często zasłaniać informacje o żądaniu przed osiągnięciem przez nią aplikacji:</span><span class="sxs-lookup"><span data-stu-id="d4b5f-106">Proxy servers, load balancers, and other network appliances often obscure information about the request before it reaches the app:</span></span>

* <span data-ttu-id="d4b5f-107">Gdy żądania HTTPS są przekazywane za pośrednictwem protokołu HTTP, oryginalnym schematem (HTTPS) zostaną utracone i muszą być przekazywane w nagłówku.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-107">When HTTPS requests are proxied over HTTP, the original scheme (HTTPS) is lost and must be forwarded in a header.</span></span>
* <span data-ttu-id="d4b5f-108">Ponieważ aplikacja odbiera żądanie z serwera proxy i jej wartość true, źródła w Internecie lub w sieci firmowej, źródłowy adres IP klienta, również muszą być przekazywane w nagłówku.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-108">Because an app receives a request from the proxy and not its true source on the Internet or corporate network, the originating client IP address must also be forwarded in a header.</span></span>

<span data-ttu-id="d4b5f-109">Te informacje mogą być ważne przetwarzanie żądań, na przykład w przekierowania, uwierzytelnianie, generowanie konsolidacji, ocena zasad i geolokalizacja klienta.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-109">This information may be important in request processing, for example in redirects, authentication, link generation, policy evaluation, and client geolocation.</span></span>

## <a name="forwarded-headers"></a><span data-ttu-id="d4b5f-110">Nagłówki przekazywane</span><span class="sxs-lookup"><span data-stu-id="d4b5f-110">Forwarded headers</span></span>

<span data-ttu-id="d4b5f-111">Zgodnie z Konwencją serwery proxy przesyłania dalej informacji w nagłówkach HTTP.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-111">By convention, proxies forward information in HTTP headers.</span></span>

| <span data-ttu-id="d4b5f-112">nagłówek</span><span class="sxs-lookup"><span data-stu-id="d4b5f-112">Header</span></span> | <span data-ttu-id="d4b5f-113">Opis</span><span class="sxs-lookup"><span data-stu-id="d4b5f-113">Description</span></span> |
| ------ | ----------- |
| <span data-ttu-id="d4b5f-114">X-Forwarded-For</span><span class="sxs-lookup"><span data-stu-id="d4b5f-114">X-Forwarded-For</span></span> | <span data-ttu-id="d4b5f-115">Przechowuje informacje dotyczące klienta, który zainicjował żądań i kolejne serwery proxy w łańcuchu serwery proxy.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-115">Holds information about the client that initiated the request and subsequent proxies in a chain of proxies.</span></span> <span data-ttu-id="d4b5f-116">Ten parametr może zawierać adresów IP adresy (i, opcjonalnie, numery portów).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-116">This parameter may contain IP addresses (and, optionally, port numbers).</span></span> <span data-ttu-id="d4b5f-117">Łańcuch serwerów proxy pierwszy parametr wskazuje klienta, w którym żądanie zostało wystosowane najpierw.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-117">In a chain of proxy servers, the first parameter indicates the client where the request was first made.</span></span> <span data-ttu-id="d4b5f-118">Postępuj zgodnie z identyfikatorami kolejnych serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-118">Subsequent proxy identifiers follow.</span></span> <span data-ttu-id="d4b5f-119">Ostatni serwer proxy w łańcuchu nie ma na liście parametrów.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-119">The last proxy in the chain isn't in the list of parameters.</span></span> <span data-ttu-id="d4b5f-120">Adres IP ostatniego serwera proxy i opcjonalnie numeru portu są dostępne jako zdalny adres IP w warstwie transportowej.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-120">The last proxy's IP address, and optionally a port number, are available as the remote IP address at the transport layer.</span></span> |
| <span data-ttu-id="d4b5f-121">X-Forwarded-Proto</span><span class="sxs-lookup"><span data-stu-id="d4b5f-121">X-Forwarded-Proto</span></span> | <span data-ttu-id="d4b5f-122">Wartość źródłowy schemat (HTTP/HTTPS).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-122">The value of the originating scheme (HTTP/HTTPS).</span></span> <span data-ttu-id="d4b5f-123">Wartość może być też listy programów, jeśli żądanie jest przesunięta wielu serwerów proxy.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-123">The value may also be a list of schemes if the request has traversed multiple proxies.</span></span> |
| <span data-ttu-id="d4b5f-124">X-Forwarded-Host</span><span class="sxs-lookup"><span data-stu-id="d4b5f-124">X-Forwarded-Host</span></span> | <span data-ttu-id="d4b5f-125">Oryginalna wartość pola nagłówka hosta.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-125">The original value of the Host header field.</span></span> <span data-ttu-id="d4b5f-126">Zazwyczaj serwery proxy nie należy modyfikować nagłówka hosta.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-126">Usually, proxies don't modify the Host header.</span></span> <span data-ttu-id="d4b5f-127">Zobacz [Microsoft Security doradztwa technicznego dotyczącego CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) uzyskać informacji na temat luk w zabezpieczeniach typu "podniesienia uprawnień", która dotyczy systemów, w którym serwer proxy nie sprawdzania poprawności, lub ograniczyć nagłówki hosta do znane dobre wartości.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-127">See [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) for information on an elevation-of-privileges vulnerability that affects systems where the proxy doesn't validate or restrict Host headers to known good values.</span></span> |

<span data-ttu-id="d4b5f-128">Pośredniczącym nagłówki przekazywane z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pakietu, odczytuje tych nagłówków i wypełnia skojarzonymi polami na <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-128">The Forwarded Headers Middleware, from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package, reads these headers and fills in the associated fields on <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

<span data-ttu-id="d4b5f-129">Aktualizacje oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="d4b5f-129">The middleware updates:</span></span>

* <span data-ttu-id="d4b5f-130">[HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress) &ndash; można ustawić przy użyciu `X-Forwarded-For` wartość nagłówka.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-130">[HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress) &ndash; Set using the `X-Forwarded-For` header value.</span></span> <span data-ttu-id="d4b5f-131">Dodatkowe ustawienia mają wpływ na sposób ustawiania oprogramowanie pośredniczące `RemoteIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-131">Additional settings influence how the middleware sets `RemoteIpAddress`.</span></span> <span data-ttu-id="d4b5f-132">Aby uzyskać więcej informacji, zobacz [opcje przekazywane oprogramowania pośredniczącego nagłówki](#forwarded-headers-middleware-options).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-132">For details, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>
* <span data-ttu-id="d4b5f-133">[HttpContext.Request.Scheme](xref:Microsoft.AspNetCore.Http.HttpRequest.Scheme) &ndash; można ustawić przy użyciu `X-Forwarded-Proto` wartość nagłówka.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-133">[HttpContext.Request.Scheme](xref:Microsoft.AspNetCore.Http.HttpRequest.Scheme) &ndash; Set using the `X-Forwarded-Proto` header value.</span></span>
* <span data-ttu-id="d4b5f-134">[HttpContext.Request.Host](xref:Microsoft.AspNetCore.Http.HttpRequest.Host) &ndash; można ustawić przy użyciu `X-Forwarded-Host` wartość nagłówka.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-134">[HttpContext.Request.Host](xref:Microsoft.AspNetCore.Http.HttpRequest.Host) &ndash; Set using the `X-Forwarded-Host` header value.</span></span>

<span data-ttu-id="d4b5f-135">Oprogramowanie pośredniczące nagłówki przekazywane [domyślne ustawienia](#forwarded-headers-middleware-options) można skonfigurować.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-135">Forwarded Headers Middleware [default settings](#forwarded-headers-middleware-options) can be configured.</span></span> <span data-ttu-id="d4b5f-136">Ustawienia domyślne są:</span><span class="sxs-lookup"><span data-stu-id="d4b5f-136">The default settings are:</span></span>

* <span data-ttu-id="d4b5f-137">Jest dostępny tylko *jeden serwer proxy* między aplikacją a źródło żądania.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-137">There is only *one proxy* between the app and the source of the requests.</span></span>
* <span data-ttu-id="d4b5f-138">Tylko adresy sprzężenia zwrotnego są skonfigurowane dla znanych serwerów proxy i znane sieci.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-138">Only loopback addresses are configured for known proxies and known networks.</span></span>
* <span data-ttu-id="d4b5f-139">Nagłówki przekazywane są nazywane `X-Forwarded-For` i `X-Forwarded-Proto`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-139">The forwarded headers are named `X-Forwarded-For` and `X-Forwarded-Proto`.</span></span>

<span data-ttu-id="d4b5f-140">Nie wszystkie urządzenia sieciowe, Dodaj `X-Forwarded-For` i `X-Forwarded-Proto` nagłówki bez dodatkowej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-140">Not all network appliances add the `X-Forwarded-For` and `X-Forwarded-Proto` headers without additional configuration.</span></span> <span data-ttu-id="d4b5f-141">Sprawdź wskazówki z producentem urządzenia, jeśli żądań kierowanych na serwer proxy nie zawierają tych nagłówków, gdy osiągną oni aplikację.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-141">Consult your appliance manufacturer's guidance if proxied requests don't contain these headers when they reach the app.</span></span> <span data-ttu-id="d4b5f-142">Jeśli urządzenie korzysta z nazw nagłówka innego niż `X-Forwarded-For` i `X-Forwarded-Proto`ustaw <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> i <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> opcje można dopasować nazw nagłówka używany przez urządzenie.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-142">If the appliance uses different header names than `X-Forwarded-For` and `X-Forwarded-Proto`, set the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> and <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> options to match the header names used by the appliance.</span></span> <span data-ttu-id="d4b5f-143">Aby uzyskać więcej informacji, zobacz [opcje przekazywane oprogramowania pośredniczącego nagłówki](#forwarded-headers-middleware-options) i [konfigurację serwera proxy, który korzysta z nazw inny nagłówek](#configuration-for-a-proxy-that-uses-different-header-names).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-143">For more information, see [Forwarded Headers Middleware options](#forwarded-headers-middleware-options) and [Configuration for a proxy that uses different header names](#configuration-for-a-proxy-that-uses-different-header-names).</span></span>

## <a name="iisiis-express-and-aspnet-core-module"></a><span data-ttu-id="d4b5f-144">Usługi IIS/IIS Express i modułów platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d4b5f-144">IIS/IIS Express and ASP.NET Core Module</span></span>

<span data-ttu-id="d4b5f-145">Przekazane nagłówki oprogramowanie pośredniczące jest włączona domyślnie [oprogramowania pośredniczącego integracji usługi IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) gdy aplikacja jest hostowana [spoza procesu](xref:host-and-deploy/iis/index#out-of-process-hosting-model) usług IIS oraz modułu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-145">Forwarded Headers Middleware is enabled by default by [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) when the app is hosted [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind IIS and the ASP.NET Core Module.</span></span> <span data-ttu-id="d4b5f-146">Przekazane nagłówki oprogramowanie pośredniczące jest aktywowana ma być uruchomiony w potoku oprogramowania pośredniczącego przy użyciu określonej konfiguracji ograniczone do modułu ASP.NET Core ze względu na wątpliwości relacji zaufania z nagłówkami przekazywane (na przykład [fałszowanie adresów IP](https://www.iplocation.net/ip-spoofing)).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-146">Forwarded Headers Middleware is activated to run first in the middleware pipeline with a restricted configuration specific to the ASP.NET Core Module due to trust concerns with forwarded headers (for example, [IP spoofing](https://www.iplocation.net/ip-spoofing)).</span></span> <span data-ttu-id="d4b5f-147">Oprogramowanie pośredniczące jest skonfigurowana do przesyłania dalej `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków i jest ograniczona do jednego hosta lokalnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-147">The middleware is configured to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers and is restricted to a single localhost proxy.</span></span> <span data-ttu-id="d4b5f-148">Jeśli wymagana jest dodatkowa konfiguracja, zobacz [opcje przekazywane oprogramowania pośredniczącego nagłówki](#forwarded-headers-middleware-options).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-148">If additional configuration is required, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>

## <a name="other-proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="d4b5f-149">Inne serwera proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="d4b5f-149">Other proxy server and load balancer scenarios</span></span>

<span data-ttu-id="d4b5f-150">Poza przy użyciu [integracji usług IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) hostując [spoza procesu](xref:host-and-deploy/iis/index#out-of-process-hosting-model), przekazywane oprogramowania pośredniczącego nagłówków nie jest włączona domyślnie.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-150">Outside of using [IIS Integration](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) when hosting [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), Forwarded Headers Middleware isn't enabled by default.</span></span> <span data-ttu-id="d4b5f-151">Przekazane nagłówki oprogramowania pośredniczącego musi być włączona dla aplikacji do nagłówków procesów przekazywanych za pomocą <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-151">Forwarded Headers Middleware must be enabled for an app to process forwarded headers with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>.</span></span> <span data-ttu-id="d4b5f-152">Po włączeniu oprogramowanie pośredniczące, jeśli nie <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> określonych do oprogramowania pośredniczącego, domyślnie [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) są [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-152">After enabling the middleware if no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified to the middleware, the default [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) are [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span></span>

<span data-ttu-id="d4b5f-153">Konfigurowanie oprogramowania pośredniczącego z <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> do przekazywania `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-153">Configure the middleware with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d4b5f-154">Wywoływanie <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in Class metoda `Startup.Configure` przed wywołaniem innym oprogramowaniu pośredniczącym:</span><span class="sxs-lookup"><span data-stu-id="d4b5f-154">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in `Startup.Configure` before calling other middleware:</span></span>

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
> <span data-ttu-id="d4b5f-155">Jeśli nie <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> są określone w `Startup.ConfigureServices` lub bezpośrednio do metody rozszerzenia za pomocą <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>, są domyślne nagłówki do przekazywania [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-155">If no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified in `Startup.ConfigureServices` or directly to the extension method with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>, the default headers to forward are [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span></span> <span data-ttu-id="d4b5f-156"><xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> Właściwość musi być skonfigurowany przy użyciu nagłówków do przesyłania dalej.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-156">The <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> property must be configured with the headers to forward.</span></span>

## <a name="nginx-configuration"></a><span data-ttu-id="d4b5f-157">Konfigurację serwera Nginx</span><span class="sxs-lookup"><span data-stu-id="d4b5f-157">Nginx configuration</span></span>

<span data-ttu-id="d4b5f-158">Do przekazywania `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków, zobacz <xref:host-and-deploy/linux-nginx#configure-nginx>.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-158">To forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers, see <xref:host-and-deploy/linux-nginx#configure-nginx>.</span></span> <span data-ttu-id="d4b5f-159">Aby uzyskać więcej informacji, zobacz [NGINX: Przy użyciu nagłówka przesłanym](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-159">For more information, see [NGINX: Using the Forwarded header](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/).</span></span>

## <a name="apache-configuration"></a><span data-ttu-id="d4b5f-160">Konfiguracja Apache</span><span class="sxs-lookup"><span data-stu-id="d4b5f-160">Apache configuration</span></span>

<span data-ttu-id="d4b5f-161">`X-Forwarded-For` zostanie dodany automatycznie (patrz [mod_proxy Apache modułu: Reverse Proxy Request Headers](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers)).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-161">`X-Forwarded-For` is added automatically (see [Apache Module mod_proxy: Reverse Proxy Request Headers](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers)).</span></span> <span data-ttu-id="d4b5f-162">Aby uzyskać informacje na temat sposobu przekazywania `X-Forwarded-Proto` nagłówka, zobacz <xref:host-and-deploy/linux-apache#configure-apache>.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-162">For information on how to forward the `X-Forwarded-Proto` header, see <xref:host-and-deploy/linux-apache#configure-apache>.</span></span>

## <a name="forwarded-headers-middleware-options"></a><span data-ttu-id="d4b5f-163">Przekazane opcje oprogramowania pośredniczącego nagłówki</span><span class="sxs-lookup"><span data-stu-id="d4b5f-163">Forwarded Headers Middleware options</span></span>

<span data-ttu-id="d4b5f-164"><xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> Sterowanie zachowaniem oprogramowania pośredniczącego nagłówki przekazywane.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-164"><xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> control the behavior of the Forwarded Headers Middleware.</span></span> <span data-ttu-id="d4b5f-165">Poniższy przykład umożliwia zmianę wartości domyślne:</span><span class="sxs-lookup"><span data-stu-id="d4b5f-165">The following example changes the default values:</span></span>

* <span data-ttu-id="d4b5f-166">Ogranicz liczbę wpisów w nagłówki przekazywane do `2`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-166">Limit the number of entries in the forwarded headers to `2`.</span></span>
* <span data-ttu-id="d4b5f-167">Dodaj adres serwera proxy znanych `127.0.10.1`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-167">Add a known proxy address of `127.0.10.1`.</span></span>
* <span data-ttu-id="d4b5f-168">Zmień nazwę nagłówka przekazywane z domyślnego `X-Forwarded-For` do `X-Forwarded-For-My-Custom-Header-Name`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-168">Change the forwarded header name from the default `X-Forwarded-For` to `X-Forwarded-For-My-Custom-Header-Name`.</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-My-Custom-Header-Name";
});
```

| <span data-ttu-id="d4b5f-169">Opcja</span><span class="sxs-lookup"><span data-stu-id="d4b5f-169">Option</span></span> | <span data-ttu-id="d4b5f-170">Opis</span><span class="sxs-lookup"><span data-stu-id="d4b5f-170">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> | <span data-ttu-id="d4b5f-171">Ogranicza hosty `X-Forwarded-Host` nagłówka do podanych wartości.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-171">Restricts hosts by the `X-Forwarded-Host` header to the values provided.</span></span><ul><li><span data-ttu-id="d4b5f-172">Wartości są porównywane, liczba porządkowa Ignoruj-wielkie i małe litery.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-172">Values are compared using ordinal-ignore-case.</span></span></li><li><span data-ttu-id="d4b5f-173">Numery portów muszą zostać wykluczone.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-173">Port numbers must be excluded.</span></span></li><li><span data-ttu-id="d4b5f-174">Jeśli lista jest pusta, wszystkie hosty są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-174">If the list is empty, all hosts are allowed.</span></span></li><li><span data-ttu-id="d4b5f-175">Symbol wieloznaczny najwyższego poziomu `*` zezwala na wszystkich hostach niepusta.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-175">A top-level wildcard `*` allows all non-empty hosts.</span></span></li><li><span data-ttu-id="d4b5f-176">Poddomena symbole wieloznaczne są dozwolone, ale nie są zgodne z domeny katalogu głównego.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-176">Subdomain wildcards are permitted but don't match the root domain.</span></span> <span data-ttu-id="d4b5f-177">Na przykład `*.contoso.com` odpowiada poddomeny `foo.contoso.com` , ale nie domeny katalogu głównego `contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-177">For example, `*.contoso.com` matches the subdomain `foo.contoso.com` but not the root domain `contoso.com`.</span></span></li><li><span data-ttu-id="d4b5f-178">Nazwy hostów Unicode są dozwolone, ale są konwertowane na [Punycode](https://tools.ietf.org/html/rfc3492) do dopasowania.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-178">Unicode host names are allowed but are converted to [Punycode](https://tools.ietf.org/html/rfc3492) for matching.</span></span></li><li><span data-ttu-id="d4b5f-179">[Adresy IPv6](https://tools.ietf.org/html/rfc4291) musi zawierać blokujących nawiasy kwadratowe i musi być w [konwencjonalne formularza](https://tools.ietf.org/html/rfc4291#section-2.2) (na przykład `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-179">[IPv6 addresses](https://tools.ietf.org/html/rfc4291) must include bounding brackets and be in [conventional form](https://tools.ietf.org/html/rfc4291#section-2.2) (for example, `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`).</span></span> <span data-ttu-id="d4b5f-180">Adresy IPv6 nie są specjalne — z uwzględnieniem wielkości liter do sprawdzenia pod kątem równości logicznych między różnych formatów, a nie kanoniczną jest wykonywane.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-180">IPv6 addresses aren't special-cased to check for logical equality between different formats, and no canonicalization is performed.</span></span></li><li><span data-ttu-id="d4b5f-181">Nie można ograniczyć dozwolone hosty mogą zezwolić osobie atakującej podszywały się pod łącza wygenerowanej przez usługę.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-181">Failure to restrict the allowed hosts may allow an attacker to spoof links generated by the service.</span></span></li></ul><span data-ttu-id="d4b5f-182">Wartość domyślna to pusta `IList<string>`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-182">The default value is an empty `IList<string>`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> | <span data-ttu-id="d4b5f-183">Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XForwardedForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedForHeaderName).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-183">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedForHeaderName).</span></span> <span data-ttu-id="d4b5f-184">Ta opcja jest używana, gdy nie korzysta z serwera proxy/usługi przesyłania dalej `X-Forwarded-For` nagłówka, ale używa niektórych innych nagłówka do przekazywania informacji.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-184">This option is used when the proxy/forwarder doesn't use the `X-Forwarded-For` header but uses some other header to forward the information.</span></span><br><br><span data-ttu-id="d4b5f-185">Wartość domyślna to `X-Forwarded-For`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-185">The default is `X-Forwarded-For`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> | <span data-ttu-id="d4b5f-186">Określa, które usług przesyłania dalej powinny zostać przetworzone.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-186">Identifies which forwarders should be processed.</span></span> <span data-ttu-id="d4b5f-187">Zobacz [wyliczenia ForwardedHeaders](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders) dla listy pól, które są stosowane.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-187">See the [ForwardedHeaders Enum](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders) for the list of fields that apply.</span></span> <span data-ttu-id="d4b5f-188">Typowe wartości przypisane do tej właściwości to "ForwardedHeaders.XForwardedFor</span><span class="sxs-lookup"><span data-stu-id="d4b5f-188">Typical values assigned to this property are \`ForwardedHeaders.XForwardedFor</span></span> | <span data-ttu-id="d4b5f-189">ForwardedHeaders.XForwardedProto\`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-189">ForwardedHeaders.XForwardedProto\`.</span></span><br><br><span data-ttu-id="d4b5f-190">Wartość domyślna to [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-190">The default value is [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHostHeaderName> | <span data-ttu-id="d4b5f-191">Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XForwardedHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedHostHeaderName).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-191">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedHostHeaderName).</span></span> <span data-ttu-id="d4b5f-192">Ta opcja jest używana, gdy nie korzysta z serwera proxy/usługi przesyłania dalej `X-Forwarded-Host` nagłówka, ale używa niektórych innych nagłówka do przekazywania informacji.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-192">This option is used when the proxy/forwarder doesn't use the `X-Forwarded-Host` header but uses some other header to forward the information.</span></span><br><br><span data-ttu-id="d4b5f-193">Wartość domyślna to `X-Forwarded-Host`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-193">The default is `X-Forwarded-Host`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> | <span data-ttu-id="d4b5f-194">Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XForwardedProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedProtoHeaderName).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-194">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedProtoHeaderName).</span></span> <span data-ttu-id="d4b5f-195">Ta opcja jest używana, gdy nie korzysta z serwera proxy/usługi przesyłania dalej `X-Forwarded-Proto` nagłówka, ale używa niektórych innych nagłówka do przekazywania informacji.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-195">This option is used when the proxy/forwarder doesn't use the `X-Forwarded-Proto` header but uses some other header to forward the information.</span></span><br><br><span data-ttu-id="d4b5f-196">Wartość domyślna to `X-Forwarded-Proto`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-196">The default is `X-Forwarded-Proto`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardLimit> | <span data-ttu-id="d4b5f-197">Ogranicza liczbę wpisów w nagłówki, które są przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-197">Limits the number of entries in the headers that are processed.</span></span> <span data-ttu-id="d4b5f-198">Ustaw `null` wyłączyć limit, ale powinno mieć miejsce tylko jeśli `KnownProxies` lub `KnownNetworks` są skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-198">Set to `null` to disable the limit, but this should only be done if `KnownProxies` or `KnownNetworks` are configured.</span></span> <span data-ttu-id="d4b5f-199">Ustawienie innej niż`null` wartość środek ostrożności (ale nie stanowi ona gwarancji), aby zabezpieczyć się przed nieprawidłowo serwery proxy i złośliwymi żądaniami pochodzące z usługi po stronie kanałów w sieci.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-199">Setting a non-`null` value is a precaution (but not a guarantee) to guard against misconfigured proxies and malicious requests arriving from side-channels on the network.</span></span><br><br><span data-ttu-id="d4b5f-200">Przekazane nagłówki oprogramowanie pośredniczące przetwarza nagłówków w odwrotnej kolejności od prawej do lewej.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-200">Forwarded Headers Middleware processes headers in reverse order from right to left.</span></span> <span data-ttu-id="d4b5f-201">Jeśli wartość domyślna (`1`) jest używany, wartość prawej z nagłówków są przetwarzane, o ile wartości `ForwardLimit` wzrasta.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-201">If the default value (`1`) is used, only the rightmost value from the headers is processed unless the value of `ForwardLimit` is increased.</span></span><br><br><span data-ttu-id="d4b5f-202">Wartość domyślna to `1`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-202">The default is `1`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks> | <span data-ttu-id="d4b5f-203">Zakresy znanych sieci w celu akceptowania nagłówki przekazywane z adresów.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-203">Address ranges of known networks to accept forwarded headers from.</span></span> <span data-ttu-id="d4b5f-204">Podaj zakresów adresów IP przy użyciu notacji Bezklasowego routingu międzydomenowego (CIDR).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-204">Provide IP ranges using Classless Interdomain Routing (CIDR) notation.</span></span><br><br><span data-ttu-id="d4b5f-205">Jeśli serwer używa podwójnych gniazd, adresy IPv4, są dostarczane w formacie protokołu IPv6 (na przykład `10.0.0.1` w protokole IPv4 reprezentowane w protokole IPv6 jako `::ffff:10.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-205">If the server is using dual-mode sockets, IPv4 addresses are supplied in an IPv6 format (for example, `10.0.0.1` in IPv4 represented in IPv6 as `::ffff:10.0.0.1`).</span></span> <span data-ttu-id="d4b5f-206">See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-206">See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span></span> <span data-ttu-id="d4b5f-207">Określić, jeśli ten format jest wymagany, analizując [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-207">Determine if this format is required by looking at the [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span></span> <span data-ttu-id="d4b5f-208">Aby uzyskać więcej informacji, zobacz [konfigurację dla adresu IPv4 reprezentowane jako adres IPv6](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) sekcji.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-208">For more information, see the [Configuration for an IPv4 address represented as an IPv6 address](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) section.</span></span><br><br><span data-ttu-id="d4b5f-209">Wartość domyślna to `IList` \< <xref:Microsoft.AspNetCore.HttpOverrides.IPNetwork>> zawierający pojedynczy wpis dla `IPAddress.Loopback`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-209">The default is an `IList`\<<xref:Microsoft.AspNetCore.HttpOverrides.IPNetwork>> containing a single entry for `IPAddress.Loopback`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies> | <span data-ttu-id="d4b5f-210">Adresy znanych serwerów proxy, aby zaakceptować nagłówki przekazywane z.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-210">Addresses of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="d4b5f-211">Użyj `KnownProxies` określić dokładny adres IP jest zgodny.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-211">Use `KnownProxies` to specify exact IP address matches.</span></span><br><br><span data-ttu-id="d4b5f-212">Jeśli serwer używa podwójnych gniazd, adresy IPv4, są dostarczane w formacie protokołu IPv6 (na przykład `10.0.0.1` w protokole IPv4 reprezentowane w protokole IPv6 jako `::ffff:10.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-212">If the server is using dual-mode sockets, IPv4 addresses are supplied in an IPv6 format (for example, `10.0.0.1` in IPv4 represented in IPv6 as `::ffff:10.0.0.1`).</span></span> <span data-ttu-id="d4b5f-213">See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-213">See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span></span> <span data-ttu-id="d4b5f-214">Określić, jeśli ten format jest wymagany, analizując [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-214">Determine if this format is required by looking at the [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span></span> <span data-ttu-id="d4b5f-215">Aby uzyskać więcej informacji, zobacz [konfigurację dla adresu IPv4 reprezentowane jako adres IPv6](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) sekcji.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-215">For more information, see the [Configuration for an IPv4 address represented as an IPv6 address](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) section.</span></span><br><br><span data-ttu-id="d4b5f-216">Wartość domyślna to `IList` \< <xref:System.Net.IPAddress>> zawierający pojedynczy wpis dla `IPAddress.IPv6Loopback`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-216">The default is an `IList`\<<xref:System.Net.IPAddress>> containing a single entry for `IPAddress.IPv6Loopback`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalForHeaderName> | <span data-ttu-id="d4b5f-217">Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XOriginalForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalForHeaderName).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-217">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalForHeaderName).</span></span><br><br><span data-ttu-id="d4b5f-218">Wartość domyślna to `X-Original-For`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-218">The default is `X-Original-For`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalHostHeaderName> | <span data-ttu-id="d4b5f-219">Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XOriginalHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalHostHeaderName).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-219">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalHostHeaderName).</span></span><br><br><span data-ttu-id="d4b5f-220">Wartość domyślna to `X-Original-Host`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-220">The default is `X-Original-Host`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalProtoHeaderName> | <span data-ttu-id="d4b5f-221">Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XOriginalProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalProtoHeaderName).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-221">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalProtoHeaderName).</span></span><br><br><span data-ttu-id="d4b5f-222">Wartość domyślna to `X-Original-Proto`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-222">The default is `X-Original-Proto`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.RequireHeaderSymmetry> | <span data-ttu-id="d4b5f-223">Wymagana jest liczba wartości nagłówka, aby był zsynchronizowany między [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) przetwarzany.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-223">Require the number of header values to be in sync between the [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) being processed.</span></span><br><br><span data-ttu-id="d4b5f-224">Wartość domyślna w programie ASP.NET Core 1.x jest `true`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-224">The default in ASP.NET Core 1.x is `true`.</span></span> <span data-ttu-id="d4b5f-225">Domyślnie w programie ASP.NET Core 2.0 lub nowszym `false`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-225">The default in ASP.NET Core 2.0 or later is `false`.</span></span> |

## <a name="scenarios-and-use-cases"></a><span data-ttu-id="d4b5f-226">Scenariuszy i przypadków użycia</span><span class="sxs-lookup"><span data-stu-id="d4b5f-226">Scenarios and use cases</span></span>

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a><span data-ttu-id="d4b5f-227">Nie można dodać przekazywane nagłówki oraz wszystkie żądania są bezpieczne</span><span class="sxs-lookup"><span data-stu-id="d4b5f-227">When it isn't possible to add forwarded headers and all requests are secure</span></span>

<span data-ttu-id="d4b5f-228">W niektórych przypadkach może być nie można dodać nagłówki przekazywane do żądania, które są przekazywane do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-228">In some cases, it might not be possible to add forwarded headers to the requests proxied to the app.</span></span> <span data-ttu-id="d4b5f-229">Jeśli serwer proxy jest wymuszania, czy wszystkie publiczne zewnętrznych żądania HTTPS, schemat można ręcznie ustawić `Startup.Configure` przed użyciem dowolnego typu oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="d4b5f-229">If the proxy is enforcing that all public external requests are HTTPS, the scheme can be manually set in `Startup.Configure` before using any type of middleware:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

<span data-ttu-id="d4b5f-230">Ten kod można wyłączyć za pomocą zmiennej środowiskowej lub inne ustawienia konfiguracji w rozwoju lub w środowisku przejściowym.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-230">This code can be disabled with an environment variable or other configuration setting in a development or staging environment.</span></span>

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a><span data-ttu-id="d4b5f-231">Podstawa ścieżki i serwery proxy, które zmieniają ścieżki żądania</span><span class="sxs-lookup"><span data-stu-id="d4b5f-231">Deal with path base and proxies that change the request path</span></span>

<span data-ttu-id="d4b5f-232">Niektóre serwery proxy Przekaż ścieżkę bez zmian, ale z aplikacją ścieżki podstawowej, które powinno zostać usunięte, tak aby routing działa poprawnie.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-232">Some proxies pass the path intact but with an app base path that should be removed so that routing works properly.</span></span> <span data-ttu-id="d4b5f-233">[UsePathBaseExtensions.UsePathBase](xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*) oprogramowania pośredniczącego dzieli ścieżkę do [HttpRequest.Path](xref:Microsoft.AspNetCore.Http.HttpRequest.Path) i ścieżki podstawowej aplikacji do [HttpRequest.PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-233">[UsePathBaseExtensions.UsePathBase](xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*) middleware splits the path into [HttpRequest.Path](xref:Microsoft.AspNetCore.Http.HttpRequest.Path) and the app base path into [HttpRequest.PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase).</span></span>

<span data-ttu-id="d4b5f-234">Jeśli `/foo` jest ścieżką serwera proxy są przekazywane jako ścieżki podstawowej aplikacji `/foo/api/1`, zestawy oprogramowania pośredniczącego `Request.PathBase` do `/foo` i `Request.Path` do `/api/1` za pomocą następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="d4b5f-234">If `/foo` is the app base path for a proxy path passed as `/foo/api/1`, the middleware sets `Request.PathBase` to `/foo` and `Request.Path` to `/api/1` with the following command:</span></span>

```csharp
app.UsePathBase("/foo");
```

<span data-ttu-id="d4b5f-235">Oryginalna ścieżka i podstawa ścieżki zostaną ponownie zastosowane, gdy oprogramowanie pośredniczące wywoływana jest ponownie w odwrotnej kolejności.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-235">The original path and path base are reapplied when the middleware is called again in reverse.</span></span> <span data-ttu-id="d4b5f-236">Aby uzyskać więcej informacji na temat przetwarzania zamówień oprogramowanie pośredniczące, zobacz <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-236">For more information on middleware order processing, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="d4b5f-237">Jeśli serwer proxy usuwa ścieżki (na przykład przekazywania `/foo/api/1` do `/api/1`), poprawka przekierowuje i łączy, ustawiając żądania [PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase) właściwości:</span><span class="sxs-lookup"><span data-stu-id="d4b5f-237">If the proxy trims the path (for example, forwarding `/foo/api/1` to `/api/1`), fix redirects and links by setting the request's [PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase) property:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

<span data-ttu-id="d4b5f-238">Jeśli serwer proxy dodaje dane ścieżki, odrzucić część ścieżki rozwiązać za pomocą przekierowań i łącza <xref:Microsoft.AspNetCore.Http.PathString.StartsWithSegments*> i przypisywanie do <xref:Microsoft.AspNetCore.Http.HttpRequest.Path> właściwości:</span><span class="sxs-lookup"><span data-stu-id="d4b5f-238">If the proxy is adding path data, discard part of the path to fix redirects and links by using <xref:Microsoft.AspNetCore.Http.PathString.StartsWithSegments*> and assigning to the <xref:Microsoft.AspNetCore.Http.HttpRequest.Path> property:</span></span>

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

### <a name="configuration-for-a-proxy-that-uses-different-header-names"></a><span data-ttu-id="d4b5f-239">Konfiguracja serwera proxy, który korzysta z nazw nagłówka</span><span class="sxs-lookup"><span data-stu-id="d4b5f-239">Configuration for a proxy that uses different header names</span></span>

<span data-ttu-id="d4b5f-240">Jeśli serwer proxy nie używa nagłówków o nazwie `X-Forwarded-For` i `X-Forwarded-Proto` do przekazywania adresów/port serwera proxy i pochodzącymi informacji schematu, ustaw <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> i <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> opcje można dopasować nazw nagłówka używany przez serwer proxy:</span><span class="sxs-lookup"><span data-stu-id="d4b5f-240">If the proxy doesn't use headers named `X-Forwarded-For` and `X-Forwarded-Proto` to forward the proxy address/port and originating scheme information, set the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> and <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> options to match the header names used by the proxy:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardedForHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-For_Header";
    options.ForwardedProtoHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-Proto_Header";
});
```

### <a name="configuration-for-an-ipv4-address-represented-as-an-ipv6-address"></a><span data-ttu-id="d4b5f-241">Konfiguracja dla adresu IPv4, reprezentowane jako adres IPv6</span><span class="sxs-lookup"><span data-stu-id="d4b5f-241">Configuration for an IPv4 address represented as an IPv6 address</span></span>

<span data-ttu-id="d4b5f-242">Jeśli serwer używa podwójnych gniazd, adresy IPv4, są dostarczane w formacie protokołu IPv6 (na przykład `10.0.0.1` w protokole IPv4 reprezentowane w protokole IPv6 jako `::ffff:10.0.0.1` lub `::ffff:a00:1`).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-242">If the server is using dual-mode sockets, IPv4 addresses are supplied in an IPv6 format (for example, `10.0.0.1` in IPv4 represented in IPv6 as `::ffff:10.0.0.1` or `::ffff:a00:1`).</span></span> <span data-ttu-id="d4b5f-243">See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-243">See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span></span> <span data-ttu-id="d4b5f-244">Określić, jeśli ten format jest wymagany, analizując [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-244">Determine if this format is required by looking at the [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span></span>

<span data-ttu-id="d4b5f-245">W poniższym przykładzie adresu sieciowego, który dostarcza nagłówki przekazywane jest dodawany do `KnownNetworks` listy w formacie IPv6.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-245">In the following example, a network address that supplies forwarded headers is added to the `KnownNetworks` list in IPv6 format.</span></span>

<span data-ttu-id="d4b5f-246">Adres IPv4: `10.11.12.1/8`</span><span class="sxs-lookup"><span data-stu-id="d4b5f-246">IPv4 address: `10.11.12.1/8`</span></span>

<span data-ttu-id="d4b5f-247">Przekonwertowana adres IPv6: `::ffff:10.11.12.1`</span><span class="sxs-lookup"><span data-stu-id="d4b5f-247">Converted IPv6 address: `::ffff:10.11.12.1`</span></span>  
<span data-ttu-id="d4b5f-248">Długość prefiksu przekonwertowana: 104</span><span class="sxs-lookup"><span data-stu-id="d4b5f-248">Converted prefix length: 104</span></span>

<span data-ttu-id="d4b5f-249">Można też podać adres w formacie szesnastkowym (`10.11.12.1` reprezentowane w protokole IPv6 jako `::ffff:0a0b:0c01`).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-249">You can also supply the address in hexadecimal format (`10.11.12.1` represented in IPv6 as `::ffff:0a0b:0c01`).</span></span> <span data-ttu-id="d4b5f-250">Podczas konwertowania adres IPv4, IPv6, Dodaj 96 do długości prefiksów CIDR (`8` w przykładzie), aby uwzględnić dodatkowe `::ffff:` prefiks IPv6 (8 + 96 = 104).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-250">When converting an IPv4 address to IPv6, add 96 to the CIDR Prefix Length (`8` in the example) to account for the additional `::ffff:` IPv6 prefix (8 + 96 = 104).</span></span> 

```csharp
// To access IPNetwork and IPAddress, add the following namespaces:
// using using System.Net;
// using Microsoft.AspNetCore.HttpOverrides;
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardedHeaders =
        ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    options.KnownNetworks.Add(new IPNetwork(
        IPAddress.Parse("::ffff:10.11.12.1"), 104));
});
```

## <a name="forward-the-scheme-for-linux-and-non-iis-reverse-proxies"></a><span data-ttu-id="d4b5f-251">Do przodu schematu dla systemów Linux i innej niż IIS zwrotnych serwerów proxy</span><span class="sxs-lookup"><span data-stu-id="d4b5f-251">Forward the scheme for Linux and non-IIS reverse proxies</span></span>

<span data-ttu-id="d4b5f-252">Aplikacje, które wywołują <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> i <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> umieścić lokację w pętli nieskończonej w przypadku wdrażania usługi Azure Linux App Service, Azure Linux maszyny wirtualnej (VM), lub za każdy inny odwrotnego serwera proxy, oprócz usług IIS.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-252">Apps that call <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> and <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> put a site into an infinite loop if deployed to an Azure Linux App Service, Azure Linux virtual machine (VM), or behind any other reverse proxy besides IIS.</span></span> <span data-ttu-id="d4b5f-253">Protokół TLS jest zakończony przez zwrotny serwer proxy i Kestrel nie jest powiadomieni schematu właściwe żądanie.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-253">TLS is terminated by the reverse proxy, and Kestrel isn't made aware of the correct request scheme.</span></span> <span data-ttu-id="d4b5f-254">OAuth i OIDC również zakończyć się niepowodzeniem w tej konfiguracji ponieważ generują niepoprawne przekierowania.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-254">OAuth and OIDC also fail in this configuration because they generate incorrect redirects.</span></span> <span data-ttu-id="d4b5f-255"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> dodaje i konfiguruje przekazany oprogramowania pośredniczącego nagłówków, gdy działające poza usługą IIS, ale brak pasującego automatycznej konfiguracji dla systemu Linux (Integracja Apache i Nginx).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-255"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> adds and configures Forwarded Headers Middleware when running behind IIS, but there's no matching automatic configuration for Linux (Apache or Nginx integration).</span></span>

<span data-ttu-id="d4b5f-256">Aby przesłać dalej schemat z serwera proxy w scenariuszach innej niż IIS, należy dodać i skonfigurować przekazywane oprogramowania pośredniczącego nagłówków.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-256">To forward the scheme from the proxy in non-IIS scenarios, add and configure Forwarded Headers Middleware.</span></span> <span data-ttu-id="d4b5f-257">W `Startup.ConfigureServices`, użyj następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="d4b5f-257">In `Startup.ConfigureServices`, use the following code:</span></span>

```csharp
// using Microsoft.AspNetCore.HttpOverrides;

if (string.Equals(
    Environment.GetEnvironmentVariable("ASPNETCORE_FORWARDEDHEADERS_ENABLED"), 
    "true", StringComparison.OrdinalIgnoreCase))
{
    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = ForwardedHeaders.XForwardedFor | 
            ForwardedHeaders.XForwardedProto;
        // Only loopback proxies are allowed by default.
        // Clear that restriction because forwarders are enabled by explicit 
        // configuration.
        options.KnownNetworks.Clear();
        options.KnownProxies.Clear();
    });
}
```

## <a name="troubleshoot"></a><span data-ttu-id="d4b5f-258">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="d4b5f-258">Troubleshoot</span></span>

<span data-ttu-id="d4b5f-259">Gdy nagłówki nie są przekazywane, zgodnie z oczekiwaniami, Włącz [rejestrowania](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-259">When headers aren't forwarded as expected, enable [logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="d4b5f-260">Jeśli dzienniki nie zapewniają wystarczające informacje, aby rozwiązać problem, wyliczanie nagłówków żądania odebranego przez serwer.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-260">If the logs don't provide sufficient information to troubleshoot the problem, enumerate the request headers received by the server.</span></span> <span data-ttu-id="d4b5f-261">Oprogramowanie pośredniczące wbudowane umożliwia zapisania nagłówki żądania odpowiedzi aplikacji lub dziennika nagłówków.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-261">Use inline middleware to write request headers to an app response or log the headers.</span></span> 

<span data-ttu-id="d4b5f-262">Do zapisania nagłówki odpowiedzi aplikacji, należy umieścić następujące oprogramowanie pośredniczące terminalu wbudowane natychmiast po wywołaniu <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="d4b5f-262">To write the headers to the app's response, place the following terminal inline middleware immediately after the call to <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> in `Startup.Configure`:</span></span>

```csharp
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
});
```

<span data-ttu-id="d4b5f-263">Możesz zapisywać dzienniki zamiast treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-263">You can write to logs instead of the response body.</span></span> <span data-ttu-id="d4b5f-264">Zapisywanie dzienników zazwyczaj umożliwia witryny funkcji podczas debugowania.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-264">Writing to logs allows the site to function normally while debugging.</span></span>

<span data-ttu-id="d4b5f-265">Zapisywanie dzienników, a nie treści odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="d4b5f-265">To write logs rather than to the response body:</span></span>

* <span data-ttu-id="d4b5f-266">Wstrzykiwanie `ILogger<Startup>` do `Startup` klasy zgodnie z opisem w [twórz dzienniki w uruchamiania](xref:fundamentals/logging/index#create-logs-in-startup).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-266">Inject `ILogger<Startup>` into the `Startup` class as described in [Create logs in Startup](xref:fundamentals/logging/index#create-logs-in-startup).</span></span>
* <span data-ttu-id="d4b5f-267">Umieść następujące oprogramowanie pośredniczące wbudowane natychmiast po wywołaniu <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-267">Place the following inline middleware immediately after the call to <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> in `Startup.Configure`.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // Request method, scheme, and path
    _logger.LogDebug("Request Method: {METHOD}", context.Request.Method);
    _logger.LogDebug("Request Scheme: {SCHEME}", context.Request.Scheme);
    _logger.LogDebug("Request Path: {PATH}", context.Request.Path);

    // Headers
    foreach (var header in context.Request.Headers)
    {
        _logger.LogDebug("Header: {KEY}: {VALUE}", header.Key, header.Value);
    }

    // Connection: RemoteIp
    _logger.LogDebug("Request RemoteIp: {REMOTE_IP_ADDRESS}", 
        context.Connection.RemoteIpAddress);

    await next();
});
```

<span data-ttu-id="d4b5f-268">Podczas przetwarzania `X-Forwarded-{For|Proto|Host}` wartości są przenoszone do `X-Original-{For|Proto|Host}`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-268">When processed, `X-Forwarded-{For|Proto|Host}` values are moved to `X-Original-{For|Proto|Host}`.</span></span> <span data-ttu-id="d4b5f-269">Jeśli dany nagłówek jest wiele wartości, przekazywane oprogramowania pośredniczącego nagłówki przetwarza nagłówków w odwrotnej kolejności od prawej do lewej.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-269">If there are multiple values in a given header, Forwarded Headers Middleware processes headers in reverse order from right to left.</span></span> <span data-ttu-id="d4b5f-270">Wartość domyślna `ForwardLimit` jest `1` (jeden) jest przetwarzany tylko wartość prawej z nagłówków, chyba że wartość `ForwardLimit` wzrasta.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-270">The default `ForwardLimit` is `1` (one), so only the rightmost value from the headers is processed unless the value of `ForwardLimit` is increased.</span></span>

<span data-ttu-id="d4b5f-271">Oryginalny zdalny adres IP żądania musi pasować do wpisu w `KnownProxies` lub `KnownNetworks` Wyświetla przed nagłówki przekazywane są przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-271">The request's original remote IP must match an entry in the `KnownProxies` or `KnownNetworks` lists before forwarded headers are processed.</span></span> <span data-ttu-id="d4b5f-272">Ogranicza to nagłówek fałszowania przez nie akceptuje usług przesyłania dalej z niezaufanych serwerów proxy.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-272">This limits header spoofing by not accepting forwarders from untrusted proxies.</span></span> <span data-ttu-id="d4b5f-273">Po wykryciu nieznany proxy rejestrowania wskazuje adres serwera proxy:</span><span class="sxs-lookup"><span data-stu-id="d4b5f-273">When an unknown proxy is detected, logging indicates the address of the proxy:</span></span>

```console
September 20th 2018, 15:49:44.168 Unknown proxy: 10.0.0.100:54321
```

<span data-ttu-id="d4b5f-274">W powyższym przykładzie 10.0.0.100 jest serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-274">In the preceding example, 10.0.0.100 is a proxy server.</span></span> <span data-ttu-id="d4b5f-275">Jeśli serwer jest zaufany serwer proxy, należy dodać adres IP serwera do `KnownProxies` (lub zaufanej sieci, aby dodać `KnownNetworks`) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-275">If the server is a trusted proxy, add the server's IP address to `KnownProxies` (or add a trusted network to `KnownNetworks`) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d4b5f-276">Aby uzyskać więcej informacji, zobacz [opcje przekazywane oprogramowania pośredniczącego nagłówki](#forwarded-headers-middleware-options) sekcji.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-276">For more information, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options) section.</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

> [!IMPORTANT]
> <span data-ttu-id="d4b5f-277">Zezwalaj tylko zaufanych serwerów proxy i sieci do przekazywania nagłówków.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-277">Only allow trusted proxies and networks to forward headers.</span></span> <span data-ttu-id="d4b5f-278">W przeciwnym razie [fałszowanie adresów IP](https://www.iplocation.net/ip-spoofing) ataki są możliwe.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-278">Otherwise, [IP spoofing](https://www.iplocation.net/ip-spoofing) attacks are possible.</span></span>

## <a name="certificate-forwarding"></a><span data-ttu-id="d4b5f-279">Przekazywanie certyfikatu</span><span class="sxs-lookup"><span data-stu-id="d4b5f-279">Certificate forwarding</span></span> 

### <a name="on-azure"></a><span data-ttu-id="d4b5f-280">Na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="d4b5f-280">On Azure</span></span>

<span data-ttu-id="d4b5f-281">Zobacz [dokumentacji platformy Azure](/azure/app-service/app-service-web-configure-tls-mutual-auth) do konfigurowania usługi Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-281">See the [Azure documentation](/azure/app-service/app-service-web-configure-tls-mutual-auth) to configure Azure Web Apps.</span></span> <span data-ttu-id="d4b5f-282">W swojej aplikacji `Startup.Configure` metody, Dodaj następujący kod przed wywołaniem do `app.UseAuthentication();`:</span><span class="sxs-lookup"><span data-stu-id="d4b5f-282">In your app's `Startup.Configure` method, add the following code before the call to `app.UseAuthentication();`:</span></span>

```csharp
app.UseCertificateForwarding();
```

<span data-ttu-id="d4b5f-283">Należy również skonfigurować oprogramowanie pośredniczące przekazywania certyfikatu, aby określić nazwę nagłówka, która korzysta z platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-283">You'll also need to configure the Certificate Forwarding middleware to specify the header name that Azure uses.</span></span> <span data-ttu-id="d4b5f-284">W swojej aplikacji `Startup.ConfigureServices` metody, Dodaj następujący kod, aby skonfigurować nagłówek, w którym oprogramowanie pośredniczące tworzy certyfikat:</span><span class="sxs-lookup"><span data-stu-id="d4b5f-284">In your app's `Startup.ConfigureServices` method, add the following code to configure the header from which the middleware builds a certificate:</span></span>

```csharp
services.AddCertificateForwarding(options =>
    options.CertificateHeader = "X-ARR-ClientCert");
```

### <a name="with-other-web-proxies"></a><span data-ttu-id="d4b5f-285">Za pomocą innych serwerów proxy sieci web</span><span class="sxs-lookup"><span data-stu-id="d4b5f-285">With other web proxies</span></span>

<span data-ttu-id="d4b5f-286">Jeśli używasz serwera proxy, który nie jest lub platformy Azure Web Apps Routing żądań aplikacji IIS, należy skonfigurować serwer proxy do przekazywania certyfikatu otrzymanego w nagłówku HTTP.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-286">If you're using a proxy that isn't IIS or Azure's Web Apps Application Request Routing, configure your proxy to forward the certificate it received in an HTTP header.</span></span> <span data-ttu-id="d4b5f-287">W swojej aplikacji `Startup.Configure` metody, Dodaj następujący kod przed wywołaniem do `app.UseAuthentication();`:</span><span class="sxs-lookup"><span data-stu-id="d4b5f-287">In your app's `Startup.Configure` method, add the following code before the call to `app.UseAuthentication();`:</span></span>

```csharp
app.UseCertificateForwarding();
```

<span data-ttu-id="d4b5f-288">Należy również skonfigurować oprogramowanie pośredniczące przekazywania certyfikatu, aby określić nazwę nagłówka.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-288">You'll also need to configure the Certificate Forwarding middleware to specify the header name.</span></span> <span data-ttu-id="d4b5f-289">W swojej aplikacji `Startup.ConfigureServices` metody, Dodaj następujący kod, aby skonfigurować nagłówek, w którym oprogramowanie pośredniczące tworzy certyfikat:</span><span class="sxs-lookup"><span data-stu-id="d4b5f-289">In your app's `Startup.ConfigureServices` method, add the following code to configure the header from which the middleware builds a certificate:</span></span>

```csharp
services.AddCertificateForwarding(options =>
    options.CertificateHeader = "YOUR_CERTIFICATE_HEADER_NAME");
```

<span data-ttu-id="d4b5f-290">Ponadto jeśli serwer proxy jest coś innego niż base64 kodowanie certyfikatu (tak jak w przypadku przy użyciu serwera Nginx) ustaw `HeaderConverter` opcji.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-290">Finally, if the proxy is doing something other than base64 encoding the certificate (as is the case with Nginx), set the `HeaderConverter` option.</span></span> <span data-ttu-id="d4b5f-291">Rozważmy następujący przykład w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d4b5f-291">Consider the following example in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddCertificateForwarding(options =>
{
    options.CertificateHeader = "YOUR_CUSTOM_HEADER_NAME";
    options.HeaderConverter = (headerValue) => 
    {
        var clientCertificate = 
           /* some conversion logic to create an X509Certificate2 */
        return clientCertificate;
    }
});
```

## <a name="additional-resources"></a><span data-ttu-id="d4b5f-292">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d4b5f-292">Additional resources</span></span>

* <xref:host-and-deploy/web-farm>
* [<span data-ttu-id="d4b5f-293">Microsoft Security Advisory CVE-2018-0787: Platforma ASP.NET Core podniesienie uprawnień</span><span class="sxs-lookup"><span data-stu-id="d4b5f-293">Microsoft Security Advisory CVE-2018-0787: ASP.NET Core Elevation Of Privilege Vulnerability</span></span>](https://github.com/aspnet/Announcements/issues/295)
