---
title: Host platformy ASP.NET Core w systemie Linux z Apache
description: Dowiedz się, jak skonfigurować przekierowywanie ruchu HTTP do aplikacji sieci web platformy ASP.NET Core uruchomionych na Kestrel Apache jako zwrotny serwer proxy serwera na CentOS.
author: spboyer
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
uid: host-and-deploy/linux-apache
ms.openlocfilehash: d02fbd82be37e6d67214a9a0bf5851662b577cb9
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433977"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="2adde-103">Host platformy ASP.NET Core w systemie Linux z Apache</span><span class="sxs-lookup"><span data-stu-id="2adde-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="2adde-104">Przez [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="2adde-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="2adde-105">Za pomocą tego przewodnika, Dowiedz się, jak skonfigurować [Apache](https://httpd.apache.org/) jako zwrotny serwer proxy serwera na [CentOS 7](https://www.centos.org/) przekierowywania ruchu HTTP do aplikacji sieci web platformy ASP.NET Core uruchomionej na [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="2adde-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="2adde-106">[Rozszerzenia mod_proxy](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) i powiązane moduły Utwórz zwrotny serwer proxy serwera.</span><span class="sxs-lookup"><span data-stu-id="2adde-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2adde-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="2adde-107">Prerequisites</span></span>

1. <span data-ttu-id="2adde-108">Serwer z systemem CentOS 7 przy użyciu konta użytkownika standardowego przy użyciu uprawnień "sudo".</span><span class="sxs-lookup"><span data-stu-id="2adde-108">Server running CentOS 7 with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="2adde-109">Na serwerze, należy zainstalować środowisko uruchomieniowe platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2adde-109">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="2adde-110">Odwiedź stronę [.NET Core wszystkie strony plików do pobrania](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="2adde-110">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="2adde-111">Z listy w obszarze wybierz najnowsze środowisko uruchomieniowe — wersja zapoznawcza **środowiska uruchomieniowego**.</span><span class="sxs-lookup"><span data-stu-id="2adde-111">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="2adde-112">Wybierz, a następnie postępuj zgodnie z instrukcjami na oprogramowanie Oracle, CentOS /.</span><span class="sxs-lookup"><span data-stu-id="2adde-112">Select and follow the instructions for CentOS/Oracle.</span></span>
1. <span data-ttu-id="2adde-113">Istniejącą aplikację ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2adde-113">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="2adde-114">Publikowanie i skopiuj aplikacji</span><span class="sxs-lookup"><span data-stu-id="2adde-114">Publish and copy over the app</span></span>

<span data-ttu-id="2adde-115">Konfigurowanie aplikacji na potrzeby [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="2adde-115">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="2adde-116">Uruchom [publikowania dotnet](/dotnet/core/tools/dotnet-publish) ze środowiska projektowego, aby utworzyć pakiet aplikacji do katalogu (na przykład *bin/wydawania/&lt;target_framework_moniker&gt;/ publish*), można Uruchom na serwerze:</span><span class="sxs-lookup"><span data-stu-id="2adde-116">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="2adde-117">Aplikację można także publikować jako [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd) Jeśli wolisz nie zachować środowisko uruchomieniowe platformy .NET Core na serwerze.</span><span class="sxs-lookup"><span data-stu-id="2adde-117">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="2adde-118">Skopiuj aplikacji ASP.NET Core na serwer przy użyciu narzędzia, która integruje się z przepływu pracy w organizacji, (na przykład punkt połączenia usługi, SFTP).</span><span class="sxs-lookup"><span data-stu-id="2adde-118">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="2adde-119">Często do lokalizowania aplikacji sieci web w obszarze *var* katalog (na przykład *aspnetcore/var/hellomvc*).</span><span class="sxs-lookup"><span data-stu-id="2adde-119">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="2adde-120">W przypadku wdrożenia produkcyjnego przepływu pracy ciągłej integracji działa publikowania aplikacji i kopiowanie zasobów do serwera.</span><span class="sxs-lookup"><span data-stu-id="2adde-120">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

## <a name="configure-a-proxy-server"></a><span data-ttu-id="2adde-121">Konfiguracja serwera proxy</span><span class="sxs-lookup"><span data-stu-id="2adde-121">Configure a proxy server</span></span>

<span data-ttu-id="2adde-122">Zwrotny serwer proxy jest wspólne dla aplikacji sieci web dynamicznego obsługująca.</span><span class="sxs-lookup"><span data-stu-id="2adde-122">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="2adde-123">Zwrotny serwer proxy kończy żądanie HTTP i przekazuje je do aplikacji platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2adde-123">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="2adde-124">Serwer proxy jest jedną, która przekazuje żądania klienta do innego serwera, a nie sam wypełniania żądań.</span><span class="sxs-lookup"><span data-stu-id="2adde-124">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="2adde-125">Zwrotny serwer proxy przekazuje do środka miejsca docelowego, zazwyczaj w imieniu dowolnego klientów.</span><span class="sxs-lookup"><span data-stu-id="2adde-125">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="2adde-126">W tym przewodniku Apache jest skonfigurowany jako zwrotny serwer proxy, uruchomione na tym samym serwerze, że Kestrel działa jako aplikacja platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2adde-126">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="2adde-127">Ponieważ żądania są przekazywane przez zwrotny serwer proxy, należy użyć [przekazywane oprogramowania pośredniczącego nagłówki](xref:host-and-deploy/proxy-load-balancer) z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="2adde-127">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="2adde-128">Aktualizacje oprogramowania pośredniczącego `Request.Scheme`przy użyciu `X-Forwarded-Proto` nagłówka, więc działanie tego identyfikatory URI przekierowań i innych zasad zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="2adde-128">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="2adde-129">Dowolny składnik, który jest zależny od systemu, takie jak uwierzytelnianie, generowanie konsolidacji, przekierowań i geolokalizacja, muszą być umieszczone po wywołaniu oprogramowanie pośredniczące przekazane nagłówków.</span><span class="sxs-lookup"><span data-stu-id="2adde-129">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="2adde-130">Zgodnie z ogólną zasadą przekazywane oprogramowania pośredniczącego nagłówki należy uruchomić przed innym oprogramowaniu pośredniczącym, z wyjątkiem diagnostyki i obsługi oprogramowania pośredniczącego błędów.</span><span class="sxs-lookup"><span data-stu-id="2adde-130">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="2adde-131">Ta kolejność gwarantuje, że oprogramowanie pośredniczące, opierając się na nagłówki przekazywane informacje mogą wykorzystywać wartości nagłówka do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="2adde-131">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

::: moniker range=">= aspnetcore-2.0"
> [!NOTE]
> <span data-ttu-id="2adde-132">Każda konfiguracja&mdash;z lub bez serwera proxy odwrotnej&mdash;jest prawidłowy i obsługiwanych konfiguracji hostingu dla platformy ASP.NET Core 2.0 lub nowszej aplikacje.</span><span class="sxs-lookup"><span data-stu-id="2adde-132">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="2adde-133">Aby uzyskać więcej informacji, zobacz [kiedy należy używać Kestrel przy użyciu zwrotnego serwera proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="2adde-133">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>
::: moniker-end

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2adde-134">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2adde-134">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2adde-135">Wywoływanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in Class metoda `Startup.Configure` przed wywołaniem [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) lub podobne oprogramowanie pośredniczące schematu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="2adde-135">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="2adde-136">Konfigurowanie oprogramowania pośredniczącego, aby przekazywać `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków:</span><span class="sxs-lookup"><span data-stu-id="2adde-136">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2adde-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2adde-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2adde-138">Wywoływanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in Class metoda `Startup.Configure` przed wywołaniem [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) i [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) lub podobne schematu uwierzytelniania oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="2adde-138">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="2adde-139">Konfigurowanie oprogramowania pośredniczącego, aby przekazywać `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków:</span><span class="sxs-lookup"><span data-stu-id="2adde-139">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

---

<span data-ttu-id="2adde-140">Jeśli nie [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) są określone oprogramowanie pośredniczące, są domyślne nagłówki do przekazywania `None`.</span><span class="sxs-lookup"><span data-stu-id="2adde-140">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="2adde-141">Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych za serwery proxy i moduły równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="2adde-141">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="2adde-142">Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="2adde-142">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-apache"></a><span data-ttu-id="2adde-143">Zainstaluj Apache</span><span class="sxs-lookup"><span data-stu-id="2adde-143">Install Apache</span></span>

<span data-ttu-id="2adde-144">Aktualizowanie pakietów CentOS do ich najnowszej stabilnej wersji:</span><span class="sxs-lookup"><span data-stu-id="2adde-144">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="2adde-145">Instalowanie serwera internetowego Apache na CentOS, za pomocą jednego `yum` polecenia:</span><span class="sxs-lookup"><span data-stu-id="2adde-145">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="2adde-146">Przykładowe dane wyjściowe po uruchomieniu polecenia:</span><span class="sxs-lookup"><span data-stu-id="2adde-146">Sample output after running the command:</span></span>

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
> <span data-ttu-id="2adde-147">W tym przykładzie dane wyjściowe odzwierciedla httpd.86_64, ponieważ wersja CentOS 7 jest 64-bitowych.</span><span class="sxs-lookup"><span data-stu-id="2adde-147">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="2adde-148">Aby sprawdzić, w którym zainstalowano Apache, uruchom `whereis httpd` z poziomu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="2adde-148">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache"></a><span data-ttu-id="2adde-149">Skonfigurowania serwera Apache</span><span class="sxs-lookup"><span data-stu-id="2adde-149">Configure Apache</span></span>

<span data-ttu-id="2adde-150">Pliki konfiguracji Apache znajdują się w `/etc/httpd/conf.d/` katalogu.</span><span class="sxs-lookup"><span data-stu-id="2adde-150">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="2adde-151">Każdy plik z *.conf* rozszerzenia są przetwarzane w kolejności alfabetycznej, oprócz plików konfiguracji modułu w `/etc/httpd/conf.modules.d/`, który zawiera żadnej konfiguracji pliki niezbędne do ładowania modułów.</span><span class="sxs-lookup"><span data-stu-id="2adde-151">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="2adde-152">Utwórz plik konfiguracji o nazwie *hellomvc.conf*, dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="2adde-152">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

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
    ErrorLog ${APACHE_LOG_DIR}hellomvc-error.log
    CustomLog ${APACHE_LOG_DIR}hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="2adde-153">`VirtualHost` Bloku mogą pojawić się wiele razy w jeden lub więcej plików na serwerze.</span><span class="sxs-lookup"><span data-stu-id="2adde-153">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="2adde-154">W poprzednim pliku konfiguracji Apache akceptuje publicznych ruch na porcie 80.</span><span class="sxs-lookup"><span data-stu-id="2adde-154">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="2adde-155">Domena `www.example.com` obsługiwanych danych, a `*.example.com` aliasu jest rozpoznawany jako ten sam witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="2adde-155">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="2adde-156">Zobacz [obsługi na podstawie nazwy hosta wirtualnego](https://httpd.apache.org/docs/current/vhosts/name-based.html) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="2adde-156">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="2adde-157">Żądania są przekierowywane w katalogu głównym na porcie 5000 serwer pod adresem 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="2adde-157">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="2adde-158">Do komunikacji dwukierunkowej `ProxyPass` i `ProxyPassReverse` są wymagane.</span><span class="sxs-lookup"><span data-stu-id="2adde-158">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span> <span data-ttu-id="2adde-159">Aby zmienić port adresu IP firmy Kestrel, zobacz [Kestrel: Konfiguracja punktu końcowego](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="2adde-159">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="2adde-160">Błąd, aby określić poprawną [dyrektywy ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername) w **VirtualHost** bloku ujawnia luki w zabezpieczeniach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2adde-160">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="2adde-161">Powiązanie symbol wieloznaczny domeny podrzędnej (na przykład `*.example.com`) nie stanowić to zagrożenie bezpieczeństwa, jeśli możesz kontrolować domenę nadrzędną całego (w przeciwieństwie do `*.com`, który jest narażony).</span><span class="sxs-lookup"><span data-stu-id="2adde-161">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="2adde-162">Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="2adde-162">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="2adde-163">Można skonfigurować rejestrowanie `VirtualHost` przy użyciu `ErrorLog` i `CustomLog` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="2adde-163">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="2adde-164">`ErrorLog` jest to lokalizacja, w których dzienniki błędów serwera i `CustomLog` ustawia nazwę pliku i format pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="2adde-164">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="2adde-165">W tym przypadku jest to, gdzie jest rejestrowane informacje o żądaniu.</span><span class="sxs-lookup"><span data-stu-id="2adde-165">In this case, this is where request information is logged.</span></span> <span data-ttu-id="2adde-166">Istnieje jeden wiersz dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="2adde-166">There's one line for each request.</span></span>

<span data-ttu-id="2adde-167">Zapisz plik i wykonaj test konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="2adde-167">Save the file and test the configuration.</span></span> <span data-ttu-id="2adde-168">Jeśli wszystko przebiegnie pomyślnie, odpowiedź powinna wyglądać `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="2adde-168">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="2adde-169">Uruchom ponownie usługę Apache:</span><span class="sxs-lookup"><span data-stu-id="2adde-169">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="2adde-170">Monitorowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="2adde-170">Monitoring the app</span></span>

<span data-ttu-id="2adde-171">Apache jest teraz Instalatora w celu przekazywania żądań kierowanych do `http://localhost:80` do aplikacji platformy ASP.NET Core uruchomionych na Kestrel na `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="2adde-171">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="2adde-172">Apache skonfigurować nie jest jednak do zarządzania procesem Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2adde-172">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="2adde-173">Użyj *systemd* i Utwórz plik usługi, aby uruchomić i monitorować podstawowej aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="2adde-173">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="2adde-174">*systemd* to system init, który zapewnia wiele funkcji zaawansowanych uruchamianie, zatrzymywanie oraz zarządzanie procesami.</span><span class="sxs-lookup"><span data-stu-id="2adde-174">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="2adde-175">Utwórz plik usługi</span><span class="sxs-lookup"><span data-stu-id="2adde-175">Create the service file</span></span>

<span data-ttu-id="2adde-176">Tworzenie pliku definicji usługi:</span><span class="sxs-lookup"><span data-stu-id="2adde-176">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="2adde-177">Przykładowy plik usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="2adde-177">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> <span data-ttu-id="2adde-178">**Użytkownik** &mdash; Jeśli użytkownik *apache* nie jest używany przez tę konfigurację, użytkownik musi najpierw utworzyć i biorąc pod uwagę odpowiednie prawa własności plików.</span><span class="sxs-lookup"><span data-stu-id="2adde-178">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

> [!NOTE]
> <span data-ttu-id="2adde-179">Niektóre wartości (na przykład parametry połączenia SQL), należy użyć znaków ucieczki dla dostawców konfiguracji można odczytać zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="2adde-179">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="2adde-180">Użyj następującego polecenia do generowania prawidłowo o zmienionym znaczeniu wartości do użycia w pliku konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="2adde-180">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="2adde-181">Zapisz plik i włączyć usługę:</span><span class="sxs-lookup"><span data-stu-id="2adde-181">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="2adde-182">Uruchom usługę i sprawdź, czy jest uruchomiona:</span><span class="sxs-lookup"><span data-stu-id="2adde-182">Start the service and verify that it's running:</span></span>

```bash
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="2adde-183">Przy użyciu zwrotnego serwera proxy, skonfigurowane i Kestrel zarządzane za pośrednictwem *systemd*, aplikacji sieci web jest w pełni skonfigurowane i dostępne za pomocą przeglądarki na komputerze lokalnym w `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="2adde-183">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="2adde-184">Inspekcja nagłówki odpowiedzi **serwera** nagłówek wskazuje, że aplikacja platformy ASP.NET Core jest obsługiwana przez Kestrel:</span><span class="sxs-lookup"><span data-stu-id="2adde-184">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="2adde-185">Wyświetlanie dzienników</span><span class="sxs-lookup"><span data-stu-id="2adde-185">Viewing logs</span></span>

<span data-ttu-id="2adde-186">Ponieważ aplikacja sieci web przy użyciu Kestrel odbywa się przy użyciu *systemd*, scentralizowane dziennika są rejestrowane zdarzenia i procesów.</span><span class="sxs-lookup"><span data-stu-id="2adde-186">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="2adde-187">Jednak ten dziennik zawiera wpisy dla wszystkich usług i procesów, które zarządza *systemd*.</span><span class="sxs-lookup"><span data-stu-id="2adde-187">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="2adde-188">Aby wyświetlić `kestrel-hellomvc.service`— określone elementy, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="2adde-188">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="2adde-189">Filtrowanie czasu, określ opcje czasu za pomocą polecenia.</span><span class="sxs-lookup"><span data-stu-id="2adde-189">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="2adde-190">Na przykład użyć `--since today` do filtrowania dla bieżącego dnia lub `--until 1 hour ago` Aby wyświetlić wpisy poprzedniej godziny.</span><span class="sxs-lookup"><span data-stu-id="2adde-190">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="2adde-191">Aby uzyskać więcej informacji, zobacz [man strona journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="2adde-191">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="2adde-192">Zabezpieczanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="2adde-192">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="2adde-193">Konfigurowanie zapory</span><span class="sxs-lookup"><span data-stu-id="2adde-193">Configure firewall</span></span>

<span data-ttu-id="2adde-194">*Firewalld* jest dynamiczna demona się zarządzać zaporą z obsługą stref sieci.</span><span class="sxs-lookup"><span data-stu-id="2adde-194">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="2adde-195">Porty i filtrowanie pakietów nadal mogą być zarządzane przez iptables.</span><span class="sxs-lookup"><span data-stu-id="2adde-195">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="2adde-196">*Firewalld* powinny być instalowane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="2adde-196">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="2adde-197">`yum` może służyć do zainstalowania pakietu lub upewnij się, że jest ona zainstalowana.</span><span class="sxs-lookup"><span data-stu-id="2adde-197">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="2adde-198">Użyj `firewalld` można otworzyć tylko te porty, które są potrzebne dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2adde-198">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="2adde-199">W tym przypadku port 80 i 443 są używane.</span><span class="sxs-lookup"><span data-stu-id="2adde-199">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="2adde-200">Porty 80 i 443, aby otworzyć można trwale ustawione w następujących poleceń:</span><span class="sxs-lookup"><span data-stu-id="2adde-200">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="2adde-201">Załaduj ponownie ustawienia zapory.</span><span class="sxs-lookup"><span data-stu-id="2adde-201">Reload the firewall settings.</span></span> <span data-ttu-id="2adde-202">Sprawdź dostępnych usług i portów w strefie domyślnej.</span><span class="sxs-lookup"><span data-stu-id="2adde-202">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="2adde-203">Opcje są dostępne, sprawdzając `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="2adde-203">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="2adde-204">Konfiguracja protokołu SSL</span><span class="sxs-lookup"><span data-stu-id="2adde-204">SSL configuration</span></span>

<span data-ttu-id="2adde-205">Do skonfigurowania serwera Apache dla protokołu SSL, *mod_ssl* moduł jest używany.</span><span class="sxs-lookup"><span data-stu-id="2adde-205">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="2adde-206">Gdy *host z wieloma adresami* moduł został zainstalowany, *mod_ssl* również został zainstalowany moduł.</span><span class="sxs-lookup"><span data-stu-id="2adde-206">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="2adde-207">Jeśli nie została zainstalowana za pomocą `yum` Aby dodać go do konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="2adde-207">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```

<span data-ttu-id="2adde-208">Aby Wymuszanie protokołu SSL, należy zainstalować `mod_rewrite` modułu, aby umożliwić ponownego zapisywania adresów URL:</span><span class="sxs-lookup"><span data-stu-id="2adde-208">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="2adde-209">Modyfikowanie *hellomvc.conf* plik, aby włączyć ponownego zapisywania adresów URL i zabezpieczają komunikację na porcie 443:</span><span class="sxs-lookup"><span data-stu-id="2adde-209">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> <span data-ttu-id="2adde-210">W tym przykładzie używa lokalnie wygenerowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="2adde-210">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="2adde-211">**SSLCertificateFile** powinien być plik certyfikatu podstawowego dla nazwy domeny.</span><span class="sxs-lookup"><span data-stu-id="2adde-211">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="2adde-212">**SSLCertificateKeyFile** powinny być plik klucza generowane podczas tworzenia żądania CSR.</span><span class="sxs-lookup"><span data-stu-id="2adde-212">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="2adde-213">**SSLCertificateChainFile** powinien być pliku pośredniego certyfikatu (jeśli istnieje) który został dostarczony przez urząd certyfikacji.</span><span class="sxs-lookup"><span data-stu-id="2adde-213">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="2adde-214">Zapisz plik i wykonaj test konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="2adde-214">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="2adde-215">Uruchom ponownie usługę Apache:</span><span class="sxs-lookup"><span data-stu-id="2adde-215">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="2adde-216">Dodatkowe sugestie Apache</span><span class="sxs-lookup"><span data-stu-id="2adde-216">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="2adde-217">Dodatkowe nagłówki</span><span class="sxs-lookup"><span data-stu-id="2adde-217">Additional headers</span></span>

<span data-ttu-id="2adde-218">Aby zabezpieczyć się przed złośliwymi atakami, istnieje kilka nagłówki, które powinny być zmodyfikowany lub dodane.</span><span class="sxs-lookup"><span data-stu-id="2adde-218">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="2adde-219">Upewnij się, że `mod_headers` zainstalowany moduł:</span><span class="sxs-lookup"><span data-stu-id="2adde-219">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="2adde-220">Zabezpieczanie Apache przed atakami porywaniu kliknięć</span><span class="sxs-lookup"><span data-stu-id="2adde-220">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="2adde-221">[Porywaniu kliknięć](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), znane również jako *interfejsu użytkownika odszkodowania ataku*, jest złośliwymi atakami, gdzie zwiódł już obiekt odwiedzający witrynę sieci Web do kliknięcia łącza lub przycisku na innej stronie nie są one obecnie odwiedzający.</span><span class="sxs-lookup"><span data-stu-id="2adde-221">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="2adde-222">Użyj `X-FRAME-OPTIONS` na zabezpieczenie witryny.</span><span class="sxs-lookup"><span data-stu-id="2adde-222">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="2adde-223">Edytuj *httpd.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="2adde-223">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="2adde-224">Dodaj wiersz `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="2adde-224">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="2adde-225">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="2adde-225">Save the file.</span></span> <span data-ttu-id="2adde-226">Uruchom ponownie Apache.</span><span class="sxs-lookup"><span data-stu-id="2adde-226">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="2adde-227">Wykrywanie typu MIME</span><span class="sxs-lookup"><span data-stu-id="2adde-227">MIME-type sniffing</span></span>

<span data-ttu-id="2adde-228">`X-Content-Type-Options` Nagłówka uniemożliwia programowi Internet Explorer z *wykrywanie MIME* (Określanie pliku `Content-Type` z zawartości pliku).</span><span class="sxs-lookup"><span data-stu-id="2adde-228">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="2adde-229">Jeśli serwer ustawia `Content-Type` nagłówka do `text/html` z `nosniff` zestaw opcji, program Internet Explorer renderuje zawartość jako `text/html` niezależnie od zawartości pliku.</span><span class="sxs-lookup"><span data-stu-id="2adde-229">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="2adde-230">Edytuj *httpd.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="2adde-230">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="2adde-231">Dodaj wiersz `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="2adde-231">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="2adde-232">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="2adde-232">Save the file.</span></span> <span data-ttu-id="2adde-233">Uruchom ponownie Apache.</span><span class="sxs-lookup"><span data-stu-id="2adde-233">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="2adde-234">Równoważenie obciążenia</span><span class="sxs-lookup"><span data-stu-id="2adde-234">Load Balancing</span></span>

<span data-ttu-id="2adde-235">W tym przykładzie pokazano, jak zainstalować i skonfigurować Apache CentOS 7 i Kestrel na tym samym komputerze wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="2adde-235">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="2adde-236">Aby można było ma pojedynczy punkt awarii; za pomocą *mod_proxy_balancer* i modyfikowanie **VirtualHost** pozwalają na zarządzanie wielu wystąpień funkcji web apps za serwerem proxy Apache.</span><span class="sxs-lookup"><span data-stu-id="2adde-236">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="2adde-237">W pliku konfiguracyjnym pokazano poniżej, dodatkowe wystąpienia `hellomvc` aplikacja jest skonfigurowana do uruchamiania na porcie 5001.</span><span class="sxs-lookup"><span data-stu-id="2adde-237">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="2adde-238">*Proxy* sekcji została ustawiona za pomocą konfiguracji usługi równoważenia, za pomocą dwóch członków do równoważenia obciążenia *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="2adde-238">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a><span data-ttu-id="2adde-239">Limity szybkości</span><span class="sxs-lookup"><span data-stu-id="2adde-239">Rate Limits</span></span>

<span data-ttu-id="2adde-240">Za pomocą *mod_ratelimit*, który znajduje się w *host z wieloma adresami* modułu, może być ograniczona przepustowość klientów:</span><span class="sxs-lookup"><span data-stu-id="2adde-240">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="2adde-241">Przykładowy plik ogranicza przepustowość jako 600 KB/s, w obszarze Katalog główny:</span><span class="sxs-lookup"><span data-stu-id="2adde-241">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

## <a name="additional-resources"></a><span data-ttu-id="2adde-242">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2adde-242">Additional resources</span></span>

* [<span data-ttu-id="2adde-243">Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="2adde-243">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
