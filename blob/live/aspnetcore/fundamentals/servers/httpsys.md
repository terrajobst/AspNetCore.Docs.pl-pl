---
title: Sterownik HTTP.sys implementacja serwera sieci web platformy ASP.NET Core
author: rick-anderson
description: "Wprowadza HTTP.sys, serwer sieci web platformy ASP.NET Core w systemie Windows. W oparciu sterownik trybu jądra Http.Sys, sterownik HTTP.sys stanowi alternatywę dla Kestrel, który może służyć do bezpośredniego połączenia z Internetem bez usług IIS."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 60301e1e3eb96f51e86ef9f8be61f5fd8a4c009c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="ed957-104">Sterownik HTTP.sys implementacja serwera sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed957-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="ed957-105">Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Roaming Krzysztof](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="ed957-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="ed957-106">Ten temat dotyczy tylko programu ASP.NET Core 2.0 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="ed957-106">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="ed957-107">We wcześniejszych wersjach programu ASP.NET Core, nosi nazwę HTTP.sys [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="ed957-107">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="ed957-108">Sterownik HTTP.sys jest [serwera sieci web dla platformy ASP.NET Core](index.md) , na którym działają tylko w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="ed957-108">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="ed957-109">Jest zbudowany na [sterownik trybu jądra Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="ed957-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="ed957-110">Sterownik HTTP.sys stanowi alternatywę dla [Kestrel](kestrel.md) oferująca niektórych funkcji, które nie Kestel.</span><span class="sxs-lookup"><span data-stu-id="ed957-110">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="ed957-111">**Składnik HTTP.sys nie można używać z usług IIS lub usług IIS Express, ponieważ nie jest zgodny z [platformy ASP.NET Core modułu](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="ed957-111">**HTTP.sys can't be used with IIS or IIS Express, as it's incompatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="ed957-112">Sterownik HTTP.sys obsługuje następujące funkcje:</span><span class="sxs-lookup"><span data-stu-id="ed957-112">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="ed957-113">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="ed957-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="ed957-114">Udostępnianie portów</span><span class="sxs-lookup"><span data-stu-id="ed957-114">Port sharing</span></span>
- <span data-ttu-id="ed957-115">Protokół HTTPS z SNI</span><span class="sxs-lookup"><span data-stu-id="ed957-115">HTTPS with SNI</span></span>
- <span data-ttu-id="ed957-116">HTTP/2 za pośrednictwem protokołu TLS (z systemem Windows 10)</span><span class="sxs-lookup"><span data-stu-id="ed957-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="ed957-117">Przekazywanie pliku bezpośredniego</span><span class="sxs-lookup"><span data-stu-id="ed957-117">Direct file transmission</span></span>
- <span data-ttu-id="ed957-118">Buforowanie odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="ed957-118">Response caching</span></span>
- <span data-ttu-id="ed957-119">Protokół WebSockets (z systemem Windows 8)</span><span class="sxs-lookup"><span data-stu-id="ed957-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="ed957-120">Obsługiwane wersje systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="ed957-120">Supported Windows versions:</span></span>

- <span data-ttu-id="ed957-121">Windows 7 i Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="ed957-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="ed957-122">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ed957-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="ed957-123">Kiedy należy używać HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ed957-123">When to use HTTP.sys</span></span>

<span data-ttu-id="ed957-124">Sterownik HTTP.sys jest przydatne w przypadku wdrożeń, których należy udostępnić server bezpośrednio do Internetu, nie za pomocą usług IIS.</span><span class="sxs-lookup"><span data-stu-id="ed957-124">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Sterownik HTTP.sys komunikuje się bezpośrednio z Internetem](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="ed957-126">Ponieważ jest zbudowany na Http.Sys, sterownik HTTP.sys nie wymaga zwrotnego serwera proxy dla ochrony przed atakami.</span><span class="sxs-lookup"><span data-stu-id="ed957-126">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="ed957-127">Sterownik Http.Sys jest dojrzała technologia, która chroni przed wiele rodzajów ataków i zapewnia niezawodność, zabezpieczeń i skalowalność serwera sieci web kompletne.</span><span class="sxs-lookup"><span data-stu-id="ed957-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="ed957-128">SAM działa jako odbiornik HTTP u góry pliku Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="ed957-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="ed957-129">Sterownik HTTP.sys jest dobrym rozwiązaniem w przypadku wdrożeń wewnętrznego, gdy będziesz potrzebować funkcja nie jest dostępna w Kestrel, takich jak uwierzytelnianie systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="ed957-129">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![Sterownik HTTP.sys komunikuje się bezpośrednio z sieci wewnętrznej](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="ed957-131">Jak używać HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ed957-131">How to use HTTP.sys</span></span>

<span data-ttu-id="ed957-132">Poniżej przedstawiono omówienie zadań instalacji systemu operacyjnego hosta i aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ed957-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="ed957-133">Konfigurowanie systemu Windows Server</span><span class="sxs-lookup"><span data-stu-id="ed957-133">Configure Windows Server</span></span>

* <span data-ttu-id="ed957-134">Zainstaluj wersję platformy .NET, która wymaga aplikacji, takich jak [.NET Core](https://www.microsoft.com/net/download/core) lub [.NET Framework](https://www.microsoft.com/net/download/framework).</span><span class="sxs-lookup"><span data-stu-id="ed957-134">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="ed957-135">Preregister prefiksów URL do powiązania do pliku HTTP.sys i skonfigurować certyfikaty SSL</span><span class="sxs-lookup"><span data-stu-id="ed957-135">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="ed957-136">Jeśli nie preregister prefiksy adresów URL w systemie Windows, należy do uruchamiania aplikacji z uprawnieniami administratora.</span><span class="sxs-lookup"><span data-stu-id="ed957-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="ed957-137">Jedynym wyjątkiem jest powiązaniu localhost przy użyciu protokołu HTTP (a nie HTTPS) z większą niż 1024; numeru portu w takim przypadku uprawnienia administratora nie są wymagane.</span><span class="sxs-lookup"><span data-stu-id="ed957-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="ed957-138">Aby uzyskać więcej informacji, zobacz [preregister prefiksy i konfigurowanie protokołu SSL](#preregister-url-prefixes-and-configure-ssl) dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="ed957-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="ed957-139">Otwórz porty zapory, aby zezwolić na ruch do pliku HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="ed957-139">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="ed957-140">Można użyć *netsh.exe* lub [poleceń cmdlet programu PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="ed957-140">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="ed957-141">Dostępne są także [ustawienia rejestru Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="ed957-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="ed957-142">Konfigurowanie aplikacji platformy ASP.NET Core używać HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ed957-142">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="ed957-143">Żadna instalacja pakietu jest niezbędny w przypadku używasz [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span><span class="sxs-lookup"><span data-stu-id="ed957-143">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="ed957-144">[Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) pakietu znajduje się w metapackage.</span><span class="sxs-lookup"><span data-stu-id="ed957-144">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="ed957-145">Wywołanie `UseHttpSys` — metoda rozszerzenia na `WebHostBuilder` w Twojej `Main` metoda określania żadnego [opcje HTTP.sys](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) potrzebne, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="ed957-145">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="ed957-146">Skonfiguruj opcje HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ed957-146">Configure HTTP.sys options</span></span>

<span data-ttu-id="ed957-147">Poniżej przedstawiono niektóre ustawienia HTTP.sys i ograniczeń, które można skonfigurować.</span><span class="sxs-lookup"><span data-stu-id="ed957-147">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="ed957-148">**Maksymalna liczba połączeń klientów**</span><span class="sxs-lookup"><span data-stu-id="ed957-148">**Maximum client connections**</span></span>

<span data-ttu-id="ed957-149">Można ustawić maksymalną liczbę równoczesnych otwartych połączeń TCP dla całej aplikacji w następującym kodem *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="ed957-149">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="ed957-150">Maksymalna liczba połączeń jest nieograniczony (null) domyślnie.</span><span class="sxs-lookup"><span data-stu-id="ed957-150">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="ed957-151">**Żądanie maksymalny rozmiar treści**</span><span class="sxs-lookup"><span data-stu-id="ed957-151">**Maximum request body size**</span></span>

<span data-ttu-id="ed957-152">Domyślny rozmiar treści żądania maksymalna jest 30,000,000 bajtów, czyli około 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="ed957-152">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="ed957-153">Zalecanym sposobem zastąpienie limit w aplikacji platformy ASP.NET Core MVC jest użycie [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) atrybutu metody akcji:</span><span class="sxs-lookup"><span data-stu-id="ed957-153">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="ed957-154">Oto przykład, który pokazuje, jak skonfigurować ograniczenia dla całej aplikacji, każde żądanie:</span><span class="sxs-lookup"><span data-stu-id="ed957-154">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="ed957-155">Można zastąpić ustawienie dla określonego żądania w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ed957-155">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="ed957-156">Jeśli próbujesz skonfigurować limit na żądanie, po uruchomieniu aplikacji odczytu żądania, jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="ed957-156">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="ed957-157">Brak `IsReadOnly` właściwość, która informuje, jeśli `MaxRequestBodySize` właściwość jest w stanie tylko do odczytu, co oznacza jest za późno skonfigurować limit.</span><span class="sxs-lookup"><span data-stu-id="ed957-157">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="ed957-158">Aby uzyskać informacje o innych opcjach HTTP.sys, zobacz [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="ed957-158">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="ed957-159">Skonfiguruj adresy URL i portów do nasłuchiwania</span><span class="sxs-lookup"><span data-stu-id="ed957-159">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="ed957-160">Domyślnie wiąże platformy ASP.NET Core `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="ed957-160">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="ed957-161">Aby skonfigurować prefiksy URL i portów, można użyć `UseUrls` — metoda rozszerzenia, `urls` argumentu wiersza polecenia, zmienną środowiskową ASPNETCORE_URLS lub `UrlPrefixes` właściwość [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="ed957-161">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="ed957-162">Poniższy przykład kodu wykorzystuje `UrlPrefixes`.</span><span class="sxs-lookup"><span data-stu-id="ed957-162">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="ed957-163">Zaletą `UrlPrefixes` jest, że zostanie wyświetlony komunikat o błędzie natychmiast Jeśli próbujesz dodać prefiks, który jest sformatowany problem.</span><span class="sxs-lookup"><span data-stu-id="ed957-163">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that is formatted wrong.</span></span> <span data-ttu-id="ed957-164">Zaletą `UseUrls` (udostępniane `urls` i ASPNETCORE_URLS) to, że można łatwo przełączać się między Kestrel i sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="ed957-164">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="ed957-165">Jeśli używasz zarówno `UseUrls` (lub `urls` lub ASPNETCORE_URLS) i `UrlPrefixes`, ustawienia w `UrlPrefixes` przesłonić te w `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="ed957-165">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="ed957-166">Aby uzyskać więcej informacji, zobacz [hostingu](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="ed957-166">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="ed957-167">Korzysta z pliku HTTP.sys [formaty ciągu UrlPrefix interfejsu API serwera HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="ed957-167">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="ed957-168">Upewnij się, że określono tego samego prefiksu ciągi w `UseUrls` lub `UrlPrefixes` który preregister na serwerze.</span><span class="sxs-lookup"><span data-stu-id="ed957-168">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="ed957-169">Nie używaj usług IIS</span><span class="sxs-lookup"><span data-stu-id="ed957-169">Don't use IIS</span></span>

<span data-ttu-id="ed957-170">Upewnij się, że aplikacja nie jest skonfigurowana do uruchamiania usług IIS lub usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="ed957-170">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="ed957-171">W programie Visual Studio domyślnego profilu uruchamiania jest dla usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="ed957-171">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="ed957-172">Aby uruchomić projekt jako aplikacji konsoli, ręcznie zmienić wybranego profilu, jak pokazano na poniższym zrzucie ekranu.</span><span class="sxs-lookup"><span data-stu-id="ed957-172">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![Wybierz profil aplikacji konsoli](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="ed957-174">Preregister prefiksy URL i skonfigurować protokół SSL</span><span class="sxs-lookup"><span data-stu-id="ed957-174">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="ed957-175">Zarówno usług IIS, jak i składnik HTTP.sys polegać na podstawowy sterownik trybu jądra Http.Sys do nasłuchiwania żądań i wstępne przetwarzanie.</span><span class="sxs-lookup"><span data-stu-id="ed957-175">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="ed957-176">W usługach IIS interfejs użytkownika zarządzania umożliwia stosunkowo łatwa do skonfigurowania wszystko.</span><span class="sxs-lookup"><span data-stu-id="ed957-176">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="ed957-177">Jednak należy skonfigurować serwer Http.Sys samodzielnie.</span><span class="sxs-lookup"><span data-stu-id="ed957-177">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="ed957-178">Wbudowane narzędzie do wykonywania, czyli *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="ed957-178">The built-in tool for doing that is *netsh.exe*.</span></span> 

<span data-ttu-id="ed957-179">Z *netsh.exe* można zarezerwować prefiksy URL i przypisać certyfikatów SSL.</span><span class="sxs-lookup"><span data-stu-id="ed957-179">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="ed957-180">Narzędzie wymaga uprawnień administracyjnych.</span><span class="sxs-lookup"><span data-stu-id="ed957-180">The tool requires administrative privileges.</span></span>

<span data-ttu-id="ed957-181">W poniższym przykładzie przedstawiono minimalnej liczbie niezbędnej do zarezerwowania prefiksy URL porty 80 i 443:</span><span class="sxs-lookup"><span data-stu-id="ed957-181">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="ed957-182">Poniższy przykład przedstawia sposób przypisania certyfikatu SSL:</span><span class="sxs-lookup"><span data-stu-id="ed957-182">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="ed957-183">W tym miejscu znajduje się dokumentacja odwołanie *netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="ed957-183">Here is the reference documentation for *netsh.exe*:</span></span>

* [<span data-ttu-id="ed957-184">Polecenia Netsh dla Hypertext Transfer Protocol (HTTP)</span><span class="sxs-lookup"><span data-stu-id="ed957-184">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="ed957-185">Ciągi UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="ed957-185">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="ed957-186">Następujące zasoby zawierają szczegółowe instrukcje dotyczące kilka scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="ed957-186">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="ed957-187">Artykuły, które odwołują się do HttpListener są stosowane jednakowo do pliku HTTP.sys, zgodnie z obu są oparte na pliku Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="ed957-187">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="ed957-188">Instrukcje: konfigurowanie portu z certyfikatem SSL</span><span class="sxs-lookup"><span data-stu-id="ed957-188">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="ed957-189">[Komunikacja HTTPS - HttpListener na podstawie hostingu oraz certyfikatu klienta](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) to jest blogu innych firm i jest dość stary, ale nadal zawiera przydatne informacje.</span><span class="sxs-lookup"><span data-stu-id="ed957-189">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="ed957-190">[Porady: HttpListener za pomocą wskazówki lub serwer Http niezarządzanych kodu (C++) jako serwera SSL proste](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) starsze blogu zawierający przydatne informacje jest zbyt.</span><span class="sxs-lookup"><span data-stu-id="ed957-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>

<span data-ttu-id="ed957-191">Poniżej przedstawiono niektóre narzędzia innych firm, które mogą być łatwiejsze niż w *netsh.exe* wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="ed957-191">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="ed957-192">Nie są one udostępniane przez lub podpisane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ed957-192">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="ed957-193">Narzędzia Uruchom jako administrator domyślnie, ponieważ *netsh.exe* sam wymaga uprawnień administratora.</span><span class="sxs-lookup"><span data-stu-id="ed957-193">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="ed957-194">[Sterownik HTTP.sys Menedżera](http://httpsysmanager.codeplex.com/) udostępnia interfejs do listy i konfigurowania certyfikatów SSL i opcji, zastrzeżenia prefiksu i listy zaufanych certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="ed957-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="ed957-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) pozwala wyświetlić lub skonfigurować certyfikaty SSL i prefiksy URL.</span><span class="sxs-lookup"><span data-stu-id="ed957-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="ed957-196">Interfejs użytkownika jest bardziej precyzyjnych niż http.sys Manager i udostępnia kilka więcej opcji konfiguracji, ale w przeciwnym razie zapewnia funkcje podobne.</span><span class="sxs-lookup"><span data-stu-id="ed957-196">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="ed957-197">Nie można utworzyć nową listę zaufania certyfikatów (CTL), ale można przypisać istniejące.</span><span class="sxs-lookup"><span data-stu-id="ed957-197">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="ed957-198">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="ed957-198">Next steps</span></span>

<span data-ttu-id="ed957-199">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="ed957-199">For more information, see the following resources:</span></span>

* [<span data-ttu-id="ed957-200">Przykładowa aplikacja dla tego artykułu</span><span class="sxs-lookup"><span data-stu-id="ed957-200">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [<span data-ttu-id="ed957-201">Sterownik HTTP.sys kodu źródłowego</span><span class="sxs-lookup"><span data-stu-id="ed957-201">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="ed957-202">Hosting</span><span class="sxs-lookup"><span data-stu-id="ed957-202">Hosting</span></span>](xref:fundamentals/hosting)
