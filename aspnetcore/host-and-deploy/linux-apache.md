---
title: Hostowanie ASP.NET Core w systemie Linux przy użyciu oprogramowania Apache
author: rick-anderson
description: Dowiedz się, jak skonfigurować Apache jako zwrotny serwer proxy w usłudze CentOS, aby przekierować ruch HTTP do aplikacji internetowej ASP.NET Core działającej w systemie Kestrel.
monikerRange: '>= aspnetcore-2.1'
ms.author: shboyer
ms.custom: mvc
ms.date: 02/05/2020
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 3a3edd961b08c1952e6ded8038ed7ada381c54b0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78657900"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="0b1e5-103">Hostowanie ASP.NET Core w systemie Linux przy użyciu oprogramowania Apache</span><span class="sxs-lookup"><span data-stu-id="0b1e5-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="0b1e5-104">Autor [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="0b1e5-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="0b1e5-105">Korzystając z tego przewodnika, Dowiedz się, jak skonfigurować [Apache](https://httpd.apache.org/) jako zwrotny serwer proxy w [CentOS 7](https://www.centos.org/) , aby przekierować ruch HTTP do aplikacji internetowej ASP.NET Core działającej na serwerze [Kestrel](xref:fundamentals/servers/kestrel) .</span><span class="sxs-lookup"><span data-stu-id="0b1e5-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="0b1e5-106">[Rozszerzenie mod_proxy](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html) i powiązane moduły tworzą zwrotny serwer proxy serwera.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-106">The [mod_proxy extension](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0b1e5-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="0b1e5-107">Prerequisites</span></span>

* <span data-ttu-id="0b1e5-108">Serwer z systemem CentOS 7 z kontem użytkownika standardowego z uprawnieniami sudo.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-108">Server running CentOS 7 with a standard user account with sudo privilege.</span></span>
* <span data-ttu-id="0b1e5-109">Zainstaluj środowisko uruchomieniowe platformy .NET Core na serwerze.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-109">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="0b1e5-110">Odwiedź [stronę pobieranie platformy .NET Core](https://dotnet.microsoft.com/download/dotnet-core).</span><span class="sxs-lookup"><span data-stu-id="0b1e5-110">Visit the [Download .NET Core page](https://dotnet.microsoft.com/download/dotnet-core).</span></span>
   1. <span data-ttu-id="0b1e5-111">Wybierz najnowszą wersję programu .NET Core niebędącą podglądem.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-111">Select the latest non-preview .NET Core version.</span></span>
   1. <span data-ttu-id="0b1e5-112">Pobierz najnowszą wersję środowiska uruchomieniowego bez podglądu w tabeli w obszarze **Uruchamianie aplikacji — środowisko uruchomieniowe**.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-112">Download the latest non-preview runtime in the table under **Run apps - Runtime**.</span></span>
   1. <span data-ttu-id="0b1e5-113">Wybierz łącze **instrukcje dotyczące Menedżera pakietów** systemu Linux i postępuj zgodnie z instrukcjami CentOS.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-113">Select the Linux **Package manager instructions** link and follow the CentOS instructions.</span></span>
* <span data-ttu-id="0b1e5-114">Istniejąca aplikacja ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-114">An existing ASP.NET Core app.</span></span>

<span data-ttu-id="0b1e5-115">W dowolnym momencie w przyszłości po uaktualnieniu struktury udostępnionej ponownie uruchom ASP.NET Core aplikacje hostowane przez serwer.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-115">At any point in the future after upgrading the shared framework, restart the ASP.NET Core apps hosted by the server.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="0b1e5-116">Publikowanie i kopiowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="0b1e5-116">Publish and copy over the app</span></span>

<span data-ttu-id="0b1e5-117">Skonfiguruj aplikację dla [wdrożenia zależnego od platformy](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="0b1e5-117">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="0b1e5-118">Jeśli aplikacja jest uruchamiana lokalnie i nie jest skonfigurowana do nawiązywania bezpiecznych połączeń (HTTPS), należy zastosować jedną z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-118">If the app is run locally and isn't configured to make secure connections (HTTPS), adopt either of the following approaches:</span></span>

* <span data-ttu-id="0b1e5-119">Skonfiguruj aplikację do obsługi bezpiecznych połączeń lokalnych.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-119">Configure the app to handle secure local connections.</span></span> <span data-ttu-id="0b1e5-120">Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja protokołu HTTPS](#https-configuration) .</span><span class="sxs-lookup"><span data-stu-id="0b1e5-120">For more information, see the [HTTPS configuration](#https-configuration) section.</span></span>
* <span data-ttu-id="0b1e5-121">Usuń `https://localhost:5001` (jeśli istnieje) z właściwości `applicationUrl` w pliku *Properties/profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="0b1e5-121">Remove `https://localhost:5001` (if present) from the `applicationUrl` property in the *Properties/launchSettings.json* file.</span></span>

<span data-ttu-id="0b1e5-122">Uruchom [dotnet Publish](/dotnet/core/tools/dotnet-publish) ze środowiska programistycznego, aby spakować aplikację do katalogu (na przykład *bin/Release/&lt;target_framework_moniker&gt;/Publish*), które można uruchomić na serwerze:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-122">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```dotnetcli
dotnet publish --configuration Release
```

<span data-ttu-id="0b1e5-123">Aplikację można również opublikować jako [samodzielne wdrożenie](/dotnet/core/deploying/#self-contained-deployments-scd) , jeśli wolisz, aby nie obsługiwać środowiska uruchomieniowego .NET Core na serwerze.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-123">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="0b1e5-124">Skopiuj aplikację ASP.NET Core na serwer przy użyciu narzędzia, które integruje się z przepływem pracy organizacji (na przykład SCP, SFTP).</span><span class="sxs-lookup"><span data-stu-id="0b1e5-124">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="0b1e5-125">Często można zlokalizować aplikacje sieci Web w katalogu *var* (na przykład *var/www/helloapp*).</span><span class="sxs-lookup"><span data-stu-id="0b1e5-125">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="0b1e5-126">W obszarze scenariusza wdrożenia produkcyjnego przepływ pracy ciągłej integracji wykonuje zadania publikowania aplikacji i kopiowania zasobów na serwer.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-126">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

## <a name="configure-a-proxy-server"></a><span data-ttu-id="0b1e5-127">Konfigurowanie serwera proxy</span><span class="sxs-lookup"><span data-stu-id="0b1e5-127">Configure a proxy server</span></span>

<span data-ttu-id="0b1e5-128">Zwrotny serwer proxy to typowa konfiguracja służąca do obsługi dynamicznych aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-128">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="0b1e5-129">Zwrotny serwer proxy przerywa żądanie HTTP i przekazuje go do aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-129">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="0b1e5-130">Serwer proxy, który przekazuje żądania klientów na inny serwer zamiast zaspokajać same żądania.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-130">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="0b1e5-131">Zwrotny serwer proxy przesyła do stałego miejsca docelowego, zazwyczaj w imieniu dowolnych klientów.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-131">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="0b1e5-132">W tym przewodniku program Apache jest skonfigurowany jako zwrotny serwer proxy uruchomiony na tym samym serwerze, na którym Kestrel obsługuje aplikację ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-132">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="0b1e5-133">Ze względu na to, że żądania są przekazywane przez zwrotny serwer proxy, należy użyć [oprogramowania pośredniczącego "przesłane nagłówki](xref:host-and-deploy/proxy-load-balancer) " z pakietu [Microsoft. AspNetCore. HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) .</span><span class="sxs-lookup"><span data-stu-id="0b1e5-133">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="0b1e5-134">Oprogramowanie pośredniczące aktualizuje `Request.Scheme`przy użyciu nagłówka `X-Forwarded-Proto`, dzięki czemu identyfikatory URI przekierowania i inne zasady zabezpieczeń działają poprawnie.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-134">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="0b1e5-135">Wszelkie składniki, które są zależne od schematu, takie jak uwierzytelnianie, generowanie linków, przekierowania i geolokalizacja, muszą być umieszczone po wywołaniu bezpośrednich nagłówków.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-135">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="0b1e5-136">Zgodnie z ogólną zasadą przekazane nagłówki oprogramowania pośredniczącego powinny zostać uruchomione przed innymi oprogramowania pośredniczącego, z wyjątkiem diagnostyki i błędów obsługi oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-136">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="0b1e5-137">Takie porządkowanie zapewnia, że oprogramowanie pośredniczące polegające na informacjach o przekazanych nagłówkach może zużywać wartości nagłówka do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-137">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

<span data-ttu-id="0b1e5-138">Wywołaj metodę <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> w `Startup.Configure` przed wywołaniem <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> lub podobnego oprogramowania pośredniczącego schematu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-138">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in `Startup.Configure` before calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> or similar authentication scheme middleware.</span></span> <span data-ttu-id="0b1e5-139">Skonfiguruj oprogramowanie pośredniczące, aby przekazywać `X-Forwarded-For` i `X-Forwarded-Proto` nagłówki:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-139">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
// using Microsoft.AspNetCore.HttpOverrides;

app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

<span data-ttu-id="0b1e5-140">Jeśli żadne <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> nie są określone dla oprogramowania pośredniczącego, domyślne nagłówki do przodu są `None`.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-140">If no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="0b1e5-141">Serwery proxy uruchomione na adresach sprzężenia zwrotnego (127.0.0.0/8, [:: 1]), w tym standardowy adres localhost (127.0.0.1), są domyślnie zaufane.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-141">Proxies running on loopback addresses (127.0.0.0/8, [::1]), including the standard localhost address (127.0.0.1), are trusted by default.</span></span> <span data-ttu-id="0b1e5-142">Jeśli inne zaufane serwery proxy lub sieci w organizacji obsługują żądania między Internetem a serwerem sieci Web, należy dodać je do listy <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> lub <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> z <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-142">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="0b1e5-143">Poniższy przykład dodaje zaufany serwer proxy pod adresem IP 10.0.0.100 do przekazanych nagłówków pośredniczących `KnownProxies` w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-143">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
// using System.Net;

services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="0b1e5-144">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-144">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-apache"></a><span data-ttu-id="0b1e5-145">Instalowanie oprogramowania Apache</span><span class="sxs-lookup"><span data-stu-id="0b1e5-145">Install Apache</span></span>

<span data-ttu-id="0b1e5-146">Aktualizowanie pakietów CentOS do najnowszych stabilnych wersji:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-146">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="0b1e5-147">Zainstaluj serwer Apache Web Server w systemie CentOS za pomocą jednego polecenia `yum`:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-147">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="0b1e5-148">Przykładowe dane wyjściowe po uruchomieniu polecenia:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-148">Sample output after running the command:</span></span>

```bash
Downloading packages:
httpd-2.4.6-40.el7.centos.4.x86_64.rpm               | 2.7 MB  00:00:01
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Installing : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 
Verifying  : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 

Installed:
httpd.x86_64 0:2.4.6-40.el7.centos.4

Complete!
```

> [!NOTE]
> <span data-ttu-id="0b1e5-149">W tym przykładzie dane wyjściowe odzwierciedlają http. 86_64, ponieważ wersja CentOS 7 jest 64 bit.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-149">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="0b1e5-150">Aby sprawdzić, gdzie jest zainstalowany program Apache, uruchom `whereis httpd` z poziomu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-150">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache"></a><span data-ttu-id="0b1e5-151">Konfiguruj Apache</span><span class="sxs-lookup"><span data-stu-id="0b1e5-151">Configure Apache</span></span>

<span data-ttu-id="0b1e5-152">Pliki konfiguracji dla oprogramowania Apache znajdują się w katalogu `/etc/httpd/conf.d/`.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-152">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="0b1e5-153">Każdy plik z rozszerzeniem *. conf* jest przetwarzany w kolejności alfabetycznej oprócz plików konfiguracji modułu w `/etc/httpd/conf.modules.d/`, które zawierają pliki konfiguracyjne niezbędne do załadowania modułów.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-153">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="0b1e5-154">Utwórz plik konfiguracji o nazwie *helloapp. conf*dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-154">Create a configuration file, named *helloapp.conf*, for the app:</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}helloapp-error.log
    CustomLog ${APACHE_LOG_DIR}helloapp-access.log common
</VirtualHost>
```

<span data-ttu-id="0b1e5-155">Blok `VirtualHost` może występować wiele razy w co najmniej jednym pliku na serwerze.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-155">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="0b1e5-156">W poprzednim pliku konfiguracyjnym Apache akceptuje ruch publiczny na porcie 80.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-156">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="0b1e5-157">`www.example.com` domeny jest obsługiwany, a alias `*.example.com` jest rozpoznawany jako ta sama witryna sieci Web.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-157">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="0b1e5-158">Aby uzyskać więcej informacji, zobacz [Obsługa hosta wirtualnego opartego na nazwach](https://httpd.apache.org/docs/current/vhosts/name-based.html) .</span><span class="sxs-lookup"><span data-stu-id="0b1e5-158">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="0b1e5-159">Żądania są przekazywane w katalogu głównym do portu 5000 serwera o wartości 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-159">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="0b1e5-160">W przypadku komunikacji dwukierunkowej wymagane są `ProxyPass` i `ProxyPassReverse`.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-160">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span> <span data-ttu-id="0b1e5-161">Aby zmienić adres IP/port Kestrel, zobacz [Kestrel: Konfiguracja punktu końcowego](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="0b1e5-161">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="0b1e5-162">Niepowodzenie określenia odpowiedniej [dyrektywy ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername) w bloku **VirtualHost** uwidacznia aplikację pod kątem luk w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-162">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="0b1e5-163">Powiązanie symboli wieloznacznych w poddomenie (na przykład `*.example.com`) nie ma znaczenia dla tego zagrożenia bezpieczeństwa, jeśli kontrolujesz całą domenę nadrzędną (w przeciwieństwie do `*.com`, która jest narażona).</span><span class="sxs-lookup"><span data-stu-id="0b1e5-163">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="0b1e5-164">Aby uzyskać więcej informacji, zobacz [sekcję rfc7230-5,4](https://tools.ietf.org/html/rfc7230#section-5.4) .</span><span class="sxs-lookup"><span data-stu-id="0b1e5-164">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="0b1e5-165">Rejestrowanie można skonfigurować na `VirtualHost` przy użyciu dyrektyw `ErrorLog` i `CustomLog`.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-165">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="0b1e5-166">`ErrorLog` to lokalizacja, w której serwer rejestruje błędy, a `CustomLog` ustawia nazwę pliku dziennika i jego format.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-166">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="0b1e5-167">W tym przypadku jest to miejsce, w którym rejestrowane są informacje o żądaniu.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-167">In this case, this is where request information is logged.</span></span> <span data-ttu-id="0b1e5-168">Jeden wiersz dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-168">There's one line for each request.</span></span>

<span data-ttu-id="0b1e5-169">Zapisz plik i przetestuj konfigurację.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-169">Save the file and test the configuration.</span></span> <span data-ttu-id="0b1e5-170">Jeśli wszystko kończy się, odpowiedź powinna być `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-170">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="0b1e5-171">Uruchom ponownie Apache:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-171">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitor-the-app"></a><span data-ttu-id="0b1e5-172">Monitorowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="0b1e5-172">Monitor the app</span></span>

<span data-ttu-id="0b1e5-173">Program Apache jest teraz skonfigurowany do przesyłania dalej żądań `http://localhost:80` do aplikacji ASP.NET Core uruchomionej w witrynie Kestrel w `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-173">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="0b1e5-174">Program Apache nie jest jednak skonfigurowany do zarządzania procesem Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-174">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="0b1e5-175">Użyj *systemu* i Utwórz plik usługi, aby uruchomić i monitorować podstawową aplikację sieci Web.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-175">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="0b1e5-176">*system* to system inicjujący, który udostępnia wiele zaawansowanych funkcji uruchamiania, zatrzymywania i zarządzania procesami.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-176">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span>

### <a name="create-the-service-file"></a><span data-ttu-id="0b1e5-177">Utwórz plik usługi</span><span class="sxs-lookup"><span data-stu-id="0b1e5-177">Create the service file</span></span>

<span data-ttu-id="0b1e5-178">Utwórz plik definicji usługi:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-178">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="0b1e5-179">Przykładowy plik usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-179">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/www/helloapp
ExecStart=/usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="0b1e5-180">W poprzednim przykładzie użytkownik zarządzający usługą jest określony przez opcję `User`.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-180">In the preceding example, the user that manages the service is specified by the `User` option.</span></span> <span data-ttu-id="0b1e5-181">Użytkownik (`apache`) musi istnieć i mieć właściwy własność plików aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-181">The user (`apache`) must exist and have proper ownership of the app's files.</span></span>

<span data-ttu-id="0b1e5-182">Użyj `TimeoutStopSec`, aby skonfigurować czas oczekiwania na wyłączenie aplikacji po odebraniu początkowego sygnału przerwania.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-182">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="0b1e5-183">Jeśli aplikacja nie zostanie zamknięta w tym okresie, SIGKILL jest wystawiony, aby zakończyć działanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-183">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="0b1e5-184">Podaj wartość w postaci bezjednostkowej sekund (na przykład `150`), wartość przedziału czasu (na przykład `2min 30s`) lub `infinity`, aby wyłączyć limit czasu.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-184">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="0b1e5-185">`TimeoutStopSec` domyślną wartością `DefaultTimeoutStopSec` w pliku konfiguracji Menedżera (*systemd-system. conf*, *System. conf. d*, *systemed-User. conf*, *User. conf. d*).</span><span class="sxs-lookup"><span data-stu-id="0b1e5-185">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="0b1e5-186">Domyślny limit czasu dla większości dystrybucji wynosi 90 sekund.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-186">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="0b1e5-187">Niektóre wartości (na przykład parametry połączenia SQL) muszą zostać zmienione dla dostawców konfiguracji, aby odczytywać zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-187">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="0b1e5-188">Użyj poniższego polecenia, aby wygenerować poprawną wartość ucieczki do użycia w pliku konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-188">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

<span data-ttu-id="0b1e5-189">Separatory dwukropek (`:`) nie są obsługiwane w nazwach zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-189">Colon (`:`) separators aren't supported in environment variable names.</span></span> <span data-ttu-id="0b1e5-190">Użyj podwójnego podkreślenia (`__`) zamiast dwukropka.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-190">Use a double underscore (`__`) in place of a colon.</span></span> <span data-ttu-id="0b1e5-191">[Dostawca konfiguracji zmiennych środowiskowych](xref:fundamentals/configuration/index#environment-variables-configuration-provider) konwertuje podwójne podkreślenie na dwukropek, gdy zmienne środowiskowe są odczytywane w konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-191">The [Environment Variables configuration provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider) converts double-underscores into colons when environment variables are read into configuration.</span></span> <span data-ttu-id="0b1e5-192">W poniższym przykładzie klucz parametrów połączenia `ConnectionStrings:DefaultConnection` jest ustawiany w pliku definicji usługi jako `ConnectionStrings__DefaultConnection`:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-192">In the following example, the connection string key `ConnectionStrings:DefaultConnection` is set into the service definition file as `ConnectionStrings__DefaultConnection`:</span></span>

```
Environment=ConnectionStrings__DefaultConnection={Connection String}
```

<span data-ttu-id="0b1e5-193">Zapisz plik i Włącz usługę:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-193">Save the file and enable the service:</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="0b1e5-194">Uruchom usługę i sprawdź, czy jest uruchomiona:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-194">Start the service and verify that it's running:</span></span>

```bash
sudo systemctl start kestrel-helloapp.service
sudo systemctl status kestrel-helloapp.service

● kestrel-helloapp.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-helloapp.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-helloapp.service
            └─9021 /usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
```

<span data-ttu-id="0b1e5-195">Gdy zwrotny serwer proxy został skonfigurowany i Kestrel zarządzany za pomocą *systemu*, aplikacja sieci Web jest w pełni skonfigurowana i można uzyskać do niej dostęp z przeglądarki na komputerze lokalnym w `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-195">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="0b1e5-196">Sprawdzanie nagłówków odpowiedzi, nagłówek **serwera** wskazuje, że aplikacja ASP.NET Core jest obsługiwana przez Kestrel:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-196">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="view-logs"></a><span data-ttu-id="0b1e5-197">Wyświetlanie dzienników</span><span class="sxs-lookup"><span data-stu-id="0b1e5-197">View logs</span></span>

<span data-ttu-id="0b1e5-198">Ponieważ aplikacja sieci Web używająca Kestrel jest zarządzana przy użyciu *systemu*, zdarzenia i procesy są rejestrowane w scentralizowanym dzienniku.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-198">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="0b1e5-199">Ten dziennik zawiera jednak wpisy dla wszystkich usług i procesów zarządzanych przez *system*.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-199">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="0b1e5-200">Aby wyświetlić elementy specyficzne dla `kestrel-helloapp.service`, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-200">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="0b1e5-201">W przypadku filtrowania czasu określ opcje czasu za pomocą polecenia.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-201">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="0b1e5-202">Na przykład użyj `--since today`, aby odfiltrować bieżący dzień lub `--until 1 hour ago` w celu wyświetlenia wpisów z poprzedniej godziny.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-202">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="0b1e5-203">Aby uzyskać więcej informacji, zobacz [stronę Man for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="0b1e5-203">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="0b1e5-204">Ochrona danych</span><span class="sxs-lookup"><span data-stu-id="0b1e5-204">Data protection</span></span>

<span data-ttu-id="0b1e5-205">[ASP.NET Core stosu ochrony danych](xref:security/data-protection/introduction) jest używany przez kilka ASP.NET Core [middlewares](xref:fundamentals/middleware/index), w tym uwierzytelnianie pośredniczące uwierzytelniania (np. Oprogramowanie pośredniczące plików cookie) i ochrona za żądania między lokacjami (CSRF).</span><span class="sxs-lookup"><span data-stu-id="0b1e5-205">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="0b1e5-206">Nawet jeśli interfejsy API ochrony danych nie są wywoływane przez kod użytkownika, należy skonfigurować ochronę danych w celu utworzenia trwałego [magazynu kluczy](xref:security/data-protection/implementation/key-management)kryptograficznych.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-206">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="0b1e5-207">Jeśli nie jest skonfigurowana ochrona danych, klucze są przechowywane w pamięci i odrzucone po ponownym uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-207">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="0b1e5-208">Jeśli pierścień klucz jest przechowywany w pamięci, po ponownym uruchomieniu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-208">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="0b1e5-209">Wszystkie tokeny na podstawie plików cookie uwierzytelniania są unieważniane.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-209">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="0b1e5-210">Użytkownicy muszą ponownie zaloguj się na ich następnego żądania.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-210">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="0b1e5-211">Wszystkie dane chronione za pomocą pierścień klucz może już nie mogły być odszyfrowane.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-211">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="0b1e5-212">Może to obejmować [tokeny CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) [ASP.NET Core i pliki cookie MVC TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="0b1e5-212">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="0b1e5-213">Aby skonfigurować ochronę danych w celu utrwalenia i szyfrowania pierścienia kluczy, zobacz:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-213">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="secure-the-app"></a><span data-ttu-id="0b1e5-214">Zabezpieczanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="0b1e5-214">Secure the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="0b1e5-215">Konfigurowanie zapory</span><span class="sxs-lookup"><span data-stu-id="0b1e5-215">Configure firewall</span></span>

<span data-ttu-id="0b1e5-216">*Zapora* jest demonem dynamicznym do zarządzania zaporą z obsługą stref sieciowych.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-216">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="0b1e5-217">Porty i filtrowanie pakietów nadal mogą być zarządzane przez dołączenie iptables.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-217">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="0b1e5-218">*Zapora* powinna być instalowana domyślnie.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-218">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="0b1e5-219">`yum` można użyć, aby zainstalować pakiet lub sprawdzić jego instalację.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-219">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="0b1e5-220">Użyj `firewalld`, aby otworzyć tylko te porty, które są konieczne dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-220">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="0b1e5-221">W takim przypadku używane są porty 80 i 443.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-221">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="0b1e5-222">Następujące polecenia trwale ustawiają porty 80 i 443, aby otworzyć:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-222">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="0b1e5-223">Załaduj ponownie ustawienia zapory.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-223">Reload the firewall settings.</span></span> <span data-ttu-id="0b1e5-224">Sprawdź dostępne usługi i porty w strefie domyślnej.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-224">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="0b1e5-225">Opcje są dostępne przez sprawdzenie `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-225">Options are available by inspecting `firewall-cmd -h`.</span></span>

```bash
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

```bash
public (default, active)
interfaces: eth0
sources: 
services: dhcpv6-client
ports: 443/tcp 80/tcp
masquerade: no
forward-ports: 
icmp-blocks: 
rich rules: 
```

### <a name="https-configuration"></a><span data-ttu-id="0b1e5-226">Konfiguracja protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="0b1e5-226">HTTPS configuration</span></span>

<span data-ttu-id="0b1e5-227">**Konfigurowanie aplikacji do połączeń lokalnych (HTTPS)**</span><span class="sxs-lookup"><span data-stu-id="0b1e5-227">**Configure the app for secure (HTTPS) local connections**</span></span>

<span data-ttu-id="0b1e5-228">Polecenie [dotnet Run](/dotnet/core/tools/dotnet-run) używa pliku *właściwości/profilu launchsettings. JSON* aplikacji, który konfiguruje aplikację do nasłuchiwania na adresach url dostarczonych przez właściwość `applicationUrl` (na przykład `https://localhost:5001; http://localhost:5000`).</span><span class="sxs-lookup"><span data-stu-id="0b1e5-228">The [dotnet run](/dotnet/core/tools/dotnet-run) command uses the app's *Properties/launchSettings.json* file, which configures the app to listen on the URLs provided by the `applicationUrl` property (for example, `https://localhost:5001;http://localhost:5000`).</span></span>

<span data-ttu-id="0b1e5-229">Skonfiguruj aplikację do korzystania z certyfikatu w środowisku programistycznym dla polecenia `dotnet run` lub środowiska programistycznego (F5 lub CTRL + F5 w Visual Studio Code), korzystając z jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-229">Configure the app to use a certificate in development for the `dotnet run` command or development environment (F5 or Ctrl+F5 in Visual Studio Code) using one of the following approaches:</span></span>

* <span data-ttu-id="0b1e5-230">[Zastąp domyślny certyfikat z konfiguracji](xref:fundamentals/servers/kestrel#configuration) (*zalecane*)</span><span class="sxs-lookup"><span data-stu-id="0b1e5-230">[Replace the default certificate from configuration](xref:fundamentals/servers/kestrel#configuration) (*Recommended*)</span></span>
* [<span data-ttu-id="0b1e5-231">KestrelServerOptions.ConfigureHttpsDefaults</span><span class="sxs-lookup"><span data-stu-id="0b1e5-231">KestrelServerOptions.ConfigureHttpsDefaults</span></span>](xref:fundamentals/servers/kestrel#configurehttpsdefaultsactionhttpsconnectionadapteroptions)

<span data-ttu-id="0b1e5-232">**Konfigurowanie zwrotnego serwera proxy dla połączeń zabezpieczonych za pośrednictwem protokołu HTTPS**</span><span class="sxs-lookup"><span data-stu-id="0b1e5-232">**Configure the reverse proxy for secure (HTTPS) client connections**</span></span>

<span data-ttu-id="0b1e5-233">Aby skonfigurować Apache for HTTPS, używany jest moduł *mod_ssl* .</span><span class="sxs-lookup"><span data-stu-id="0b1e5-233">To configure Apache for HTTPS, the *mod_ssl* module is used.</span></span> <span data-ttu-id="0b1e5-234">Po zainstalowaniu modułu *http* zainstalowano również moduł *mod_ssl* .</span><span class="sxs-lookup"><span data-stu-id="0b1e5-234">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="0b1e5-235">Jeśli nie została zainstalowana, użyj `yum`, aby dodać ją do konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-235">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```

<span data-ttu-id="0b1e5-236">Aby wymusić HTTPS, zainstaluj moduł `mod_rewrite`, aby umożliwić ponowne zapisywanie adresów URL:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-236">To enforce HTTPS, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="0b1e5-237">Zmodyfikuj plik *helloapp. conf* , aby umożliwić ponowne zapisywanie adresów URL i bezpieczną komunikację na porcie 443:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-237">Modify the *helloapp.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/helloapp-error.log
    CustomLog /var/log/httpd/helloapp-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> <span data-ttu-id="0b1e5-238">W tym przykładzie użyto certyfikatu wygenerowanego lokalnie.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-238">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="0b1e5-239">**SSLCertificateFile** powinien być podstawowym plikiem certyfikatu dla nazwy domeny.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-239">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="0b1e5-240">**SSLCertificateKeyFile** powinien być plikiem klucza generowanym podczas tworzenia CSR.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-240">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="0b1e5-241">**SSLCertificateChainFile** powinien być pośrednim plikiem certyfikatu (jeśli istnieje), który został dostarczony przez urząd certyfikacji.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-241">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="0b1e5-242">Zapisz plik i przetestuj konfigurację:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-242">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="0b1e5-243">Uruchom ponownie Apache:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-243">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="0b1e5-244">Dodatkowe sugestie Apache</span><span class="sxs-lookup"><span data-stu-id="0b1e5-244">Additional Apache suggestions</span></span>

### <a name="restart-apps-with-shared-framework-updates"></a><span data-ttu-id="0b1e5-245">Ponowne uruchamianie aplikacji przy użyciu aktualizacji platformy udostępnionej</span><span class="sxs-lookup"><span data-stu-id="0b1e5-245">Restart apps with shared framework updates</span></span>

<span data-ttu-id="0b1e5-246">Po uaktualnieniu platformy udostępnionej na serwerze uruchom ponownie ASP.NET Core aplikacje hostowane przez serwer.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-246">After upgrading the shared framework on the server, restart the ASP.NET Core apps hosted by the server.</span></span>

### <a name="additional-headers"></a><span data-ttu-id="0b1e5-247">Dodatkowe nagłówki</span><span class="sxs-lookup"><span data-stu-id="0b1e5-247">Additional headers</span></span>

<span data-ttu-id="0b1e5-248">Aby zabezpieczyć przed złośliwymi atakami, istnieje kilka nagłówków, które należy zmodyfikować lub dodać.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-248">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="0b1e5-249">Upewnij się, że moduł `mod_headers` jest zainstalowany:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-249">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="0b1e5-250">Zabezpiecz ataki Apache from clickjacking</span><span class="sxs-lookup"><span data-stu-id="0b1e5-250">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="0b1e5-251">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), znana także jako *atak polegająca na zaskarżeniu interfejsu użytkownika*, to złośliwy atak polegający na tym, że odwiedzanie witryny sieci Web jest trudne do kliknięcia linku lub przycisku na innej stronie niż aktualnie odwiedzane.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-251">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="0b1e5-252">Użyj `X-FRAME-OPTIONS`, aby zabezpieczyć lokację.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-252">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="0b1e5-253">Aby wyeliminować ataki clickjacking:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-253">To mitigate clickjacking attacks:</span></span>

1. <span data-ttu-id="0b1e5-254">Edytuj plik *http. conf* :</span><span class="sxs-lookup"><span data-stu-id="0b1e5-254">Edit the *httpd.conf* file:</span></span>

   ```bash
   sudo nano /etc/httpd/conf/httpd.conf
   ```

   <span data-ttu-id="0b1e5-255">Dodaj wiersz `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-255">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span>
1. <span data-ttu-id="0b1e5-256">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-256">Save the file.</span></span>
1. <span data-ttu-id="0b1e5-257">Uruchom ponownie Apache.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-257">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="0b1e5-258">Wykrywanie typu MIME</span><span class="sxs-lookup"><span data-stu-id="0b1e5-258">MIME-type sniffing</span></span>

<span data-ttu-id="0b1e5-259">Nagłówek `X-Content-Type-Options` uniemożliwia programowi Internet Explorer przeprowadzanie *wykrywania MIME* (Określanie `Content-Type` pliku z zawartości pliku).</span><span class="sxs-lookup"><span data-stu-id="0b1e5-259">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="0b1e5-260">Jeśli serwer ustawi `Content-Type` nagłówek, aby `text/html` z zestawem opcji `nosniff`, program Internet Explorer będzie renderować zawartość jako `text/html` niezależnie od zawartości pliku.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-260">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="0b1e5-261">Edytuj plik *http. conf* :</span><span class="sxs-lookup"><span data-stu-id="0b1e5-261">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="0b1e5-262">Dodaj wiersz `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-262">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="0b1e5-263">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-263">Save the file.</span></span> <span data-ttu-id="0b1e5-264">Uruchom ponownie Apache.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-264">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="0b1e5-265">Równoważenie obciążenia</span><span class="sxs-lookup"><span data-stu-id="0b1e5-265">Load Balancing</span></span>

<span data-ttu-id="0b1e5-266">W tym przykładzie przedstawiono sposób konfigurowania i konfigurowania oprogramowania Apache w systemie CentOS 7 i Kestrel na tym samym komputerze wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-266">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="0b1e5-267">Aby nie mieć single point of failure; Używanie *mod_proxy_balancer* i modyfikowanie **VirtualHost** umożliwi zarządzanie wieloma wystąpieniami aplikacji sieci Web za serwerem Apache proxy.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-267">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="0b1e5-268">W pliku konfiguracyjnym przedstawionym poniżej dodatkowe wystąpienie `helloapp` jest skonfigurowane do uruchamiania na porcie 5001.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-268">In the configuration file shown below, an additional instance of the `helloapp` is set up to run on port 5001.</span></span> <span data-ttu-id="0b1e5-269">Sekcja *proxy* jest ustawiana z konfiguracją modułu równoważenia obciążenia z dwoma elementami członkowskimi w celu zrównoważenia *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-269">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPass / balancer://mycluster/ 

    ProxyPassReverse / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5001/

    <Proxy balancer://mycluster>
        BalancerMember http://127.0.0.1:5000
        BalancerMember http://127.0.0.1:5001 
        ProxySet lbmethod=byrequests
    </Proxy>

    <Location />
        SetHandler balancer
    </Location>
    ErrorLog /var/log/httpd/helloapp-error.log
    CustomLog /var/log/httpd/helloapp-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a><span data-ttu-id="0b1e5-270">Limity szybkości</span><span class="sxs-lookup"><span data-stu-id="0b1e5-270">Rate Limits</span></span>

<span data-ttu-id="0b1e5-271">Za pomocą *mod_ratelimit*, który jest uwzględniony w module *http* , przepustowość klientów może być ograniczona:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-271">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```

<span data-ttu-id="0b1e5-272">Przykładowy plik ogranicza przepustowość jako 600 KB/s w lokalizacji głównej:</span><span class="sxs-lookup"><span data-stu-id="0b1e5-272">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

### <a name="long-request-header-fields"></a><span data-ttu-id="0b1e5-273">Długie pola nagłówka żądania</span><span class="sxs-lookup"><span data-stu-id="0b1e5-273">Long request header fields</span></span>

<span data-ttu-id="0b1e5-274">Ustawienia domyślne serwera proxy zwykle ograniczają pola nagłówka żądania do 8 190 bajtów.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-274">Proxy server default settings typically limit request header fields to 8,190 bytes.</span></span> <span data-ttu-id="0b1e5-275">Aplikacja może wymagać pól, które są dłuższe niż domyślne (na przykład aplikacje, które używają [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)).</span><span class="sxs-lookup"><span data-stu-id="0b1e5-275">An app may require fields longer than the default (for example, apps that use [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)).</span></span> <span data-ttu-id="0b1e5-276">Jeśli są wymagane dłuższe pola, dyrektywa [LimitRequestFieldSize](https://httpd.apache.org/docs/2.4/mod/core.html#LimitRequestFieldSize) serwera proxy wymaga korekty.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-276">If longer fields are required, the proxy server's [LimitRequestFieldSize](https://httpd.apache.org/docs/2.4/mod/core.html#LimitRequestFieldSize) directive requires adjustment.</span></span> <span data-ttu-id="0b1e5-277">Wartość, która ma zostać zastosowana, zależy od scenariusza.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-277">The value to apply depends on the scenario.</span></span> <span data-ttu-id="0b1e5-278">Aby uzyskać więcej informacji, zapoznaj się z dokumentacją serwera.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-278">For more information, see your server's documentation.</span></span>

> [!WARNING]
> <span data-ttu-id="0b1e5-279">Nie należy zwiększać wartości domyślnej `LimitRequestFieldSize`, o ile nie jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-279">Don't increase the default value of `LimitRequestFieldSize` unless necessary.</span></span> <span data-ttu-id="0b1e5-280">Zwiększenie wartości zwiększa ryzyko ataków przepełnienia buforu (przepełnienie) i ataki typu "odmowa usługi" (DoS) przez złośliwych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="0b1e5-280">Increasing the value increases the risk of buffer overrun (overflow) and Denial of Service (DoS) attacks by malicious users.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0b1e5-281">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="0b1e5-281">Additional resources</span></span>

* [<span data-ttu-id="0b1e5-282">Wymagania wstępne dotyczące programu .NET Core w systemie Linux</span><span class="sxs-lookup"><span data-stu-id="0b1e5-282">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* <xref:test/troubleshoot>
* <xref:host-and-deploy/proxy-load-balancer>
