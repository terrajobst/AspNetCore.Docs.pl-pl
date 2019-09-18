---
title: Hostowanie ASP.NET Core w systemie Linux za pomocą Nginx
author: guardrex
description: Dowiedz się, jak skonfigurować Nginx jako zwrotny serwer proxy w Ubuntu 16,04 do przesyłania dalej ruchu HTTP do aplikacji internetowej ASP.NET Core działającej na Kestrel.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/31/2019
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: b71bc0464892f15ef8db0324a8e66a28a6192577
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080872"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="d49ba-103">Hostowanie ASP.NET Core w systemie Linux za pomocą Nginx</span><span class="sxs-lookup"><span data-stu-id="d49ba-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="d49ba-104">Autor [sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="d49ba-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="d49ba-105">W tym przewodniku wyjaśniono Konfigurowanie środowiska ASP.NET Core gotowego do produkcji na serwerze z systemem Ubuntu 16,04.</span><span class="sxs-lookup"><span data-stu-id="d49ba-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="d49ba-106">Te instrukcje będą działały z nowszymi wersjami Ubuntu, ale instrukcje nie zostały przetestowane z nowszymi wersjami.</span><span class="sxs-lookup"><span data-stu-id="d49ba-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

<span data-ttu-id="d49ba-107">Aby uzyskać informacje dotyczące innych dystrybucji systemu Linux obsługiwanych przez ASP.NET Core, zobacz [wymagania wstępne dotyczące platformy .NET Core w systemie Linux](/dotnet/core/linux-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="d49ba-107">For information on other Linux distributions supported by ASP.NET Core, see [Prerequisites for .NET Core on Linux](/dotnet/core/linux-prerequisites).</span></span>

> [!NOTE]
> <span data-ttu-id="d49ba-108">W przypadku Ubuntu 14,04 zaleca się, *Aby program był* monitorowany jako rozwiązanie do monitorowania procesu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d49ba-108">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="d49ba-109">*system* nie jest dostępny w systemie Ubuntu 14,04.</span><span class="sxs-lookup"><span data-stu-id="d49ba-109">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="d49ba-110">Instrukcje dotyczące programu Ubuntu 14,04 znajdują się w [poprzedniej wersji tego tematu](https://github.com/aspnet/AspNetCore.Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="d49ba-110">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/AspNetCore.Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="d49ba-111">Ten przewodnik:</span><span class="sxs-lookup"><span data-stu-id="d49ba-111">This guide:</span></span>

* <span data-ttu-id="d49ba-112">Umieszcza istniejącą aplikację ASP.NET Core za odwrotnym serwerem proxy.</span><span class="sxs-lookup"><span data-stu-id="d49ba-112">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="d49ba-113">Konfiguruje odwrotny serwer proxy do przesyłania żądań do serwera sieci Web Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d49ba-113">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="d49ba-114">Zapewnia, że aplikacja sieci Web będzie uruchamiana przy uruchamianiu jako demon.</span><span class="sxs-lookup"><span data-stu-id="d49ba-114">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="d49ba-115">Konfiguruje narzędzie do zarządzania procesami, aby pomóc w ponownym uruchomieniu aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d49ba-115">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d49ba-116">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d49ba-116">Prerequisites</span></span>

1. <span data-ttu-id="d49ba-117">Dostęp do serwera z systemem Ubuntu 16,04 przy użyciu konta użytkownika standardowego z uprawnieniami sudo.</span><span class="sxs-lookup"><span data-stu-id="d49ba-117">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="d49ba-118">Zainstaluj środowisko uruchomieniowe platformy .NET Core na serwerze.</span><span class="sxs-lookup"><span data-stu-id="d49ba-118">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="d49ba-119">Odwiedź [stronę wszystkie pliki do pobrania w programie .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="d49ba-119">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="d49ba-120">Wybierz najnowszą wersję środowiska uruchomieniowego bez podglądu z listy w obszarze **środowisko uruchomieniowe**.</span><span class="sxs-lookup"><span data-stu-id="d49ba-120">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="d49ba-121">Wybierz i postępuj zgodnie z instrukcjami dla Ubuntu, które pasują do wersji Ubuntu serwera.</span><span class="sxs-lookup"><span data-stu-id="d49ba-121">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="d49ba-122">Istniejąca aplikacja ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d49ba-122">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="d49ba-123">Publikowanie i kopiowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="d49ba-123">Publish and copy over the app</span></span>

<span data-ttu-id="d49ba-124">Skonfiguruj aplikację dla [wdrożenia zależnego od platformy](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="d49ba-124">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="d49ba-125">Jeśli aplikacja jest uruchamiana lokalnie i nie jest skonfigurowana do nawiązywania bezpiecznych połączeń (HTTPS), należy zastosować jedną z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="d49ba-125">If the app is run locally and isn't configured to make secure connections (HTTPS), adopt either of the following approaches:</span></span>

* <span data-ttu-id="d49ba-126">Skonfiguruj aplikację do obsługi bezpiecznych połączeń lokalnych.</span><span class="sxs-lookup"><span data-stu-id="d49ba-126">Configure the app to handle secure local connections.</span></span> <span data-ttu-id="d49ba-127">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja protokołu HTTPS](#https-configuration) .</span><span class="sxs-lookup"><span data-stu-id="d49ba-127">For more information, see the [HTTPS configuration](#https-configuration) section.</span></span>
* <span data-ttu-id="d49ba-128">Usuń `https://localhost:5001` (jeśli istnieje) `applicationUrl` z właściwości w pliku *Properties/profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="d49ba-128">Remove `https://localhost:5001` (if present) from the `applicationUrl` property in the *Properties/launchSettings.json* file.</span></span>

<span data-ttu-id="d49ba-129">Uruchom [dotnet Publish](/dotnet/core/tools/dotnet-publish) ze środowiska programistycznego, aby spakować aplikację do katalogu (na przykład *bin/Release&lt;/target_framework_moniker&gt;/Publish*), które można uruchomić na serwerze:</span><span class="sxs-lookup"><span data-stu-id="d49ba-129">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```dotnetcli
dotnet publish --configuration Release
```

<span data-ttu-id="d49ba-130">Aplikację można również opublikować jako [samodzielne wdrożenie](/dotnet/core/deploying/#self-contained-deployments-scd) , jeśli wolisz, aby nie obsługiwać środowiska uruchomieniowego .NET Core na serwerze.</span><span class="sxs-lookup"><span data-stu-id="d49ba-130">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="d49ba-131">Skopiuj aplikację ASP.NET Core na serwer przy użyciu narzędzia, które integruje się z przepływem pracy organizacji (na przykład SCP, SFTP).</span><span class="sxs-lookup"><span data-stu-id="d49ba-131">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="d49ba-132">Często można zlokalizować aplikacje sieci Web w katalogu *var* (na przykład *var/www/helloapp*).</span><span class="sxs-lookup"><span data-stu-id="d49ba-132">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="d49ba-133">W obszarze scenariusza wdrożenia produkcyjnego przepływ pracy ciągłej integracji wykonuje zadania publikowania aplikacji i kopiowania zasobów na serwer.</span><span class="sxs-lookup"><span data-stu-id="d49ba-133">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="d49ba-134">Testowanie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="d49ba-134">Test the app:</span></span>

1. <span data-ttu-id="d49ba-135">W wierszu polecenia Uruchom aplikację: `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="d49ba-135">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="d49ba-136">W przeglądarce przejdź do `http://<serveraddress>:<port>` , aby sprawdzić, czy aplikacja działa lokalnie w systemie Linux.</span><span class="sxs-lookup"><span data-stu-id="d49ba-136">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="d49ba-137">Konfigurowanie zwrotnego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="d49ba-137">Configure a reverse proxy server</span></span>

<span data-ttu-id="d49ba-138">Zwrotny serwer proxy to typowa konfiguracja służąca do obsługi dynamicznych aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d49ba-138">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="d49ba-139">Zwrotny serwer proxy przerywa żądanie HTTP i przekazuje go do aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d49ba-139">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="d49ba-140">Korzystanie z serwera zwrotnego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="d49ba-140">Use a reverse proxy server</span></span>

<span data-ttu-id="d49ba-141">Kestrel doskonale nadaje się do obsługi zawartości dynamicznej z ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d49ba-141">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="d49ba-142">Jednak funkcje obsługujące sieci Web nie są tak bogate jak takie jak serwery usług IIS, Apache lub nginx.</span><span class="sxs-lookup"><span data-stu-id="d49ba-142">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="d49ba-143">Odwrotny serwer proxy może odciążać działania, takie jak obsługa zawartości statycznej, buforowanie żądań, kompresowanie żądań i zakończenie protokołu HTTPS z serwera HTTP.</span><span class="sxs-lookup"><span data-stu-id="d49ba-143">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and HTTPS termination from the HTTP server.</span></span> <span data-ttu-id="d49ba-144">Odwrotny serwer proxy może znajdować się na dedykowanym komputerze lub może zostać wdrożony razem z serwerem HTTP.</span><span class="sxs-lookup"><span data-stu-id="d49ba-144">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="d49ba-145">Na potrzeby tego przewodnika jest używane pojedyncze wystąpienie Nginx.</span><span class="sxs-lookup"><span data-stu-id="d49ba-145">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="d49ba-146">Działa na tym samym serwerze, obok serwera HTTP.</span><span class="sxs-lookup"><span data-stu-id="d49ba-146">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="d49ba-147">W zależności od wymagań można wybrać inną konfigurację.</span><span class="sxs-lookup"><span data-stu-id="d49ba-147">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="d49ba-148">Ze względu na to, że żądania są przekazywane przez zwrotny serwer proxy, należy użyć [oprogramowania pośredniczącego "przesłane nagłówki](xref:host-and-deploy/proxy-load-balancer) " z pakietu [Microsoft. AspNetCore. HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) .</span><span class="sxs-lookup"><span data-stu-id="d49ba-148">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="d49ba-149">Oprogramowanie pośredniczące aktualizuje `Request.Scheme`, `X-Forwarded-Proto` używając nagłówka, tak aby identyfikatory URI przekierowania i inne zasady zabezpieczeń działały prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="d49ba-149">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="d49ba-150">Wszelkie składniki, które są zależne od schematu, takie jak uwierzytelnianie, generowanie linków, przekierowania i geolokalizacja, muszą być umieszczone po wywołaniu bezpośrednich nagłówków.</span><span class="sxs-lookup"><span data-stu-id="d49ba-150">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="d49ba-151">Zgodnie z ogólną zasadą przekazane nagłówki oprogramowania pośredniczącego powinny zostać uruchomione przed innymi oprogramowania pośredniczącego, z wyjątkiem diagnostyki i błędów obsługi oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="d49ba-151">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="d49ba-152">Takie porządkowanie zapewnia, że oprogramowanie pośredniczące polegające na informacjach o przekazanych nagłówkach może zużywać wartości nagłówka do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="d49ba-152">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

<span data-ttu-id="d49ba-153">Wywołaj `Startup.Configure` <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> metodę w przed wywołaniem lub podobnym schematem uwierzytelniania oprogramowania pośredniczącego. <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*></span><span class="sxs-lookup"><span data-stu-id="d49ba-153">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in `Startup.Configure` before calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> or similar authentication scheme middleware.</span></span> <span data-ttu-id="d49ba-154">Skonfiguruj oprogramowanie pośredniczące do przesyłania dalej `X-Forwarded-For` nagłówków `X-Forwarded-Proto` i:</span><span class="sxs-lookup"><span data-stu-id="d49ba-154">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

<span data-ttu-id="d49ba-155">Jeśli wartość <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> nie jest określona dla oprogramowania pośredniczącego, domyślne nagłówki są `None`do przodu.</span><span class="sxs-lookup"><span data-stu-id="d49ba-155">If no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="d49ba-156">Serwery proxy uruchomione na adresach sprzężenia zwrotnego (127.0.0.0/8, [:: 1]), w tym standardowy adres localhost (127.0.0.1), są domyślnie zaufane.</span><span class="sxs-lookup"><span data-stu-id="d49ba-156">Proxies running on loopback addresses (127.0.0.0/8, [::1]), including the standard localhost address (127.0.0.1), are trusted by default.</span></span> <span data-ttu-id="d49ba-157">Jeśli inne zaufane serwery proxy lub sieci w organizacji obsługują żądania między Internetem a serwerem sieci Web, należy dodać je do listy <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> lub <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> z <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span><span class="sxs-lookup"><span data-stu-id="d49ba-157">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="d49ba-158">Poniższy przykład dodaje zaufany serwer proxy pod adresem IP 10.0.0.100 do przesyłanych nagłówków pośredniczących `KnownProxies` w programie: `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="d49ba-158">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="d49ba-159">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="d49ba-159">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-nginx"></a><span data-ttu-id="d49ba-160">Zainstaluj Nginx</span><span class="sxs-lookup"><span data-stu-id="d49ba-160">Install Nginx</span></span>

<span data-ttu-id="d49ba-161">Służy `apt-get` do instalowania Nginx.</span><span class="sxs-lookup"><span data-stu-id="d49ba-161">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="d49ba-162">Instalator tworzy *systemowy* skrypt inicjujący, który uruchamia Nginx jako demon przy uruchamianiu systemu.</span><span class="sxs-lookup"><span data-stu-id="d49ba-162">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="d49ba-163">Postępuj zgodnie z instrukcjami dotyczącymi [instalacji Ubuntu na Nginx: Oficjalne pakiety](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)Debian/Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="d49ba-163">Follow the installation instructions for Ubuntu at [Nginx: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="d49ba-164">Jeśli Opcjonalne moduły Nginx są wymagane, może być wymagane skompilowanie Nginx ze źródła.</span><span class="sxs-lookup"><span data-stu-id="d49ba-164">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="d49ba-165">Ponieważ Nginx został zainstalowany po raz pierwszy, należy jawnie uruchomić go, uruchamiając polecenie:</span><span class="sxs-lookup"><span data-stu-id="d49ba-165">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="d49ba-166">Sprawdź, czy w przeglądarce jest wyświetlana domyślna strona docelowa dla Nginx.</span><span class="sxs-lookup"><span data-stu-id="d49ba-166">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="d49ba-167">Strona docelowa jest dostępna `http://<server_IP_address>/index.nginx-debian.html`pod adresem.</span><span class="sxs-lookup"><span data-stu-id="d49ba-167">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="d49ba-168">Konfigurowanie Nginx</span><span class="sxs-lookup"><span data-stu-id="d49ba-168">Configure Nginx</span></span>

<span data-ttu-id="d49ba-169">Aby skonfigurować Nginx jako zwrotny serwer proxy do przesyłania dalej żądań do aplikacji ASP.NET Core, zmodyfikuj */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="d49ba-169">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="d49ba-170">Otwórz go w edytorze tekstów i Zastąp zawartość następującym:</span><span class="sxs-lookup"><span data-stu-id="d49ba-170">Open it in a text editor, and replace the contents with the following:</span></span>

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

<span data-ttu-id="d49ba-171">Gdy nie `server_name` są zgodne, Nginx używa serwera domyślnego.</span><span class="sxs-lookup"><span data-stu-id="d49ba-171">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="d49ba-172">W przypadku braku zdefiniowanego serwera domyślnego pierwszy serwer w pliku konfiguracji jest domyślnym serwerem.</span><span class="sxs-lookup"><span data-stu-id="d49ba-172">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="d49ba-173">Najlepszym rozwiązaniem jest dodanie określonego serwera domyślnego, który zwraca kod stanu 444 w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="d49ba-173">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="d49ba-174">Domyślnym przykładem konfiguracji serwera jest:</span><span class="sxs-lookup"><span data-stu-id="d49ba-174">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="d49ba-175">W przypadku powyższego pliku konfiguracji i domyślnego serwera Nginx akceptuje publiczny ruch na porcie 80 z `example.com` nagłówkiem `*.example.com`hosta lub.</span><span class="sxs-lookup"><span data-stu-id="d49ba-175">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="d49ba-176">Żądania niepasujące do tych hostów nie zostaną przekazane do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d49ba-176">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="d49ba-177">Nginx przekazuje pasujące żądania do Kestrel w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d49ba-177">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="d49ba-178">Zobacz [, jak Nginx przetwarza żądanie,](https://nginx.org/docs/http/request_processing.html) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="d49ba-178">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span> <span data-ttu-id="d49ba-179">Aby zmienić Kestrel IP/port, zobacz [Kestrel: Konfiguracja](xref:fundamentals/servers/kestrel#endpoint-configuration)punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="d49ba-179">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="d49ba-180">Nie można określić odpowiedniej [dyrektywy nazwa_serwera](https://nginx.org/docs/http/server_names.html) , aby uwidocznić aplikację pod kątem luk w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="d49ba-180">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="d49ba-181">Powiązanie symboli wieloznacznych z poddomeną (na przykład `*.example.com`) nie ma znaczenia dla tego zagrożenia bezpieczeństwa, jeśli kontrolujesz całą domenę nadrzędną (w przeciwieństwie do `*.com`, który jest narażony).</span><span class="sxs-lookup"><span data-stu-id="d49ba-181">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="d49ba-182">Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="d49ba-182">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="d49ba-183">Po nawiązaniu konfiguracji Nginx Uruchom `sudo nginx -t` polecenie, aby zweryfikować składnię plików konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="d49ba-183">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="d49ba-184">Jeśli test pliku konfiguracji zakończy się pomyślnie, Wymuś Nginx aby pobrać zmiany, uruchamiając `sudo nginx -s reload`polecenie.</span><span class="sxs-lookup"><span data-stu-id="d49ba-184">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="d49ba-185">Aby bezpośrednio uruchomić aplikację na serwerze:</span><span class="sxs-lookup"><span data-stu-id="d49ba-185">To directly run the app on the server:</span></span>

1. <span data-ttu-id="d49ba-186">Przejdź do katalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d49ba-186">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="d49ba-187">Uruchom aplikację: `dotnet <app_assembly.dll>`, gdzie `app_assembly.dll` to nazwa pliku zestawu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d49ba-187">Run the app: `dotnet <app_assembly.dll>`, where `app_assembly.dll` is the assembly file name of the app.</span></span>

<span data-ttu-id="d49ba-188">Jeśli aplikacja działa na serwerze, ale nie odpowiada za pośrednictwem Internetu, sprawdź zaporę serwera i upewnij się, że port 80 jest otwarty.</span><span class="sxs-lookup"><span data-stu-id="d49ba-188">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="d49ba-189">W przypadku korzystania z maszyny wirtualnej usługi Azure Ubuntu Dodaj regułę sieciowej grupy zabezpieczeń (sieciowej grupy zabezpieczeń), która umożliwia ruch przychodzący portu 80.</span><span class="sxs-lookup"><span data-stu-id="d49ba-189">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="d49ba-190">Nie ma potrzeby włączania reguły portu 80 dla ruchu wychodzącego, ponieważ ruch wychodzący jest automatycznie udzielany, gdy reguła ruchu przychodzącego jest włączona.</span><span class="sxs-lookup"><span data-stu-id="d49ba-190">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="d49ba-191">Po zakończeniu testowania aplikacji Zamknij aplikację z `Ctrl+C` poziomu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="d49ba-191">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitor-the-app"></a><span data-ttu-id="d49ba-192">Monitorowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="d49ba-192">Monitor the app</span></span>

<span data-ttu-id="d49ba-193">Serwer jest skonfigurowany do przesyłania dalej żądań wysyłanych `http://<serveraddress>:80` do usługi ASP.NET Core aplikacji uruchomionej w systemie `http://127.0.0.1:5000`Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d49ba-193">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="d49ba-194">Nginx nie jest jednak skonfigurowany do zarządzania procesem Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d49ba-194">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="d49ba-195">*system* może służyć do tworzenia pliku usługi do uruchamiania i monitorowania podstawowej aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="d49ba-195">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="d49ba-196">*system* to system inicjujący, który udostępnia wiele zaawansowanych funkcji uruchamiania, zatrzymywania i zarządzania procesami.</span><span class="sxs-lookup"><span data-stu-id="d49ba-196">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="d49ba-197">Utwórz plik usługi</span><span class="sxs-lookup"><span data-stu-id="d49ba-197">Create the service file</span></span>

<span data-ttu-id="d49ba-198">Utwórz plik definicji usługi:</span><span class="sxs-lookup"><span data-stu-id="d49ba-198">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="d49ba-199">Poniżej znajduje się przykładowy plik usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="d49ba-199">The following is an example service file for the app:</span></span>

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

<span data-ttu-id="d49ba-200">Jeśli użytkownik nie korzysta z *sieci www — dane* nie są używane przez tę konfigurację, należy najpierw utworzyć użytkownika i nadać mu właściwy własność plików.</span><span class="sxs-lookup"><span data-stu-id="d49ba-200">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="d49ba-201">Użyj `TimeoutStopSec` , aby skonfigurować czas oczekiwania na wyłączenie aplikacji po odebraniu początkowego sygnału przerwania.</span><span class="sxs-lookup"><span data-stu-id="d49ba-201">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="d49ba-202">Jeśli aplikacja nie zostanie zamknięta w tym okresie, SIGKILL jest wystawiony, aby zakończyć działanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d49ba-202">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="d49ba-203">Podaj wartość jako bezjednostkowe sekundy (na przykład `150`), wartość przedziału czasu (na `2min 30s`przykład) lub `infinity` aby wyłączyć limit czasu.</span><span class="sxs-lookup"><span data-stu-id="d49ba-203">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="d49ba-204">`TimeoutStopSec`Wartością domyślną jest `DefaultTimeoutStopSec` wartość w pliku konfiguracji Menedżera (*systemd-system. conf*, *System. conf. d*, *systemed-User. conf*, *User. conf. d*).</span><span class="sxs-lookup"><span data-stu-id="d49ba-204">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="d49ba-205">Domyślny limit czasu dla większości dystrybucji wynosi 90 sekund.</span><span class="sxs-lookup"><span data-stu-id="d49ba-205">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="d49ba-206">W systemie Linux istnieje system plików z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="d49ba-206">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="d49ba-207">Ustawienie ASPNETCORE_ENVIRONMENT na "Product" skutkuje wyszukiwaniem pliku konfiguracji *appSettings. Production. JSON*, a nie *appSettings. produkcja. JSON*.</span><span class="sxs-lookup"><span data-stu-id="d49ba-207">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="d49ba-208">Niektóre wartości (na przykład parametry połączenia SQL) muszą zostać zmienione dla dostawców konfiguracji, aby odczytywać zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="d49ba-208">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="d49ba-209">Użyj poniższego polecenia, aby wygenerować poprawną wartość ucieczki do użycia w pliku konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="d49ba-209">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

<span data-ttu-id="d49ba-210">Separatory`:`dwukropek () nie są obsługiwane w nazwach zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="d49ba-210">Colon (`:`) separators aren't supported in environment variable names.</span></span> <span data-ttu-id="d49ba-211">Użyj podwójnego podkreślenia (`__`) zamiast dwukropka.</span><span class="sxs-lookup"><span data-stu-id="d49ba-211">Use a double underscore (`__`) in place of a colon.</span></span> <span data-ttu-id="d49ba-212">[Dostawca konfiguracji zmiennych środowiskowych](xref:fundamentals/configuration/index#environment-variables-configuration-provider) konwertuje podwójne podkreślenie na dwukropek, gdy zmienne środowiskowe są odczytywane w konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="d49ba-212">The [Environment Variables configuration provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider) converts double-underscores into colons when environment variables are read into configuration.</span></span> <span data-ttu-id="d49ba-213">W poniższym przykładzie klucz `ConnectionStrings:DefaultConnection` parametrów połączenia jest ustawiany na plik definicji usługi jako: `ConnectionStrings__DefaultConnection`</span><span class="sxs-lookup"><span data-stu-id="d49ba-213">In the following example, the connection string key `ConnectionStrings:DefaultConnection` is set into the service definition file as `ConnectionStrings__DefaultConnection`:</span></span>

```
Environment=ConnectionStrings__DefaultConnection={Connection String}
```

<span data-ttu-id="d49ba-214">Zapisz plik i Włącz usługę.</span><span class="sxs-lookup"><span data-stu-id="d49ba-214">Save the file and enable the service.</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="d49ba-215">Uruchom usługę i sprawdź, czy jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="d49ba-215">Start the service and verify that it's running.</span></span>

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

<span data-ttu-id="d49ba-216">Gdy zwrotny serwer proxy został skonfigurowany i Kestrel zarządzany za pomocą systemu, aplikacja sieci Web jest w pełni skonfigurowana i można uzyskać do niej dostęp z przeglądarki na `http://localhost`komputerze lokalnym pod adresem.</span><span class="sxs-lookup"><span data-stu-id="d49ba-216">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="d49ba-217">Jest ona również dostępna z komputera zdalnego, blokując zaporę, która może być zablokowana.</span><span class="sxs-lookup"><span data-stu-id="d49ba-217">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="d49ba-218">Inspekcja nagłówków `Server` odpowiedzi w nagłówku zostanie wyświetlona aplikacja ASP.NET Core obsługiwana przez Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d49ba-218">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="view-logs"></a><span data-ttu-id="d49ba-219">Wyświetlanie dzienników</span><span class="sxs-lookup"><span data-stu-id="d49ba-219">View logs</span></span>

<span data-ttu-id="d49ba-220">Ponieważ aplikacja internetowa korzystająca z usługi Kestrel `systemd`jest zarządzana przy użyciu programu, wszystkie zdarzenia i procesy są rejestrowane w scentralizowanym dzienniku.</span><span class="sxs-lookup"><span data-stu-id="d49ba-220">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="d49ba-221">Jednak ten dziennik zawiera wszystkie wpisy dla wszystkich usług i procesów zarządzanych przez `systemd`program.</span><span class="sxs-lookup"><span data-stu-id="d49ba-221">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="d49ba-222">Aby wyświetlić `kestrel-helloapp.service`konkretne elementy, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="d49ba-222">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="d49ba-223">Aby można było kontynuować filtrowanie, opcje czasu `--since today`, `--until 1 hour ago` takie jak, lub ich kombinacje mogą zmniejszyć liczbę zwróconych wpisów.</span><span class="sxs-lookup"><span data-stu-id="d49ba-223">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="d49ba-224">Ochrona danych</span><span class="sxs-lookup"><span data-stu-id="d49ba-224">Data protection</span></span>

<span data-ttu-id="d49ba-225">[ASP.NET Core stosu ochrony danych](xref:security/data-protection/introduction) jest używany przez kilka ASP.NET Core [middlewares](xref:fundamentals/middleware/index), w tym uwierzytelnianie pośredniczące uwierzytelniania (np. Oprogramowanie pośredniczące plików cookie) i ochrona za żądania między lokacjami (CSRF).</span><span class="sxs-lookup"><span data-stu-id="d49ba-225">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="d49ba-226">Nawet jeśli interfejsy API ochrony danych nie są wywoływane przez kod użytkownika, należy skonfigurować ochronę danych w celu utworzenia trwałego [magazynu kluczy](xref:security/data-protection/implementation/key-management)kryptograficznych.</span><span class="sxs-lookup"><span data-stu-id="d49ba-226">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="d49ba-227">Jeśli nie jest skonfigurowana ochrona danych, klucze są przechowywane w pamięci i odrzucone po ponownym uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d49ba-227">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="d49ba-228">Jeśli pierścień klucz jest przechowywany w pamięci, po ponownym uruchomieniu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="d49ba-228">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="d49ba-229">Wszystkie tokeny na podstawie plików cookie uwierzytelniania są unieważniane.</span><span class="sxs-lookup"><span data-stu-id="d49ba-229">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="d49ba-230">Użytkownicy muszą ponownie zaloguj się na ich następnego żądania.</span><span class="sxs-lookup"><span data-stu-id="d49ba-230">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="d49ba-231">Wszystkie dane chronione za pomocą pierścień klucz może już nie mogły być odszyfrowane.</span><span class="sxs-lookup"><span data-stu-id="d49ba-231">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="d49ba-232">Może to obejmować [tokenów CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) i [plików cookie programu ASP.NET Core MVC TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="d49ba-232">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="d49ba-233">Aby skonfigurować ochronę danych w celu utrwalenia i szyfrowania pierścienia kluczy, zobacz:</span><span class="sxs-lookup"><span data-stu-id="d49ba-233">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="long-request-header-fields"></a><span data-ttu-id="d49ba-234">Długie pola nagłówka żądania</span><span class="sxs-lookup"><span data-stu-id="d49ba-234">Long request header fields</span></span>

<span data-ttu-id="d49ba-235">Jeśli aplikacja wymaga pól nagłówka żądania dłużej niż jest to dozwolone przez domyślne ustawienia serwera proxy (zazwyczaj 4K lub 8K w zależności od platformy), następujące dyrektywy wymagają korekty.</span><span class="sxs-lookup"><span data-stu-id="d49ba-235">If the app requires request header fields longer than permitted by the proxy server's default settings (typically 4K or 8K depending on the platform), the following directives require adjustment.</span></span> <span data-ttu-id="d49ba-236">Wartości, które mają zostać zastosowane, są zależne od scenariusza.</span><span class="sxs-lookup"><span data-stu-id="d49ba-236">The values to apply are scenario-dependent.</span></span> <span data-ttu-id="d49ba-237">Aby uzyskać więcej informacji, zapoznaj się z dokumentacją serwera.</span><span class="sxs-lookup"><span data-stu-id="d49ba-237">For more information, see your server's documentation.</span></span>

* [<span data-ttu-id="d49ba-238">proxy_buffer_size</span><span class="sxs-lookup"><span data-stu-id="d49ba-238">proxy_buffer_size</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_buffer_size)
* [<span data-ttu-id="d49ba-239">proxy_buffers</span><span class="sxs-lookup"><span data-stu-id="d49ba-239">proxy_buffers</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_buffers)
* [<span data-ttu-id="d49ba-240">proxy_busy_buffers_size</span><span class="sxs-lookup"><span data-stu-id="d49ba-240">proxy_busy_buffers_size</span></span>](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_busy_buffers_size)
* [<span data-ttu-id="d49ba-241">large_client_header_buffers</span><span class="sxs-lookup"><span data-stu-id="d49ba-241">large_client_header_buffers</span></span>](https://nginx.org/docs/http/ngx_http_core_module.html#large_client_header_buffers)

> [!WARNING]
> <span data-ttu-id="d49ba-242">Nie należy zwiększać wartości domyślnych buforów serwerów proxy, chyba że jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="d49ba-242">Don't increase the default values of proxy buffers unless necessary.</span></span> <span data-ttu-id="d49ba-243">Zwiększenie tych wartości zwiększa ryzyko ataków przepełnienia buforu (przepełnienie) i ataki typu "odmowa usługi" (DoS) przez złośliwych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="d49ba-243">Increasing these values increases the risk of buffer overrun (overflow) and Denial of Service (DoS) attacks by malicious users.</span></span>

## <a name="secure-the-app"></a><span data-ttu-id="d49ba-244">Zabezpieczanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="d49ba-244">Secure the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="d49ba-245">Włącz AppArmor</span><span class="sxs-lookup"><span data-stu-id="d49ba-245">Enable AppArmor</span></span>

<span data-ttu-id="d49ba-246">Moduły zabezpieczeń systemu Linux (LSM) to struktura, która jest częścią jądra systemu Linux od systemu Linux 2,6.</span><span class="sxs-lookup"><span data-stu-id="d49ba-246">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="d49ba-247">LSM obsługuje różne implementacje modułów zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="d49ba-247">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="d49ba-248">[AppArmor](https://wiki.ubuntu.com/AppArmor) to LSM, który implementuje obowiązkowy System Access Control, który umożliwia confining programu z ograniczonym zestawem zasobów.</span><span class="sxs-lookup"><span data-stu-id="d49ba-248">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="d49ba-249">Upewnij się, że AppArmor jest włączona i prawidłowo skonfigurowana.</span><span class="sxs-lookup"><span data-stu-id="d49ba-249">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configure-the-firewall"></a><span data-ttu-id="d49ba-250">Konfigurowanie zapory</span><span class="sxs-lookup"><span data-stu-id="d49ba-250">Configure the firewall</span></span>

<span data-ttu-id="d49ba-251">Zamknij wszystkie porty zewnętrzne, które nie są używane.</span><span class="sxs-lookup"><span data-stu-id="d49ba-251">Close off all external ports that are not in use.</span></span> <span data-ttu-id="d49ba-252">Nieskomplikowana Zapora (UFW) zapewnia fronton dla programu `iptables` , dostarczając interfejs wiersza polecenia do konfigurowania zapory.</span><span class="sxs-lookup"><span data-stu-id="d49ba-252">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span>

> [!WARNING]
> <span data-ttu-id="d49ba-253">Zapora uniemożliwi dostęp do całego systemu, jeśli nie zostanie prawidłowo skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="d49ba-253">A firewall will prevent access to the whole system if not configured correctly.</span></span> <span data-ttu-id="d49ba-254">Niepowodzenie określenia poprawnego portu SSH spowoduje, że w przypadku korzystania z protokołu SSH w celu nawiązania połączenia z systemem zostanie on zablokowany.</span><span class="sxs-lookup"><span data-stu-id="d49ba-254">Failure to specify the correct SSH port will effectively lock you out of the system if you are using SSH to connect to it.</span></span> <span data-ttu-id="d49ba-255">Domyślnym portem jest 22.</span><span class="sxs-lookup"><span data-stu-id="d49ba-255">The default port is 22.</span></span> <span data-ttu-id="d49ba-256">Aby uzyskać więcej informacji, zobacz [wprowadzenie do UFW](https://help.ubuntu.com/community/UFW) i [Podręcznik](https://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span><span class="sxs-lookup"><span data-stu-id="d49ba-256">For more information, see the [introduction to ufw](https://help.ubuntu.com/community/UFW) and the [manual](https://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).</span></span>

<span data-ttu-id="d49ba-257">Zainstaluj `ufw` i skonfiguruj ją, aby zezwalać na ruch na wszystkich portach.</span><span class="sxs-lookup"><span data-stu-id="d49ba-257">Install `ufw` and configure it to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

sudo ufw enable
```

### <a name="secure-nginx"></a><span data-ttu-id="d49ba-258">Zabezpiecz Nginx</span><span class="sxs-lookup"><span data-stu-id="d49ba-258">Secure Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="d49ba-259">Zmień nazwę odpowiedzi Nginx</span><span class="sxs-lookup"><span data-stu-id="d49ba-259">Change the Nginx response name</span></span>

<span data-ttu-id="d49ba-260">Edit *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="d49ba-260">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="d49ba-261">Konfiguruj opcje</span><span class="sxs-lookup"><span data-stu-id="d49ba-261">Configure options</span></span>

<span data-ttu-id="d49ba-262">Skonfiguruj serwer przy użyciu dodatkowych wymaganych modułów.</span><span class="sxs-lookup"><span data-stu-id="d49ba-262">Configure the server with additional required modules.</span></span> <span data-ttu-id="d49ba-263">Rozważ użycie zapory aplikacji sieci Web, takiej jak [zapory ModSecurity](https://www.modsecurity.org/), aby zabezpieczyć aplikację.</span><span class="sxs-lookup"><span data-stu-id="d49ba-263">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="https-configuration"></a><span data-ttu-id="d49ba-264">Konfiguracja protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="d49ba-264">HTTPS configuration</span></span>

<span data-ttu-id="d49ba-265">**Konfigurowanie aplikacji do połączeń lokalnych (HTTPS)**</span><span class="sxs-lookup"><span data-stu-id="d49ba-265">**Configure the app for secure (HTTPS) local connections**</span></span>

<span data-ttu-id="d49ba-266">Polecenie [dotnet Run](/dotnet/core/tools/dotnet-run) używa pliku *właściwości/profilu launchsettings. JSON* aplikacji, który konfiguruje aplikację do nasłuchiwania na adresach URL `applicationUrl` dostarczonych przez właściwość (na przykład `https://localhost:5001; http://localhost:5000`).</span><span class="sxs-lookup"><span data-stu-id="d49ba-266">The [dotnet run](/dotnet/core/tools/dotnet-run) command uses the app's *Properties/launchSettings.json* file, which configures the app to listen on the URLs provided by the `applicationUrl` property (for example, `https://localhost:5001;http://localhost:5000`).</span></span>

<span data-ttu-id="d49ba-267">Skonfiguruj aplikację do korzystania z certyfikatu podczas opracowywania dla `dotnet run` polecenia lub środowiska programistycznego (F5 lub CTRL + F5 w Visual Studio Code), korzystając z jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="d49ba-267">Configure the app to use a certificate in development for the `dotnet run` command or development environment (F5 or Ctrl+F5 in Visual Studio Code) using one of the following approaches:</span></span>

* <span data-ttu-id="d49ba-268">[Zastąp domyślny certyfikat z konfiguracji](xref:fundamentals/servers/kestrel#configuration) (*Zalecane*)</span><span class="sxs-lookup"><span data-stu-id="d49ba-268">[Replace the default certificate from configuration](xref:fundamentals/servers/kestrel#configuration) (*Recommended*)</span></span>
* [<span data-ttu-id="d49ba-269">KestrelServerOptions.ConfigureHttpsDefaults</span><span class="sxs-lookup"><span data-stu-id="d49ba-269">KestrelServerOptions.ConfigureHttpsDefaults</span></span>](xref:fundamentals/servers/kestrel#configurehttpsdefaultsactionhttpsconnectionadapteroptions)

<span data-ttu-id="d49ba-270">**Konfigurowanie zwrotnego serwera proxy dla połączeń zabezpieczonych za pośrednictwem protokołu HTTPS**</span><span class="sxs-lookup"><span data-stu-id="d49ba-270">**Configure the reverse proxy for secure (HTTPS) client connections**</span></span>

* <span data-ttu-id="d49ba-271">Skonfiguruj serwer do nasłuchiwania ruchu https na porcie `443` , określając prawidłowy certyfikat wystawiony przez zaufany urząd certyfikacji (CA).</span><span class="sxs-lookup"><span data-stu-id="d49ba-271">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="d49ba-272">Ochrona zabezpieczeń poprzez zastosowanie niektórych praktyk przedstawionych w następującym pliku */etc/nginx/Nginx.conf* .</span><span class="sxs-lookup"><span data-stu-id="d49ba-272">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="d49ba-273">Przykłady obejmują wybranie silniejszego szyfru i przekierowanie całego ruchu przez protokół HTTP do protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d49ba-273">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="d49ba-274">Dodanie nagłówka `HTTP Strict-Transport-Security` (HSTS) gwarantuje, że wszystkie kolejne żądania wysyłane przez klienta korzystają z protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d49ba-274">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS.</span></span>

* <span data-ttu-id="d49ba-275">Nie dodawaj nagłówka HSTS ani nie wybieraj `max-age` odpowiedniego elementu, jeśli https zostanie wyłączony w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="d49ba-275">Don't add the HSTS header or chose an appropriate `max-age` if HTTPS will be disabled in the future.</span></span>

<span data-ttu-id="d49ba-276">Dodaj plik konfiguracji */etc/nginx/proxy.conf* :</span><span class="sxs-lookup"><span data-stu-id="d49ba-276">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="d49ba-277">Edytuj plik konfiguracji */etc/nginx/Nginx.conf* .</span><span class="sxs-lookup"><span data-stu-id="d49ba-277">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="d49ba-278">Przykład zawiera obie `http` sekcje i `server` w jednym pliku konfiguracyjnym.</span><span class="sxs-lookup"><span data-stu-id="d49ba-278">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="d49ba-279">Zabezpiecz Nginx z clickjacking</span><span class="sxs-lookup"><span data-stu-id="d49ba-279">Secure Nginx from clickjacking</span></span>

<span data-ttu-id="d49ba-280">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), znana także jako *atak polegająca na zaskarżeniu interfejsu użytkownika*, to złośliwy atak polegający na tym, że odwiedzanie witryny sieci Web jest trudne do kliknięcia linku lub przycisku na innej stronie niż aktualnie odwiedzane.</span><span class="sxs-lookup"><span data-stu-id="d49ba-280">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="d49ba-281">Użyj `X-FRAME-OPTIONS` , aby zabezpieczyć lokację.</span><span class="sxs-lookup"><span data-stu-id="d49ba-281">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="d49ba-282">Aby wyeliminować ataki clickjacking:</span><span class="sxs-lookup"><span data-stu-id="d49ba-282">To mitigate clickjacking attacks:</span></span>

1. <span data-ttu-id="d49ba-283">Edytuj plik *Nginx. conf* :</span><span class="sxs-lookup"><span data-stu-id="d49ba-283">Edit the *nginx.conf* file:</span></span>

   ```bash
   sudo nano /etc/nginx/nginx.conf
   ```

   <span data-ttu-id="d49ba-284">Dodaj wiersz `add_header X-Frame-Options "SAMEORIGIN";`.</span><span class="sxs-lookup"><span data-stu-id="d49ba-284">Add the line `add_header X-Frame-Options "SAMEORIGIN";`.</span></span>
1. <span data-ttu-id="d49ba-285">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="d49ba-285">Save the file.</span></span>
1. <span data-ttu-id="d49ba-286">Uruchom ponownie Nginx.</span><span class="sxs-lookup"><span data-stu-id="d49ba-286">Restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="d49ba-287">Wykrywanie typu MIME</span><span class="sxs-lookup"><span data-stu-id="d49ba-287">MIME-type sniffing</span></span>

<span data-ttu-id="d49ba-288">Ten nagłówek zapobiega większości przeglądarek z wykrywaniem MIME odpowiedzi w odniesieniu do zadeklarowanego typu zawartości, ponieważ nagłówek instruuje przeglądarkę, aby nie przesłaniał typu zawartości odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d49ba-288">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="d49ba-289">`nosniff` Jeśli na serwerze jest wyświetlana zawartość "text/html", przeglądarka renderuje ją jako "text/html".</span><span class="sxs-lookup"><span data-stu-id="d49ba-289">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="d49ba-290">Edytuj plik *Nginx. conf* :</span><span class="sxs-lookup"><span data-stu-id="d49ba-290">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="d49ba-291">Dodaj wiersz `add_header X-Content-Type-Options "nosniff";` i Zapisz plik, a następnie uruchom ponownie Nginx.</span><span class="sxs-lookup"><span data-stu-id="d49ba-291">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d49ba-292">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d49ba-292">Additional resources</span></span>

* [<span data-ttu-id="d49ba-293">Wymagania wstępne dotyczące programu .NET Core w systemie Linux</span><span class="sxs-lookup"><span data-stu-id="d49ba-293">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* [<span data-ttu-id="d49ba-294">Nginx Wersje binarne: Oficjalne pakiety Debian/Ubuntu</span><span class="sxs-lookup"><span data-stu-id="d49ba-294">Nginx: Binary Releases: Official Debian/Ubuntu packages</span></span>](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* <xref:test/troubleshoot>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="d49ba-295">NGINX Korzystanie z przekazanego nagłówka</span><span class="sxs-lookup"><span data-stu-id="d49ba-295">NGINX: Using the Forwarded header</span></span>](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
