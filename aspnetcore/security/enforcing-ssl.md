---
title: Wymuszanie protokołu HTTPS w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak wymagać protokołu HTTPS/TLS w aplikacji internetowej ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2019
uid: security/enforcing-ssl
ms.openlocfilehash: 43f3abfa4bc311ed246f6f2585d522661e492039
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447155"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="1f8a2-103">Wymuszanie protokołu HTTPS w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1f8a2-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="1f8a2-104">Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1f8a2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1f8a2-105">W tym dokumencie przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-105">This document shows how to:</span></span>

* <span data-ttu-id="1f8a2-106">Wymagaj protokołu HTTPS dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="1f8a2-107">Przekierowuj wszystkie żądania HTTP do protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="1f8a2-108">Żaden interfejs API nie może zapobiec wysyłaniu poufnych danych przez klienta do pierwszego żądania.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> ## <a name="api-projects"></a><span data-ttu-id="1f8a2-109">Projekty interfejsu API</span><span class="sxs-lookup"><span data-stu-id="1f8a2-109">API projects</span></span>
>
> <span data-ttu-id="1f8a2-110">**Nie** należy używać [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) w interfejsach API sieci Web, które otrzymują poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-110">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="1f8a2-111">`RequireHttpsAttribute` używa kodów stanu HTTP do przekierowywania przeglądarek z protokołu HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-111">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="1f8a2-112">Klienci interfejsu API nie mogą zrozumieć ani przestrzegać przekierowania z protokołu HTTP do protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-112">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="1f8a2-113">Tacy klienci mogą wysyłać informacje za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-113">Such clients may send information over HTTP.</span></span> <span data-ttu-id="1f8a2-114">Interfejsy API sieci Web powinny:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-114">Web APIs should either:</span></span>
>
> * <span data-ttu-id="1f8a2-115">Nie nasłuchuje na protokole HTTP.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-115">Not listen on HTTP.</span></span>
> * <span data-ttu-id="1f8a2-116">Zamknij połączenie z kodem stanu 400 (złe żądanie) i nie obsługuj żądania.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-116">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>
>
> ## <a name="hsts-and-api-projects"></a><span data-ttu-id="1f8a2-117">HSTS i projekty interfejsu API</span><span class="sxs-lookup"><span data-stu-id="1f8a2-117">HSTS and API projects</span></span>
>
> <span data-ttu-id="1f8a2-118">Domyślne projekty interfejsów API nie obejmują [HSTS](#hsts) , ponieważ HSTS jest ogólnie jedyną instrukcją przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-118">The default API projects don't include [HSTS](#hsts) because HSTS is generally a browser only instruction.</span></span> <span data-ttu-id="1f8a2-119">Inne obiekty wywołujące, takie jak telefony lub aplikacje klasyczne, **nie przestrzegają** instrukcji.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-119">Other callers, such as phone or desktop apps, do **not** obey the instruction.</span></span> <span data-ttu-id="1f8a2-120">Nawet w przeglądarkach pojedyncze uwierzytelnione wywołanie interfejsu API za pośrednictwem protokołu HTTP jest niebezpieczne dla niezabezpieczonych sieci.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-120">Even within browsers, a single authenticated call to an API over HTTP has risks on insecure networks.</span></span> <span data-ttu-id="1f8a2-121">Bezpiecznym podejściem jest skonfigurowanie projektów interfejsu API tylko do nasłuchiwania i odpowiadania za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-121">The secure approach is to configure API projects to only listen to and respond over HTTPS.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

> [!WARNING]
> ## <a name="api-projects"></a><span data-ttu-id="1f8a2-122">Projekty interfejsu API</span><span class="sxs-lookup"><span data-stu-id="1f8a2-122">API projects</span></span>
>
> <span data-ttu-id="1f8a2-123">**Nie** należy używać [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) w interfejsach API sieci Web, które otrzymują poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-123">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="1f8a2-124">`RequireHttpsAttribute` używa kodów stanu HTTP do przekierowywania przeglądarek z protokołu HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-124">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="1f8a2-125">Klienci interfejsu API nie mogą zrozumieć ani przestrzegać przekierowania z protokołu HTTP do protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-125">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="1f8a2-126">Tacy klienci mogą wysyłać informacje za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-126">Such clients may send information over HTTP.</span></span> <span data-ttu-id="1f8a2-127">Interfejsy API sieci Web powinny:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-127">Web APIs should either:</span></span>
>
> * <span data-ttu-id="1f8a2-128">Nie nasłuchuje na protokole HTTP.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-128">Not listen on HTTP.</span></span>
> * <span data-ttu-id="1f8a2-129">Zamknij połączenie z kodem stanu 400 (złe żądanie) i nie obsługuj żądania.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-129">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

::: moniker-end

## <a name="require-https"></a><span data-ttu-id="1f8a2-130">Wymagaj protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="1f8a2-130">Require HTTPS</span></span>

<span data-ttu-id="1f8a2-131">Zalecamy korzystanie z aplikacji sieci Web ASP.NET Core produkcyjnych:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-131">We recommend that production ASP.NET Core web apps use:</span></span>

* <span data-ttu-id="1f8a2-132">Oprogramowanie pośredniczące do przekierowywania HTTPS (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) do przekierowywania żądań HTTP do protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-132">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) to redirect HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="1f8a2-133">HSTSe oprogramowanie pośredniczące ([UseHsts](#http-strict-transport-security-protocol-hsts)) do wysyłania nagłówków HTTP Strict Transport Security Protocol (HSTS) do klientów.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-133">HSTS Middleware ([UseHsts](#http-strict-transport-security-protocol-hsts)) to send HTTP Strict Transport Security Protocol (HSTS) headers to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="1f8a2-134">Aplikacje wdrożone w konfiguracji zwrotnego serwera proxy pozwalają serwerowi proxy obsługiwać zabezpieczenia połączeń (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="1f8a2-134">Apps deployed in a reverse proxy configuration allow the proxy to handle connection security (HTTPS).</span></span> <span data-ttu-id="1f8a2-135">Jeśli serwer proxy obsługuje również przekierowywanie przy użyciu protokołu HTTPS, nie ma potrzeby używania oprogramowania pośredniczącego do przekierowania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-135">If the proxy also handles HTTPS redirection, there's no need to use HTTPS Redirection Middleware.</span></span> <span data-ttu-id="1f8a2-136">Jeśli serwer proxy obsługuje również zapisywanie nagłówków HSTS (na przykład [natywnej obsługi HSTS w usługach IIS 10,0 (1709) lub nowszej](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), oprogramowanie pośredniczące HSTS nie jest wymagane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-136">If the proxy server also handles writing HSTS headers (for example, [native HSTS support in IIS 10.0 (1709) or later](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS Middleware isn't required by the app.</span></span> <span data-ttu-id="1f8a2-137">Aby uzyskać więcej informacji, zobacz [rezygnacja z protokołu HTTPS/HSTS podczas tworzenia projektu](#opt-out-of-httpshsts-on-project-creation).</span><span class="sxs-lookup"><span data-stu-id="1f8a2-137">For more information, see [Opt-out of HTTPS/HSTS on project creation](#opt-out-of-httpshsts-on-project-creation).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="1f8a2-138">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="1f8a2-138">UseHttpsRedirection</span></span>

<span data-ttu-id="1f8a2-139">Następujący kod wywołuje `UseHttpsRedirection` w klasie `Startup`:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-139">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=13)]

::: moniker-end

<span data-ttu-id="1f8a2-140">Poprzedni wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-140">The preceding highlighted code:</span></span>

* <span data-ttu-id="1f8a2-141">Używa domyślnej [HttpsRedirectionOptions. RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="1f8a2-141">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="1f8a2-142">Używa domyślnego [HttpsRedirectionOptions. HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), chyba że jest zastępowany przez zmienną środowiskową `ASPNETCORE_HTTPS_PORT` lub [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="1f8a2-142">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="1f8a2-143">Zalecamy używanie tymczasowych przekierowań zamiast trwałych przekierowań.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-143">We recommend using temporary redirects rather than permanent redirects.</span></span> <span data-ttu-id="1f8a2-144">Buforowanie łączy może spowodować niestabilne zachowanie w środowiskach deweloperskich.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-144">Link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="1f8a2-145">Jeśli wolisz wysyłać kod stanu stałego przekierowania, gdy aplikacja znajduje się w środowisku innym niż programowanie, zobacz sekcję [Konfigurowanie stałych przekierowań w produkcji](#configure-permanent-redirects-in-production) .</span><span class="sxs-lookup"><span data-stu-id="1f8a2-145">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, see the [Configure permanent redirects in production](#configure-permanent-redirects-in-production) section.</span></span> <span data-ttu-id="1f8a2-146">Zalecamy używanie [HSTS](#http-strict-transport-security-protocol-hsts) do sygnalizowania klientów, że tylko zabezpieczone żądania zasobów powinny być wysyłane do aplikacji (tylko w środowisku produkcyjnym).</span><span class="sxs-lookup"><span data-stu-id="1f8a2-146">We recommend using [HSTS](#http-strict-transport-security-protocol-hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

### <a name="port-configuration"></a><span data-ttu-id="1f8a2-147">Konfiguracja portu</span><span class="sxs-lookup"><span data-stu-id="1f8a2-147">Port configuration</span></span>

<span data-ttu-id="1f8a2-148">Aby przekierować niezabezpieczone żądanie do protokołu HTTPS, Port musi być dostępny dla oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-148">A port must be available for the middleware to redirect an insecure request to HTTPS.</span></span> <span data-ttu-id="1f8a2-149">Jeśli port nie jest dostępny:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-149">If no port is available:</span></span>

* <span data-ttu-id="1f8a2-150">Przekierowanie do protokołu HTTPS nie jest wykonywane.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-150">Redirection to HTTPS doesn't occur.</span></span>
* <span data-ttu-id="1f8a2-151">Oprogramowanie pośredniczące rejestruje ostrzeżenie "nie można określić portu HTTPS do przekierowania".</span><span class="sxs-lookup"><span data-stu-id="1f8a2-151">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

<span data-ttu-id="1f8a2-152">Określ port HTTPS przy użyciu dowolnej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-152">Specify the HTTPS port using any of the following approaches:</span></span>

* <span data-ttu-id="1f8a2-153">Ustaw [HttpsRedirectionOptions. HttpsPort](#options).</span><span class="sxs-lookup"><span data-stu-id="1f8a2-153">Set [HttpsRedirectionOptions.HttpsPort](#options).</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="1f8a2-154">Ustaw [ustawienie hosta](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#https_port)`https_port`:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-154">Set the `https_port` [host setting](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#https_port):</span></span>

  * <span data-ttu-id="1f8a2-155">W obszarze Konfiguracja hosta.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-155">In host configuration.</span></span>
  * <span data-ttu-id="1f8a2-156">Ustawiając zmienną środowiskową `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-156">By setting the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
  * <span data-ttu-id="1f8a2-157">Dodając wpis najwyższego poziomu w pliku *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-157">By adding a top-level entry in *appsettings.json*:</span></span>

    [!code-json[](enforcing-ssl/sample-snapshot/3.x/appsettings.json?highlight=2)]

* <span data-ttu-id="1f8a2-158">Wskaż port z bezpiecznym schematem przy użyciu [zmiennej środowiskowej ASPNETCORE_URLS](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#urls).</span><span class="sxs-lookup"><span data-stu-id="1f8a2-158">Indicate a port with the secure scheme using the [ASPNETCORE_URLS environment variable](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#urls).</span></span> <span data-ttu-id="1f8a2-159">Zmienna środowiskowa służy do konfigurowania serwera.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-159">The environment variable configures the server.</span></span> <span data-ttu-id="1f8a2-160">Oprogramowanie pośredniczące pośrednio wykrywa port HTTPS za pośrednictwem <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-160">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="1f8a2-161">Takie podejście nie działa w przypadku wdrożeń zwrotnych serwerów proxy.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-161">This approach doesn't work in reverse proxy deployments.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

* <span data-ttu-id="1f8a2-162">Ustaw [ustawienie hosta](xref:fundamentals/host/web-host#https-port)`https_port`:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-162">Set the `https_port` [host setting](xref:fundamentals/host/web-host#https-port):</span></span>

  * <span data-ttu-id="1f8a2-163">W obszarze Konfiguracja hosta.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-163">In host configuration.</span></span>
  * <span data-ttu-id="1f8a2-164">Ustawiając zmienną środowiskową `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-164">By setting the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
  * <span data-ttu-id="1f8a2-165">Dodając wpis najwyższego poziomu w pliku *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-165">By adding a top-level entry in *appsettings.json*:</span></span>

    [!code-json[](enforcing-ssl/sample-snapshot/2.x/appsettings.json?highlight=2)]

* <span data-ttu-id="1f8a2-166">Wskaż port z bezpiecznym schematem przy użyciu [zmiennej środowiskowej ASPNETCORE_URLS](xref:fundamentals/host/web-host#server-urls).</span><span class="sxs-lookup"><span data-stu-id="1f8a2-166">Indicate a port with the secure scheme using the [ASPNETCORE_URLS environment variable](xref:fundamentals/host/web-host#server-urls).</span></span> <span data-ttu-id="1f8a2-167">Zmienna środowiskowa służy do konfigurowania serwera.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-167">The environment variable configures the server.</span></span> <span data-ttu-id="1f8a2-168">Oprogramowanie pośredniczące pośrednio wykrywa port HTTPS za pośrednictwem <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-168">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="1f8a2-169">Takie podejście nie działa w przypadku wdrożeń zwrotnych serwerów proxy.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-169">This approach doesn't work in reverse proxy deployments.</span></span>

::: moniker-end

* <span data-ttu-id="1f8a2-170">W obszarze programowanie Ustaw adres URL HTTPS w pliku *profilu launchsettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-170">In development, set an HTTPS URL in *launchsettings.json*.</span></span> <span data-ttu-id="1f8a2-171">Włącz protokół HTTPS, gdy zostanie użyta IIS Express.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-171">Enable HTTPS when IIS Express is used.</span></span>

* <span data-ttu-id="1f8a2-172">Skonfiguruj punkt końcowy adresu URL HTTPS dla wdrożenia publicznej krawędzi serwera [Kestrel](xref:fundamentals/servers/kestrel) lub [http. sys](xref:fundamentals/servers/httpsys) .</span><span class="sxs-lookup"><span data-stu-id="1f8a2-172">Configure an HTTPS URL endpoint for a public-facing edge deployment of [Kestrel](xref:fundamentals/servers/kestrel) server or [HTTP.sys](xref:fundamentals/servers/httpsys) server.</span></span> <span data-ttu-id="1f8a2-173">Aplikacja używa tylko **jednego portu HTTPS** .</span><span class="sxs-lookup"><span data-stu-id="1f8a2-173">Only **one HTTPS port** is used by the app.</span></span> <span data-ttu-id="1f8a2-174">Oprogramowanie pośredniczące odnajduje port za pośrednictwem <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-174">The middleware discovers the port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span>

> [!NOTE]
> <span data-ttu-id="1f8a2-175">Gdy aplikacja jest uruchamiana w konfiguracji zwrotnego serwera proxy, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> nie jest dostępna.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-175">When an app is run in a reverse proxy configuration, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> isn't available.</span></span> <span data-ttu-id="1f8a2-176">Ustaw port przy użyciu jednej z innych metod opisanych w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-176">Set the port using one of the other approaches described in this section.</span></span>

### <a name="edge-deployments"></a><span data-ttu-id="1f8a2-177">Wdrożenia brzegowe</span><span class="sxs-lookup"><span data-stu-id="1f8a2-177">Edge deployments</span></span> 

<span data-ttu-id="1f8a2-178">Gdy Kestrel lub HTTP. sys jest używany jako publiczny serwer graniczny, Kestrel lub HTTP. sys musi być skonfigurowany do nasłuchiwania na obu:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-178">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>

* <span data-ttu-id="1f8a2-179">Bezpieczny port do przekierowania przez klienta (zazwyczaj 443 w środowisku produkcyjnym i 5001 podczas tworzenia).</span><span class="sxs-lookup"><span data-stu-id="1f8a2-179">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
* <span data-ttu-id="1f8a2-180">Niezabezpieczony port (zazwyczaj 80 w środowisku produkcyjnym i 5000 podczas tworzenia).</span><span class="sxs-lookup"><span data-stu-id="1f8a2-180">The insecure port (typically, 80 in production and 5000 in development).</span></span>

<span data-ttu-id="1f8a2-181">Niezabezpieczony Port musi być dostępny dla klienta, aby aplikacja mogła odebrać niezabezpieczone żądanie i przekierować klienta do bezpiecznego portu.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-181">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect the client to the secure port.</span></span>

<span data-ttu-id="1f8a2-182">Aby uzyskać więcej informacji, zobacz [Kestrel Endpoint Configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) lub <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-182">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

### <a name="deployment-scenarios"></a><span data-ttu-id="1f8a2-183">Scenariusze wdrażania</span><span class="sxs-lookup"><span data-stu-id="1f8a2-183">Deployment scenarios</span></span>

<span data-ttu-id="1f8a2-184">Wszystkie zapory między klientem a serwerem muszą także mieć otwarte porty komunikacyjne dla ruchu.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-184">Any firewall between the client and server must also have communication ports open for traffic.</span></span>

<span data-ttu-id="1f8a2-185">Jeśli żądania są przekazywane w konfiguracji zwrotnego serwera proxy, przed wywołaniem oprogramowania pośredniczącego do przekierowywania HTTPS Użyj [oprogramowania pośredniczącego](xref:host-and-deploy/proxy-load-balancer) .</span><span class="sxs-lookup"><span data-stu-id="1f8a2-185">If requests are forwarded in a reverse proxy configuration, use [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) before calling HTTPS Redirection Middleware.</span></span> <span data-ttu-id="1f8a2-186">Przesłane nagłówki — oprogramowanie pośredniczące aktualizuje `Request.Scheme`przy użyciu nagłówka `X-Forwarded-Proto`.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-186">Forwarded Headers Middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header.</span></span> <span data-ttu-id="1f8a2-187">Oprogramowanie pośredniczące umożliwia poprawne działanie identyfikatorów URI przekierowania i innych zasad zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-187">The middleware permits redirect URIs and other security policies to work correctly.</span></span> <span data-ttu-id="1f8a2-188">Gdy przekazane nagłówki nie są używane, aplikacja zaplecza może nie otrzymać poprawnego schematu i zakończyć działania w pętli przekierowania.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-188">When Forwarded Headers Middleware isn't used, the backend app might not receive the correct scheme and end up in a redirect loop.</span></span> <span data-ttu-id="1f8a2-189">Typowy komunikat o błędzie użytkownika końcowego polega na tym, że wystąpiły zbyt wiele przekierowań.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-189">A common end user error message is that too many redirects have occurred.</span></span>

<span data-ttu-id="1f8a2-190">Podczas wdrażania programu w celu Azure App Service postępuj zgodnie ze wskazówkami podanymi w [samouczku: Powiąż istniejący niestandardowy certyfikat SSL z usługą Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="1f8a2-190">When deploying to Azure App Service, follow the guidance in [Tutorial: Bind an existing custom SSL certificate to Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

### <a name="options"></a><span data-ttu-id="1f8a2-191">Opcje</span><span class="sxs-lookup"><span data-stu-id="1f8a2-191">Options</span></span>

<span data-ttu-id="1f8a2-192">Następujący wyróżniony kod wywołuje [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) , aby skonfigurować opcje oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-192">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end


<span data-ttu-id="1f8a2-193">Wywoływanie `AddHttpsRedirection` jest niezbędne tylko do zmiany wartości `HttpsPort` lub `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-193">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="1f8a2-194">Poprzedni wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-194">The preceding highlighted code:</span></span>

* <span data-ttu-id="1f8a2-195">Ustawia [HttpsRedirectionOptions. RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) na <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, która jest wartością domyślną.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-195">Sets [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) to <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, which is the default value.</span></span> <span data-ttu-id="1f8a2-196">Użyj pól klasy <xref:Microsoft.AspNetCore.Http.StatusCodes>, aby przydziały `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-196">Use the fields of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="1f8a2-197">Ustawia port HTTPS na 5001.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-197">Sets the HTTPS port to 5001.</span></span>

#### <a name="configure-permanent-redirects-in-production"></a><span data-ttu-id="1f8a2-198">Konfigurowanie stałych przekierowań w środowisku produkcyjnym</span><span class="sxs-lookup"><span data-stu-id="1f8a2-198">Configure permanent redirects in production</span></span>

<span data-ttu-id="1f8a2-199">Ustawienia domyślne oprogramowania pośredniczącego do wysyłania [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) z wszystkimi przekierowaniami.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-199">The middleware defaults to sending a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) with all redirects.</span></span> <span data-ttu-id="1f8a2-200">Jeśli wolisz wysyłać kod stanu stałego przekierowania, gdy aplikacja znajduje się w środowisku nieprogramistycznym, Zapakuj konfigurację opcji oprogramowania pośredniczącego w ramach sprawdzania warunkowego dla środowiska nieprogramistycznego.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-200">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, wrap the middleware options configuration in a conditional check for a non-Development environment.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1f8a2-201">Podczas konfigurowania usług w programie *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-201">When configuring services in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IWebHostEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="1f8a2-202">Podczas konfigurowania usług w programie *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-202">When configuring services in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IHostingEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

::: moniker-end


## <a name="https-redirection-middleware-alternative-approach"></a><span data-ttu-id="1f8a2-203">Alternatywne podejście do przekierowania protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="1f8a2-203">HTTPS Redirection Middleware alternative approach</span></span>

<span data-ttu-id="1f8a2-204">Alternatywą dla korzystania z oprogramowania pośredniczącego do przekierowywania HTTPS (`UseHttpsRedirection`) jest użycie oprogramowania pośredniczącego do ponownego zapisywania adresów URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="1f8a2-204">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="1f8a2-205">`AddRedirectToHttps` można również ustawić kod stanu i port, gdy przekierowanie zostanie wykonane.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-205">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="1f8a2-206">Aby uzyskać więcej informacji, zobacz Ponowne [Zapisywanie oprogramowania pośredniczącego w adresie URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="1f8a2-206">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="1f8a2-207">Podczas przekierowywania do protokołu HTTPS bez wymagania dotyczącego dodatkowych reguł przekierowywania zalecamy używanie oprogramowania pośredniczącego do przekierowania protokołu HTTPS (`UseHttpsRedirection`) opisanej w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-207">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="1f8a2-208">Protokół HTTP Strict Transport Security Protocol (HSTS)</span><span class="sxs-lookup"><span data-stu-id="1f8a2-208">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="1f8a2-209">Na [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [http Strict Transport Security (HSTS)](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html) jest opcjonalnym ulepszeniem zabezpieczeń, określonym przez aplikację sieci Web przy użyciu nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-209">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="1f8a2-210">Gdy [przeglądarka, która obsługuje HSTS,](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html#browser-support) otrzymuje ten nagłówek:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-210">When a [browser that supports HSTS](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html#browser-support) receives this header:</span></span>

* <span data-ttu-id="1f8a2-211">Przeglądarka przechowuje konfigurację dla domeny, która uniemożliwia wysyłanie komunikacji za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-211">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="1f8a2-212">Przeglądarka wymusza całą komunikację za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-212">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="1f8a2-213">Przeglądarka uniemożliwia użytkownikowi korzystanie z niezaufanych lub nieprawidłowych certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-213">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="1f8a2-214">Przeglądarka wyłącza wyświetlanie wierszy, które umożliwiają użytkownikowi tymczasowe zaufać temu certyfikatowi.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-214">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="1f8a2-215">Ponieważ HSTS jest wymuszana przez klienta, ma pewne ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-215">Because HSTS is enforced by the client, it has some limitations:</span></span>

* <span data-ttu-id="1f8a2-216">Klient musi obsługiwać HSTS.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-216">The client must support HSTS.</span></span>
* <span data-ttu-id="1f8a2-217">HSTS wymaga co najmniej jednego pomyślnego żądania HTTPS do ustanowienia zasad HSTS.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-217">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="1f8a2-218">Aplikacja musi sprawdzić każde żądanie HTTP i przekierować lub odrzucić żądanie HTTP.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-218">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="1f8a2-219">ASP.NET Core 2,1 i nowsze implementują HSTS z metodą rozszerzenia `UseHsts`.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-219">ASP.NET Core 2.1 and later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="1f8a2-220">Następujący kod wywołuje `UseHsts`, gdy aplikacja nie jest w [trybie deweloperskim](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="1f8a2-220">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=10)]

::: moniker-end

<span data-ttu-id="1f8a2-221">`UseHsts` nie jest zalecana w programowaniu, ponieważ ustawienia HSTS mają wysoką pamięć podręczną przez przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-221">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="1f8a2-222">Domyślnie `UseHsts` wyklucza lokalny adres sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-222">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="1f8a2-223">W przypadku środowisk produkcyjnych, które wdrażają protokół HTTPS po raz pierwszy, należy ustawić początkowy [HstsOptions. maxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) na niewielką wartość przy użyciu jednej z <xref:System.TimeSpan> metod.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-223">For production environments that are implementing HTTPS for the first time, set the initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) to a small value using one of the <xref:System.TimeSpan> methods.</span></span> <span data-ttu-id="1f8a2-224">Ustaw wartość od godziny na nie więcej niż jeden dzień w przypadku konieczności przywrócenia infrastruktury HTTPS do protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-224">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="1f8a2-225">Po upewnieniu się, że trwałość konfiguracji protokołu HTTPS jest stabilna, zwiększ wartość HSTS `max-age`. najczęściej używana wartość wynosi jeden rok.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-225">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS `max-age` value; a commonly used value is one year.</span></span>

<span data-ttu-id="1f8a2-226">Następujący kod:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-226">The following code:</span></span>


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end


* <span data-ttu-id="1f8a2-227">Ustawia parametr wstępnego ładowania nagłówka `Strict-Transport-Security`.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-227">Sets the preload parameter of the `Strict-Transport-Security` header.</span></span> <span data-ttu-id="1f8a2-228">Wstępne ładowanie nie jest częścią [specyfikacji RFC HSTS](https://tools.ietf.org/html/rfc6797), ale jest obsługiwane przez przeglądarki sieci Web do wstępnego ładowania witryn HSTS w przypadku instalacji nowej.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-228">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="1f8a2-229">Aby uzyskać więcej informacji, zobacz [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="1f8a2-229">For more information, see [https://hstspreload.org/](https://hstspreload.org/).</span></span>
* <span data-ttu-id="1f8a2-230">Włącza [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), która stosuje zasady HSTS do hostowania poddomen.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-230">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="1f8a2-231">Jawnie Ustawia parametr `max-age` nagłówka `Strict-Transport-Security` na 60 dni.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-231">Explicitly sets the `max-age` parameter of the `Strict-Transport-Security` header to 60 days.</span></span> <span data-ttu-id="1f8a2-232">Jeśli nie zostanie ustawiona, wartość domyślna to 30 dni.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-232">If not set, defaults to 30 days.</span></span> <span data-ttu-id="1f8a2-233">Aby uzyskać więcej informacji, zobacz [dyrektywa max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1).</span><span class="sxs-lookup"><span data-stu-id="1f8a2-233">For more information, see the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1).</span></span>
* <span data-ttu-id="1f8a2-234">Dodaje `example.com` do listy hostów do wykluczenia.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-234">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="1f8a2-235">`UseHsts` wyklucza następujące hosty sprzężenia zwrotnego:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-235">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="1f8a2-236">`localhost`: adres sprzężenia zwrotnego IPv4.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-236">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="1f8a2-237">`127.0.0.1`: adres sprzężenia zwrotnego IPv4.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-237">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="1f8a2-238">`[::1]`: adres sprzężenia zwrotnego IPv6.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-238">`[::1]` : The IPv6 loopback address.</span></span>

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="1f8a2-239">Zrezygnuj z protokołu HTTPS/HSTS podczas tworzenia projektu</span><span class="sxs-lookup"><span data-stu-id="1f8a2-239">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="1f8a2-240">W niektórych scenariuszach usługi zaplecza, w których zabezpieczenia połączeń są obsługiwane na publicznej krawędzi sieci, skonfigurowanie zabezpieczeń połączeń w każdym węźle nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-240">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="1f8a2-241">Aplikacje sieci Web, które są generowane na podstawie szablonów w programie Visual Studio lub za pomocą polecenia [dotnet New](/dotnet/core/tools/dotnet-new) , umożliwiają [przekierowania https](#require-https) i [HSTS](#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="1f8a2-241">Web apps that are generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require-https) and [HSTS](#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="1f8a2-242">W przypadku wdrożeń, które nie wymagają tych scenariuszy, można zrezygnować z protokołu HTTPS/HSTS podczas tworzenia aplikacji na podstawie szablonu.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-242">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="1f8a2-243">Aby zrezygnować z protokołu HTTPS/HSTS:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-243">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="1f8a2-244">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1f8a2-244">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="1f8a2-245">Usuń zaznaczenie pola wyboru **Konfiguruj dla protokołu HTTPS** .</span><span class="sxs-lookup"><span data-stu-id="1f8a2-245">Uncheck the **Configure for HTTPS** check box.</span></span>

::: moniker range=">= aspnetcore-3.0"

![Okno dialogowe nowe ASP.NET Core aplikacji sieci Web z niezaznaczonym polem wyboru Konfiguruj dla protokołu HTTPS.](enforcing-ssl/_static/out-vs2019.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

![Okno dialogowe nowe ASP.NET Core aplikacji sieci Web z niezaznaczonym polem wyboru Konfiguruj dla protokołu HTTPS.](enforcing-ssl/_static/out.png)

::: moniker-end


# <a name="net-core-cli"></a>[<span data-ttu-id="1f8a2-248">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1f8a2-248">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="1f8a2-249">Użyj opcji `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-249">Use the `--no-https` option.</span></span> <span data-ttu-id="1f8a2-250">Na przykład</span><span class="sxs-lookup"><span data-stu-id="1f8a2-250">For example</span></span>

```dotnetcli
dotnet new webapp --no-https
```

---

<a name="trust"></a>

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a><span data-ttu-id="1f8a2-251">Ufaj certyfikatowi Deweloperskiemu protokołu HTTPS ASP.NET Core w systemach Windows i macOS</span><span class="sxs-lookup"><span data-stu-id="1f8a2-251">Trust the ASP.NET Core HTTPS development certificate on Windows and macOS</span></span>

<span data-ttu-id="1f8a2-252">Zestaw .NET Core SDK zawiera certyfikat programistyczny HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-252">The .NET Core SDK includes an HTTPS development certificate.</span></span> <span data-ttu-id="1f8a2-253">Certyfikat jest instalowany w ramach pierwszego uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-253">The certificate is installed as part of the first-run experience.</span></span> <span data-ttu-id="1f8a2-254">Na przykład `dotnet --info` generuje odmianę następujących danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-254">For example, `dotnet --info` produces a variation of the following output:</span></span>

```
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

<span data-ttu-id="1f8a2-255">Zainstalowanie zestaw .NET Core SDK instaluje certyfikat deweloperski ASP.NET Core HTTPS do magazynu certyfikatów użytkownika lokalnego.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-255">Installing the .NET Core SDK installs the ASP.NET Core HTTPS development certificate to the local user certificate store.</span></span> <span data-ttu-id="1f8a2-256">Certyfikat został zainstalowany, ale nie jest zaufany.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-256">The certificate has been installed, but it's not trusted.</span></span> <span data-ttu-id="1f8a2-257">Aby zaufać certyfikatowi, wykonaj jednorazowy krok w celu uruchomienia narzędzia dotnet `dev-certs`:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-257">To trust the certificate, perform the one-time step to run the dotnet `dev-certs` tool:</span></span>

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="1f8a2-258">Następujące polecenie zapewnia pomoc dotyczącą narzędzia `dev-certs`:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-258">The following command provides help on the `dev-certs` tool:</span></span>

```dotnetcli
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="1f8a2-259">Jak skonfigurować certyfikat dewelopera dla platformy Docker</span><span class="sxs-lookup"><span data-stu-id="1f8a2-259">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="1f8a2-260">Zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/6199)w serwisie GitHub.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-260">See [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6199).</span></span>

<a name="wsl"></a>

## <a name="trust-https-certificate-from-windows-subsystem-for-linux"></a><span data-ttu-id="1f8a2-261">Ufaj certyfikatowi HTTPS z podsystemu Windows dla systemu Linux</span><span class="sxs-lookup"><span data-stu-id="1f8a2-261">Trust HTTPS certificate from Windows Subsystem for Linux</span></span>

<span data-ttu-id="1f8a2-262">Podsystem Windows dla systemu Linux (WSL) generuje certyfikat z podpisem własnym HTTPS. Aby skonfigurować magazyn certyfikatów systemu Windows w celu zaufać certyfikatowi WSL:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-262">The Windows Subsystem for Linux (WSL) generates an HTTPS self-signed cert. To configure the Windows certificate store to trust the WSL certificate:</span></span>

* <span data-ttu-id="1f8a2-263">Uruchom następujące polecenie, aby wyeksportować certyfikat wygenerowany przez WSL: `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`</span><span class="sxs-lookup"><span data-stu-id="1f8a2-263">Run the following command to export the WSL-generated certificate: `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`</span></span>
* <span data-ttu-id="1f8a2-264">W oknie WSL Uruchom następujące polecenie: `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`</span><span class="sxs-lookup"><span data-stu-id="1f8a2-264">In a WSL window, run the following command: `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`</span></span>

  <span data-ttu-id="1f8a2-265">Poprzednie polecenie ustawia zmienne środowiskowe, aby system Linux używał zaufanego certyfikatu systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-265">The preceding command sets the environment variables so Linux uses the Windows trusted certificate.</span></span>

## <a name="troubleshoot-certificate-problems"></a><span data-ttu-id="1f8a2-266">Rozwiązywanie problemów z certyfikatami</span><span class="sxs-lookup"><span data-stu-id="1f8a2-266">Troubleshoot certificate problems</span></span>

<span data-ttu-id="1f8a2-267">Ta sekcja zawiera informacje ułatwiające [zainstalowanie i zaufanie](#trust)certyfikatu deweloperskiego https ASP.NET Core, ale nadal masz ostrzeżenia przeglądarki, że certyfikat nie jest zaufany.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-267">This section provides help when the ASP.NET Core HTTPS development certificate has been [installed and trusted](#trust), but you still have browser warnings that the certificate is not trusted.</span></span> <span data-ttu-id="1f8a2-268">ASP.NET Core certyfikat programistyczny HTTPS jest używany przez [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="1f8a2-268">The ASP.NET Core HTTPS development certificate is used by [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

### <a name="all-platforms---certificate-not-trusted"></a><span data-ttu-id="1f8a2-269">Wszystkie platformy — certyfikat niezaufany</span><span class="sxs-lookup"><span data-stu-id="1f8a2-269">All platforms - certificate not trusted</span></span>

<span data-ttu-id="1f8a2-270">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-270">Run the following commands:</span></span>

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

<span data-ttu-id="1f8a2-271">Zamknij wszystkie otwarte wystąpienia przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-271">Close any browser instances open.</span></span> <span data-ttu-id="1f8a2-272">Otwórz nowe okno przeglądarki do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-272">Open a new browser window to app.</span></span> <span data-ttu-id="1f8a2-273">Zaufanie certyfikatu jest buforowane przez przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-273">Certificate trust is cached by browsers.</span></span>

<span data-ttu-id="1f8a2-274">Powyższe polecenia rozwiązują większość problemów z zaufaniem do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-274">The preceding commands solve most browser trust issues.</span></span> <span data-ttu-id="1f8a2-275">Jeśli przeglądarka nadal nie ufa certyfikatowi, postępuj zgodnie z sugestiami specyficznymi dla danej platformy.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-275">If the browser is still not trusting the certificate, follow the platform-specific suggestions that follow.</span></span>

### <a name="docker---certificate-not-trusted"></a><span data-ttu-id="1f8a2-276">Zaufać certyfikatowi platformy Docker</span><span class="sxs-lookup"><span data-stu-id="1f8a2-276">Docker - certificate not trusted</span></span>

* <span data-ttu-id="1f8a2-277">Usuń folder *C:\Users\{User} \AppData\Roaming\ASP.NET\Https* .</span><span class="sxs-lookup"><span data-stu-id="1f8a2-277">Delete the *C:\Users\{USER}\AppData\Roaming\ASP.NET\Https* folder.</span></span>
* <span data-ttu-id="1f8a2-278">Wyczyść rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-278">Clean the solution.</span></span> <span data-ttu-id="1f8a2-279">Usuń foldery *bin* i *obj* .</span><span class="sxs-lookup"><span data-stu-id="1f8a2-279">Delete the *bin* and *obj* folders.</span></span>
* <span data-ttu-id="1f8a2-280">Uruchom ponownie narzędzie programistyczne.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-280">Restart the development tool.</span></span> <span data-ttu-id="1f8a2-281">Na przykład Visual Studio, Visual Studio Code lub Visual Studio dla komputerów Mac.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-281">For example, Visual Studio, Visual Studio Code, or Visual Studio for Mac.</span></span>

### <a name="windows---certificate-not-trusted"></a><span data-ttu-id="1f8a2-282">Windows-certyfikat niezaufany</span><span class="sxs-lookup"><span data-stu-id="1f8a2-282">Windows - certificate not trusted</span></span>

* <span data-ttu-id="1f8a2-283">Sprawdź certyfikaty w magazynie certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-283">Check the certificates in the certificate store.</span></span> <span data-ttu-id="1f8a2-284">Powinien istnieć certyfikat `localhost` z `ASP.NET Core HTTPS development certificate` przyjazną nazwą w obszarze `Current User > Personal > Certificates` i `Current User > Trusted root certification authorities > Certificates`</span><span class="sxs-lookup"><span data-stu-id="1f8a2-284">There should be a `localhost` certificate with the `ASP.NET Core HTTPS development certificate` friendly name both under `Current User > Personal > Certificates` and `Current User > Trusted root certification authorities > Certificates`</span></span>
* <span data-ttu-id="1f8a2-285">Usuń wszystkie znalezione certyfikaty zarówno z prywatnych, jak i zaufanych głównych urzędów certyfikacji.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-285">Remove all the found certificates from both Personal and Trusted root certification authorities.</span></span> <span data-ttu-id="1f8a2-286">**Nie** usuwaj IIS Express certyfikatu localhost.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-286">Do **not** remove the IIS Express localhost certificate.</span></span>
* <span data-ttu-id="1f8a2-287">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-287">Run the following commands:</span></span>

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

<span data-ttu-id="1f8a2-288">Zamknij wszystkie otwarte wystąpienia przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-288">Close any browser instances open.</span></span> <span data-ttu-id="1f8a2-289">Otwórz nowe okno przeglądarki do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-289">Open a new browser window to app.</span></span>

### <a name="os-x---certificate-not-trusted"></a><span data-ttu-id="1f8a2-290">OS X — certyfikat niezaufany</span><span class="sxs-lookup"><span data-stu-id="1f8a2-290">OS X - certificate not trusted</span></span>

* <span data-ttu-id="1f8a2-291">Otwórz dostęp do łańcucha kluczy.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-291">Open KeyChain Access.</span></span>
* <span data-ttu-id="1f8a2-292">Wybierz system łańcucha kluczy.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-292">Select the System keychain.</span></span>
* <span data-ttu-id="1f8a2-293">Sprawdź obecność certyfikatu localhost.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-293">Check for the presence of a localhost certificate.</span></span>
* <span data-ttu-id="1f8a2-294">Sprawdź, czy zawiera on symbol `+` na ikonie, aby wskazać, że jest on zaufany dla wszystkich użytkowników.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-294">Check that it contains a `+` symbol on the icon to indicate it's trusted for all users.</span></span>
* <span data-ttu-id="1f8a2-295">Usuń certyfikat z łańcucha kluczy systemu.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-295">Remove the certificate from the system keychain.</span></span>
* <span data-ttu-id="1f8a2-296">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="1f8a2-296">Run the following commands:</span></span>

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

<span data-ttu-id="1f8a2-297">Zamknij wszystkie otwarte wystąpienia przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-297">Close any browser instances open.</span></span> <span data-ttu-id="1f8a2-298">Otwórz nowe okno przeglądarki do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-298">Open a new browser window to app.</span></span>

<span data-ttu-id="1f8a2-299">Aby rozwiązać problemy z certyfikatami w programie Visual Studio, zobacz [błąd protokołu HTTPS przy użyciu IIS Express (#16892 dotnet/AspNetCore)](https://github.com/dotnet/AspNetCore/issues/16892) .</span><span class="sxs-lookup"><span data-stu-id="1f8a2-299">See [HTTPS Error using IIS Express (dotnet/AspNetCore #16892)](https://github.com/dotnet/AspNetCore/issues/16892) for troubleshooting certificate issues with Visual Studio.</span></span>

### <a name="iis-express-ssl-certificate-used-with-visual-studio"></a><span data-ttu-id="1f8a2-300">IIS Express certyfikat SSL używany z programem Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1f8a2-300">IIS Express SSL certificate used with Visual Studio</span></span>

<span data-ttu-id="1f8a2-301">Aby rozwiązać problemy z certyfikatem IIS Express, wybierz pozycję **napraw** w Instalatorze programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-301">To fix problems with the IIS Express certificate, select **Repair** from the Visual Studio installer.</span></span> <span data-ttu-id="1f8a2-302">Aby uzyskać więcej informacji, zobacz [ten problem](https://github.com/dotnet/aspnetcore/issues/16892)w serwisie GitHub.</span><span class="sxs-lookup"><span data-stu-id="1f8a2-302">For more information, see [this GitHub issue](https://github.com/dotnet/aspnetcore/issues/16892).</span></span>

## <a name="additional-information"></a><span data-ttu-id="1f8a2-303">Dodatkowe informacje</span><span class="sxs-lookup"><span data-stu-id="1f8a2-303">Additional information</span></span>

* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="1f8a2-304">Hostowanie ASP.NET Core w systemie Linux przy użyciu programu Apache: Konfiguracja protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="1f8a2-304">Host ASP.NET Core on Linux with Apache: HTTPS configuration</span></span>](xref:host-and-deploy/linux-apache#https-configuration)
* [<span data-ttu-id="1f8a2-305">ASP.NET Core hosta w systemie Linux z Nginx: Konfiguracja HTTPS</span><span class="sxs-lookup"><span data-stu-id="1f8a2-305">Host ASP.NET Core on Linux with Nginx: HTTPS configuration</span></span>](xref:host-and-deploy/linux-nginx#https-configuration)
* [<span data-ttu-id="1f8a2-306">Jak skonfigurować protokół SSL w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="1f8a2-306">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [<span data-ttu-id="1f8a2-307">Obsługa przeglądarki OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="1f8a2-307">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
