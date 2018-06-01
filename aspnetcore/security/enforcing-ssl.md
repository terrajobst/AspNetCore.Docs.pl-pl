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
ms.openlocfilehash: 0433ddb3bf1ef0074c683903ad4553cd6a0b4741
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/31/2018
ms.locfileid: "34687821"
---
# <a name="enforce-https-in-an-aspnet-core"></a><span data-ttu-id="56242-103">Wymuszanie protokołu HTTPS w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56242-103">Enforce HTTPS in an ASP.NET Core</span></span>

<span data-ttu-id="56242-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="56242-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="56242-105">Ten dokument zawiera jak:</span><span class="sxs-lookup"><span data-stu-id="56242-105">This document shows how to:</span></span>

- <span data-ttu-id="56242-106">Wymagać protokołu HTTPS dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="56242-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="56242-107">Przekieruj żądania HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56242-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="56242-108">Czy **nie** użyj `RequireHttpsAttribute` na interfejsów API sieci Web, czy odbierać poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="56242-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="56242-109">`RequireHttpsAttribute` używa kodów stanu HTTP do przekierowania przeglądarki z protokołu HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56242-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="56242-110">Klientów interfejsu API nie może zrozumieć lub przestrzegać przekierowania z protokołu HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56242-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="56242-111">Tacy klienci mogą wysłać informacje za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="56242-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="56242-112">Interfejsy API sieci Web powinien:</span><span class="sxs-lookup"><span data-stu-id="56242-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="56242-113">Nie nasłuchiwania protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="56242-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="56242-114">Zamknięcie połączenia z kodem stanu 400 (nieprawidłowe żądanie), a nie obsługiwać żądania.</span><span class="sxs-lookup"><span data-stu-id="56242-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="56242-115">Wymagać protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="56242-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="56242-116">Firma Microsoft zaleca wszystkie platformy ASP.NET Core wywołania aplikacji sieci web `UseHttpsRedirection` Aby przekierować wszystkie żądania HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56242-116">We recommend all ASP.NET Core web apps call `UseHttpsRedirection` to redirect all HTTP requests to HTTPS.</span></span> <span data-ttu-id="56242-117">Jeśli `UseHsts` jest wywoływana w aplikacji, musi zostać wywołana przed `UseHttpsRedirection`.</span><span class="sxs-lookup"><span data-stu-id="56242-117">If `UseHsts` is called in the app, it must be called before `UseHttpsRedirection`.</span></span>

<span data-ttu-id="56242-118">Poniższy kod wywołania `UseHttpsRedirection` w `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="56242-118">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="56242-119">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="56242-119">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>


<span data-ttu-id="56242-120">Następujący kod:</span><span class="sxs-lookup"><span data-stu-id="56242-120">The following code:</span></span>

<span data-ttu-id="56242-121">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="56242-121">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

* <span data-ttu-id="56242-122">Zestawy `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="56242-122">Sets `RedirectStatusCode`.</span></span>
* <span data-ttu-id="56242-123">Ustawia HTTPS port 5001.</span><span class="sxs-lookup"><span data-stu-id="56242-123">Sets the HTTPS port to 5001.</span></span>

::: moniker-end


::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="56242-124">[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) jest używana, aby wymagać protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56242-124">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="56242-125">`[RequireHttpsAttribute]` można dekoracji kontrolerów lub metody lub mogą być stosowane globalnie.</span><span class="sxs-lookup"><span data-stu-id="56242-125">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="56242-126">Aby zastosować atrybut globalny, Dodaj następujący kod do `ConfigureServices` w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="56242-126">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="56242-127">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="56242-127">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="56242-128">Poprzedni wyróżniony kod wymaga wszystkie żądania przy użyciu `HTTPS`; w związku z tym żądania HTTP są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="56242-128">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="56242-129">Następujący wyróżniony kod przekierowuje żądania HTTP, https:</span><span class="sxs-lookup"><span data-stu-id="56242-129">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="56242-130">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="56242-130">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="56242-131">Aby uzyskać więcej informacji, zobacz [ponowne zapisywanie adresów URL w oprogramowaniu pośredniczącym](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="56242-131">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="56242-132">Globalny wymagających protokołu HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) jest ze względów bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="56242-132">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="56242-133">Stosowanie `[RequireHttps]` atrybut, aby wszystkie kontrolery/Razor strony nie jest uznawane za należycie zabezpieczone globalnie wymagających protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56242-133">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="56242-134">Nie można zagwarantować `[RequireHttps]` atrybut jest stosowany podczas dodawania nowych kontrolerów i stron Razor.</span><span class="sxs-lookup"><span data-stu-id="56242-134">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="56242-135">Protokół zabezpieczeń Strict transportu HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="56242-135">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="56242-136">Na [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [zabezpieczeń transportu HTTP ograniczeniami (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) jest ulepszeniem zabezpieczeń opcjonalnych w określonym przez aplikację sieci web przy użyciu nagłówka odpowiedzi specjalnych.</span><span class="sxs-lookup"><span data-stu-id="56242-136">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="56242-137">Kiedy obsługiwanej przeglądarki odbiera ten nagłówek przeglądarka uniemożliwi komunikację z są wysyłane za pośrednictwem protokołu HTTP z określoną domeną i zamiast tego wyśle cała komunikacja za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56242-137">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="56242-138">Uniemożliwia także kliknij HTTPS za pomocą monity w przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="56242-138">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="56242-139">Platformy ASP.NET Core 2.1 lub nowszej implementuje HSTS z `UseHsts` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="56242-139">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="56242-140">Poniższy kod wywołania `UseHsts` gdy aplikacja nie jest w [tryb programowania](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="56242-140">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="56242-141">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="56242-141">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="56242-142">`UseHsts` nie jest zalecane w rozwoju ponieważ nagłówek HSTS jest wysoce cachable przez przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="56242-142">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="56242-143">Domyślnie UseHsts wyklucza adresu lokalnego sprzężenia zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="56242-143">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="56242-144">Następujący kod:</span><span class="sxs-lookup"><span data-stu-id="56242-144">The following code:</span></span>

<span data-ttu-id="56242-145">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="56242-145">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="56242-146">Ustawia parametr wstępnego ładowania nagłówka Strict TLS.</span><span class="sxs-lookup"><span data-stu-id="56242-146">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="56242-147">Wstępnego ładowania nie jest częścią [specyfikacji RFC HSTS](https://tools.ietf.org/html/rfc6797), ale nie jest obsługiwana przez przeglądarki sieci web wstępnie HSTS witryn na nowej instalacji.</span><span class="sxs-lookup"><span data-stu-id="56242-147">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="56242-148">Zobacz [ https://hstspreload.org/ ](https://hstspreload.org/) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="56242-148">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="56242-149">Umożliwia [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), która ma zastosowanie zasad HSTS poddomen hosta.</span><span class="sxs-lookup"><span data-stu-id="56242-149">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="56242-150">Jawnie Ustawia maksymalny wiek parametr nagłówka TLS Strict do 60 dni.</span><span class="sxs-lookup"><span data-stu-id="56242-150">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="56242-151">Jeśli nie zostanie ustawiona wartość domyślna to 30 dni.</span><span class="sxs-lookup"><span data-stu-id="56242-151">If not set, defaults to 30 days.</span></span> <span data-ttu-id="56242-152">Zobacz [dyrektywy maksymalny wiek](https://tools.ietf.org/html/rfc6797#section-6.1.1) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="56242-152">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="56242-153">Dodaje `example.com` do listy hostów, które mają zostać wykluczone.</span><span class="sxs-lookup"><span data-stu-id="56242-153">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="56242-154">`UseHsts` nie obejmuje następujące hosty sprzężenie zwrotne:</span><span class="sxs-lookup"><span data-stu-id="56242-154">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="56242-155">`localhost` Adres sprzężenia zwrotnego protokołu IPv4.</span><span class="sxs-lookup"><span data-stu-id="56242-155">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="56242-156">`127.0.0.1` Adres sprzężenia zwrotnego protokołu IPv4.</span><span class="sxs-lookup"><span data-stu-id="56242-156">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="56242-157">`[::1]` Adres sprzężenia zwrotnego protokołu IPv6.</span><span class="sxs-lookup"><span data-stu-id="56242-157">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="56242-158">Powyższy przykład przedstawia sposób dodawania dodatkowych hostów.</span><span class="sxs-lookup"><span data-stu-id="56242-158">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="56242-159">Zrezygnować z protokołu HTTPS na tworzenie projektu</span><span class="sxs-lookup"><span data-stu-id="56242-159">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="56242-160">Program ASP.NET Core 2.1 i nowsze szablonów aplikacji sieci web (od programu Visual Studio lub wiersza polecenia platformy dotnet) umożliwiają [przekierowania HTTPS](#require) i [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="56242-160">The ASP.NET Core 2.1 and later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="56242-161">W przypadku wdrożeń, które nie wymagają protokołu HTTPS użytkownik może zrezygnować z protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56242-161">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="56242-162">Na przykład niektóre usługi wewnętrznej bazy danych, gdy HTTPS jest obsługiwane zewnętrznie na krawędzi, przy użyciu protokołu HTTPS w każdym węźle nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="56242-162">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="56242-163">Aby zrezygnować z protokołu HTTPS:</span><span class="sxs-lookup"><span data-stu-id="56242-163">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="56242-164">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="56242-164">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="56242-165">Usuń zaznaczenie pola wyboru **Konfiguruj na potrzeby protokołu HTTPS** wyboru.</span><span class="sxs-lookup"><span data-stu-id="56242-165">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Diagram jednostek](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="56242-167">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="56242-167">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="56242-168">Użyj `--no-https` opcji.</span><span class="sxs-lookup"><span data-stu-id="56242-168">Use the `--no-https` option.</span></span> <span data-ttu-id="56242-169">Na przykład</span><span class="sxs-lookup"><span data-stu-id="56242-169">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="56242-170">Jak skonfigurować certyfikat dewelopera dla Docker</span><span class="sxs-lookup"><span data-stu-id="56242-170">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="56242-171">Zobacz [ten problem GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="56242-171">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
