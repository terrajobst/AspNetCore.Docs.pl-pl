---
title: Implementacja serwera sieci web WebListener w programie ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat WebListener, serwer sieci web platformy ASP.NET Core na Windows, który może służyć do bezpośredniego połączenia z Internetu bez usług IIS.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 5d72672cc48243f8ee17df615e3379143ed868f6
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206445"
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="c098b-103">Implementacja serwera sieci web WebListener w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c098b-103">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="c098b-104">Przez [Tom Dykstra](https://github.com/tdykstra) i [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="c098b-104">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="c098b-105">Ten temat dotyczy tylko programu ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="c098b-105">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="c098b-106">W programie ASP.NET Core 2.0 nosi nazwę WebListener [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="c098b-106">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="c098b-107">Jest WebListener [serwera sieci web dla platformy ASP.NET Core](index.md) , które jest uruchamiane tylko na Windows.</span><span class="sxs-lookup"><span data-stu-id="c098b-107">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="c098b-108">Jest on oparty na [sterownik trybu jądra Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="c098b-108">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="c098b-109">WebListener stanowi alternatywę [Kestrel](kestrel.md) mogą służyć do bezpośredniego połączenia z Internetem bez polegania na serwerze IIS jako serwera zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="c098b-109">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="c098b-110">W rzeczywistości **WebListener nie można używać z usług IIS Express, lub usług IIS, ponieważ nie jest zgodny z [modułu ASP.NET Core](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="c098b-110">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="c098b-111">Mimo że WebListener została opracowana dla platformy ASP.NET Core, można bezpośrednio w dowolnej aplikacji platformy .NET Core lub .NET Framework za pomocą [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="c098b-111">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="c098b-112">WebListener obsługuje następujące funkcje:</span><span class="sxs-lookup"><span data-stu-id="c098b-112">WebListener supports the following features:</span></span>

- [<span data-ttu-id="c098b-113">Uwierzytelnianie Windows</span><span class="sxs-lookup"><span data-stu-id="c098b-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="c098b-114">Współużytkowanie portów</span><span class="sxs-lookup"><span data-stu-id="c098b-114">Port sharing</span></span>
- <span data-ttu-id="c098b-115">Protokołu HTTPS z SNI</span><span class="sxs-lookup"><span data-stu-id="c098b-115">HTTPS with SNI</span></span>
- <span data-ttu-id="c098b-116">Protokołu HTTP/2 za pośrednictwem protokołu TLS (system Windows 10)</span><span class="sxs-lookup"><span data-stu-id="c098b-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="c098b-117">Przekazywanie pliku bezpośrednie</span><span class="sxs-lookup"><span data-stu-id="c098b-117">Direct file transmission</span></span>
- <span data-ttu-id="c098b-118">Buforowanie odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="c098b-118">Response caching</span></span>
- <span data-ttu-id="c098b-119">Gniazda Websocket (system Windows 8)</span><span class="sxs-lookup"><span data-stu-id="c098b-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="c098b-120">Obsługiwane wersje Windows:</span><span class="sxs-lookup"><span data-stu-id="c098b-120">Supported Windows versions:</span></span>

- <span data-ttu-id="c098b-121">Windows 7 i Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="c098b-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="c098b-122">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c098b-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="c098b-123">Kiedy należy używać WebListener</span><span class="sxs-lookup"><span data-stu-id="c098b-123">When to use WebListener</span></span>

<span data-ttu-id="c098b-124">WebListener jest przydatne w przypadku wdrożeń, w których należy udostępnić server bezpośrednio do Internetu, nie za pomocą usług IIS.</span><span class="sxs-lookup"><span data-stu-id="c098b-124">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Weblistener komunikuje się bezpośrednio z Internetu](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="c098b-126">Ponieważ jest on oparty na Http.Sys, WebListener nie wymaga zwrotnego serwera proxy w celu ochrony przed atakami.</span><span class="sxs-lookup"><span data-stu-id="c098b-126">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="c098b-127">Sterownik Http.Sys to dojrzała technologia, która chroni przed wiele rodzajów ataków i udostępnia niezawodności, bezpieczeństwa i skalowalności serwera sieci web w pełni funkcjonalne.</span><span class="sxs-lookup"><span data-stu-id="c098b-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="c098b-128">SAM działa jako odbiornik HTTP w pliku HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="c098b-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span>

<span data-ttu-id="c098b-129">WebListener jest również dobrym wyborem dla wewnętrznego wdrożeń, gdy będą potrzebne funkcje, które zapewnia, że nie można dostać się przy użyciu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c098b-129">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![Weblistener komunikuje się bezpośrednio z sieci wewnętrznej](weblistener/_static/weblistener-to-internal.png)

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="c098b-131">Uwierzytelnianie trybu jądra przy użyciu protokołu Kerberos</span><span class="sxs-lookup"><span data-stu-id="c098b-131">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="c098b-132">WebListener delegatów, aby uwierzytelnianie trybu jądra za pomocą protokołu uwierzytelniania Kerberos.</span><span class="sxs-lookup"><span data-stu-id="c098b-132">WebListener delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="c098b-133">Uwierzytelnianie w trybie użytkownika nie jest obsługiwana przy użyciu protokołu Kerberos i WebListener.</span><span class="sxs-lookup"><span data-stu-id="c098b-133">User mode authentication isn't supported with Kerberos and WebListener.</span></span> <span data-ttu-id="c098b-134">Konto komputera należy używany do odszyfrowywania tokenu/biletu Kerberos uzyskany z usługi Active Directory i przesyłany dalej przez klienta do serwera w celu uwierzytelnienia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c098b-134">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="c098b-135">Rejestrowanie głównej nazwy usługi (SPN) dla hosta, a nie użytkownika aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c098b-135">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-weblistener"></a><span data-ttu-id="c098b-136">Jak używać WebListener</span><span class="sxs-lookup"><span data-stu-id="c098b-136">How to use WebListener</span></span>

<span data-ttu-id="c098b-137">Poniżej przedstawiono omówienie zadań instalacji systemu operacyjnego hosta i aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c098b-137">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="c098b-138">Konfigurowanie systemu Windows Server</span><span class="sxs-lookup"><span data-stu-id="c098b-138">Configure Windows Server</span></span>

* <span data-ttu-id="c098b-139">Zainstaluj wersję platformy .NET, wymaganych przez aplikację, takie jak [platformy .NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) lub .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="c098b-139">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="c098b-140">Preregister przedrostki adresów URL, aby powiązać WebListener i konfigurowanie certyfikatów protokołu SSL</span><span class="sxs-lookup"><span data-stu-id="c098b-140">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="c098b-141">Nie preregister przedrostki adresów URL w Windows, musisz uruchomić aplikację z uprawnieniami administratora.</span><span class="sxs-lookup"><span data-stu-id="c098b-141">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="c098b-142">Jedynym wyjątkiem jest, jeżeli powiążesz do hosta lokalnego przy użyciu protokołu HTTP (a nie HTTPS), numer portu jest większa niż 1024; w takim przypadku uprawnienia administratora nie są wymagane.</span><span class="sxs-lookup"><span data-stu-id="c098b-142">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="c098b-143">Aby uzyskać więcej informacji, zobacz [preregister prefiksy i konfigurowanie protokołu SSL](#preregister-url-prefixes-and-configure-ssl) w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="c098b-143">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="c098b-144">Otwórz porty zapory muszą zezwalać na ruch do osiągnięcia WebListener.</span><span class="sxs-lookup"><span data-stu-id="c098b-144">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="c098b-145">Możesz użyć netsh.exe lub [poleceń cmdlet programu PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="c098b-145">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="c098b-146">Dostępne są także [ustawienia rejestru Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="c098b-146">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="c098b-147">Konfigurowanie aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c098b-147">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="c098b-148">Zainstaluj pakiet NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="c098b-148">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="c098b-149">Spowoduje to również zainstalowanie [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) jako zależność.</span><span class="sxs-lookup"><span data-stu-id="c098b-149">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="c098b-150">Wywołanie `UseWebListener` metody rozszerzenia w [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) w Twojej `Main` metody, określając wszystkie WebListener [opcje](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) i [ustawienia](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) potrzebne , jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="c098b-150">Call the `UseWebListener` extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="c098b-151">Konfigurowanie adresów URL i portów do nasłuchiwania</span><span class="sxs-lookup"><span data-stu-id="c098b-151">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="c098b-152">Domyślnie platforma ASP.NET Core wiąże `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c098b-152">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="c098b-153">Aby skonfigurować przedrostki adresów URL i portów, można użyć `UseURLs` metody rozszerzenia `urls` argument wiersza polecenia lub systemu konfiguracji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c098b-153">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="c098b-154">Aby uzyskać więcej informacji zobacz [hosta w ASP.NET Core(xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="c098b-154">For more information, see [Host in ASP.NET Core(xref:fundamentals/host/index).</span></span>

  <span data-ttu-id="c098b-155">Sieci Web używa odbiornika [formaty ciągu prefiks Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="c098b-155">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="c098b-156">Nie ma żadnych wymagań formatu ciągu prefiksu, które są specyficzne dla WebListener.</span><span class="sxs-lookup"><span data-stu-id="c098b-156">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!WARNING]
  > <span data-ttu-id="c098b-157">Powiązania najwyższego poziomu symbolu wieloznacznego (`http://*:80/` i `http://+:80`) powinien **nie** można użyć.</span><span class="sxs-lookup"><span data-stu-id="c098b-157">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="c098b-158">Powiązania najwyższego poziomu symboli wieloznacznych otworzyć aplikację w celu luk w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="c098b-158">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="c098b-159">Dotyczy to silnych i słabych symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="c098b-159">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="c098b-160">Użyj nazwy hostów jawne, a nie symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="c098b-160">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="c098b-161">Powiązanie symbol wieloznaczny domeny podrzędnej (na przykład `*.mysub.com`) nie ma to zagrożenie bezpieczeństwa, jeśli możesz kontrolować domenę nadrzędną całego (w przeciwieństwie do `*.com`, który jest narażony).</span><span class="sxs-lookup"><span data-stu-id="c098b-161">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="c098b-162">Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="c098b-162">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

  > [!NOTE]
  > <span data-ttu-id="c098b-163">Upewnij się, że ten sam prefiks ciągi w `UseUrls` , preregister na serwerze.</span><span class="sxs-lookup"><span data-stu-id="c098b-163">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="c098b-164">Upewnij się, że aplikacja nie jest skonfigurowana do uruchamiania usług IIS lub IIS Express.</span><span class="sxs-lookup"><span data-stu-id="c098b-164">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="c098b-165">W programie Visual Studio domyślnym profilem uruchamiania jest dla usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="c098b-165">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="c098b-166">Aby uruchomić projekt jako aplikację konsolową w języku należy ręcznie zmienić wybranego profilu, jak pokazano na poniższym zrzucie ekranu.</span><span class="sxs-lookup"><span data-stu-id="c098b-166">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Wybierz profil aplikacji konsoli](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="c098b-168">Jak używać WebListener poza platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c098b-168">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="c098b-169">Zainstaluj [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="c098b-169">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="c098b-170">[Przedrostki adresów URL, aby powiązać WebListener i skonfigurować certyfikaty SSL preregister](#preregister-url-prefixes-and-configure-ssl) podobnie jak w przypadku użycia w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c098b-170">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="c098b-171">Dostępne są także [ustawienia rejestru Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="c098b-171">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="c098b-172">Poniżej przedstawiono przykładowy kod, który demonstruje użycie WebListener poza platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="c098b-172">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="c098b-173">Preregister przedrostki adresów URL i konfigurowanie protokołu SSL</span><span class="sxs-lookup"><span data-stu-id="c098b-173">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="c098b-174">Usługi IIS i WebListener polegają na podstawowy sterownik trybu jądra Http.Sys do nasłuchiwania żądań i wstępne przetwarzanie.</span><span class="sxs-lookup"><span data-stu-id="c098b-174">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="c098b-175">W usługach IIS interfejs użytkownika zarządzania zapewnia stosunkowo prosty sposób skonfigurować wszystko.</span><span class="sxs-lookup"><span data-stu-id="c098b-175">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="c098b-176">Jednak jeśli używasz WebListener należy skonfigurować serwer HTTP.sys tak, samodzielnie.</span><span class="sxs-lookup"><span data-stu-id="c098b-176">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="c098b-177">Wbudowane narzędzie robić, który jest netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="c098b-177">The built-in tool for doing that's netsh.exe.</span></span> 

<span data-ttu-id="c098b-178">Najbardziej typowych zadań, należy użyć netsh.exe dla są rezerwowanie prefiksów adresu URL i przypisywania certyfikatów SSL.</span><span class="sxs-lookup"><span data-stu-id="c098b-178">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="c098b-179">NetSh.exe nie jest to proste narzędzie do użycia dla początkujących.</span><span class="sxs-lookup"><span data-stu-id="c098b-179">NetSh.exe isn't an easy tool to use for beginners.</span></span> <span data-ttu-id="c098b-180">Poniższy przykład przedstawia bez minimalnej liczbie niezbędnej do zarezerwowania przedrostki adresów URL dla portów 80 i 443:</span><span class="sxs-lookup"><span data-stu-id="c098b-180">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="c098b-181">Poniższy przykład pokazuje, jak przypisać certyfikat SSL:</span><span class="sxs-lookup"><span data-stu-id="c098b-181">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="c098b-182">Oto oficjalna dokumentacja:</span><span class="sxs-lookup"><span data-stu-id="c098b-182">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="c098b-183">Polecenia Netsh dla Hypertext Transfer Protocol (HTTP)</span><span class="sxs-lookup"><span data-stu-id="c098b-183">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="c098b-184">Ciągi UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="c098b-184">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="c098b-185">Następujące zasoby zawierają szczegółowe instrukcje dla kilku scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="c098b-185">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="c098b-186">Artykuły, które odwołują się do `HttpListener` jednakowo do zastosowania `WebListener`, ponieważ także są oparte na Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="c098b-186">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="c098b-187">Instrukcje: konfigurowanie portu z certyfikatem SSL</span><span class="sxs-lookup"><span data-stu-id="c098b-187">How to: Configure a Port with an SSL Certificate</span></span>](/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="c098b-188">[Komunikacja HTTPS - HttpListener i opartych na Hosting certyfikatu klienta](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) to jest blogu innych firm i jest dość stara, ale nadal zawiera przydatne informacje.</span><span class="sxs-lookup"><span data-stu-id="c098b-188">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="c098b-189">[Instrukcje: HttpListener za pomocą wskazówki lub serwer Http niezarządzanego kodu (C++) jako serwera prostego protokołu SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) to zbyt blogu starszy przydatnymi informacjami.</span><span class="sxs-lookup"><span data-stu-id="c098b-189">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="c098b-190">Jak skonfigurować WebListener podstawowych platformy .NET, przy użyciu protokołu SSL?</span><span class="sxs-lookup"><span data-stu-id="c098b-190">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="c098b-191">Poniżej przedstawiono niektóre narzędzia innych producentów, które mogą być łatwiejszy w obsłudze niż netsh.exe wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="c098b-191">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="c098b-192">Nie są one udostępniane przez lub zatwierdzone przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c098b-192">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="c098b-193">Narzędzia Uruchom jako administrator domyślnie, ponieważ netsh.exe sam wymaga uprawnień administratora.</span><span class="sxs-lookup"><span data-stu-id="c098b-193">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="c098b-194">[Sterownik HTTP.sys Menedżera](http://httpsysmanager.codeplex.com/) udostępnia interfejs wielokrotnego użytku listy i konfigurowania certyfikatów SSL i opcji, rezerwacje prefiksu i listy zaufanych certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="c098b-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="c098b-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) pozwala wyświetlić lub skonfigurować certyfikaty SSL i przedrostki adresów URL.</span><span class="sxs-lookup"><span data-stu-id="c098b-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="c098b-196">Interfejs użytkownika jest bardziej precyzyjnych niż http.sys Manager i udostępnia kilka więcej opcji konfiguracji, ale w przeciwnym razie zapewnia podobne funkcje.</span><span class="sxs-lookup"><span data-stu-id="c098b-196">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="c098b-197">Nie można utworzyć nowej listy zaufania certyfikatów (CTL), ale można przypisać już istniejące.</span><span class="sxs-lookup"><span data-stu-id="c098b-197">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="c098b-198">Podczas generowania certyfikatów SSL z podpisem własnym, firma Microsoft udostępnia narzędzia wiersza polecenia: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) i polecenia cmdlet programu PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="c098b-198">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="c098b-199">Dostępne są również narzędzia interfejsu użytkownika innych firm, które ułatwiają wygenerować certyfikaty SSL z podpisem własnym:</span><span class="sxs-lookup"><span data-stu-id="c098b-199">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="c098b-200">SelfCert</span><span class="sxs-lookup"><span data-stu-id="c098b-200">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="c098b-201">Interfejs użytkownika narzędzia MakeCert</span><span class="sxs-lookup"><span data-stu-id="c098b-201">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="c098b-202">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="c098b-202">Next steps</span></span>

<span data-ttu-id="c098b-203">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="c098b-203">For more information, see the following resources:</span></span>

* [<span data-ttu-id="c098b-204">Przykładowa aplikacja w tym artykule</span><span class="sxs-lookup"><span data-stu-id="c098b-204">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="c098b-205">Kod źródłowy WebListener</span><span class="sxs-lookup"><span data-stu-id="c098b-205">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="c098b-206">Hosting</span><span class="sxs-lookup"><span data-stu-id="c098b-206">Hosting</span></span>](xref:fundamentals/host/index)
