---
title: Host platformy ASP.NET Core w systemie Linux z Apache
description: Dowiedz się, jak skonfigurować przekierowywanie ruchu HTTP do aplikacji sieci web platformy ASP.NET Core uruchomionych na Kestrel Apache jako zwrotny serwer proxy serwera na CentOS.
author: spboyer
ms.author: spboyer
ms.custom: mvc
ms.date: 10/09/2018
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 237646f839a4973074bb64176a024ebb3d32ee4e
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913011"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="3b9c8-103">Host platformy ASP.NET Core w systemie Linux z Apache</span><span class="sxs-lookup"><span data-stu-id="3b9c8-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="3b9c8-104">Przez [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="3b9c8-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="3b9c8-105">Za pomocą tego przewodnika, Dowiedz się, jak skonfigurować [Apache](https://httpd.apache.org/) jako zwrotny serwer proxy serwera na [CentOS 7](https://www.centos.org/) przekierowywania ruchu HTTP do aplikacji sieci web platformy ASP.NET Core uruchomionej na [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="3b9c8-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="3b9c8-106">[Rozszerzenia mod_proxy](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) i powiązane moduły Utwórz zwrotny serwer proxy serwera.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3b9c8-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="3b9c8-107">Prerequisites</span></span>

1. <span data-ttu-id="3b9c8-108">Serwer z systemem CentOS 7 przy użyciu konta użytkownika standardowego przy użyciu uprawnień "sudo".</span><span class="sxs-lookup"><span data-stu-id="3b9c8-108">Server running CentOS 7 with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="3b9c8-109">Na serwerze, należy zainstalować środowisko uruchomieniowe platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-109">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="3b9c8-110">Odwiedź stronę [.NET Core wszystkie strony plików do pobrania](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="3b9c8-110">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="3b9c8-111">Z listy w obszarze wybierz najnowsze środowisko uruchomieniowe — wersja zapoznawcza **środowiska uruchomieniowego**.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-111">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="3b9c8-112">Wybierz, a następnie postępuj zgodnie z instrukcjami na oprogramowanie Oracle, CentOS /.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-112">Select and follow the instructions for CentOS/Oracle.</span></span>
1. <span data-ttu-id="3b9c8-113">Istniejącą aplikację ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-113">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="3b9c8-114">Publikowanie i skopiuj aplikacji</span><span class="sxs-lookup"><span data-stu-id="3b9c8-114">Publish and copy over the app</span></span>

<span data-ttu-id="3b9c8-115">Konfigurowanie aplikacji na potrzeby [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="3b9c8-115">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="3b9c8-116">Uruchom [publikowania dotnet](/dotnet/core/tools/dotnet-publish) ze środowiska projektowego, aby utworzyć pakiet aplikacji do katalogu (na przykład *bin/wydawania/&lt;target_framework_moniker&gt;/ publish*), można Uruchom na serwerze:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-116">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="3b9c8-117">Aplikację można także publikować jako [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd) Jeśli wolisz nie zachować środowisko uruchomieniowe platformy .NET Core na serwerze.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-117">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="3b9c8-118">Skopiuj aplikacji ASP.NET Core na serwer przy użyciu narzędzia, która integruje się z przepływu pracy w organizacji, (na przykład punkt połączenia usługi, SFTP).</span><span class="sxs-lookup"><span data-stu-id="3b9c8-118">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="3b9c8-119">Często do lokalizowania aplikacji sieci web w obszarze *var* katalog (na przykład *www/var/helloapp*).</span><span class="sxs-lookup"><span data-stu-id="3b9c8-119">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="3b9c8-120">W przypadku wdrożenia produkcyjnego przepływu pracy ciągłej integracji działa publikowania aplikacji i kopiowanie zasobów do serwera.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-120">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

## <a name="configure-a-proxy-server"></a><span data-ttu-id="3b9c8-121">Konfiguracja serwera proxy</span><span class="sxs-lookup"><span data-stu-id="3b9c8-121">Configure a proxy server</span></span>

<span data-ttu-id="3b9c8-122">Zwrotny serwer proxy jest wspólne dla aplikacji sieci web dynamicznego obsługująca.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-122">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="3b9c8-123">Zwrotny serwer proxy kończy żądanie HTTP i przekazuje je do aplikacji platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-123">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="3b9c8-124">Serwer proxy jest jedną, która przekazuje żądania klienta do innego serwera, a nie sam wypełniania żądań.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-124">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="3b9c8-125">Zwrotny serwer proxy przekazuje do środka miejsca docelowego, zazwyczaj w imieniu dowolnego klientów.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-125">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="3b9c8-126">W tym przewodniku Apache jest skonfigurowany jako zwrotny serwer proxy, uruchomione na tym samym serwerze, że Kestrel działa jako aplikacja platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-126">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="3b9c8-127">Ponieważ żądania są przekazywane przez zwrotny serwer proxy, należy użyć [przekazywane oprogramowania pośredniczącego nagłówki](xref:host-and-deploy/proxy-load-balancer) z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-127">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="3b9c8-128">Aktualizacje oprogramowania pośredniczącego `Request.Scheme`przy użyciu `X-Forwarded-Proto` nagłówka, więc działanie tego identyfikatory URI przekierowań i innych zasad zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-128">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="3b9c8-129">Dowolny składnik, który jest zależny od systemu, takie jak uwierzytelnianie, generowanie konsolidacji, przekierowań i geolokalizacja, muszą być umieszczone po wywołaniu oprogramowanie pośredniczące przekazane nagłówków.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-129">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="3b9c8-130">Zgodnie z ogólną zasadą przekazywane oprogramowania pośredniczącego nagłówki należy uruchomić przed innym oprogramowaniu pośredniczącym, z wyjątkiem diagnostyki i obsługi oprogramowania pośredniczącego błędów.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-130">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="3b9c8-131">Ta kolejność gwarantuje, że oprogramowanie pośredniczące, opierając się na nagłówki przekazywane informacje mogą wykorzystywać wartości nagłówka do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-131">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="3b9c8-132">Każda konfiguracja&mdash;z lub bez serwera proxy odwrotnej&mdash;jest prawidłowy i obsługiwanych konfiguracji hostingu dla platformy ASP.NET Core 2.0 lub nowszej aplikacje.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-132">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="3b9c8-133">Aby uzyskać więcej informacji, zobacz [kiedy należy używać Kestrel przy użyciu zwrotnego serwera proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="3b9c8-133">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3b9c8-134">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3b9c8-134">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3b9c8-135">Wywoływanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in Class metoda `Startup.Configure` przed wywołaniem [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) lub podobne oprogramowanie pośredniczące schematu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-135">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="3b9c8-136">Konfigurowanie oprogramowania pośredniczącego, aby przekazywać `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-136">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3b9c8-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3b9c8-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3b9c8-138">Wywoływanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in Class metoda `Startup.Configure` przed wywołaniem [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) i [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) lub podobne schematu uwierzytelniania oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-138">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="3b9c8-139">Konfigurowanie oprogramowania pośredniczącego, aby przekazywać `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-139">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="3b9c8-140">Jeśli nie [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) są określone oprogramowanie pośredniczące, są domyślne nagłówki do przekazywania `None`.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-140">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="3b9c8-141">Tylko serwery proxy uruchomiona na hoście lokalnym (127.0.0.1, [:: 1]) są zaufane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-141">Only proxies running on localhost (127.0.0.1, [::1]) are trusted by default.</span></span> <span data-ttu-id="3b9c8-142">Jeśli innych zaufanych serwerów proxy lub sieci w obrębie organizacji uchwyt żądania między Internetem a serwer sieci web Dodaj je do listy <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> lub <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> z <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-142">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="3b9c8-143">Poniższy przykład dodaje serwer proxy zaufanych pod adresem IP 10.0.0.100 z oprogramowaniem pośredniczącym nagłówki przekazywane `KnownProxies` w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-143">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="3b9c8-144">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-144">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-apache"></a><span data-ttu-id="3b9c8-145">Zainstaluj Apache</span><span class="sxs-lookup"><span data-stu-id="3b9c8-145">Install Apache</span></span>

<span data-ttu-id="3b9c8-146">Aktualizowanie pakietów CentOS do ich najnowszej stabilnej wersji:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-146">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="3b9c8-147">Instalowanie serwera internetowego Apache na CentOS, za pomocą jednego `yum` polecenia:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-147">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="3b9c8-148">Przykładowe dane wyjściowe po uruchomieniu polecenia:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-148">Sample output after running the command:</span></span>

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
> <span data-ttu-id="3b9c8-149">W tym przykładzie dane wyjściowe odzwierciedla httpd.86_64, ponieważ wersja CentOS 7 jest 64-bitowych.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-149">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="3b9c8-150">Aby sprawdzić, w którym zainstalowano Apache, uruchom `whereis httpd` z poziomu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-150">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache"></a><span data-ttu-id="3b9c8-151">Skonfigurowania serwera Apache</span><span class="sxs-lookup"><span data-stu-id="3b9c8-151">Configure Apache</span></span>

<span data-ttu-id="3b9c8-152">Pliki konfiguracji Apache znajdują się w `/etc/httpd/conf.d/` katalogu.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-152">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="3b9c8-153">Każdy plik z *.conf* rozszerzenia są przetwarzane w kolejności alfabetycznej, oprócz plików konfiguracji modułu w `/etc/httpd/conf.modules.d/`, który zawiera żadnej konfiguracji pliki niezbędne do ładowania modułów.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-153">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="3b9c8-154">Utwórz plik konfiguracji o nazwie *helloapp.conf*, dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-154">Create a configuration file, named *helloapp.conf*, for the app:</span></span>

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

<span data-ttu-id="3b9c8-155">`VirtualHost` Bloku mogą pojawić się wiele razy w jeden lub więcej plików na serwerze.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-155">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="3b9c8-156">W poprzednim pliku konfiguracji Apache akceptuje publicznych ruch na porcie 80.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-156">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="3b9c8-157">Domena `www.example.com` obsługiwanych danych, a `*.example.com` aliasu jest rozpoznawany jako ten sam witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-157">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="3b9c8-158">Zobacz [obsługi na podstawie nazwy hosta wirtualnego](https://httpd.apache.org/docs/current/vhosts/name-based.html) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-158">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="3b9c8-159">Żądania są przekierowywane w katalogu głównym na porcie 5000 serwer pod adresem 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-159">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="3b9c8-160">Do komunikacji dwukierunkowej `ProxyPass` i `ProxyPassReverse` są wymagane.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-160">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span> <span data-ttu-id="3b9c8-161">Aby zmienić port adresu IP firmy Kestrel, zobacz [Kestrel: Konfiguracja punktu końcowego](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="3b9c8-161">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="3b9c8-162">Błąd, aby określić poprawną [dyrektywy ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername) w **VirtualHost** bloku ujawnia luki w zabezpieczeniach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-162">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="3b9c8-163">Powiązanie symbol wieloznaczny domeny podrzędnej (na przykład `*.example.com`) nie stanowić to zagrożenie bezpieczeństwa, jeśli możesz kontrolować domenę nadrzędną całego (w przeciwieństwie do `*.com`, który jest narażony).</span><span class="sxs-lookup"><span data-stu-id="3b9c8-163">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="3b9c8-164">Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-164">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="3b9c8-165">Można skonfigurować rejestrowanie `VirtualHost` przy użyciu `ErrorLog` i `CustomLog` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-165">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="3b9c8-166">`ErrorLog` jest to lokalizacja, w których dzienniki błędów serwera i `CustomLog` ustawia nazwę pliku i format pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-166">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="3b9c8-167">W tym przypadku jest to, gdzie jest rejestrowane informacje o żądaniu.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-167">In this case, this is where request information is logged.</span></span> <span data-ttu-id="3b9c8-168">Istnieje jeden wiersz dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-168">There's one line for each request.</span></span>

<span data-ttu-id="3b9c8-169">Zapisz plik i wykonaj test konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-169">Save the file and test the configuration.</span></span> <span data-ttu-id="3b9c8-170">Jeśli wszystko przebiegnie pomyślnie, odpowiedź powinna wyglądać `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-170">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="3b9c8-171">Uruchom ponownie usługę Apache:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-171">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="3b9c8-172">Monitorowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="3b9c8-172">Monitoring the app</span></span>

<span data-ttu-id="3b9c8-173">Apache jest teraz Instalatora w celu przekazywania żądań kierowanych do `http://localhost:80` do aplikacji platformy ASP.NET Core uruchomionych na Kestrel na `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-173">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="3b9c8-174">Apache skonfigurować nie jest jednak do zarządzania procesem Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-174">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="3b9c8-175">Użyj *systemd* i Utwórz plik usługi, aby uruchomić i monitorować podstawowej aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-175">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="3b9c8-176">*systemd* to system init, który zapewnia wiele funkcji zaawansowanych uruchamianie, zatrzymywanie oraz zarządzanie procesami.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-176">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="3b9c8-177">Utwórz plik usługi</span><span class="sxs-lookup"><span data-stu-id="3b9c8-177">Create the service file</span></span>

<span data-ttu-id="3b9c8-178">Tworzenie pliku definicji usługi:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-178">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="3b9c8-179">Przykładowy plik usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-179">An example service file for the app:</span></span>

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

<span data-ttu-id="3b9c8-180">Jeśli użytkownik *apache* nie jest używany przez tę konfigurację, użytkownik musi najpierw utworzyć i biorąc pod uwagę odpowiednie własności plików.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-180">If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership of files.</span></span>

<span data-ttu-id="3b9c8-181">Użyj `TimeoutStopSec` skonfigurować czas oczekiwania na aplikację, aby zamknięty po odebraniu sygnału przerwania początkowej.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-181">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="3b9c8-182">Jeśli aplikacja nie zamknięty w tym okresie, aby zakończyć aplikację zgłaszany jest SIGKILL.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-182">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="3b9c8-183">Podaj wartość jako unitless sekund (na przykład `150`), czas span wartości (na przykład `2min 30s`), lub `infinity` wyłączyć limit czasu.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-183">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="3b9c8-184">`TimeoutStopSec` Wartość domyślna to wartość `DefaultTimeoutStopSec` w pliku konfiguracji Menedżera (*systemd system.conf*, *system.conf.d*, *systemd user.conf*,  *User.conf.d*).</span><span class="sxs-lookup"><span data-stu-id="3b9c8-184">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="3b9c8-185">Domyślna wartość limitu czasu dla większości dystrybucji wynosi 90 s.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-185">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="3b9c8-186">Niektóre wartości (na przykład parametry połączenia SQL), należy użyć znaków ucieczki dla dostawców konfiguracji można odczytać zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-186">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="3b9c8-187">Użyj następującego polecenia do generowania prawidłowo o zmienionym znaczeniu wartości do użycia w pliku konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-187">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

<span data-ttu-id="3b9c8-188">Zapisz plik i włączyć usługę:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-188">Save the file and enable the service:</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="3b9c8-189">Uruchom usługę i sprawdź, czy jest uruchomiona:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-189">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="3b9c8-190">Przy użyciu zwrotnego serwera proxy, skonfigurowane i Kestrel zarządzane za pośrednictwem *systemd*, aplikacji sieci web jest w pełni skonfigurowane i dostępne za pomocą przeglądarki na komputerze lokalnym w `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-190">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="3b9c8-191">Inspekcja nagłówki odpowiedzi **serwera** nagłówek wskazuje, że aplikacja platformy ASP.NET Core jest obsługiwana przez Kestrel:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-191">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="3b9c8-192">Wyświetlanie dzienników</span><span class="sxs-lookup"><span data-stu-id="3b9c8-192">Viewing logs</span></span>

<span data-ttu-id="3b9c8-193">Ponieważ aplikacja sieci web przy użyciu Kestrel odbywa się przy użyciu *systemd*, scentralizowane dziennika są rejestrowane zdarzenia i procesów.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-193">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="3b9c8-194">Jednak ten dziennik zawiera wpisy dla wszystkich usług i procesów, które zarządza *systemd*.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-194">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="3b9c8-195">Aby wyświetlić `kestrel-helloapp.service`— określone elementy, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-195">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="3b9c8-196">Filtrowanie czasu, określ opcje czasu za pomocą polecenia.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-196">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="3b9c8-197">Na przykład użyć `--since today` do filtrowania dla bieżącego dnia lub `--until 1 hour ago` Aby wyświetlić wpisy poprzedniej godziny.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-197">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="3b9c8-198">Aby uzyskać więcej informacji, zobacz [man strona journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="3b9c8-198">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="3b9c8-199">Ochrona danych</span><span class="sxs-lookup"><span data-stu-id="3b9c8-199">Data protection</span></span>

<span data-ttu-id="3b9c8-200">[Stosu ochrony danych programu ASP.NET Core](xref:security/data-protection/index) jest używana przez kilka platformy ASP.NET Core [middlewares](xref:fundamentals/middleware/index), w tym oprogramowania pośredniczącego uwierzytelniania (na przykład, oprogramowaniu pośredniczącym pliku cookie) i fałszerstwo żądania międzywitrynowego (CSRF) zabezpieczenia.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-200">The [ASP.NET Core Data Protection stack](xref:security/data-protection/index) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="3b9c8-201">Nawet wtedy, gdy interfejsów API ochrony danych nie są wywoływane przez kod użytkownika, ochrony danych należy skonfigurować tak, aby utworzyć trwałe kryptograficznych [magazynu kluczy](xref:security/data-protection/implementation/key-management).</span><span class="sxs-lookup"><span data-stu-id="3b9c8-201">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="3b9c8-202">Jeśli nie jest skonfigurowana ochrona danych, klucze są przechowywane w pamięci i odrzucone po ponownym uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-202">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="3b9c8-203">Jeśli pierścień klucz jest przechowywany w pamięci, po ponownym uruchomieniu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-203">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="3b9c8-204">Wszystkie tokeny na podstawie plików cookie uwierzytelniania są unieważniane.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-204">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="3b9c8-205">Użytkownicy muszą ponownie zaloguj się na ich następnego żądania.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-205">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="3b9c8-206">Wszystkie dane chronione za pomocą pierścień klucz może już nie mogły być odszyfrowane.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-206">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="3b9c8-207">Może to obejmować [tokenów CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) i [plików cookie programu ASP.NET Core MVC TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="3b9c8-207">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="3b9c8-208">Aby skonfigurować ochronę danych na zostaną zachowane, a pierścień klucz szyfrowania, zobacz:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-208">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="securing-the-app"></a><span data-ttu-id="3b9c8-209">Zabezpieczanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="3b9c8-209">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="3b9c8-210">Konfigurowanie zapory</span><span class="sxs-lookup"><span data-stu-id="3b9c8-210">Configure firewall</span></span>

<span data-ttu-id="3b9c8-211">*Firewalld* jest dynamiczna demona się zarządzać zaporą z obsługą stref sieci.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-211">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="3b9c8-212">Porty i filtrowanie pakietów nadal mogą być zarządzane przez iptables.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-212">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="3b9c8-213">*Firewalld* powinny być instalowane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-213">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="3b9c8-214">`yum` może służyć do zainstalowania pakietu lub upewnij się, że jest ona zainstalowana.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-214">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="3b9c8-215">Użyj `firewalld` można otworzyć tylko te porty, które są potrzebne dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-215">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="3b9c8-216">W tym przypadku port 80 i 443 są używane.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-216">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="3b9c8-217">Porty 80 i 443, aby otworzyć można trwale ustawione w następujących poleceń:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-217">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="3b9c8-218">Załaduj ponownie ustawienia zapory.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-218">Reload the firewall settings.</span></span> <span data-ttu-id="3b9c8-219">Sprawdź dostępnych usług i portów w strefie domyślnej.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-219">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="3b9c8-220">Opcje są dostępne, sprawdzając `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-220">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="3b9c8-221">Konfiguracja protokołu SSL</span><span class="sxs-lookup"><span data-stu-id="3b9c8-221">SSL configuration</span></span>

<span data-ttu-id="3b9c8-222">Do skonfigurowania serwera Apache dla protokołu SSL, *mod_ssl* moduł jest używany.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-222">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="3b9c8-223">Gdy *host z wieloma adresami* moduł został zainstalowany, *mod_ssl* również został zainstalowany moduł.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-223">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="3b9c8-224">Jeśli nie została zainstalowana za pomocą `yum` Aby dodać go do konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-224">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```

<span data-ttu-id="3b9c8-225">Aby Wymuszanie protokołu SSL, należy zainstalować `mod_rewrite` modułu, aby umożliwić ponownego zapisywania adresów URL:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-225">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="3b9c8-226">Modyfikowanie *helloapp.conf* plik, aby włączyć ponownego zapisywania adresów URL i zabezpieczają komunikację na porcie 443:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-226">Modify the *helloapp.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="3b9c8-227">W tym przykładzie używa lokalnie wygenerowany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-227">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="3b9c8-228">**SSLCertificateFile** powinien być plik certyfikatu podstawowego dla nazwy domeny.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-228">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="3b9c8-229">**SSLCertificateKeyFile** powinny być plik klucza generowane podczas tworzenia żądania CSR.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-229">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="3b9c8-230">**SSLCertificateChainFile** powinien być pliku pośredniego certyfikatu (jeśli istnieje) który został dostarczony przez urząd certyfikacji.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-230">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="3b9c8-231">Zapisz plik i wykonaj test konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-231">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="3b9c8-232">Uruchom ponownie usługę Apache:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-232">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="3b9c8-233">Dodatkowe sugestie Apache</span><span class="sxs-lookup"><span data-stu-id="3b9c8-233">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="3b9c8-234">Dodatkowe nagłówki</span><span class="sxs-lookup"><span data-stu-id="3b9c8-234">Additional headers</span></span>

<span data-ttu-id="3b9c8-235">Aby zabezpieczyć się przed złośliwymi atakami, istnieje kilka nagłówki, które powinny być zmodyfikowany lub dodane.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-235">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="3b9c8-236">Upewnij się, że `mod_headers` zainstalowany moduł:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-236">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="3b9c8-237">Zabezpieczanie Apache przed atakami porywaniu kliknięć</span><span class="sxs-lookup"><span data-stu-id="3b9c8-237">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="3b9c8-238">[Porywaniu kliknięć](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), znane również jako *interfejsu użytkownika odszkodowania ataku*, jest złośliwymi atakami, gdzie zwiódł już obiekt odwiedzający witrynę sieci Web do kliknięcia łącza lub przycisku na innej stronie nie są one obecnie odwiedzający.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-238">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="3b9c8-239">Użyj `X-FRAME-OPTIONS` na zabezpieczenie witryny.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-239">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="3b9c8-240">Edytuj *httpd.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-240">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="3b9c8-241">Dodaj wiersz `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-241">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="3b9c8-242">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-242">Save the file.</span></span> <span data-ttu-id="3b9c8-243">Uruchom ponownie Apache.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-243">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="3b9c8-244">Wykrywanie typu MIME</span><span class="sxs-lookup"><span data-stu-id="3b9c8-244">MIME-type sniffing</span></span>

<span data-ttu-id="3b9c8-245">`X-Content-Type-Options` Nagłówka uniemożliwia programowi Internet Explorer z *wykrywanie MIME* (Określanie pliku `Content-Type` z zawartości pliku).</span><span class="sxs-lookup"><span data-stu-id="3b9c8-245">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="3b9c8-246">Jeśli serwer ustawia `Content-Type` nagłówka do `text/html` z `nosniff` zestaw opcji, program Internet Explorer renderuje zawartość jako `text/html` niezależnie od zawartości pliku.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-246">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="3b9c8-247">Edytuj *httpd.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-247">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="3b9c8-248">Dodaj wiersz `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-248">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="3b9c8-249">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-249">Save the file.</span></span> <span data-ttu-id="3b9c8-250">Uruchom ponownie Apache.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-250">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="3b9c8-251">Równoważenie obciążenia</span><span class="sxs-lookup"><span data-stu-id="3b9c8-251">Load Balancing</span></span>

<span data-ttu-id="3b9c8-252">W tym przykładzie pokazano, jak zainstalować i skonfigurować Apache CentOS 7 i Kestrel na tym samym komputerze wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-252">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="3b9c8-253">Aby można było ma pojedynczy punkt awarii; za pomocą *mod_proxy_balancer* i modyfikowanie **VirtualHost** pozwalają na zarządzanie wielu wystąpień funkcji web apps za serwerem proxy Apache.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-253">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="3b9c8-254">W pliku konfiguracyjnym pokazano poniżej, dodatkowe wystąpienia `helloapp` zdefiniowano działają na portu 5001.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-254">In the configuration file shown below, an additional instance of the `helloapp` is set up to run on port 5001.</span></span> <span data-ttu-id="3b9c8-255">*Proxy* sekcji została ustawiona za pomocą konfiguracji usługi równoważenia, za pomocą dwóch członków do równoważenia obciążenia *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="3b9c8-255">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="3b9c8-256">Limity szybkości</span><span class="sxs-lookup"><span data-stu-id="3b9c8-256">Rate Limits</span></span>

<span data-ttu-id="3b9c8-257">Za pomocą *mod_ratelimit*, który znajduje się w *host z wieloma adresami* modułu, może być ograniczona przepustowość klientów:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-257">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="3b9c8-258">Przykładowy plik ogranicza przepustowość jako 600 KB/s, w obszarze Katalog główny:</span><span class="sxs-lookup"><span data-stu-id="3b9c8-258">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

## <a name="additional-resources"></a><span data-ttu-id="3b9c8-259">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3b9c8-259">Additional resources</span></span>

* [<span data-ttu-id="3b9c8-260">Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="3b9c8-260">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
