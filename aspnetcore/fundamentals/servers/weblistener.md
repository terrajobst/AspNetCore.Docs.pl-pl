---
title: WebListener implementacja serwera sieci web platformy ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat WebListener, serwer sieci web platformy ASP.NET Core w systemie Windows, który może służyć do bezpośredniego połączenia z Internetem bez usług IIS.
manager: wpickett
ms.author: riande
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 46871edb744ad152df8eb958b344068b7408dd1e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/17/2018
ms.locfileid: "34248456"
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="59683-103">WebListener implementacja serwera sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="59683-103">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="59683-104">Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Roaming Krzysztof](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="59683-104">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="59683-105">Ten temat dotyczy tylko platformy ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="59683-105">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="59683-106">W programie ASP.NET 2.0 Core, nosi nazwę WebListener [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="59683-106">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="59683-107">Jest WebListener [serwera sieci web dla platformy ASP.NET Core](index.md) , na którym działają tylko w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="59683-107">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="59683-108">Jest zbudowany na [sterownik trybu jądra Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="59683-108">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="59683-109">WebListener stanowi alternatywę dla [Kestrel](kestrel.md) który może służyć do bezpośredniego połączenia z Internetem bez polegania na serwerze IIS jako serwera zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="59683-109">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="59683-110">W rzeczywistości **WebListener nie można używać z usług IIS lub usług IIS Express, ponieważ nie jest zgodna z [platformy ASP.NET Core modułu](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="59683-110">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="59683-111">Mimo że WebListener został opracowany dla platformy ASP.NET Core, można bezpośrednio w dowolnej aplikacji .NET Core lub .NET Framework za pomocą [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="59683-111">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="59683-112">WebListener obsługuje następujące funkcje:</span><span class="sxs-lookup"><span data-stu-id="59683-112">WebListener supports the following features:</span></span>

- [<span data-ttu-id="59683-113">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="59683-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="59683-114">Udostępnianie portów</span><span class="sxs-lookup"><span data-stu-id="59683-114">Port sharing</span></span>
- <span data-ttu-id="59683-115">Protokół HTTPS z SNI</span><span class="sxs-lookup"><span data-stu-id="59683-115">HTTPS with SNI</span></span>
- <span data-ttu-id="59683-116">HTTP/2 za pośrednictwem protokołu TLS (z systemem Windows 10)</span><span class="sxs-lookup"><span data-stu-id="59683-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="59683-117">Przekazywanie pliku bezpośredniego</span><span class="sxs-lookup"><span data-stu-id="59683-117">Direct file transmission</span></span>
- <span data-ttu-id="59683-118">Buforowanie odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="59683-118">Response caching</span></span>
- <span data-ttu-id="59683-119">Protokół WebSockets (z systemem Windows 8)</span><span class="sxs-lookup"><span data-stu-id="59683-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="59683-120">Obsługiwane wersje systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="59683-120">Supported Windows versions:</span></span>

- <span data-ttu-id="59683-121">Windows 7 i Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="59683-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="59683-122">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="59683-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="59683-123">Kiedy należy używać WebListener</span><span class="sxs-lookup"><span data-stu-id="59683-123">When to use WebListener</span></span>

<span data-ttu-id="59683-124">WebListener jest przydatne w przypadku wdrożeń, których należy udostępnić server bezpośrednio do Internetu, nie za pomocą usług IIS.</span><span class="sxs-lookup"><span data-stu-id="59683-124">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Weblistener komunikuje się bezpośrednio z Internetem](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="59683-126">Ponieważ jest zbudowany na Http.Sys, WebListener nie wymaga zwrotnego serwera proxy dla ochrony przed atakami.</span><span class="sxs-lookup"><span data-stu-id="59683-126">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="59683-127">Sterownik Http.Sys jest dojrzała technologia, która chroni przed wiele rodzajów ataków i zapewnia niezawodność, zabezpieczeń i skalowalność serwera sieci web kompletne.</span><span class="sxs-lookup"><span data-stu-id="59683-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="59683-128">SAM działa jako odbiornik HTTP u góry pliku Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="59683-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="59683-129">WebListener jest również dobrym rozwiązaniem w przypadku wdrożeń wewnętrznego, gdy będziesz potrzebować funkcje, które zapewnia, że nie można pobrać przy użyciu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="59683-129">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![Weblistener komunikuje się bezpośrednio z sieci wewnętrznej](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="59683-131">Jak używać WebListener</span><span class="sxs-lookup"><span data-stu-id="59683-131">How to use WebListener</span></span>

<span data-ttu-id="59683-132">Poniżej przedstawiono omówienie zadań instalacji systemu operacyjnego hosta i aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="59683-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="59683-133">Konfigurowanie systemu Windows Server</span><span class="sxs-lookup"><span data-stu-id="59683-133">Configure Windows Server</span></span>

* <span data-ttu-id="59683-134">Zainstaluj wersję platformy .NET, która wymaga aplikacji, takich jak [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) lub .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="59683-134">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="59683-135">Preregister prefiksy URL, aby powiązać WebListener i skonfigurować certyfikaty SSL</span><span class="sxs-lookup"><span data-stu-id="59683-135">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="59683-136">Jeśli nie preregister prefiksy adresów URL w systemie Windows, należy do uruchamiania aplikacji z uprawnieniami administratora.</span><span class="sxs-lookup"><span data-stu-id="59683-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="59683-137">Jedynym wyjątkiem jest powiązaniu localhost przy użyciu protokołu HTTP (a nie HTTPS) z większą niż 1024; numeru portu w takim przypadku uprawnienia administratora nie są wymagane.</span><span class="sxs-lookup"><span data-stu-id="59683-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="59683-138">Aby uzyskać więcej informacji, zobacz [preregister prefiksy i konfigurowanie protokołu SSL](#preregister-url-prefixes-and-configure-ssl) dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="59683-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="59683-139">Otwórz porty zapory, aby zezwolić na ruch do osiągnięcia WebListener.</span><span class="sxs-lookup"><span data-stu-id="59683-139">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="59683-140">Można użyć netsh.exe lub [poleceń cmdlet programu PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="59683-140">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="59683-141">Dostępne są także [ustawienia rejestru Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="59683-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="59683-142">Konfigurowanie aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="59683-142">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="59683-143">Zainstaluj pakiet NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="59683-143">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="59683-144">Spowoduje to również zainstalowanie [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) jako zależność.</span><span class="sxs-lookup"><span data-stu-id="59683-144">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="59683-145">Wywołanie `UseWebListener` — metoda rozszerzenia na [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) w Twojej `Main` metody, określając żadnych WebListener [opcje](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) i [ustawienia](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) potrzebne , jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="59683-145">Call the `UseWebListener` extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="59683-146">Skonfiguruj adresy URL i portów do nasłuchiwania</span><span class="sxs-lookup"><span data-stu-id="59683-146">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="59683-147">Domyślnie program ASP.NET Core wiąże `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="59683-147">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="59683-148">Aby skonfigurować prefiksy URL i portów, można użyć `UseURLs` — metoda rozszerzenia, `urls` argumentu wiersza polecenia lub systemu konfiguracji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="59683-148">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="59683-149">Aby uzyskać więcej informacji zobacz [hosta w ASP.NET Core(xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="59683-149">For more information, see [Host in ASP.NET Core(xref:fundamentals/host/index).</span></span>

  <span data-ttu-id="59683-150">Sieci Web używa odbiornika [formaty ciągu prefiksu Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="59683-150">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="59683-151">Nie ma żadnych wymagań formatu ciągu prefiksu, które są specyficzne dla WebListener.</span><span class="sxs-lookup"><span data-stu-id="59683-151">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!WARNING]
  > <span data-ttu-id="59683-152">Powiązania najwyższego poziomu symbolu wieloznacznego (`http://*:80/` i `http://+:80`) powinien **nie** można użyć.</span><span class="sxs-lookup"><span data-stu-id="59683-152">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="59683-153">Powiązania wieloznaczny najwyższego poziomu można otwarcie luk w zabezpieczeniach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="59683-153">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="59683-154">Dotyczy to zarówno silne i słabe symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="59683-154">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="59683-155">Użyj nazwy hostów jawne zamiast symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="59683-155">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="59683-156">Powiązanie symbolu wieloznacznego domeny podrzędnej (na przykład `*.mysub.com`) nie ma to zagrożenie bezpieczeństwa, jeśli kontrolować domeny nadrzędnej całego (w przeciwieństwie do `*.com`, której występuje).</span><span class="sxs-lookup"><span data-stu-id="59683-156">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="59683-157">Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="59683-157">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

  > [!NOTE]
  > <span data-ttu-id="59683-158">Upewnij się, że określono tego samego prefiksu ciągi w `UseUrls` który preregister na serwerze.</span><span class="sxs-lookup"><span data-stu-id="59683-158">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="59683-159">Upewnij się, że aplikacja nie jest skonfigurowana do uruchamiania usług IIS lub usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="59683-159">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="59683-160">W programie Visual Studio domyślnego profilu uruchamiania jest dla usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="59683-160">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="59683-161">Aby uruchomić projekt jako aplikacji konsoli należy ręcznie zmienić wybranego profilu, jak pokazano na poniższym zrzucie ekranu.</span><span class="sxs-lookup"><span data-stu-id="59683-161">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Wybierz profil aplikacji konsoli](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="59683-163">Jak używać WebListener poza platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="59683-163">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="59683-164">Zainstaluj [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="59683-164">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="59683-165">[Preregister prefiksy URL, aby powiązać WebListener i skonfigurować certyfikaty SSL](#preregister-url-prefixes-and-configure-ssl) jak w przypadku użycia w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="59683-165">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="59683-166">Dostępne są także [ustawienia rejestru Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="59683-166">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="59683-167">Oto przykład kodu, który demonstruje używać WebListener poza platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="59683-167">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="59683-168">Preregister prefiksy URL i skonfigurować protokół SSL</span><span class="sxs-lookup"><span data-stu-id="59683-168">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="59683-169">Zarówno usług IIS, jak i WebListener polegać na podstawowy sterownik trybu jądra Http.Sys do nasłuchiwania żądań i wstępne przetwarzanie.</span><span class="sxs-lookup"><span data-stu-id="59683-169">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="59683-170">W usługach IIS interfejs użytkownika zarządzania umożliwia stosunkowo łatwa do skonfigurowania wszystko.</span><span class="sxs-lookup"><span data-stu-id="59683-170">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="59683-171">Jednak jeśli używasz WebListener należy skonfigurować serwer Http.Sys samodzielnie.</span><span class="sxs-lookup"><span data-stu-id="59683-171">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="59683-172">Wbudowane narzędzie robić, który jest netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="59683-172">The built-in tool for doing that's netsh.exe.</span></span> 

<span data-ttu-id="59683-173">Najbardziej typowych zadań, należy użyć netsh.exe dla są rezerwowania prefiksy URL i przypisywanie certyfikatów SSL.</span><span class="sxs-lookup"><span data-stu-id="59683-173">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="59683-174">NetSh.exe nie jest łatwo narzędzia do użycia dla początkujących użytkowników.</span><span class="sxs-lookup"><span data-stu-id="59683-174">NetSh.exe isn't an easy tool to use for beginners.</span></span> <span data-ttu-id="59683-175">W poniższym przykładzie przedstawiono podstawowe czynności potrzebne do zarezerwowania prefiksy URL porty 80 i 443:</span><span class="sxs-lookup"><span data-stu-id="59683-175">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="59683-176">Poniższy przykład przedstawia sposób przypisania certyfikatu SSL:</span><span class="sxs-lookup"><span data-stu-id="59683-176">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="59683-177">Oto oficjalnego dokumentacji:</span><span class="sxs-lookup"><span data-stu-id="59683-177">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="59683-178">Polecenia Netsh dla Hypertext Transfer Protocol (HTTP)</span><span class="sxs-lookup"><span data-stu-id="59683-178">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="59683-179">Ciągi UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="59683-179">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="59683-180">Następujące zasoby zawierają szczegółowe instrukcje dotyczące kilka scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="59683-180">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="59683-181">Artykuły, które odwołują się do `HttpListener` stosowane jednakowo do `WebListener`, ponieważ zarówno są oparte na pliku Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="59683-181">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="59683-182">Instrukcje: konfigurowanie portu z certyfikatem SSL</span><span class="sxs-lookup"><span data-stu-id="59683-182">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="59683-183">[Komunikacja HTTPS - HttpListener na podstawie hostingu oraz certyfikatu klienta](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) to jest blogu innych firm i jest dość stary, ale nadal zawiera przydatne informacje.</span><span class="sxs-lookup"><span data-stu-id="59683-183">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="59683-184">[Porady: HttpListener za pomocą wskazówki lub serwer Http niezarządzanych kodu (C++) jako serwera SSL proste](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) starsze blogu zawierający przydatne informacje jest zbyt.</span><span class="sxs-lookup"><span data-stu-id="59683-184">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="59683-185">Jak skonfigurować WebListener Core .NET, przy użyciu protokołu SSL?</span><span class="sxs-lookup"><span data-stu-id="59683-185">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="59683-186">Poniżej przedstawiono niektóre narzędzia innych firm, które mogą być łatwiejsze do użycia niż netsh.exe wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="59683-186">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="59683-187">Nie są one udostępniane przez lub podpisane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="59683-187">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="59683-188">Narzędzia Uruchom jako administrator domyślnie, ponieważ netsh.exe sam wymaga uprawnień administratora.</span><span class="sxs-lookup"><span data-stu-id="59683-188">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="59683-189">[Sterownik HTTP.sys Menedżera](http://httpsysmanager.codeplex.com/) udostępnia interfejs do listy i konfigurowania certyfikatów SSL i opcji, zastrzeżenia prefiksu i listy zaufanych certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="59683-189">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="59683-190">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) pozwala wyświetlić lub skonfigurować certyfikaty SSL i prefiksy URL.</span><span class="sxs-lookup"><span data-stu-id="59683-190">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="59683-191">Interfejs użytkownika jest bardziej precyzyjnych niż http.sys Manager i udostępnia kilka więcej opcji konfiguracji, ale w przeciwnym razie zapewnia funkcje podobne.</span><span class="sxs-lookup"><span data-stu-id="59683-191">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="59683-192">Nie można utworzyć nową listę zaufania certyfikatów (CTL), ale można przypisać istniejące.</span><span class="sxs-lookup"><span data-stu-id="59683-192">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="59683-193">Podczas generowania certyfikatów SSL z podpisem własnym, firma Microsoft udostępnia narzędzia wiersza polecenia: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) i polecenia cmdlet programu PowerShell [SelfSignedCertificate nowy](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="59683-193">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="59683-194">Dostępne są również narzędzia interfejsu użytkownika innych firm, które ułatwiają wygenerować certyfikaty SSL z podpisem własnym:</span><span class="sxs-lookup"><span data-stu-id="59683-194">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="59683-195">SelfCert</span><span class="sxs-lookup"><span data-stu-id="59683-195">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="59683-196">MakeCert interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="59683-196">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="59683-197">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="59683-197">Next steps</span></span>

<span data-ttu-id="59683-198">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="59683-198">For more information, see the following resources:</span></span>

* [<span data-ttu-id="59683-199">Przykładowa aplikacja dla tego artykułu</span><span class="sxs-lookup"><span data-stu-id="59683-199">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="59683-200">Kod źródłowy WebListener</span><span class="sxs-lookup"><span data-stu-id="59683-200">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="59683-201">Hosting</span><span class="sxs-lookup"><span data-stu-id="59683-201">Hosting</span></span>](xref:fundamentals/host/index)
