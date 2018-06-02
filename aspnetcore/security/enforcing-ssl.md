---
title: Wymuszanie protokołu HTTPS w platformy ASP.NET Core
author: rick-anderson
description: Pokazuje, jak będą musieli HTTPS/TLS w ASP.NET Core aplikacji sieci web.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 24ab83192ded381b46fab337a986f51fb22b2227
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729504"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="af68d-103">Wymuszanie protokołu HTTPS w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="af68d-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="af68d-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="af68d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="af68d-105">Ten dokument zawiera jak:</span><span class="sxs-lookup"><span data-stu-id="af68d-105">This document shows how to:</span></span>

* <span data-ttu-id="af68d-106">Wymagać protokołu HTTPS dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="af68d-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="af68d-107">Przekieruj żądania HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af68d-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="af68d-108">Czy **nie** użyj [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) na interfejsów API sieci Web, czy odbierać poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="af68d-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="af68d-109">`RequireHttpsAttribute` używa kodów stanu HTTP do przekierowania przeglądarki z protokołu HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af68d-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="af68d-110">Klientów interfejsu API nie może zrozumieć lub przestrzegać przekierowania z protokołu HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af68d-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="af68d-111">Tacy klienci mogą wysłać informacje za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="af68d-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="af68d-112">Interfejsy API sieci Web powinien:</span><span class="sxs-lookup"><span data-stu-id="af68d-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="af68d-113">Nie nasłuchiwania protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="af68d-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="af68d-114">Zamknięcie połączenia z kodem stanu 400 (nieprawidłowe żądanie), a nie obsługiwać żądania.</span><span class="sxs-lookup"><span data-stu-id="af68d-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="af68d-115">Wymagać protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="af68d-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="af68d-116">Firma Microsoft zaleca, wszystkie aplikacje sieci web platformy ASP.NET Core wywoła oprogramowanie pośredniczące przekierowania protokołu HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) aby przekierować wszystkie żądania HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af68d-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="af68d-117">Poniższy kod wywołania `UseHttpsRedirection` w `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="af68d-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="af68d-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="af68d-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>

<span data-ttu-id="af68d-119">Poniższy kod wywołania [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) Aby skonfigurować opcje oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="af68d-119">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

<span data-ttu-id="af68d-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="af68d-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

<span data-ttu-id="af68d-121">Wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="af68d-121">The preceding highlighted code:</span></span>

* <span data-ttu-id="af68d-122">Ustawia [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode).</span><span class="sxs-lookup"><span data-stu-id="af68d-122">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode).</span></span>
* <span data-ttu-id="af68d-123">Ustawia HTTPS port 5001.</span><span class="sxs-lookup"><span data-stu-id="af68d-123">Sets the HTTPS port to 5001.</span></span>

<span data-ttu-id="af68d-124">Następujące mechanizmy automatycznie ustawiony port:</span><span class="sxs-lookup"><span data-stu-id="af68d-124">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="af68d-125">Oprogramowanie pośredniczące mogą odnajdować portów za pomocą [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) gdy mają zastosowanie następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="af68d-125">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="af68d-126">Kestrel lub sterownik HTTP.sys jest używana bezpośrednio z punktów końcowych HTTPS (ma zastosowanie także do uruchamiania aplikacji z debugera w programie Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="af68d-126">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="af68d-127">Tylko **jeden port HTTPS** jest używany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="af68d-127">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="af68d-128">Używany jest program Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="af68d-128">Visual Studio is used:</span></span>
  - <span data-ttu-id="af68d-129">Usługi IIS Express zawiera obsługujące protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af68d-129">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="af68d-130">*launchSettings.json* ustawia `sslPort` przez usługi IIS Express.</span><span class="sxs-lookup"><span data-stu-id="af68d-130">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="af68d-131">Gdy aplikacja jest uruchamiana za zwrotnego serwera proxy (na przykład usług IIS, usługi IIS Express) `IServerAddressesFeature` nie jest dostępna.</span><span class="sxs-lookup"><span data-stu-id="af68d-131">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="af68d-132">Port należy skonfigurować ręcznie.</span><span class="sxs-lookup"><span data-stu-id="af68d-132">The port must be manually configured.</span></span> <span data-ttu-id="af68d-133">Jeśli port nie jest ustawiona, przekierowanie nie są żądania.</span><span class="sxs-lookup"><span data-stu-id="af68d-133">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="af68d-134">Przez ustawienie można skonfigurować port:</span><span class="sxs-lookup"><span data-stu-id="af68d-134">The port can be configured by setting the:</span></span>

* <span data-ttu-id="af68d-135">`ASPNETCORE_HTTPS_PORT` Zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="af68d-135">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="af68d-136">`http_port` Klucz konfiguracji hosta (na przykład za pomocą *hostsettings.json* lub argumentu wiersza polecenia).</span><span class="sxs-lookup"><span data-stu-id="af68d-136">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="af68d-137">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="af68d-137">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="af68d-138">Zapoznaj się z poprzednim przykładem, pokazujący sposób Ustaw port 5001.</span><span class="sxs-lookup"><span data-stu-id="af68d-138">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="af68d-139">Port można skonfigurować pośrednio przez ustawienie adresu URL z `ASPNETCORE_URLS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="af68d-139">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="af68d-140">Zmienna środowiskowa konfiguruje serwer, a następnie oprogramowanie pośredniczące pośrednio odnajduje port HTTPS za pomocą `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="af68d-140">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="af68d-141">Jeśli port nie jest ustawiony:</span><span class="sxs-lookup"><span data-stu-id="af68d-141">If no port is set:</span></span>

* <span data-ttu-id="af68d-142">Nie można przekierować żądania.</span><span class="sxs-lookup"><span data-stu-id="af68d-142">Requests aren't redirected.</span></span>
* <span data-ttu-id="af68d-143">Oprogramowanie pośredniczące rejestruje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="af68d-143">The middleware logs a warning.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="af68d-144">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) jest używana, aby wymagać protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af68d-144">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="af68d-145">`[RequireHttpsAttribute]` można dekoracji kontrolerów lub metody lub mogą być stosowane globalnie.</span><span class="sxs-lookup"><span data-stu-id="af68d-145">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="af68d-146">Aby zastosować atrybut globalny, Dodaj następujący kod do `ConfigureServices` w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="af68d-146">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="af68d-147">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="af68d-147">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="af68d-148">Poprzedni wyróżniony kod wymaga wszystkie żądania przy użyciu `HTTPS`; w związku z tym żądania HTTP są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="af68d-148">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="af68d-149">Następujący wyróżniony kod przekierowuje żądania HTTP, https:</span><span class="sxs-lookup"><span data-stu-id="af68d-149">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="af68d-150">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="af68d-150">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="af68d-151">Aby uzyskać więcej informacji, zobacz [ponowne zapisywanie adresów URL w oprogramowaniu pośredniczącym](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="af68d-151">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="af68d-152">Globalny wymagających protokołu HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) jest ze względów bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="af68d-152">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="af68d-153">Stosowanie `[RequireHttps]` atrybut, aby wszystkie kontrolery/Razor strony nie jest uznawane za należycie zabezpieczone globalnie wymagających protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af68d-153">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="af68d-154">Nie można zagwarantować `[RequireHttps]` atrybut jest stosowany podczas dodawania nowych kontrolerów i stron Razor.</span><span class="sxs-lookup"><span data-stu-id="af68d-154">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="af68d-155">Protokół zabezpieczeń Strict transportu HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="af68d-155">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="af68d-156">Na [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [zabezpieczeń transportu HTTP ograniczeniami (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) jest ulepszeniem zabezpieczeń opcjonalnych w określonym przez aplikację sieci web przy użyciu nagłówka odpowiedzi specjalnych.</span><span class="sxs-lookup"><span data-stu-id="af68d-156">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="af68d-157">Kiedy obsługiwanej przeglądarki odbiera ten nagłówek przeglądarka uniemożliwi komunikację z są wysyłane za pośrednictwem protokołu HTTP z określoną domeną i zamiast tego wyśle cała komunikacja za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af68d-157">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="af68d-158">Uniemożliwia także kliknij HTTPS za pomocą monity w przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="af68d-158">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="af68d-159">Platformy ASP.NET Core 2.1 lub nowszej implementuje HSTS z `UseHsts` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="af68d-159">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="af68d-160">Poniższy kod wywołania `UseHsts` gdy aplikacja nie jest w [tryb programowania](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="af68d-160">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="af68d-161">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="af68d-161">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="af68d-162">`UseHsts` nie jest zalecane w rozwoju ponieważ nagłówek HSTS jest wysoce cachable przez przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="af68d-162">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="af68d-163">Domyślnie UseHsts wyklucza adresu lokalnego sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="af68d-163">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="af68d-164">Następujący kod:</span><span class="sxs-lookup"><span data-stu-id="af68d-164">The following code:</span></span>

<span data-ttu-id="af68d-165">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="af68d-165">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="af68d-166">Ustawia parametr wstępnego ładowania nagłówka Strict TLS.</span><span class="sxs-lookup"><span data-stu-id="af68d-166">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="af68d-167">Wstępnego ładowania nie jest częścią [specyfikacji RFC HSTS](https://tools.ietf.org/html/rfc6797), ale nie jest obsługiwana przez przeglądarki sieci web wstępnie HSTS witryn na nowej instalacji.</span><span class="sxs-lookup"><span data-stu-id="af68d-167">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="af68d-168">Zobacz [ https://hstspreload.org/ ](https://hstspreload.org/) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="af68d-168">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="af68d-169">Umożliwia [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), która ma zastosowanie zasad HSTS poddomen hosta.</span><span class="sxs-lookup"><span data-stu-id="af68d-169">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="af68d-170">Jawnie Ustawia maksymalny wiek parametr nagłówka TLS Strict do 60 dni.</span><span class="sxs-lookup"><span data-stu-id="af68d-170">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="af68d-171">Jeśli nie zostanie ustawiona wartość domyślna to 30 dni.</span><span class="sxs-lookup"><span data-stu-id="af68d-171">If not set, defaults to 30 days.</span></span> <span data-ttu-id="af68d-172">Zobacz [dyrektywy maksymalny wiek](https://tools.ietf.org/html/rfc6797#section-6.1.1) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="af68d-172">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="af68d-173">Dodaje `example.com` do listy hostów, które mają zostać wykluczone.</span><span class="sxs-lookup"><span data-stu-id="af68d-173">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="af68d-174">`UseHsts` nie obejmuje następujące hosty sprzężenie zwrotne:</span><span class="sxs-lookup"><span data-stu-id="af68d-174">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="af68d-175">`localhost` Adres sprzężenia zwrotnego protokołu IPv4.</span><span class="sxs-lookup"><span data-stu-id="af68d-175">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="af68d-176">`127.0.0.1` Adres sprzężenia zwrotnego protokołu IPv4.</span><span class="sxs-lookup"><span data-stu-id="af68d-176">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="af68d-177">`[::1]` Adres sprzężenia zwrotnego protokołu IPv6.</span><span class="sxs-lookup"><span data-stu-id="af68d-177">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="af68d-178">Powyższy przykład przedstawia sposób dodawania dodatkowych hostów.</span><span class="sxs-lookup"><span data-stu-id="af68d-178">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="af68d-179">Zrezygnować z protokołu HTTPS na tworzenie projektu</span><span class="sxs-lookup"><span data-stu-id="af68d-179">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="af68d-180">Włącz szablony aplikacji sieci web 2.1 lub nowszej platformy ASP.NET Core (z programu Visual Studio lub wiersza polecenia platformy dotnet) [przekierowania HTTPS](#require) i [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="af68d-180">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="af68d-181">W przypadku wdrożeń, które nie wymagają protokołu HTTPS użytkownik może zrezygnować z protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af68d-181">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="af68d-182">Na przykład niektóre usługi wewnętrznej bazy danych, gdy HTTPS jest obsługiwane zewnętrznie na krawędzi, przy użyciu protokołu HTTPS w każdym węźle nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="af68d-182">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="af68d-183">Aby zrezygnować z protokołu HTTPS:</span><span class="sxs-lookup"><span data-stu-id="af68d-183">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="af68d-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="af68d-184">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="af68d-185">Usuń zaznaczenie pola wyboru **Konfiguruj na potrzeby protokołu HTTPS** wyboru.</span><span class="sxs-lookup"><span data-stu-id="af68d-185">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagram jednostek](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="af68d-187">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="af68d-187">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="af68d-188">Użyj `--no-https` opcji.</span><span class="sxs-lookup"><span data-stu-id="af68d-188">Use the `--no-https` option.</span></span> <span data-ttu-id="af68d-189">Na przykład</span><span class="sxs-lookup"><span data-stu-id="af68d-189">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="af68d-190">Jak skonfigurować certyfikat dewelopera dla Docker</span><span class="sxs-lookup"><span data-stu-id="af68d-190">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="af68d-191">Zobacz [ten problem GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="af68d-191">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
