---
title: Host platformy ASP.NET Core w systemie Linux z Nginx
description: "Opisuje sposób instalacji Nginx jako zwrotny serwer proxy na 16.04 Ubuntu, aby przesyłał dalej ruch HTTP do aplikacji sieci web platformy ASP.NET Core systemem Kestrel."
keywords: Platformy ASP.NET Core, Linux, nginx, Ubuntu, wstecznego serwera Proxy
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 08/21/2017
ms.topic: article
ms.assetid: 1c33e576-33de-481a-8ad3-896b94fde0e3
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/linuxproduction
ms.openlocfilehash: 01768263fe82dc75a7da0e113b1850c8d788bfd3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-nginx-and-deploy-to-it"></a><span data-ttu-id="70ad8-104">Konfigurowanie środowiska macierzystego dla platformy ASP.NET Core w systemie Linux z Nginx i wdrażać do niego</span><span class="sxs-lookup"><span data-stu-id="70ad8-104">Set up a hosting environment for ASP.NET Core on Linux with Nginx, and deploy to it</span></span>

<span data-ttu-id="70ad8-105">Przez [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="70ad8-105">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="70ad8-106">W tym przewodniku opisano konfigurowanie środowiska ASP.NET Core gotowe do produkcji na 16.04 Ubuntu Server.</span><span class="sxs-lookup"><span data-stu-id="70ad8-106">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

<span data-ttu-id="70ad8-107">**Uwaga:** Ubuntu 14.04 supervisord zaleca się jako rozwiązanie do monitorowania procesu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="70ad8-107">**Note:** For Ubuntu 14.04, supervisord is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="70ad8-108">systemd nie jest dostępna w Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="70ad8-108">systemd is not available on Ubuntu 14.04.</span></span> [<span data-ttu-id="70ad8-109">Zobacz poprzednią wersję tego dokumentu</span><span class="sxs-lookup"><span data-stu-id="70ad8-109">See previous version of this document</span></span>](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

<span data-ttu-id="70ad8-110">Ten przewodnik:</span><span class="sxs-lookup"><span data-stu-id="70ad8-110">This guide:</span></span>

* <span data-ttu-id="70ad8-111">Umieszcza istniejącej aplikacji platformy ASP.NET Core za serwerem proxy wstecznego</span><span class="sxs-lookup"><span data-stu-id="70ad8-111">Places an existing ASP.NET Core application behind a reverse proxy server</span></span>
* <span data-ttu-id="70ad8-112">Konfiguruje zwrotnego serwera proxy do przekazywania żądań do serwera sieci web Kestrel</span><span class="sxs-lookup"><span data-stu-id="70ad8-112">Sets up the reverse proxy server to forward requests to the Kestrel web server</span></span>
* <span data-ttu-id="70ad8-113">Gwarantuje, że aplikacja sieci web uruchamiany podczas uruchamiania jako demon</span><span class="sxs-lookup"><span data-stu-id="70ad8-113">Ensures the web application runs on startup as a daemon</span></span>
* <span data-ttu-id="70ad8-114">Konfiguruje narzędzie do zarządzania procesu, aby ułatwić ponowne uruchomienie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="70ad8-114">Configures a process management tool to help restart the web application</span></span>

## <a name="prerequisites"></a><span data-ttu-id="70ad8-115">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="70ad8-115">Prerequisites</span></span>

1. <span data-ttu-id="70ad8-116">Dostęp do serwera 16.04 Ubuntu przy użyciu konta użytkowników standardowych z uprawnieniami sudo</span><span class="sxs-lookup"><span data-stu-id="70ad8-116">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="70ad8-117">Istniejącej aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="70ad8-117">An existing ASP.NET Core application</span></span>

## <a name="copy-over-your-app"></a><span data-ttu-id="70ad8-118">Skopiuj przez aplikację</span><span class="sxs-lookup"><span data-stu-id="70ad8-118">Copy over your app</span></span>

<span data-ttu-id="70ad8-119">Uruchom `dotnet publish` ze środowiska deweloperskiego do pakietu aplikacji do katalogu autonomiczną, która może działać na serwerze.</span><span class="sxs-lookup"><span data-stu-id="70ad8-119">Run `dotnet publish` from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="70ad8-120">Skopiuj aplikacji platformy ASP.NET Core na serwerze przy użyciu dowolnego narzędzia (punkt połączenia usługi, FTP, itp.) integruje przepływ pracy.</span><span class="sxs-lookup"><span data-stu-id="70ad8-120">Copy the ASP.NET Core app to the server using whatever tool (SCP, FTP, etc.) integrates into your workflow.</span></span> <span data-ttu-id="70ad8-121">Testowanie aplikacji, na przykład:</span><span class="sxs-lookup"><span data-stu-id="70ad8-121">Test the app, for example:</span></span>
 - <span data-ttu-id="70ad8-122">W wierszu polecenia Uruchom`dotnet yourapp.dll`</span><span class="sxs-lookup"><span data-stu-id="70ad8-122">From the command line, run `dotnet yourapp.dll`</span></span>
 - <span data-ttu-id="70ad8-123">W przeglądarce przejdź do `http://<serveraddress>:<port>` można zweryfikować aplikacja działa w systemie Linux.</span><span class="sxs-lookup"><span data-stu-id="70ad8-123">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
<span data-ttu-id="70ad8-124">**Uwaga:** użyj [narzędzia Yeoman](xref:client-side/yeoman) do utworzenia nowej aplikacji platformy ASP.NET Core dla nowego projektu.</span><span class="sxs-lookup"><span data-stu-id="70ad8-124">**Note:** Use [Yeoman](xref:client-side/yeoman) to create a new ASP.NET Core app for a new project.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="70ad8-125">Skonfiguruj serwer zwrotnego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="70ad8-125">Configure a reverse proxy server</span></span>

<span data-ttu-id="70ad8-126">Zwrotny serwer proxy jest typowe dla obsługująca dynamicznych aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="70ad8-126">A reverse proxy is a common setup for serving dynamic web applications.</span></span> <span data-ttu-id="70ad8-127">Zwrotny serwer proxy kończy żądanie HTTP i przekazuje je do aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="70ad8-127">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core application.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="70ad8-128">Dlaczego warto używać zwrotnego serwera proxy?</span><span class="sxs-lookup"><span data-stu-id="70ad8-128">Why use a reverse proxy server?</span></span>

<span data-ttu-id="70ad8-129">Kestrel stanowi doskonałe rozwiązanie do obsługi zawartości dynamicznej z platformy ASP.NET Core; jednak części usług sieci web nie są jako funkcja sformatowany jako serwery, takimi jak usługi IIS, Apache lub Nginx.</span><span class="sxs-lookup"><span data-stu-id="70ad8-129">Kestrel is great for serving dynamic content from ASP.NET Core; however, the web serving parts aren’t as feature rich as servers like IIS, Apache, or Nginx.</span></span> <span data-ttu-id="70ad8-130">Zwrotnego serwera proxy można odciążyć pracy, takich jak obsługę zawartości statycznej, buforowanie żądań, kompresowania żądań i kończenia żądań SSL z serwera HTTP.</span><span class="sxs-lookup"><span data-stu-id="70ad8-130">A reverse proxy server can offload work like serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="70ad8-131">Zwrotnego serwera proxy może znajdować się na dedykowanym komputerze lub mogą można wdrożyć obok serwera HTTP.</span><span class="sxs-lookup"><span data-stu-id="70ad8-131">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="70ad8-132">Na potrzeby tego przewodnika jest używany przez pojedyncze wystąpienie Nginx.</span><span class="sxs-lookup"><span data-stu-id="70ad8-132">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="70ad8-133">Uruchamia go na tym samym serwerze, z serwera HTTP.</span><span class="sxs-lookup"><span data-stu-id="70ad8-133">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="70ad8-134">W zależności od wymagań można wybrać różne ustawienia.</span><span class="sxs-lookup"><span data-stu-id="70ad8-134">Based on your requirements, you may choose a different setup.</span></span>

<span data-ttu-id="70ad8-135">Ponieważ żądania są przekazywane przez zwrotny serwer proxy, należy użyć `ForwardedHeaders` oprogramowania pośredniczącego z `Microsoft.AspNetCore.HttpOverrides` pakietu.</span><span class="sxs-lookup"><span data-stu-id="70ad8-135">Because requests are forwarded by reverse proxy, use the `ForwardedHeaders` middleware from the `Microsoft.AspNetCore.HttpOverrides` package.</span></span> <span data-ttu-id="70ad8-136">Aktualizacje tego oprogramowania pośredniczącego `Request.Scheme`za pomocą `X-Forwarded-Proto` nagłówka, więc poprawne działanie tego przekierowania URI i innymi zasadami zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="70ad8-136">This middleware updates `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="70ad8-137">Podczas konfigurowania serwera zwrotnego serwera proxy, oprogramowanie pośredniczące uwierzytelniania musi `UseForwardedHeaders` ma być uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="70ad8-137">When setting up a reverse proxy server, the authentication middleware needs `UseForwardedHeaders` to run first.</span></span> <span data-ttu-id="70ad8-138">Ta kolejność zapewnia, że oprogramowanie pośredniczące uwierzytelniania może wykorzystywać odpowiednich wartości i generowanie poprawne przekierowania URI.</span><span class="sxs-lookup"><span data-stu-id="70ad8-138">This ordering ensures that the authentication middleware can consume the affected values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="70ad8-139">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="70ad8-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="70ad8-140">Wywołanie `UseForwardedHeaders` — metoda (w `Configure` metody *Startup.cs*) przed wywołaniem `UseAuthentication` lub podobne oprogramowanie pośredniczące schemat uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="70ad8-140">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseAuthentication` or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="70ad8-141">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="70ad8-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="70ad8-142">Wywołanie `UseForwardedHeaders` — metoda (w `Configure` metody *Startup.cs*) przed wywołaniem `UseIdentity` i `UseFacebookAuthentication` lub podobne oprogramowanie pośredniczące schemat uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="70ad8-142">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseIdentity` and `UseFacebookAuthentication` or similar authentication scheme middleware:</span></span>

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

### <a name="install-nginx"></a><span data-ttu-id="70ad8-143">Zainstaluj Nginx</span><span class="sxs-lookup"><span data-stu-id="70ad8-143">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="70ad8-144">Jeśli planujesz zainstalować opcjonalne moduły Nginx, konieczne może zbudować Nginx ze źródła.</span><span class="sxs-lookup"><span data-stu-id="70ad8-144">If you plan to install optional Nginx modules, you may be required to build Nginx from source.</span></span>

<span data-ttu-id="70ad8-145">Użyj `apt-get` do zainstalowania Nginx.</span><span class="sxs-lookup"><span data-stu-id="70ad8-145">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="70ad8-146">Instalator tworzy skrypt init System V uruchamiany Nginx jako demon podczas uruchamiania systemu.</span><span class="sxs-lookup"><span data-stu-id="70ad8-146">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="70ad8-147">Po zainstalowaniu Nginx po raz pierwszy, jawnie uruchomić:</span><span class="sxs-lookup"><span data-stu-id="70ad8-147">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="70ad8-148">Sprawdź, czy przeglądarka wyświetla domyślna strona początkowa dla Nginx.</span><span class="sxs-lookup"><span data-stu-id="70ad8-148">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="70ad8-149">Skonfiguruj Nginx</span><span class="sxs-lookup"><span data-stu-id="70ad8-149">Configure Nginx</span></span>

<span data-ttu-id="70ad8-150">Aby skonfigurować Nginx jako zwrotny serwer proxy do przekazywania żądań do naszej aplikacji platformy ASP.NET Core, zmodyfikuj `/etc/nginx/sites-available/default`.</span><span class="sxs-lookup"><span data-stu-id="70ad8-150">To configure Nginx as a reverse proxy to forward requests to our ASP.NET Core application, modify `/etc/nginx/sites-available/default`.</span></span> <span data-ttu-id="70ad8-151">Otwórz go w edytorze tekstów i Zastąp zawartość z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="70ad8-151">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="70ad8-152">Ten plik konfiguracji Nginx przekazuje ruch przychodzący publicznych z portu `80` do portu `5000`.</span><span class="sxs-lookup"><span data-stu-id="70ad8-152">This Nginx configuration file forwards incoming public traffic from port `80` to port `5000`.</span></span>

<span data-ttu-id="70ad8-153">Po zakończeniu wprowadzania zmian w konfiguracji Nginx, można uruchomić `sudo nginx -t` zweryfikować składni plików konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="70ad8-153">Once you have completed making changes to your Nginx configuration, you can run `sudo nginx -t` to verify the syntax of your configuration files.</span></span> <span data-ttu-id="70ad8-154">Jeśli test konfiguracji w pliku zakończy się pomyślnie, możesz poprosić Nginx do zastosowania zmian, uruchamiając `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="70ad8-154">If the configuration file test is successful, you can ask Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-our-application"></a><span data-ttu-id="70ad8-155">Monitorowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="70ad8-155">Monitoring our application</span></span>

<span data-ttu-id="70ad8-156">Nginx jest teraz skonfigurowana do przekazywania żądań wysyłanych do `http://yourhost:80` do aplikacji platformy ASP.NET Core systemem Kestrel na `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="70ad8-156">Nginx is now setup to forward requests made to `http://yourhost:80` on to the ASP.NET Core application running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="70ad8-157">Jednak Nginx nie skonfigurowano do zarządzania procesem Kestrel.</span><span class="sxs-lookup"><span data-stu-id="70ad8-157">However, Nginx is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="70ad8-158">Można użyć *systemd* i utworzenie pliku usługi, aby uruchomić i monitorować podstawowej aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="70ad8-158">You can use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="70ad8-159">*systemd* to system init zapewnia wiele zaawansowanych funkcji uruchamianie, zatrzymywanie oraz procesy zarządzania.</span><span class="sxs-lookup"><span data-stu-id="70ad8-159">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="70ad8-160">Tworzenie pliku usługi</span><span class="sxs-lookup"><span data-stu-id="70ad8-160">Create the service file</span></span>

<span data-ttu-id="70ad8-161">Tworzenie pliku definicji usługi:</span><span class="sxs-lookup"><span data-stu-id="70ad8-161">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="70ad8-162">Poniżej przedstawiono przykładowy plik usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="70ad8-162">The following is an example service file for our application:</span></span>

```ini
[Unit]
Description=Example .NET Web API Application running on Ubuntu

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

<span data-ttu-id="70ad8-163">**Uwaga:** Jeśli użytkownik *danych www* nie jest używana w bieżącej konfiguracji użytkownika zdefiniowane w tym miejscu należy najpierw utworzyć i podane odpowiednie własność plików.</span><span class="sxs-lookup"><span data-stu-id="70ad8-163">**Note:** If the user *www-data* is not used by your configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="70ad8-164">Zapisz plik i włączyć usługę.</span><span class="sxs-lookup"><span data-stu-id="70ad8-164">Save the file, and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="70ad8-165">Uruchom usługę i sprawdź, czy jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="70ad8-165">Start the service and verify that it is running.</span></span>

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API Application running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="70ad8-166">Zwrotny serwer proxy skonfigurowane i zarządzanych za pomocą systemd Kestrel, aplikacji sieci web jest w pełni skonfigurowane i dostępne w przeglądarce na komputerze lokalnym w `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="70ad8-166">With the reverse proxy configured and Kestrel managed through systemd, the web application is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="70ad8-167">Jest również dostępny z komputera zdalnego, blokowanie wszelkich zaporą, która może być blokowane.</span><span class="sxs-lookup"><span data-stu-id="70ad8-167">It is also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="70ad8-168">Zapoznanie się nagłówki odpowiedzi `Server` nagłówek zawiera obsługiwanej przez Kestrel aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="70ad8-168">Inspecting the response headers, the `Server` header shows the ASP.NET Core application being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="70ad8-169">Przeglądanie dzienników</span><span class="sxs-lookup"><span data-stu-id="70ad8-169">Viewing logs</span></span>

<span data-ttu-id="70ad8-170">Ponieważ aplikacja sieci web przy użyciu Kestrel odbywa się przy użyciu `systemd`, scentralizowane dziennika są rejestrowane wszystkie zdarzenia i procesów.</span><span class="sxs-lookup"><span data-stu-id="70ad8-170">Since the web application using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="70ad8-171">Jednak ten dziennik zawiera wszystkie wpisy dla wszystkich usług i procesów zarządza `systemd`.</span><span class="sxs-lookup"><span data-stu-id="70ad8-171">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="70ad8-172">Aby wyświetlić `kestrel-hellomvc.service`— określone elementy, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="70ad8-172">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="70ad8-173">Dla dalszego filtrowania, opcje czasu takich jak `--since today`, `--until 1 hour ago` lub kombinacji tych można zmniejszyć liczbę wpisów zwracane.</span><span class="sxs-lookup"><span data-stu-id="70ad8-173">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a><span data-ttu-id="70ad8-174">Zabezpieczanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="70ad8-174">Securing our application</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="70ad8-175">Włącz AppArmor</span><span class="sxs-lookup"><span data-stu-id="70ad8-175">Enable AppArmor</span></span>

<span data-ttu-id="70ad8-176">Linux zabezpieczeń modułów (LSM) tak, to platforma, która jest częścią jądra systemu Linux od 2.6 systemu Linux.</span><span class="sxs-lookup"><span data-stu-id="70ad8-176">Linux Security Modules (LSM) is a framework that is part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="70ad8-177">LSM obsługuje różne implementacje modułów zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="70ad8-177">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="70ad8-178">[AppArmor](https://wiki.ubuntu.com/AppArmor) jest LSM, implementujący systemu obowiązkowe kontroli dostępu, który umożliwia ograniczenie program do określonych zasobów.</span><span class="sxs-lookup"><span data-stu-id="70ad8-178">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="70ad8-179">Upewnij się, AppArmor jest włączony i poprawnie skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="70ad8-179">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-our-firewall"></a><span data-ttu-id="70ad8-180">Konfigurowanie naszych zapory</span><span class="sxs-lookup"><span data-stu-id="70ad8-180">Configuring our firewall</span></span>

<span data-ttu-id="70ad8-181">Zamknij Wyłącz wszystkie porty zewnętrznych, które nie są używane.</span><span class="sxs-lookup"><span data-stu-id="70ad8-181">Close off all external ports that are not in use.</span></span> <span data-ttu-id="70ad8-182">Zapora prostotę (ufw) zapewnia frontonu dla `iptables` korzystanie z interfejsu wiersza polecenia dla konfiguracji zapory.</span><span class="sxs-lookup"><span data-stu-id="70ad8-182">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="70ad8-183">Sprawdź, czy `ufw` zezwalają na ruch na portach, wszelkie potrzebne.</span><span class="sxs-lookup"><span data-stu-id="70ad8-183">Verify that `ufw` is configured to allow traffic on any ports you need.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="70ad8-184">Zabezpieczanie Nginx</span><span class="sxs-lookup"><span data-stu-id="70ad8-184">Securing Nginx</span></span>

<span data-ttu-id="70ad8-185">Rozkład domyślne Nginx nie Włącz protokół SSL.</span><span class="sxs-lookup"><span data-stu-id="70ad8-185">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="70ad8-186">Aby włączyć dodatkowe funkcje zabezpieczeń, kompilacji ze źródła.</span><span class="sxs-lookup"><span data-stu-id="70ad8-186">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="70ad8-187">Pobierz źródła i zainstaluj zależności kompilacji</span><span class="sxs-lookup"><span data-stu-id="70ad8-187">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="70ad8-188">Zmień nazwę Nginx odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="70ad8-188">Change the Nginx response name</span></span>

<span data-ttu-id="70ad8-189">Edytuj *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="70ad8-189">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Your Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Your Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="70ad8-190">Skonfiguruj opcje i kompilacji</span><span class="sxs-lookup"><span data-stu-id="70ad8-190">Configure the options and build</span></span>

<span data-ttu-id="70ad8-191">Biblioteka PCRE jest wymagany dla wyrażeń regularnych.</span><span class="sxs-lookup"><span data-stu-id="70ad8-191">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="70ad8-192">Wyrażenia regularne są używane w dyrektywie lokalizacji dla ngx_http_rewrite_module.</span><span class="sxs-lookup"><span data-stu-id="70ad8-192">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="70ad8-193">Http_ssl_module dodaje obsługę protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="70ad8-193">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="70ad8-194">Należy rozważyć użycie zapory aplikacji sieci web, takich jak *ModSecurity* celu ograniczenia funkcjonalności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="70ad8-194">Consider using a web application firewall like *ModSecurity* to harden your application.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="70ad8-195">Konfigurowanie protokołu SSL</span><span class="sxs-lookup"><span data-stu-id="70ad8-195">Configure SSL</span></span>

* <span data-ttu-id="70ad8-196">Skonfigurowanie serwera do nasłuchiwania ruchu HTTPS na porcie `443` , określając prawidłowy certyfikat wystawiony przez zaufany urząd certyfikacji.</span><span class="sxs-lookup"><span data-stu-id="70ad8-196">Configure your server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="70ad8-197">Ograniczenia funkcjonalności zabezpieczeń poprzez zastosowanie niektórych rozwiązań przedstawione poniżej */etc/nginx/nginx.conf* pliku.</span><span class="sxs-lookup"><span data-stu-id="70ad8-197">Harden your security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="70ad8-198">Przykłady obejmują wybranie silniejszego szyfrowania i przekierowywania całego ruchu za pośrednictwem protokołu HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="70ad8-198">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="70ad8-199">Dodawanie `HTTP Strict-Transport-Security` nagłówka (HSTS) zapewnia wszystkie kolejne żądania przez klienta są tylko za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="70ad8-199">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="70ad8-200">Nie dodawaj nagłówka TLS Strict lub wybrać odpowiednie `max-age` Jeśli planujesz wyłączyć protokół SSL w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="70ad8-200">Do not add the Strict-Transport-Security header or chose an appropriate `max-age` if you plan to disable SSL in the future.</span></span>

<span data-ttu-id="70ad8-201">Dodaj */etc/nginx/proxy.conf* pliku konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="70ad8-201">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[Main](linuxproduction/proxy.conf)]

<span data-ttu-id="70ad8-202">Edytuj */etc/nginx/nginx.conf* pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="70ad8-202">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="70ad8-203">Przykład zawiera zarówno `http` i `server` sekcji w pliku konfiguracji jednego.</span><span class="sxs-lookup"><span data-stu-id="70ad8-203">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[Main](../publishing/linuxproduction/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="70ad8-204">Bezpieczny Nginx z porywaniu kliknięć</span><span class="sxs-lookup"><span data-stu-id="70ad8-204">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="70ad8-205">Porywaniu kliknięć to technika złośliwego zbierać zainfekowane użytkownik klika polecenie.</span><span class="sxs-lookup"><span data-stu-id="70ad8-205">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="70ad8-206">Porywaniu kliknięć sztuczki ofiara (użytkownik) do kliknięcia zainfekowane w witrynie.</span><span class="sxs-lookup"><span data-stu-id="70ad8-206">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="70ad8-207">Użyj X-FRAME-OPTIONS do zabezpieczenia witryny.</span><span class="sxs-lookup"><span data-stu-id="70ad8-207">Use X-FRAME-OPTIONS to secure your site.</span></span>

<span data-ttu-id="70ad8-208">Edytuj *nginx.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="70ad8-208">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="70ad8-209">Dodaj wiersz `add_header X-Frame-Options "SAMEORIGIN";` i Zapisz plik, a następnie uruchom ponownie Nginx.</span><span class="sxs-lookup"><span data-stu-id="70ad8-209">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="70ad8-210">Wykrywanie typ MIME</span><span class="sxs-lookup"><span data-stu-id="70ad8-210">MIME-type sniffing</span></span>

<span data-ttu-id="70ad8-211">Ten nagłówek zapobiega w większości przeglądarek z wykrywanie MIME odpowiedzi od deklarowanego typu zawartości, jak nagłówka powoduje, że przeglądarka nie, aby zastąpić typ zawartości odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="70ad8-211">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="70ad8-212">Z `nosniff` opcję, jeśli serwer jest wyświetlany komunikat "text/html" jest zawartość przeglądarki renderuje go jako "text/html".</span><span class="sxs-lookup"><span data-stu-id="70ad8-212">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="70ad8-213">Edytuj *nginx.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="70ad8-213">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="70ad8-214">Dodaj wiersz `add_header X-Content-Type-Options "nosniff";` i Zapisz plik, a następnie uruchom ponownie Nginx.</span><span class="sxs-lookup"><span data-stu-id="70ad8-214">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
