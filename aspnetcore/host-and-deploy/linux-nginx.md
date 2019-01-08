---
title: Host platformy ASP.NET Core w systemie Linux przy użyciu serwera Nginx
author: rick-anderson
description: Dowiedz się, jak skonfigurować serwer Nginx jako zwrotny serwer proxy w systemie Ubuntu 16.04 do przesyłania ruchu HTTP do aplikacji sieci web ASP.NET Core uruchomionych na Kestrel.
ms.author: riande
ms.custom: mvc
ms.date: 12/20/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 24973e7bedcb219ac411948db8aa27d7219eac31
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099289"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="97988-103">Host platformy ASP.NET Core w systemie Linux przy użyciu serwera Nginx</span><span class="sxs-lookup"><span data-stu-id="97988-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="97988-104">Przez [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="97988-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="97988-105">W tym przewodniku wyjaśniono Konfigurowanie środowiska ASP.NET Core gotowe do produkcji na serwerze z systemem Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="97988-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="97988-106">Te instrukcje, które mogą działać z nowszymi wersjami systemu Ubuntu, ale instrukcje nie zostały przetestowane z nowszymi wersjami.</span><span class="sxs-lookup"><span data-stu-id="97988-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

<span data-ttu-id="97988-107">Aby uzyskać informacje na inne dystrybucje systemu Linux obsługiwane przez program ASP.NET Core, zobacz [wymagania wstępne dla platformy .NET Core w systemie Linux](/dotnet/core/linux-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="97988-107">For information on other Linux distributions supported by ASP.NET Core, see [Prerequisites for .NET Core on Linux](/dotnet/core/linux-prerequisites).</span></span>

> [!NOTE]
> <span data-ttu-id="97988-108">Aby uzyskać Ubuntu 14.04 *supervisord* jest zalecane jako rozwiązanie do monitorowania procesu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="97988-108">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="97988-109">*systemd* nie jest dostępna w systemie Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="97988-109">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="97988-110">Ubuntu 14.04 instrukcje można znaleźć [poprzednią wersję tego tematu](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="97988-110">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="97988-111">Ten przewodnik:</span><span class="sxs-lookup"><span data-stu-id="97988-111">This guide:</span></span>

* <span data-ttu-id="97988-112">Umieszcza istniejącej aplikacji platformy ASP.NET Core za serwerem proxy odwrotnej.</span><span class="sxs-lookup"><span data-stu-id="97988-112">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="97988-113">Konfiguruje zwrotnego serwera proxy do przekazywania żądań do serwera sieci web Kestrel.</span><span class="sxs-lookup"><span data-stu-id="97988-113">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="97988-114">Gwarantuje, że aplikacja sieci web jest uruchamiana podczas uruchamiania jako demon.</span><span class="sxs-lookup"><span data-stu-id="97988-114">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="97988-115">Konfiguruje narzędzia do zarządzania procesami, aby pomóc, uruchom ponownie aplikację internetową.</span><span class="sxs-lookup"><span data-stu-id="97988-115">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="97988-116">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="97988-116">Prerequisites</span></span>

1. <span data-ttu-id="97988-117">Dostęp do serwera Ubuntu 16.04 przy użyciu konta użytkownika standardowego przy użyciu uprawnień "sudo".</span><span class="sxs-lookup"><span data-stu-id="97988-117">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="97988-118">Na serwerze, należy zainstalować środowisko uruchomieniowe platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="97988-118">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="97988-119">Odwiedź stronę [.NET Core wszystkie strony plików do pobrania](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="97988-119">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="97988-120">Z listy w obszarze wybierz najnowsze środowisko uruchomieniowe — wersja zapoznawcza **środowiska uruchomieniowego**.</span><span class="sxs-lookup"><span data-stu-id="97988-120">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="97988-121">Wybierz, a następnie postępuj zgodnie z instrukcjami dotyczącymi Ubuntu, które są zgodne z wersją systemu Ubuntu server.</span><span class="sxs-lookup"><span data-stu-id="97988-121">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="97988-122">Istniejącą aplikację ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="97988-122">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="97988-123">Publikowanie i skopiuj aplikacji</span><span class="sxs-lookup"><span data-stu-id="97988-123">Publish and copy over the app</span></span>

<span data-ttu-id="97988-124">Konfigurowanie aplikacji na potrzeby [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="97988-124">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="97988-125">Uruchom [publikowania dotnet](/dotnet/core/tools/dotnet-publish) ze środowiska projektowego, aby utworzyć pakiet aplikacji do katalogu (na przykład *bin/wydawania/&lt;target_framework_moniker&gt;/ publish*), można Uruchom na serwerze:</span><span class="sxs-lookup"><span data-stu-id="97988-125">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="97988-126">Aplikację można także publikować jako [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd) Jeśli wolisz nie zachować środowisko uruchomieniowe platformy .NET Core na serwerze.</span><span class="sxs-lookup"><span data-stu-id="97988-126">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="97988-127">Skopiuj aplikacji ASP.NET Core na serwer przy użyciu narzędzia, która integruje się z przepływu pracy w organizacji, (na przykład punkt połączenia usługi, SFTP).</span><span class="sxs-lookup"><span data-stu-id="97988-127">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="97988-128">Często do lokalizowania aplikacji sieci web w obszarze *var* katalog (na przykład *www/var/helloapp*).</span><span class="sxs-lookup"><span data-stu-id="97988-128">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="97988-129">W przypadku wdrożenia produkcyjnego przepływu pracy ciągłej integracji działa publikowania aplikacji i kopiowanie zasobów do serwera.</span><span class="sxs-lookup"><span data-stu-id="97988-129">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="97988-130">Testowanie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="97988-130">Test the app:</span></span>

1. <span data-ttu-id="97988-131">W wierszu polecenia Uruchom aplikację: `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="97988-131">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="97988-132">W przeglądarce przejdź do `http://<serveraddress>:<port>` Aby sprawdzić, aplikacja działa lokalnie w systemie Linux.</span><span class="sxs-lookup"><span data-stu-id="97988-132">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="97988-133">Konfigurowanie zwrotnego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="97988-133">Configure a reverse proxy server</span></span>

<span data-ttu-id="97988-134">Zwrotny serwer proxy jest wspólne dla aplikacji sieci web dynamicznego obsługująca.</span><span class="sxs-lookup"><span data-stu-id="97988-134">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="97988-135">Zwrotny serwer proxy kończy żądanie HTTP i przekazuje je do aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="97988-135">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="97988-136">Użyj serwera proxy odwrotnej</span><span class="sxs-lookup"><span data-stu-id="97988-136">Use a reverse proxy server</span></span>

<span data-ttu-id="97988-137">Kestrel nadaje się doskonale dla obsługujących zawartość dynamiczną z platformą ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="97988-137">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="97988-138">Jednak możliwości usług sieci web nie są jako wyposażonym jako serwerów, takich jak usługi IIS, Apache i Nginx.</span><span class="sxs-lookup"><span data-stu-id="97988-138">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="97988-139">Serwer proxy odwrotnej można odciążyć pracy, takich jak obsługująca zawartość statyczną, buforowanie żądań kompresowania żądań i zakończenia połączenia HTTPS z serwera HTTP.</span><span class="sxs-lookup"><span data-stu-id="97988-139">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and HTTPS termination from the HTTP server.</span></span> <span data-ttu-id="97988-140">Zwrotnego serwera proxy mogą znajdować się na dedykowanym komputerze lub można wdrażać wraz z serwerem HTTP.</span><span class="sxs-lookup"><span data-stu-id="97988-140">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="97988-141">Na potrzeby tego przewodnika pojedyncze wystąpienie serwera Nginx jest używany.</span><span class="sxs-lookup"><span data-stu-id="97988-141">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="97988-142">Działa na tym samym serwerze, wraz z serwerem HTTP.</span><span class="sxs-lookup"><span data-stu-id="97988-142">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="97988-143">Na podstawie wymagań, można wybrać różne ustawienia.</span><span class="sxs-lookup"><span data-stu-id="97988-143">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="97988-144">Ponieważ żądania są przekazywane przez zwrotny serwer proxy, należy użyć [przekazywane oprogramowania pośredniczącego nagłówki](xref:host-and-deploy/proxy-load-balancer) z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="97988-144">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="97988-145">Aktualizacje oprogramowania pośredniczącego `Request.Scheme`przy użyciu `X-Forwarded-Proto` nagłówka, więc działanie tego identyfikatory URI przekierowań i innych zasad zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="97988-145">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="97988-146">Dowolny składnik, który jest zależny od systemu, takie jak uwierzytelnianie, generowanie konsolidacji, przekierowań i geolokalizacja, muszą być umieszczone po wywołaniu oprogramowanie pośredniczące przekazane nagłówków.</span><span class="sxs-lookup"><span data-stu-id="97988-146">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="97988-147">Zgodnie z ogólną zasadą przekazywane oprogramowania pośredniczącego nagłówki należy uruchomić przed innym oprogramowaniu pośredniczącym, z wyjątkiem diagnostyki i obsługi oprogramowania pośredniczącego błędów.</span><span class="sxs-lookup"><span data-stu-id="97988-147">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="97988-148">Ta kolejność gwarantuje, że oprogramowanie pośredniczące, opierając się na nagłówki przekazywane informacje mogą wykorzystywać wartości nagłówka do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="97988-148">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="97988-149">Wywoływanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in Class metoda `Startup.Configure` przed wywołaniem [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) lub podobne oprogramowanie pośredniczące schematu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="97988-149">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="97988-150">Konfigurowanie oprogramowania pośredniczącego, aby przekazywać `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków:</span><span class="sxs-lookup"><span data-stu-id="97988-150">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="97988-151">Wywoływanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in Class metoda `Startup.Configure` przed wywołaniem [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) i [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) lub podobne schematu uwierzytelniania oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="97988-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="97988-152">Konfigurowanie oprogramowania pośredniczącego, aby przekazywać `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków:</span><span class="sxs-lookup"><span data-stu-id="97988-152">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

<span data-ttu-id="97988-153">Jeśli nie [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) są określone oprogramowanie pośredniczące, są domyślne nagłówki do przekazywania `None`.</span><span class="sxs-lookup"><span data-stu-id="97988-153">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="97988-154">Tylko serwery proxy uruchomiona na hoście lokalnym (127.0.0.1, [:: 1]) są zaufane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="97988-154">Only proxies running on localhost (127.0.0.1, [::1]) are trusted by default.</span></span> <span data-ttu-id="97988-155">Jeśli innych zaufanych serwerów proxy lub sieci w obrębie organizacji uchwyt żądania między Internetem a serwer sieci web Dodaj je do listy <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> lub <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> z <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span><span class="sxs-lookup"><span data-stu-id="97988-155">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="97988-156">Poniższy przykład dodaje serwer proxy zaufanych pod adresem IP 10.0.0.100 z oprogramowaniem pośredniczącym nagłówki przekazywane `KnownProxies` w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="97988-156">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="97988-157">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="97988-157">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-nginx"></a><span data-ttu-id="97988-158">Zainstalować rozwiązanie Nginx</span><span class="sxs-lookup"><span data-stu-id="97988-158">Install Nginx</span></span>

<span data-ttu-id="97988-159">Użyj `apt-get` do zainstalowania serwera Nginx.</span><span class="sxs-lookup"><span data-stu-id="97988-159">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="97988-160">Instalator tworzy *systemd* skryptu init, który uruchamia serwer Nginx jako demon przy uruchamianiu systemu.</span><span class="sxs-lookup"><span data-stu-id="97988-160">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="97988-161">Postępuj zgodnie z instrukcjami instalacji dla systemu Ubuntu na [Nginx: Oficjalna pakietów systemu Debian/Ubuntu](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span><span class="sxs-lookup"><span data-stu-id="97988-161">Follow the installation instructions for Ubuntu at [Nginx: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="97988-162">Jeśli wymagane są opcjonalne modułów serwera Nginx, może być wymagane tworzenia Nginx ze źródła.</span><span class="sxs-lookup"><span data-stu-id="97988-162">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="97988-163">Po zainstalowaniu serwera Nginx po raz pierwszy, jawnie uruchom ją, uruchamiając:</span><span class="sxs-lookup"><span data-stu-id="97988-163">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="97988-164">Sprawdź, czy przeglądarka wyświetla wartość domyślna strona docelowa dla kontenera Nginx.</span><span class="sxs-lookup"><span data-stu-id="97988-164">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="97988-165">Strona docelowa jest dostępny na `http://<server_IP_address>/index.nginx-debian.html`.</span><span class="sxs-lookup"><span data-stu-id="97988-165">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="97988-166">Konfigurowanie serwera Nginx</span><span class="sxs-lookup"><span data-stu-id="97988-166">Configure Nginx</span></span>

<span data-ttu-id="97988-167">Aby skonfigurować serwer Nginx jako zwrotny serwer proxy, aby przekazywać żądania do aplikacji platformy ASP.NET Core, zmodyfikuj */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="97988-167">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="97988-168">Otwórz go w edytorze tekstów i Zastąp zawartość następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="97988-168">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

<span data-ttu-id="97988-169">Gdy nie `server_name` dopasowania, Nginx korzysta z domyślnego serwera.</span><span class="sxs-lookup"><span data-stu-id="97988-169">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="97988-170">Jeśli żaden serwer domyślny jest zdefiniowany, pierwszy serwer w pliku konfiguracji jest domyślny serwer.</span><span class="sxs-lookup"><span data-stu-id="97988-170">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="97988-171">Najlepszym rozwiązaniem jest dodanie określonych domyślnego serwera, która zwraca kod stanu 444 w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="97988-171">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="97988-172">Przedstawiono przykładową konfigurację serwera domyślnego:</span><span class="sxs-lookup"><span data-stu-id="97988-172">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="97988-173">Za pomocą poprzedniego pliku i domyślnego serwera konfiguracji Nginx akceptuje publicznych ruch na porcie 80 z nagłówkiem hosta `example.com` lub `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="97988-173">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="97988-174">Żądania, które nie pasują te hosty nie uzyskać przekazywane do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="97988-174">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="97988-175">Serwer Nginx przekazuje żądania pasujących do Kestrel w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="97988-175">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="97988-176">Zobacz [przetwarzaniu żądania przez serwer nginx](https://nginx.org/docs/http/request_processing.html) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="97988-176">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span> <span data-ttu-id="97988-177">Aby zmienić port adresu IP firmy Kestrel, zobacz [Kestrel: Konfiguracja punktu końcowego](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="97988-177">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="97988-178">Nie można określić poprawną [dyrektywy nazwa_serwera](https://nginx.org/docs/http/server_names.html) ujawnia luki w zabezpieczeniach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="97988-178">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="97988-179">Powiązanie symbol wieloznaczny domeny podrzędnej (na przykład `*.example.com`) nie stanowić to zagrożenie bezpieczeństwa, jeśli możesz kontrolować domenę nadrzędną całego (w przeciwieństwie do `*.com`, który jest narażony).</span><span class="sxs-lookup"><span data-stu-id="97988-179">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="97988-180">Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="97988-180">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="97988-181">Po ustanowieniu konfigurację serwera Nginx, uruchom `sudo nginx -t` Aby sprawdzić składnię plików konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="97988-181">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="97988-182">Jeśli test konfiguracji w pliku zakończy się pomyślnie, wymusić Nginx, aby wczytać zmiany, uruchamiając `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="97988-182">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="97988-183">Aby bezpośrednio uruchamiać aplikację na serwerze:</span><span class="sxs-lookup"><span data-stu-id="97988-183">To directly run the app on the server:</span></span>

1. <span data-ttu-id="97988-184">Przejdź do katalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="97988-184">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="97988-185">Uruchom aplikację: `dotnet <app_assembly.dll>`, gdzie `app_assembly.dll` jest nazwą pliku zestawu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="97988-185">Run the app: `dotnet <app_assembly.dll>`, where `app_assembly.dll` is the assembly file name of the app.</span></span>

<span data-ttu-id="97988-186">Jeśli aplikacja działa na serwerze, ale nie odpowiada za pośrednictwem Internetu, sprawdź ustawienia zapory serwera i upewnij się, że port 80 jest otwarty.</span><span class="sxs-lookup"><span data-stu-id="97988-186">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="97988-187">Jeśli używasz systemu Ubuntu Maszynie wirtualnej platformy Azure, Dodaj regułę sieciowej grupy zabezpieczeń (NSG), która włącza port przychodzący ruch 80.</span><span class="sxs-lookup"><span data-stu-id="97988-187">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="97988-188">Nie ma potrzeby można włączyć reguły ruchu wychodzącego portu 80, jak ruch wychodzący jest udzielany automatycznie po włączeniu reguły dla ruchu przychodzącego.</span><span class="sxs-lookup"><span data-stu-id="97988-188">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="97988-189">Po zakończeniu testowania aplikacji, zamknij aplikację za pomocą `Ctrl+C` w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="97988-189">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitor-the-app"></a><span data-ttu-id="97988-190">Monitorowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="97988-190">Monitor the app</span></span>

<span data-ttu-id="97988-191">Serwer jest skonfigurowany do przekazywania żądań kierowanych do `http://<serveraddress>:80` się do aplikacji platformy ASP.NET Core uruchomionych na Kestrel na `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="97988-191">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="97988-192">Jednak serwer Nginx nie jest skonfigurować do zarządzania procesem Kestrel.</span><span class="sxs-lookup"><span data-stu-id="97988-192">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="97988-193">*systemd* może służyć do tworzenia pliku usługi, aby uruchomić i monitorować podstawowej aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="97988-193">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="97988-194">*systemd* to system init, który zapewnia wiele funkcji zaawansowanych uruchamianie, zatrzymywanie oraz zarządzanie procesami.</span><span class="sxs-lookup"><span data-stu-id="97988-194">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="97988-195">Utwórz plik usługi</span><span class="sxs-lookup"><span data-stu-id="97988-195">Create the service file</span></span>

<span data-ttu-id="97988-196">Tworzenie pliku definicji usługi:</span><span class="sxs-lookup"><span data-stu-id="97988-196">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="97988-197">Poniżej przedstawiono przykładowy plik usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="97988-197">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/www/helloapp
ExecStart=/usr/bin/dotnet /var/www/helloapp/helloapp.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="97988-198">Jeśli użytkownik *danych www* nie jest używany przez tę konfigurację użytkownika zdefiniowane w tym miejscu należy najpierw utworzyć i podane odpowiednie prawa własności plików.</span><span class="sxs-lookup"><span data-stu-id="97988-198">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="97988-199">Użyj `TimeoutStopSec` skonfigurować czas oczekiwania na aplikację, aby zamknięty po odebraniu sygnału przerwania początkowej.</span><span class="sxs-lookup"><span data-stu-id="97988-199">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="97988-200">Jeśli aplikacja nie zamknięty w tym okresie, aby zakończyć aplikację zgłaszany jest SIGKILL.</span><span class="sxs-lookup"><span data-stu-id="97988-200">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="97988-201">Podaj wartość jako unitless sekund (na przykład `150`), czas span wartości (na przykład `2min 30s`), lub `infinity` wyłączyć limit czasu.</span><span class="sxs-lookup"><span data-stu-id="97988-201">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="97988-202">`TimeoutStopSec` Wartość domyślna to wartość `DefaultTimeoutStopSec` w pliku konfiguracji Menedżera (*systemd system.conf*, *system.conf.d*, *systemd user.conf*,  *User.conf.d*).</span><span class="sxs-lookup"><span data-stu-id="97988-202">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="97988-203">Domyślna wartość limitu czasu dla większości dystrybucji wynosi 90 s.</span><span class="sxs-lookup"><span data-stu-id="97988-203">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="97988-204">System plików rozróżniana wielkość liter jest systemu Linux.</span><span class="sxs-lookup"><span data-stu-id="97988-204">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="97988-205">Ustawienie ASPNETCORE_ENVIRONMENT "Produkcyjne" wyniki wyszukiwania dla pliku konfiguracji w *appsettings. Production.JSON*, a nie *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="97988-205">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="97988-206">Niektóre wartości (na przykład parametry połączenia SQL), należy użyć znaków ucieczki dla dostawców konfiguracji można odczytać zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="97988-206">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="97988-207">Użyj następującego polecenia do generowania prawidłowo o zmienionym znaczeniu wartości do użycia w pliku konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="97988-207">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

<span data-ttu-id="97988-208">Zapisz plik i włączyć usługę.</span><span class="sxs-lookup"><span data-stu-id="97988-208">Save the file and enable the service.</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="97988-209">Uruchom usługę i sprawdź, czy jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="97988-209">Start the service and verify that it's running.</span></span>

```
sudo systemctl start kestrel-helloapp.service
sudo systemctl status kestrel-helloapp.service

● kestrel-helloapp.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-helloapp.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-helloapp.service
            └─9021 /usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
```

<span data-ttu-id="97988-210">Zwrotny serwer proxy, skonfigurowane i Kestrel zarządzane za pośrednictwem systemd, aplikacji sieci web jest w pełni skonfigurowane i dostępne za pomocą przeglądarki na komputerze lokalnym w `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="97988-210">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="97988-211">Jest także dostępny z komputera zdalnego, wyłączając wszelkie zapory, która może blokować.</span><span class="sxs-lookup"><span data-stu-id="97988-211">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="97988-212">Inspekcja nagłówki odpowiedzi `Server` nagłówka przedstawiono aplikację ASP.NET Core, obsługiwanej przez Kestrel.</span><span class="sxs-lookup"><span data-stu-id="97988-212">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="view-logs"></a><span data-ttu-id="97988-213">Wyświetlanie dzienników</span><span class="sxs-lookup"><span data-stu-id="97988-213">View logs</span></span>

<span data-ttu-id="97988-214">Ponieważ aplikacja sieci web przy użyciu Kestrel odbywa się przy użyciu `systemd`, scentralizowane dziennika są rejestrowane wszystkie zdarzenia i procesów.</span><span class="sxs-lookup"><span data-stu-id="97988-214">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="97988-215">Jednak ten dziennik zawiera wszystkie wpisy dla wszystkich usług i procesów, które zarządza `systemd`.</span><span class="sxs-lookup"><span data-stu-id="97988-215">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="97988-216">Aby wyświetlić `kestrel-helloapp.service`— określone elementy, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="97988-216">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="97988-217">Do dalszego filtrowania, opcje czasu takie jak `--since today`, `--until 1 hour ago` lub kombinację tych można zmniejszyć liczbę zwróconych pozycji.</span><span class="sxs-lookup"><span data-stu-id="97988-217">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="97988-218">Ochrona danych</span><span class="sxs-lookup"><span data-stu-id="97988-218">Data protection</span></span>

<span data-ttu-id="97988-219">[Stosu ochrony danych programu ASP.NET Core](xref:security/data-protection/introduction) jest używana przez kilka platformy ASP.NET Core [middlewares](xref:fundamentals/middleware/index), w tym oprogramowania pośredniczącego uwierzytelniania (na przykład, oprogramowaniu pośredniczącym pliku cookie) i fałszerstwo żądania międzywitrynowego (CSRF) zabezpieczenia.</span><span class="sxs-lookup"><span data-stu-id="97988-219">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="97988-220">Nawet wtedy, gdy interfejsów API ochrony danych nie są wywoływane przez kod użytkownika, ochrony danych należy skonfigurować tak, aby utworzyć trwałe kryptograficznych [magazynu kluczy](xref:security/data-protection/implementation/key-management).</span><span class="sxs-lookup"><span data-stu-id="97988-220">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="97988-221">Jeśli nie jest skonfigurowana ochrona danych, klucze są przechowywane w pamięci i odrzucone po ponownym uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="97988-221">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="97988-222">Jeśli pierścień klucz jest przechowywany w pamięci, po ponownym uruchomieniu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="97988-222">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="97988-223">Wszystkie tokeny na podstawie plików cookie uwierzytelniania są unieważniane.</span><span class="sxs-lookup"><span data-stu-id="97988-223">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="97988-224">Użytkownicy muszą ponownie zaloguj się na ich następnego żądania.</span><span class="sxs-lookup"><span data-stu-id="97988-224">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="97988-225">Wszystkie dane chronione za pomocą pierścień klucz może już nie mogły być odszyfrowane.</span><span class="sxs-lookup"><span data-stu-id="97988-225">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="97988-226">Może to obejmować [tokenów CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) i [plików cookie programu ASP.NET Core MVC TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="97988-226">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="97988-227">Aby skonfigurować ochronę danych na zostaną zachowane, a pierścień klucz szyfrowania, zobacz:</span><span class="sxs-lookup"><span data-stu-id="97988-227">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="long-request-header-fields"></a><span data-ttu-id="97988-228">Pola nagłówka długiego żądania</span><span class="sxs-lookup"><span data-stu-id="97988-228">Long request header fields</span></span>

<span data-ttu-id="97988-229">Jeśli aplikacja wymaga żądanie dłużej, niż dozwolone przez serwer proxy pola nagłówka domyślne ustawienia (zwykle 4K lub 8K, w zależności od platformy), następujące dyrektywy wymaga dostosowania.</span><span class="sxs-lookup"><span data-stu-id="97988-229">If the app requires request header fields longer than permitted by the proxy server's default settings (typically 4K or 8K depending on the platform), the following directives require adjustment.</span></span> <span data-ttu-id="97988-230">Wartości do zastosowania są zależne od scenariusza.</span><span class="sxs-lookup"><span data-stu-id="97988-230">The values to apply are scenario-dependent.</span></span> <span data-ttu-id="97988-231">Aby uzyskać więcej informacji można znaleźć w temacie serwera dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="97988-231">For more information, see your server's documentation.</span></span>

* [<span data-ttu-id="97988-232">proxy_buffer_size</span><span class="sxs-lookup"><span data-stu-id="97988-232">proxy_buffer_size</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_buffer_size)
* [<span data-ttu-id="97988-233">proxy_buffers</span><span class="sxs-lookup"><span data-stu-id="97988-233">proxy_buffers</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_buffers)
* [<span data-ttu-id="97988-234">proxy_busy_buffers_size</span><span class="sxs-lookup"><span data-stu-id="97988-234">proxy_busy_buffers_size</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_busy_buffers_size)
* [<span data-ttu-id="97988-235">large_client_header_buffers</span><span class="sxs-lookup"><span data-stu-id="97988-235">large_client_header_buffers</span></span>](https://nginx.org/docs/http/ngx_http_core_module.html#large_client_header_buffers)

> [!WARNING]
> <span data-ttu-id="97988-236">Nie narastał do wartości domyślnych buforów proxy konieczne.</span><span class="sxs-lookup"><span data-stu-id="97988-236">Don't increase the default values of proxy buffers unless necessary.</span></span> <span data-ttu-id="97988-237">Zwiększenie wartości te zwiększa ryzyko przepełnienia buforu (przepełnienie), a następnie przeprowadzenie ataku typu "odmowa usługi" (DoS) ataków złośliwych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="97988-237">Increasing these values increases the risk of buffer overrun (overflow) and Denial of Service (DoS) attacks by malicious users.</span></span>

## <a name="secure-the-app"></a><span data-ttu-id="97988-238">Zabezpieczanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="97988-238">Secure the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="97988-239">Włącz AppArmor</span><span class="sxs-lookup"><span data-stu-id="97988-239">Enable AppArmor</span></span>

<span data-ttu-id="97988-240">Linux zabezpieczeń modułów (LSM) tak, to struktura, która jest częścią jądra systemu Linux od systemu Linux w wersji 2.6.</span><span class="sxs-lookup"><span data-stu-id="97988-240">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="97988-241">LSM obsługuje różne implementacje modułach zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="97988-241">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="97988-242">[AppArmor](https://wiki.ubuntu.com/AppArmor) jest LSM, implementujący systemu obowiązkowe kontroli dostępu, który umożliwia ograniczenie program ma ograniczony zestaw zasobów.</span><span class="sxs-lookup"><span data-stu-id="97988-242">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="97988-243">Upewnij się, AppArmor jest włączone i skonfigurowane prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="97988-243">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configure-the-firewall"></a><span data-ttu-id="97988-244">Konfigurowanie zapory</span><span class="sxs-lookup"><span data-stu-id="97988-244">Configure the firewall</span></span>

<span data-ttu-id="97988-245">Zamknij wyłączanie wszystkich portów zewnętrznych, które nie są używane.</span><span class="sxs-lookup"><span data-stu-id="97988-245">Close off all external ports that are not in use.</span></span> <span data-ttu-id="97988-246">Zapora prostotę (ufw) zawiera frontonu na potrzeby `iptables` , zapewniając interfejs wiersza polecenia dla konfiguracji zapory.</span><span class="sxs-lookup"><span data-stu-id="97988-246">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span>

> [!WARNING]
> <span data-ttu-id="97988-247">Zapora uniemożliwi dostęp do całego systemu Jeśli nie zostały skonfigurowane prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="97988-247">A firewall will prevent access to the whole system if not configured correctly.</span></span> <span data-ttu-id="97988-248">Nie można określić prawidłowy port SSH będzie efektywnie zablokowania systemu korzystania z protokołu SSH do nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="97988-248">Failure to specify the correct SSH port will effectively lock you out of the system if you are using SSH to connect to it.</span></span> <span data-ttu-id="97988-249">Domyślny port to 22.</span><span class="sxs-lookup"><span data-stu-id="97988-249">The default port is 22.</span></span> <span data-ttu-id="97988-250">Aby uzyskać więcej informacji, zobacz [wprowadzenie do ufw](https://help.ubuntu.com/community/UFW) i [ręczne](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span><span class="sxs-lookup"><span data-stu-id="97988-250">For more information, see the [introduction to ufw](https://help.ubuntu.com/community/UFW) and the [manual](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span></span>

<span data-ttu-id="97988-251">Zainstaluj `ufw` i skonfiguruj ją tak, aby zezwolić na ruch na portach, wszelkie potrzebne.</span><span class="sxs-lookup"><span data-stu-id="97988-251">Install `ufw` and configure it to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

sudo ufw enable
```

### <a name="secure-nginx"></a><span data-ttu-id="97988-252">Zabezpieczenia serwera Nginx</span><span class="sxs-lookup"><span data-stu-id="97988-252">Secure Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="97988-253">Zmień nazwę odpowiedzi serwera Nginx</span><span class="sxs-lookup"><span data-stu-id="97988-253">Change the Nginx response name</span></span>

<span data-ttu-id="97988-254">Edit *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="97988-254">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="97988-255">Skonfiguruj opcje</span><span class="sxs-lookup"><span data-stu-id="97988-255">Configure options</span></span>

<span data-ttu-id="97988-256">Konfigurowanie serwera przy użyciu dodatkowych wymaganych modułów.</span><span class="sxs-lookup"><span data-stu-id="97988-256">Configure the server with additional required modules.</span></span> <span data-ttu-id="97988-257">Należy wziąć pod uwagę przy użyciu zapory aplikacji sieci web, takich jak [zapory ModSecurity](https://www.modsecurity.org/), w celu ograniczenia funkcjonalności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="97988-257">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="https-configuration"></a><span data-ttu-id="97988-258">Konfiguracja protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="97988-258">HTTPS configuration</span></span>

* <span data-ttu-id="97988-259">Konfigurowanie serwera do nasłuchiwania ruchu HTTPS na porcie `443` , określając prawidłowy certyfikat wystawiony przez zaufany urząd certyfikacji (CA).</span><span class="sxs-lookup"><span data-stu-id="97988-259">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="97988-260">Wzmocnić zabezpieczenia przez wprowadzenie niektórych rozwiązań przedstawiony w następującym */etc/nginx/nginx.conf* pliku.</span><span class="sxs-lookup"><span data-stu-id="97988-260">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="97988-261">Przykłady obejmują wybranie silniejszego szyfrowania i przekierowania całego ruchu za pośrednictwem protokołu HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="97988-261">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="97988-262">Dodawanie `HTTP Strict-Transport-Security` nagłówka (HSTS) zapewnia, wszystkie kolejne żądania wysłane przez klienta za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="97988-262">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS.</span></span>

* <span data-ttu-id="97988-263">Nie należy dodawać nagłówek HSTS lub wybrać odpowiednią `max-age` Jeśli protokół HTTPS zostanie wyłączona w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="97988-263">Don't add the HSTS header or chose an appropriate `max-age` if HTTPS will be disabled in the future.</span></span>

<span data-ttu-id="97988-264">Dodaj */etc/nginx/proxy.conf* pliku konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="97988-264">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="97988-265">Edytuj */etc/nginx/nginx.conf* pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="97988-265">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="97988-266">Przykład zawiera zarówno `http` i `server` sekcji w pliku w jednej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="97988-266">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="97988-267">Zabezpieczenia serwera Nginx z porywaniu kliknięć</span><span class="sxs-lookup"><span data-stu-id="97988-267">Secure Nginx from clickjacking</span></span>

<span data-ttu-id="97988-268">[Porywaniu kliknięć](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), znane również jako *interfejsu użytkownika odszkodowania ataku*, jest złośliwymi atakami, gdzie zwiódł już obiekt odwiedzający witrynę sieci Web do kliknięcia łącza lub przycisku na innej stronie nie są one obecnie odwiedzający.</span><span class="sxs-lookup"><span data-stu-id="97988-268">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="97988-269">Użyj `X-FRAME-OPTIONS` na zabezpieczenie witryny.</span><span class="sxs-lookup"><span data-stu-id="97988-269">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="97988-270">Aby uniknąć porywaniu kliknięć ataków:</span><span class="sxs-lookup"><span data-stu-id="97988-270">To mitigate clickjacking attacks:</span></span>

1. <span data-ttu-id="97988-271">Edytuj *nginx.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="97988-271">Edit the *nginx.conf* file:</span></span>

   ```bash
   sudo nano /etc/nginx/nginx.conf
   ```

   <span data-ttu-id="97988-272">Dodaj wiersz `add_header X-Frame-Options "SAMEORIGIN";`.</span><span class="sxs-lookup"><span data-stu-id="97988-272">Add the line `add_header X-Frame-Options "SAMEORIGIN";`.</span></span>
1. <span data-ttu-id="97988-273">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="97988-273">Save the file.</span></span>
1. <span data-ttu-id="97988-274">Uruchom ponownie serwer Nginx.</span><span class="sxs-lookup"><span data-stu-id="97988-274">Restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="97988-275">Wykrywanie typu MIME</span><span class="sxs-lookup"><span data-stu-id="97988-275">MIME-type sniffing</span></span>

<span data-ttu-id="97988-276">Tego pliku nagłówkowego zapobiega w większości przeglądarek z wykrywanie MIME odpowiedzi od deklarowanej typu zawartości jako nagłówek powoduje, że przeglądarka nie, aby zastąpić typ zawartości odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="97988-276">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="97988-277">Za pomocą `nosniff` opcji, jeśli serwer jest w stanie "text/html" jest zawartość, Przeglądarka renderuje je jako "text/html".</span><span class="sxs-lookup"><span data-stu-id="97988-277">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="97988-278">Edytuj *nginx.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="97988-278">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="97988-279">Dodaj wiersz `add_header X-Content-Type-Options "nosniff";` i Zapisz plik, a następnie uruchom ponownie serwer Nginx.</span><span class="sxs-lookup"><span data-stu-id="97988-279">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="97988-280">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="97988-280">Additional resources</span></span>

* [<span data-ttu-id="97988-281">Wymagania wstępne dla platformy .NET Core w systemie Linux</span><span class="sxs-lookup"><span data-stu-id="97988-281">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* [<span data-ttu-id="97988-282">Serwer Nginx: Binarny wydania: Oficjalna pakietów systemu Debian/Ubuntu</span><span class="sxs-lookup"><span data-stu-id="97988-282">Nginx: Binary Releases: Official Debian/Ubuntu packages</span></span>](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* <xref:test/troubleshoot>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="97988-283">SERWER NGINX: Przy użyciu nagłówka przekazane</span><span class="sxs-lookup"><span data-stu-id="97988-283">NGINX: Using the Forwarded header</span></span>](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
