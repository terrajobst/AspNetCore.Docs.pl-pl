---
title: Host platformy ASP.NET Core w systemie Linux z Apache
description: Dowiedz się, jak ustawić Apache jako zwrotny serwer proxy serwera na CentOS, aby przekierować ruch HTTP do aplikacji sieci web platformy ASP.NET Core systemem Kestrel.
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: b073f00469ada915244a2db71540fd7c971d55ea
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/24/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="bca4e-103">Host platformy ASP.NET Core w systemie Linux z Apache</span><span class="sxs-lookup"><span data-stu-id="bca4e-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="bca4e-104">Przez [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="bca4e-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="bca4e-105">Przy użyciu tego przewodnika, Dowiedz się, jak skonfigurować [Apache](https://httpd.apache.org/) jako zwrotny serwer proxy serwera na [CentOS 7](https://www.centos.org/) przekierowywanie ruchu HTTP do aplikacji sieci web platformy ASP.NET Core systemem [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="bca4e-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="bca4e-106">[Rozszerzenia mod_proxy](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) i powiązane moduły Utwórz zwrotny serwer proxy serwera.</span><span class="sxs-lookup"><span data-stu-id="bca4e-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bca4e-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="bca4e-107">Prerequisites</span></span>

1. <span data-ttu-id="bca4e-108">Serwer z systemem CentOS 7 przy użyciu konta użytkowników standardowych z uprawnieniem sudo.</span><span class="sxs-lookup"><span data-stu-id="bca4e-108">Server running CentOS 7 with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="bca4e-109">Zainstaluj środowisko uruchomieniowe .NET Core na serwerze.</span><span class="sxs-lookup"><span data-stu-id="bca4e-109">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="bca4e-110">Odwiedź stronę [.NET Core wszystkie pliki do pobrania strony](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="bca4e-110">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="bca4e-111">Wybierz z listy w obszarze najnowsze środowisko uruchomieniowe-preview **środowiska uruchomieniowego**.</span><span class="sxs-lookup"><span data-stu-id="bca4e-111">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="bca4e-112">Wybierz i postępuj zgodnie z instrukcjami dotyczącymi CentOS/Oracle.</span><span class="sxs-lookup"><span data-stu-id="bca4e-112">Select and follow the instructions for CentOS/Oracle.</span></span>
1. <span data-ttu-id="bca4e-113">Istniejącej aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bca4e-113">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="bca4e-114">Publikowanie i skopiuj przez aplikację</span><span class="sxs-lookup"><span data-stu-id="bca4e-114">Publish and copy over the app</span></span>

<span data-ttu-id="bca4e-115">Konfigurowanie aplikacji na potrzeby [wdrożenia zależne od framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="bca4e-115">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="bca4e-116">Uruchom [publikowania dotnet](/dotnet/core/tools/dotnet-publish) z Środowisko deweloperskie do tworzenia pakietów aplikacji w katalogu (na przykład *bin i wersji/&lt;target_framework_moniker&gt;/ publish*) który można Uruchom na serwerze:</span><span class="sxs-lookup"><span data-stu-id="bca4e-116">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="bca4e-117">Aplikacja również mogą być publikowane jako [niezależne wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd) nie, aby obsługa środowiska uruchomieniowego .NET Core na serwerze.</span><span class="sxs-lookup"><span data-stu-id="bca4e-117">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="bca4e-118">Skopiuj aplikacji platformy ASP.NET Core do serwera przy użyciu narzędzia, która integruje się z przepływu organizacji (na przykład punkt połączenia usługi, SFTP).</span><span class="sxs-lookup"><span data-stu-id="bca4e-118">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="bca4e-119">Często można znaleźć aplikacji sieci web w obszarze *var* katalog (na przykład *var/aspnetcore/hellomvc*).</span><span class="sxs-lookup"><span data-stu-id="bca4e-119">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="bca4e-120">W przypadku wdrożenia produkcyjnego ciągłej integracji przepływu pracy wykonuje pracę publikowania aplikacji i kopiowanie zasoby na serwerze.</span><span class="sxs-lookup"><span data-stu-id="bca4e-120">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

## <a name="configure-a-proxy-server"></a><span data-ttu-id="bca4e-121">Konfiguracja serwera proxy</span><span class="sxs-lookup"><span data-stu-id="bca4e-121">Configure a proxy server</span></span>

<span data-ttu-id="bca4e-122">Zwrotny serwer proxy jest typowe dla obsługi aplikacji sieci web dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="bca4e-122">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="bca4e-123">Zwrotny serwer proxy kończy żądanie HTTP i przekazuje je do aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bca4e-123">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="bca4e-124">Serwer proxy to taki, który przekazuje żądania klienta do innego serwera zamiast samego spełnienie żądania.</span><span class="sxs-lookup"><span data-stu-id="bca4e-124">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="bca4e-125">Zwrotny serwer proxy przekazuje do stałej miejsca docelowego, zwykle w imieniu dowolnego klientów.</span><span class="sxs-lookup"><span data-stu-id="bca4e-125">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="bca4e-126">W tym przewodniku Apache jest skonfigurowana jako zwrotny serwer proxy, uruchomione na tym samym serwerze, że Kestrel służy aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bca4e-126">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="bca4e-127">Ponieważ żądania są przekazywane przez zwrotny serwer proxy, należy używać oprogramowania pośredniczącego nagłówki przekazywane z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="bca4e-127">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="bca4e-128">Aktualizacje oprogramowania pośredniczącego `Request.Scheme`za pomocą `X-Forwarded-Proto` nagłówka, więc poprawne działanie tego przekierowania URI i innymi zasadami zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="bca4e-128">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="bca4e-129">Korzystając z dowolnego typu uwierzytelniania oprogramowania pośredniczącego, najpierw należy uruchomić oprogramowanie pośredniczące przekazane nagłówków.</span><span class="sxs-lookup"><span data-stu-id="bca4e-129">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="bca4e-130">Ta kolejność zapewnia, że oprogramowanie pośredniczące uwierzytelniania można używać wartości nagłówka i generowanie poprawne przekierowania URI.</span><span class="sxs-lookup"><span data-stu-id="bca4e-130">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

::: moniker range=">= aspnetcore-2.0"
> [!NOTE]
> <span data-ttu-id="bca4e-131">Albo konfiguracji&mdash;z lub bez zwrotnego serwera proxy&mdash;jest prawidłowa i obsługiwanych konfiguracji hostingu dla platformy ASP.NET Core w wersji 2.0 lub nowszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bca4e-131">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="bca4e-132">Aby uzyskać więcej informacji, zobacz [użycie Kestrel z zwrotny serwer proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="bca4e-132">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>
::: moniker-end

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bca4e-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bca4e-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="bca4e-134">Wywołanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metody w `Startup.Configure` przed wywołaniem [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) lub podobne oprogramowanie pośredniczące schematu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="bca4e-134">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="bca4e-135">Konfigurowanie oprogramowania pośredniczącego do przekazywania `X-Forwarded-For` i `X-Forwarded-Proto` nagłówki:</span><span class="sxs-lookup"><span data-stu-id="bca4e-135">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bca4e-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bca4e-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="bca4e-137">Wywołanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metody w `Startup.Configure` przed wywołaniem [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) i [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) lub podobne schemat uwierzytelniania oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="bca4e-137">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="bca4e-138">Konfigurowanie oprogramowania pośredniczącego do przekazywania `X-Forwarded-For` i `X-Forwarded-Proto` nagłówki:</span><span class="sxs-lookup"><span data-stu-id="bca4e-138">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="bca4e-139">Jeśli nie [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) są określone przez oprogramowanie pośredniczące są domyślne nagłówki do przekazywania `None`.</span><span class="sxs-lookup"><span data-stu-id="bca4e-139">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="bca4e-140">Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych serwerów proxy i moduły równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="bca4e-140">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="bca4e-141">Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="bca4e-141">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-apache"></a><span data-ttu-id="bca4e-142">Zainstaluj Apache</span><span class="sxs-lookup"><span data-stu-id="bca4e-142">Install Apache</span></span>

<span data-ttu-id="bca4e-143">Pakietów CentOS aktualizacji w celu ich najnowsze wersje stabilna:</span><span class="sxs-lookup"><span data-stu-id="bca4e-143">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="bca4e-144">Zainstaluj serwer sieci web Apache na CentOS za pomocą jednej `yum` polecenia:</span><span class="sxs-lookup"><span data-stu-id="bca4e-144">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="bca4e-145">Przykładowe dane wyjściowe po uruchomieniu polecenia:</span><span class="sxs-lookup"><span data-stu-id="bca4e-145">Sample output after running the command:</span></span>

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
> <span data-ttu-id="bca4e-146">W tym przykładzie danych wyjściowych odzwierciedla httpd.86_64, ponieważ wersja CentOS 7 jest 64-bitowym.</span><span class="sxs-lookup"><span data-stu-id="bca4e-146">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="bca4e-147">Aby sprawdzić, w którym zainstalowano Apache, uruchom `whereis httpd` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="bca4e-147">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="bca4e-148">Konfigurowanie Apache dla zwrotnego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="bca4e-148">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="bca4e-149">Pliki konfiguracji Apache znajdują się w `/etc/httpd/conf.d/` katalogu.</span><span class="sxs-lookup"><span data-stu-id="bca4e-149">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="bca4e-150">Dowolne pliki z *.conf* rozszerzenia są przetwarzane w kolejności alfabetycznej oprócz plików konfiguracji modułu w `/etc/httpd/conf.modules.d/`, który zawiera żadnej konfiguracji pliki niezbędne do ładowania modułów.</span><span class="sxs-lookup"><span data-stu-id="bca4e-150">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="bca4e-151">Utwórz plik konfiguracji o nazwie *hellomvc.conf*, dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="bca4e-151">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

```
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

<span data-ttu-id="bca4e-152">`VirtualHost` Bloku może pojawić się wiele razy w jeden lub więcej plików na serwerze.</span><span class="sxs-lookup"><span data-stu-id="bca4e-152">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="bca4e-153">W pliku konfiguracyjnym poprzedniego Apache akceptuje publicznego ruch na porcie 80.</span><span class="sxs-lookup"><span data-stu-id="bca4e-153">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="bca4e-154">Domena `www.example.com` obsługiwanej jest i `*.example.com` Usuwa alias do tej samej witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bca4e-154">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="bca4e-155">Zobacz [obsługi na podstawie nazwy hostów wirtualnych](https://httpd.apache.org/docs/current/vhosts/name-based.html) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="bca4e-155">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="bca4e-156">Żądania są przekazywane przez serwer proxy w katalogu głównym, aby port 5000 serwera na 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="bca4e-156">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="bca4e-157">Dla komunikacja dwukierunkowa `ProxyPass` i `ProxyPassReverse` są wymagane.</span><span class="sxs-lookup"><span data-stu-id="bca4e-157">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span>

> [!WARNING]
> <span data-ttu-id="bca4e-158">Błąd w celu określenia odpowiedniego [dyrektywy ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername) w **VirtualHost** blokowy przedstawia aplikacji luk w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="bca4e-158">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="bca4e-159">Powiązanie symbolu wieloznacznego domeny podrzędnej (na przykład `*.example.com`) nie stanowić to zagrożenie bezpieczeństwa, jeśli kontrolować domeny nadrzędnej cały (w przeciwieństwie do `*.com`, której występuje).</span><span class="sxs-lookup"><span data-stu-id="bca4e-159">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="bca4e-160">Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="bca4e-160">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="bca4e-161">Można skonfigurować rejestrowania `VirtualHost` przy użyciu `ErrorLog` i `CustomLog` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="bca4e-161">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="bca4e-162">`ErrorLog` Lokalizacja, w którym serwer rejestruje błędy, i `CustomLog` ustawia nazwę pliku i format pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="bca4e-162">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="bca4e-163">W tym przypadku jest to, gdzie jest rejestrowane informacje o żądaniu.</span><span class="sxs-lookup"><span data-stu-id="bca4e-163">In this case, this is where request information is logged.</span></span> <span data-ttu-id="bca4e-164">Istnieje jeden wiersz dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="bca4e-164">There's one line for each request.</span></span>

<span data-ttu-id="bca4e-165">Zapisz plik i Przetestuj konfigurację.</span><span class="sxs-lookup"><span data-stu-id="bca4e-165">Save the file and test the configuration.</span></span> <span data-ttu-id="bca4e-166">Jeśli wszystko przebiegnie pomyślnie, powinien być odpowiedzi `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="bca4e-166">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="bca4e-167">Uruchom ponownie Apache:</span><span class="sxs-lookup"><span data-stu-id="bca4e-167">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="bca4e-168">Monitorowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="bca4e-168">Monitoring the app</span></span>

<span data-ttu-id="bca4e-169">Apache jest teraz skonfigurowana do przekazywania żądań wysyłanych do `http://localhost:80` do aplikacji platformy ASP.NET Core systemem Kestrel na `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="bca4e-169">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="bca4e-170">Apache Konfigurowanie nie jest jednak do zarządzania procesem Kestrel.</span><span class="sxs-lookup"><span data-stu-id="bca4e-170">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="bca4e-171">Użyj *systemd* i utworzenie pliku usługi, aby uruchomić i monitorować podstawowej aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="bca4e-171">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="bca4e-172">*systemd* to system init zapewnia wiele zaawansowanych funkcji uruchamianie, zatrzymywanie oraz procesy zarządzania.</span><span class="sxs-lookup"><span data-stu-id="bca4e-172">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="bca4e-173">Tworzenie pliku usługi</span><span class="sxs-lookup"><span data-stu-id="bca4e-173">Create the service file</span></span>

<span data-ttu-id="bca4e-174">Tworzenie pliku definicji usługi:</span><span class="sxs-lookup"><span data-stu-id="bca4e-174">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="bca4e-175">Przykładowy plik usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="bca4e-175">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if dotnet service crashes
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> <span data-ttu-id="bca4e-176">**Użytkownik** &mdash; Jeśli użytkownik *apache* nie jest używany przez tę konfigurację, użytkownik musi najpierw utworzyć i podane odpowiednie własność plików.</span><span class="sxs-lookup"><span data-stu-id="bca4e-176">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

> [!NOTE]
> <span data-ttu-id="bca4e-177">Niektóre wartości (na przykład parametry połączenia SQL), należy użyć znaków ucieczki dla dostawców konfiguracji można odczytać zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="bca4e-177">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="bca4e-178">Użyj następującego polecenia, aby wygenerować prawidłowo zmienionym wartość do użycia w pliku konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="bca4e-178">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="bca4e-179">Zapisz plik i włączyć usługę:</span><span class="sxs-lookup"><span data-stu-id="bca4e-179">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="bca4e-180">Uruchom usługę i sprawdź, czy jest uruchomiona:</span><span class="sxs-lookup"><span data-stu-id="bca4e-180">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="bca4e-181">Przy użyciu zwrotnego serwera proxy skonfigurowane i Kestrel zarządzanych za pomocą *systemd*, aplikacji sieci web jest w pełni skonfigurowane i dostępne w przeglądarce na komputerze lokalnym w `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="bca4e-181">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="bca4e-182">Zapoznanie się nagłówki odpowiedzi **serwera** nagłówek wskazuje, że aplikacja platformy ASP.NET Core jest obsługiwana przez Kestrel:</span><span class="sxs-lookup"><span data-stu-id="bca4e-182">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="bca4e-183">Przeglądanie dzienników</span><span class="sxs-lookup"><span data-stu-id="bca4e-183">Viewing logs</span></span>

<span data-ttu-id="bca4e-184">Ponieważ aplikacja sieci web przy użyciu Kestrel odbywa się przy użyciu *systemd*, scentralizowane dziennika są rejestrowane zdarzenia i procesów.</span><span class="sxs-lookup"><span data-stu-id="bca4e-184">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="bca4e-185">Jednak ten dziennik zawiera wpisy dla wszystkich usług i procesów zarządza *systemd*.</span><span class="sxs-lookup"><span data-stu-id="bca4e-185">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="bca4e-186">Aby wyświetlić `kestrel-hellomvc.service`— określone elementy, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="bca4e-186">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="bca4e-187">Potrzeby filtrowania czasu, określ opcje czasu przy użyciu polecenia.</span><span class="sxs-lookup"><span data-stu-id="bca4e-187">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="bca4e-188">Na przykład użyć `--since today` filtrowania dla bieżącego dnia lub `--until 1 hour ago` wyświetlić wpisy poprzedniej godziny.</span><span class="sxs-lookup"><span data-stu-id="bca4e-188">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="bca4e-189">Aby uzyskać więcej informacji, zobacz [man stronę journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="bca4e-189">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="bca4e-190">Zabezpieczanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="bca4e-190">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="bca4e-191">Konfigurowanie zapory</span><span class="sxs-lookup"><span data-stu-id="bca4e-191">Configure firewall</span></span>

<span data-ttu-id="bca4e-192">*Firewalld* jest demon dynamiczne zarządzanie zaporą z obsługą stref sieci.</span><span class="sxs-lookup"><span data-stu-id="bca4e-192">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="bca4e-193">Porty i filtrowanie pakietów można nadal będą zarządzane przez iptables.</span><span class="sxs-lookup"><span data-stu-id="bca4e-193">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="bca4e-194">*Firewalld* powinny być instalowane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="bca4e-194">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="bca4e-195">`yum` może służyć do zainstalowania pakietu lub sprawdź, czy jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="bca4e-195">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="bca4e-196">Użyj `firewalld` można otworzyć tylko te porty, które są wymagane dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bca4e-196">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="bca4e-197">W takim przypadku portu 80 i 443 są używane.</span><span class="sxs-lookup"><span data-stu-id="bca4e-197">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="bca4e-198">Porty 80 i 443, aby otworzyć na stałe ustawiony program następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="bca4e-198">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="bca4e-199">Ponowne załadowanie ustawień zapory.</span><span class="sxs-lookup"><span data-stu-id="bca4e-199">Reload the firewall settings.</span></span> <span data-ttu-id="bca4e-200">Sprawdź dostępnych usług i portów w strefie domyślnej.</span><span class="sxs-lookup"><span data-stu-id="bca4e-200">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="bca4e-201">Opcje są dostępne, sprawdzając `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="bca4e-201">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="bca4e-202">Konfiguracja protokołu SSL</span><span class="sxs-lookup"><span data-stu-id="bca4e-202">SSL configuration</span></span>

<span data-ttu-id="bca4e-203">Aby skonfigurować Apache dla protokołu SSL, *mod_ssl* moduł jest używany.</span><span class="sxs-lookup"><span data-stu-id="bca4e-203">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="bca4e-204">Gdy *host z wieloma adresami* moduł został zainstalowany, *mod_ssl* modułu również został zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="bca4e-204">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="bca4e-205">Jeśli nie została zainstalowana za pomocą `yum` Aby dodać je do konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="bca4e-205">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="bca4e-206">Aby wymusić protokołu SSL, należy zainstalować `mod_rewrite` modułu, aby umożliwić ponowne zapisywanie adresów URL:</span><span class="sxs-lookup"><span data-stu-id="bca4e-206">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="bca4e-207">Modyfikowanie *hellomvc.conf* plik, aby umożliwić ponowne zapisywanie adresów URL i zabezpieczania komunikacji na porcie 443:</span><span class="sxs-lookup"><span data-stu-id="bca4e-207">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
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
> <span data-ttu-id="bca4e-208">W tym przykładzie używa certyfikatu, generowany lokalnie.</span><span class="sxs-lookup"><span data-stu-id="bca4e-208">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="bca4e-209">**SSLCertificateFile** powinien być plikiem podstawowego certyfikatu dla nazwy domeny.</span><span class="sxs-lookup"><span data-stu-id="bca4e-209">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="bca4e-210">**SSLCertificateKeyFile** powinny być pliku klucza generowane po utworzeniu renderowanie po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="bca4e-210">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="bca4e-211">**SSLCertificateChainFile** powinien być pliku certyfikatu pośredniego (jeśli istnieją) podane przez urząd certyfikacji.</span><span class="sxs-lookup"><span data-stu-id="bca4e-211">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="bca4e-212">Zapisz plik i testowanie konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="bca4e-212">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="bca4e-213">Uruchom ponownie Apache:</span><span class="sxs-lookup"><span data-stu-id="bca4e-213">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="bca4e-214">Dodatkowe sugestie Apache</span><span class="sxs-lookup"><span data-stu-id="bca4e-214">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="bca4e-215">Dodatkowych nagłówków</span><span class="sxs-lookup"><span data-stu-id="bca4e-215">Additional headers</span></span>

<span data-ttu-id="bca4e-216">Aby zabezpieczyć przed atakami, istnieje kilka nagłówki, które powinny być zmodyfikowany lub dodane.</span><span class="sxs-lookup"><span data-stu-id="bca4e-216">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="bca4e-217">Upewnij się, że `mod_headers` moduł jest zainstalowany:</span><span class="sxs-lookup"><span data-stu-id="bca4e-217">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="bca4e-218">Zabezpieczenia przed atakami porywaniu kliknięć Apache</span><span class="sxs-lookup"><span data-stu-id="bca4e-218">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="bca4e-219">[Porywaniu kliknięć](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), znanej także jako *interfejsu użytkownika odszkodowania ataku*, jest złośliwymi atakami, gdy obiekt odwiedzający witrynę sieci Web jest zachęceni do kliknięcia łącza lub przycisku na innej stronie, nie są one obecnie odwiedzający.</span><span class="sxs-lookup"><span data-stu-id="bca4e-219">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="bca4e-220">Użyj `X-FRAME-OPTIONS` do zabezpieczenia witryny.</span><span class="sxs-lookup"><span data-stu-id="bca4e-220">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="bca4e-221">Edytuj *httpd.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="bca4e-221">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="bca4e-222">Dodaj wiersz `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="bca4e-222">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="bca4e-223">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="bca4e-223">Save the file.</span></span> <span data-ttu-id="bca4e-224">Uruchom ponownie Apache.</span><span class="sxs-lookup"><span data-stu-id="bca4e-224">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="bca4e-225">Wykrywanie typ MIME</span><span class="sxs-lookup"><span data-stu-id="bca4e-225">MIME-type sniffing</span></span>

<span data-ttu-id="bca4e-226">`X-Content-Type-Options` Nagłówka uniemożliwia programowi Internet Explorer z *wykrywanie MIME* (Określanie pliku `Content-Type` z zawartości pliku).</span><span class="sxs-lookup"><span data-stu-id="bca4e-226">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="bca4e-227">Jeśli serwer ustawia `Content-Type` nagłówka do `text/html` z `nosniff` zestaw opcji, program Internet Explorer renderuje zawartość jako `text/html` niezależnie od zawartości pliku.</span><span class="sxs-lookup"><span data-stu-id="bca4e-227">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="bca4e-228">Edytuj *httpd.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="bca4e-228">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="bca4e-229">Dodaj wiersz `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="bca4e-229">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="bca4e-230">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="bca4e-230">Save the file.</span></span> <span data-ttu-id="bca4e-231">Uruchom ponownie Apache.</span><span class="sxs-lookup"><span data-stu-id="bca4e-231">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="bca4e-232">Równoważenie obciążenia</span><span class="sxs-lookup"><span data-stu-id="bca4e-232">Load Balancing</span></span> 

<span data-ttu-id="bca4e-233">W tym przykładzie przedstawiono sposób instalacji i konfiguracji Apache CentOS 7 i Kestrel na tym samym komputerze wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="bca4e-233">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="bca4e-234">Aby można było ma pojedynczego punktu awarii; przy użyciu *mod_proxy_balancer* i modyfikowanie **VirtualHost** umożliwiałyby zarządzania wiele wystąpień aplikacji sieci web za serwerem proxy Apache.</span><span class="sxs-lookup"><span data-stu-id="bca4e-234">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="bca4e-235">W pliku konfiguracyjnym pokazano poniżej, dodatkowego wystąpienia `hellomvc` aplikacji jest skonfigurowana do uruchamiania na porcie 5001.</span><span class="sxs-lookup"><span data-stu-id="bca4e-235">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="bca4e-236">*Proxy* sekcji ustawiono na potrzeby równoważenia obciążenia z konfiguracją modułu równoważenia z dwóch członków *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="bca4e-236">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
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

### <a name="rate-limits"></a><span data-ttu-id="bca4e-237">Limity szybkości</span><span class="sxs-lookup"><span data-stu-id="bca4e-237">Rate Limits</span></span>
<span data-ttu-id="bca4e-238">Przy użyciu *mod_ratelimit*, który znajduje się w *host z wieloma adresami* moduł, może być ograniczona przepustowość klientów:</span><span class="sxs-lookup"><span data-stu-id="bca4e-238">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="bca4e-239">Przykładowy plik ogranicza przepustowość jako 600 KB/s w lokalizacji głównej:</span><span class="sxs-lookup"><span data-stu-id="bca4e-239">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
