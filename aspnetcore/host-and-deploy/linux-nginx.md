---
title: Host platformy ASP.NET Core w systemie Linux z nginx
description: "Opisuje sposób instalacji nginx jako zwrotny serwer proxy na 16.04 Ubuntu, aby przesyłał dalej ruch HTTP dla aplikacji sieci web platformy ASP.NET Core systemem Kestrel."
author: rick-anderson
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 08/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: bad84b8c68bd0bc63bcd125e1873bc99a2ed2afd
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="565b9-103">Host platformy ASP.NET Core w systemie Linux z nginx</span><span class="sxs-lookup"><span data-stu-id="565b9-103">Host ASP.NET Core on Linux with nginx</span></span>

<span data-ttu-id="565b9-104">Przez [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="565b9-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="565b9-105">W tym przewodniku opisano konfigurowanie środowiska ASP.NET Core gotowe do produkcji na 16.04 Ubuntu Server.</span><span class="sxs-lookup"><span data-stu-id="565b9-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

<span data-ttu-id="565b9-106">**Uwaga:** dla Ubuntu 14.04 *supervisord* zaleca się rozwiązanie do monitorowania procesu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="565b9-106">**Note:** For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="565b9-107">*systemd* nie jest dostępna w Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="565b9-107">*systemd* isn't available on Ubuntu 14.04.</span></span> [<span data-ttu-id="565b9-108">Zobacz poprzednią wersję tego dokumentu</span><span class="sxs-lookup"><span data-stu-id="565b9-108">See previous version of this document</span></span>](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

<span data-ttu-id="565b9-109">Ten przewodnik:</span><span class="sxs-lookup"><span data-stu-id="565b9-109">This guide:</span></span>

* <span data-ttu-id="565b9-110">Umieszcza istniejącej aplikacji platformy ASP.NET Core za zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="565b9-110">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="565b9-111">Konfiguruje zwrotnego serwera proxy do przekazywania żądań do serwera sieci web Kestrel.</span><span class="sxs-lookup"><span data-stu-id="565b9-111">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="565b9-112">Gwarantuje, że aplikacja sieci web uruchamiany podczas uruchamiania jako demon.</span><span class="sxs-lookup"><span data-stu-id="565b9-112">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="565b9-113">Konfiguruje narzędzie do zarządzania procesu, aby ponownie uruchomić aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="565b9-113">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="565b9-114">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="565b9-114">Prerequisites</span></span>

1. <span data-ttu-id="565b9-115">Dostęp do serwera 16.04 Ubuntu przy użyciu konta użytkowników standardowych z uprawnieniami sudo</span><span class="sxs-lookup"><span data-stu-id="565b9-115">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
1. <span data-ttu-id="565b9-116">Istniejącej aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="565b9-116">An existing ASP.NET Core app</span></span>

## <a name="copy-over-the-app"></a><span data-ttu-id="565b9-117">Skopiuj przez aplikację</span><span class="sxs-lookup"><span data-stu-id="565b9-117">Copy over the app</span></span>

<span data-ttu-id="565b9-118">Uruchom `dotnet publish` ze środowiska deweloperskiego do pakietu aplikacji do katalogu autonomiczną, która może działać na serwerze.</span><span class="sxs-lookup"><span data-stu-id="565b9-118">Run `dotnet publish` from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="565b9-119">Skopiuj aplikacji platformy ASP.NET Core na serwerze przy użyciu dowolnego narzędzia integruje przepływu organizacji (na przykład punkt połączenia usługi, FTP).</span><span class="sxs-lookup"><span data-stu-id="565b9-119">Copy the ASP.NET Core app to the server using whatever tool integrates into the organization's workflow (for example, SCP, FTP).</span></span> <span data-ttu-id="565b9-120">Testowanie aplikacji, na przykład:</span><span class="sxs-lookup"><span data-stu-id="565b9-120">Test the app, for example:</span></span>

* <span data-ttu-id="565b9-121">W wierszu polecenia Uruchom `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="565b9-121">From the command line, run `dotnet <app_assembly>.dll`.</span></span>
* <span data-ttu-id="565b9-122">W przeglądarce przejdź do `http://<serveraddress>:<port>` można zweryfikować aplikacja działa w systemie Linux.</span><span class="sxs-lookup"><span data-stu-id="565b9-122">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="565b9-123">Skonfiguruj serwer zwrotnego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="565b9-123">Configure a reverse proxy server</span></span>

<span data-ttu-id="565b9-124">Zwrotny serwer proxy jest typowe dla obsługi aplikacji sieci web dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="565b9-124">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="565b9-125">Zwrotny serwer proxy kończy żądanie HTTP i przekazuje je do aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="565b9-125">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="565b9-126">Dlaczego warto używać zwrotnego serwera proxy?</span><span class="sxs-lookup"><span data-stu-id="565b9-126">Why use a reverse proxy server?</span></span>

<span data-ttu-id="565b9-127">Kestrel stanowi doskonałe rozwiązanie do obsługi zawartości dynamicznej z platformy ASP.NET Core; jednak części usług sieci web nie są jako funkcja sformatowany jako serwery, takimi jak usługi IIS, Apache lub nginx.</span><span class="sxs-lookup"><span data-stu-id="565b9-127">Kestrel is great for serving dynamic content from ASP.NET Core; however, the web serving parts aren’t as feature rich as servers like IIS, Apache, or nginx.</span></span> <span data-ttu-id="565b9-128">Zwrotnego serwera proxy można odciążyć pracy, takich jak obsługę zawartości statycznej, buforowanie żądań, kompresowania żądań i kończenia żądań SSL z serwera HTTP.</span><span class="sxs-lookup"><span data-stu-id="565b9-128">A reverse proxy server can offload work like serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="565b9-129">Zwrotnego serwera proxy może znajdować się na dedykowanym komputerze lub mogą można wdrożyć obok serwera HTTP.</span><span class="sxs-lookup"><span data-stu-id="565b9-129">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="565b9-130">Na potrzeby tego przewodnika jest używany przez pojedyncze wystąpienie nginx.</span><span class="sxs-lookup"><span data-stu-id="565b9-130">For the purposes of this guide, a single instance of nginx is used.</span></span> <span data-ttu-id="565b9-131">Uruchamia go na tym samym serwerze, z serwera HTTP.</span><span class="sxs-lookup"><span data-stu-id="565b9-131">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="565b9-132">Na podstawie wymagań, różnych konfiguracji może być wybrana opcja.</span><span class="sxs-lookup"><span data-stu-id="565b9-132">Based on requirements, a different setup may be choosen.</span></span>

<span data-ttu-id="565b9-133">Ponieważ żądania są przekazywane przez zwrotny serwer proxy, należy użyć `ForwardedHeaders` oprogramowania pośredniczącego z `Microsoft.AspNetCore.HttpOverrides` pakietu.</span><span class="sxs-lookup"><span data-stu-id="565b9-133">Because requests are forwarded by reverse proxy, use the `ForwardedHeaders` middleware from the `Microsoft.AspNetCore.HttpOverrides` package.</span></span> <span data-ttu-id="565b9-134">Aktualizacje tego oprogramowania pośredniczącego `Request.Scheme`za pomocą `X-Forwarded-Proto` nagłówka, więc poprawne działanie tego przekierowania URI i innymi zasadami zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="565b9-134">This middleware updates `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="565b9-135">Podczas konfigurowania serwera zwrotnego serwera proxy, oprogramowanie pośredniczące uwierzytelniania musi `UseForwardedHeaders` ma być uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="565b9-135">When setting up a reverse proxy server, the authentication middleware needs `UseForwardedHeaders` to run first.</span></span> <span data-ttu-id="565b9-136">Ta kolejność zapewnia, że oprogramowanie pośredniczące uwierzytelniania może wykorzystywać odpowiednich wartości i generowanie poprawne przekierowania URI.</span><span class="sxs-lookup"><span data-stu-id="565b9-136">This ordering ensures that the authentication middleware can consume the affected values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="565b9-137">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="565b9-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="565b9-138">Wywołanie `UseForwardedHeaders` — metoda (w `Configure` metody *Startup.cs*) przed wywołaniem `UseAuthentication` lub podobne oprogramowanie pośredniczące schemat uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="565b9-138">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseAuthentication` or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="565b9-139">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="565b9-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="565b9-140">Wywołanie `UseForwardedHeaders` — metoda (w `Configure` metody *Startup.cs*) przed wywołaniem `UseIdentity` i `UseFacebookAuthentication` lub podobne oprogramowanie pośredniczące schemat uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="565b9-140">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseIdentity` and `UseFacebookAuthentication` or similar authentication scheme middleware:</span></span>

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

### <a name="install-nginx"></a><span data-ttu-id="565b9-141">Zainstaluj nginx</span><span class="sxs-lookup"><span data-stu-id="565b9-141">Install nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="565b9-142">Jeśli zostaną zainstalowane moduły opcjonalne nginx, może być wymagane tworzenie nginx ze źródła.</span><span class="sxs-lookup"><span data-stu-id="565b9-142">If optional nginx modules will be installed, building nginx from source might be required.</span></span>

<span data-ttu-id="565b9-143">Użyj `apt-get` do zainstalowania nginx.</span><span class="sxs-lookup"><span data-stu-id="565b9-143">Use `apt-get` to install nginx.</span></span> <span data-ttu-id="565b9-144">Instalator tworzy skrypt init System V uruchamiany nginx jako demon podczas uruchamiania systemu.</span><span class="sxs-lookup"><span data-stu-id="565b9-144">The installer creates a System V init script that runs nginx as daemon on system startup.</span></span> <span data-ttu-id="565b9-145">Po zainstalowaniu nginx po raz pierwszy, jawnie uruchomić:</span><span class="sxs-lookup"><span data-stu-id="565b9-145">Since nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="565b9-146">Sprawdź, czy przeglądarka wyświetla domyślna strona dla nginx początkowa.</span><span class="sxs-lookup"><span data-stu-id="565b9-146">Verify a browser displays the default landing page for nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="565b9-147">Skonfiguruj nginx</span><span class="sxs-lookup"><span data-stu-id="565b9-147">Configure nginx</span></span>

<span data-ttu-id="565b9-148">Aby skonfigurować nginx jako zwrotny serwer proxy do przesyłania żądań do aplikacji platformy ASP.NET Core, zmodyfikuj `/etc/nginx/sites-available/default`.</span><span class="sxs-lookup"><span data-stu-id="565b9-148">To configure nginx as a reverse proxy to forward requests to our ASP.NET Core app, modify `/etc/nginx/sites-available/default`.</span></span> <span data-ttu-id="565b9-149">Otwórz go w edytorze tekstów i Zastąp zawartość z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="565b9-149">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="565b9-150">Ten plik konfiguracji nginx przekazuje ruch przychodzący publicznych z portu `80` do portu `5000`.</span><span class="sxs-lookup"><span data-stu-id="565b9-150">This nginx configuration file forwards incoming public traffic from port `80` to port `5000`.</span></span>

<span data-ttu-id="565b9-151">Po ustanowieniu konfiguracji nginx, uruchom `sudo nginx -t` Aby sprawdzić składnię plików konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="565b9-151">Once the nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="565b9-152">Jeśli test konfiguracji w pliku zakończy się pomyślnie, wymusić nginx do zastosowania zmian, uruchamiając `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="565b9-152">If the configuration file test is successful, force nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="565b9-153">Monitorowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="565b9-153">Monitoring the app</span></span>

<span data-ttu-id="565b9-154">Serwer jest skonfigurowany do przekazywania żądań wysyłanych do `http://<serveraddress>:80` się do aplikacji platformy ASP.NET Core systemem Kestrel na `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="565b9-154">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="565b9-155">Jednak nginx nie skonfigurowano do zarządzania procesem Kestrel.</span><span class="sxs-lookup"><span data-stu-id="565b9-155">However, nginx is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="565b9-156">*systemd* może służyć do tworzenia pliku usługi, aby uruchomić i monitorować podstawowej aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="565b9-156">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="565b9-157">*systemd* to system init zapewnia wiele zaawansowanych funkcji uruchamianie, zatrzymywanie oraz procesy zarządzania.</span><span class="sxs-lookup"><span data-stu-id="565b9-157">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="565b9-158">Tworzenie pliku usługi</span><span class="sxs-lookup"><span data-stu-id="565b9-158">Create the service file</span></span>

<span data-ttu-id="565b9-159">Tworzenie pliku definicji usługi:</span><span class="sxs-lookup"><span data-stu-id="565b9-159">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="565b9-160">Poniżej przedstawiono przykładowy plik usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="565b9-160">The following is an example service file for the app:</span></span>

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

<span data-ttu-id="565b9-161">**Uwaga:** Jeśli użytkownik *danych www* nie jest używany przez tę konfigurację użytkownika zdefiniowane w tym miejscu musi być najpierw utworzyć i podane odpowiednie własność plików.</span><span class="sxs-lookup"><span data-stu-id="565b9-161">**Note:** If the user *www-data* is not used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="565b9-162">Zapisz plik i włączyć usługę.</span><span class="sxs-lookup"><span data-stu-id="565b9-162">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="565b9-163">Uruchom usługę i sprawdź, czy jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="565b9-163">Start the service and verify that it is running.</span></span>

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

<span data-ttu-id="565b9-164">Zwrotny serwer proxy skonfigurowane i zarządzanych za pomocą systemd Kestrel, aplikacji sieci web jest w pełni skonfigurowane i dostępne w przeglądarce na komputerze lokalnym w `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="565b9-164">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="565b9-165">Jest również dostępny z komputera zdalnego, blokowanie wszelkich zaporą, która może być blokowane.</span><span class="sxs-lookup"><span data-stu-id="565b9-165">It is also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="565b9-166">Zapoznanie się nagłówki odpowiedzi `Server` nagłówek zawiera obsługiwanej przez Kestrel aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="565b9-166">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="565b9-167">Przeglądanie dzienników</span><span class="sxs-lookup"><span data-stu-id="565b9-167">Viewing logs</span></span>

<span data-ttu-id="565b9-168">Ponieważ aplikacja sieci web przy użyciu Kestrel odbywa się przy użyciu `systemd`, scentralizowane dziennika są rejestrowane wszystkie zdarzenia i procesów.</span><span class="sxs-lookup"><span data-stu-id="565b9-168">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="565b9-169">Jednak ten dziennik zawiera wszystkie wpisy dla wszystkich usług i procesów zarządza `systemd`.</span><span class="sxs-lookup"><span data-stu-id="565b9-169">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="565b9-170">Aby wyświetlić `kestrel-hellomvc.service`— określone elementy, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="565b9-170">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="565b9-171">Dla dalszego filtrowania, opcje czasu takich jak `--since today`, `--until 1 hour ago` lub kombinacji tych można zmniejszyć liczbę wpisów zwracane.</span><span class="sxs-lookup"><span data-stu-id="565b9-171">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="565b9-172">Zabezpieczanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="565b9-172">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="565b9-173">Włącz AppArmor</span><span class="sxs-lookup"><span data-stu-id="565b9-173">Enable AppArmor</span></span>

<span data-ttu-id="565b9-174">Linux zabezpieczeń modułów (LSM) tak, to platforma, która jest częścią jądra systemu Linux od 2.6 systemu Linux.</span><span class="sxs-lookup"><span data-stu-id="565b9-174">Linux Security Modules (LSM) is a framework that is part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="565b9-175">LSM obsługuje różne implementacje modułów zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="565b9-175">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="565b9-176">[AppArmor](https://wiki.ubuntu.com/AppArmor) jest LSM, implementujący systemu obowiązkowe kontroli dostępu, który umożliwia ograniczenie program do określonych zasobów.</span><span class="sxs-lookup"><span data-stu-id="565b9-176">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="565b9-177">Upewnij się, AppArmor jest włączony i poprawnie skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="565b9-177">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="565b9-178">Konfigurowanie zapory</span><span class="sxs-lookup"><span data-stu-id="565b9-178">Configuring the firewall</span></span>

<span data-ttu-id="565b9-179">Zamknij Wyłącz wszystkie porty zewnętrznych, które nie są używane.</span><span class="sxs-lookup"><span data-stu-id="565b9-179">Close off all external ports that are not in use.</span></span> <span data-ttu-id="565b9-180">Zapora prostotę (ufw) zapewnia frontonu dla `iptables` korzystanie z interfejsu wiersza polecenia dla konfiguracji zapory.</span><span class="sxs-lookup"><span data-stu-id="565b9-180">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="565b9-181">Sprawdź, czy `ufw` zezwalają na ruch na dowolne porty.</span><span class="sxs-lookup"><span data-stu-id="565b9-181">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="565b9-182">Zabezpieczanie nginx</span><span class="sxs-lookup"><span data-stu-id="565b9-182">Securing nginx</span></span>

<span data-ttu-id="565b9-183">Rozkład domyślne nginx nie Włącz protokół SSL.</span><span class="sxs-lookup"><span data-stu-id="565b9-183">The default distribution of nginx doesn't enable SSL.</span></span> <span data-ttu-id="565b9-184">Aby włączyć dodatkowe funkcje zabezpieczeń, kompilacji ze źródła.</span><span class="sxs-lookup"><span data-stu-id="565b9-184">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="565b9-185">Pobierz źródła i zainstaluj zależności kompilacji</span><span class="sxs-lookup"><span data-stu-id="565b9-185">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="565b9-186">Zmień nazwę nginx odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="565b9-186">Change the nginx response name</span></span>

<span data-ttu-id="565b9-187">Edytuj *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="565b9-187">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="565b9-188">Skonfiguruj opcje i kompilacji</span><span class="sxs-lookup"><span data-stu-id="565b9-188">Configure the options and build</span></span>

<span data-ttu-id="565b9-189">Biblioteka PCRE jest wymagany dla wyrażeń regularnych.</span><span class="sxs-lookup"><span data-stu-id="565b9-189">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="565b9-190">Wyrażenia regularne są używane w dyrektywie lokalizacji dla ngx_http_rewrite_module.</span><span class="sxs-lookup"><span data-stu-id="565b9-190">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="565b9-191">Http_ssl_module dodaje obsługę protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="565b9-191">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="565b9-192">Należy rozważyć użycie zapory aplikacji sieci web, takich jak *ModSecurity* celu ograniczenia funkcjonalności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="565b9-192">Consider using a web app firewall like *ModSecurity* to harden the app.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="565b9-193">Konfigurowanie protokołu SSL</span><span class="sxs-lookup"><span data-stu-id="565b9-193">Configure SSL</span></span>

* <span data-ttu-id="565b9-194">Konfigurowanie serwera do nasłuchiwania na ruch HTTPS na porcie `443` , określając prawidłowy certyfikat wystawiony przez zaufany urząd certyfikacji.</span><span class="sxs-lookup"><span data-stu-id="565b9-194">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="565b9-195">Ograniczenia funkcjonalności zabezpieczeń poprzez zastosowanie niektórych rozwiązań przedstawione poniżej */etc/nginx/nginx.conf* pliku.</span><span class="sxs-lookup"><span data-stu-id="565b9-195">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="565b9-196">Przykłady obejmują wybranie silniejszego szyfrowania i przekierowywania całego ruchu za pośrednictwem protokołu HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="565b9-196">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="565b9-197">Dodawanie `HTTP Strict-Transport-Security` nagłówka (HSTS) zapewnia wszystkie kolejne żądania przez klienta są tylko za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="565b9-197">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="565b9-198">Nie dodawaj nagłówka TLS Strict lub wybrać odpowiednie `max-age` Jeśli SSL zostanie wyłączone w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="565b9-198">Do not add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="565b9-199">Dodaj */etc/nginx/proxy.conf* pliku konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="565b9-199">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[Main](linux-nginx/proxy.conf)]

<span data-ttu-id="565b9-200">Edytuj */etc/nginx/nginx.conf* pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="565b9-200">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="565b9-201">Przykład zawiera zarówno `http` i `server` sekcji w pliku konfiguracji jednego.</span><span class="sxs-lookup"><span data-stu-id="565b9-201">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[Main](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="565b9-202">Bezpieczne nginx z porywaniu kliknięć</span><span class="sxs-lookup"><span data-stu-id="565b9-202">Secure nginx from clickjacking</span></span>
<span data-ttu-id="565b9-203">Porywaniu kliknięć to technika złośliwego zbierać zainfekowane użytkownik klika polecenie.</span><span class="sxs-lookup"><span data-stu-id="565b9-203">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="565b9-204">Porywaniu kliknięć sztuczki ofiara (użytkownik) do kliknięcia zainfekowane w witrynie.</span><span class="sxs-lookup"><span data-stu-id="565b9-204">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="565b9-205">Użyj X-FRAME-OPTIONS do zabezpieczenia witryny.</span><span class="sxs-lookup"><span data-stu-id="565b9-205">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="565b9-206">Edytuj *nginx.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="565b9-206">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="565b9-207">Dodaj wiersz `add_header X-Frame-Options "SAMEORIGIN";` i Zapisz plik, a następnie uruchom ponownie nginx.</span><span class="sxs-lookup"><span data-stu-id="565b9-207">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="565b9-208">Wykrywanie typ MIME</span><span class="sxs-lookup"><span data-stu-id="565b9-208">MIME-type sniffing</span></span>

<span data-ttu-id="565b9-209">Ten nagłówek zapobiega w większości przeglądarek z wykrywanie MIME odpowiedzi od deklarowanego typu zawartości, jak nagłówka powoduje, że przeglądarka nie, aby zastąpić typ zawartości odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="565b9-209">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="565b9-210">Z `nosniff` opcję, jeśli serwer jest wyświetlany komunikat "text/html" jest zawartość przeglądarki renderuje go jako "text/html".</span><span class="sxs-lookup"><span data-stu-id="565b9-210">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="565b9-211">Edytuj *nginx.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="565b9-211">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="565b9-212">Dodaj wiersz `add_header X-Content-Type-Options "nosniff";` i Zapisz plik, a następnie uruchom ponownie nginx.</span><span class="sxs-lookup"><span data-stu-id="565b9-212">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart nginx.</span></span>
