---
title: Wymuszanie protokołu HTTPS w programie ASP.NET Core
author: rick-anderson
description: Pokazuje, jak HTTPS/TLS w programie ASP.NET Core wymagają aplikacji sieci web.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: d8bf11d7d2df8d8b197f001570a8fab1f3262814
ms.sourcegitcommit: 4e34ce61e1e7f1317102b16012ce0742abf2cca6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514807"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="5a093-103">Wymuszanie protokołu HTTPS w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5a093-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="5a093-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5a093-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5a093-105">W tym dokumencie przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="5a093-105">This document shows how to:</span></span>

* <span data-ttu-id="5a093-106">Wymaganie protokołu HTTPS dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="5a093-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="5a093-107">Przekieruj wszystkie żądania HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5a093-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="5a093-108">Czy **nie** użyj [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) na interfejsy API sieci Web, która odbierać poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="5a093-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="5a093-109">`RequireHttpsAttribute` używa kodów stanu HTTP do przekierowania przeglądarki z protokołu HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5a093-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="5a093-110">Klienci interfejsu API może zrozumieć lub nie przestrzegają przekierowuje z protokołu HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5a093-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="5a093-111">Tacy klienci mogą wysłać informacje za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="5a093-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="5a093-112">Interfejsy API sieci Web powinien:</span><span class="sxs-lookup"><span data-stu-id="5a093-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="5a093-113">Nasłuchuj od protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="5a093-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="5a093-114">Zamknij połączenie z kodem stanu 400 (złe żądanie), a nie obsłużyć żądania.</span><span class="sxs-lookup"><span data-stu-id="5a093-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="5a093-115">Wymaganie protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="5a093-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5a093-116">Firma Microsoft zaleca wszystkich aplikacji sieci web platformy ASP.NET Core wywołać oprogramowanie pośredniczące przekierowania protokołu HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) aby przekierować wszystkie żądania HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5a093-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="5a093-117">Poniższy kod wywoła `UseHttpsRedirection` w `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="5a093-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="5a093-118">Wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="5a093-118">The preceding highlighted code:</span></span>

* <span data-ttu-id="5a093-119">Używa domyślnej [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span><span class="sxs-lookup"><span data-stu-id="5a093-119">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span> <span data-ttu-id="5a093-120">Aplikacje produkcyjne powinny wywoływać [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="5a093-120">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="5a093-121">Używa domyślnej [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span><span class="sxs-lookup"><span data-stu-id="5a093-121">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span></span>

<span data-ttu-id="5a093-122">Poniższy kod wywoła [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) skonfigurować opcje oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="5a093-122">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="5a093-123">Wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="5a093-123">The preceding highlighted code:</span></span>

* <span data-ttu-id="5a093-124">Zestawy [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) do `Status307TemporaryRedirect`, która jest wartością domyślną.</span><span class="sxs-lookup"><span data-stu-id="5a093-124">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="5a093-125">Aplikacje produkcyjne powinny wywoływać [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="5a093-125">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="5a093-126">Ustawia HTTPS port 5001.</span><span class="sxs-lookup"><span data-stu-id="5a093-126">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="5a093-127">Wartość domyślna to 443.</span><span class="sxs-lookup"><span data-stu-id="5a093-127">The default value is 443.</span></span>

<span data-ttu-id="5a093-128">Automatycznie Ustaw port przez następujących mechanizmów:</span><span class="sxs-lookup"><span data-stu-id="5a093-128">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="5a093-129">Oprogramowanie pośredniczące może odnajdować portów za pomocą [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) gdy są spełnione następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="5a093-129">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="5a093-130">Kestrel lub sterownik HTTP.sys jest używana bezpośrednio z punktów końcowych HTTPS (dotyczy także uruchamianie aplikacji za pomocą debugera programu Visual Studio Code firmy).</span><span class="sxs-lookup"><span data-stu-id="5a093-130">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="5a093-131">Tylko **jeden port HTTPS** jest używany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="5a093-131">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="5a093-132">Program Visual Studio jest używany:</span><span class="sxs-lookup"><span data-stu-id="5a093-132">Visual Studio is used:</span></span>
  - <span data-ttu-id="5a093-133">Usługi IIS Express ma obsługujące protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5a093-133">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="5a093-134">*launchSettings.json* ustawia `sslPort` dla usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="5a093-134">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="5a093-135">Gdy aplikacja jest uruchamiana za zwrotny serwer proxy (na przykład, IIS, usługi IIS Express), `IServerAddressesFeature` jest niedostępna.</span><span class="sxs-lookup"><span data-stu-id="5a093-135">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="5a093-136">Numer portu musi być skonfigurowany ręcznie.</span><span class="sxs-lookup"><span data-stu-id="5a093-136">The port must be manually configured.</span></span> <span data-ttu-id="5a093-137">Jeśli port nie jest ustawiony, przekierowanie nie nastąpi żądań.</span><span class="sxs-lookup"><span data-stu-id="5a093-137">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="5a093-138">Można skonfigurować port, ustawiając [ustawienia konfiguracji hosta sieci Web https_port](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="5a093-138">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="5a093-139">**Klucz**: https_port **typu**: *ciąg*
**domyślne**: nie ustawiono wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="5a093-139">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="5a093-140">**Można ustawić przy użyciu**: `UseSetting` 
 **zmiennej środowiskowej**: `<PREFIX_>HTTPS_PORT` (prefiks jest `ASPNETCORE_` przy użyciu hosta sieci Web.)</span><span class="sxs-lookup"><span data-stu-id="5a093-140">**Set using**: `UseSetting`
**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="5a093-141">Port można skonfigurować pośrednio przez ustawienie adresu URL z `ASPNETCORE_URLS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="5a093-141">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="5a093-142">Zmienna środowiskowa konfiguruje serwer, a następnie oprogramowanie pośredniczące pośrednio odnajduje portu HTTPS za pośrednictwem `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="5a093-142">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="5a093-143">Jeśli port nie jest ustawiony:</span><span class="sxs-lookup"><span data-stu-id="5a093-143">If no port is set:</span></span>

* <span data-ttu-id="5a093-144">Żądania nie są przekierowywane.</span><span class="sxs-lookup"><span data-stu-id="5a093-144">Requests aren't redirected.</span></span>
* <span data-ttu-id="5a093-145">Oprogramowanie pośredniczące rejestruje ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="5a093-145">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="5a093-146">Alternatywa dla użycia oprogramowania pośredniczącego przekierowania protokołu HTTPS (`UseHttpsRedirection`) jest użycie oprogramowanie pośredniczące ponownego zapisywania adresów URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="5a093-146">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="5a093-147">`AddRedirectToHttps` Podczas wykonywania przekierowania, można również ustawić kod stanu i port.</span><span class="sxs-lookup"><span data-stu-id="5a093-147">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="5a093-148">Aby uzyskać więcej informacji, zobacz [oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="5a093-148">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="5a093-149">Podczas przekierowywania HTTPS bez potrzeby przekierowania dodatkowe reguły, firma Microsoft zaleca używanie oprogramowania pośredniczącego przekierowania protokołu HTTPS (`UseHttpsRedirection`) opisane w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="5a093-149">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="5a093-150">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) służy do wymagania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5a093-150">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="5a093-151">`[RequireHttpsAttribute]` można dekoracji kontrolery lub metody lub mogą być stosowane globalnie.</span><span class="sxs-lookup"><span data-stu-id="5a093-151">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="5a093-152">Aby zastosować atrybut globalnie, Dodaj następujący kod do `ConfigureServices` w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="5a093-152">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="5a093-153">Powyższy kod wyróżniony wymaga, wszystkie żądania przy użyciu `HTTPS`; dlatego żądania HTTP są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="5a093-153">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="5a093-154">Następujący wyróżniony kod przekierowuje żądania HTTP do HTTPS:</span><span class="sxs-lookup"><span data-stu-id="5a093-154">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="5a093-155">Aby uzyskać więcej informacji, zobacz [oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="5a093-155">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="5a093-156">Oprogramowanie pośredniczące pozwala również aplikację, aby ustawić kod stanu lub kod stanu i port, podczas wykonywania przekierowania.</span><span class="sxs-lookup"><span data-stu-id="5a093-156">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="5a093-157">Globalnie wymagania protokołu HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) jest ze względów bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="5a093-157">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="5a093-158">Stosowanie `[RequireHttps]` atrybutu, aby wszystkie kontrolery/Razor strony nie jest uważany za bezpieczny globalnie wymagania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5a093-158">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="5a093-159">Nie może zagwarantować `[RequireHttps]` atrybut jest stosowany podczas dodawania nowych kontrolerów i stron Razor.</span><span class="sxs-lookup"><span data-stu-id="5a093-159">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="5a093-160">Protokół zabezpieczeń Strict transportu HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="5a093-160">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="5a093-161">Na [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [zabezpieczeń transportu HTTP ograniczeniami (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) jest ulepszeniem zabezpieczeń zgłoszenie zgody na uczestnictwo w określonym przez aplikację sieci web przy użyciu nagłówka odpowiedzi specjalne.</span><span class="sxs-lookup"><span data-stu-id="5a093-161">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a special response header.</span></span> <span data-ttu-id="5a093-162">Gdy przeglądarka, która obsługuje HSTS otrzymuje tego pliku nagłówkowego, przechowuje konfigurację dla domeny, która uniemożliwia wysłanie Każde powiadomienie za pośrednictwem protokołu HTTP i zamiast tego wymuszenie całą komunikację za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5a093-162">When a browser that supports HSTS receives this header, it stores configuration for the domain that prevents sending any communication over HTTP and instead forces all communication over HTTPS.</span></span> <span data-ttu-id="5a093-163">Uniemożliwia także użytkownikowi z korzystania z certyfikatów niezaufanych lub jest nieprawidłowy, wyłączenie monitów przeglądarki, które umożliwiają użytkownikowi tymczasowo ufać takiego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="5a093-163">It also prevents the user from using untrusted or invalid certificates, disabling the browser prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="5a093-164">Platforma ASP.NET Core 2.1 lub nowszej implementuje HSTS z `UseHsts` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="5a093-164">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="5a093-165">Poniższy kod wywoła `UseHsts` gdy aplikacja nie jest [trybu opracowywania](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="5a093-165">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="5a093-166">`UseHsts` nie jest zalecane w rozwoju, ponieważ nagłówek HSTS jest wysoce podlega buforowaniu przez przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="5a093-166">`UseHsts` isn't recommended in development because the HSTS header is highly cacheable by browsers.</span></span> <span data-ttu-id="5a093-167">Domyślnie `UseHsts` nie obejmuje adresu lokalnego sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="5a093-167">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="5a093-168">W środowiskach produkcyjnych, implementacja protokołu HTTPS, po raz pierwszy należy ustawić wartość początkową HSTS małej wartości.</span><span class="sxs-lookup"><span data-stu-id="5a093-168">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="5a093-169">Ustaw wartość od kilku godzin do nie więcej niż jednego dziennie w przypadku konieczności przywrócenia infrastruktury protokołu HTTPS do protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="5a093-169">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="5a093-170">Po masz pewność, że trwałości Konfiguracja protokołu HTTPS, należy zwiększyć wartość max-age HSTS; często używane wartości jest jeden rok.</span><span class="sxs-lookup"><span data-stu-id="5a093-170">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="5a093-171">Poniższy kod:</span><span class="sxs-lookup"><span data-stu-id="5a093-171">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="5a093-172">Ustawia wstępnego ładowania parametr nagłówka zabezpieczeń w przypadku transportu Strict.</span><span class="sxs-lookup"><span data-stu-id="5a093-172">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="5a093-173">Wstępne załadowanie nie jest częścią [specyfikacji RFC HSTS](https://tools.ietf.org/html/rfc6797), ale jest obsługiwane przez przeglądarki sieci web do wstępnego witryn HSTS na zainstalować od nowa.</span><span class="sxs-lookup"><span data-stu-id="5a093-173">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="5a093-174">Zobacz [ https://hstspreload.org/ ](https://hstspreload.org/) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="5a093-174">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="5a093-175">Włącza [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), co ma zastosowanie zasad HSTS poddomen hosta.</span><span class="sxs-lookup"><span data-stu-id="5a093-175">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="5a093-176">Jawnie Ustawia parametr max-age nagłówka zabezpieczeń w przypadku transportu Strict do 60 dni.</span><span class="sxs-lookup"><span data-stu-id="5a093-176">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="5a093-177">Jeśli nie zostanie ustawiona wartość domyślna to 30 dni.</span><span class="sxs-lookup"><span data-stu-id="5a093-177">If not set, defaults to 30 days.</span></span> <span data-ttu-id="5a093-178">Zobacz [dyrektywy max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="5a093-178">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="5a093-179">Dodaje `example.com` do listy hostów, które mają zostać wykluczone.</span><span class="sxs-lookup"><span data-stu-id="5a093-179">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="5a093-180">`UseHsts` nie obejmuje następujące hosty sprzężenia zwrotnego:</span><span class="sxs-lookup"><span data-stu-id="5a093-180">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="5a093-181">`localhost` Adres sprzężenia zwrotnego protokołu IPv4.</span><span class="sxs-lookup"><span data-stu-id="5a093-181">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="5a093-182">`127.0.0.1` Adres sprzężenia zwrotnego protokołu IPv4.</span><span class="sxs-lookup"><span data-stu-id="5a093-182">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="5a093-183">`[::1]` Adres sprzężenia zwrotnego protokołu IPv6.</span><span class="sxs-lookup"><span data-stu-id="5a093-183">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="5a093-184">Poprzedni przykład pokazuje, jak dodać kolejne hosty.</span><span class="sxs-lookup"><span data-stu-id="5a093-184">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="5a093-185">Zrezygnować z protokołu HTTPS przy tworzeniu projektu</span><span class="sxs-lookup"><span data-stu-id="5a093-185">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="5a093-186">Włącz szablony ASP.NET Core 2.1 lub nowszej sieci web aplikacji (z programu Visual Studio lub wiersza polecenia dotnet) [przekierowania protokołu HTTPS](#require) i [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="5a093-186">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="5a093-187">W przypadku wdrożeń, które nie wymagają protokołu HTTPS użytkownik może zrezygnować protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5a093-187">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="5a093-188">Na przykład niektóre usługi zaplecza, gdzie HTTPS jest obsługiwane zewnętrznie na urządzeniach brzegowych, przy użyciu protokołu HTTPS w każdym węźle nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="5a093-188">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="5a093-189">Aby zrezygnować z protokołu HTTPS:</span><span class="sxs-lookup"><span data-stu-id="5a093-189">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5a093-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5a093-190">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="5a093-191">Usuń zaznaczenie pola wyboru **Konfigurowanie protokołu HTTPS** pola wyboru.</span><span class="sxs-lookup"><span data-stu-id="5a093-191">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagram jednostek](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5a093-193">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="5a093-193">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="5a093-194">Użyj `--no-https` opcji.</span><span class="sxs-lookup"><span data-stu-id="5a093-194">Use the `--no-https` option.</span></span> <span data-ttu-id="5a093-195">Na przykład</span><span class="sxs-lookup"><span data-stu-id="5a093-195">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="5a093-196">Jak skonfigurować certyfikat dewelopera dla programu Docker</span><span class="sxs-lookup"><span data-stu-id="5a093-196">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="5a093-197">Zobacz [problem w usłudze GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="5a093-197">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
