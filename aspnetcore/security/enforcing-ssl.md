---
title: Wymuszanie protokołu HTTPS w programie ASP.NET Core
author: rick-anderson
description: Pokazuje, jak HTTPS/TLS w programie ASP.NET Core wymagają aplikacji sieci web.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 06ff89d30fb3586c69274cc7cb3e6c75065b098a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011329"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="85056-103">Wymuszanie protokołu HTTPS w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="85056-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="85056-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="85056-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="85056-105">W tym dokumencie przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="85056-105">This document shows how to:</span></span>

* <span data-ttu-id="85056-106">Wymaganie protokołu HTTPS dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="85056-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="85056-107">Przekieruj wszystkie żądania HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="85056-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="85056-108">Nie interfejsu API mogą uniemożliwić wysyłanie danych poufnych na pierwsze żądanie klienta.</span><span class="sxs-lookup"><span data-stu-id="85056-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="85056-109">Czy **nie** użyj [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) na interfejsy API sieci Web, która odbierać poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="85056-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="85056-110">`RequireHttpsAttribute` używa kodów stanu HTTP do przekierowania przeglądarki z protokołu HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="85056-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="85056-111">Klienci interfejsu API może zrozumieć lub nie przestrzegają przekierowuje z protokołu HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="85056-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="85056-112">Tacy klienci mogą wysłać informacje za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="85056-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="85056-113">Interfejsy API sieci Web powinien:</span><span class="sxs-lookup"><span data-stu-id="85056-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="85056-114">Nasłuchuj od protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="85056-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="85056-115">Zamknij połączenie z kodem stanu 400 (złe żądanie), a nie obsłużyć żądania.</span><span class="sxs-lookup"><span data-stu-id="85056-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="85056-116">Wymaganie protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="85056-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="85056-117">Firma Microsoft zaleca wszystkich produkcyjnym platformy ASP.NET Core wywołanie aplikacji sieci web:</span><span class="sxs-lookup"><span data-stu-id="85056-117">We recommend all production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="85056-118">Oprogramowanie pośredniczące przekierowania protokołu HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) aby przekierować wszystkie żądania HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="85056-118">The HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="85056-119">[UseHsts](#hsts), protokół zabezpieczeń Strict transportu HTTP (HSTS).</span><span class="sxs-lookup"><span data-stu-id="85056-119">[UseHsts](#hsts), HTTP Strict Transport Security Protocol (HSTS).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="85056-120">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="85056-120">UseHttpsRedirection</span></span>

<span data-ttu-id="85056-121">Poniższy kod wywoła `UseHttpsRedirection` w `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="85056-121">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="85056-122">Wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="85056-122">The preceding highlighted code:</span></span>

* <span data-ttu-id="85056-123">Używa domyślnej [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span><span class="sxs-lookup"><span data-stu-id="85056-123">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span>
* <span data-ttu-id="85056-124">Używa domyślnej [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), chyba że zostaną zastąpione `ASPNETCORE_HTTPS_PORT` zmiennej środowiskowej lub [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="85056-124">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

> [!WARNING] 
><span data-ttu-id="85056-125">Port musi być dostępny dla oprogramowania pośredniczącego do przekierowania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="85056-125">A port must be available for the middleware to redirect to HTTPS.</span></span> <span data-ttu-id="85056-126">Jeśli port nie jest dostępny, przekierowania protokołu HTTPS nie występuje.</span><span class="sxs-lookup"><span data-stu-id="85056-126">If no port is available, redirection to HTTPS does not occur.</span></span> <span data-ttu-id="85056-127">HTTPS port można określić przez jedną z następujących ustawień:</span><span class="sxs-lookup"><span data-stu-id="85056-127">The HTTPS port can be specified by any of the following setting:</span></span>
> 
>* `HttpsRedirectionOptions.HttpsPort` 
>* <span data-ttu-id="85056-128">`ASPNETCORE_HTTPS_PORT` Zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="85056-128">The `ASPNETCORE_HTTPS_PORT` environment variable.</span></span> 
>* <span data-ttu-id="85056-129">Na etapie opracowywania adresu url HTTPS w *launchsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="85056-129">In development, an HTTPS url in *launchsettings.json*.</span></span> 
>* <span data-ttu-id="85056-130">Adres url HTTPS, skonfigurować bezpośrednio na Kestrel lub słowa kluczowego HttpSys.</span><span class="sxs-lookup"><span data-stu-id="85056-130">An HTTPS url configured directly on Kestrel or HttpSys.</span></span> 

<span data-ttu-id="85056-131">Następujący wyróżniony kod wywołuje [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) skonfigurować opcje oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="85056-131">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="85056-132">Wywoływanie `AddHttpsRedirection` tylko jest konieczna zmiana wartości ` HttpsPort` lub ` RedirectStatusCode`;</span><span class="sxs-lookup"><span data-stu-id="85056-132">Calling `AddHttpsRedirection` is only necessary to change the values of ` HttpsPort` or ` RedirectStatusCode`;</span></span>

<span data-ttu-id="85056-133">Wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="85056-133">The preceding highlighted code:</span></span>

* <span data-ttu-id="85056-134">Zestawy [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) do `Status307TemporaryRedirect`, która jest wartością domyślną.</span><span class="sxs-lookup"><span data-stu-id="85056-134">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span>
* <span data-ttu-id="85056-135">Ustawia HTTPS port 5001.</span><span class="sxs-lookup"><span data-stu-id="85056-135">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="85056-136">Wartość domyślna to 443.</span><span class="sxs-lookup"><span data-stu-id="85056-136">The default value is 443.</span></span>

<span data-ttu-id="85056-137">Automatycznie Ustaw port przez następujących mechanizmów:</span><span class="sxs-lookup"><span data-stu-id="85056-137">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="85056-138">Oprogramowanie pośredniczące może odnajdować portów za pomocą [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) gdy są spełnione następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="85056-138">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>

   * <span data-ttu-id="85056-139">Kestrel lub sterownik HTTP.sys jest używana bezpośrednio z punktów końcowych HTTPS (dotyczy także uruchamianie aplikacji za pomocą debugera programu Visual Studio Code firmy).</span><span class="sxs-lookup"><span data-stu-id="85056-139">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
   * <span data-ttu-id="85056-140">Tylko **jeden port HTTPS** jest używany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="85056-140">Only **one HTTPS port** is used by the app.</span></span>

* <span data-ttu-id="85056-141">Program Visual Studio jest używany:</span><span class="sxs-lookup"><span data-stu-id="85056-141">Visual Studio is used:</span></span>
   * <span data-ttu-id="85056-142">Usługi IIS Express ma obsługujące protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="85056-142">IIS Express has HTTPS enabled.</span></span>
   * <span data-ttu-id="85056-143">*launchSettings.json* ustawia `sslPort` dla usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="85056-143">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="85056-144">Gdy aplikacja jest uruchamiana za zwrotny serwer proxy (na przykład, IIS, usługi IIS Express), `IServerAddressesFeature` jest niedostępna.</span><span class="sxs-lookup"><span data-stu-id="85056-144">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="85056-145">Numer portu musi być skonfigurowany ręcznie.</span><span class="sxs-lookup"><span data-stu-id="85056-145">The port must be manually configured.</span></span> <span data-ttu-id="85056-146">Jeśli port nie jest ustawiony, przekierowanie nie nastąpi żądań.</span><span class="sxs-lookup"><span data-stu-id="85056-146">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="85056-147">Można skonfigurować port, ustawiając [ustawienia konfiguracji hosta sieci Web https_port](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="85056-147">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="85056-148">**Klucz**: https_port</span><span class="sxs-lookup"><span data-stu-id="85056-148">**Key**: https_port</span></span>  
<span data-ttu-id="85056-149">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="85056-149">**Type**: *string*</span></span>  
<span data-ttu-id="85056-150">**Domyślne**: nie ustawiono wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="85056-150">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="85056-151">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="85056-151">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="85056-152">**Zmienna środowiskowa**: `<PREFIX_>HTTPS_PORT` (prefiks jest `ASPNETCORE_` przy użyciu hosta sieci Web.)</span><span class="sxs-lookup"><span data-stu-id="85056-152">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="85056-153">Port można skonfigurować pośrednio przez ustawienie adresu URL z `ASPNETCORE_URLS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="85056-153">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="85056-154">Zmienna środowiskowa konfiguruje serwer, a następnie oprogramowanie pośredniczące pośrednio odnajduje portu HTTPS za pośrednictwem `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="85056-154">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="85056-155">Jeśli port nie jest ustawiony:</span><span class="sxs-lookup"><span data-stu-id="85056-155">If no port is set:</span></span>

* <span data-ttu-id="85056-156">Żądania nie są przekierowywane.</span><span class="sxs-lookup"><span data-stu-id="85056-156">Requests aren't redirected.</span></span>
* <span data-ttu-id="85056-157">Oprogramowanie pośredniczące rejestruje ostrzeżenie "Nie można określić port https dla przekierowania."</span><span class="sxs-lookup"><span data-stu-id="85056-157">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

> [!NOTE]
> <span data-ttu-id="85056-158">Alternatywa dla użycia oprogramowania pośredniczącego przekierowania protokołu HTTPS (`UseHttpsRedirection`) jest użycie oprogramowanie pośredniczące ponownego zapisywania adresów URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="85056-158">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="85056-159">`AddRedirectToHttps` Podczas wykonywania przekierowania, można również ustawić kod stanu i port.</span><span class="sxs-lookup"><span data-stu-id="85056-159">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="85056-160">Aby uzyskać więcej informacji, zobacz [oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="85056-160">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="85056-161">Podczas przekierowywania HTTPS bez potrzeby przekierowania dodatkowe reguły, firma Microsoft zaleca używanie oprogramowania pośredniczącego przekierowania protokołu HTTPS (`UseHttpsRedirection`) opisane w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="85056-161">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="85056-162">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) służy do wymagania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="85056-162">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="85056-163">`[RequireHttpsAttribute]` można dekoracji kontrolery lub metody lub mogą być stosowane globalnie.</span><span class="sxs-lookup"><span data-stu-id="85056-163">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="85056-164">Aby zastosować atrybut globalnie, Dodaj następujący kod do `ConfigureServices` w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="85056-164">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="85056-165">Powyższy kod wyróżniony wymaga, wszystkie żądania przy użyciu `HTTPS`; dlatego żądania HTTP są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="85056-165">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="85056-166">Następujący wyróżniony kod przekierowuje żądania HTTP do HTTPS:</span><span class="sxs-lookup"><span data-stu-id="85056-166">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="85056-167">Aby uzyskać więcej informacji, zobacz [oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="85056-167">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="85056-168">Oprogramowanie pośredniczące pozwala również aplikację, aby ustawić kod stanu lub kod stanu i port, podczas wykonywania przekierowania.</span><span class="sxs-lookup"><span data-stu-id="85056-168">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="85056-169">Globalnie wymagania protokołu HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) jest ze względów bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="85056-169">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="85056-170">Stosowanie `[RequireHttps]` atrybutu, aby wszystkie kontrolery/Razor strony nie jest uważany za bezpieczny globalnie wymagania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="85056-170">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="85056-171">Nie może zagwarantować `[RequireHttps]` atrybut jest stosowany podczas dodawania nowych kontrolerów i stron Razor.</span><span class="sxs-lookup"><span data-stu-id="85056-171">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="85056-172">Protokół zabezpieczeń Strict transportu HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="85056-172">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="85056-173">Na [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [zabezpieczeń transportu HTTP ograniczeniami (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) jest ulepszeniem zabezpieczeń zgłoszenie zgody na uczestnictwo w określonym przez aplikację sieci web przy użyciu nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="85056-173">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="85056-174">Gdy [przeglądarki obsługującej HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) odbiera tego nagłówka:</span><span class="sxs-lookup"><span data-stu-id="85056-174">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="85056-175">Przeglądarka przechowuje konfigurację dla domeny, która uniemożliwia wysłanie Każde powiadomienie za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="85056-175">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="85056-176">Przeglądarka wymusza całą komunikację za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="85056-176">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="85056-177">Przeglądarka uniemożliwia użytkownikowi za pomocą certyfikatów niezaufanych lub jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="85056-177">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="85056-178">Przeglądarka wyłącza monity, które umożliwiają użytkownikowi tymczasowo ufać takiego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="85056-178">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="85056-179">Ponieważ HSTS jest wymuszana przez klienta ma pewne ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="85056-179">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="85056-180">Klient musi obsługiwać HSTS.</span><span class="sxs-lookup"><span data-stu-id="85056-180">The client must support HSTS.</span></span>
* <span data-ttu-id="85056-181">HSTS wymaga co najmniej jeden pomyślne żądanie HTTPS do ustanowienia zasad HSTS.</span><span class="sxs-lookup"><span data-stu-id="85056-181">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="85056-182">Aplikacja musi sprawdzić każdego żądania HTTP i przekierowania lub odrzucanie żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="85056-182">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="85056-183">Platforma ASP.NET Core 2.1 lub nowszej implementuje HSTS z `UseHsts` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="85056-183">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="85056-184">Poniższy kod wywoła `UseHsts` gdy aplikacja nie jest [trybu opracowywania](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="85056-184">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="85056-185">`UseHsts` nie jest zalecane w rozwoju, ponieważ ustawienia HSTS są wysoce podlega buforowaniu przez przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="85056-185">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="85056-186">Domyślnie `UseHsts` nie obejmuje adresu lokalnego sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="85056-186">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="85056-187">W środowiskach produkcyjnych, implementacja protokołu HTTPS, po raz pierwszy należy ustawić wartość początkową HSTS małej wartości.</span><span class="sxs-lookup"><span data-stu-id="85056-187">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="85056-188">Ustaw wartość od kilku godzin do nie więcej niż jednego dziennie w przypadku konieczności przywrócenia infrastruktury protokołu HTTPS do protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="85056-188">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="85056-189">Po masz pewność, że trwałości Konfiguracja protokołu HTTPS, należy zwiększyć wartość max-age HSTS; często używane wartości jest jeden rok.</span><span class="sxs-lookup"><span data-stu-id="85056-189">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="85056-190">Poniższy kod:</span><span class="sxs-lookup"><span data-stu-id="85056-190">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="85056-191">Ustawia wstępnego ładowania parametr nagłówka zabezpieczeń w przypadku transportu Strict.</span><span class="sxs-lookup"><span data-stu-id="85056-191">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="85056-192">Wstępnego ładowania nie jest częścią [specyfikacji RFC HSTS](https://tools.ietf.org/html/rfc6797), ale jest obsługiwane przez przeglądarki sieci web do wstępnego witryn HSTS na zainstalować od nowa.</span><span class="sxs-lookup"><span data-stu-id="85056-192">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="85056-193">Zobacz [ https://hstspreload.org/ ](https://hstspreload.org/) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="85056-193">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="85056-194">Włącza [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), co ma zastosowanie zasad HSTS poddomen hosta.</span><span class="sxs-lookup"><span data-stu-id="85056-194">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="85056-195">Jawnie Ustawia parametr max-age nagłówka zabezpieczeń w przypadku transportu Strict do 60 dni.</span><span class="sxs-lookup"><span data-stu-id="85056-195">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="85056-196">Jeśli nie zostanie ustawiona wartość domyślna to 30 dni.</span><span class="sxs-lookup"><span data-stu-id="85056-196">If not set, defaults to 30 days.</span></span> <span data-ttu-id="85056-197">Zobacz [dyrektywy max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="85056-197">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="85056-198">Dodaje `example.com` do listy hostów, które mają zostać wykluczone.</span><span class="sxs-lookup"><span data-stu-id="85056-198">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="85056-199">`UseHsts` nie obejmuje następujące hosty sprzężenia zwrotnego:</span><span class="sxs-lookup"><span data-stu-id="85056-199">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="85056-200">`localhost` Adres sprzężenia zwrotnego protokołu IPv4.</span><span class="sxs-lookup"><span data-stu-id="85056-200">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="85056-201">`127.0.0.1` Adres sprzężenia zwrotnego protokołu IPv4.</span><span class="sxs-lookup"><span data-stu-id="85056-201">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="85056-202">`[::1]` Adres sprzężenia zwrotnego protokołu IPv6.</span><span class="sxs-lookup"><span data-stu-id="85056-202">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="85056-203">Poprzedni przykład pokazuje, jak dodać kolejne hosty.</span><span class="sxs-lookup"><span data-stu-id="85056-203">The preceding example shows how to add additional hosts.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="85056-204">Zrezygnować z protokołu HTTPS przy tworzeniu projektu</span><span class="sxs-lookup"><span data-stu-id="85056-204">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="85056-205">Włącz szablony ASP.NET Core 2.1 lub nowszej sieci web aplikacji (z programu Visual Studio lub wiersza polecenia dotnet) [przekierowania protokołu HTTPS](#require) i [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="85056-205">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="85056-206">W przypadku wdrożeń, które nie wymagają protokołu HTTPS użytkownik może zrezygnować protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="85056-206">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="85056-207">Na przykład niektóre usługi zaplecza, gdzie HTTPS jest obsługiwane zewnętrznie na urządzeniach brzegowych, przy użyciu protokołu HTTPS w każdym węźle nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="85056-207">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node isn't needed.</span></span>

<span data-ttu-id="85056-208">Aby zrezygnować z protokołu HTTPS:</span><span class="sxs-lookup"><span data-stu-id="85056-208">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="85056-209">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="85056-209">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="85056-210">Usuń zaznaczenie pola wyboru **Konfigurowanie protokołu HTTPS** pola wyboru.</span><span class="sxs-lookup"><span data-stu-id="85056-210">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagram jednostek](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="85056-212">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="85056-212">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="85056-213">Użyj `--no-https` opcji.</span><span class="sxs-lookup"><span data-stu-id="85056-213">Use the `--no-https` option.</span></span> <span data-ttu-id="85056-214">Na przykład</span><span class="sxs-lookup"><span data-stu-id="85056-214">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="85056-215">Jak skonfigurować certyfikat dewelopera dla programu Docker</span><span class="sxs-lookup"><span data-stu-id="85056-215">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="85056-216">Zobacz [problem w usłudze GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="85056-216">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="85056-217">Dodatkowe informacje</span><span class="sxs-lookup"><span data-stu-id="85056-217">Additional information</span></span>

* [<span data-ttu-id="85056-218">Obsługa przeglądarek OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="85056-218">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
