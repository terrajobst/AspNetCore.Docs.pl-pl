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
ms.openlocfilehash: 5a8a035ff3f127d01655888d4f83a871645b0bf5
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/30/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="b1279-103">Host platformy ASP.NET Core w systemie Linux z Apache</span><span class="sxs-lookup"><span data-stu-id="b1279-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="b1279-104">Przez [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="b1279-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="b1279-105">Przy użyciu tego przewodnika, Dowiedz się, jak skonfigurować [Apache](https://httpd.apache.org/) jako zwrotny serwer proxy serwera na [CentOS 7](https://www.centos.org/) przekierowywanie ruchu HTTP do aplikacji sieci web platformy ASP.NET Core systemem [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="b1279-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="b1279-106">[Rozszerzenia mod_proxy](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) i powiązane moduły Utwórz zwrotny serwer proxy serwera.</span><span class="sxs-lookup"><span data-stu-id="b1279-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b1279-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b1279-107">Prerequisites</span></span>

1. <span data-ttu-id="b1279-108">Serwer z systemem CentOS 7 przy użyciu konta użytkowników standardowych z uprawnieniami sudo</span><span class="sxs-lookup"><span data-stu-id="b1279-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="b1279-109">Aplikacja platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b1279-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="b1279-110">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="b1279-110">Publish the app</span></span>

<span data-ttu-id="b1279-111">Publikowanie aplikacji jako [niezależne wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd) w konfiguracji wersji środowiska uruchomieniowego CentOS 7 (`centos.7-x64`).</span><span class="sxs-lookup"><span data-stu-id="b1279-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="b1279-112">Skopiuj zawartość *bin/Release/netcoreapp2.0/centos.7-x64/publish* folderu na serwerze przy użyciu połączenia, FTP lub innej metody transferu plików.</span><span class="sxs-lookup"><span data-stu-id="b1279-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="b1279-113">W przypadku wdrożenia produkcyjnego ciągłej integracji przepływu pracy wykonuje pracę publikowania aplikacji i kopiowanie zasoby na serwerze.</span><span class="sxs-lookup"><span data-stu-id="b1279-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="b1279-114">Konfiguracja serwera proxy</span><span class="sxs-lookup"><span data-stu-id="b1279-114">Configure a proxy server</span></span>

<span data-ttu-id="b1279-115">Zwrotny serwer proxy jest typowe dla obsługi aplikacji sieci web dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="b1279-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="b1279-116">Zwrotny serwer proxy kończy żądanie HTTP i przekazuje je do aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b1279-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="b1279-117">Serwer proxy to taki, który przekazuje żądania klienta do innego serwera zamiast samego spełnienie żądania.</span><span class="sxs-lookup"><span data-stu-id="b1279-117">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="b1279-118">Zwrotny serwer proxy przekazuje do stałej miejsca docelowego, zwykle w imieniu dowolnego klientów.</span><span class="sxs-lookup"><span data-stu-id="b1279-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="b1279-119">W tym przewodniku Apache jest skonfigurowana jako zwrotny serwer proxy, uruchomione na tym samym serwerze, że Kestrel służy aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1279-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="b1279-120">Ponieważ żądania są przekazywane przez zwrotny serwer proxy, należy używać oprogramowania pośredniczącego nagłówki przekazywane z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="b1279-120">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="b1279-121">Aktualizacje oprogramowania pośredniczącego `Request.Scheme`za pomocą `X-Forwarded-Proto` nagłówka, więc poprawne działanie tego przekierowania URI i innymi zasadami zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="b1279-121">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="b1279-122">Korzystając z dowolnego typu uwierzytelniania oprogramowania pośredniczącego, najpierw należy uruchomić oprogramowanie pośredniczące przekazane nagłówków.</span><span class="sxs-lookup"><span data-stu-id="b1279-122">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="b1279-123">Ta kolejność zapewnia, że oprogramowanie pośredniczące uwierzytelniania można używać wartości nagłówka i generowanie poprawne przekierowania URI.</span><span class="sxs-lookup"><span data-stu-id="b1279-123">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b1279-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b1279-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b1279-125">Wywołanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metody w `Startup.Configure` przed wywołaniem [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) lub podobne oprogramowanie pośredniczące schematu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="b1279-125">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="b1279-126">Konfigurowanie oprogramowania pośredniczącego do przekazywania `X-Forwarded-For` i `X-Forwarded-Proto` nagłówki:</span><span class="sxs-lookup"><span data-stu-id="b1279-126">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b1279-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b1279-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b1279-128">Wywołanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metody w `Startup.Configure` przed wywołaniem [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) i [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) lub podobne schemat uwierzytelniania oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="b1279-128">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="b1279-129">Konfigurowanie oprogramowania pośredniczącego do przekazywania `X-Forwarded-For` i `X-Forwarded-Proto` nagłówki:</span><span class="sxs-lookup"><span data-stu-id="b1279-129">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="b1279-130">Jeśli nie [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) są określone przez oprogramowanie pośredniczące są domyślne nagłówki do przekazywania `None`.</span><span class="sxs-lookup"><span data-stu-id="b1279-130">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="b1279-131">Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych serwerów proxy i moduły równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="b1279-131">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="b1279-132">Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="b1279-132">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-apache"></a><span data-ttu-id="b1279-133">Zainstaluj Apache</span><span class="sxs-lookup"><span data-stu-id="b1279-133">Install Apache</span></span>

<span data-ttu-id="b1279-134">Pakietów CentOS aktualizacji w celu ich najnowsze wersje stabilna:</span><span class="sxs-lookup"><span data-stu-id="b1279-134">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="b1279-135">Zainstaluj serwer sieci web Apache na CentOS za pomocą jednej `yum` polecenia:</span><span class="sxs-lookup"><span data-stu-id="b1279-135">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="b1279-136">Przykładowe dane wyjściowe po uruchomieniu polecenia:</span><span class="sxs-lookup"><span data-stu-id="b1279-136">Sample output after running the command:</span></span>

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
> <span data-ttu-id="b1279-137">W tym przykładzie danych wyjściowych odzwierciedla httpd.86_64, ponieważ wersja CentOS 7 jest 64-bitowym.</span><span class="sxs-lookup"><span data-stu-id="b1279-137">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="b1279-138">Aby sprawdzić, w którym zainstalowano Apache, uruchom `whereis httpd` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="b1279-138">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="b1279-139">Konfigurowanie Apache dla zwrotnego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="b1279-139">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="b1279-140">Pliki konfiguracji Apache znajdują się w `/etc/httpd/conf.d/` katalogu.</span><span class="sxs-lookup"><span data-stu-id="b1279-140">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="b1279-141">Dowolne pliki z *.conf* rozszerzenia są przetwarzane w kolejności alfabetycznej oprócz plików konfiguracji modułu w `/etc/httpd/conf.modules.d/`, który zawiera żadnej konfiguracji pliki niezbędne do ładowania modułów.</span><span class="sxs-lookup"><span data-stu-id="b1279-141">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="b1279-142">Utwórz plik konfiguracji o nazwie *hellomvc.conf*, dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="b1279-142">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

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

<span data-ttu-id="b1279-143">`VirtualHost` Bloku może pojawić się wiele razy w jeden lub więcej plików na serwerze.</span><span class="sxs-lookup"><span data-stu-id="b1279-143">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="b1279-144">W pliku konfiguracyjnym poprzedniego Apache akceptuje publicznego ruch na porcie 80.</span><span class="sxs-lookup"><span data-stu-id="b1279-144">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="b1279-145">Domena `www.example.com` obsługiwanej jest i `*.example.com` Usuwa alias do tej samej witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b1279-145">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="b1279-146">Zobacz [obsługi na podstawie nazwy hostów wirtualnych](https://httpd.apache.org/docs/current/vhosts/name-based.html) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="b1279-146">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="b1279-147">Żądania są przekazywane przez serwer proxy w katalogu głównym, aby port 5000 serwera na 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="b1279-147">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="b1279-148">Dla komunikacja dwukierunkowa `ProxyPass` i `ProxyPassReverse` są wymagane.</span><span class="sxs-lookup"><span data-stu-id="b1279-148">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span>

> [!WARNING]
> <span data-ttu-id="b1279-149">Błąd w celu określenia odpowiedniego [dyrektywy ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername) w **VirtualHost** blokowy przedstawia aplikacji luk w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="b1279-149">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="b1279-150">Powiązanie symbolu wieloznacznego domeny podrzędnej (na przykład `*.example.com`) nie stanowić to zagrożenie bezpieczeństwa, jeśli kontrolować domeny nadrzędnej cały (w przeciwieństwie do `*.com`, której występuje).</span><span class="sxs-lookup"><span data-stu-id="b1279-150">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="b1279-151">Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="b1279-151">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="b1279-152">Można skonfigurować rejestrowania `VirtualHost` przy użyciu `ErrorLog` i `CustomLog` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="b1279-152">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="b1279-153">`ErrorLog` Lokalizacja, w którym serwer rejestruje błędy, i `CustomLog` ustawia nazwę pliku i format pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="b1279-153">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="b1279-154">W tym przypadku jest to, gdzie jest rejestrowane informacje o żądaniu.</span><span class="sxs-lookup"><span data-stu-id="b1279-154">In this case, this is where request information is logged.</span></span> <span data-ttu-id="b1279-155">Istnieje jeden wiersz dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="b1279-155">There's one line for each request.</span></span>

<span data-ttu-id="b1279-156">Zapisz plik i Przetestuj konfigurację.</span><span class="sxs-lookup"><span data-stu-id="b1279-156">Save the file and test the configuration.</span></span> <span data-ttu-id="b1279-157">Jeśli wszystko przebiegnie pomyślnie, powinien być odpowiedzi `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="b1279-157">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="b1279-158">Uruchom ponownie Apache:</span><span class="sxs-lookup"><span data-stu-id="b1279-158">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="b1279-159">Monitorowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="b1279-159">Monitoring the app</span></span>

<span data-ttu-id="b1279-160">Apache jest teraz skonfigurowana do przekazywania żądań wysyłanych do `http://localhost:80` do aplikacji platformy ASP.NET Core systemem Kestrel na `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="b1279-160">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="b1279-161">Apache Konfigurowanie nie jest jednak do zarządzania procesem Kestrel.</span><span class="sxs-lookup"><span data-stu-id="b1279-161">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="b1279-162">Użyj *systemd* i utworzenie pliku usługi, aby uruchomić i monitorować podstawowej aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="b1279-162">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="b1279-163">*systemd* to system init zapewnia wiele zaawansowanych funkcji uruchamianie, zatrzymywanie oraz procesy zarządzania.</span><span class="sxs-lookup"><span data-stu-id="b1279-163">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="b1279-164">Tworzenie pliku usługi</span><span class="sxs-lookup"><span data-stu-id="b1279-164">Create the service file</span></span>

<span data-ttu-id="b1279-165">Tworzenie pliku definicji usługi:</span><span class="sxs-lookup"><span data-stu-id="b1279-165">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="b1279-166">Przykładowy plik usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="b1279-166">An example service file for the app:</span></span>

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
> <span data-ttu-id="b1279-167">**Użytkownik** &mdash; Jeśli użytkownik *apache* nie jest używany przez tę konfigurację, użytkownik musi najpierw utworzyć i podane odpowiednie własność plików.</span><span class="sxs-lookup"><span data-stu-id="b1279-167">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="b1279-168">Zapisz plik i włączyć usługę:</span><span class="sxs-lookup"><span data-stu-id="b1279-168">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="b1279-169">Uruchom usługę i sprawdź, czy jest uruchomiona:</span><span class="sxs-lookup"><span data-stu-id="b1279-169">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="b1279-170">Przy użyciu zwrotnego serwera proxy skonfigurowane i Kestrel zarządzanych za pomocą *systemd*, aplikacji sieci web jest w pełni skonfigurowane i dostępne w przeglądarce na komputerze lokalnym w `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="b1279-170">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="b1279-171">Zapoznanie się nagłówki odpowiedzi **serwera** nagłówek wskazuje, że aplikacja platformy ASP.NET Core jest obsługiwana przez Kestrel:</span><span class="sxs-lookup"><span data-stu-id="b1279-171">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="b1279-172">Przeglądanie dzienników</span><span class="sxs-lookup"><span data-stu-id="b1279-172">Viewing logs</span></span>

<span data-ttu-id="b1279-173">Ponieważ aplikacja sieci web przy użyciu Kestrel odbywa się przy użyciu *systemd*, scentralizowane dziennika są rejestrowane zdarzenia i procesów.</span><span class="sxs-lookup"><span data-stu-id="b1279-173">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="b1279-174">Jednak ten dziennik zawiera wpisy dla wszystkich usług i procesów zarządza *systemd*.</span><span class="sxs-lookup"><span data-stu-id="b1279-174">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="b1279-175">Aby wyświetlić `kestrel-hellomvc.service`— określone elementy, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="b1279-175">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="b1279-176">Potrzeby filtrowania czasu, określ opcje czasu przy użyciu polecenia.</span><span class="sxs-lookup"><span data-stu-id="b1279-176">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="b1279-177">Na przykład użyć `--since today` filtrowania dla bieżącego dnia lub `--until 1 hour ago` wyświetlić wpisy poprzedniej godziny.</span><span class="sxs-lookup"><span data-stu-id="b1279-177">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="b1279-178">Aby uzyskać więcej informacji, zobacz [man stronę journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="b1279-178">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="b1279-179">Zabezpieczanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="b1279-179">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="b1279-180">Konfigurowanie zapory</span><span class="sxs-lookup"><span data-stu-id="b1279-180">Configure firewall</span></span>

<span data-ttu-id="b1279-181">*Firewalld* jest demon dynamiczne zarządzanie zaporą z obsługą stref sieci.</span><span class="sxs-lookup"><span data-stu-id="b1279-181">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="b1279-182">Porty i filtrowanie pakietów można nadal będą zarządzane przez iptables.</span><span class="sxs-lookup"><span data-stu-id="b1279-182">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="b1279-183">*Firewalld* powinny być instalowane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="b1279-183">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="b1279-184">`yum` może służyć do zainstalowania pakietu lub sprawdź, czy jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="b1279-184">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="b1279-185">Użyj `firewalld` można otworzyć tylko te porty, które są wymagane dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b1279-185">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="b1279-186">W takim przypadku portu 80 i 443 są używane.</span><span class="sxs-lookup"><span data-stu-id="b1279-186">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="b1279-187">Porty 80 i 443, aby otworzyć na stałe ustawiony program następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="b1279-187">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="b1279-188">Ponowne załadowanie ustawień zapory.</span><span class="sxs-lookup"><span data-stu-id="b1279-188">Reload the firewall settings.</span></span> <span data-ttu-id="b1279-189">Sprawdź dostępnych usług i portów w strefie domyślnej.</span><span class="sxs-lookup"><span data-stu-id="b1279-189">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="b1279-190">Opcje są dostępne, sprawdzając `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="b1279-190">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="b1279-191">Konfiguracja protokołu SSL</span><span class="sxs-lookup"><span data-stu-id="b1279-191">SSL configuration</span></span>

<span data-ttu-id="b1279-192">Aby skonfigurować Apache dla protokołu SSL, *mod_ssl* moduł jest używany.</span><span class="sxs-lookup"><span data-stu-id="b1279-192">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="b1279-193">Gdy *host z wieloma adresami* moduł został zainstalowany, *mod_ssl* modułu również został zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="b1279-193">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="b1279-194">Jeśli nie została zainstalowana za pomocą `yum` Aby dodać je do konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b1279-194">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="b1279-195">Aby wymusić protokołu SSL, należy zainstalować `mod_rewrite` modułu, aby umożliwić ponowne zapisywanie adresów URL:</span><span class="sxs-lookup"><span data-stu-id="b1279-195">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="b1279-196">Modyfikowanie *hellomvc.conf* plik, aby umożliwić ponowne zapisywanie adresów URL i zabezpieczania komunikacji na porcie 443:</span><span class="sxs-lookup"><span data-stu-id="b1279-196">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="b1279-197">W tym przykładzie używa certyfikatu, generowany lokalnie.</span><span class="sxs-lookup"><span data-stu-id="b1279-197">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="b1279-198">**SSLCertificateFile** powinien być plikiem podstawowego certyfikatu dla nazwy domeny.</span><span class="sxs-lookup"><span data-stu-id="b1279-198">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="b1279-199">**SSLCertificateKeyFile** powinny być pliku klucza generowane po utworzeniu renderowanie po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="b1279-199">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="b1279-200">**SSLCertificateChainFile** powinien być pliku certyfikatu pośredniego (jeśli istnieją) podane przez urząd certyfikacji.</span><span class="sxs-lookup"><span data-stu-id="b1279-200">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="b1279-201">Zapisz plik i testowanie konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="b1279-201">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="b1279-202">Uruchom ponownie Apache:</span><span class="sxs-lookup"><span data-stu-id="b1279-202">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="b1279-203">Dodatkowe sugestie Apache</span><span class="sxs-lookup"><span data-stu-id="b1279-203">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="b1279-204">Dodatkowych nagłówków</span><span class="sxs-lookup"><span data-stu-id="b1279-204">Additional headers</span></span>

<span data-ttu-id="b1279-205">Aby zabezpieczyć przed atakami, istnieje kilka nagłówki, które powinny być zmodyfikowany lub dodane.</span><span class="sxs-lookup"><span data-stu-id="b1279-205">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="b1279-206">Upewnij się, że `mod_headers` moduł jest zainstalowany:</span><span class="sxs-lookup"><span data-stu-id="b1279-206">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="b1279-207">Zabezpieczenia przed atakami porywaniu kliknięć Apache</span><span class="sxs-lookup"><span data-stu-id="b1279-207">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="b1279-208">[Porywaniu kliknięć](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), znanej także jako *interfejsu użytkownika odszkodowania ataku*, jest złośliwymi atakami, gdy obiekt odwiedzający witrynę sieci Web jest zachęceni do kliknięcia łącza lub przycisku na innej stronie, nie są one obecnie odwiedzający.</span><span class="sxs-lookup"><span data-stu-id="b1279-208">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="b1279-209">Użyj `X-FRAME-OPTIONS` do zabezpieczenia witryny.</span><span class="sxs-lookup"><span data-stu-id="b1279-209">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="b1279-210">Edytuj *httpd.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="b1279-210">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="b1279-211">Dodaj wiersz `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="b1279-211">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="b1279-212">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="b1279-212">Save the file.</span></span> <span data-ttu-id="b1279-213">Uruchom ponownie Apache.</span><span class="sxs-lookup"><span data-stu-id="b1279-213">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="b1279-214">Wykrywanie typ MIME</span><span class="sxs-lookup"><span data-stu-id="b1279-214">MIME-type sniffing</span></span>

<span data-ttu-id="b1279-215">`X-Content-Type-Options` Nagłówka uniemożliwia programowi Internet Explorer z *wykrywanie MIME* (Określanie pliku `Content-Type` z zawartości pliku).</span><span class="sxs-lookup"><span data-stu-id="b1279-215">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="b1279-216">Jeśli serwer ustawia `Content-Type` nagłówka do `text/html` z `nosniff` zestaw opcji, program Internet Explorer renderuje zawartość jako `text/html` niezależnie od zawartości pliku.</span><span class="sxs-lookup"><span data-stu-id="b1279-216">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="b1279-217">Edytuj *httpd.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="b1279-217">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="b1279-218">Dodaj wiersz `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="b1279-218">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="b1279-219">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="b1279-219">Save the file.</span></span> <span data-ttu-id="b1279-220">Uruchom ponownie Apache.</span><span class="sxs-lookup"><span data-stu-id="b1279-220">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="b1279-221">Równoważenie obciążenia</span><span class="sxs-lookup"><span data-stu-id="b1279-221">Load Balancing</span></span> 

<span data-ttu-id="b1279-222">W tym przykładzie przedstawiono sposób instalacji i konfiguracji Apache CentOS 7 i Kestrel na tym samym komputerze wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="b1279-222">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="b1279-223">Aby można było ma pojedynczego punktu awarii; przy użyciu *mod_proxy_balancer* i modyfikowanie **VirtualHost** umożliwiałyby zarządzania wiele wystąpień aplikacji sieci web za serwerem proxy Apache.</span><span class="sxs-lookup"><span data-stu-id="b1279-223">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="b1279-224">W pliku konfiguracyjnym pokazano poniżej, dodatkowego wystąpienia `hellomvc` aplikacji jest skonfigurowana do uruchamiania na porcie 5001.</span><span class="sxs-lookup"><span data-stu-id="b1279-224">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="b1279-225">*Proxy* sekcji ustawiono na potrzeby równoważenia obciążenia z konfiguracją modułu równoważenia z dwóch członków *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="b1279-225">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="b1279-226">Limity szybkości</span><span class="sxs-lookup"><span data-stu-id="b1279-226">Rate Limits</span></span>
<span data-ttu-id="b1279-227">Przy użyciu *mod_ratelimit*, który znajduje się w *host z wieloma adresami* moduł, może być ograniczona przepustowość klientów:</span><span class="sxs-lookup"><span data-stu-id="b1279-227">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="b1279-228">Przykładowy plik ogranicza przepustowość jako 600 KB/s w lokalizacji głównej:</span><span class="sxs-lookup"><span data-stu-id="b1279-228">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
