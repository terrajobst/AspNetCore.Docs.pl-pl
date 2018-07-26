---
title: Wymuszanie protokołu HTTPS w programie ASP.NET Core
author: rick-anderson
description: Pokazuje, jak HTTPS/TLS w programie ASP.NET Core wymagają aplikacji sieci web.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: c3d92994c0331b1408e246953454910ca1f4dc43
ms.sourcegitcommit: c8e62aa766641aa55105f7db79cdf2b27a6e5977
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39254834"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="0c127-103">Wymuszanie protokołu HTTPS w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c127-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="0c127-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0c127-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0c127-105">W tym dokumencie przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="0c127-105">This document shows how to:</span></span>

* <span data-ttu-id="0c127-106">Wymaganie protokołu HTTPS dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="0c127-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="0c127-107">Przekieruj wszystkie żądania HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0c127-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="0c127-108">Czy **nie** użyj [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) na interfejsy API sieci Web, która odbierać poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="0c127-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="0c127-109">`RequireHttpsAttribute` używa kodów stanu HTTP do przekierowania przeglądarki z protokołu HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0c127-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="0c127-110">Klienci interfejsu API może zrozumieć lub nie przestrzegają przekierowuje z protokołu HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0c127-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="0c127-111">Tacy klienci mogą wysłać informacje za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="0c127-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="0c127-112">Interfejsy API sieci Web powinien:</span><span class="sxs-lookup"><span data-stu-id="0c127-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="0c127-113">Nasłuchuj od protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="0c127-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="0c127-114">Zamknij połączenie z kodem stanu 400 (złe żądanie), a nie obsłużyć żądania.</span><span class="sxs-lookup"><span data-stu-id="0c127-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="0c127-115">Wymaganie protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="0c127-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0c127-116">Firma Microsoft zaleca wszystkich aplikacji sieci web platformy ASP.NET Core wywołać oprogramowanie pośredniczące przekierowania protokołu HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) aby przekierować wszystkie żądania HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0c127-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="0c127-117">Poniższy kod wywoła `UseHttpsRedirection` w `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="0c127-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="0c127-118">Wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="0c127-118">The preceding highlighted code:</span></span>

* <span data-ttu-id="0c127-119">Używa domyślnej [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span><span class="sxs-lookup"><span data-stu-id="0c127-119">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span> <span data-ttu-id="0c127-120">Aplikacje produkcyjne powinny wywoływać [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="0c127-120">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="0c127-121">Używa domyślnej [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span><span class="sxs-lookup"><span data-stu-id="0c127-121">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span></span>

<span data-ttu-id="0c127-122">Poniższy kod wywoła [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) skonfigurować opcje oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="0c127-122">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="0c127-123">Wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="0c127-123">The preceding highlighted code:</span></span>

* <span data-ttu-id="0c127-124">Zestawy [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) do `Status307TemporaryRedirect`, która jest wartością domyślną.</span><span class="sxs-lookup"><span data-stu-id="0c127-124">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="0c127-125">Aplikacje produkcyjne powinny wywoływać [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="0c127-125">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="0c127-126">Ustawia HTTPS port 5001.</span><span class="sxs-lookup"><span data-stu-id="0c127-126">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="0c127-127">Wartość domyślna to 443.</span><span class="sxs-lookup"><span data-stu-id="0c127-127">The default value is 443.</span></span>

<span data-ttu-id="0c127-128">Automatycznie Ustaw port przez następujących mechanizmów:</span><span class="sxs-lookup"><span data-stu-id="0c127-128">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="0c127-129">Oprogramowanie pośredniczące może odnajdować portów za pomocą [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) gdy są spełnione następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="0c127-129">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="0c127-130">Kestrel lub sterownik HTTP.sys jest używana bezpośrednio z punktów końcowych HTTPS (dotyczy także uruchamianie aplikacji za pomocą debugera programu Visual Studio Code firmy).</span><span class="sxs-lookup"><span data-stu-id="0c127-130">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="0c127-131">Tylko **jeden port HTTPS** jest używany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="0c127-131">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="0c127-132">Program Visual Studio jest używany:</span><span class="sxs-lookup"><span data-stu-id="0c127-132">Visual Studio is used:</span></span>
  - <span data-ttu-id="0c127-133">Usługi IIS Express ma obsługujące protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0c127-133">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="0c127-134">*launchSettings.json* ustawia `sslPort` dla usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="0c127-134">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="0c127-135">Gdy aplikacja jest uruchamiana za zwrotny serwer proxy (na przykład, IIS, usługi IIS Express), `IServerAddressesFeature` jest niedostępna.</span><span class="sxs-lookup"><span data-stu-id="0c127-135">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="0c127-136">Numer portu musi być skonfigurowany ręcznie.</span><span class="sxs-lookup"><span data-stu-id="0c127-136">The port must be manually configured.</span></span> <span data-ttu-id="0c127-137">Jeśli port nie jest ustawiony, przekierowanie nie nastąpi żądań.</span><span class="sxs-lookup"><span data-stu-id="0c127-137">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="0c127-138">Przez ustawienie można skonfigurować port:</span><span class="sxs-lookup"><span data-stu-id="0c127-138">The port can be configured by setting the:</span></span>

* <span data-ttu-id="0c127-139">`ASPNETCORE_HTTPS_PORT` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="0c127-139">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="0c127-140">`http_port` Klucz konfiguracji hosta (na przykład za pośrednictwem *hostsettings.json* lub argument wiersza polecenia).</span><span class="sxs-lookup"><span data-stu-id="0c127-140">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="0c127-141">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="0c127-141">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="0c127-142">Zobacz poprzedni przykład, który pokazuje, jak i Ustaw port 5001.</span><span class="sxs-lookup"><span data-stu-id="0c127-142">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="0c127-143">Port można skonfigurować pośrednio przez ustawienie adresu URL z `ASPNETCORE_URLS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="0c127-143">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="0c127-144">Zmienna środowiskowa konfiguruje serwer, a następnie oprogramowanie pośredniczące pośrednio odnajduje portu HTTPS za pośrednictwem `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="0c127-144">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="0c127-145">Jeśli port nie jest ustawiony:</span><span class="sxs-lookup"><span data-stu-id="0c127-145">If no port is set:</span></span>

* <span data-ttu-id="0c127-146">Żądania nie są przekierowywane.</span><span class="sxs-lookup"><span data-stu-id="0c127-146">Requests aren't redirected.</span></span>
* <span data-ttu-id="0c127-147">Oprogramowanie pośredniczące rejestruje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="0c127-147">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="0c127-148">Alternatywa dla użycia oprogramowania pośredniczącego przekierowania protokołu HTTPS (`UseHttpsRedirection`) jest użycie oprogramowanie pośredniczące ponownego zapisywania adresów URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="0c127-148">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="0c127-149">`AddRedirectToHttps` Podczas wykonywania przekierowania, można również ustawić kod stanu i port.</span><span class="sxs-lookup"><span data-stu-id="0c127-149">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="0c127-150">Aby uzyskać więcej informacji, zobacz [oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="0c127-150">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="0c127-151">Podczas przekierowywania HTTPS bez potrzeby przekierowania dodatkowe reguły, firma Microsoft zaleca używanie oprogramowania pośredniczącego przekierowania protokołu HTTPS (`UseHttpsRedirection`) opisane w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="0c127-151">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="0c127-152">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) służy do wymagania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0c127-152">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="0c127-153">`[RequireHttpsAttribute]` można dekoracji kontrolery lub metody lub mogą być stosowane globalnie.</span><span class="sxs-lookup"><span data-stu-id="0c127-153">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="0c127-154">Aby zastosować atrybut globalnie, Dodaj następujący kod do `ConfigureServices` w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="0c127-154">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="0c127-155">Powyższy kod wyróżniony wymaga, wszystkie żądania przy użyciu `HTTPS`; dlatego żądania HTTP są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="0c127-155">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="0c127-156">Następujący wyróżniony kod przekierowuje żądania HTTP do HTTPS:</span><span class="sxs-lookup"><span data-stu-id="0c127-156">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="0c127-157">Aby uzyskać więcej informacji, zobacz [oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="0c127-157">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="0c127-158">Oprogramowanie pośredniczące pozwala również aplikację, aby ustawić kod stanu lub kod stanu i port, podczas wykonywania przekierowania.</span><span class="sxs-lookup"><span data-stu-id="0c127-158">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="0c127-159">Globalnie wymagania protokołu HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) jest ze względów bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="0c127-159">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="0c127-160">Stosowanie `[RequireHttps]` atrybutu, aby wszystkie kontrolery/Razor strony nie jest uważany za bezpieczny globalnie wymagania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0c127-160">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="0c127-161">Nie może zagwarantować `[RequireHttps]` atrybut jest stosowany podczas dodawania nowych kontrolerów i stron Razor.</span><span class="sxs-lookup"><span data-stu-id="0c127-161">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="0c127-162">Protokół zabezpieczeń Strict transportu HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="0c127-162">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="0c127-163">Na [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [zabezpieczeń transportu HTTP ograniczeniami (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) jest ulepszeniem zabezpieczeń zgłoszenie zgody na uczestnictwo w określonym przez aplikację sieci web przy użyciu nagłówka odpowiedzi specjalne.</span><span class="sxs-lookup"><span data-stu-id="0c127-163">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="0c127-164">Po obsługiwanej przeglądarki odbiera tego pliku nagłówkowego tej przeglądarki uniemożliwi każdej komunikacji z wysyłanych za pośrednictwem protokołu HTTP do określonej domeny i zamiast tego będzie wysyłać całej komunikacji za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0c127-164">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="0c127-165">Uniemożliwia ona kliknij HTTPS za pośrednictwem monity w przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="0c127-165">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="0c127-166">Platforma ASP.NET Core 2.1 lub nowszej implementuje HSTS z `UseHsts` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="0c127-166">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="0c127-167">Poniższy kod wywoła `UseHsts` gdy aplikacja nie jest [trybu opracowywania](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="0c127-167">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="0c127-168">`UseHsts` nie jest zalecane w rozwoju, ponieważ nagłówek HSTS jest wysoce podlega buforowaniu przez przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0c127-168">`UseHsts` isn't recommended in development because the HSTS header is highly cacheable by browsers.</span></span> <span data-ttu-id="0c127-169">Domyślnie `UseHsts` nie obejmuje adresu lokalnego sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="0c127-169">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="0c127-170">Poniższy kod:</span><span class="sxs-lookup"><span data-stu-id="0c127-170">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="0c127-171">Ustawia wstępnego ładowania parametr nagłówka zabezpieczeń w przypadku transportu Strict.</span><span class="sxs-lookup"><span data-stu-id="0c127-171">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="0c127-172">Wstępne załadowanie nie jest częścią [specyfikacji RFC HSTS](https://tools.ietf.org/html/rfc6797), ale jest obsługiwane przez przeglądarki sieci web do wstępnego witryn HSTS na zainstalować od nowa.</span><span class="sxs-lookup"><span data-stu-id="0c127-172">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="0c127-173">Zobacz [ https://hstspreload.org/ ](https://hstspreload.org/) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="0c127-173">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="0c127-174">Włącza [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), co ma zastosowanie zasad HSTS poddomen hosta.</span><span class="sxs-lookup"><span data-stu-id="0c127-174">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="0c127-175">Jawnie Ustawia parametr max-age nagłówka zabezpieczeń w przypadku transportu Strict do 60 dni.</span><span class="sxs-lookup"><span data-stu-id="0c127-175">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="0c127-176">Jeśli nie zostanie ustawiona wartość domyślna to 30 dni.</span><span class="sxs-lookup"><span data-stu-id="0c127-176">If not set, defaults to 30 days.</span></span> <span data-ttu-id="0c127-177">Zobacz [dyrektywy max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="0c127-177">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="0c127-178">Dodaje `example.com` do listy hostów, które mają zostać wykluczone.</span><span class="sxs-lookup"><span data-stu-id="0c127-178">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="0c127-179">`UseHsts` nie obejmuje następujące hosty sprzężenia zwrotnego:</span><span class="sxs-lookup"><span data-stu-id="0c127-179">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="0c127-180">`localhost` Adres sprzężenia zwrotnego protokołu IPv4.</span><span class="sxs-lookup"><span data-stu-id="0c127-180">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="0c127-181">`127.0.0.1` Adres sprzężenia zwrotnego protokołu IPv4.</span><span class="sxs-lookup"><span data-stu-id="0c127-181">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="0c127-182">`[::1]` Adres sprzężenia zwrotnego protokołu IPv6.</span><span class="sxs-lookup"><span data-stu-id="0c127-182">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="0c127-183">Poprzedni przykład pokazuje, jak dodać kolejne hosty.</span><span class="sxs-lookup"><span data-stu-id="0c127-183">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="0c127-184">Zrezygnować z protokołu HTTPS przy tworzeniu projektu</span><span class="sxs-lookup"><span data-stu-id="0c127-184">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="0c127-185">Włącz szablony ASP.NET Core 2.1 lub nowszej sieci web aplikacji (z programu Visual Studio lub wiersza polecenia dotnet) [przekierowania protokołu HTTPS](#require) i [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="0c127-185">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="0c127-186">W przypadku wdrożeń, które nie wymagają protokołu HTTPS użytkownik może zrezygnować protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0c127-186">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="0c127-187">Na przykład niektóre usługi zaplecza, gdzie HTTPS jest obsługiwane zewnętrznie na urządzeniach brzegowych, przy użyciu protokołu HTTPS w każdym węźle nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="0c127-187">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="0c127-188">Aby zrezygnować z protokołu HTTPS:</span><span class="sxs-lookup"><span data-stu-id="0c127-188">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0c127-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c127-189">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="0c127-190">Usuń zaznaczenie pola wyboru **Konfigurowanie protokołu HTTPS** pola wyboru.</span><span class="sxs-lookup"><span data-stu-id="0c127-190">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagram jednostek](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0c127-192">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="0c127-192">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="0c127-193">Użyj `--no-https` opcji.</span><span class="sxs-lookup"><span data-stu-id="0c127-193">Use the `--no-https` option.</span></span> <span data-ttu-id="0c127-194">Na przykład</span><span class="sxs-lookup"><span data-stu-id="0c127-194">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="0c127-195">Jak skonfigurować certyfikat dewelopera dla programu Docker</span><span class="sxs-lookup"><span data-stu-id="0c127-195">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="0c127-196">Zobacz [problem w usłudze GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="0c127-196">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
