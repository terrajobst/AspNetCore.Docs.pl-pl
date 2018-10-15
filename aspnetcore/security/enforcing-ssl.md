---
title: Wymuszanie protokołu HTTPS w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak wymaganie protokołu HTTPS/TLS w aplikacji sieci web platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/enforcing-ssl
ms.openlocfilehash: b4c058d3b4f00276043d9520d756e62ed8cac5d9
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325604"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="83c56-103">Wymuszanie protokołu HTTPS w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="83c56-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="83c56-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="83c56-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="83c56-105">W tym dokumencie przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="83c56-105">This document shows how to:</span></span>

* <span data-ttu-id="83c56-106">Wymaganie protokołu HTTPS dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="83c56-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="83c56-107">Przekieruj wszystkie żądania HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="83c56-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="83c56-108">Nie interfejsu API mogą uniemożliwić wysyłanie danych poufnych na pierwsze żądanie klienta.</span><span class="sxs-lookup"><span data-stu-id="83c56-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="83c56-109">Czy **nie** użyj [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) na interfejsy API sieci Web, która odbierać poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="83c56-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="83c56-110">`RequireHttpsAttribute` używa kodów stanu HTTP do przekierowania przeglądarki z protokołu HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="83c56-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="83c56-111">Klienci interfejsu API może zrozumieć lub nie przestrzegają przekierowuje z protokołu HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="83c56-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="83c56-112">Tacy klienci mogą wysłać informacje za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="83c56-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="83c56-113">Interfejsy API sieci Web powinien:</span><span class="sxs-lookup"><span data-stu-id="83c56-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="83c56-114">Nasłuchuj od protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="83c56-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="83c56-115">Zamknij połączenie z kodem stanu 400 (złe żądanie), a nie obsłużyć żądania.</span><span class="sxs-lookup"><span data-stu-id="83c56-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>

## <a name="require-https"></a><span data-ttu-id="83c56-116">Wymaganie protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="83c56-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="83c56-117">Firma Microsoft zaleca wszystkich produkcyjnym platformy ASP.NET Core wywołanie aplikacji sieci web:</span><span class="sxs-lookup"><span data-stu-id="83c56-117">We recommend all production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="83c56-118">Oprogramowanie pośredniczące przekierowania protokołu HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) aby przekierować wszystkie żądania HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="83c56-118">The HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="83c56-119">[UseHsts](#hsts), protokół zabezpieczeń Strict transportu HTTP (HSTS).</span><span class="sxs-lookup"><span data-stu-id="83c56-119">[UseHsts](#hsts), HTTP Strict Transport Security Protocol (HSTS).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="83c56-120">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="83c56-120">UseHttpsRedirection</span></span>

<span data-ttu-id="83c56-121">Poniższy kod wywoła `UseHttpsRedirection` w `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="83c56-121">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="83c56-122">Wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="83c56-122">The preceding highlighted code:</span></span>

* <span data-ttu-id="83c56-123">Używa domyślnej [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="83c56-123">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="83c56-124">Używa domyślnej [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), chyba że zostaną zastąpione `ASPNETCORE_HTTPS_PORT` zmiennej środowiskowej lub [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="83c56-124">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="83c56-125">Firma Microsoft zaleca, za pomocą przekierowania tymczasowego, a nie stałych przekierowań, ponieważ buforowanie linków może spowodować niestabilność zachowanie w środowiskach programistycznych.</span><span class="sxs-lookup"><span data-stu-id="83c56-125">We recommend using temporary redirects rather than permanent redirects, as link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="83c56-126">Firma Microsoft zaleca używanie [HSTS](#hsts) celu sygnalizowania, że dla klientów, którzy tylko zabezpieczyć zasób w aplikacji (tylko w środowisku produkcyjnym) powinny być wysyłane żądania.</span><span class="sxs-lookup"><span data-stu-id="83c56-126">We recommend using [HSTS](#hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

> [!WARNING]
> <span data-ttu-id="83c56-127">Port musi być dostępny dla oprogramowania pośredniczącego do przekierowania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="83c56-127">A port must be available for the middleware to redirect to HTTPS.</span></span> <span data-ttu-id="83c56-128">Jeśli port nie jest dostępny, nie zachodzi przekierowania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="83c56-128">If no port is available, redirection to HTTPS doesn't occur.</span></span> <span data-ttu-id="83c56-129">HTTPS port można określić przy użyciu dowolnej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="83c56-129">The HTTPS port can be specified using any of the following approaches:</span></span>
>
> * <span data-ttu-id="83c56-130">Ustaw `HttpsRedirectionOptions.HttpsPort`.</span><span class="sxs-lookup"><span data-stu-id="83c56-130">Set `HttpsRedirectionOptions.HttpsPort`.</span></span>
> * <span data-ttu-id="83c56-131">Ustaw `ASPNETCORE_HTTPS_PORT` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="83c56-131">Set the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
> * <span data-ttu-id="83c56-132">W trakcie opracowywania, należy ustawić adres URL protokołu HTTPS *launchsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="83c56-132">In development, set an HTTPS URL in *launchsettings.json*.</span></span>
> * <span data-ttu-id="83c56-133">Konfigurowanie punktu końcowego adresu URL HTTPS dla [Kestrel](xref:fundamentals/servers/kestrel) lub [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="83c56-133">Configure an HTTPS URL endpoint for [Kestrel](xref:fundamentals/servers/kestrel) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>
>
> <span data-ttu-id="83c56-134">Podczas Kestrel lub sterownik HTTP.sys jest używany jako serwer graniczny publicznymi, Kestrel lub sterownik HTTP.sys muszą być skonfigurowane do nasłuchiwania na obu:</span><span class="sxs-lookup"><span data-stu-id="83c56-134">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>
>
> * <span data-ttu-id="83c56-135">Bezpieczny port, na którym klient zostanie przekierowany (zazwyczaj port 443 w środowisku produkcyjnym i 5001 w trakcie programowania).</span><span class="sxs-lookup"><span data-stu-id="83c56-135">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
> * <span data-ttu-id="83c56-136">Port niezabezpieczone (zwykle 80 w środowisku produkcyjnym) do 5000 w trakcie opracowywania.</span><span class="sxs-lookup"><span data-stu-id="83c56-136">The insecure port (typically, 80 in production and 5000 in development).</span></span>
>
> <span data-ttu-id="83c56-137">Niebezpieczne port musi być dostępny dla klientów w kolejności dla aplikacji do odbierania niezabezpieczone żądania i Przekieruj do bezpiecznego portu.</span><span class="sxs-lookup"><span data-stu-id="83c56-137">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect it to the secure port.</span></span>
>
> <span data-ttu-id="83c56-138">Wszystkie zapory między klientem i serwerem musi mieć również porty otworzyć dla ruchu.</span><span class="sxs-lookup"><span data-stu-id="83c56-138">Any firewall between the client and server must also have the ports open for traffic.</span></span>
>
> <span data-ttu-id="83c56-139">Aby uzyskać więcej informacji, zobacz [konfiguracji punktu końcowego Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) lub <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="83c56-139">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

<span data-ttu-id="83c56-140">Następujący wyróżniony kod wywołuje [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) skonfigurować opcje oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="83c56-140">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="83c56-141">Wywoływanie `AddHttpsRedirection` tylko jest konieczna zmiana wartości `HttpsPort` lub `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="83c56-141">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="83c56-142">Wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="83c56-142">The preceding highlighted code:</span></span>

* <span data-ttu-id="83c56-143">Zestawy [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) do [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect), która jest wartością domyślną.</span><span class="sxs-lookup"><span data-stu-id="83c56-143">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect), which is the default value.</span></span> <span data-ttu-id="83c56-144">Użyj pola [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) klasy do przypisania do `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="83c56-144">Use the fields of the [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="83c56-145">Ustawia HTTPS port 5001.</span><span class="sxs-lookup"><span data-stu-id="83c56-145">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="83c56-146">Wartość domyślna to 443.</span><span class="sxs-lookup"><span data-stu-id="83c56-146">The default value is 443.</span></span>

<span data-ttu-id="83c56-147">Automatycznie Ustaw port przez następujących mechanizmów:</span><span class="sxs-lookup"><span data-stu-id="83c56-147">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="83c56-148">Oprogramowanie pośredniczące może odnajdować portów za pomocą [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) gdy są spełnione następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="83c56-148">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>

  * <span data-ttu-id="83c56-149">Kestrel lub sterownik HTTP.sys jest używana bezpośrednio z punktów końcowych HTTPS (dotyczy także uruchamianie aplikacji za pomocą debugera programu Visual Studio Code firmy).</span><span class="sxs-lookup"><span data-stu-id="83c56-149">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  * <span data-ttu-id="83c56-150">Tylko **jeden port HTTPS** jest używany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="83c56-150">Only **one HTTPS port** is used by the app.</span></span>

* <span data-ttu-id="83c56-151">Program Visual Studio jest używany:</span><span class="sxs-lookup"><span data-stu-id="83c56-151">Visual Studio is used:</span></span>

  * <span data-ttu-id="83c56-152">Usługi IIS Express ma obsługujące protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="83c56-152">IIS Express has HTTPS enabled.</span></span>
  * <span data-ttu-id="83c56-153">*launchSettings.json* ustawia `sslPort` dla usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="83c56-153">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="83c56-154">Gdy aplikacja jest uruchamiana za zwrotny serwer proxy (na przykład, IIS, usługi IIS Express), `IServerAddressesFeature` jest niedostępna.</span><span class="sxs-lookup"><span data-stu-id="83c56-154">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="83c56-155">Numer portu musi być skonfigurowany ręcznie.</span><span class="sxs-lookup"><span data-stu-id="83c56-155">The port must be manually configured.</span></span> <span data-ttu-id="83c56-156">Jeśli port nie jest ustawiony, przekierowanie nie nastąpi żądań.</span><span class="sxs-lookup"><span data-stu-id="83c56-156">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="83c56-157">Można skonfigurować port, ustawiając [ustawienia konfiguracji hosta sieci Web https_port](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="83c56-157">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="83c56-158">**Klucz**: https_port</span><span class="sxs-lookup"><span data-stu-id="83c56-158">**Key**: https_port</span></span>  
<span data-ttu-id="83c56-159">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="83c56-159">**Type**: *string*</span></span>  
<span data-ttu-id="83c56-160">**Domyślne**: nie ustawiono wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="83c56-160">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="83c56-161">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="83c56-161">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="83c56-162">**Zmienna środowiskowa**: `<PREFIX_>HTTPS_PORT` (prefiks jest `ASPNETCORE_` przy użyciu hosta sieci Web.)</span><span class="sxs-lookup"><span data-stu-id="83c56-162">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="83c56-163">Port można skonfigurować pośrednio przez ustawienie adresu URL z `ASPNETCORE_URLS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="83c56-163">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="83c56-164">Zmienna środowiskowa konfiguruje serwer, a następnie oprogramowanie pośredniczące pośrednio odnajduje portu HTTPS za pośrednictwem `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="83c56-164">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="83c56-165">Jeśli port nie jest ustawiony:</span><span class="sxs-lookup"><span data-stu-id="83c56-165">If no port is set:</span></span>

* <span data-ttu-id="83c56-166">Żądania nie są przekierowywane.</span><span class="sxs-lookup"><span data-stu-id="83c56-166">Requests aren't redirected.</span></span>
* <span data-ttu-id="83c56-167">Oprogramowanie pośredniczące rejestruje ostrzeżenie "Nie można określić port https dla przekierowania."</span><span class="sxs-lookup"><span data-stu-id="83c56-167">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

> [!NOTE]
> <span data-ttu-id="83c56-168">Alternatywa dla użycia oprogramowania pośredniczącego przekierowania protokołu HTTPS (`UseHttpsRedirection`) jest użycie oprogramowanie pośredniczące ponownego zapisywania adresów URL (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="83c56-168">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="83c56-169">`AddRedirectToHttps` Podczas wykonywania przekierowania, można również ustawić kod stanu i port.</span><span class="sxs-lookup"><span data-stu-id="83c56-169">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="83c56-170">Aby uzyskać więcej informacji, zobacz [oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="83c56-170">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="83c56-171">Podczas przekierowywania HTTPS bez potrzeby przekierowania dodatkowe reguły, firma Microsoft zaleca używanie oprogramowania pośredniczącego przekierowania protokołu HTTPS (`UseHttpsRedirection`) opisane w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="83c56-171">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="83c56-172">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) służy do wymagania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="83c56-172">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="83c56-173">`[RequireHttpsAttribute]` można dekoracji kontrolery lub metody lub mogą być stosowane globalnie.</span><span class="sxs-lookup"><span data-stu-id="83c56-173">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="83c56-174">Aby zastosować atrybut globalnie, Dodaj następujący kod do `ConfigureServices` w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="83c56-174">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="83c56-175">Powyższy kod wyróżniony wymaga, wszystkie żądania przy użyciu `HTTPS`; dlatego żądania HTTP są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="83c56-175">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="83c56-176">Następujący wyróżniony kod przekierowuje żądania HTTP do HTTPS:</span><span class="sxs-lookup"><span data-stu-id="83c56-176">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="83c56-177">Aby uzyskać więcej informacji, zobacz [oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="83c56-177">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="83c56-178">Oprogramowanie pośredniczące pozwala również aplikację, aby ustawić kod stanu lub kod stanu i port, podczas wykonywania przekierowania.</span><span class="sxs-lookup"><span data-stu-id="83c56-178">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="83c56-179">Globalnie wymagania protokołu HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) jest ze względów bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="83c56-179">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="83c56-180">Stosowanie `[RequireHttps]` atrybutu, aby wszystkie kontrolery/Razor strony nie jest uważany za bezpieczny globalnie wymagania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="83c56-180">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="83c56-181">Nie może zagwarantować `[RequireHttps]` atrybut jest stosowany podczas dodawania nowych kontrolerów i stron Razor.</span><span class="sxs-lookup"><span data-stu-id="83c56-181">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="83c56-182">Protokół zabezpieczeń Strict transportu HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="83c56-182">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="83c56-183">Na [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [zabezpieczeń transportu HTTP ograniczeniami (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) jest ulepszeniem zabezpieczeń zgłoszenie zgody na uczestnictwo w określonym przez aplikację sieci web przy użyciu nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="83c56-183">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="83c56-184">Gdy [przeglądarki obsługującej HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) odbiera tego nagłówka:</span><span class="sxs-lookup"><span data-stu-id="83c56-184">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="83c56-185">Przeglądarka przechowuje konfigurację dla domeny, która uniemożliwia wysłanie Każde powiadomienie za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="83c56-185">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="83c56-186">Przeglądarka wymusza całą komunikację za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="83c56-186">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="83c56-187">Przeglądarka uniemożliwia użytkownikowi za pomocą certyfikatów niezaufanych lub jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="83c56-187">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="83c56-188">Przeglądarka wyłącza monity, które umożliwiają użytkownikowi tymczasowo ufać takiego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="83c56-188">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="83c56-189">Ponieważ HSTS jest wymuszana przez klienta ma pewne ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="83c56-189">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="83c56-190">Klient musi obsługiwać HSTS.</span><span class="sxs-lookup"><span data-stu-id="83c56-190">The client must support HSTS.</span></span>
* <span data-ttu-id="83c56-191">HSTS wymaga co najmniej jeden pomyślne żądanie HTTPS do ustanowienia zasad HSTS.</span><span class="sxs-lookup"><span data-stu-id="83c56-191">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="83c56-192">Aplikacja musi sprawdzić każdego żądania HTTP i przekierowania lub odrzucanie żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="83c56-192">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="83c56-193">Platforma ASP.NET Core 2.1 lub nowszej implementuje HSTS z `UseHsts` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="83c56-193">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="83c56-194">Poniższy kod wywoła `UseHsts` gdy aplikacja nie jest [trybu opracowywania](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="83c56-194">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="83c56-195">`UseHsts` nie jest zalecane w rozwoju, ponieważ ustawienia HSTS są wysoce podlega buforowaniu przez przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="83c56-195">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="83c56-196">Domyślnie `UseHsts` nie obejmuje adresu lokalnego sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="83c56-196">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="83c56-197">W środowiskach produkcyjnych, implementacja protokołu HTTPS, po raz pierwszy należy ustawić wartość początkową HSTS małej wartości.</span><span class="sxs-lookup"><span data-stu-id="83c56-197">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="83c56-198">Ustaw wartość od kilku godzin do nie więcej niż jednego dziennie w przypadku konieczności przywrócenia infrastruktury protokołu HTTPS do protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="83c56-198">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="83c56-199">Po masz pewność, że trwałości Konfiguracja protokołu HTTPS, należy zwiększyć wartość max-age HSTS; często używane wartości jest jeden rok.</span><span class="sxs-lookup"><span data-stu-id="83c56-199">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="83c56-200">Poniższy kod:</span><span class="sxs-lookup"><span data-stu-id="83c56-200">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="83c56-201">Ustawia wstępnego ładowania parametr nagłówka zabezpieczeń w przypadku transportu Strict.</span><span class="sxs-lookup"><span data-stu-id="83c56-201">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="83c56-202">Wstępnego ładowania nie jest częścią [specyfikacji RFC HSTS](https://tools.ietf.org/html/rfc6797), ale jest obsługiwane przez przeglądarki sieci web do wstępnego witryn HSTS na zainstalować od nowa.</span><span class="sxs-lookup"><span data-stu-id="83c56-202">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="83c56-203">Zobacz [ https://hstspreload.org/ ](https://hstspreload.org/) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="83c56-203">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="83c56-204">Włącza [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), co ma zastosowanie zasad HSTS poddomen hosta.</span><span class="sxs-lookup"><span data-stu-id="83c56-204">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="83c56-205">Jawnie Ustawia parametr max-age nagłówka zabezpieczeń w przypadku transportu Strict do 60 dni.</span><span class="sxs-lookup"><span data-stu-id="83c56-205">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="83c56-206">Jeśli nie zostanie ustawiona wartość domyślna to 30 dni.</span><span class="sxs-lookup"><span data-stu-id="83c56-206">If not set, defaults to 30 days.</span></span> <span data-ttu-id="83c56-207">Zobacz [dyrektywy max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="83c56-207">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="83c56-208">Dodaje `example.com` do listy hostów, które mają zostać wykluczone.</span><span class="sxs-lookup"><span data-stu-id="83c56-208">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="83c56-209">`UseHsts` nie obejmuje następujące hosty sprzężenia zwrotnego:</span><span class="sxs-lookup"><span data-stu-id="83c56-209">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="83c56-210">`localhost` Adres sprzężenia zwrotnego protokołu IPv4.</span><span class="sxs-lookup"><span data-stu-id="83c56-210">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="83c56-211">`127.0.0.1` Adres sprzężenia zwrotnego protokołu IPv4.</span><span class="sxs-lookup"><span data-stu-id="83c56-211">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="83c56-212">`[::1]` Adres sprzężenia zwrotnego protokołu IPv6.</span><span class="sxs-lookup"><span data-stu-id="83c56-212">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="83c56-213">Poprzedni przykład pokazuje, jak dodać kolejne hosty.</span><span class="sxs-lookup"><span data-stu-id="83c56-213">The preceding example shows how to add additional hosts.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="83c56-214">Zrezygnować z protokołu HTTPS/HSTS przy tworzeniu projektu</span><span class="sxs-lookup"><span data-stu-id="83c56-214">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="83c56-215">W niektórych scenariuszach usługi zaplecza gdzie zabezpieczenia połączeń jest obsługiwane na urządzeniach brzegowych publicznych sieci konfigurowanie zabezpieczenia połączeń w każdym węźle nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="83c56-215">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="83c56-216">Wygenerowany na podstawie szablonów w programie Visual Studio lub z aplikacji sieci Web [dotnet nowe](/dotnet/core/tools/dotnet-new) Włącz polecenie [przekierowania protokołu HTTPS](#require) i [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="83c56-216">Web apps generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="83c56-217">W przypadku wdrożeń, które nie wymagają tych scenariuszy użytkownik może zrezygnować z HTTPS/HSTS po utworzeniu aplikacji z szablonu.</span><span class="sxs-lookup"><span data-stu-id="83c56-217">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="83c56-218">Aby zrezygnować z protokołu HTTPS/HSTS:</span><span class="sxs-lookup"><span data-stu-id="83c56-218">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="83c56-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83c56-219">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="83c56-220">Usuń zaznaczenie pola wyboru **Konfigurowanie protokołu HTTPS** pola wyboru.</span><span class="sxs-lookup"><span data-stu-id="83c56-220">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagram jednostek](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="83c56-222">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="83c56-222">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="83c56-223">Użyj `--no-https` opcji.</span><span class="sxs-lookup"><span data-stu-id="83c56-223">Use the `--no-https` option.</span></span> <span data-ttu-id="83c56-224">Na przykład</span><span class="sxs-lookup"><span data-stu-id="83c56-224">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="83c56-225">Jak skonfigurować certyfikat dewelopera dla programu Docker</span><span class="sxs-lookup"><span data-stu-id="83c56-225">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="83c56-226">Zobacz [problem w usłudze GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="83c56-226">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="83c56-227">Dodatkowe informacje</span><span class="sxs-lookup"><span data-stu-id="83c56-227">Additional information</span></span>

* [<span data-ttu-id="83c56-228">Obsługa przeglądarek OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="83c56-228">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
