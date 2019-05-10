---
title: Wymuszanie protokołu HTTPS w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak wymaganie protokołu HTTPS/TLS w aplikacji sieci web platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: security/enforcing-ssl
ms.openlocfilehash: ab407436afb16687fa285a836b608ad2e6a4802f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64900748"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="db72d-103">Wymuszanie protokołu HTTPS w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="db72d-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="db72d-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="db72d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="db72d-105">W tym dokumencie przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="db72d-105">This document shows how to:</span></span>

* <span data-ttu-id="db72d-106">Wymaganie protokołu HTTPS dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="db72d-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="db72d-107">Przekieruj wszystkie żądania HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="db72d-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="db72d-108">Nie interfejsu API mogą uniemożliwić wysyłanie danych poufnych na pierwsze żądanie klienta.</span><span class="sxs-lookup"><span data-stu-id="db72d-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="db72d-109">Czy **nie** użyj [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) na interfejsy API sieci Web, która odbierać poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="db72d-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="db72d-110">`RequireHttpsAttribute` używa kodów stanu HTTP do przekierowania przeglądarki z protokołu HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="db72d-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="db72d-111">Klienci interfejsu API może zrozumieć lub nie przestrzegają przekierowuje z protokołu HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="db72d-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="db72d-112">Tacy klienci mogą wysłać informacje za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="db72d-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="db72d-113">Interfejsy API sieci Web powinien:</span><span class="sxs-lookup"><span data-stu-id="db72d-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="db72d-114">Nasłuchuj od protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="db72d-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="db72d-115">Zamknij połączenie z kodem stanu 400 (złe żądanie), a nie obsłużyć żądania.</span><span class="sxs-lookup"><span data-stu-id="db72d-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="db72d-116">Wymaganie protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="db72d-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="db72d-117">Firma Microsoft zaleca tej platformy ASP.NET Core w środowisku produkcyjnym wywołanie aplikacji sieci web:</span><span class="sxs-lookup"><span data-stu-id="db72d-117">We recommend that production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="db72d-118">Oprogramowanie pośredniczące przekierowania protokołu HTTPS (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) do przekierowywania żądań HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="db72d-118">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) to redirect HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="db72d-119">Oprogramowanie pośredniczące HSTS ([UseHsts](#http-strict-transport-security-protocol-hsts)) do wysyłania nagłówków interfejsów HTTP Strict transportu zabezpieczeń protokołu (HSTS) do klientów.</span><span class="sxs-lookup"><span data-stu-id="db72d-119">HSTS Middleware ([UseHsts](#http-strict-transport-security-protocol-hsts)) to send HTTP Strict Transport Security Protocol (HSTS) headers to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="db72d-120">Aplikacje wdrożone w konfiguracji zwrotny serwer proxy zezwala na serwer proxy, aby obsłużyć zabezpieczenia połączeń (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="db72d-120">Apps deployed in a reverse proxy configuration allow the proxy to handle connection security (HTTPS).</span></span> <span data-ttu-id="db72d-121">Jeśli serwer proxy obsługuje także przekierowania protokołu HTTPS, nie ma potrzeby używania oprogramowania pośredniczącego przekierowania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="db72d-121">If the proxy also handles HTTPS redirection, there's no need to use HTTPS Redirection Middleware.</span></span> <span data-ttu-id="db72d-122">Jeśli serwer proxy obsługuje także zapisywanie nagłówki HSTS (na przykład [HSTS natywnej obsługi w programie IIS 10.0 (1709) lub nowszym](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), oprogramowanie pośredniczące HSTS nie jest wymagane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="db72d-122">If the proxy server also handles writing HSTS headers (for example, [native HSTS support in IIS 10.0 (1709) or later](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS Middleware isn't required by the app.</span></span> <span data-ttu-id="db72d-123">Aby uzyskać więcej informacji, zobacz [rezygnacji z protokołu HTTPS/HSTS przy tworzeniu projektu](#opt-out-of-httpshsts-on-project-creation).</span><span class="sxs-lookup"><span data-stu-id="db72d-123">For more information, see [Opt-out of HTTPS/HSTS on project creation](#opt-out-of-httpshsts-on-project-creation).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="db72d-124">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="db72d-124">UseHttpsRedirection</span></span>

<span data-ttu-id="db72d-125">Poniższy kod wywoła `UseHttpsRedirection` w `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="db72d-125">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="db72d-126">Wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="db72d-126">The preceding highlighted code:</span></span>

* <span data-ttu-id="db72d-127">Używa domyślnej [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="db72d-127">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="db72d-128">Używa domyślnej [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), chyba że zostaną zastąpione `ASPNETCORE_HTTPS_PORT` zmiennej środowiskowej lub [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="db72d-128">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="db72d-129">Zalecamy używanie przekierowania tymczasowego, a nie stałych przekierowania.</span><span class="sxs-lookup"><span data-stu-id="db72d-129">We recommend using temporary redirects rather than permanent redirects.</span></span> <span data-ttu-id="db72d-130">Buforowanie linków może spowodować niestabilność zachowanie w środowiskach programistycznych.</span><span class="sxs-lookup"><span data-stu-id="db72d-130">Link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="db72d-131">Jeśli wolisz wysłać kod stanu trwałe przekierowanie, gdy aplikacja jest w środowisku deweloperskim bez, zobacz [Konfigurowanie przekierowania stałe w środowisku produkcyjnym](#configure-permanent-redirects-in-production) sekcji.</span><span class="sxs-lookup"><span data-stu-id="db72d-131">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, see the [Configure permanent redirects in production](#configure-permanent-redirects-in-production) section.</span></span> <span data-ttu-id="db72d-132">Firma Microsoft zaleca używanie [HSTS](#http-strict-transport-security-protocol-hsts) celu sygnalizowania, że dla klientów, którzy tylko zabezpieczyć zasób w aplikacji (tylko w środowisku produkcyjnym) powinny być wysyłane żądania.</span><span class="sxs-lookup"><span data-stu-id="db72d-132">We recommend using [HSTS](#http-strict-transport-security-protocol-hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

### <a name="port-configuration"></a><span data-ttu-id="db72d-133">Konfiguracja portu</span><span class="sxs-lookup"><span data-stu-id="db72d-133">Port configuration</span></span>

<span data-ttu-id="db72d-134">Port musi być dostępny dla oprogramowania pośredniczącego do przekierowania niezabezpieczone żądania HTTPS.</span><span class="sxs-lookup"><span data-stu-id="db72d-134">A port must be available for the middleware to redirect an insecure request to HTTPS.</span></span> <span data-ttu-id="db72d-135">Jeśli port nie jest dostępna:</span><span class="sxs-lookup"><span data-stu-id="db72d-135">If no port is available:</span></span>

* <span data-ttu-id="db72d-136">Przekierowania protokołu HTTPS nie zachodzi.</span><span class="sxs-lookup"><span data-stu-id="db72d-136">Redirection to HTTPS doesn't occur.</span></span>
* <span data-ttu-id="db72d-137">Oprogramowanie pośredniczące rejestruje ostrzeżenie "Nie można określić port https dla przekierowania."</span><span class="sxs-lookup"><span data-stu-id="db72d-137">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

<span data-ttu-id="db72d-138">Określ port HTTPS przy użyciu dowolnej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="db72d-138">Specify the HTTPS port using any of the following approaches:</span></span>

* <span data-ttu-id="db72d-139">Ustaw [HttpsRedirectionOptions.HttpsPort](#options).</span><span class="sxs-lookup"><span data-stu-id="db72d-139">Set [HttpsRedirectionOptions.HttpsPort](#options).</span></span>
* <span data-ttu-id="db72d-140">Ustaw `ASPNETCORE_HTTPS_PORT` zmiennej środowiskowej lub [ustawienia konfiguracji hosta sieci Web https_port](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="db72d-140">Set the `ASPNETCORE_HTTPS_PORT` environment variable or [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

  <span data-ttu-id="db72d-141">**Klucz**: `https_port`</span><span class="sxs-lookup"><span data-stu-id="db72d-141">**Key**: `https_port`</span></span>  
  <span data-ttu-id="db72d-142">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="db72d-142">**Type**: *string*</span></span>  
  <span data-ttu-id="db72d-143">**Domyślne**: Nie ustawiono wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="db72d-143">**Default**: A default value isn't set.</span></span>  
  <span data-ttu-id="db72d-144">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="db72d-144">**Set using**: `UseSetting`</span></span>  
  <span data-ttu-id="db72d-145">**Zmienna środowiskowa**: `<PREFIX_>HTTPS_PORT` (Prefiks jest `ASPNETCORE_` przy użyciu [hosta sieci Web](xref:fundamentals/host/web-host).)</span><span class="sxs-lookup"><span data-stu-id="db72d-145">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the [Web Host](xref:fundamentals/host/web-host).)</span></span>

  <span data-ttu-id="db72d-146">Podczas konfigurowania <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> w `Program`:</span><span class="sxs-lookup"><span data-stu-id="db72d-146">When configuring an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> in `Program`:</span></span>

  [!code-csharp[](enforcing-ssl/sample-snapshot/Program.cs?name=snippet_Program&highlight=10)]
* <span data-ttu-id="db72d-147">Wskazać port z bezpiecznego schematu przy użyciu `ASPNETCORE_URLS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="db72d-147">Indicate a port with the secure scheme using the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="db72d-148">Zmienna środowiskowa konfiguruje serwer.</span><span class="sxs-lookup"><span data-stu-id="db72d-148">The environment variable configures the server.</span></span> <span data-ttu-id="db72d-149">Oprogramowanie pośredniczące pośrednio odnajduje portu HTTPS za pośrednictwem <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="db72d-149">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="db72d-150">To podejście nie działa w przypadku wdrożeń zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="db72d-150">This approach doesn't work in reverse proxy deployments.</span></span>
* <span data-ttu-id="db72d-151">W trakcie opracowywania, należy ustawić adres URL protokołu HTTPS *launchsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="db72d-151">In development, set an HTTPS URL in *launchsettings.json*.</span></span> <span data-ttu-id="db72d-152">Włączanie obsługi protokołu HTTPS, gdy usługi IIS Express jest używana.</span><span class="sxs-lookup"><span data-stu-id="db72d-152">Enable HTTPS when IIS Express is used.</span></span>
* <span data-ttu-id="db72d-153">Skonfiguruj punkt końcowy HTTPS URL we wdrożeniu usługi edge publicznego [Kestrel](xref:fundamentals/servers/kestrel) serwera lub [HTTP.sys](xref:fundamentals/servers/httpsys) serwera.</span><span class="sxs-lookup"><span data-stu-id="db72d-153">Configure an HTTPS URL endpoint for a public-facing edge deployment of [Kestrel](xref:fundamentals/servers/kestrel) server or [HTTP.sys](xref:fundamentals/servers/httpsys) server.</span></span> <span data-ttu-id="db72d-154">Tylko **jeden port HTTPS** jest używany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="db72d-154">Only **one HTTPS port** is used by the app.</span></span> <span data-ttu-id="db72d-155">Oprogramowanie pośredniczące odnajduje portów za pomocą <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="db72d-155">The middleware discovers the port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span>

> [!NOTE]
> <span data-ttu-id="db72d-156">Po uruchomieniu aplikacji w ramach konfiguracji zwrotny serwer proxy <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> jest niedostępna.</span><span class="sxs-lookup"><span data-stu-id="db72d-156">When an app is run in a reverse proxy configuration, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> isn't available.</span></span> <span data-ttu-id="db72d-157">Ustaw port przy użyciu jednej z innych metod opisanych w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="db72d-157">Set the port using one of the other approaches described in this section.</span></span>

<span data-ttu-id="db72d-158">Podczas Kestrel lub sterownik HTTP.sys jest używany jako serwer graniczny publicznymi, Kestrel lub sterownik HTTP.sys muszą być skonfigurowane do nasłuchiwania na obu:</span><span class="sxs-lookup"><span data-stu-id="db72d-158">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>

* <span data-ttu-id="db72d-159">Bezpieczny port, na którym klient zostanie przekierowany (zazwyczaj port 443 w środowisku produkcyjnym i 5001 w trakcie programowania).</span><span class="sxs-lookup"><span data-stu-id="db72d-159">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
* <span data-ttu-id="db72d-160">Port niezabezpieczone (zwykle 80 w środowisku produkcyjnym) do 5000 w trakcie opracowywania.</span><span class="sxs-lookup"><span data-stu-id="db72d-160">The insecure port (typically, 80 in production and 5000 in development).</span></span>

<span data-ttu-id="db72d-161">Niebezpieczne port musi być dostępny dla klientów w kolejności dla aplikacji do odbierania niezabezpieczone żądania i przekierowywania klientów do bezpiecznego portu.</span><span class="sxs-lookup"><span data-stu-id="db72d-161">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect the client to the secure port.</span></span>

<span data-ttu-id="db72d-162">Aby uzyskać więcej informacji, zobacz [konfiguracji punktu końcowego Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) lub <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="db72d-162">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

### <a name="deployment-scenarios"></a><span data-ttu-id="db72d-163">Scenariusze wdrażania</span><span class="sxs-lookup"><span data-stu-id="db72d-163">Deployment scenarios</span></span>

<span data-ttu-id="db72d-164">Wszystkie zapory między klientem i serwerem musi mieć również komunikacji otwarte porty niezbędne dla ruchu.</span><span class="sxs-lookup"><span data-stu-id="db72d-164">Any firewall between the client and server must also have communication ports open for traffic.</span></span>

<span data-ttu-id="db72d-165">Jeśli żądania są przekazywane w konfiguracji zwrotny serwer proxy, należy użyć [przekazywane oprogramowania pośredniczącego nagłówki](xref:host-and-deploy/proxy-load-balancer) przed wywołaniem oprogramowania pośredniczącego przekierowania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="db72d-165">If requests are forwarded in a reverse proxy configuration, use [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) before calling HTTPS Redirection Middleware.</span></span> <span data-ttu-id="db72d-166">Aktualizacje oprogramowania pośredniczącego nagłówki przekazywane `Request.Scheme`przy użyciu `X-Forwarded-Proto` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="db72d-166">Forwarded Headers Middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header.</span></span> <span data-ttu-id="db72d-167">Zezwala oprogramowania pośredniczącego przekierowania URI i innych zasad zabezpieczeń, do prawidłowego działania.</span><span class="sxs-lookup"><span data-stu-id="db72d-167">The middleware permits redirect URIs and other security policies to work correctly.</span></span> <span data-ttu-id="db72d-168">Po przekazywane oprogramowania pośredniczącego nagłówków nie jest używany, aplikacji wewnętrznej bazy danych może być wyświetlony prawidłowy schemat i nie pozostać w pętli przekierowania.</span><span class="sxs-lookup"><span data-stu-id="db72d-168">When Forwarded Headers Middleware isn't used, the backend app might not receive the correct scheme and end up in a redirect loop.</span></span> <span data-ttu-id="db72d-169">Typowe komunikat o błędzie użytkownika końcowego jest wystąpiły za dużo przekierowań.</span><span class="sxs-lookup"><span data-stu-id="db72d-169">A common end user error message is that too many redirects have occurred.</span></span>

<span data-ttu-id="db72d-170">W przypadku wdrażania w usłudze Azure App Service, postępuj zgodnie ze wskazówkami w [samouczka: Powiązania istniejącego niestandardowego certyfikatu SSL w usłudze Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="db72d-170">When deploying to Azure App Service, follow the guidance in [Tutorial: Bind an existing custom SSL certificate to Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

### <a name="options"></a><span data-ttu-id="db72d-171">Opcje</span><span class="sxs-lookup"><span data-stu-id="db72d-171">Options</span></span>

<span data-ttu-id="db72d-172">Następujący wyróżniony kod wywołuje [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) skonfigurować opcje oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="db72d-172">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="db72d-173">Wywoływanie `AddHttpsRedirection` tylko jest konieczna zmiana wartości `HttpsPort` lub `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="db72d-173">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="db72d-174">Wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="db72d-174">The preceding highlighted code:</span></span>

* <span data-ttu-id="db72d-175">Zestawy [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) do <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, która jest wartością domyślną.</span><span class="sxs-lookup"><span data-stu-id="db72d-175">Sets [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) to <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, which is the default value.</span></span> <span data-ttu-id="db72d-176">Użyj pola <xref:Microsoft.AspNetCore.Http.StatusCodes> klasy do przypisania do `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="db72d-176">Use the fields of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="db72d-177">Ustawia HTTPS port 5001.</span><span class="sxs-lookup"><span data-stu-id="db72d-177">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="db72d-178">Wartość domyślna to 443.</span><span class="sxs-lookup"><span data-stu-id="db72d-178">The default value is 443.</span></span>

#### <a name="configure-permanent-redirects-in-production"></a><span data-ttu-id="db72d-179">Konfigurowanie przekierowania stałe w środowisku produkcyjnym</span><span class="sxs-lookup"><span data-stu-id="db72d-179">Configure permanent redirects in production</span></span>

<span data-ttu-id="db72d-180">Oprogramowanie pośredniczące, wartość domyślna to wysyłanie [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) przy użyciu wszystkich przekierowań.</span><span class="sxs-lookup"><span data-stu-id="db72d-180">The middleware defaults to sending a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) with all redirects.</span></span> <span data-ttu-id="db72d-181">Jeśli wolisz wysłać kod stanu trwałe przekierowanie, gdy aplikacja jest w środowisku deweloperskim bez zabalit warunkowego Wyszukaj środowisko — opcje konfiguracji oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="db72d-181">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, wrap the middleware options configuration in a conditional check for a non-Development environment.</span></span>

<span data-ttu-id="db72d-182">Podczas konfigurowania `IWebHostBuilder` w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="db72d-182">When configuring an `IWebHostBuilder` in *Startup.cs*:</span></span>

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

## <a name="https-redirection-middleware-alternative-approach"></a><span data-ttu-id="db72d-183">Informacje o innym podejściu oprogramowania pośredniczącego przekierowania protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="db72d-183">HTTPS Redirection Middleware alternative approach</span></span>

<span data-ttu-id="db72d-184">Alternatywa dla użycia oprogramowania pośredniczącego przekierowania protokołu HTTPS (`UseHttpsRedirection`) jest użycie oprogramowanie pośredniczące ponownego zapisywania adresów URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="db72d-184">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="db72d-185">`AddRedirectToHttps` Podczas wykonywania przekierowania, można również ustawić kod stanu i port.</span><span class="sxs-lookup"><span data-stu-id="db72d-185">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="db72d-186">Aby uzyskać więcej informacji, zobacz [oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="db72d-186">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="db72d-187">Podczas przekierowywania HTTPS bez potrzeby przekierowania dodatkowe reguły, firma Microsoft zaleca używanie oprogramowania pośredniczącego przekierowania protokołu HTTPS (`UseHttpsRedirection`) opisane w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="db72d-187">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="db72d-188">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) służy do wymagania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="db72d-188">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="db72d-189">`[RequireHttpsAttribute]` można dekoracji kontrolery lub metody lub mogą być stosowane globalnie.</span><span class="sxs-lookup"><span data-stu-id="db72d-189">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="db72d-190">Aby zastosować atrybut globalnie, Dodaj następujący kod do `ConfigureServices` w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="db72d-190">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="db72d-191">Powyższy kod wyróżniony wymaga, wszystkie żądania przy użyciu `HTTPS`; dlatego żądania HTTP są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="db72d-191">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="db72d-192">Następujący wyróżniony kod przekierowuje żądania HTTP do HTTPS:</span><span class="sxs-lookup"><span data-stu-id="db72d-192">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="db72d-193">Aby uzyskać więcej informacji, zobacz [oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="db72d-193">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="db72d-194">Oprogramowanie pośredniczące pozwala również aplikację, aby ustawić kod stanu lub kod stanu i port, podczas wykonywania przekierowania.</span><span class="sxs-lookup"><span data-stu-id="db72d-194">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="db72d-195">Globalnie wymagania protokołu HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) jest ze względów bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="db72d-195">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="db72d-196">Stosowanie `[RequireHttps]` atrybutu, aby wszystkie kontrolery/Razor strony nie jest uważany za bezpieczny globalnie wymagania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="db72d-196">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="db72d-197">Nie może zagwarantować `[RequireHttps]` atrybut jest stosowany podczas dodawania nowych kontrolerów i stron Razor.</span><span class="sxs-lookup"><span data-stu-id="db72d-197">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="db72d-198">Protokół zabezpieczeń Strict transportu HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="db72d-198">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="db72d-199">Na [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [zabezpieczeń transportu HTTP ograniczeniami (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) jest ulepszeniem zabezpieczeń zgłoszenie zgody na uczestnictwo w określonym przez aplikację sieci web przy użyciu nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="db72d-199">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="db72d-200">Gdy [przeglądarki obsługującej HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) odbiera tego nagłówka:</span><span class="sxs-lookup"><span data-stu-id="db72d-200">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="db72d-201">Przeglądarka przechowuje konfigurację dla domeny, która uniemożliwia wysłanie Każde powiadomienie za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="db72d-201">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="db72d-202">Przeglądarka wymusza całą komunikację za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="db72d-202">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="db72d-203">Przeglądarka uniemożliwia użytkownikowi za pomocą certyfikatów niezaufanych lub jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="db72d-203">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="db72d-204">Przeglądarka wyłącza monity, które umożliwiają użytkownikowi tymczasowo ufać takiego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="db72d-204">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="db72d-205">Ponieważ HSTS jest wymuszana przez klienta ma pewne ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="db72d-205">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="db72d-206">Klient musi obsługiwać HSTS.</span><span class="sxs-lookup"><span data-stu-id="db72d-206">The client must support HSTS.</span></span>
* <span data-ttu-id="db72d-207">HSTS wymaga co najmniej jeden pomyślne żądanie HTTPS do ustanowienia zasad HSTS.</span><span class="sxs-lookup"><span data-stu-id="db72d-207">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="db72d-208">Aplikacja musi sprawdzić każdego żądania HTTP i przekierowania lub odrzucanie żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="db72d-208">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="db72d-209">Platforma ASP.NET Core 2.1 lub nowszej implementuje HSTS z `UseHsts` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="db72d-209">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="db72d-210">Poniższy kod wywoła `UseHsts` gdy aplikacja nie jest [trybu opracowywania](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="db72d-210">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="db72d-211">`UseHsts` nie jest zalecane w rozwoju, ponieważ ustawienia HSTS są wysoce podlega buforowaniu przez przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="db72d-211">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="db72d-212">Domyślnie `UseHsts` nie obejmuje adresu lokalnego sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="db72d-212">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="db72d-213">W środowiskach produkcyjnych implementacji protokołu HTTPS, po raz pierwszy, ustaw początkowy [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) małej wartości przy użyciu jednej z <xref:System.TimeSpan> metody.</span><span class="sxs-lookup"><span data-stu-id="db72d-213">For production environments implementing HTTPS for the first time, set the initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) to a small value using one of the <xref:System.TimeSpan> methods.</span></span> <span data-ttu-id="db72d-214">Ustaw wartość od kilku godzin do nie więcej niż jednego dziennie w przypadku konieczności przywrócenia infrastruktury protokołu HTTPS do protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="db72d-214">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="db72d-215">Po masz pewność, że trwałości Konfiguracja protokołu HTTPS, należy zwiększyć wartość max-age HSTS; często używane wartości jest jeden rok.</span><span class="sxs-lookup"><span data-stu-id="db72d-215">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span>

<span data-ttu-id="db72d-216">Poniższy kod:</span><span class="sxs-lookup"><span data-stu-id="db72d-216">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="db72d-217">Ustawia wstępnego ładowania parametr nagłówka zabezpieczeń w przypadku transportu Strict.</span><span class="sxs-lookup"><span data-stu-id="db72d-217">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="db72d-218">Wstępnego ładowania nie jest częścią [specyfikacji RFC HSTS](https://tools.ietf.org/html/rfc6797), ale jest obsługiwane przez przeglądarki sieci web do wstępnego witryn HSTS na zainstalować od nowa.</span><span class="sxs-lookup"><span data-stu-id="db72d-218">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="db72d-219">Zobacz [ https://hstspreload.org/ ](https://hstspreload.org/) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="db72d-219">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="db72d-220">Włącza [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), co ma zastosowanie zasad HSTS poddomen hosta.</span><span class="sxs-lookup"><span data-stu-id="db72d-220">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="db72d-221">Jawnie Ustawia parametr max-age nagłówka zabezpieczeń w przypadku transportu Strict do 60 dni.</span><span class="sxs-lookup"><span data-stu-id="db72d-221">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="db72d-222">Jeśli nie zostanie ustawiona wartość domyślna to 30 dni.</span><span class="sxs-lookup"><span data-stu-id="db72d-222">If not set, defaults to 30 days.</span></span> <span data-ttu-id="db72d-223">Zobacz [dyrektywy max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="db72d-223">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="db72d-224">Dodaje `example.com` do listy hostów, które mają zostać wykluczone.</span><span class="sxs-lookup"><span data-stu-id="db72d-224">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="db72d-225">`UseHsts` nie obejmuje następujące hosty sprzężenia zwrotnego:</span><span class="sxs-lookup"><span data-stu-id="db72d-225">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="db72d-226">`localhost` : Adres IPv4 sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="db72d-226">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="db72d-227">`127.0.0.1` : Adres IPv4 sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="db72d-227">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="db72d-228">`[::1]` : Adres sprzężenia zwrotnego protokołu IPv6.</span><span class="sxs-lookup"><span data-stu-id="db72d-228">`[::1]` : The IPv6 loopback address.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="db72d-229">Zrezygnować z protokołu HTTPS/HSTS przy tworzeniu projektu</span><span class="sxs-lookup"><span data-stu-id="db72d-229">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="db72d-230">W niektórych scenariuszach usługi zaplecza gdzie zabezpieczenia połączeń jest obsługiwane na urządzeniach brzegowych publicznych sieci konfigurowanie zabezpieczenia połączeń w każdym węźle nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="db72d-230">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="db72d-231">Wygenerowany na podstawie szablonów w programie Visual Studio lub z aplikacji sieci Web [dotnet nowe](/dotnet/core/tools/dotnet-new) Włącz polecenie [przekierowania protokołu HTTPS](#require-https) i [HSTS](#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="db72d-231">Web apps generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require-https) and [HSTS](#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="db72d-232">W przypadku wdrożeń, które nie wymagają tych scenariuszy użytkownik może zrezygnować z HTTPS/HSTS po utworzeniu aplikacji z szablonu.</span><span class="sxs-lookup"><span data-stu-id="db72d-232">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="db72d-233">Aby zrezygnować z protokołu HTTPS/HSTS:</span><span class="sxs-lookup"><span data-stu-id="db72d-233">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="db72d-234">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="db72d-234">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="db72d-235">Usuń zaznaczenie pola wyboru **Konfigurowanie protokołu HTTPS** pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="db72d-235">Uncheck the **Configure for HTTPS** check box.</span></span>

![Nowe aplikacje sieci Web programu ASP.NET Core Wyświetla okno dialogowe Konfigurowanie usunięto zaznaczenie pola wyboru protokołu HTTPS.](enforcing-ssl/_static/out.png)

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="db72d-237">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="db72d-237">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="db72d-238">Użyj `--no-https` opcji.</span><span class="sxs-lookup"><span data-stu-id="db72d-238">Use the `--no-https` option.</span></span> <span data-ttu-id="db72d-239">Na przykład</span><span class="sxs-lookup"><span data-stu-id="db72d-239">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a><span data-ttu-id="db72d-240">Certyfikat jako zaufany platformy ASP.NET Core HTTPS programowanie na Windows i macOS</span><span class="sxs-lookup"><span data-stu-id="db72d-240">Trust the ASP.NET Core HTTPS development certificate on Windows and macOS</span></span>

<span data-ttu-id="db72d-241">Zestaw .NET core SDK zawiera certyfikatu deweloperskiego protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="db72d-241">.NET Core SDK includes a HTTPS development certificate.</span></span> <span data-ttu-id="db72d-242">Certyfikat jest instalowany jako część pierwszego uruchomienia komputera.</span><span class="sxs-lookup"><span data-stu-id="db72d-242">The certificate is installed as part of the first-run experience.</span></span> <span data-ttu-id="db72d-243">Na przykład `dotnet --info` generuje dane wyjściowe podobne do następującego:</span><span class="sxs-lookup"><span data-stu-id="db72d-243">For example, `dotnet --info` produces output similar to the following:</span></span>

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

<span data-ttu-id="db72d-244">Instalowanie programu .NET Core SDK instaluje certyfikatu deweloperskiego platformy ASP.NET Core HTTPS do magazynu certyfikatów użytkownika lokalnego.</span><span class="sxs-lookup"><span data-stu-id="db72d-244">Installing the .NET Core SDK installs the ASP.NET Core HTTPS development certificate to the local user certificate store.</span></span> <span data-ttu-id="db72d-245">Certyfikat został zainstalowany, ale nie jest zaufany.</span><span class="sxs-lookup"><span data-stu-id="db72d-245">The certificate has been installed, but it's not trusted.</span></span> <span data-ttu-id="db72d-246">Aby zaufania certyfikatu wykonaj jednorazowy krok, aby uruchomić program dotnet `dev-certs` narzędzie:</span><span class="sxs-lookup"><span data-stu-id="db72d-246">To trust the certificate perform the one-time step to run the dotnet `dev-certs` tool:</span></span>

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="db72d-247">Następujące polecenie zapewnia pomoc w `dev-certs` narzędzie:</span><span class="sxs-lookup"><span data-stu-id="db72d-247">The following command provides help on the `dev-certs` tool:</span></span>

```console
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="db72d-248">Jak skonfigurować certyfikat dewelopera dla programu Docker</span><span class="sxs-lookup"><span data-stu-id="db72d-248">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="db72d-249">Zobacz [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="db72d-249">See [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="db72d-250">Dodatkowe informacje</span><span class="sxs-lookup"><span data-stu-id="db72d-250">Additional information</span></span>

* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="db72d-251">Host platformy ASP.NET Core w systemie Linux z Apache: Konfiguracja protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="db72d-251">Host ASP.NET Core on Linux with Apache: HTTPS configuration</span></span>](xref:host-and-deploy/linux-apache#https-configuration)
* [<span data-ttu-id="db72d-252">Host platformy ASP.NET Core w systemie Linux przy użyciu serwera Nginx: Konfiguracja protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="db72d-252">Host ASP.NET Core on Linux with Nginx: HTTPS configuration</span></span>](xref:host-and-deploy/linux-nginx#https-configuration)
* [<span data-ttu-id="db72d-253">Jak skonfigurować protokół SSL na serwerze IIS</span><span class="sxs-lookup"><span data-stu-id="db72d-253">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [<span data-ttu-id="db72d-254">Obsługa przeglądarek OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="db72d-254">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
