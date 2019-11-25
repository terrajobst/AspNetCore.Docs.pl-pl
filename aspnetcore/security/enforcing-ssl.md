---
title: Wymuszanie protokołu HTTPS w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak wymagać protokołu HTTPS/TLS w aplikacji internetowej ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/14/2019
uid: security/enforcing-ssl
ms.openlocfilehash: 82cd2e52f3bd929682b9eae24611ad04fd9f8682
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317370"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="157c0-103">Wymuszanie protokołu HTTPS w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="157c0-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="157c0-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="157c0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="157c0-105">W tym dokumencie przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="157c0-105">This document shows how to:</span></span>

* <span data-ttu-id="157c0-106">Wymagaj protokołu HTTPS dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="157c0-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="157c0-107">Przekierowuj wszystkie żądania HTTP do protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="157c0-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="157c0-108">Żaden interfejs API nie może zapobiec wysyłaniu poufnych danych przez klienta do pierwszego żądania.</span><span class="sxs-lookup"><span data-stu-id="157c0-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> ## <a name="api-projects"></a><span data-ttu-id="157c0-109">Projekty interfejsu API</span><span class="sxs-lookup"><span data-stu-id="157c0-109">API projects</span></span>
>
> <span data-ttu-id="157c0-110">**Nie** należy używać [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) w interfejsach API sieci Web, które otrzymują poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="157c0-110">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="157c0-111">`RequireHttpsAttribute` używa kodów stanu HTTP do przekierowywania przeglądarek z protokołu HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="157c0-111">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="157c0-112">Klienci interfejsu API nie mogą zrozumieć ani przestrzegać przekierowania z protokołu HTTP do protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="157c0-112">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="157c0-113">Tacy klienci mogą wysyłać informacje za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="157c0-113">Such clients may send information over HTTP.</span></span> <span data-ttu-id="157c0-114">Interfejsy API sieci Web powinny:</span><span class="sxs-lookup"><span data-stu-id="157c0-114">Web APIs should either:</span></span>
>
> * <span data-ttu-id="157c0-115">Nie nasłuchuje na protokole HTTP.</span><span class="sxs-lookup"><span data-stu-id="157c0-115">Not listen on HTTP.</span></span>
> * <span data-ttu-id="157c0-116">Zamknij połączenie z kodem stanu 400 (złe żądanie) i nie obsługuj żądania.</span><span class="sxs-lookup"><span data-stu-id="157c0-116">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>
>
> ## <a name="hsts-and-api-projects"></a><span data-ttu-id="157c0-117">HSTS i projekty interfejsu API</span><span class="sxs-lookup"><span data-stu-id="157c0-117">HSTS and API projects</span></span>
>
> <span data-ttu-id="157c0-118">Domyślne projekty interfejsów API nie obejmują [HSTS](#hsts) , ponieważ HSTS jest ogólnie jedyną instrukcją przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="157c0-118">The default API projects don't include [HSTS](#hsts) because HSTS is generally a browser only instruction.</span></span> <span data-ttu-id="157c0-119">Inne obiekty wywołujące, takie jak telefony lub aplikacje klasyczne, **nie przestrzegają** instrukcji.</span><span class="sxs-lookup"><span data-stu-id="157c0-119">Other callers, such as phone or desktop apps, do **not** obey the instruction.</span></span> <span data-ttu-id="157c0-120">Nawet w przeglądarkach pojedyncze uwierzytelnione wywołanie interfejsu API za pośrednictwem protokołu HTTP jest niebezpieczne dla niezabezpieczonych sieci.</span><span class="sxs-lookup"><span data-stu-id="157c0-120">Even within browsers, a single authenticated call to an API over HTTP has risks on insecure networks.</span></span> <span data-ttu-id="157c0-121">Bezpiecznym podejściem jest skonfigurowanie projektów interfejsu API tylko do nasłuchiwania i odpowiadania za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="157c0-121">The secure approach is to configure API projects to only listen to and respond over HTTPS.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

> [!WARNING]
> ## <a name="api-projects"></a><span data-ttu-id="157c0-122">Projekty interfejsu API</span><span class="sxs-lookup"><span data-stu-id="157c0-122">API projects</span></span>
>
> <span data-ttu-id="157c0-123">**Nie** należy używać [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) w interfejsach API sieci Web, które otrzymują poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="157c0-123">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="157c0-124">`RequireHttpsAttribute` używa kodów stanu HTTP do przekierowywania przeglądarek z protokołu HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="157c0-124">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="157c0-125">Klienci interfejsu API nie mogą zrozumieć ani przestrzegać przekierowania z protokołu HTTP do protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="157c0-125">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="157c0-126">Tacy klienci mogą wysyłać informacje za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="157c0-126">Such clients may send information over HTTP.</span></span> <span data-ttu-id="157c0-127">Interfejsy API sieci Web powinny:</span><span class="sxs-lookup"><span data-stu-id="157c0-127">Web APIs should either:</span></span>
>
> * <span data-ttu-id="157c0-128">Nie nasłuchuje na protokole HTTP.</span><span class="sxs-lookup"><span data-stu-id="157c0-128">Not listen on HTTP.</span></span>
> * <span data-ttu-id="157c0-129">Zamknij połączenie z kodem stanu 400 (złe żądanie) i nie obsługuj żądania.</span><span class="sxs-lookup"><span data-stu-id="157c0-129">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

::: moniker-end

## <a name="require-https"></a><span data-ttu-id="157c0-130">Wymagaj protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="157c0-130">Require HTTPS</span></span>

<span data-ttu-id="157c0-131">Zalecamy korzystanie z aplikacji sieci Web ASP.NET Core produkcyjnych:</span><span class="sxs-lookup"><span data-stu-id="157c0-131">We recommend that production ASP.NET Core web apps use:</span></span>

* <span data-ttu-id="157c0-132">Oprogramowanie pośredniczące do przekierowywania HTTPS (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) do przekierowywania żądań HTTP do protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="157c0-132">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) to redirect HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="157c0-133">HSTSe oprogramowanie pośredniczące ([UseHsts](#http-strict-transport-security-protocol-hsts)) do wysyłania nagłówków HTTP Strict Transport Security Protocol (HSTS) do klientów.</span><span class="sxs-lookup"><span data-stu-id="157c0-133">HSTS Middleware ([UseHsts](#http-strict-transport-security-protocol-hsts)) to send HTTP Strict Transport Security Protocol (HSTS) headers to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="157c0-134">Aplikacje wdrożone w konfiguracji zwrotnego serwera proxy pozwalają serwerowi proxy obsługiwać zabezpieczenia połączeń (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="157c0-134">Apps deployed in a reverse proxy configuration allow the proxy to handle connection security (HTTPS).</span></span> <span data-ttu-id="157c0-135">Jeśli serwer proxy obsługuje również przekierowywanie przy użyciu protokołu HTTPS, nie ma potrzeby używania oprogramowania pośredniczącego do przekierowania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="157c0-135">If the proxy also handles HTTPS redirection, there's no need to use HTTPS Redirection Middleware.</span></span> <span data-ttu-id="157c0-136">Jeśli serwer proxy obsługuje również zapisywanie nagłówków HSTS (na przykład [natywnej obsługi HSTS w usługach IIS 10,0 (1709) lub nowszej](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), oprogramowanie pośredniczące HSTS nie jest wymagane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="157c0-136">If the proxy server also handles writing HSTS headers (for example, [native HSTS support in IIS 10.0 (1709) or later](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS Middleware isn't required by the app.</span></span> <span data-ttu-id="157c0-137">Aby uzyskać więcej informacji, zobacz [rezygnacja z protokołu HTTPS/HSTS podczas tworzenia projektu](#opt-out-of-httpshsts-on-project-creation).</span><span class="sxs-lookup"><span data-stu-id="157c0-137">For more information, see [Opt-out of HTTPS/HSTS on project creation](#opt-out-of-httpshsts-on-project-creation).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="157c0-138">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="157c0-138">UseHttpsRedirection</span></span>

<span data-ttu-id="157c0-139">Następujący kod wywołuje `UseHttpsRedirection` w klasie `Startup`:</span><span class="sxs-lookup"><span data-stu-id="157c0-139">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=13)]

::: moniker-end

<span data-ttu-id="157c0-140">Poprzedni wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="157c0-140">The preceding highlighted code:</span></span>

* <span data-ttu-id="157c0-141">Używa domyślnej [HttpsRedirectionOptions. RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="157c0-141">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="157c0-142">Używa domyślnego [HttpsRedirectionOptions. HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), chyba że jest zastępowany przez zmienną środowiskową `ASPNETCORE_HTTPS_PORT` lub [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="157c0-142">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="157c0-143">Zalecamy używanie tymczasowych przekierowań zamiast trwałych przekierowań.</span><span class="sxs-lookup"><span data-stu-id="157c0-143">We recommend using temporary redirects rather than permanent redirects.</span></span> <span data-ttu-id="157c0-144">Buforowanie łączy może spowodować niestabilne zachowanie w środowiskach deweloperskich.</span><span class="sxs-lookup"><span data-stu-id="157c0-144">Link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="157c0-145">Jeśli wolisz wysyłać kod stanu stałego przekierowania, gdy aplikacja znajduje się w środowisku innym niż programowanie, zobacz sekcję [Konfigurowanie stałych przekierowań w produkcji](#configure-permanent-redirects-in-production) .</span><span class="sxs-lookup"><span data-stu-id="157c0-145">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, see the [Configure permanent redirects in production](#configure-permanent-redirects-in-production) section.</span></span> <span data-ttu-id="157c0-146">Zalecamy używanie [HSTS](#http-strict-transport-security-protocol-hsts) do sygnalizowania klientów, że tylko zabezpieczone żądania zasobów powinny być wysyłane do aplikacji (tylko w środowisku produkcyjnym).</span><span class="sxs-lookup"><span data-stu-id="157c0-146">We recommend using [HSTS](#http-strict-transport-security-protocol-hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

### <a name="port-configuration"></a><span data-ttu-id="157c0-147">Konfiguracja portu</span><span class="sxs-lookup"><span data-stu-id="157c0-147">Port configuration</span></span>

<span data-ttu-id="157c0-148">Aby przekierować niezabezpieczone żądanie do protokołu HTTPS, Port musi być dostępny dla oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="157c0-148">A port must be available for the middleware to redirect an insecure request to HTTPS.</span></span> <span data-ttu-id="157c0-149">Jeśli port nie jest dostępny:</span><span class="sxs-lookup"><span data-stu-id="157c0-149">If no port is available:</span></span>

* <span data-ttu-id="157c0-150">Przekierowanie do protokołu HTTPS nie jest wykonywane.</span><span class="sxs-lookup"><span data-stu-id="157c0-150">Redirection to HTTPS doesn't occur.</span></span>
* <span data-ttu-id="157c0-151">Oprogramowanie pośredniczące rejestruje ostrzeżenie "nie można określić portu HTTPS do przekierowania".</span><span class="sxs-lookup"><span data-stu-id="157c0-151">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

<span data-ttu-id="157c0-152">Określ port HTTPS przy użyciu dowolnej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="157c0-152">Specify the HTTPS port using any of the following approaches:</span></span>

* <span data-ttu-id="157c0-153">Ustaw [HttpsRedirectionOptions. HttpsPort](#options).</span><span class="sxs-lookup"><span data-stu-id="157c0-153">Set [HttpsRedirectionOptions.HttpsPort](#options).</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="157c0-154">Ustaw [ustawienie hosta](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#https_port)`https_port`:</span><span class="sxs-lookup"><span data-stu-id="157c0-154">Set the `https_port` [host setting](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#https_port):</span></span>

  * <span data-ttu-id="157c0-155">W obszarze Konfiguracja hosta.</span><span class="sxs-lookup"><span data-stu-id="157c0-155">In host configuration.</span></span>
  * <span data-ttu-id="157c0-156">Ustawiając zmienną środowiskową `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="157c0-156">By setting the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
  * <span data-ttu-id="157c0-157">Dodając wpis najwyższego poziomu w pliku *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="157c0-157">By adding a top-level entry in *appsettings.json*:</span></span>

    [!code-json[](enforcing-ssl/sample-snapshot/3.x/appsettings.json?highlight=2)]

* <span data-ttu-id="157c0-158">Wskaż port z bezpiecznym schematem przy użyciu [zmiennej środowiskowej ASPNETCORE_URLS](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#urls).</span><span class="sxs-lookup"><span data-stu-id="157c0-158">Indicate a port with the secure scheme using the [ASPNETCORE_URLS environment variable](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#urls).</span></span> <span data-ttu-id="157c0-159">Zmienna środowiskowa służy do konfigurowania serwera.</span><span class="sxs-lookup"><span data-stu-id="157c0-159">The environment variable configures the server.</span></span> <span data-ttu-id="157c0-160">Oprogramowanie pośredniczące pośrednio wykrywa port HTTPS za pośrednictwem <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="157c0-160">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="157c0-161">Takie podejście nie działa w przypadku wdrożeń zwrotnych serwerów proxy.</span><span class="sxs-lookup"><span data-stu-id="157c0-161">This approach doesn't work in reverse proxy deployments.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

* <span data-ttu-id="157c0-162">Ustaw [ustawienie hosta](xref:fundamentals/host/web-host#https-port)`https_port`:</span><span class="sxs-lookup"><span data-stu-id="157c0-162">Set the `https_port` [host setting](xref:fundamentals/host/web-host#https-port):</span></span>

  * <span data-ttu-id="157c0-163">W obszarze Konfiguracja hosta.</span><span class="sxs-lookup"><span data-stu-id="157c0-163">In host configuration.</span></span>
  * <span data-ttu-id="157c0-164">Ustawiając zmienną środowiskową `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="157c0-164">By setting the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
  * <span data-ttu-id="157c0-165">Dodając wpis najwyższego poziomu w pliku *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="157c0-165">By adding a top-level entry in *appsettings.json*:</span></span>

    [!code-json[](enforcing-ssl/sample-snapshot/2.x/appsettings.json?highlight=2)]

* <span data-ttu-id="157c0-166">Wskaż port z bezpiecznym schematem przy użyciu [zmiennej środowiskowej ASPNETCORE_URLS](xref:fundamentals/host/web-host#server-urls).</span><span class="sxs-lookup"><span data-stu-id="157c0-166">Indicate a port with the secure scheme using the [ASPNETCORE_URLS environment variable](xref:fundamentals/host/web-host#server-urls).</span></span> <span data-ttu-id="157c0-167">Zmienna środowiskowa służy do konfigurowania serwera.</span><span class="sxs-lookup"><span data-stu-id="157c0-167">The environment variable configures the server.</span></span> <span data-ttu-id="157c0-168">Oprogramowanie pośredniczące pośrednio wykrywa port HTTPS za pośrednictwem <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="157c0-168">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="157c0-169">Takie podejście nie działa w przypadku wdrożeń zwrotnych serwerów proxy.</span><span class="sxs-lookup"><span data-stu-id="157c0-169">This approach doesn't work in reverse proxy deployments.</span></span>

::: moniker-end

* <span data-ttu-id="157c0-170">W obszarze programowanie Ustaw adres URL HTTPS w pliku *profilu launchsettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="157c0-170">In development, set an HTTPS URL in *launchsettings.json*.</span></span> <span data-ttu-id="157c0-171">Włącz protokół HTTPS, gdy zostanie użyta IIS Express.</span><span class="sxs-lookup"><span data-stu-id="157c0-171">Enable HTTPS when IIS Express is used.</span></span>

* <span data-ttu-id="157c0-172">Skonfiguruj punkt końcowy adresu URL HTTPS dla wdrożenia publicznej krawędzi serwera [Kestrel](xref:fundamentals/servers/kestrel) lub [http. sys](xref:fundamentals/servers/httpsys) .</span><span class="sxs-lookup"><span data-stu-id="157c0-172">Configure an HTTPS URL endpoint for a public-facing edge deployment of [Kestrel](xref:fundamentals/servers/kestrel) server or [HTTP.sys](xref:fundamentals/servers/httpsys) server.</span></span> <span data-ttu-id="157c0-173">Aplikacja używa tylko **jednego portu HTTPS** .</span><span class="sxs-lookup"><span data-stu-id="157c0-173">Only **one HTTPS port** is used by the app.</span></span> <span data-ttu-id="157c0-174">Oprogramowanie pośredniczące odnajduje port za pośrednictwem <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="157c0-174">The middleware discovers the port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span>

> [!NOTE]
> <span data-ttu-id="157c0-175">Gdy aplikacja jest uruchamiana w konfiguracji zwrotnego serwera proxy, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> nie jest dostępna.</span><span class="sxs-lookup"><span data-stu-id="157c0-175">When an app is run in a reverse proxy configuration, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> isn't available.</span></span> <span data-ttu-id="157c0-176">Ustaw port przy użyciu jednej z innych metod opisanych w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="157c0-176">Set the port using one of the other approaches described in this section.</span></span>

### <a name="edge-deployments"></a><span data-ttu-id="157c0-177">Wdrożenia brzegowe</span><span class="sxs-lookup"><span data-stu-id="157c0-177">Edge deployments</span></span> 

<span data-ttu-id="157c0-178">Gdy Kestrel lub HTTP. sys jest używany jako publiczny serwer graniczny, Kestrel lub HTTP. sys musi być skonfigurowany do nasłuchiwania na obu:</span><span class="sxs-lookup"><span data-stu-id="157c0-178">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>

* <span data-ttu-id="157c0-179">Bezpieczny port do przekierowania przez klienta (zazwyczaj 443 w środowisku produkcyjnym i 5001 podczas tworzenia).</span><span class="sxs-lookup"><span data-stu-id="157c0-179">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
* <span data-ttu-id="157c0-180">Niezabezpieczony port (zazwyczaj 80 w środowisku produkcyjnym i 5000 podczas tworzenia).</span><span class="sxs-lookup"><span data-stu-id="157c0-180">The insecure port (typically, 80 in production and 5000 in development).</span></span>

<span data-ttu-id="157c0-181">Niezabezpieczony Port musi być dostępny dla klienta, aby aplikacja mogła odebrać niezabezpieczone żądanie i przekierować klienta do bezpiecznego portu.</span><span class="sxs-lookup"><span data-stu-id="157c0-181">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect the client to the secure port.</span></span>

<span data-ttu-id="157c0-182">Aby uzyskać więcej informacji, zobacz [Kestrel Endpoint Configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) lub <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="157c0-182">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

### <a name="deployment-scenarios"></a><span data-ttu-id="157c0-183">Scenariusze wdrażania</span><span class="sxs-lookup"><span data-stu-id="157c0-183">Deployment scenarios</span></span>

<span data-ttu-id="157c0-184">Wszystkie zapory między klientem a serwerem muszą także mieć otwarte porty komunikacyjne dla ruchu.</span><span class="sxs-lookup"><span data-stu-id="157c0-184">Any firewall between the client and server must also have communication ports open for traffic.</span></span>

<span data-ttu-id="157c0-185">Jeśli żądania są przekazywane w konfiguracji zwrotnego serwera proxy, przed wywołaniem oprogramowania pośredniczącego do przekierowywania HTTPS Użyj [oprogramowania pośredniczącego](xref:host-and-deploy/proxy-load-balancer) .</span><span class="sxs-lookup"><span data-stu-id="157c0-185">If requests are forwarded in a reverse proxy configuration, use [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) before calling HTTPS Redirection Middleware.</span></span> <span data-ttu-id="157c0-186">Przesłane nagłówki — oprogramowanie pośredniczące aktualizuje `Request.Scheme`przy użyciu nagłówka `X-Forwarded-Proto`.</span><span class="sxs-lookup"><span data-stu-id="157c0-186">Forwarded Headers Middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header.</span></span> <span data-ttu-id="157c0-187">Oprogramowanie pośredniczące umożliwia poprawne działanie identyfikatorów URI przekierowania i innych zasad zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="157c0-187">The middleware permits redirect URIs and other security policies to work correctly.</span></span> <span data-ttu-id="157c0-188">Gdy przekazane nagłówki nie są używane, aplikacja zaplecza może nie otrzymać poprawnego schematu i zakończyć działania w pętli przekierowania.</span><span class="sxs-lookup"><span data-stu-id="157c0-188">When Forwarded Headers Middleware isn't used, the backend app might not receive the correct scheme and end up in a redirect loop.</span></span> <span data-ttu-id="157c0-189">Typowy komunikat o błędzie użytkownika końcowego polega na tym, że wystąpiły zbyt wiele przekierowań.</span><span class="sxs-lookup"><span data-stu-id="157c0-189">A common end user error message is that too many redirects have occurred.</span></span>

<span data-ttu-id="157c0-190">Podczas wdrażania programu w celu Azure App Service postępuj zgodnie ze wskazówkami podanymi w [samouczku: Powiąż istniejący niestandardowy certyfikat SSL z usługą Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="157c0-190">When deploying to Azure App Service, follow the guidance in [Tutorial: Bind an existing custom SSL certificate to Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

### <a name="options"></a><span data-ttu-id="157c0-191">Opcje</span><span class="sxs-lookup"><span data-stu-id="157c0-191">Options</span></span>

<span data-ttu-id="157c0-192">Następujący wyróżniony kod wywołuje [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) , aby skonfigurować opcje oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="157c0-192">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end


<span data-ttu-id="157c0-193">Wywoływanie `AddHttpsRedirection` jest niezbędne tylko do zmiany wartości `HttpsPort` lub `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="157c0-193">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="157c0-194">Poprzedni wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="157c0-194">The preceding highlighted code:</span></span>

* <span data-ttu-id="157c0-195">Ustawia [HttpsRedirectionOptions. RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) na <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, która jest wartością domyślną.</span><span class="sxs-lookup"><span data-stu-id="157c0-195">Sets [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) to <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, which is the default value.</span></span> <span data-ttu-id="157c0-196">Użyj pól klasy <xref:Microsoft.AspNetCore.Http.StatusCodes>, aby przydziały `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="157c0-196">Use the fields of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="157c0-197">Ustawia port HTTPS na 5001.</span><span class="sxs-lookup"><span data-stu-id="157c0-197">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="157c0-198">Wartość domyślna to 443.</span><span class="sxs-lookup"><span data-stu-id="157c0-198">The default value is 443.</span></span>

#### <a name="configure-permanent-redirects-in-production"></a><span data-ttu-id="157c0-199">Konfigurowanie stałych przekierowań w środowisku produkcyjnym</span><span class="sxs-lookup"><span data-stu-id="157c0-199">Configure permanent redirects in production</span></span>

<span data-ttu-id="157c0-200">Ustawienia domyślne oprogramowania pośredniczącego do wysyłania [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) z wszystkimi przekierowaniami.</span><span class="sxs-lookup"><span data-stu-id="157c0-200">The middleware defaults to sending a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) with all redirects.</span></span> <span data-ttu-id="157c0-201">Jeśli wolisz wysyłać kod stanu stałego przekierowania, gdy aplikacja znajduje się w środowisku nieprogramistycznym, Zapakuj konfigurację opcji oprogramowania pośredniczącego w ramach sprawdzania warunkowego dla środowiska nieprogramistycznego.</span><span class="sxs-lookup"><span data-stu-id="157c0-201">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, wrap the middleware options configuration in a conditional check for a non-Development environment.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="157c0-202">Podczas konfigurowania usług w programie *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="157c0-202">When configuring services in *Startup.cs*:</span></span>

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

<span data-ttu-id="157c0-203">Podczas konfigurowania usług w programie *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="157c0-203">When configuring services in *Startup.cs*:</span></span>

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


## <a name="https-redirection-middleware-alternative-approach"></a><span data-ttu-id="157c0-204">Alternatywne podejście do przekierowania protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="157c0-204">HTTPS Redirection Middleware alternative approach</span></span>

<span data-ttu-id="157c0-205">Alternatywą dla korzystania z oprogramowania pośredniczącego do przekierowywania HTTPS (`UseHttpsRedirection`) jest użycie oprogramowania pośredniczącego do ponownego zapisywania adresów URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="157c0-205">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="157c0-206">`AddRedirectToHttps` można również ustawić kod stanu i port, gdy przekierowanie zostanie wykonane.</span><span class="sxs-lookup"><span data-stu-id="157c0-206">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="157c0-207">Aby uzyskać więcej informacji, zobacz Ponowne [Zapisywanie oprogramowania pośredniczącego w adresie URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="157c0-207">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="157c0-208">Podczas przekierowywania do protokołu HTTPS bez wymagania dotyczącego dodatkowych reguł przekierowywania zalecamy używanie oprogramowania pośredniczącego do przekierowania protokołu HTTPS (`UseHttpsRedirection`) opisanej w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="157c0-208">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="157c0-209">Protokół HTTP Strict Transport Security Protocol (HSTS)</span><span class="sxs-lookup"><span data-stu-id="157c0-209">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="157c0-210">Na [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [http Strict Transport Security (HSTS)](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html) jest opcjonalnym ulepszeniem zabezpieczeń, określonym przez aplikację sieci Web przy użyciu nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="157c0-210">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="157c0-211">Gdy [przeglądarka, która obsługuje HSTS,](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html#browser-support) otrzymuje ten nagłówek:</span><span class="sxs-lookup"><span data-stu-id="157c0-211">When a [browser that supports HSTS](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html#browser-support) receives this header:</span></span>

* <span data-ttu-id="157c0-212">Przeglądarka przechowuje konfigurację dla domeny, która uniemożliwia wysyłanie komunikacji za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="157c0-212">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="157c0-213">Przeglądarka wymusza całą komunikację za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="157c0-213">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="157c0-214">Przeglądarka uniemożliwia użytkownikowi korzystanie z niezaufanych lub nieprawidłowych certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="157c0-214">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="157c0-215">Przeglądarka wyłącza wyświetlanie wierszy, które umożliwiają użytkownikowi tymczasowe zaufać temu certyfikatowi.</span><span class="sxs-lookup"><span data-stu-id="157c0-215">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="157c0-216">Ponieważ HSTS jest wymuszany przez klienta, ma pewne ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="157c0-216">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="157c0-217">Klient musi obsługiwać HSTS.</span><span class="sxs-lookup"><span data-stu-id="157c0-217">The client must support HSTS.</span></span>
* <span data-ttu-id="157c0-218">HSTS wymaga co najmniej jednego pomyślnego żądania HTTPS do ustanowienia zasad HSTS.</span><span class="sxs-lookup"><span data-stu-id="157c0-218">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="157c0-219">Aplikacja musi sprawdzić każde żądanie HTTP i przekierować lub odrzucić żądanie HTTP.</span><span class="sxs-lookup"><span data-stu-id="157c0-219">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="157c0-220">ASP.NET Core 2,1 i nowsze implementują HSTS z metodą rozszerzenia `UseHsts`.</span><span class="sxs-lookup"><span data-stu-id="157c0-220">ASP.NET Core 2.1 and later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="157c0-221">Następujący kod wywołuje `UseHsts`, gdy aplikacja nie jest w [trybie deweloperskim](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="157c0-221">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=10)]

::: moniker-end

<span data-ttu-id="157c0-222">`UseHsts` nie jest zalecana w programowaniu, ponieważ ustawienia HSTS mają wysoką pamięć podręczną przez przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="157c0-222">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="157c0-223">Domyślnie `UseHsts` wyklucza lokalny adres sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="157c0-223">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="157c0-224">W przypadku środowisk produkcyjnych, które wdrażają protokół HTTPS po raz pierwszy, należy ustawić początkowy [HstsOptions. maxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) na niewielką wartość przy użyciu jednej z <xref:System.TimeSpan> metod.</span><span class="sxs-lookup"><span data-stu-id="157c0-224">For production environments that are implementing HTTPS for the first time, set the initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) to a small value using one of the <xref:System.TimeSpan> methods.</span></span> <span data-ttu-id="157c0-225">Ustaw wartość od godziny na nie więcej niż jeden dzień w przypadku konieczności przywrócenia infrastruktury HTTPS do protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="157c0-225">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="157c0-226">Po uzyskaniu pewności w trwałości konfiguracji protokołu HTTPS Zwiększ wartość HSTS Max-Age; najczęściej używana wartość wynosi jeden rok.</span><span class="sxs-lookup"><span data-stu-id="157c0-226">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span>

<span data-ttu-id="157c0-227">Następujący kod:</span><span class="sxs-lookup"><span data-stu-id="157c0-227">The following code:</span></span>


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end


* <span data-ttu-id="157c0-228">Ustawia parametr Preload nagłówka Strict-Transport-Security.</span><span class="sxs-lookup"><span data-stu-id="157c0-228">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="157c0-229">Wstępne ładowanie nie jest częścią [specyfikacji RFC HSTS](https://tools.ietf.org/html/rfc6797), ale jest obsługiwane przez przeglądarki sieci Web do wstępnego ładowania witryn HSTS w przypadku instalacji nowej.</span><span class="sxs-lookup"><span data-stu-id="157c0-229">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="157c0-230">Aby uzyskać więcej informacji, zobacz [https://hstspreload.org/](https://hstspreload.org/) .</span><span class="sxs-lookup"><span data-stu-id="157c0-230">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="157c0-231">Włącza [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), która stosuje zasady HSTS do hostowania poddomen.</span><span class="sxs-lookup"><span data-stu-id="157c0-231">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="157c0-232">Jawnie ustawia wartość maksymalnego wieku w nagłówku Strict-Transport-Security na 60 dni.</span><span class="sxs-lookup"><span data-stu-id="157c0-232">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="157c0-233">Jeśli nie zostanie ustawiona, wartość domyślna to 30 dni.</span><span class="sxs-lookup"><span data-stu-id="157c0-233">If not set, defaults to 30 days.</span></span> <span data-ttu-id="157c0-234">Aby uzyskać więcej informacji, zobacz [dyrektywę max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) .</span><span class="sxs-lookup"><span data-stu-id="157c0-234">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="157c0-235">Dodaje `example.com` do listy hostów do wykluczenia.</span><span class="sxs-lookup"><span data-stu-id="157c0-235">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="157c0-236">`UseHsts` wyklucza następujące hosty sprzężenia zwrotnego:</span><span class="sxs-lookup"><span data-stu-id="157c0-236">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="157c0-237">`localhost`: adres sprzężenia zwrotnego IPv4.</span><span class="sxs-lookup"><span data-stu-id="157c0-237">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="157c0-238">`127.0.0.1`: adres sprzężenia zwrotnego IPv4.</span><span class="sxs-lookup"><span data-stu-id="157c0-238">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="157c0-239">`[::1]`: adres sprzężenia zwrotnego IPv6.</span><span class="sxs-lookup"><span data-stu-id="157c0-239">`[::1]` : The IPv6 loopback address.</span></span>

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="157c0-240">Zrezygnuj z protokołu HTTPS/HSTS podczas tworzenia projektu</span><span class="sxs-lookup"><span data-stu-id="157c0-240">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="157c0-241">W niektórych scenariuszach usługi zaplecza, w których zabezpieczenia połączeń są obsługiwane na publicznej krawędzi sieci, skonfigurowanie zabezpieczeń połączeń w każdym węźle nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="157c0-241">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="157c0-242">Aplikacje sieci Web, które są generowane na podstawie szablonów w programie Visual Studio lub za pomocą polecenia [dotnet New](/dotnet/core/tools/dotnet-new) , umożliwiają [przekierowania https](#require-https) i [HSTS](#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="157c0-242">Web apps that are generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require-https) and [HSTS](#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="157c0-243">W przypadku wdrożeń, które nie wymagają tych scenariuszy, można zrezygnować z protokołu HTTPS/HSTS podczas tworzenia aplikacji na podstawie szablonu.</span><span class="sxs-lookup"><span data-stu-id="157c0-243">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="157c0-244">Aby zrezygnować z protokołu HTTPS/HSTS:</span><span class="sxs-lookup"><span data-stu-id="157c0-244">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="157c0-245">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="157c0-245">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="157c0-246">Usuń zaznaczenie pola wyboru **Konfiguruj dla protokołu HTTPS** .</span><span class="sxs-lookup"><span data-stu-id="157c0-246">Uncheck the **Configure for HTTPS** check box.</span></span>

::: moniker range=">= aspnetcore-3.0"

![Okno dialogowe nowe ASP.NET Core aplikacji sieci Web z niezaznaczonym polem wyboru Konfiguruj dla protokołu HTTPS.](enforcing-ssl/_static/out-vs2019.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

![Okno dialogowe nowe ASP.NET Core aplikacji sieci Web z niezaznaczonym polem wyboru Konfiguruj dla protokołu HTTPS.](enforcing-ssl/_static/out.png)

::: moniker-end


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="157c0-249">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="157c0-249">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="157c0-250">Użyj opcji `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="157c0-250">Use the `--no-https` option.</span></span> <span data-ttu-id="157c0-251">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="157c0-251">For example</span></span>

```dotnetcli
dotnet new webapp --no-https
```

---

<a name="trust"></a>

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a><span data-ttu-id="157c0-252">Ufaj certyfikatowi Deweloperskiemu protokołu HTTPS ASP.NET Core w systemach Windows i macOS</span><span class="sxs-lookup"><span data-stu-id="157c0-252">Trust the ASP.NET Core HTTPS development certificate on Windows and macOS</span></span>

<span data-ttu-id="157c0-253">Zestaw .NET Core SDK zawiera certyfikat programistyczny HTTPS.</span><span class="sxs-lookup"><span data-stu-id="157c0-253">The .NET Core SDK includes an HTTPS development certificate.</span></span> <span data-ttu-id="157c0-254">Certyfikat jest instalowany w ramach pierwszego uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="157c0-254">The certificate is installed as part of the first-run experience.</span></span> <span data-ttu-id="157c0-255">Na przykład `dotnet --info` generuje dane wyjściowe podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="157c0-255">For example, `dotnet --info` produces output similar to the following:</span></span>

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

<span data-ttu-id="157c0-256">Zainstalowanie zestaw .NET Core SDK instaluje certyfikat deweloperski ASP.NET Core HTTPS do magazynu certyfikatów użytkownika lokalnego.</span><span class="sxs-lookup"><span data-stu-id="157c0-256">Installing the .NET Core SDK installs the ASP.NET Core HTTPS development certificate to the local user certificate store.</span></span> <span data-ttu-id="157c0-257">Certyfikat został zainstalowany, ale nie jest zaufany.</span><span class="sxs-lookup"><span data-stu-id="157c0-257">The certificate has been installed, but it's not trusted.</span></span> <span data-ttu-id="157c0-258">Aby zaufać certyfikatowi, wykonaj jednorazowy krok w celu uruchomienia narzędzia dotnet `dev-certs`:</span><span class="sxs-lookup"><span data-stu-id="157c0-258">To trust the certificate perform the one-time step to run the dotnet `dev-certs` tool:</span></span>

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="157c0-259">Następujące polecenie zapewnia pomoc dotyczącą narzędzia `dev-certs`:</span><span class="sxs-lookup"><span data-stu-id="157c0-259">The following command provides help on the `dev-certs` tool:</span></span>

```dotnetcli
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="157c0-260">Jak skonfigurować certyfikat dewelopera dla platformy Docker</span><span class="sxs-lookup"><span data-stu-id="157c0-260">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="157c0-261">Zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/6199)w serwisie GitHub.</span><span class="sxs-lookup"><span data-stu-id="157c0-261">See [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6199).</span></span>

<a name="wsl"></a>

## <a name="trust-https-certificate-from-windows-subsystem-for-linux"></a><span data-ttu-id="157c0-262">Ufaj certyfikatowi HTTPS z podsystemu Windows dla systemu Linux</span><span class="sxs-lookup"><span data-stu-id="157c0-262">Trust HTTPS certificate from Windows Subsystem for Linux</span></span>

<span data-ttu-id="157c0-263">Podsystem Windows dla systemu Linux (WSL) generuje certyfikat z podpisem własnym HTTPS. Aby skonfigurować magazyn certyfikatów systemu Windows w celu zaufać certyfikatowi WSL:</span><span class="sxs-lookup"><span data-stu-id="157c0-263">The Windows Subsystem for Linux (WSL) generates a HTTPS self-signed cert. To configure the Windows certificate store to trust the WSL certificate:</span></span>

* <span data-ttu-id="157c0-264">Uruchom następujące polecenie, aby wyeksportować certyfikat wygenerowany przez WSL: `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`</span><span class="sxs-lookup"><span data-stu-id="157c0-264">Run the following command to export the WSL generated certificate: `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`</span></span>
* <span data-ttu-id="157c0-265">W oknie WSL Uruchom następujące polecenie: `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`</span><span class="sxs-lookup"><span data-stu-id="157c0-265">In a WSL window, run the following command: `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`</span></span>

  <span data-ttu-id="157c0-266">Poprzednie polecenie ustawia zmienne środowiskowe, aby system Linux używał zaufanego certyfikatu systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="157c0-266">The preceding command sets the environment variables so Linux uses the Windows trusted certificate.</span></span>

## <a name="troubleshoot-certificate-problems"></a><span data-ttu-id="157c0-267">Rozwiązywanie problemów z certyfikatami</span><span class="sxs-lookup"><span data-stu-id="157c0-267">Troubleshoot certificate problems</span></span>

<span data-ttu-id="157c0-268">Ta sekcja zawiera informacje ułatwiające [zainstalowanie i zaufanie](#trust)certyfikatu deweloperskiego https ASP.NET Core, ale nadal masz ostrzeżenia przeglądarki, że certyfikat nie jest zaufany.</span><span class="sxs-lookup"><span data-stu-id="157c0-268">This section provides help when the ASP.NET Core HTTPS development certificate has been [installed and trusted](#trust), but you still have browser warnings that the certificate is not trusted.</span></span> <span data-ttu-id="157c0-269">ASP.NET Core certyfikat programistyczny HTTPS jest używany przez [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="157c0-269">The ASP.NET Core HTTPS development certificate is used by [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

### <a name="all-platforms---certificate-not-trusted"></a><span data-ttu-id="157c0-270">Wszystkie platformy — certyfikat niezaufany</span><span class="sxs-lookup"><span data-stu-id="157c0-270">All platforms - certificate not trusted</span></span>

<span data-ttu-id="157c0-271">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="157c0-271">Run the following commands:</span></span>

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

<span data-ttu-id="157c0-272">Zamknij wszystkie otwarte wystąpienia przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="157c0-272">Close any browser instances open.</span></span> <span data-ttu-id="157c0-273">Otwórz nowe okno przeglądarki do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="157c0-273">Open a new browser window to app.</span></span> <span data-ttu-id="157c0-274">Zaufanie certyfikatu jest buforowane przez przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="157c0-274">Certificate trust is cached by browsers.</span></span>

<span data-ttu-id="157c0-275">Powyższe polecenia rozwiązują większość problemów z zaufaniem do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="157c0-275">The preceding commands solve most browser trust issues.</span></span> <span data-ttu-id="157c0-276">Jeśli przeglądarka nadal nie ufa certyfikatowi, postępuj zgodnie z poniższymi sugestiami dotyczącymi platformy.</span><span class="sxs-lookup"><span data-stu-id="157c0-276">If the browser is still not trusting the certificate, follow the platform specific suggestions that follow.</span></span>

### <a name="docker---certificate-not-trusted"></a><span data-ttu-id="157c0-277">Zaufać certyfikatowi platformy Docker</span><span class="sxs-lookup"><span data-stu-id="157c0-277">Docker - certificate not trusted</span></span>

* <span data-ttu-id="157c0-278">Usuń folder *C:\Users\{User} \AppData\Roaming\ASP.NET\Https* .</span><span class="sxs-lookup"><span data-stu-id="157c0-278">Delete the *C:\Users\{USER}\AppData\Roaming\ASP.NET\Https* folder.</span></span>
* <span data-ttu-id="157c0-279">Wyczyść rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="157c0-279">Clean the solution.</span></span> <span data-ttu-id="157c0-280">Usuń *bin* i *obj* folderów.</span><span class="sxs-lookup"><span data-stu-id="157c0-280">Delete the *bin* and *obj* folders.</span></span>
* <span data-ttu-id="157c0-281">Uruchom ponownie narzędzie programistyczne.</span><span class="sxs-lookup"><span data-stu-id="157c0-281">Restart the development tool.</span></span> <span data-ttu-id="157c0-282">Na przykład Visual Studio, Visual Studio Code lub Visual Studio dla komputerów Mac.</span><span class="sxs-lookup"><span data-stu-id="157c0-282">For example, Visual Studio, Visual Studio Code, or Visual Studio for Mac.</span></span>

### <a name="windows---certificate-not-trusted"></a><span data-ttu-id="157c0-283">Windows-certyfikat niezaufany</span><span class="sxs-lookup"><span data-stu-id="157c0-283">Windows - certificate not trusted</span></span>

* <span data-ttu-id="157c0-284">Sprawdź certyfikaty w magazynie certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="157c0-284">Check the certificates in the certificate store.</span></span> <span data-ttu-id="157c0-285">Powinien istnieć certyfikat `localhost` z `ASP.NET Core HTTPS development certificate` przyjazną nazwą w obszarze `Current User > Personal > Certificates` i `Current User > Trusted root certification authorities > Certificates`</span><span class="sxs-lookup"><span data-stu-id="157c0-285">There should be a `localhost` certificate with the `ASP.NET Core HTTPS development certificate` friendly name both under `Current User > Personal > Certificates` and `Current User > Trusted root certification authorities > Certificates`</span></span>
* <span data-ttu-id="157c0-286">Usuń wszystkie znalezione certyfikaty zarówno z prywatnych, jak i zaufanych głównych urzędów certyfikacji.</span><span class="sxs-lookup"><span data-stu-id="157c0-286">Remove all the found certificates from both Personal and Trusted root certification authorities.</span></span> <span data-ttu-id="157c0-287">**Nie** usuwaj IIS Express certyfikatu localhost.</span><span class="sxs-lookup"><span data-stu-id="157c0-287">Do **not** remove the IIS Express localhost certificate.</span></span>
* <span data-ttu-id="157c0-288">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="157c0-288">Run the following commands:</span></span>

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

<span data-ttu-id="157c0-289">Zamknij wszystkie otwarte wystąpienia przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="157c0-289">Close any browser instances open.</span></span> <span data-ttu-id="157c0-290">Otwórz nowe okno przeglądarki do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="157c0-290">Open a new browser window to app.</span></span>

### <a name="os-x---certificate-not-trusted"></a><span data-ttu-id="157c0-291">OS X — certyfikat niezaufany</span><span class="sxs-lookup"><span data-stu-id="157c0-291">OS X - certificate not trusted</span></span>

* <span data-ttu-id="157c0-292">Otwórz dostęp do łańcucha kluczy.</span><span class="sxs-lookup"><span data-stu-id="157c0-292">Open KeyChain Access.</span></span>
* <span data-ttu-id="157c0-293">Wybierz system łańcucha kluczy.</span><span class="sxs-lookup"><span data-stu-id="157c0-293">Select the System keychain.</span></span>
* <span data-ttu-id="157c0-294">Sprawdź obecność certyfikatu localhost.</span><span class="sxs-lookup"><span data-stu-id="157c0-294">Check for the presence of a localhost certificate.</span></span>
* <span data-ttu-id="157c0-295">Sprawdź, czy zawiera on symbol `+` na ikonie, aby wskazać jego zaufaną dla wszystkich użytkowników.</span><span class="sxs-lookup"><span data-stu-id="157c0-295">Check that it contains a `+` symbol on the icon to indicate its trusted for all users.</span></span>
* <span data-ttu-id="157c0-296">Usuń certyfikat z łańcucha kluczy systemu.</span><span class="sxs-lookup"><span data-stu-id="157c0-296">Remove the certificate from the system keychain.</span></span>
* <span data-ttu-id="157c0-297">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="157c0-297">Run the following commands:</span></span>

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

<span data-ttu-id="157c0-298">Zamknij wszystkie otwarte wystąpienia przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="157c0-298">Close any browser instances open.</span></span> <span data-ttu-id="157c0-299">Otwórz nowe okno przeglądarki do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="157c0-299">Open a new browser window to app.</span></span>

<span data-ttu-id="157c0-300">Aby rozwiązać problemy z certyfikatami w programie Visual Studio, zobacz [błąd HTTPS przy użyciu IIS Express (ASPNET/AspNetCore #16892)](https://github.com/aspnet/AspNetCore/issues/16892) .</span><span class="sxs-lookup"><span data-stu-id="157c0-300">See [HTTPS Error using IIS Express (aspnet/AspNetCore #16892)](https://github.com/aspnet/AspNetCore/issues/16892) for troubleshooting certificate issues with Visual Studio.</span></span>

### <a name="iis-express-ssl-certificate-used-with-visual-studio"></a><span data-ttu-id="157c0-301">IIS Express certyfikat SSL używany z programem Visual Studio</span><span class="sxs-lookup"><span data-stu-id="157c0-301">IIS Express SSL certificate used with Visual Studio</span></span>

<span data-ttu-id="157c0-302">Aby rozwiązać problemy z certyfikatem IIS Express, wybierz pozycję **napraw** w Instalatorze programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="157c0-302">To fix problems with the IIS Express certificate, select **Repair** from the Visual Studio installer.</span></span>

## <a name="additional-information"></a><span data-ttu-id="157c0-303">Dodatkowe informacje</span><span class="sxs-lookup"><span data-stu-id="157c0-303">Additional information</span></span>

* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="157c0-304">Hostowanie ASP.NET Core w systemie Linux przy użyciu programu Apache: Konfiguracja protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="157c0-304">Host ASP.NET Core on Linux with Apache: HTTPS configuration</span></span>](xref:host-and-deploy/linux-apache#https-configuration)
* [<span data-ttu-id="157c0-305">ASP.NET Core hosta w systemie Linux z Nginx: Konfiguracja HTTPS</span><span class="sxs-lookup"><span data-stu-id="157c0-305">Host ASP.NET Core on Linux with Nginx: HTTPS configuration</span></span>](xref:host-and-deploy/linux-nginx#https-configuration)
* [<span data-ttu-id="157c0-306">Jak skonfigurować protokół SSL w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="157c0-306">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [<span data-ttu-id="157c0-307">Obsługa przeglądarki OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="157c0-307">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
