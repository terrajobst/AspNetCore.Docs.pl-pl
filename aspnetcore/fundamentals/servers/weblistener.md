---
title: WebListener implementacja serwera sieci web platformy ASP.NET Core
author: rick-anderson
description: "Wprowadza WebListener, serwer sieci web platformy ASP.NET Core w systemie Windows. W oparciu sterownik trybu jądra Http.Sys, WebListener stanowi alternatywę dla Kestrel, który może służyć do bezpośredniego połączenia z Internetem bez usług IIS."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 5073a1663ec99a1b161092d74ab035ee9782becd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="8b97d-104">WebListener implementacja serwera sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b97d-104">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="8b97d-105">Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Roaming Krzysztof](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="8b97d-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="8b97d-106">Ten temat dotyczy tylko platformy ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="8b97d-106">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="8b97d-107">W programie ASP.NET 2.0 Core, nosi nazwę WebListener [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="8b97d-107">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="8b97d-108">Jest WebListener [serwera sieci web dla platformy ASP.NET Core](index.md) , na którym działają tylko w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="8b97d-108">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="8b97d-109">Jest zbudowany na [sterownik trybu jądra Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="8b97d-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="8b97d-110">WebListener stanowi alternatywę dla [Kestrel](kestrel.md) który może służyć do bezpośredniego połączenia z Internetem bez polegania na serwerze IIS jako serwera zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="8b97d-110">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="8b97d-111">W rzeczywistości **WebListener nie można używać z usług IIS lub usług IIS Express, ponieważ nie jest zgodna z [platformy ASP.NET Core modułu](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="8b97d-111">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="8b97d-112">Mimo że WebListener został opracowany dla platformy ASP.NET Core, można bezpośrednio w dowolnej aplikacji .NET Core lub .NET Framework za pomocą [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="8b97d-112">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="8b97d-113">WebListener obsługuje następujące funkcje:</span><span class="sxs-lookup"><span data-stu-id="8b97d-113">WebListener supports the following features:</span></span>

- [<span data-ttu-id="8b97d-114">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="8b97d-114">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="8b97d-115">Udostępnianie portów</span><span class="sxs-lookup"><span data-stu-id="8b97d-115">Port sharing</span></span>
- <span data-ttu-id="8b97d-116">Protokół HTTPS z SNI</span><span class="sxs-lookup"><span data-stu-id="8b97d-116">HTTPS with SNI</span></span>
- <span data-ttu-id="8b97d-117">HTTP/2 za pośrednictwem protokołu TLS (z systemem Windows 10)</span><span class="sxs-lookup"><span data-stu-id="8b97d-117">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="8b97d-118">Przekazywanie pliku bezpośredniego</span><span class="sxs-lookup"><span data-stu-id="8b97d-118">Direct file transmission</span></span>
- <span data-ttu-id="8b97d-119">Buforowanie odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="8b97d-119">Response caching</span></span>
- <span data-ttu-id="8b97d-120">Protokół WebSockets (z systemem Windows 8)</span><span class="sxs-lookup"><span data-stu-id="8b97d-120">WebSockets (Windows 8)</span></span>

<span data-ttu-id="8b97d-121">Obsługiwane wersje systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="8b97d-121">Supported Windows versions:</span></span>

- <span data-ttu-id="8b97d-122">Windows 7 i Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="8b97d-122">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="8b97d-123">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8b97d-123">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="8b97d-124">Kiedy należy używać WebListener</span><span class="sxs-lookup"><span data-stu-id="8b97d-124">When to use WebListener</span></span>

<span data-ttu-id="8b97d-125">WebListener jest przydatne w przypadku wdrożeń, których należy udostępnić server bezpośrednio do Internetu, nie za pomocą usług IIS.</span><span class="sxs-lookup"><span data-stu-id="8b97d-125">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Weblistener komunikuje się bezpośrednio z Internetem](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="8b97d-127">Ponieważ jest zbudowany na Http.Sys, WebListener nie wymaga zwrotnego serwera proxy dla ochrony przed atakami.</span><span class="sxs-lookup"><span data-stu-id="8b97d-127">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="8b97d-128">Sterownik Http.Sys jest dojrzała technologia, która chroni przed wiele rodzajów ataków i zapewnia niezawodność, zabezpieczeń i skalowalność serwera sieci web kompletne.</span><span class="sxs-lookup"><span data-stu-id="8b97d-128">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="8b97d-129">SAM działa jako odbiornik HTTP u góry pliku Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="8b97d-129">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="8b97d-130">WebListener jest również dobrym rozwiązaniem w przypadku wdrożeń wewnętrznego, gdy będziesz potrzebować funkcje, które zapewnia, że nie można pobrać przy użyciu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="8b97d-130">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![Weblistener komunikuje się bezpośrednio z sieci wewnętrznej](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="8b97d-132">Jak używać WebListener</span><span class="sxs-lookup"><span data-stu-id="8b97d-132">How to use WebListener</span></span>

<span data-ttu-id="8b97d-133">Poniżej przedstawiono omówienie zadań instalacji systemu operacyjnego hosta i aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8b97d-133">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="8b97d-134">Konfigurowanie systemu Windows Server</span><span class="sxs-lookup"><span data-stu-id="8b97d-134">Configure Windows Server</span></span>

* <span data-ttu-id="8b97d-135">Zainstaluj wersję platformy .NET, która wymaga aplikacji, takich jak [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) lub .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="8b97d-135">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="8b97d-136">Preregister prefiksy URL, aby powiązać WebListener i skonfigurować certyfikaty SSL</span><span class="sxs-lookup"><span data-stu-id="8b97d-136">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="8b97d-137">Jeśli nie preregister prefiksy adresów URL w systemie Windows, należy do uruchamiania aplikacji z uprawnieniami administratora.</span><span class="sxs-lookup"><span data-stu-id="8b97d-137">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="8b97d-138">Jedynym wyjątkiem jest powiązaniu localhost przy użyciu protokołu HTTP (a nie HTTPS) z większą niż 1024; numeru portu w takim przypadku uprawnienia administratora nie są wymagane.</span><span class="sxs-lookup"><span data-stu-id="8b97d-138">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="8b97d-139">Aby uzyskać więcej informacji, zobacz [preregister prefiksy i konfigurowanie protokołu SSL](#preregister-url-prefixes-and-configure-ssl) dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="8b97d-139">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="8b97d-140">Otwórz porty zapory, aby zezwolić na ruch do osiągnięcia WebListener.</span><span class="sxs-lookup"><span data-stu-id="8b97d-140">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="8b97d-141">Można użyć netsh.exe lub [poleceń cmdlet programu PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="8b97d-141">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="8b97d-142">Dostępne są także [ustawienia rejestru Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="8b97d-142">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="8b97d-143">Konfigurowanie aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b97d-143">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="8b97d-144">Zainstaluj pakiet NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="8b97d-144">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="8b97d-145">Spowoduje to również zainstalowanie [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) jako zależność.</span><span class="sxs-lookup"><span data-stu-id="8b97d-145">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="8b97d-146">Wywołanie `UseWebListener` — metoda rozszerzenia na [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) w Twojej `Main` metody, określając żadnych WebListener [opcje](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) i [ustawienia](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) potrzebne , jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="8b97d-146">Call the `UseWebListener` extension method on [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="8b97d-147">Skonfiguruj adresy URL i portów do nasłuchiwania</span><span class="sxs-lookup"><span data-stu-id="8b97d-147">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="8b97d-148">Domyślnie wiąże platformy ASP.NET Core `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="8b97d-148">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="8b97d-149">Aby skonfigurować prefiksy URL i portów, można użyć `UseURLs` — metoda rozszerzenia, `urls` argumentu wiersza polecenia lub systemu konfiguracji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8b97d-149">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="8b97d-150">Aby uzyskać więcej informacji, zobacz [hostingu](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="8b97d-150">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="8b97d-151">Sieci Web używa odbiornika [formaty ciągu prefiksu Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="8b97d-151">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="8b97d-152">Nie ma żadnych wymagań formatu ciągu prefiksu, które są specyficzne dla WebListener.</span><span class="sxs-lookup"><span data-stu-id="8b97d-152">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!NOTE]
  > <span data-ttu-id="8b97d-153">Upewnij się, że określono tego samego prefiksu ciągi w `UseUrls` który preregister na serwerze.</span><span class="sxs-lookup"><span data-stu-id="8b97d-153">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="8b97d-154">Upewnij się, że aplikacja nie jest skonfigurowana do uruchamiania usług IIS lub usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="8b97d-154">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="8b97d-155">W programie Visual Studio domyślnego profilu uruchamiania jest dla usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="8b97d-155">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="8b97d-156">Aby uruchomić projekt jako aplikacji konsoli należy ręcznie zmienić wybranego profilu, jak pokazano na poniższym zrzucie ekranu.</span><span class="sxs-lookup"><span data-stu-id="8b97d-156">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Wybierz profil aplikacji konsoli](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="8b97d-158">Jak używać WebListener poza platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b97d-158">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="8b97d-159">Zainstaluj [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="8b97d-159">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="8b97d-160">[Preregister prefiksy URL, aby powiązać WebListener i skonfigurować certyfikaty SSL](#preregister-url-prefixes-and-configure-ssl) jak w przypadku użycia w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8b97d-160">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="8b97d-161">Dostępne są także [ustawienia rejestru Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="8b97d-161">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="8b97d-162">Oto przykład kodu, który demonstruje używać WebListener poza platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="8b97d-162">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

```csharp
var settings = new WebListenerSettings();
settings.UrlPrefixes.Add("http://localhost:8080");

using (WebListener listener = new WebListener(settings))
{
    listener.Start();

    while (true)
    {
        var context = await listener.AcceptAsync();
        byte[] bytes = Encoding.ASCII.GetBytes("Hello World: " + DateTime.Now);
        context.Response.ContentLength = bytes.Length;
        context.Response.ContentType = "text/plain";

        await context.Response.Body.WriteAsync(bytes, 0, bytes.Length);
        context.Dispose();
    }
}
```

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="8b97d-163">Preregister prefiksy URL i skonfigurować protokół SSL</span><span class="sxs-lookup"><span data-stu-id="8b97d-163">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="8b97d-164">Zarówno usług IIS, jak i WebListener polegać na podstawowy sterownik trybu jądra Http.Sys do nasłuchiwania żądań i wstępne przetwarzanie.</span><span class="sxs-lookup"><span data-stu-id="8b97d-164">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="8b97d-165">W usługach IIS interfejs użytkownika zarządzania umożliwia stosunkowo łatwa do skonfigurowania wszystko.</span><span class="sxs-lookup"><span data-stu-id="8b97d-165">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="8b97d-166">Jednak jeśli używasz WebListener należy skonfigurować serwer Http.Sys samodzielnie.</span><span class="sxs-lookup"><span data-stu-id="8b97d-166">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="8b97d-167">Wbudowane narzędzie robić, który jest netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="8b97d-167">The built-in tool for doing that's netsh.exe.</span></span> 

<span data-ttu-id="8b97d-168">Najbardziej typowych zadań, należy użyć netsh.exe dla są rezerwowania prefiksy URL i przypisywanie certyfikatów SSL.</span><span class="sxs-lookup"><span data-stu-id="8b97d-168">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="8b97d-169">NetSh.exe nie jest łatwo narzędzia do użycia dla początkujących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="8b97d-169">NetSh.exe isn't an easy tool to use for beginners.</span></span> <span data-ttu-id="8b97d-170">W poniższym przykładzie przedstawiono podstawowe czynności potrzebne do zarezerwowania prefiksy URL porty 80 i 443:</span><span class="sxs-lookup"><span data-stu-id="8b97d-170">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="8b97d-171">Poniższy przykład przedstawia sposób przypisania certyfikatu SSL:</span><span class="sxs-lookup"><span data-stu-id="8b97d-171">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="8b97d-172">Oto oficjalnego dokumentacji:</span><span class="sxs-lookup"><span data-stu-id="8b97d-172">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="8b97d-173">Polecenia Netsh dla Hypertext Transfer Protocol (HTTP)</span><span class="sxs-lookup"><span data-stu-id="8b97d-173">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="8b97d-174">Ciągi UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="8b97d-174">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="8b97d-175">Następujące zasoby zawierają szczegółowe instrukcje dotyczące kilka scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="8b97d-175">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="8b97d-176">Artykuły, które odwołują się do `HttpListener` stosowane jednakowo do `WebListener`, ponieważ zarówno są oparte na pliku Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="8b97d-176">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="8b97d-177">Instrukcje: konfigurowanie portu z certyfikatem SSL</span><span class="sxs-lookup"><span data-stu-id="8b97d-177">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="8b97d-178">[Komunikacja HTTPS - HttpListener na podstawie hostingu oraz certyfikatu klienta](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) to jest blogu innych firm i jest dość stary, ale nadal zawiera przydatne informacje.</span><span class="sxs-lookup"><span data-stu-id="8b97d-178">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="8b97d-179">[Porady: HttpListener za pomocą wskazówki lub serwer Http niezarządzanych kodu (C++) jako serwera SSL proste](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) starsze blogu zawierający przydatne informacje jest zbyt.</span><span class="sxs-lookup"><span data-stu-id="8b97d-179">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="8b97d-180">Jak skonfigurować WebListener Core .NET, przy użyciu protokołu SSL?</span><span class="sxs-lookup"><span data-stu-id="8b97d-180">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="8b97d-181">Poniżej przedstawiono niektóre narzędzia innych firm, które mogą być łatwiejsze do użycia niż netsh.exe wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="8b97d-181">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="8b97d-182">Nie są one udostępniane przez lub podpisane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8b97d-182">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="8b97d-183">Narzędzia Uruchom jako administrator domyślnie, ponieważ netsh.exe sam wymaga uprawnień administratora.</span><span class="sxs-lookup"><span data-stu-id="8b97d-183">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="8b97d-184">[Sterownik HTTP.sys Menedżera](http://httpsysmanager.codeplex.com/) udostępnia interfejs do listy i konfigurowania certyfikatów SSL i opcji, zastrzeżenia prefiksu i listy zaufanych certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="8b97d-184">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="8b97d-185">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) pozwala wyświetlić lub skonfigurować certyfikaty SSL i prefiksy URL.</span><span class="sxs-lookup"><span data-stu-id="8b97d-185">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="8b97d-186">Interfejs użytkownika jest bardziej precyzyjnych niż http.sys Manager i udostępnia kilka więcej opcji konfiguracji, ale w przeciwnym razie zapewnia funkcje podobne.</span><span class="sxs-lookup"><span data-stu-id="8b97d-186">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="8b97d-187">Nie można utworzyć nową listę zaufania certyfikatów (CTL), ale można przypisać istniejące.</span><span class="sxs-lookup"><span data-stu-id="8b97d-187">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="8b97d-188">Podczas generowania certyfikatów SSL z podpisem własnym, firma Microsoft udostępnia narzędzia wiersza polecenia: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) i polecenia cmdlet programu PowerShell [SelfSignedCertificate nowy](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="8b97d-188">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="8b97d-189">Dostępne są również narzędzia interfejsu użytkownika innych firm, które ułatwiają wygenerować certyfikaty SSL z podpisem własnym:</span><span class="sxs-lookup"><span data-stu-id="8b97d-189">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="8b97d-190">SelfCert</span><span class="sxs-lookup"><span data-stu-id="8b97d-190">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="8b97d-191">MakeCert interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="8b97d-191">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="8b97d-192">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="8b97d-192">Next steps</span></span>

<span data-ttu-id="8b97d-193">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="8b97d-193">For more information, see the following resources:</span></span>

* [<span data-ttu-id="8b97d-194">Przykładowa aplikacja dla tego artykułu</span><span class="sxs-lookup"><span data-stu-id="8b97d-194">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="8b97d-195">Kod źródłowy WebListener</span><span class="sxs-lookup"><span data-stu-id="8b97d-195">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="8b97d-196">Hosting</span><span class="sxs-lookup"><span data-stu-id="8b97d-196">Hosting</span></span>](../hosting.md)
