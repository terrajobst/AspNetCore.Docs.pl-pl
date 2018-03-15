---
title: Host platformy ASP.NET Core w systemie Linux z Nginx
author: rick-anderson
description: "Opisuje sposób instalacji Nginx jako zwrotny serwer proxy na 16.04 Ubuntu, aby przesyłał dalej ruch HTTP dla aplikacji sieci web platformy ASP.NET Core systemem Kestrel."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: a1de177fcd41c925a85e5aab9a0d236249b7da0b
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="f6bb4-103">Host platformy ASP.NET Core w systemie Linux z Nginx</span><span class="sxs-lookup"><span data-stu-id="f6bb4-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="f6bb4-104">Przez [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="f6bb4-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="f6bb4-105">W tym przewodniku opisano konfigurowanie środowiska ASP.NET Core gotowe do produkcji na 16.04 Ubuntu Server.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

> [!NOTE]
> <span data-ttu-id="f6bb4-106">Dla Ubuntu 14.04 *supervisord* zaleca się rozwiązanie do monitorowania procesu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-106">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="f6bb4-107">*systemd* nie jest dostępna w Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-107">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="f6bb4-108">[Zobacz poprzednią wersję tego dokumentu](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="f6bb4-108">[See previous version of this document](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="f6bb4-109">Ten przewodnik:</span><span class="sxs-lookup"><span data-stu-id="f6bb4-109">This guide:</span></span>

* <span data-ttu-id="f6bb4-110">Umieszcza istniejącej aplikacji platformy ASP.NET Core za zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-110">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="f6bb4-111">Konfiguruje zwrotnego serwera proxy do przekazywania żądań do serwera sieci web Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-111">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="f6bb4-112">Gwarantuje, że aplikacja sieci web uruchamiany podczas uruchamiania jako demon.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-112">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="f6bb4-113">Konfiguruje narzędzie do zarządzania procesu, aby ponownie uruchomić aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-113">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f6bb4-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="f6bb4-114">Prerequisites</span></span>

1. <span data-ttu-id="f6bb4-115">Dostęp do serwera 16.04 Ubuntu przy użyciu konta użytkowników standardowych z uprawnieniami sudo</span><span class="sxs-lookup"><span data-stu-id="f6bb4-115">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
1. <span data-ttu-id="f6bb4-116">Istniejącej aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f6bb4-116">An existing ASP.NET Core app</span></span>

## <a name="copy-over-the-app"></a><span data-ttu-id="f6bb4-117">Skopiuj przez aplikację</span><span class="sxs-lookup"><span data-stu-id="f6bb4-117">Copy over the app</span></span>

<span data-ttu-id="f6bb4-118">Uruchom [publikowania dotnet](/dotnet/core/tools/dotnet-publish) ze środowiska deweloperskiego do pakietu aplikacji do katalogu autonomiczną, która może działać na serwerze.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-118">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="f6bb4-119">Skopiuj aplikacji platformy ASP.NET Core na serwerze przy użyciu dowolnego narzędzia integruje przepływu organizacji (na przykład punkt połączenia usługi, FTP).</span><span class="sxs-lookup"><span data-stu-id="f6bb4-119">Copy the ASP.NET Core app to the server using whatever tool integrates into the organization's workflow (for example, SCP, FTP).</span></span> <span data-ttu-id="f6bb4-120">Testowanie aplikacji, na przykład:</span><span class="sxs-lookup"><span data-stu-id="f6bb4-120">Test the app, for example:</span></span>

* <span data-ttu-id="f6bb4-121">W wierszu polecenia Uruchom `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-121">From the command line, run `dotnet <app_assembly>.dll`.</span></span>
* <span data-ttu-id="f6bb4-122">W przeglądarce przejdź do `http://<serveraddress>:<port>` można zweryfikować aplikacja działa w systemie Linux.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-122">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="f6bb4-123">Skonfiguruj serwer zwrotnego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="f6bb4-123">Configure a reverse proxy server</span></span>

<span data-ttu-id="f6bb4-124">Zwrotny serwer proxy jest typowe dla obsługi aplikacji sieci web dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-124">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="f6bb4-125">Zwrotny serwer proxy kończy żądanie HTTP i przekazuje je do aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-125">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="f6bb4-126">Dlaczego warto używać zwrotnego serwera proxy?</span><span class="sxs-lookup"><span data-stu-id="f6bb4-126">Why use a reverse proxy server?</span></span>

<span data-ttu-id="f6bb4-127">Kestrel stanowi doskonałe rozwiązanie do obsługi zawartości dynamicznej z platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-127">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="f6bb4-128">Funkcji obsługi sieci web nie są jednak jako funkcja sformatowany jako serwery usług IIS, Apache lub Nginx.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-128">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="f6bb4-129">Zwrotnego serwera proxy można odciążyć pracy, takie jak obsługę zawartości statycznej, buforowanie żądań kompresowania żądań i kończenia żądań SSL z serwera HTTP.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-129">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="f6bb4-130">Zwrotnego serwera proxy może znajdować się na dedykowanym komputerze lub mogą można wdrożyć obok serwera HTTP.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-130">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="f6bb4-131">Na potrzeby tego przewodnika jest używany przez pojedyncze wystąpienie Nginx.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-131">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="f6bb4-132">Uruchamia go na tym samym serwerze, z serwera HTTP.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-132">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="f6bb4-133">Na podstawie wymagań, można wybrać różne ustawienia.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-133">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="f6bb4-134">Ponieważ żądania są przekazywane przez zwrotny serwer proxy, należy używać oprogramowania pośredniczącego nagłówki przekazywane z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-134">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="f6bb4-135">Aktualizacje oprogramowania pośredniczącego `Request.Scheme`za pomocą `X-Forwarded-Proto` nagłówka, więc poprawne działanie tego przekierowania URI i innymi zasadami zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-135">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="f6bb4-136">Korzystając z dowolnego typu uwierzytelniania oprogramowania pośredniczącego, najpierw należy uruchomić oprogramowanie pośredniczące przekazane nagłówków.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-136">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="f6bb4-137">Ta kolejność zapewnia, że oprogramowanie pośredniczące uwierzytelniania można używać wartości nagłówka i generowanie poprawne przekierowania URI.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-137">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f6bb4-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f6bb4-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f6bb4-139">Wywołanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metody w `Startup.Configure` przed wywołaniem [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) lub podobne oprogramowanie pośredniczące schemat uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="f6bb4-139">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f6bb4-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f6bb4-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f6bb4-141">Wywołanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metody w `Startup.Configure` przed wywołaniem [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) i [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) lub podobne schemat uwierzytelniania oprogramowanie pośredniczące:</span><span class="sxs-lookup"><span data-stu-id="f6bb4-141">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware:</span></span>

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

<span data-ttu-id="f6bb4-142">Jeśli nie [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) są określone przez oprogramowanie pośredniczące są domyślne nagłówki do przekazywania `None`.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-142">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

### <a name="install-nginx"></a><span data-ttu-id="f6bb4-143">Zainstaluj Nginx</span><span class="sxs-lookup"><span data-stu-id="f6bb4-143">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="f6bb4-144">Jeśli zostanie zainstalowana opcjonalnych modułów Nginx, może być wymagane tworzenie Nginx ze źródła.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-144">If optional Nginx modules will be installed, building Nginx from source might be required.</span></span>

<span data-ttu-id="f6bb4-145">Użyj `apt-get` do zainstalowania Nginx.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-145">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="f6bb4-146">Instalator tworzy skrypt init System V uruchamiany Nginx jako demon podczas uruchamiania systemu.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-146">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="f6bb4-147">Po zainstalowaniu Nginx po raz pierwszy, jawnie uruchomić:</span><span class="sxs-lookup"><span data-stu-id="f6bb4-147">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="f6bb4-148">Sprawdź, czy przeglądarka wyświetla domyślna strona początkowa dla Nginx.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-148">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="f6bb4-149">Skonfiguruj Nginx</span><span class="sxs-lookup"><span data-stu-id="f6bb4-149">Configure Nginx</span></span>

<span data-ttu-id="f6bb4-150">Aby skonfigurować Nginx jako zwrotny serwer proxy do przesyłania żądań do aplikacji platformy ASP.NET Core, zmodyfikuj */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-150">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="f6bb4-151">Otwórz go w edytorze tekstów i Zastąp zawartość z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="f6bb4-151">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="f6bb4-152">Gdy nie `server_name` dopasowania, Nginx korzysta z domyślnego serwera.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-152">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="f6bb4-153">Jeśli żaden serwer domyślny jest zdefiniowany, pierwszy serwer w pliku konfiguracji jest serwerem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-153">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="f6bb4-154">Najlepszym rozwiązaniem jest dodanie określonego domyślnego serwera, która zwraca kod stanu 444 w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-154">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="f6bb4-155">To jest przykład domyślny serwer konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="f6bb4-155">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="f6bb4-156">Z poprzedniego pliku i domyślnego serwera konfiguracji, Nginx akceptuje publicznego ruch na porcie 80 z nagłówkiem hosta `example.com` lub `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-156">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="f6bb4-157">Żądania niezgodni te hosty nie pobrać przekazywane do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-157">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="f6bb4-158">Nginx przekazuje żądania pasujące do Kestrel na `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-158">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="f6bb4-159">Zobacz [jak nginx przetwarza żądanie](https://nginx.org/docs/http/request_processing.html) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-159">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span>

> [!WARNING]
> <span data-ttu-id="f6bb4-160">Błąd w celu określenia odpowiedniego [dyrektywy nazwa_serwera](https://nginx.org/docs/http/server_names.html) przedstawia aplikacji luk w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-160">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="f6bb4-161">Powiązanie symbolu wieloznacznego domeny podrzędnej (na przykład `*.example.com`) nie stanowić to zagrożenie bezpieczeństwa, jeśli kontrolować domeny nadrzędnej cały (w przeciwieństwie do `*.com`, której występuje).</span><span class="sxs-lookup"><span data-stu-id="f6bb4-161">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="f6bb4-162">Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-162">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="f6bb4-163">Po ustanowieniu konfiguracji Nginx, uruchom `sudo nginx -t` Aby sprawdzić składnię plików konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-163">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="f6bb4-164">Jeśli test konfiguracji w pliku zakończy się pomyślnie, wymusić Nginx do zastosowania zmian, uruchamiając `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-164">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="f6bb4-165">Monitorowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="f6bb4-165">Monitoring the app</span></span>

<span data-ttu-id="f6bb4-166">Serwer jest skonfigurowany do przekazywania żądań wysyłanych do `http://<serveraddress>:80` się do aplikacji platformy ASP.NET Core systemem Kestrel na `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-166">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="f6bb4-167">Nginx Konfigurowanie nie jest jednak do zarządzania procesem Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-167">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="f6bb4-168">*systemd* może służyć do tworzenia pliku usługi, aby uruchomić i monitorować podstawowej aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-168">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="f6bb4-169">*systemd* to system init zapewnia wiele zaawansowanych funkcji uruchamianie, zatrzymywanie oraz procesy zarządzania.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-169">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="f6bb4-170">Tworzenie pliku usługi</span><span class="sxs-lookup"><span data-stu-id="f6bb4-170">Create the service file</span></span>

<span data-ttu-id="f6bb4-171">Tworzenie pliku definicji usługi:</span><span class="sxs-lookup"><span data-stu-id="f6bb4-171">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="f6bb4-172">Poniżej przedstawiono przykładowy plik usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="f6bb4-172">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="f6bb4-173">**Uwaga:** Jeśli użytkownik *danych www* nie jest używany przez tę konfigurację użytkownika zdefiniowane w tym miejscu musi być najpierw utworzyć i podane odpowiednie własność plików.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-173">**Note:** If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>
<span data-ttu-id="f6bb4-174">**Uwaga:** Linux ma system plików z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-174">**Note:** Linux has a case-sensitive file system.</span></span> <span data-ttu-id="f6bb4-175">Ustawienie "Production" spowoduje wyszukiwanie w pliku konfiguracyjnym ASPNETCORE_ENVIRONMENT *appsettings. Production.JSON*, a nie *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-175">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="f6bb4-176">Zapisz plik i włączyć usługę.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-176">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="f6bb4-177">Uruchom usługę i sprawdź, czy jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-177">Start the service and verify that it's running.</span></span>

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="f6bb4-178">Zwrotny serwer proxy skonfigurowane i zarządzanych za pomocą systemd Kestrel, aplikacji sieci web jest w pełni skonfigurowane i dostępne w przeglądarce na komputerze lokalnym w `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-178">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="f6bb4-179">Jest również dostępny z komputera zdalnego, blokowanie wszelkich zaporą, która może być blokowane.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-179">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="f6bb4-180">Zapoznanie się nagłówki odpowiedzi `Server` nagłówek zawiera obsługiwanej przez Kestrel aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-180">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="f6bb4-181">Przeglądanie dzienników</span><span class="sxs-lookup"><span data-stu-id="f6bb4-181">Viewing logs</span></span>

<span data-ttu-id="f6bb4-182">Ponieważ aplikacja sieci web przy użyciu Kestrel odbywa się przy użyciu `systemd`, scentralizowane dziennika są rejestrowane wszystkie zdarzenia i procesów.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-182">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="f6bb4-183">Jednak ten dziennik zawiera wszystkie wpisy dla wszystkich usług i procesów zarządza `systemd`.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-183">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="f6bb4-184">Aby wyświetlić `kestrel-hellomvc.service`— określone elementy, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="f6bb4-184">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="f6bb4-185">Dla dalszego filtrowania, opcje czasu takich jak `--since today`, `--until 1 hour ago` lub kombinacji tych można zmniejszyć liczbę wpisów zwracane.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-185">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="f6bb4-186">Zabezpieczanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="f6bb4-186">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="f6bb4-187">Włącz AppArmor</span><span class="sxs-lookup"><span data-stu-id="f6bb4-187">Enable AppArmor</span></span>

<span data-ttu-id="f6bb4-188">Linux zabezpieczeń modułów (LSM) tak, to platforma, która jest częścią jądra systemu Linux od 2.6 systemu Linux.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-188">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="f6bb4-189">LSM obsługuje różne implementacje modułów zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-189">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="f6bb4-190">[AppArmor](https://wiki.ubuntu.com/AppArmor) jest LSM, implementujący systemu obowiązkowe kontroli dostępu, który umożliwia ograniczenie program do określonych zasobów.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-190">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="f6bb4-191">Upewnij się, AppArmor jest włączony i poprawnie skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-191">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="f6bb4-192">Konfigurowanie zapory</span><span class="sxs-lookup"><span data-stu-id="f6bb4-192">Configuring the firewall</span></span>

<span data-ttu-id="f6bb4-193">Zamknij Wyłącz wszystkie porty zewnętrznych, które nie są używane.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-193">Close off all external ports that are not in use.</span></span> <span data-ttu-id="f6bb4-194">Zapora prostotę (ufw) zapewnia frontonu dla `iptables` korzystanie z interfejsu wiersza polecenia dla konfiguracji zapory.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-194">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="f6bb4-195">Sprawdź, czy `ufw` zezwalają na ruch na dowolne porty.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-195">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="f6bb4-196">Securing Nginx</span><span class="sxs-lookup"><span data-stu-id="f6bb4-196">Securing Nginx</span></span>

<span data-ttu-id="f6bb4-197">Rozkład domyślne Nginx nie Włącz protokół SSL.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-197">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="f6bb4-198">Aby włączyć dodatkowe funkcje zabezpieczeń, kompilacji ze źródła.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-198">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="f6bb4-199">Pobierz źródła i zainstaluj zależności kompilacji</span><span class="sxs-lookup"><span data-stu-id="f6bb4-199">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="f6bb4-200">Zmień nazwę Nginx odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="f6bb4-200">Change the Nginx response name</span></span>

<span data-ttu-id="f6bb4-201">Edit *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="f6bb4-201">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="f6bb4-202">Skonfiguruj opcje i kompilacji</span><span class="sxs-lookup"><span data-stu-id="f6bb4-202">Configure the options and build</span></span>

<span data-ttu-id="f6bb4-203">Biblioteka PCRE jest wymagany dla wyrażeń regularnych.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-203">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="f6bb4-204">Wyrażenia regularne są używane w dyrektywie lokalizacji dla ngx_http_rewrite_module.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-204">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="f6bb4-205">Http_ssl_module dodaje obsługę protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-205">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="f6bb4-206">Należy rozważyć użycie zapory aplikacji sieci web, takich jak *ModSecurity* celu ograniczenia funkcjonalności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-206">Consider using a web app firewall like *ModSecurity* to harden the app.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="f6bb4-207">Konfigurowanie protokołu SSL</span><span class="sxs-lookup"><span data-stu-id="f6bb4-207">Configure SSL</span></span>

* <span data-ttu-id="f6bb4-208">Konfigurowanie serwera do nasłuchiwania na ruch HTTPS na porcie `443` , określając prawidłowy certyfikat wystawiony przez zaufany urząd certyfikacji.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-208">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="f6bb4-209">Ograniczenia funkcjonalności zabezpieczeń poprzez zastosowanie niektórych rozwiązań przedstawione poniżej */etc/nginx/nginx.conf* pliku.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-209">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="f6bb4-210">Przykłady obejmują wybranie silniejszego szyfrowania i przekierowywania całego ruchu za pośrednictwem protokołu HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-210">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="f6bb4-211">Dodawanie `HTTP Strict-Transport-Security` nagłówka (HSTS) zapewnia wszystkie kolejne żądania przez klienta są tylko za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-211">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="f6bb4-212">Nie dodawaj nagłówka TLS Strict lub wybrać odpowiednie `max-age` Jeśli SSL zostanie wyłączone w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-212">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="f6bb4-213">Dodaj */etc/nginx/proxy.conf* pliku konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="f6bb4-213">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="f6bb4-214">Edytuj */etc/nginx/nginx.conf* pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-214">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="f6bb4-215">Przykład zawiera zarówno `http` i `server` sekcji w pliku konfiguracji jednego.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-215">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="f6bb4-216">Bezpieczny Nginx z porywaniu kliknięć</span><span class="sxs-lookup"><span data-stu-id="f6bb4-216">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="f6bb4-217">Porywaniu kliknięć to technika złośliwego zbierać zainfekowane użytkownik klika polecenie.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-217">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="f6bb4-218">Porywaniu kliknięć sztuczki ofiara (użytkownik) do kliknięcia zainfekowane w witrynie.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-218">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="f6bb4-219">Użyj X-FRAME-OPTIONS do zabezpieczenia witryny.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-219">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="f6bb4-220">Edytuj *nginx.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="f6bb4-220">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="f6bb4-221">Dodaj wiersz `add_header X-Frame-Options "SAMEORIGIN";` i Zapisz plik, a następnie uruchom ponownie Nginx.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-221">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="f6bb4-222">Wykrywanie typ MIME</span><span class="sxs-lookup"><span data-stu-id="f6bb4-222">MIME-type sniffing</span></span>

<span data-ttu-id="f6bb4-223">Ten nagłówek zapobiega w większości przeglądarek z wykrywanie MIME odpowiedzi od deklarowanego typu zawartości, jak nagłówka powoduje, że przeglądarka nie, aby zastąpić typ zawartości odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-223">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="f6bb4-224">Z `nosniff` opcję, jeśli serwer jest wyświetlany komunikat "text/html" jest zawartość przeglądarki renderuje go jako "text/html".</span><span class="sxs-lookup"><span data-stu-id="f6bb4-224">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="f6bb4-225">Edytuj *nginx.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="f6bb4-225">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="f6bb4-226">Dodaj wiersz `add_header X-Content-Type-Options "nosniff";` i Zapisz plik, a następnie uruchom ponownie Nginx.</span><span class="sxs-lookup"><span data-stu-id="f6bb4-226">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
