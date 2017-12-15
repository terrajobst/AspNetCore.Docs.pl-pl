---
title: Sterownik HTTP.sys implementacja serwera sieci web platformy ASP.NET Core
author: rick-anderson
description: "Wprowadza HTTP.sys, serwer sieci web platformy ASP.NET Core w systemie Windows. W oparciu sterownik trybu jądra Http.Sys, sterownik HTTP.sys stanowi alternatywę dla Kestrel, który może służyć do bezpośredniego połączenia z Internetem bez usług IIS."
keywords: "ASP.NET Core,HttpSys,HTTP.sys,HttpListener,url prefiksy protokołu SSL"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 8d46862af44379d8592efdf214a80214dce2d69d
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="7b112-105">Sterownik HTTP.sys implementacja serwera sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7b112-105">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="7b112-106">Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Roaming Krzysztof](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="7b112-106">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="7b112-107">Ten temat dotyczy tylko programu ASP.NET Core 2.0 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="7b112-107">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="7b112-108">We wcześniejszych wersjach programu ASP.NET Core, nosi nazwę HTTP.sys [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="7b112-108">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="7b112-109">Sterownik HTTP.sys jest [serwera sieci web dla platformy ASP.NET Core](index.md) , na którym działają tylko w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="7b112-109">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="7b112-110">Jest zbudowany na [sterownik trybu jądra Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="7b112-110">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="7b112-111">Sterownik HTTP.sys stanowi alternatywę dla [Kestrel](kestrel.md) oferująca niektórych funkcji, które nie Kestel.</span><span class="sxs-lookup"><span data-stu-id="7b112-111">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="7b112-112">**Składnik HTTP.sys nie można używać z usług IIS lub usług IIS Express, ponieważ nie jest zgodny z [platformy ASP.NET Core modułu](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="7b112-112">**HTTP.sys can't be used with IIS or IIS Express, as it's incompatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="7b112-113">Sterownik HTTP.sys obsługuje następujące funkcje:</span><span class="sxs-lookup"><span data-stu-id="7b112-113">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="7b112-114">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="7b112-114">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="7b112-115">Udostępnianie portów</span><span class="sxs-lookup"><span data-stu-id="7b112-115">Port sharing</span></span>
- <span data-ttu-id="7b112-116">Protokół HTTPS z SNI</span><span class="sxs-lookup"><span data-stu-id="7b112-116">HTTPS with SNI</span></span>
- <span data-ttu-id="7b112-117">HTTP/2 za pośrednictwem protokołu TLS (z systemem Windows 10)</span><span class="sxs-lookup"><span data-stu-id="7b112-117">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="7b112-118">Przekazywanie pliku bezpośredniego</span><span class="sxs-lookup"><span data-stu-id="7b112-118">Direct file transmission</span></span>
- <span data-ttu-id="7b112-119">Buforowanie odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="7b112-119">Response caching</span></span>
- <span data-ttu-id="7b112-120">Protokół WebSockets (z systemem Windows 8)</span><span class="sxs-lookup"><span data-stu-id="7b112-120">WebSockets (Windows 8)</span></span>

<span data-ttu-id="7b112-121">Obsługiwane wersje systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="7b112-121">Supported Windows versions:</span></span>

- <span data-ttu-id="7b112-122">Windows 7 i Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="7b112-122">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="7b112-123">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7b112-123">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="7b112-124">Kiedy należy używać HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="7b112-124">When to use HTTP.sys</span></span>

<span data-ttu-id="7b112-125">Sterownik HTTP.sys jest przydatne w przypadku wdrożeń, których należy udostępnić server bezpośrednio do Internetu, nie za pomocą usług IIS.</span><span class="sxs-lookup"><span data-stu-id="7b112-125">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Sterownik HTTP.sys komunikuje się bezpośrednio z Internetem](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="7b112-127">Ponieważ jest zbudowany na Http.Sys, sterownik HTTP.sys nie wymaga zwrotnego serwera proxy dla ochrony przed atakami.</span><span class="sxs-lookup"><span data-stu-id="7b112-127">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="7b112-128">Sterownik Http.Sys jest dojrzała technologia, która chroni przed wiele rodzajów ataków i zapewnia niezawodność, zabezpieczeń i skalowalność serwera sieci web kompletne.</span><span class="sxs-lookup"><span data-stu-id="7b112-128">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="7b112-129">SAM działa jako odbiornik HTTP u góry pliku Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="7b112-129">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="7b112-130">Sterownik HTTP.sys jest dobrym rozwiązaniem w przypadku wdrożeń wewnętrznego, gdy będziesz potrzebować funkcja nie jest dostępna w Kestrel, takich jak uwierzytelnianie systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="7b112-130">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![Sterownik HTTP.sys komunikuje się bezpośrednio z sieci wewnętrznej](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="7b112-132">Jak używać HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="7b112-132">How to use HTTP.sys</span></span>

<span data-ttu-id="7b112-133">Poniżej przedstawiono omówienie zadań instalacji systemu operacyjnego hosta i aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7b112-133">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="7b112-134">Konfigurowanie systemu Windows Server</span><span class="sxs-lookup"><span data-stu-id="7b112-134">Configure Windows Server</span></span>

* <span data-ttu-id="7b112-135">Zainstaluj wersję platformy .NET, która wymaga aplikacji, takich jak [.NET Core](https://www.microsoft.com/net/download/core) lub [.NET Framework](https://www.microsoft.com/net/download/framework).</span><span class="sxs-lookup"><span data-stu-id="7b112-135">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="7b112-136">Preregister prefiksów URL do powiązania do pliku HTTP.sys i skonfigurować certyfikaty SSL</span><span class="sxs-lookup"><span data-stu-id="7b112-136">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="7b112-137">Jeśli nie preregister prefiksy adresów URL w systemie Windows, należy do uruchamiania aplikacji z uprawnieniami administratora.</span><span class="sxs-lookup"><span data-stu-id="7b112-137">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="7b112-138">Jedynym wyjątkiem jest powiązaniu localhost przy użyciu protokołu HTTP (a nie HTTPS) z większą niż 1024; numeru portu w takim przypadku uprawnienia administratora nie są wymagane.</span><span class="sxs-lookup"><span data-stu-id="7b112-138">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="7b112-139">Aby uzyskać więcej informacji, zobacz [preregister prefiksy i konfigurowanie protokołu SSL](#preregister-url-prefixes-and-configure-ssl) dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="7b112-139">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="7b112-140">Otwórz porty zapory, aby zezwolić na ruch do pliku HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="7b112-140">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="7b112-141">Można użyć *netsh.exe* lub [poleceń cmdlet programu PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="7b112-141">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="7b112-142">Dostępne są także [ustawienia rejestru Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="7b112-142">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="7b112-143">Konfigurowanie aplikacji platformy ASP.NET Core używać HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="7b112-143">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="7b112-144">Żadna instalacja pakietu jest niezbędny w przypadku używasz [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span><span class="sxs-lookup"><span data-stu-id="7b112-144">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="7b112-145">[Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) pakietu znajduje się w metapackage.</span><span class="sxs-lookup"><span data-stu-id="7b112-145">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="7b112-146">Wywołanie `UseHttpSys` — metoda rozszerzenia na `WebHostBuilder` w Twojej `Main` metoda określania żadnego [opcje HTTP.sys](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) potrzebne, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="7b112-146">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="7b112-147">Skonfiguruj opcje HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="7b112-147">Configure HTTP.sys options</span></span>

<span data-ttu-id="7b112-148">Poniżej przedstawiono niektóre ustawienia HTTP.sys i ograniczeń, które można skonfigurować.</span><span class="sxs-lookup"><span data-stu-id="7b112-148">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="7b112-149">**Maksymalna liczba połączeń klientów**</span><span class="sxs-lookup"><span data-stu-id="7b112-149">**Maximum client connections**</span></span>

<span data-ttu-id="7b112-150">Można ustawić maksymalną liczbę równoczesnych otwartych połączeń TCP dla całej aplikacji w następującym kodem *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="7b112-150">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="7b112-151">Maksymalna liczba połączeń jest nieograniczony (null) domyślnie.</span><span class="sxs-lookup"><span data-stu-id="7b112-151">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="7b112-152">**Żądanie maksymalny rozmiar treści**</span><span class="sxs-lookup"><span data-stu-id="7b112-152">**Maximum request body size**</span></span>

<span data-ttu-id="7b112-153">Domyślny rozmiar treści żądania maksymalna jest 30,000,000 bajtów, czyli około 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="7b112-153">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="7b112-154">Zalecanym sposobem zastąpienie limit w aplikacji platformy ASP.NET Core MVC jest użycie [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) atrybutu metody akcji:</span><span class="sxs-lookup"><span data-stu-id="7b112-154">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="7b112-155">Oto przykład, który pokazuje, jak skonfigurować ograniczenia dla całej aplikacji, każde żądanie:</span><span class="sxs-lookup"><span data-stu-id="7b112-155">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="7b112-156">Można zastąpić ustawienie dla określonego żądania w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7b112-156">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="7b112-157">Jeśli próbujesz skonfigurować limit na żądanie, po uruchomieniu aplikacji odczytu żądania, jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="7b112-157">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="7b112-158">Brak `IsReadOnly` właściwość, która informuje, jeśli `MaxRequestBodySize` właściwość jest w stanie tylko do odczytu, co oznacza jest za późno skonfigurować limit.</span><span class="sxs-lookup"><span data-stu-id="7b112-158">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="7b112-159">Aby uzyskać informacje o innych opcjach HTTP.sys, zobacz [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="7b112-159">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="7b112-160">Skonfiguruj adresy URL i portów do nasłuchiwania</span><span class="sxs-lookup"><span data-stu-id="7b112-160">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="7b112-161">Domyślnie wiąże platformy ASP.NET Core `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="7b112-161">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="7b112-162">Aby skonfigurować prefiksy URL i portów, można użyć `UseUrls` — metoda rozszerzenia, `urls` argumentu wiersza polecenia, zmienną środowiskową ASPNETCORE_URLS lub `UrlPrefixes` właściwość [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="7b112-162">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="7b112-163">Poniższy przykład kodu wykorzystuje `UrlPrefixes`.</span><span class="sxs-lookup"><span data-stu-id="7b112-163">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="7b112-164">Zaletą `UrlPrefixes` jest, że zostanie wyświetlony komunikat o błędzie natychmiast Jeśli próbujesz dodać prefiks, który jest sformatowany problem.</span><span class="sxs-lookup"><span data-stu-id="7b112-164">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that is formatted wrong.</span></span> <span data-ttu-id="7b112-165">Zaletą `UseUrls` (udostępniane `urls` i ASPNETCORE_URLS) to, że można łatwo przełączać się między Kestrel i sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="7b112-165">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="7b112-166">Jeśli używasz zarówno `UseUrls` (lub `urls` lub ASPNETCORE_URLS) i `UrlPrefixes`, ustawienia w `UrlPrefixes` przesłonić te w `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="7b112-166">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="7b112-167">Aby uzyskać więcej informacji, zobacz [hostingu](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="7b112-167">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="7b112-168">Korzysta z pliku HTTP.sys [formaty ciągu UrlPrefix interfejsu API serwera HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="7b112-168">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="7b112-169">Upewnij się, że określono tego samego prefiksu ciągi w `UseUrls` lub `UrlPrefixes` który preregister na serwerze.</span><span class="sxs-lookup"><span data-stu-id="7b112-169">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="7b112-170">Nie używaj usług IIS</span><span class="sxs-lookup"><span data-stu-id="7b112-170">Don't use IIS</span></span>

<span data-ttu-id="7b112-171">Upewnij się, że aplikacja nie jest skonfigurowana do uruchamiania usług IIS lub usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="7b112-171">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="7b112-172">W programie Visual Studio domyślnego profilu uruchamiania jest dla usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="7b112-172">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="7b112-173">Aby uruchomić projekt jako aplikacji konsoli, ręcznie zmienić wybranego profilu, jak pokazano na poniższym zrzucie ekranu.</span><span class="sxs-lookup"><span data-stu-id="7b112-173">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![Wybierz profil aplikacji konsoli](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="7b112-175">Preregister prefiksy URL i skonfigurować protokół SSL</span><span class="sxs-lookup"><span data-stu-id="7b112-175">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="7b112-176">Zarówno usług IIS, jak i składnik HTTP.sys polegać na podstawowy sterownik trybu jądra Http.Sys do nasłuchiwania żądań i wstępne przetwarzanie.</span><span class="sxs-lookup"><span data-stu-id="7b112-176">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="7b112-177">W usługach IIS interfejs użytkownika zarządzania umożliwia stosunkowo łatwa do skonfigurowania wszystko.</span><span class="sxs-lookup"><span data-stu-id="7b112-177">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="7b112-178">Jednak należy skonfigurować serwer Http.Sys samodzielnie.</span><span class="sxs-lookup"><span data-stu-id="7b112-178">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="7b112-179">Wbudowane narzędzie do wykonywania, czyli *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="7b112-179">The built-in tool for doing that is *netsh.exe*.</span></span> 

<span data-ttu-id="7b112-180">Z *netsh.exe* można zarezerwować prefiksy URL i przypisać certyfikatów SSL.</span><span class="sxs-lookup"><span data-stu-id="7b112-180">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="7b112-181">Narzędzie wymaga uprawnień administracyjnych.</span><span class="sxs-lookup"><span data-stu-id="7b112-181">The tool requires administrative privileges.</span></span>

<span data-ttu-id="7b112-182">W poniższym przykładzie przedstawiono minimalnej liczbie niezbędnej do zarezerwowania prefiksy URL porty 80 i 443:</span><span class="sxs-lookup"><span data-stu-id="7b112-182">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="7b112-183">Poniższy przykład przedstawia sposób przypisania certyfikatu SSL:</span><span class="sxs-lookup"><span data-stu-id="7b112-183">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="7b112-184">W tym miejscu znajduje się dokumentacja odwołanie *netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="7b112-184">Here is the reference documentation for *netsh.exe*:</span></span>

* [<span data-ttu-id="7b112-185">Polecenia Netsh dla Hypertext Transfer Protocol (HTTP)</span><span class="sxs-lookup"><span data-stu-id="7b112-185">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="7b112-186">Ciągi UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="7b112-186">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="7b112-187">Następujące zasoby zawierają szczegółowe instrukcje dotyczące kilka scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="7b112-187">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="7b112-188">Artykuły, które odwołują się do HttpListener są stosowane jednakowo do pliku HTTP.sys, zgodnie z obu są oparte na pliku Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="7b112-188">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="7b112-189">Instrukcje: konfigurowanie portu z certyfikatem SSL</span><span class="sxs-lookup"><span data-stu-id="7b112-189">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="7b112-190">[Komunikacja HTTPS - HttpListener na podstawie hostingu oraz certyfikatu klienta](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) to jest blogu innych firm i jest dość stary, ale nadal zawiera przydatne informacje.</span><span class="sxs-lookup"><span data-stu-id="7b112-190">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="7b112-191">[Porady: HttpListener za pomocą wskazówki lub serwer Http niezarządzanych kodu (C++) jako serwera SSL proste](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) starsze blogu zawierający przydatne informacje jest zbyt.</span><span class="sxs-lookup"><span data-stu-id="7b112-191">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>

<span data-ttu-id="7b112-192">Poniżej przedstawiono niektóre narzędzia innych firm, które mogą być łatwiejsze niż w *netsh.exe* wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="7b112-192">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="7b112-193">Nie są one udostępniane przez lub podpisane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="7b112-193">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="7b112-194">Narzędzia Uruchom jako administrator domyślnie, ponieważ *netsh.exe* sam wymaga uprawnień administratora.</span><span class="sxs-lookup"><span data-stu-id="7b112-194">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="7b112-195">[Sterownik HTTP.sys Menedżera](http://httpsysmanager.codeplex.com/) udostępnia interfejs do listy i konfigurowania certyfikatów SSL i opcji, zastrzeżenia prefiksu i listy zaufanych certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="7b112-195">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="7b112-196">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) pozwala wyświetlić lub skonfigurować certyfikaty SSL i prefiksy URL.</span><span class="sxs-lookup"><span data-stu-id="7b112-196">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="7b112-197">Interfejs użytkownika jest bardziej precyzyjnych niż http.sys Manager i udostępnia kilka więcej opcji konfiguracji, ale w przeciwnym razie zapewnia funkcje podobne.</span><span class="sxs-lookup"><span data-stu-id="7b112-197">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="7b112-198">Nie można utworzyć nową listę zaufania certyfikatów (CTL), ale można przypisać istniejące.</span><span class="sxs-lookup"><span data-stu-id="7b112-198">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="7b112-199">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="7b112-199">Next steps</span></span>

<span data-ttu-id="7b112-200">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="7b112-200">For more information, see the following resources:</span></span>

* [<span data-ttu-id="7b112-201">Przykładowa aplikacja dla tego artykułu</span><span class="sxs-lookup"><span data-stu-id="7b112-201">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [<span data-ttu-id="7b112-202">Sterownik HTTP.sys kodu źródłowego</span><span class="sxs-lookup"><span data-stu-id="7b112-202">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="7b112-203">Hosting</span><span class="sxs-lookup"><span data-stu-id="7b112-203">Hosting</span></span>](xref:fundamentals/hosting)
