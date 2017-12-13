---
title: Host platformy ASP.NET Core w systemie Linux z Apache
description: "Dowiedz się, jak skonfigurować Apache jako zwrotny serwer proxy serwera na CentOS do przekierowywania ruchu HTTP na uruchomioną Kestrel aplikację sieci web platformy ASP.NET Core."
keywords: Platformy ASP.NET Core, Apache, CentOS, wstecznego serwera Proxy, Linux, mod_proxy, host z wieloma adresami, hostingu
author: spboyer
ms.author: spboyer
manager: wpickett
ms.date: 10/19/2016
ms.topic: article
ms.assetid: fa9b0cb7-afb3-4361-9e7e-33afffeaca0c
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/apache-proxy
ms.openlocfilehash: 34ede2fdebe0e9516f62e39618e7adba3c8c89db
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-apache-and-deploy-to-it"></a><span data-ttu-id="2a9f2-104">Konfigurowanie środowiska macierzystego dla platformy ASP.NET Core w systemie Linux z Apache i wdrażać do niego</span><span class="sxs-lookup"><span data-stu-id="2a9f2-104">Set up a hosting environment for ASP.NET Core on Linux with Apache, and deploy to it</span></span>

<span data-ttu-id="2a9f2-105">Przez [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="2a9f2-105">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="2a9f2-106">Apache jest bardzo popularny serwera HTTP i może być skonfigurowany jako serwer proxy do przekierowywania ruchu HTTP podobny do nginx.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-106">Apache is a very popular HTTP server and can be configured as a proxy to redirect HTTP traffic similar to nginx.</span></span> <span data-ttu-id="2a9f2-107">W tym przewodniku firma Microsoft będzie Dowiedz się, jak skonfigurować Apache na CentOS 7 i użyć jej jako zwrotny serwer proxy do Witamy połączenia przychodzące i Przekierowanie do aplikacji platformy ASP.NET Core systemem Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-107">In this guide, we will learn how to set up Apache on CentOS 7 and use it as a reverse proxy to welcome incoming connections and redirect them to the ASP.NET Core application running on Kestrel.</span></span> <span data-ttu-id="2a9f2-108">W tym celu użyjemy *mod_proxy* rozszerzenia i inne powiązane moduły Apache.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-108">For this purpose, we will use the *mod_proxy* extension and other related Apache modules.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2a9f2-109">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="2a9f2-109">Prerequisites</span></span>

1. <span data-ttu-id="2a9f2-110">Serwer z systemem CentOS 7 przy użyciu konta użytkowników standardowych z uprawnieniem sudo.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-110">A server running CentOS 7, with a standard user account with sudo privilege.</span></span>
2. <span data-ttu-id="2a9f2-111">Istniejącej aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-111">An existing ASP.NET Core application.</span></span> 

## <a name="publish-your-application"></a><span data-ttu-id="2a9f2-112">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="2a9f2-112">Publish your application</span></span>

<span data-ttu-id="2a9f2-113">Uruchom `dotnet publish -c Release` ze środowiska deweloperskiego do pakietu aplikacji do katalogu niezależne, które mogą być uruchamiane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-113">Run `dotnet publish -c Release` from your development environment to package your application into a self-contained directory that can run on your server.</span></span> <span data-ttu-id="2a9f2-114">Opublikowana aplikacja należy następnie skopiować na serwerze przy użyciu połączenia, FTP lub innej metody transferu plików.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-114">The published application must then be copied to the server using SCP, FTP or other file transfer method.</span></span> 

> [!NOTE]
> <span data-ttu-id="2a9f2-115">W przypadku wdrożenia produkcyjnego ciągłej integracji przepływu pracy wykonuje pracę publikowania aplikacji i kopiowanie zasoby na serwerze.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-115">Under a production deployment scenario, a continuous integration workflow does the work of publishing the application and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="2a9f2-116">Konfiguracja serwera proxy</span><span class="sxs-lookup"><span data-stu-id="2a9f2-116">Configure a proxy server</span></span>

<span data-ttu-id="2a9f2-117">Zwrotny serwer proxy jest typowe dla obsługująca dynamicznych aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-117">A reverse proxy is a common setup for serving dynamic web applications.</span></span> <span data-ttu-id="2a9f2-118">Zwrotny serwer proxy kończy żądanie HTTP i przekazuje je do aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-118">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET application.</span></span>

<span data-ttu-id="2a9f2-119">Serwer proxy to taki, który przekazuje żądania klienta do innego serwera zamiast wykonanie samego.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-119">A proxy server is one which forwards client requests to another server instead of fulfilling them itself.</span></span> <span data-ttu-id="2a9f2-120">Zwrotny serwer proxy przekazuje do stałej miejsca docelowego, zwykle w imieniu dowolnego klientów.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-120">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="2a9f2-121">W tym przewodniku Apache jest konfigurowany jako zwrotny serwer proxy, uruchomionych na tym samym serwerze, że Kestrel służy aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-121">In this guide, Apache is being configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core application.</span></span> 

<span data-ttu-id="2a9f2-122">Każda aplikacja może występować na oddzielnych komputerów fizycznych, kontenery Docker lub kombinację konfiguracji, w zależności od ograniczeń lub architektury potrzeb.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-122">Each piece of the application can exist on separate physical machines, Docker containers, or a combination of configurations depending on your architectural needs or restrictions.</span></span>

### <a name="install-apache"></a><span data-ttu-id="2a9f2-123">Zainstaluj Apache</span><span class="sxs-lookup"><span data-stu-id="2a9f2-123">Install Apache</span></span>

<span data-ttu-id="2a9f2-124">Zainstalowanie serwera sieci web programu Apache na CentOS jest teraz jednego polecenia, ale pierwszym naszych pakietów aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-124">Installing the Apache web server on CentOS is a single command, but first let's update our packages.</span></span>

```bash
    sudo yum update -y
```

<span data-ttu-id="2a9f2-125">Dzięki temu wszystkie zainstalowane pakiety zostały zaktualizowane do swoich najnowszej wersji.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-125">This ensures that all of the installed packages are updated to their latest version.</span></span> <span data-ttu-id="2a9f2-126">Instalowanie przy użyciu Apache`yum`</span><span class="sxs-lookup"><span data-stu-id="2a9f2-126">Install Apache using `yum`</span></span>

```bash
    sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="2a9f2-127">Dane wyjściowe powinien odzwierciedlać podobny do następującego.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-127">The output should reflect something similar to the following.</span></span>

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
> <span data-ttu-id="2a9f2-128">W tym przykładzie httpd.86_64 odzwierciedla dane wyjściowe, ponieważ wersja CentOS 7 jest 64-bitowym.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-128">In this example the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="2a9f2-129">Dane wyjściowe mogą być inne dla serwera.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-129">The output may be different for your server.</span></span> <span data-ttu-id="2a9f2-130">Aby sprawdzić, w którym zainstalowano Apache, uruchom `whereis httpd` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-130">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="2a9f2-131">Konfigurowanie Apache dla zwrotnego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="2a9f2-131">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="2a9f2-132">Pliki konfiguracji Apache znajdują się w `/etc/httpd/conf.d/` katalogu.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-132">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="2a9f2-133">Dowolne pliki z **.conf** rozszerzenia będą przetwarzane w kolejności alfabetycznej oprócz plików konfiguracji modułu w `/etc/httpd/conf.modules.d/`, który zawiera żadnej konfiguracji pliki niezbędne do ładowania modułów.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-133">Any file with the **.conf** extension will be processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="2a9f2-134">Utwórz plik konfiguracji aplikacji, w tym przykładzie, że Zadzwonimy go`hellomvc.conf`</span><span class="sxs-lookup"><span data-stu-id="2a9f2-134">Create a configuration file for your app, for this example we'll call it `hellomvc.conf`</span></span>

```text
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
    </VirtualHost>
```

<span data-ttu-id="2a9f2-135">*VirtualHost* węzła, którego może istnieć wiele w pliku lub na serwerze w wielu plikach ustawiono do nasłuchiwania na dowolny adres IP przy użyciu portu 80.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-135">The *VirtualHost* node, of which there can be multiple in a file or on a server in many files, is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="2a9f2-136">Następne dwa wiersze są ustawione na przekazywanie wszystkich żądań odebranych w katalogu głównym port maszyny 127.0.0.1 5000 i odwrotnie.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-136">The next two lines are set to pass all requests received at the root to the machine 127.0.0.1 port 5000 and in reverse.</span></span> <span data-ttu-id="2a9f2-137">Celu zapewnienia dwukierunkowej komunikacji, oba ustawienia *ProxyPass* i *ProxyPassReverse* są wymagane.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-137">For there to be bi-directional communication, both settings *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="2a9f2-138">Można skonfigurować rejestrowania na VirtualHost przy użyciu *ErrorLog* i *CustomLog* dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-138">Logging can be configured per VirtualHost using *ErrorLog* and *CustomLog* directives.</span></span> <span data-ttu-id="2a9f2-139">*Dziennik błędów* to lokalizacja, w której serwer będzie rejestrować błędy i *CustomLog* ustawia nazwę pliku i format pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-139">*ErrorLog* is the location where the server will log errors and *CustomLog* sets the filename and format of log file.</span></span> <span data-ttu-id="2a9f2-140">W tym przypadku jest to, którym będą rejestrowane informacje o żądaniu.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-140">In our case this is where request information will be logged.</span></span> <span data-ttu-id="2a9f2-141">Będzie jeden wiersz dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-141">There will be one line for each request.</span></span>

<span data-ttu-id="2a9f2-142">Zapisz plik i przetestowania konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-142">Save the file, and test the configuration.</span></span> <span data-ttu-id="2a9f2-143">Jeśli wszystko przebiegnie pomyślnie, powinien być odpowiedzi `Syntax [OK]`.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-143">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
    sudo service httpd configtest
```

<span data-ttu-id="2a9f2-144">Uruchom ponownie Apache.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-144">Restart Apache.</span></span>

```text
    sudo systemctl restart httpd
    sudo systemctl enable httpd
```

## <a name="monitoring-our-application"></a><span data-ttu-id="2a9f2-145">Monitorowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="2a9f2-145">Monitoring our application</span></span>

<span data-ttu-id="2a9f2-146">Apache jest teraz skonfigurowana do przekazywania żądań wysyłanych do `http://localhost:80` do aplikacji platformy ASP.NET Core systemem Kestrel na `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-146">Apache is now setup to forward requests made to `http://localhost:80` on to the ASP.NET Core application running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="2a9f2-147">Jednak Apache nie skonfigurowano do zarządzania procesem Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-147">However, Apache is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="2a9f2-148">Używamy *systemd* i utworzenie pliku usługi, aby uruchomić i monitorować podstawowej aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-148">We will use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="2a9f2-149">*systemd* to system init zapewnia wiele zaawansowanych funkcji uruchamianie, zatrzymywanie oraz procesy zarządzania.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-149">*systemd* is an init system that provides many powerful features for starting, stopping and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="2a9f2-150">Tworzenie pliku usługi</span><span class="sxs-lookup"><span data-stu-id="2a9f2-150">Create the service file</span></span>

<span data-ttu-id="2a9f2-151">Tworzenie pliku definicji usługi</span><span class="sxs-lookup"><span data-stu-id="2a9f2-151">Create the service definition file</span></span> 

```bash
    sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="2a9f2-152">Przykładowy plik usługi dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-152">An example service file for our application.</span></span>

```text
[Unit]
    Description=Example .NET Web API Application running on CentOS 7

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
> <span data-ttu-id="2a9f2-153">**Użytkownik** — Jeśli użytkownik *apache* nie jest używana w bieżącej konfiguracji użytkownika zdefiniowane w tym miejscu należy najpierw utworzyć i podane odpowiednie własność plików</span><span class="sxs-lookup"><span data-stu-id="2a9f2-153">**User** -- If the user *apache* is not used by your configuration, the user defined here must be created first and given proper ownership for files</span></span>

<span data-ttu-id="2a9f2-154">Zapisz plik i włączyć usługę.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-154">Save the file and enable the service.</span></span>

```bash
    systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="2a9f2-155">Uruchom usługę i sprawdź, czy jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-155">Start the service and verify that it is running.</span></span>

```
    systemctl start kestrel-hellomvc.service
    systemctl status kestrel-hellomvc.service

    ● kestrel-hellomvc.service - Example .NET Web API Application running on CentOS 7
        Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
        Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
    Main PID: 9021 (dotnet)
        CGroup: /system.slice/kestrel-hellomvc.service
                └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="2a9f2-156">Zwrotny serwer proxy skonfigurowane i zarządzanych za pomocą systemd Kestrel, aplikacji sieci web jest w pełni skonfigurowane i dostępne w przeglądarce na komputerze lokalnym w `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-156">With the reverse proxy configured and Kestrel managed through systemd, the web application is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="2a9f2-157">Zapoznanie się nagłówki odpowiedzi **serwera** pozostanie obsługiwanej przez Kestrel aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-157">Inspecting the response headers, the **Server** still shows the ASP.NET Core application being served by Kestrel.</span></span>

```text
    HTTP/1.1 200 OK
    Date: Tue, 11 Oct 2016 16:22:23 GMT
    Server: Kestrel
    Keep-Alive: timeout=5, max=98
    Connection: Keep-Alive
    Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="2a9f2-158">Przeglądanie dzienników</span><span class="sxs-lookup"><span data-stu-id="2a9f2-158">Viewing logs</span></span>

<span data-ttu-id="2a9f2-159">Ponieważ do aplikacji sieci web przy użyciu Kestrel odbywa się przy użyciu systemd, scentralizowane dziennika są rejestrowane wszystkie zdarzenia i procesy.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-159">Since the web application using Kestrel is managed using systemd, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="2a9f2-160">Jednak ten dziennik zawiera wszystkie wpisy dla wszystkich usług i procesów zarządza systemd.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-160">However, this journal includes all entries for all services and processes managed by systemd.</span></span> <span data-ttu-id="2a9f2-161">Aby wyświetlić `kestrel-hellomvc.service` określone elementy, użyj następującego polecenia.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-161">To view the `kestrel-hellomvc.service` specific items, use the following command.</span></span>

```bash
    sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="2a9f2-162">Dla dalszego filtrowania, opcje czasu takich jak `--since today`, `--until 1 hour ago` lub kombinacji tych można zmniejszyć liczbę wpisów zwracane.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-162">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
    sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a><span data-ttu-id="2a9f2-163">Zabezpieczanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="2a9f2-163">Securing our application</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="2a9f2-164">Konfigurowanie zapory</span><span class="sxs-lookup"><span data-stu-id="2a9f2-164">Configure firewall</span></span>

<span data-ttu-id="2a9f2-165">*Firewalld* jest demon dynamiczne zarządzanie zaporą z obsługą stref sieci, mimo że iptables można nadal używać do zarządzania portów i filtrowanie pakietów.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-165">*Firewalld* is a dynamic daemon to manage firewall with support for network zones, although you can still use iptables to manage ports and packet filtering.</span></span> <span data-ttu-id="2a9f2-166">Domyślnie, należy zainstalować Firewalld `yum` może służyć do zainstalowania pakietu lub weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-166">Firewalld should be installed by default, `yum` can be used to install the package or verify.</span></span>

```bash
    sudo yum install firewalld -y
```

<span data-ttu-id="2a9f2-167">Przy użyciu `firewalld` można otworzyć tylko te porty, które są wymagane dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-167">Using `firewalld` you can open only the ports needed for the application.</span></span> <span data-ttu-id="2a9f2-168">W takim przypadku portu 80 i 443 są używane.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-168">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="2a9f2-169">Następujące polecenia ustawia trwale je otworzyć.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-169">The following commands permanently sets these to open.</span></span>

```bash
    sudo firewall-cmd --add-port=80/tcp --permanent
    sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="2a9f2-170">Załaduj ponownie ustawienia zapory i sprawdź dostępnych usług i portów w strefie domyślnej.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-170">Reload the firewall settings, and check the available services and ports in the default zone.</span></span> <span data-ttu-id="2a9f2-171">Opcje są dostępne, sprawdzając`firewall-cmd -h`</span><span class="sxs-lookup"><span data-stu-id="2a9f2-171">Options are available by inspecting `firewall-cmd -h`</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="2a9f2-172">Konfiguracja protokołu SSL</span><span class="sxs-lookup"><span data-stu-id="2a9f2-172">SSL configuration</span></span>

<span data-ttu-id="2a9f2-173">Aby skonfigurować Apache dla protokołu SSL, moduł mod_ssl jest używany.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-173">To configure Apache for SSL, the mod_ssl module is used.</span></span>  <span data-ttu-id="2a9f2-174">To zainstalowano początkowo możemy zainstalowany `httpd` modułu.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-174">This was installed initially when we installed the `httpd` module.</span></span> <span data-ttu-id="2a9f2-175">Jeżeli zostało pominięte lub nie jest zainstalowany, należy dodać ją do konfiguracji za pomocą yum.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-175">If it was missed or not installed, use yum to add it to your configuration.</span></span>

```bash
    sudo yum install mod_ssl
```
<span data-ttu-id="2a9f2-176">Aby wymusić protokołu SSL, należy zainstalować`mod_rewrite`</span><span class="sxs-lookup"><span data-stu-id="2a9f2-176">To enforce SSL, install `mod_rewrite`</span></span>

```bash
    sudo yum install mod_rewrite
```

<span data-ttu-id="2a9f2-177">`hellomvc.conf` Pliku, który został utworzony dla w tym przykładzie musi zostać zmodyfikowane w celu włączenia poprawione, a także dodawanie nowej **VirtualHost** sekcji dla protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-177">The `hellomvc.conf` file that was created for this example needs to be modified to enable the rewrite as well as adding the new **VirtualHost** section for HTTPS.</span></span>

```text
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
> <span data-ttu-id="2a9f2-178">W tym przykładzie używa certyfikatu, wygenerowaną lokalnie.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-178">This example is using a locally generated certificate.</span></span> <span data-ttu-id="2a9f2-179">**SSLCertificateFile** powinien być pliku podstawowego certyfikatu dla nazwy domeny.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-179">**SSLCertificateFile** should be your primary certificate file for your domain name.</span></span> <span data-ttu-id="2a9f2-180">**SSLCertificateKeyFile** powinien być generowany po utworzeniu obsługi pliku klucza.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-180">**SSLCertificateKeyFile** should be the key file generated when you created the CSR.</span></span> <span data-ttu-id="2a9f2-181">**SSLCertificateChainFile** powinien być pliku certyfikatu pośredniego (jeśli istnieją) podane przez urzędu certyfikacji</span><span class="sxs-lookup"><span data-stu-id="2a9f2-181">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by your certificate authority</span></span>

<span data-ttu-id="2a9f2-182">Zapisz plik i przetestowania konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-182">Save the file, and test the configuration.</span></span>

```bash
    sudo service httpd configtest
```

<span data-ttu-id="2a9f2-183">Uruchom ponownie Apache.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-183">Restart Apache.</span></span>

```bash
    sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="2a9f2-184">Dodatkowe sugestie Apache</span><span class="sxs-lookup"><span data-stu-id="2a9f2-184">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="2a9f2-185">Dodatkowych nagłówków</span><span class="sxs-lookup"><span data-stu-id="2a9f2-185">Additional Headers</span></span> 
<span data-ttu-id="2a9f2-186">Aby zabezpieczyć przed złośliwymi atakami są kilka nagłówki, które powinny być zmodyfikowany lub dodać.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-186">In order to secure against malicious attacks there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="2a9f2-187">Upewnij się, że `mod_headers` moduł jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-187">Ensure that the `mod_headers` module is installed.</span></span>

```bash
    sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking"></a><span data-ttu-id="2a9f2-188">Bezpieczny Apache z porywaniu kliknięć</span><span class="sxs-lookup"><span data-stu-id="2a9f2-188">Secure Apache from clickjacking</span></span>
<span data-ttu-id="2a9f2-189">Porywaniu kliknięć to technika złośliwego zbierać zainfekowane użytkownik klika polecenie.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-189">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="2a9f2-190">Porywaniu kliknięć sztuczki ofiara (użytkownik) do kliknięcia zainfekowane w witrynie.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-190">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="2a9f2-191">Użyj X-FRAME-OPTIONS do zabezpieczenia witryny.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-191">Use X-FRAME-OPTIONS to secure your site.</span></span>

<span data-ttu-id="2a9f2-192">Edytuj plik httpd.conf.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-192">Edit the httpd.conf file.</span></span>

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="2a9f2-193">Dodaj wiersz `Header append X-FRAME-OPTIONS "SAMEORIGIN"` i Zapisz plik, a następnie uruchom ponownie Apache.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-193">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"` and save the file, then restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="2a9f2-194">Wykrywanie typ MIME</span><span class="sxs-lookup"><span data-stu-id="2a9f2-194">MIME-type sniffing</span></span>

<span data-ttu-id="2a9f2-195">Ten nagłówek uniemożliwia programowi Internet Explorer z wykrywanie MIME odpowiedzi od deklarowanego typu zawartości jako nagłówek powoduje, że przeglądarka nie, aby zastąpić typ zawartości odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-195">This header prevents Internet Explorer from MIME-sniffing a response away from the declared content-type as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="2a9f2-196">Z opcją nosniff Jeśli na serwerze mówi się, że zawartość jest text/html, przeglądarka wyświetli go jako tekst i html.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-196">With the nosniff option, if the server says the content is text/html, the browser will render it as text/html.</span></span>

<span data-ttu-id="2a9f2-197">Edytuj plik httpd.conf.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-197">Edit the httpd.conf file.</span></span>

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="2a9f2-198">Dodaj wiersz `Header set X-Content-Type-Options "nosniff"` i Zapisz plik, a następnie uruchom ponownie Apache.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-198">Add the line `Header set X-Content-Type-Options "nosniff"` and save the file, then restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="2a9f2-199">Równoważenie obciążenia</span><span class="sxs-lookup"><span data-stu-id="2a9f2-199">Load Balancing</span></span> 

<span data-ttu-id="2a9f2-200">W tym przykładzie przedstawiono sposób instalacji i konfiguracji Apache CentOS 7 i Kestrel na tym samym komputerze wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-200">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span>  <span data-ttu-id="2a9f2-201">Jednak, aby nie mieć pojedynczy punkt awarii; przy użyciu *mod_proxy_balancer* i modyfikowanie VirtualHost czy umożliwiają zarządzanie wiele wystąpień aplikacji sieci web za serwerem proxy Apache.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-201">However, in order to not have a single point of failure; using *mod_proxy_balancer* and modifying the VirtualHost would allow for managing mutliple instances of the web applications behind the Apache proxy server.</span></span>

```bash
    sudo yum install mod_proxy_balancer
```

<span data-ttu-id="2a9f2-202">W pliku konfiguracyjnym dodatkowego wystąpienia `hellomvc` aplikacji została skonfigurowana do uruchamiania na porcie 5001 i *Proxy* sekcja została ustawiona w konfiguracji usługi równoważenia z dwóch członków na potrzeby równoważenia obciążenia *byrequests* .</span><span class="sxs-lookup"><span data-stu-id="2a9f2-202">In the configuration file, an additional instance of the `hellomvc` app has been setup to run on port 5001 and the *Proxy* section has been set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```text
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

### <a name="rate-limits"></a><span data-ttu-id="2a9f2-203">Limity szybkości</span><span class="sxs-lookup"><span data-stu-id="2a9f2-203">Rate Limits</span></span>
<span data-ttu-id="2a9f2-204">Przy użyciu `mod_ratelimit`, który znajduje się w `htttpd` modułu można ograniczyć przepustowość klientów.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-204">Using `mod_ratelimit`, which is included in the `htttpd` module you can limit the amount of bandwidth of clients.</span></span> 

```bash
    sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="2a9f2-205">Przykładowy plik ogranicza przepustowość jako 600 KB/s w lokalizacji głównej.</span><span class="sxs-lookup"><span data-stu-id="2a9f2-205">The example file limits bandwidth as 600 KB/sec under the root location.</span></span>

```text
    <IfModule mod_ratelimit.c>
        <Location />
            SetOutputFilter RATE_LIMIT
            SetEnv rate-limit 600
        </Location>
    </IfModule>
```
