---
title: Implementacja serwera sieci web HTTP.sys, w programie ASP.NET Core
author: guardrex
description: Więcej informacji na temat HTTP.sys, serwer sieci web platformy ASP.NET Core na Windows. Oparta na sterownik trybu jądra HTTP.sys, sterownik HTTP.sys stanowi alternatywę Kestrel, który może służyć do bezpośredniego połączenia z Internetu bez usług IIS.
monikerRange: '>= aspnetcore-2.0'
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/21/2019
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 6ec9b7bf3da0015b8ac3918a4d47644fffc14cdb
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209177"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="6b1d7-104">Implementacja serwera sieci web HTTP.sys, w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6b1d7-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="6b1d7-105">Przez [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6b1d7-105">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6b1d7-106">[Sterownik HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) jest [serwera sieci web dla platformy ASP.NET Core](xref:fundamentals/servers/index) uruchomioną w Windows.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) is a [web server for ASP.NET Core](xref:fundamentals/servers/index) that only runs on Windows.</span></span> <span data-ttu-id="6b1d7-107">Sterownik HTTP.sys stanowi alternatywę [Kestrel](xref:fundamentals/servers/kestrel) serwera i ofert, niektóre funkcje, że nie zapewnia Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-107">HTTP.sys is an alternative to [Kestrel](xref:fundamentals/servers/kestrel) server and offers some features that Kestrel doesn't provide.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6b1d7-108">Sterownik HTTP.sys nie jest zgodny z [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) i nie można używać z usług IIS lub IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-108">HTTP.sys isn't compatible with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and can't be used with IIS or IIS Express.</span></span>

<span data-ttu-id="6b1d7-109">Sterownik HTTP.sys obsługuje następujące funkcje:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-109">HTTP.sys supports the following features:</span></span>

* [<span data-ttu-id="6b1d7-110">Uwierzytelnianie Windows</span><span class="sxs-lookup"><span data-stu-id="6b1d7-110">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
* <span data-ttu-id="6b1d7-111">Współużytkowanie portów</span><span class="sxs-lookup"><span data-stu-id="6b1d7-111">Port sharing</span></span>
* <span data-ttu-id="6b1d7-112">Protokołu HTTPS z SNI</span><span class="sxs-lookup"><span data-stu-id="6b1d7-112">HTTPS with SNI</span></span>
* <span data-ttu-id="6b1d7-113">Protokołu HTTP/2 za pośrednictwem protokołu TLS (system Windows 10 lub nowsza wersja)</span><span class="sxs-lookup"><span data-stu-id="6b1d7-113">HTTP/2 over TLS (Windows 10 or later)</span></span>
* <span data-ttu-id="6b1d7-114">Przekazywanie pliku bezpośrednie</span><span class="sxs-lookup"><span data-stu-id="6b1d7-114">Direct file transmission</span></span>
* <span data-ttu-id="6b1d7-115">Buforowanie odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="6b1d7-115">Response caching</span></span>
* <span data-ttu-id="6b1d7-116">Gniazda Websocket (system Windows 8 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="6b1d7-116">WebSockets (Windows 8 or later)</span></span>

<span data-ttu-id="6b1d7-117">Obsługiwane wersje Windows:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-117">Supported Windows versions:</span></span>

* <span data-ttu-id="6b1d7-118">Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="6b1d7-118">Windows 7 or later</span></span>
* <span data-ttu-id="6b1d7-119">Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="6b1d7-119">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="6b1d7-120">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6b1d7-120">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="6b1d7-121">Kiedy należy używać HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="6b1d7-121">When to use HTTP.sys</span></span>

<span data-ttu-id="6b1d7-122">Sterownik HTTP.sys jest przydatne w przypadku wdrożeń gdzie:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-122">HTTP.sys is useful for deployments where:</span></span>

* <span data-ttu-id="6b1d7-123">Istnieje potrzeba, aby uwidocznić server bezpośrednio do Internetu bez korzystania z usług IIS.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-123">There's a need to expose the server directly to the Internet without using IIS.</span></span>

  ![Sterownik HTTP.sys komunikuje się bezpośrednio z Internetu](httpsys/_static/httpsys-to-internet.png)

* <span data-ttu-id="6b1d7-125">Wewnętrznego wdrażania wymaga funkcji nie jest dostępna w Kestrel, takich jak [uwierzytelniania Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="6b1d7-125">An internal deployment requires a feature not available in Kestrel, such as [Windows Authentication](xref:security/authentication/windowsauth).</span></span>

  ![Sterownik HTTP.sys komunikuje się bezpośrednio z siecią wewnętrzną](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="6b1d7-127">Sterownik HTTP.sys to dojrzała technologia, która chroni przed wiele rodzajów ataków i udostępnia niezawodności, bezpieczeństwa i skalowalności serwera sieci web w pełni funkcjonalne.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-127">HTTP.sys is mature technology that protects against many types of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="6b1d7-128">SAM działa jako odbiornik HTTP w pliku HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-128">IIS itself runs as an HTTP listener on top of HTTP.sys.</span></span>

## <a name="http2-support"></a><span data-ttu-id="6b1d7-129">Obsługa protokołu HTTP/2</span><span class="sxs-lookup"><span data-stu-id="6b1d7-129">HTTP/2 support</span></span>

<span data-ttu-id="6b1d7-130">[Protokołu HTTP/2](https://httpwg.org/specs/rfc7540.html) jest włączone dla aplikacji platformy ASP.NET Core, jeśli następujące podstawowa wymagania zostały spełnione:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-130">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is enabled for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="6b1d7-131">Windows Server 2016 i Windows 10 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="6b1d7-131">Windows Server 2016/Windows 10 or later</span></span>
* <span data-ttu-id="6b1d7-132">[Negocjowania protokołu warstwy aplikacji (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) połączenia</span><span class="sxs-lookup"><span data-stu-id="6b1d7-132">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="6b1d7-133">Protokół TLS 1.2 lub nowszej połączenia</span><span class="sxs-lookup"><span data-stu-id="6b1d7-133">TLS 1.2 or later connection</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6b1d7-134">Jeśli zostanie nawiązane połączenie HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporty `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-134">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="6b1d7-135">Jeśli zostanie nawiązane połączenie HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporty `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-135">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="6b1d7-136">Protokołu HTTP/2 jest domyślnie włączona.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-136">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="6b1d7-137">Jeśli nie jest nawiązane połączenie HTTP/2, połączenie jest powraca do protokołu HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-137">If an HTTP/2 connection isn't established, the connection falls back to HTTP/1.1.</span></span> <span data-ttu-id="6b1d7-138">W przyszłej wersji systemu Windows flagi konfiguracji protokołu HTTP/2 będą dostępne, w tym możliwość wyłączenia protokołu HTTP/2 w pliku HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-138">In a future release of Windows, HTTP/2 configuration flags will be available, including the ability to disable HTTP/2 with HTTP.sys.</span></span>

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="6b1d7-139">Uwierzytelnianie trybu jądra przy użyciu protokołu Kerberos</span><span class="sxs-lookup"><span data-stu-id="6b1d7-139">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="6b1d7-140">Sterownik HTTP.sys delegatów, aby uwierzytelnianie trybu jądra za pomocą protokołu uwierzytelniania Kerberos.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-140">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="6b1d7-141">Uwierzytelnianie w trybie użytkownika nie jest obsługiwana przy użyciu protokołu Kerberos i sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-141">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="6b1d7-142">Konto komputera należy używany do odszyfrowywania tokenu/biletu Kerberos uzyskany z usługi Active Directory i przesyłany dalej przez klienta do serwera w celu uwierzytelnienia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-142">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="6b1d7-143">Rejestrowanie głównej nazwy usługi (SPN) dla hosta, a nie użytkownika aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-143">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-httpsys"></a><span data-ttu-id="6b1d7-144">Jak używać HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="6b1d7-144">How to use HTTP.sys</span></span>

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a><span data-ttu-id="6b1d7-145">Konfigurowanie aplikacji platformy ASP.NET Core do użycia w pliku HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="6b1d7-145">Configure the ASP.NET Core app to use HTTP.sys</span></span>

1. <span data-ttu-id="6b1d7-146">Odwołania do pakietu w pliku projektu nie jest wymagana, gdy za pomocą [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (platformy ASP.NET Core 2.1 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="6b1d7-146">A package reference in the project file isn't required when using the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="6b1d7-147">Bez korzystania z `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all, Dodaj odwołanie do pakietu [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span><span class="sxs-lookup"><span data-stu-id="6b1d7-147">When not using the `Microsoft.AspNetCore.App` metapackage, add a package reference to [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span></span>

2. <span data-ttu-id="6b1d7-148">Wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> — metoda rozszerzenia podczas tworzenia hosta sieci Web, określając wymagane <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-148">Call the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> extension method when building Web Host, specifying any required <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>:</span></span>

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   <span data-ttu-id="6b1d7-149">Dodatkowa konfiguracja HTTP.sys odbywa się za pośrednictwem [ustawień rejestru](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span><span class="sxs-lookup"><span data-stu-id="6b1d7-149">Additional HTTP.sys configuration is handled through [registry settings](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span></span>

   <span data-ttu-id="6b1d7-150">**Opcje HTTP.sys**</span><span class="sxs-lookup"><span data-stu-id="6b1d7-150">**HTTP.sys options**</span></span>

   | <span data-ttu-id="6b1d7-151">Właściwość</span><span class="sxs-lookup"><span data-stu-id="6b1d7-151">Property</span></span> | <span data-ttu-id="6b1d7-152">Opis</span><span class="sxs-lookup"><span data-stu-id="6b1d7-152">Description</span></span> | <span data-ttu-id="6b1d7-153">Domyślny</span><span class="sxs-lookup"><span data-stu-id="6b1d7-153">Default</span></span> |
   | -------- | ----------- | :-----: |
   | [<span data-ttu-id="6b1d7-154">AllowSynchronousIO</span><span class="sxs-lookup"><span data-stu-id="6b1d7-154">AllowSynchronousIO</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | <span data-ttu-id="6b1d7-155">Kontrolowanie, czy synchroniczna wejścia/wyjścia jest dozwolone dla `HttpContext.Request.Body` i `HttpContext.Response.Body`.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-155">Control whether synchronous input/output is allowed for the `HttpContext.Request.Body` and `HttpContext.Response.Body`.</span></span> | `true` |
   | [<span data-ttu-id="6b1d7-156">Authentication.AllowAnonymous</span><span class="sxs-lookup"><span data-stu-id="6b1d7-156">Authentication.AllowAnonymous</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | <span data-ttu-id="6b1d7-157">Zezwalaj na anonimowe żądania.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-157">Allow anonymous requests.</span></span> | `true` |
   | [<span data-ttu-id="6b1d7-158">Authentication.Schemes</span><span class="sxs-lookup"><span data-stu-id="6b1d7-158">Authentication.Schemes</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | <span data-ttu-id="6b1d7-159">Umożliwia określenie schematów uwierzytelniania dozwolone.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-159">Specify the allowed authentication schemes.</span></span> <span data-ttu-id="6b1d7-160">Może być modyfikowany w dowolnym momencie przed disposing odbiornika.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-160">May be modified at any time prior to disposing the listener.</span></span> <span data-ttu-id="6b1d7-161">Wartości są dostarczane przez [wyliczenia AuthenticationSchemes](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, i `NTLM`.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-161">Values are provided by the [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, and `NTLM`.</span></span> | `None` |
   | [<span data-ttu-id="6b1d7-162">EnableResponseCaching</span><span class="sxs-lookup"><span data-stu-id="6b1d7-162">EnableResponseCaching</span></span>](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | <span data-ttu-id="6b1d7-163">Próba [trybu jądra](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) buforowanie odpowiedzi z kwalifikujących się nagłówków.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-163">Attempt [kernel-mode](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) caching for responses with eligible headers.</span></span> <span data-ttu-id="6b1d7-164">Odpowiedź nie może zawierać `Set-Cookie`, `Vary`, lub `Pragma` nagłówków.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-164">The response may not include `Set-Cookie`, `Vary`, or `Pragma` headers.</span></span> <span data-ttu-id="6b1d7-165">Musi on zawierać `Cache-Control` nagłówka to `public` i `shared-max-age` lub `max-age` wartość lub `Expires` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-165">It must include a `Cache-Control` header that's `public` and either a `shared-max-age` or `max-age` value, or an `Expires` header.</span></span> | `true` |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | <span data-ttu-id="6b1d7-166">Maksymalna liczba współbieżnych akceptuje.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-166">The maximum number of concurrent accepts.</span></span> | <span data-ttu-id="6b1d7-167">5 &times; [środowiska.<br> ProcessorCount](xref:System.Environment.ProcessorCount)</span><span class="sxs-lookup"><span data-stu-id="6b1d7-167">5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | <span data-ttu-id="6b1d7-168">Maksymalna liczba jednoczesnych połączeń, aby zaakceptować.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-168">The maximum number of concurrent connections to accept.</span></span> <span data-ttu-id="6b1d7-169">Użyj `-1` = nieskończoność.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-169">Use `-1` for infinite.</span></span> <span data-ttu-id="6b1d7-170">Użyj `null` używać ustawienia komputera.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-170">Use `null` to use the registry's machine-wide setting.</span></span> | `null`<br><span data-ttu-id="6b1d7-171">(unlimited)</span><span class="sxs-lookup"><span data-stu-id="6b1d7-171">(unlimited)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | <span data-ttu-id="6b1d7-172">Zobacz <a href="#maxrequestbodysize">MaxRequestBodySize</a> sekcji.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-172">See the <a href="#maxrequestbodysize">MaxRequestBodySize</a> section.</span></span> | <span data-ttu-id="6b1d7-173">Bajty 30000000</span><span class="sxs-lookup"><span data-stu-id="6b1d7-173">30000000 bytes</span></span><br><span data-ttu-id="6b1d7-174">(~28.6 MB)</span><span class="sxs-lookup"><span data-stu-id="6b1d7-174">(~28.6 MB)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | <span data-ttu-id="6b1d7-175">Maksymalna liczba żądań, które można umieścić w kolejce.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-175">The maximum number of requests that can be queued.</span></span> | <span data-ttu-id="6b1d7-176">1000</span><span class="sxs-lookup"><span data-stu-id="6b1d7-176">1000</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | <span data-ttu-id="6b1d7-177">Wskazuje, jeśli operacji zapisu treści odpowiedzi, które rozłączy się niepowodzeniem z powodu klienta należy zgłaszać wyjątki lub zakończone normalnie.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-177">Indicate if response body writes that fail due to client disconnects should throw exceptions or complete normally.</span></span> | `false`<br><span data-ttu-id="6b1d7-178">(zazwyczaj zakończenie)</span><span class="sxs-lookup"><span data-stu-id="6b1d7-178">(complete normally)</span></span> |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | <span data-ttu-id="6b1d7-179">Udostępnianie HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> konfiguracji, która może być również konfigurowane w rejestrze.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-179">Expose the HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> configuration, which may also be configured in the registry.</span></span> <span data-ttu-id="6b1d7-180">Skorzystaj z linków interfejsu API, aby dowiedzieć się więcej na temat każdego ustawienia, w tym wartości domyślne:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-180">Follow the API links to learn more about each setting, including default values:</span></span><ul><li><span data-ttu-id="6b1d7-181">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; czasu dozwolony dla interfejsu API serwera HTTP opróżnić treści jednostki połączenia Keep-Alive.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-181">[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; Time allowed for the HTTP Server API to drain the entity body on a Keep-Alive connection.</span></span></li><li><span data-ttu-id="6b1d7-182">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; czasu dozwolony dla treści jednostki żądania dostarczenie.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-182">[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; Time allowed for the request entity body to arrive.</span></span></li><li><span data-ttu-id="6b1d7-183">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; czasu dozwolony dla interfejsu API serwera HTTP, można przeanalizować żądanego nagłówka.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-183">[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; Time allowed for the HTTP Server API to parse the request header.</span></span></li><li><span data-ttu-id="6b1d7-184">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; czasu dozwolony dla bezczynnego połączenia.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-184">[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; Time allowed for an idle connection.</span></span></li><li><span data-ttu-id="6b1d7-185">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; minimum Wyślij szybkości odpowiedzi na.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-185">[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; The minimum send rate for the response.</span></span></li><li><span data-ttu-id="6b1d7-186">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; czas przeznaczony na żądanie pozostawać w kolejce żądań, zanim go przejmuje przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-186">[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; Time allowed for the request to remain in the request queue before the app picks it up.</span></span></li></ul> |  |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | <span data-ttu-id="6b1d7-187">Określ <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> do rejestrowania w pliku HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-187">Specify the <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> to register with HTTP.sys.</span></span> <span data-ttu-id="6b1d7-188">Najbardziej przydatna jest [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), która umożliwia Dodaj prefiks do kolekcji.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-188">The most useful is [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), which is used to add a prefix to the collection.</span></span> <span data-ttu-id="6b1d7-189">Te mogą zostać zmienione w dowolnym momencie przed disposing odbiornika.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-189">These may be modified at any time prior to disposing the listener.</span></span> |  |

   <a name="maxrequestbodysize"></a>

   <span data-ttu-id="6b1d7-190">**MaxRequestBodySize**</span><span class="sxs-lookup"><span data-stu-id="6b1d7-190">**MaxRequestBodySize**</span></span>

   <span data-ttu-id="6b1d7-191">Maksymalny dozwolony rozmiar wszelkie treści żądania, w bajtach.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-191">The maximum allowed size of any request body in bytes.</span></span> <span data-ttu-id="6b1d7-192">Po ustawieniu `null`, żądanie maksymalny rozmiar treści jest nieograniczona.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-192">When set to `null`, the maximum request body size is unlimited.</span></span> <span data-ttu-id="6b1d7-193">To ograniczenie nie ma wpływu na uaktualnionym połączeń, które zawsze są nieograniczone.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-193">This limit has no effect on upgraded connections, which are always unlimited.</span></span>

   <span data-ttu-id="6b1d7-194">Zalecaną metodą do zastąpienia limitu w aplikacji ASP.NET Core MVC dla pojedynczego `IActionResult` jest użycie <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> atrybutu metody akcji:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-194">The recommended method to override the limit in an ASP.NET Core MVC app for a single `IActionResult` is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   <span data-ttu-id="6b1d7-195">Wyjątek jest generowany, jeśli aplikacja próbuje skonfigurować limit na żądanie, po uruchomieniu aplikacji odczytu żądania.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-195">An exception is thrown if the app attempts to configure the limit on a request after the app has started reading the request.</span></span> <span data-ttu-id="6b1d7-196">`IsReadOnly` Właściwość może służyć do wskazania, jeśli `MaxRequestBodySize` właściwość jest w stanie tylko do odczytu, co oznacza, jest za późno, aby skonfigurować limit.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-196">An `IsReadOnly` property can be used to indicate if the `MaxRequestBodySize` property is in a read-only state, meaning it's too late to configure the limit.</span></span>

   <span data-ttu-id="6b1d7-197">Jeśli aplikacja powinien przesłonić <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> żądań, należy użyć <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-197">If the app should override <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> per-request, use the <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:</span></span>

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

3. <span data-ttu-id="6b1d7-198">Jeśli używasz programu Visual Studio, upewnij się, że aplikacja nie jest skonfigurowana do uruchamiania usług IIS lub IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-198">If using Visual Studio, make sure the app isn't configured to run IIS or IIS Express.</span></span>

   <span data-ttu-id="6b1d7-199">W programie Visual Studio domyślnym profilem uruchamiania jest dla usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-199">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="6b1d7-200">Aby uruchomić projekt jako aplikację konsolową, ręcznie zmienić wybranego profilu, jak pokazano na poniższym zrzucie ekranu:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-200">To run the project as a console app, manually change the selected profile, as shown in the following screen shot:</span></span>

   ![Wybierz profil aplikacji konsoli](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a><span data-ttu-id="6b1d7-202">Konfigurowanie systemu Windows Server</span><span class="sxs-lookup"><span data-stu-id="6b1d7-202">Configure Windows Server</span></span>

1. <span data-ttu-id="6b1d7-203">Określenia portów, aby otworzyć aplikację i używania [zapory Windows](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) lub [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) polecenia cmdlet programu PowerShell, aby otworzyć porty zapory muszą zezwalać na ruch do osiągnięcia HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-203">Determine the ports to open for the app and use [Windows Firewall](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) or the [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell cmdlet to open firewall ports to allow traffic to reach HTTP.sys.</span></span> <span data-ttu-id="6b1d7-204">W poniższych poleceń i konfiguracji aplikacji używany jest port 443.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-204">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="6b1d7-205">Podczas wdrażania aplikacji na Maszynie wirtualnej platformy Azure, należy otworzyć ruch na portach [sieciowej grupy zabezpieczeń](/azure/virtual-machines/windows/nsg-quickstart-portal).</span><span class="sxs-lookup"><span data-stu-id="6b1d7-205">When deploying to an Azure VM, open the ports in the [Network Security Group](/azure/virtual-machines/windows/nsg-quickstart-portal).</span></span> <span data-ttu-id="6b1d7-206">W poniższych poleceń i konfiguracji aplikacji używany jest port 443.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-206">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="6b1d7-207">Uzyskaj i zainstaluj certyfikaty X.509, jeśli jest to wymagane.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-207">Obtain and install X.509 certificates, if required.</span></span>

   <span data-ttu-id="6b1d7-208">W Windows, tworzenie certyfikatów z podpisem własnym za pomocą [polecenia cmdlet programu PowerShell New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="6b1d7-208">On Windows, create self-signed certificates using the [New-SelfSignedCertificate PowerShell cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate).</span></span> <span data-ttu-id="6b1d7-209">Na przykład nieobsługiwany zobacz [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span><span class="sxs-lookup"><span data-stu-id="6b1d7-209">For an unsupported example, see [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span></span>

   <span data-ttu-id="6b1d7-210">Instalowanie certyfikatów z podpisem własnym albo podpisanych przez urząd certyfikacji na serwerze **komputera lokalnego** > **osobistych** przechowywania.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-210">Install either self-signed or CA-signed certificates in the server's **Local Machine** > **Personal** store.</span></span>

1. <span data-ttu-id="6b1d7-211">Jeśli aplikacja jest [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd), zainstalowania platformy .NET Core i .NET Framework (Jeśli aplikacja jest aplikacją platformy .NET Core przeznaczonych dla platformy .NET Framework).</span><span class="sxs-lookup"><span data-stu-id="6b1d7-211">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd), install .NET Core, .NET Framework, or both (if the app is a .NET Core app targeting the .NET Framework).</span></span>

   * <span data-ttu-id="6b1d7-212">**.NET core** &ndash; Jeśli aplikacja wymaga platformy .NET Core, uzyskanie i uruchomienie **środowisko uruchomieniowe programu .NET Core** Instalator [pobierania programu .NET Core](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="6b1d7-212">**.NET Core** &ndash; If the app requires .NET Core, obtain and run the **.NET Core Runtime** installer from [.NET Core Downloads](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="6b1d7-213">Nie należy instalować pełnego zestawu SDK na serwerze.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-213">Don't install the full SDK on the server.</span></span>
   * <span data-ttu-id="6b1d7-214">**.NET framework** &ndash; Jeśli aplikacja wymaga programu .NET Framework, zobacz [Przewodnik instalacji .NET Framework](/dotnet/framework/install/).</span><span class="sxs-lookup"><span data-stu-id="6b1d7-214">**.NET Framework** &ndash; If the app requires .NET Framework, see the [.NET Framework installation guide](/dotnet/framework/install/).</span></span> <span data-ttu-id="6b1d7-215">Zainstaluj wymagane .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-215">Install the required .NET Framework.</span></span> <span data-ttu-id="6b1d7-216">Instalator dla najnowszej wersji .NET Framework jest dostępna z [pobierania programu .NET Core](https://dotnet.microsoft.com/download) strony.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-216">The installer for the latest .NET Framework is available from the [.NET Core Downloads](https://dotnet.microsoft.com/download) page.</span></span>

   <span data-ttu-id="6b1d7-217">Jeśli aplikacja jest [niezależna wdrożenia](/dotnet/core/deploying/#framework-dependent-deployments-scd), aplikacja zawiera środowisko uruchomieniowe w jej wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-217">If the app is a [self-contained deployment](/dotnet/core/deploying/#framework-dependent-deployments-scd), the app includes the runtime in its deployment.</span></span> <span data-ttu-id="6b1d7-218">Instalacja framework nie jest wymagane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-218">No framework installation is required on the server.</span></span>

1. <span data-ttu-id="6b1d7-219">Konfigurowanie adresów URL i portów w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-219">Configure URLs and ports in the app.</span></span>

   <span data-ttu-id="6b1d7-220">Domyślnie platforma ASP.NET Core wiąże `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-220">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="6b1d7-221">Aby skonfigurować przedrostki adresów URL i portów, dostępne są następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-221">To configure URL prefixes and ports, options include:</span></span>

   * <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
   * <span data-ttu-id="6b1d7-222">`urls` argument wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="6b1d7-222">`urls` command-line argument</span></span>
   * <span data-ttu-id="6b1d7-223">`ASPNETCORE_URLS` Zmienna środowiskowa</span><span class="sxs-lookup"><span data-stu-id="6b1d7-223">`ASPNETCORE_URLS` environment variable</span></span>
   * <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes>

   <span data-ttu-id="6b1d7-224">Poniższy przykład kodu pokazuje sposób użycia <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> z serwera lokalnego adresu IP `10.0.0.4` na porcie 443:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-224">The following code example shows how to use <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> with the server's local IP address `10.0.0.4` on port 443:</span></span>

   [!code-csharp[](httpsys/sample_snapshot/Program.cs?name=snippet1&highlight=11)]

   <span data-ttu-id="6b1d7-225">Zaletą `UrlPrefixes` jest natychmiast wygenerowany komunikat o błędzie w przypadku prefiksów niewłaściwie sformatowany.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-225">An advantage of `UrlPrefixes` is that an error message is generated immediately for improperly formatted prefixes.</span></span>

   <span data-ttu-id="6b1d7-226">Ustawienia w `UrlPrefixes` zastąpienia `UseUrls` / `urls` / `ASPNETCORE_URLS` ustawienia.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-226">The settings in `UrlPrefixes` override `UseUrls`/`urls`/`ASPNETCORE_URLS` settings.</span></span> <span data-ttu-id="6b1d7-227">W związku z tym, zaletą `UseUrls`, `urls`i `ASPNETCORE_URLS` zmienna środowiskowa jest łatwiejsze przełączanie między Kestrel i sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-227">Therefore, an advantage of `UseUrls`, `urls`, and the `ASPNETCORE_URLS` environment variable is that it's easier to switch between Kestrel and HTTP.sys.</span></span> <span data-ttu-id="6b1d7-228">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-228">For more information, see <xref:fundamentals/host/web-host>.</span></span>

   <span data-ttu-id="6b1d7-229">Używa HTTP.sys [UrlPrefix interfejsu API serwera HTTP, formaty ciągu](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="6b1d7-229">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

   > [!WARNING]
   > <span data-ttu-id="6b1d7-230">Powiązania najwyższego poziomu symbolu wieloznacznego (`http://*:80/` i `http://+:80`) powinien **nie** można użyć.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-230">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="6b1d7-231">Symbol wieloznaczny najwyższego poziomu powiązania Utwórz aplikację luk w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-231">Top-level wildcard bindings create app security vulnerabilities.</span></span> <span data-ttu-id="6b1d7-232">Dotyczy to silnych i słabych symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-232">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="6b1d7-233">Użyj hosta jawne nazwy lub adresy IP, a nie symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-233">Use explicit host names or IP addresses rather than wildcards.</span></span> <span data-ttu-id="6b1d7-234">Powiązanie symbol wieloznaczny domeny podrzędnej (na przykład `*.mysub.com`) nie jest to zagrożenie bezpieczeństwa, jeśli możesz kontrolować domenę nadrzędną całego (w przeciwieństwie do `*.com`, który jest narażony).</span><span class="sxs-lookup"><span data-stu-id="6b1d7-234">Subdomain wildcard binding (for example, `*.mysub.com`) isn't a security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="6b1d7-235">Aby uzyskać więcej informacji, zobacz [RFC 7230: Ppkt 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).</span><span class="sxs-lookup"><span data-stu-id="6b1d7-235">For more information, see [RFC 7230: Section 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).</span></span>

1. <span data-ttu-id="6b1d7-236">Preregister przedrostki adresów URL na serwerze.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-236">Preregister URL prefixes on the server.</span></span>

   <span data-ttu-id="6b1d7-237">Wbudowane narzędzie do konfigurowania HTTP.sys *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-237">The built-in tool for configuring HTTP.sys is *netsh.exe*.</span></span> <span data-ttu-id="6b1d7-238">*netsh.exe* służy do zarezerwowania przedrostki adresów URL i przypisywać certyfikaty X.509.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-238">*netsh.exe* is used to reserve URL prefixes and assign X.509 certificates.</span></span> <span data-ttu-id="6b1d7-239">Narzędzie wymaga uprawnień administratora.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-239">The tool requires administrator privileges.</span></span>

   <span data-ttu-id="6b1d7-240">Użyj *netsh.exe* narzędzie, aby zarejestrować adresy URL aplikacji:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-240">Use the *netsh.exe* tool to register URLs for the app:</span></span>

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * <span data-ttu-id="6b1d7-241">`<URL>` &ndash; W pełni kwalifikowaną URL Uniform Resource Locator ().</span><span class="sxs-lookup"><span data-stu-id="6b1d7-241">`<URL>` &ndash; The fully qualified Uniform Resource Locator (URL).</span></span> <span data-ttu-id="6b1d7-242">Nie używaj wiązania symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-242">Don't use a wildcard binding.</span></span> <span data-ttu-id="6b1d7-243">Za pomocą prawidłową nazwę hosta lokalnego adresu IP.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-243">Use a valid hostname or local IP address.</span></span> <span data-ttu-id="6b1d7-244">*Adres URL musi zawierać końcowego ukośnika.*</span><span class="sxs-lookup"><span data-stu-id="6b1d7-244">*The URL must include a trailing slash.*</span></span>
   * <span data-ttu-id="6b1d7-245">`<USER>` &ndash; Określa nazwę użytkownika lub grupy użytkowników.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-245">`<USER>` &ndash; Specifies the user or user-group name.</span></span>

   <span data-ttu-id="6b1d7-246">W poniższym przykładzie jest lokalny adres IP serwera `10.0.0.4`:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-246">In the following example, the local IP address of the server is `10.0.0.4`:</span></span>

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   <span data-ttu-id="6b1d7-247">Po zarejestrowaniu się adres URL, narzędzie odpowiada za pomocą `URL reservation successfully added`.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-247">When a URL is registered, the tool responds with `URL reservation successfully added`.</span></span>

   <span data-ttu-id="6b1d7-248">Aby usunąć zarejestrowanego adresu URL, użyj `delete urlacl` polecenia:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-248">To delete a registered URL, use the `delete urlacl` command:</span></span>

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. <span data-ttu-id="6b1d7-249">Zarejestruj certyfikaty X.509 na serwerze.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-249">Register X.509 certificates on the server.</span></span>

   <span data-ttu-id="6b1d7-250">Użyj *netsh.exe* narzędzia, aby zarejestrować certyfikat dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-250">Use the *netsh.exe* tool to register certificates for the app:</span></span>

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * <span data-ttu-id="6b1d7-251">`<IP>` &ndash; Określa lokalny adres IP dla wiązania.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-251">`<IP>` &ndash; Specifies the local IP address for the binding.</span></span> <span data-ttu-id="6b1d7-252">Nie używaj wiązania symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-252">Don't use a wildcard binding.</span></span> <span data-ttu-id="6b1d7-253">Użyj prawidłowego adresu IP.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-253">Use a valid IP address.</span></span>
   * <span data-ttu-id="6b1d7-254">`<PORT>` &ndash; Określa port dla wiązania.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-254">`<PORT>` &ndash; Specifies the port for the binding.</span></span>
   * <span data-ttu-id="6b1d7-255">`<THUMBPRINT>` &ndash; Odcisk palca certyfikatu X.509.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-255">`<THUMBPRINT>` &ndash; The X.509 certificate thumbprint.</span></span>
   * <span data-ttu-id="6b1d7-256">`<GUID>` &ndash; Wygenerowany przez projektanta identyfikator GUID do reprezentowania aplikacji do celów informacyjnych.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-256">`<GUID>` &ndash; A developer-generated GUID to represent the app for informational purposes.</span></span>

   <span data-ttu-id="6b1d7-257">Do celów referencyjnych zapisywania identyfikatora GUID w aplikacji jako tag pakietu:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-257">For reference purposes, store the GUID in the app as a package tag:</span></span>

   * <span data-ttu-id="6b1d7-258">W programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-258">In Visual Studio:</span></span>
     * <span data-ttu-id="6b1d7-259">Otwórz właściwości projektu aplikacji klikając prawym przyciskiem myszy w aplikacji w **Eksploratora rozwiązań** i wybierając polecenie **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-259">Open the app's project properties by right-clicking on the app in **Solution Explorer** and selecting **Properties**.</span></span>
     * <span data-ttu-id="6b1d7-260">Wybierz **pakietu** kartę.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-260">Select the **Package** tab.</span></span>
     * <span data-ttu-id="6b1d7-261">Wprowadź identyfikator GUID, który został utworzony w **tagi** pola.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-261">Enter the GUID that you created in the **Tags** field.</span></span>
   * <span data-ttu-id="6b1d7-262">Bez korzystania z programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-262">When not using Visual Studio:</span></span>
     * <span data-ttu-id="6b1d7-263">Otwórz plik projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-263">Open the app's project file.</span></span>
     * <span data-ttu-id="6b1d7-264">Dodaj `<PackageTags>` właściwości na nową lub istniejącą `<PropertyGroup>` z identyfikatorem GUID, który został utworzony:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-264">Add a `<PackageTags>` property to a new or existing `<PropertyGroup>` with the GUID that you created:</span></span>

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   <span data-ttu-id="6b1d7-265">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-265">In the following example:</span></span>

   * <span data-ttu-id="6b1d7-266">Lokalny adres IP serwera jest `10.0.0.4`.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-266">The local IP address of the server is `10.0.0.4`.</span></span>
   * <span data-ttu-id="6b1d7-267">Zapewnia online losowe generator GUID `appid` wartość.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-267">An online random GUID generator provides the `appid` value.</span></span>

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   <span data-ttu-id="6b1d7-268">Po zarejestrowaniu certyfikatu, narzędzie odpowiada za pomocą `SSL Certificate successfully added`.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-268">When a certificate is registered, the tool responds with `SSL Certificate successfully added`.</span></span>

   <span data-ttu-id="6b1d7-269">Aby usunąć rejestracji certyfikatu, użyj `delete sslcert` polecenia:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-269">To delete a certificate registration, use the `delete sslcert` command:</span></span>

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   <span data-ttu-id="6b1d7-270">Dokumentacji dla *netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="6b1d7-270">Reference documentation for *netsh.exe*:</span></span>

   * [<span data-ttu-id="6b1d7-271">Polecenia Netsh dla Hypertext Transfer Protocol (HTTP)</span><span class="sxs-lookup"><span data-stu-id="6b1d7-271">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
   * [<span data-ttu-id="6b1d7-272">Ciągi UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="6b1d7-272">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

1. <span data-ttu-id="6b1d7-273">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-273">Run the app.</span></span>

   <span data-ttu-id="6b1d7-274">Uprawnienia administratora nie są wymagane do uruchomienia aplikacji, podczas tworzenia wiązania do hosta lokalnego przy użyciu protokołu HTTP (a nie HTTPS), numer portu jest większa niż 1024.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-274">Administrator privileges aren't required to run the app when binding to localhost using HTTP (not HTTPS) with a port number greater than 1024.</span></span> <span data-ttu-id="6b1d7-275">W przypadku innych konfiguracji (na przykład przy użyciu lokalnego adresu IP lub powiązania z portem 443) należy uruchomić tę aplikację z uprawnieniami administratora.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-275">For other configurations (for example, using a local IP address or binding to port 443), run the app with administrator privileges.</span></span>

   <span data-ttu-id="6b1d7-276">Aplikacja reaguje na publiczny adres IP serwera.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-276">The app responds at the server's public IP address.</span></span> <span data-ttu-id="6b1d7-277">W tym przykładzie jest połączyć się z serwerem z sieci Internet, publiczny adres IP `104.214.79.47`.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-277">In this example, the server is reached from the Internet at its public IP address of `104.214.79.47`.</span></span>

   <span data-ttu-id="6b1d7-278">Certyfikatu deweloperskiego jest używany w tym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-278">A development certificate is used in this example.</span></span> <span data-ttu-id="6b1d7-279">Strona ładuje się bezpiecznie po pomijanie ostrzeżeń niezaufanego certyfikatu w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-279">The page loads securely after bypassing the browser's untrusted certificate warning.</span></span>

   ![Załadowano indeksu aplikacji wyświetlana w oknie przeglądarki](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="6b1d7-281">Serwer proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="6b1d7-281">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="6b1d7-282">Dla aplikacji hostowanych przez rozszerzenie HTTP.sys, które współdziałają z żądaniami z Internetu lub sieci firmowej dodatkowa konfiguracja może być wymagane w przypadku hostowania za serwery proxy i moduły równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="6b1d7-282">For apps hosted by HTTP.sys that interact with requests from the Internet or a corporate network, additional configuration might be required when hosting behind proxy servers and load balancers.</span></span> <span data-ttu-id="6b1d7-283">Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="6b1d7-283">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6b1d7-284">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6b1d7-284">Additional resources</span></span>

* [<span data-ttu-id="6b1d7-285">Włącz uwierzytelnianie Windows w pliku HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="6b1d7-285">Enable Windows Authentication with HTTP.sys</span></span>](xref:security/authentication/windowsauth#enable-windows-authentication-with-httpsys)
* [<span data-ttu-id="6b1d7-286">Serwer HTTP interfejsu API</span><span class="sxs-lookup"><span data-stu-id="6b1d7-286">HTTP Server API</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [<span data-ttu-id="6b1d7-287">repozytorium GitHub ASPNET/HttpSysServer (kodu źródłowego)</span><span class="sxs-lookup"><span data-stu-id="6b1d7-287">aspnet/HttpSysServer GitHub repository (source code)</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="6b1d7-288">Host</span><span class="sxs-lookup"><span data-stu-id="6b1d7-288">The host</span></span>](xref:fundamentals/index#host)
* <xref:test/troubleshoot>
