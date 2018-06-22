---
title: Wymuszanie protokołu HTTPS w platformy ASP.NET Core
author: rick-anderson
description: Pokazuje, jak będą musieli HTTPS/TLS w ASP.NET Core aplikacji sieci web.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 6a16bb2253fcb6e81a294f1c484db1a3e80796e2
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277273"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="78d27-103">Wymuszanie protokołu HTTPS w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="78d27-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="78d27-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="78d27-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="78d27-105">Ten dokument zawiera jak:</span><span class="sxs-lookup"><span data-stu-id="78d27-105">This document shows how to:</span></span>

* <span data-ttu-id="78d27-106">Wymagać protokołu HTTPS dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="78d27-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="78d27-107">Przekieruj żądania HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="78d27-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="78d27-108">Czy **nie** użyj [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) na interfejsów API sieci Web, czy odbierać poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="78d27-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="78d27-109">`RequireHttpsAttribute` używa kodów stanu HTTP do przekierowania przeglądarki z protokołu HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="78d27-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="78d27-110">Klientów interfejsu API nie może zrozumieć lub przestrzegać przekierowania z protokołu HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="78d27-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="78d27-111">Tacy klienci mogą wysłać informacje za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="78d27-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="78d27-112">Interfejsy API sieci Web powinien:</span><span class="sxs-lookup"><span data-stu-id="78d27-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="78d27-113">Nie nasłuchiwania protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="78d27-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="78d27-114">Zamknięcie połączenia z kodem stanu 400 (nieprawidłowe żądanie), a nie obsługiwać żądania.</span><span class="sxs-lookup"><span data-stu-id="78d27-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="78d27-115">Wymagać protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="78d27-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="78d27-116">Firma Microsoft zaleca, wszystkie aplikacje sieci web platformy ASP.NET Core wywoła oprogramowanie pośredniczące przekierowania protokołu HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) aby przekierować wszystkie żądania HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="78d27-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="78d27-117">Poniższy kod wywołania `UseHttpsRedirection` w `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="78d27-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="78d27-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="78d27-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>

<span data-ttu-id="78d27-119">Poniższy kod wywołania [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) Aby skonfigurować opcje oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="78d27-119">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

<span data-ttu-id="78d27-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="78d27-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

<span data-ttu-id="78d27-121">Wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="78d27-121">The preceding highlighted code:</span></span>

* <span data-ttu-id="78d27-122">Ustawia [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) do `Status307TemporaryRedirect`, która jest wartością domyślną.</span><span class="sxs-lookup"><span data-stu-id="78d27-122">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="78d27-123">Aplikacje w środowisku produkcyjnym powinny wywoływać [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="78d27-123">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="78d27-124">Ustawia HTTPS port 5001.</span><span class="sxs-lookup"><span data-stu-id="78d27-124">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="78d27-125">Wartość domyślna to 443.</span><span class="sxs-lookup"><span data-stu-id="78d27-125">The default value is 443.</span></span>

<span data-ttu-id="78d27-126">Następujące mechanizmy automatycznie ustawiony port:</span><span class="sxs-lookup"><span data-stu-id="78d27-126">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="78d27-127">Oprogramowanie pośredniczące mogą odnajdować portów za pomocą [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) gdy mają zastosowanie następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="78d27-127">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="78d27-128">Kestrel lub sterownik HTTP.sys jest używana bezpośrednio z punktów końcowych HTTPS (ma zastosowanie także do uruchamiania aplikacji z debugera w programie Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="78d27-128">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="78d27-129">Tylko **jeden port HTTPS** jest używany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="78d27-129">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="78d27-130">Używany jest program Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="78d27-130">Visual Studio is used:</span></span>
  - <span data-ttu-id="78d27-131">Usługi IIS Express zawiera obsługujące protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="78d27-131">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="78d27-132">*launchSettings.json* ustawia `sslPort` przez usługi IIS Express.</span><span class="sxs-lookup"><span data-stu-id="78d27-132">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="78d27-133">Gdy aplikacja jest uruchamiana za zwrotnego serwera proxy (na przykład usług IIS, usługi IIS Express) `IServerAddressesFeature` nie jest dostępna.</span><span class="sxs-lookup"><span data-stu-id="78d27-133">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="78d27-134">Port należy skonfigurować ręcznie.</span><span class="sxs-lookup"><span data-stu-id="78d27-134">The port must be manually configured.</span></span> <span data-ttu-id="78d27-135">Jeśli port nie jest ustawiona, przekierowanie nie są żądania.</span><span class="sxs-lookup"><span data-stu-id="78d27-135">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="78d27-136">Przez ustawienie można skonfigurować port:</span><span class="sxs-lookup"><span data-stu-id="78d27-136">The port can be configured by setting the:</span></span>

* <span data-ttu-id="78d27-137">`ASPNETCORE_HTTPS_PORT` Zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="78d27-137">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="78d27-138">`http_port` Klucz konfiguracji hosta (na przykład za pomocą *hostsettings.json* lub argumentu wiersza polecenia).</span><span class="sxs-lookup"><span data-stu-id="78d27-138">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="78d27-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="78d27-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="78d27-140">Zapoznaj się z poprzednim przykładem, pokazujący sposób Ustaw port 5001.</span><span class="sxs-lookup"><span data-stu-id="78d27-140">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="78d27-141">Port można skonfigurować pośrednio przez ustawienie adresu URL z `ASPNETCORE_URLS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="78d27-141">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="78d27-142">Zmienna środowiskowa konfiguruje serwer, a następnie oprogramowanie pośredniczące pośrednio odnajduje port HTTPS za pomocą `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="78d27-142">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="78d27-143">Jeśli port nie jest ustawiony:</span><span class="sxs-lookup"><span data-stu-id="78d27-143">If no port is set:</span></span>

* <span data-ttu-id="78d27-144">Nie można przekierować żądania.</span><span class="sxs-lookup"><span data-stu-id="78d27-144">Requests aren't redirected.</span></span>
* <span data-ttu-id="78d27-145">Oprogramowanie pośredniczące rejestruje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="78d27-145">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="78d27-146">Alternatywa dla użycia oprogramowania pośredniczącego przekierowania protokołu HTTPS (`UseHttpsRedirection`) jest użycie ponowne zapisywanie adresów URL w oprogramowaniu pośredniczącym (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="78d27-146">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="78d27-147">`AddRedirectToHttps` Podczas wykonywania przekierowania, można również ustawić kod stanu i portu.</span><span class="sxs-lookup"><span data-stu-id="78d27-147">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="78d27-148">Aby uzyskać więcej informacji, zobacz [ponowne zapisywanie adresów URL w oprogramowaniu pośredniczącym](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="78d27-148">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="78d27-149">Gdy przekierowywany do protokołu HTTPS, bez potrzeby przekierowania dodatkowe reguły, firma Microsoft zaleca używanie oprogramowania pośredniczącego przekierowania protokołu HTTPS (`UseHttpsRedirection`) opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="78d27-149">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="78d27-150">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) jest używana, aby wymagać protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="78d27-150">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="78d27-151">`[RequireHttpsAttribute]` można dekoracji kontrolerów lub metody lub mogą być stosowane globalnie.</span><span class="sxs-lookup"><span data-stu-id="78d27-151">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="78d27-152">Aby zastosować atrybut globalny, Dodaj następujący kod do `ConfigureServices` w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="78d27-152">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="78d27-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="78d27-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="78d27-154">Poprzedni wyróżniony kod wymaga wszystkie żądania przy użyciu `HTTPS`; w związku z tym żądania HTTP są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="78d27-154">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="78d27-155">Następujący wyróżniony kod przekierowuje żądania HTTP, https:</span><span class="sxs-lookup"><span data-stu-id="78d27-155">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="78d27-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="78d27-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="78d27-157">Aby uzyskać więcej informacji, zobacz [ponowne zapisywanie adresów URL w oprogramowaniu pośredniczącym](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="78d27-157">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="78d27-158">Oprogramowanie pośredniczące umożliwia również aplikacji, aby określić kod stanu lub kod stanu i portu podczas wykonywania przekierowania.</span><span class="sxs-lookup"><span data-stu-id="78d27-158">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="78d27-159">Globalny wymagających protokołu HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) jest ze względów bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="78d27-159">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="78d27-160">Stosowanie `[RequireHttps]` atrybut, aby wszystkie kontrolery/Razor strony nie jest uznawane za należycie zabezpieczone globalnie wymagających protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="78d27-160">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="78d27-161">Nie można zagwarantować `[RequireHttps]` atrybut jest stosowany podczas dodawania nowych kontrolerów i stron Razor.</span><span class="sxs-lookup"><span data-stu-id="78d27-161">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="78d27-162">Protokół zabezpieczeń Strict transportu HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="78d27-162">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="78d27-163">Na [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [zabezpieczeń transportu HTTP ograniczeniami (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) jest ulepszeniem zabezpieczeń opcjonalnych w określonym przez aplikację sieci web przy użyciu nagłówka odpowiedzi specjalnych.</span><span class="sxs-lookup"><span data-stu-id="78d27-163">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="78d27-164">Kiedy obsługiwanej przeglądarki odbiera ten nagłówek przeglądarka uniemożliwi komunikację z są wysyłane za pośrednictwem protokołu HTTP z określoną domeną i zamiast tego wyśle cała komunikacja za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="78d27-164">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="78d27-165">Uniemożliwia także kliknij HTTPS za pomocą monity w przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="78d27-165">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="78d27-166">Platformy ASP.NET Core 2.1 lub nowszej implementuje HSTS z `UseHsts` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="78d27-166">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="78d27-167">Poniższy kod wywołania `UseHsts` gdy aplikacja nie jest w [tryb programowania](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="78d27-167">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="78d27-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="78d27-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="78d27-169">`UseHsts` nie jest zalecane w rozwoju ponieważ nagłówek HSTS jest wysoce buforowalnej przez przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="78d27-169">`UseHsts` isn't recommended in development because the HSTS header is highly cacheable by browsers.</span></span> <span data-ttu-id="78d27-170">Domyślnie `UseHsts` nie obejmuje adresu lokalnego sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="78d27-170">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="78d27-171">Następujący kod:</span><span class="sxs-lookup"><span data-stu-id="78d27-171">The following code:</span></span>

<span data-ttu-id="78d27-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="78d27-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="78d27-173">Ustawia parametr wstępnego ładowania nagłówka Strict TLS.</span><span class="sxs-lookup"><span data-stu-id="78d27-173">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="78d27-174">Wstępnego ładowania nie jest częścią [specyfikacji RFC HSTS](https://tools.ietf.org/html/rfc6797), ale nie jest obsługiwana przez przeglądarki sieci web wstępnie HSTS witryn na nowej instalacji.</span><span class="sxs-lookup"><span data-stu-id="78d27-174">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="78d27-175">Zobacz [ https://hstspreload.org/ ](https://hstspreload.org/) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="78d27-175">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="78d27-176">Umożliwia [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), która ma zastosowanie zasad HSTS poddomen hosta.</span><span class="sxs-lookup"><span data-stu-id="78d27-176">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="78d27-177">Jawnie Ustawia maksymalny wiek parametr nagłówka TLS Strict do 60 dni.</span><span class="sxs-lookup"><span data-stu-id="78d27-177">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="78d27-178">Jeśli nie zostanie ustawiona wartość domyślna to 30 dni.</span><span class="sxs-lookup"><span data-stu-id="78d27-178">If not set, defaults to 30 days.</span></span> <span data-ttu-id="78d27-179">Zobacz [dyrektywy maksymalny wiek](https://tools.ietf.org/html/rfc6797#section-6.1.1) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="78d27-179">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="78d27-180">Dodaje `example.com` do listy hostów, które mają zostać wykluczone.</span><span class="sxs-lookup"><span data-stu-id="78d27-180">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="78d27-181">`UseHsts` nie obejmuje następujące hosty sprzężenie zwrotne:</span><span class="sxs-lookup"><span data-stu-id="78d27-181">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="78d27-182">`localhost` Adres sprzężenia zwrotnego protokołu IPv4.</span><span class="sxs-lookup"><span data-stu-id="78d27-182">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="78d27-183">`127.0.0.1` Adres sprzężenia zwrotnego protokołu IPv4.</span><span class="sxs-lookup"><span data-stu-id="78d27-183">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="78d27-184">`[::1]` Adres sprzężenia zwrotnego protokołu IPv6.</span><span class="sxs-lookup"><span data-stu-id="78d27-184">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="78d27-185">Powyższy przykład przedstawia sposób dodawania dodatkowych hostów.</span><span class="sxs-lookup"><span data-stu-id="78d27-185">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="78d27-186">Zrezygnować z protokołu HTTPS na tworzenie projektu</span><span class="sxs-lookup"><span data-stu-id="78d27-186">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="78d27-187">Włącz szablony aplikacji sieci web 2.1 lub nowszej platformy ASP.NET Core (z programu Visual Studio lub wiersza polecenia platformy dotnet) [przekierowania HTTPS](#require) i [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="78d27-187">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="78d27-188">W przypadku wdrożeń, które nie wymagają protokołu HTTPS użytkownik może zrezygnować z protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="78d27-188">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="78d27-189">Na przykład niektóre usługi wewnętrznej bazy danych, gdy HTTPS jest obsługiwane zewnętrznie na krawędzi, przy użyciu protokołu HTTPS w każdym węźle nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="78d27-189">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="78d27-190">Aby zrezygnować z protokołu HTTPS:</span><span class="sxs-lookup"><span data-stu-id="78d27-190">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="78d27-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="78d27-191">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="78d27-192">Usuń zaznaczenie pola wyboru **Konfiguruj na potrzeby protokołu HTTPS** wyboru.</span><span class="sxs-lookup"><span data-stu-id="78d27-192">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagram jednostek](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="78d27-194">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="78d27-194">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="78d27-195">Użyj `--no-https` opcji.</span><span class="sxs-lookup"><span data-stu-id="78d27-195">Use the `--no-https` option.</span></span> <span data-ttu-id="78d27-196">Na przykład</span><span class="sxs-lookup"><span data-stu-id="78d27-196">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="78d27-198">Jak skonfigurować certyfikat dewelopera dla Docker</span><span class="sxs-lookup"><span data-stu-id="78d27-198">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="78d27-199">Zobacz [ten problem GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="78d27-199">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
