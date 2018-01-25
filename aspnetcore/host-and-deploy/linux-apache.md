---
title: Host platformy ASP.NET Core w systemie Linux z Apache
description: "Dowiedz się, jak ustawić Apache jako zwrotny serwer proxy serwera na CentOS, aby przekierować ruch HTTP do aplikacji sieci web platformy ASP.NET Core systemem Kestrel."
author: spboyer
ms.author: spboyer
manager: wpickett
ms.custom: mvc
ms.date: 10/19/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/linux-apache
ms.openlocfilehash: caa8cce53b3982815f1eab75792a10ec390f3257
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="ae424-103">Host platformy ASP.NET Core w systemie Linux z Apache</span><span class="sxs-lookup"><span data-stu-id="ae424-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="ae424-104">Przez [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="ae424-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="ae424-105">Przy użyciu tego przewodnika, Dowiedz się, jak skonfigurować [Apache](https://httpd.apache.org/) jako zwrotny serwer proxy serwera na [CentOS 7](https://www.centos.org/) przekierowywanie ruchu HTTP do aplikacji sieci web platformy ASP.NET Core systemem [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="ae424-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="ae424-106">[Rozszerzenia mod_proxy](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) i powiązane moduły Utwórz zwrotny serwer proxy serwera.</span><span class="sxs-lookup"><span data-stu-id="ae424-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ae424-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="ae424-107">Prerequisites</span></span>

1. <span data-ttu-id="ae424-108">Serwer z systemem CentOS 7 przy użyciu konta użytkowników standardowych z uprawnieniami sudo</span><span class="sxs-lookup"><span data-stu-id="ae424-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="ae424-109">Aplikacja platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ae424-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="ae424-110">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="ae424-110">Publish the app</span></span>

<span data-ttu-id="ae424-111">Publikowanie aplikacji jako [niezależne wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd) w konfiguracji wersji środowiska uruchomieniowego CentOS 7 (`centos.7-x64`).</span><span class="sxs-lookup"><span data-stu-id="ae424-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="ae424-112">Skopiuj zawartość *bin/Release/netcoreapp2.0/centos.7-x64/publish* folderu na serwerze przy użyciu połączenia, FTP lub innej metody transferu plików.</span><span class="sxs-lookup"><span data-stu-id="ae424-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="ae424-113">W przypadku wdrożenia produkcyjnego ciągłej integracji przepływu pracy wykonuje pracę publikowania aplikacji i kopiowanie zasoby na serwerze.</span><span class="sxs-lookup"><span data-stu-id="ae424-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="ae424-114">Konfiguracja serwera proxy</span><span class="sxs-lookup"><span data-stu-id="ae424-114">Configure a proxy server</span></span>

<span data-ttu-id="ae424-115">Zwrotny serwer proxy jest typowe dla obsługi aplikacji sieci web dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="ae424-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="ae424-116">Zwrotny serwer proxy kończy żądanie HTTP i przekazuje je do aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ae424-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="ae424-117">Serwer proxy to taki, który przekazuje żądania klienta do innego serwera zamiast wykonanie samego.</span><span class="sxs-lookup"><span data-stu-id="ae424-117">A proxy server is one which forwards client requests to another server instead of fulfilling them itself.</span></span> <span data-ttu-id="ae424-118">Zwrotny serwer proxy przekazuje do stałej miejsca docelowego, zwykle w imieniu dowolnego klientów.</span><span class="sxs-lookup"><span data-stu-id="ae424-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="ae424-119">W tym przewodniku Apache jest skonfigurowana jako zwrotny serwer proxy, uruchomione na tym samym serwerze, że Kestrel służy aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ae424-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

### <a name="install-apache"></a><span data-ttu-id="ae424-120">Zainstaluj Apache</span><span class="sxs-lookup"><span data-stu-id="ae424-120">Install Apache</span></span>

<span data-ttu-id="ae424-121">Pakietów CentOS aktualizacji w celu ich najnowsze wersje stabilna:</span><span class="sxs-lookup"><span data-stu-id="ae424-121">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="ae424-122">Zainstaluj serwer sieci web Apache na CentOS za pomocą jednej `yum` polecenia:</span><span class="sxs-lookup"><span data-stu-id="ae424-122">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="ae424-123">Przykładowe dane wyjściowe po uruchomieniu polecenia:</span><span class="sxs-lookup"><span data-stu-id="ae424-123">Sample output after running the command:</span></span>

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
> <span data-ttu-id="ae424-124">W tym przykładzie danych wyjściowych odzwierciedla httpd.86_64, ponieważ wersja CentOS 7 jest 64-bitowym.</span><span class="sxs-lookup"><span data-stu-id="ae424-124">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="ae424-125">Aby sprawdzić, w którym zainstalowano Apache, uruchom `whereis httpd` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="ae424-125">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="ae424-126">Konfigurowanie Apache dla zwrotnego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="ae424-126">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="ae424-127">Pliki konfiguracji Apache znajdują się w `/etc/httpd/conf.d/` katalogu.</span><span class="sxs-lookup"><span data-stu-id="ae424-127">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="ae424-128">Dowolne pliki z *.conf* rozszerzenia są przetwarzane w kolejności alfabetycznej oprócz plików konfiguracji modułu w `/etc/httpd/conf.modules.d/`, który zawiera żadnej konfiguracji pliki niezbędne do ładowania modułów.</span><span class="sxs-lookup"><span data-stu-id="ae424-128">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="ae424-129">Utwórz plik konfiguracji dla aplikacji o nazwie `hellomvc.conf`:</span><span class="sxs-lookup"><span data-stu-id="ae424-129">Create a configuration file for the app named `hellomvc.conf`:</span></span>

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="ae424-130">**VirtualHost** węzeł może pojawić się wiele razy w jeden lub więcej plików na serwerze.</span><span class="sxs-lookup"><span data-stu-id="ae424-130">The **VirtualHost** node can appear multiple times in one or more files on a server.</span></span> <span data-ttu-id="ae424-131">**VirtualHost** ustawiono do nasłuchiwania na dowolny adres IP przy użyciu portu 80.</span><span class="sxs-lookup"><span data-stu-id="ae424-131">**VirtualHost** is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="ae424-132">Następne dwa wiersze są ustawiane na żądania serwera proxy w katalogu głównym na serwerze w 127.0.0.1 na porcie 5000.</span><span class="sxs-lookup"><span data-stu-id="ae424-132">The next two lines are set to proxy requests at the root to the server at 127.0.0.1 on port 5000.</span></span> <span data-ttu-id="ae424-133">Dla komunikacja dwukierunkowa *ProxyPass* i *ProxyPassReverse* są wymagane.</span><span class="sxs-lookup"><span data-stu-id="ae424-133">For bi-directional communication, *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="ae424-134">Można skonfigurować rejestrowania **VirtualHost** przy użyciu **ErrorLog** i **CustomLog** dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="ae424-134">Logging can be configured per **VirtualHost** using **ErrorLog** and **CustomLog** directives.</span></span> <span data-ttu-id="ae424-135">**Dziennik błędów** to lokalizacja, w którym serwer rejestruje błędy, i **CustomLog** ustawia nazwę pliku i format pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="ae424-135">**ErrorLog** is the location where the server logs errors, and **CustomLog** sets the filename and format of log file.</span></span> <span data-ttu-id="ae424-136">W tym przypadku jest to, gdzie jest rejestrowane informacje o żądaniu.</span><span class="sxs-lookup"><span data-stu-id="ae424-136">In this case, this is where request information is logged.</span></span> <span data-ttu-id="ae424-137">Istnieje jeden wiersz dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="ae424-137">There's one line for each request.</span></span>

<span data-ttu-id="ae424-138">Zapisz plik i Przetestuj konfigurację.</span><span class="sxs-lookup"><span data-stu-id="ae424-138">Save the file and test the configuration.</span></span> <span data-ttu-id="ae424-139">Jeśli wszystko przebiegnie pomyślnie, powinien być odpowiedzi `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="ae424-139">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="ae424-140">Uruchom ponownie Apache:</span><span class="sxs-lookup"><span data-stu-id="ae424-140">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="ae424-141">Monitorowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="ae424-141">Monitoring the app</span></span>

<span data-ttu-id="ae424-142">Apache jest teraz skonfigurowana do przekazywania żądań wysyłanych do `http://localhost:80` do aplikacji platformy ASP.NET Core systemem Kestrel na `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="ae424-142">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="ae424-143">Apache Konfigurowanie nie jest jednak do zarządzania procesem Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ae424-143">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="ae424-144">Użyj *systemd* i utworzenie pliku usługi, aby uruchomić i monitorować podstawowej aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="ae424-144">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="ae424-145">*systemd* to system init zapewnia wiele zaawansowanych funkcji uruchamianie, zatrzymywanie oraz procesy zarządzania.</span><span class="sxs-lookup"><span data-stu-id="ae424-145">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="ae424-146">Tworzenie pliku usługi</span><span class="sxs-lookup"><span data-stu-id="ae424-146">Create the service file</span></span>

<span data-ttu-id="ae424-147">Tworzenie pliku definicji usługi:</span><span class="sxs-lookup"><span data-stu-id="ae424-147">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="ae424-148">Przykładowy plik usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="ae424-148">An example service file for the app:</span></span>

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
> <span data-ttu-id="ae424-149">**Użytkownik** &mdash; Jeśli użytkownik *apache* nie jest używany przez tę konfigurację, użytkownik musi najpierw utworzyć i podane odpowiednie własność plików.</span><span class="sxs-lookup"><span data-stu-id="ae424-149">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="ae424-150">Zapisz plik i włączyć usługę:</span><span class="sxs-lookup"><span data-stu-id="ae424-150">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="ae424-151">Uruchom usługę i sprawdź, czy jest uruchomiona:</span><span class="sxs-lookup"><span data-stu-id="ae424-151">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="ae424-152">Przy użyciu zwrotnego serwera proxy skonfigurowane i Kestrel zarządzanych za pomocą *systemd*, aplikacji sieci web jest w pełni skonfigurowane i dostępne w przeglądarce na komputerze lokalnym w `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="ae424-152">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="ae424-153">Zapoznanie się nagłówki odpowiedzi **serwera** nagłówek wskazuje, że aplikacja platformy ASP.NET Core jest obsługiwana przez Kestrel:</span><span class="sxs-lookup"><span data-stu-id="ae424-153">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="ae424-154">Przeglądanie dzienników</span><span class="sxs-lookup"><span data-stu-id="ae424-154">Viewing logs</span></span>

<span data-ttu-id="ae424-155">Ponieważ aplikacja sieci web przy użyciu Kestrel odbywa się przy użyciu *systemd*, scentralizowane dziennika są rejestrowane zdarzenia i procesów.</span><span class="sxs-lookup"><span data-stu-id="ae424-155">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="ae424-156">Jednak ten dziennik zawiera wpisy dla wszystkich usług i procesów zarządza *systemd*.</span><span class="sxs-lookup"><span data-stu-id="ae424-156">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="ae424-157">Aby wyświetlić `kestrel-hellomvc.service`— określone elementy, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="ae424-157">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="ae424-158">Potrzeby filtrowania czasu, określ opcje czasu przy użyciu polecenia.</span><span class="sxs-lookup"><span data-stu-id="ae424-158">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="ae424-159">Na przykład użyć `--since today` filtrowania dla bieżącego dnia lub `--until 1 hour ago` wyświetlić wpisy poprzedniej godziny.</span><span class="sxs-lookup"><span data-stu-id="ae424-159">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="ae424-160">Aby uzyskać więcej informacji, zobacz [man stronę journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span><span class="sxs-lookup"><span data-stu-id="ae424-160">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="ae424-161">Zabezpieczanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="ae424-161">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="ae424-162">Konfigurowanie zapory</span><span class="sxs-lookup"><span data-stu-id="ae424-162">Configure firewall</span></span>

<span data-ttu-id="ae424-163">*Firewalld* jest demon dynamiczne zarządzanie zaporą z obsługą stref sieci.</span><span class="sxs-lookup"><span data-stu-id="ae424-163">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="ae424-164">Porty i filtrowanie pakietów można nadal będą zarządzane przez iptables.</span><span class="sxs-lookup"><span data-stu-id="ae424-164">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="ae424-165">*Firewalld* powinny być instalowane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="ae424-165">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="ae424-166">`yum`może służyć do zainstalowania pakietu lub sprawdź, czy jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="ae424-166">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="ae424-167">Użyj `firewalld` można otworzyć tylko te porty, które są wymagane dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ae424-167">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="ae424-168">W takim przypadku portu 80 i 443 są używane.</span><span class="sxs-lookup"><span data-stu-id="ae424-168">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="ae424-169">Porty 80 i 443, aby otworzyć na stałe ustawiony program następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="ae424-169">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="ae424-170">Ponowne załadowanie ustawień zapory.</span><span class="sxs-lookup"><span data-stu-id="ae424-170">Reload the firewall settings.</span></span> <span data-ttu-id="ae424-171">Sprawdź dostępnych usług i portów w strefie domyślnej.</span><span class="sxs-lookup"><span data-stu-id="ae424-171">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="ae424-172">Opcje są dostępne, sprawdzając `firewall-cmd -h`.</span><span class="sxs-lookup"><span data-stu-id="ae424-172">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="ae424-173">Konfiguracja protokołu SSL</span><span class="sxs-lookup"><span data-stu-id="ae424-173">SSL configuration</span></span>

<span data-ttu-id="ae424-174">Aby skonfigurować Apache dla protokołu SSL, *mod_ssl* moduł jest używany.</span><span class="sxs-lookup"><span data-stu-id="ae424-174">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="ae424-175">Gdy *host z wieloma adresami* moduł został zainstalowany, *mod_ssl* modułu również został zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="ae424-175">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="ae424-176">Jeśli nie została zainstalowana za pomocą `yum` Aby dodać je do konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="ae424-176">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="ae424-177">Aby wymusić protokołu SSL, należy zainstalować `mod_rewrite` modułu, aby umożliwić ponowne zapisywanie adresów URL:</span><span class="sxs-lookup"><span data-stu-id="ae424-177">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="ae424-178">Modyfikowanie *hellomvc.conf* plik, aby umożliwić ponowne zapisywanie adresów URL i zabezpieczania komunikacji na porcie 443:</span><span class="sxs-lookup"><span data-stu-id="ae424-178">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="ae424-179">W tym przykładzie używa certyfikatu, generowany lokalnie.</span><span class="sxs-lookup"><span data-stu-id="ae424-179">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="ae424-180">**SSLCertificateFile** powinien być plikiem podstawowego certyfikatu dla nazwy domeny.</span><span class="sxs-lookup"><span data-stu-id="ae424-180">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="ae424-181">**SSLCertificateKeyFile** powinny być pliku klucza generowane po utworzeniu renderowanie po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="ae424-181">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="ae424-182">**SSLCertificateChainFile** powinien być pliku certyfikatu pośredniego (jeśli istnieją) podane przez urząd certyfikacji.</span><span class="sxs-lookup"><span data-stu-id="ae424-182">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="ae424-183">Zapisz plik i testowanie konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="ae424-183">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="ae424-184">Uruchom ponownie Apache:</span><span class="sxs-lookup"><span data-stu-id="ae424-184">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="ae424-185">Dodatkowe sugestie Apache</span><span class="sxs-lookup"><span data-stu-id="ae424-185">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="ae424-186">Dodatkowych nagłówków</span><span class="sxs-lookup"><span data-stu-id="ae424-186">Additional headers</span></span>

<span data-ttu-id="ae424-187">Aby zabezpieczyć przed atakami, istnieje kilka nagłówki, które powinny być zmodyfikowany lub dodane.</span><span class="sxs-lookup"><span data-stu-id="ae424-187">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="ae424-188">Upewnij się, że `mod_headers` moduł jest zainstalowany:</span><span class="sxs-lookup"><span data-stu-id="ae424-188">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="ae424-189">Zabezpieczenia przed atakami porywaniu kliknięć Apache</span><span class="sxs-lookup"><span data-stu-id="ae424-189">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="ae424-190">[Porywaniu kliknięć](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), znanej także jako *interfejsu użytkownika odszkodowania ataku*, jest złośliwymi atakami, gdy obiekt odwiedzający witrynę sieci Web jest zachęceni do kliknięcia łącza lub przycisku na innej stronie, nie są one obecnie odwiedzający.</span><span class="sxs-lookup"><span data-stu-id="ae424-190">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="ae424-191">Użyj `X-FRAME-OPTIONS` do zabezpieczenia witryny.</span><span class="sxs-lookup"><span data-stu-id="ae424-191">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="ae424-192">Edytuj *httpd.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="ae424-192">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="ae424-193">Dodaj wiersz `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span><span class="sxs-lookup"><span data-stu-id="ae424-193">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="ae424-194">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="ae424-194">Save the file.</span></span> <span data-ttu-id="ae424-195">Uruchom ponownie Apache.</span><span class="sxs-lookup"><span data-stu-id="ae424-195">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="ae424-196">Wykrywanie typ MIME</span><span class="sxs-lookup"><span data-stu-id="ae424-196">MIME-type sniffing</span></span>

<span data-ttu-id="ae424-197">`X-Content-Type-Options` Nagłówka uniemożliwia programowi Internet Explorer z *wykrywanie MIME* (Określanie pliku `Content-Type` z zawartości pliku).</span><span class="sxs-lookup"><span data-stu-id="ae424-197">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determing a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="ae424-198">Jeśli serwer ustawia `Content-Type` nagłówka do `text/html` z `nosniff` zestaw opcji, program Internet Explorer renderuje zawartość jako `text/html` niezależnie od zawartości pliku.</span><span class="sxs-lookup"><span data-stu-id="ae424-198">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="ae424-199">Edytuj *httpd.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="ae424-199">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="ae424-200">Dodaj wiersz `Header set X-Content-Type-Options "nosniff"`.</span><span class="sxs-lookup"><span data-stu-id="ae424-200">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="ae424-201">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="ae424-201">Save the file.</span></span> <span data-ttu-id="ae424-202">Uruchom ponownie Apache.</span><span class="sxs-lookup"><span data-stu-id="ae424-202">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="ae424-203">Równoważenie obciążenia</span><span class="sxs-lookup"><span data-stu-id="ae424-203">Load Balancing</span></span> 

<span data-ttu-id="ae424-204">W tym przykładzie przedstawiono sposób instalacji i konfiguracji Apache CentOS 7 i Kestrel na tym samym komputerze wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ae424-204">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="ae424-205">Aby można było ma pojedynczego punktu awarii; przy użyciu *mod_proxy_balancer* i modyfikowanie **VirtualHost** umożliwiałyby zarządzania wiele wystąpień aplikacji sieci web za serwerem proxy Apache.</span><span class="sxs-lookup"><span data-stu-id="ae424-205">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing mutliple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="ae424-206">W pliku konfiguracyjnym pokazano poniżej, dodatkowego wystąpienia `hellomvc` aplikacji jest skonfigurowana do uruchamiania na porcie 5001.</span><span class="sxs-lookup"><span data-stu-id="ae424-206">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="ae424-207">*Proxy* sekcji ustawiono na potrzeby równoważenia obciążenia z konfiguracją modułu równoważenia z dwóch członków *byrequests*.</span><span class="sxs-lookup"><span data-stu-id="ae424-207">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="ae424-208">Limity szybkości</span><span class="sxs-lookup"><span data-stu-id="ae424-208">Rate Limits</span></span>
<span data-ttu-id="ae424-209">Przy użyciu *mod_ratelimit*, który znajduje się w *host z wieloma adresami* moduł, może być ograniczona przepustowość klientów:</span><span class="sxs-lookup"><span data-stu-id="ae424-209">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="ae424-210">Przykładowy plik ogranicza przepustowość jako 600 KB/s w lokalizacji głównej:</span><span class="sxs-lookup"><span data-stu-id="ae424-210">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
