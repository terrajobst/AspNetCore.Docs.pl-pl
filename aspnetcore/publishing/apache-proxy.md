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
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/22/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-apache-and-deploy-to-it"></a>Konfigurowanie środowiska macierzystego dla platformy ASP.NET Core w systemie Linux z Apache i wdrażać do niego

Przez [Shayne Boyer](https://github.com/spboyer)

Apache jest bardzo popularny serwera HTTP i może być skonfigurowany jako serwer proxy do przekierowywania ruchu HTTP podobny do nginx. W tym przewodniku firma Microsoft będzie Dowiedz się, jak skonfigurować Apache na CentOS 7 i użyć jej jako zwrotny serwer proxy do Witamy połączenia przychodzące i Przekierowanie do aplikacji platformy ASP.NET Core systemem Kestrel. W tym celu użyjemy *mod_proxy* rozszerzenia i inne powiązane moduły Apache.

## <a name="prerequisites"></a>Wymagania wstępne

1. Serwer z systemem CentOS 7 przy użyciu konta użytkowników standardowych z uprawnieniem sudo.
2. Istniejącej aplikacji platformy ASP.NET Core. 

## <a name="publish-your-application"></a>Publikowanie aplikacji

Uruchom `dotnet publish -c Release` ze środowiska deweloperskiego do pakietu aplikacji do katalogu niezależne, które mogą być uruchamiane na serwerze. Opublikowana aplikacja należy następnie skopiować na serwerze przy użyciu połączenia, FTP lub innej metody transferu plików. 

> [!NOTE]
> W przypadku wdrożenia produkcyjnego ciągłej integracji przepływu pracy wykonuje pracę publikowania aplikacji i kopiowanie zasoby na serwerze. 

## <a name="configure-a-proxy-server"></a>Konfiguracja serwera proxy

Zwrotny serwer proxy jest typowe dla obsługująca dynamicznych aplikacji sieci web. Zwrotny serwer proxy kończy żądanie HTTP i przekazuje je do aplikacji ASP.NET.

Serwer proxy to taki, który przekazuje żądania klienta do innego serwera zamiast wykonanie samego. Zwrotny serwer proxy przekazuje do stałej miejsca docelowego, zwykle w imieniu dowolnego klientów. W tym przewodniku Apache jest konfigurowany jako zwrotny serwer proxy, uruchomionych na tym samym serwerze, że Kestrel służy aplikacji platformy ASP.NET Core. 

Każda aplikacja może występować na oddzielnych komputerów fizycznych, kontenery Docker lub kombinację konfiguracji, w zależności od ograniczeń lub architektury potrzeb.

### <a name="install-apache"></a>Zainstaluj Apache

Zainstalowanie serwera sieci web programu Apache na CentOS jest teraz jednego polecenia, ale pierwszym naszych pakietów aktualizacji.

```bash
    sudo yum update -y
```

Dzięki temu wszystkie zainstalowane pakiety zostały zaktualizowane do swoich najnowszej wersji. Instalowanie przy użyciu Apache`yum`

```bash
    sudo yum -y install httpd mod_ssl
```

Dane wyjściowe powinien odzwierciedlać podobny do następującego.

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
> W tym przykładzie httpd.86_64 odzwierciedla dane wyjściowe, ponieważ wersja CentOS 7 jest 64-bitowym. Dane wyjściowe mogą być inne dla serwera. Aby sprawdzić, w którym zainstalowano Apache, uruchom `whereis httpd` z wiersza polecenia. 

### <a name="configure-apache-for-reverse-proxy"></a>Konfigurowanie Apache dla zwrotnego serwera proxy

Pliki konfiguracji Apache znajdują się w `/etc/httpd/conf.d/` katalogu. Dowolne pliki z **.conf** rozszerzenia będą przetwarzane w kolejności alfabetycznej oprócz plików konfiguracji modułu w `/etc/httpd/conf.modules.d/`, który zawiera żadnej konfiguracji pliki niezbędne do ładowania modułów.

Utwórz plik konfiguracji aplikacji, w tym przykładzie, że Zadzwonimy go`hellomvc.conf`

```text
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
    </VirtualHost>
```

*VirtualHost* węzła, którego może istnieć wiele w pliku lub na serwerze w wielu plikach ustawiono do nasłuchiwania na dowolny adres IP przy użyciu portu 80. Następne dwa wiersze są ustawione na przekazywanie wszystkich żądań odebranych w katalogu głównym port maszyny 127.0.0.1 5000 i odwrotnie. Celu zapewnienia dwukierunkowej komunikacji, oba ustawienia *ProxyPass* i *ProxyPassReverse* są wymagane.

Można skonfigurować rejestrowania na VirtualHost przy użyciu *ErrorLog* i *CustomLog* dyrektywy. *Dziennik błędów* to lokalizacja, w której serwer będzie rejestrować błędy i *CustomLog* ustawia nazwę pliku i format pliku dziennika. W tym przypadku jest to, którym będą rejestrowane informacje o żądaniu. Będzie jeden wiersz dla każdego żądania.

Zapisz plik i przetestowania konfiguracji. Jeśli wszystko przebiegnie pomyślnie, powinien być odpowiedzi `Syntax [OK]`.

```bash
    sudo service httpd configtest
```

Uruchom ponownie Apache.

```text
    sudo systemctl restart httpd
    sudo systemctl enable httpd
```

## <a name="monitoring-our-application"></a>Monitorowanie aplikacji

Apache jest teraz skonfigurowana do przekazywania żądań wysyłanych do `http://localhost:80` do aplikacji platformy ASP.NET Core systemem Kestrel na `http://127.0.0.1:5000`.  Jednak Apache nie skonfigurowano do zarządzania procesem Kestrel. Używamy *systemd* i utworzenie pliku usługi, aby uruchomić i monitorować podstawowej aplikacji sieci web. *systemd* to system init zapewnia wiele zaawansowanych funkcji uruchamianie, zatrzymywanie oraz procesy zarządzania. 


### <a name="create-the-service-file"></a>Tworzenie pliku usługi

Tworzenie pliku definicji usługi 

```bash
    sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Przykładowy plik usługi dla aplikacji.

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
> **Użytkownik** — Jeśli użytkownik *apache* nie jest używana w bieżącej konfiguracji użytkownika zdefiniowane w tym miejscu należy najpierw utworzyć i podane odpowiednie własność plików

Zapisz plik i włączyć usługę.

```bash
    systemctl enable kestrel-hellomvc.service
```

Uruchom usługę i sprawdź, czy jest uruchomiona.

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

Zwrotny serwer proxy skonfigurowane i zarządzanych za pomocą systemd Kestrel, aplikacji sieci web jest w pełni skonfigurowane i dostępne w przeglądarce na komputerze lokalnym w `http://localhost`. Zapoznanie się nagłówki odpowiedzi **serwera** pozostanie obsługiwanej przez Kestrel aplikacji platformy ASP.NET Core.

```text
    HTTP/1.1 200 OK
    Date: Tue, 11 Oct 2016 16:22:23 GMT
    Server: Kestrel
    Keep-Alive: timeout=5, max=98
    Connection: Keep-Alive
    Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Przeglądanie dzienników

Ponieważ do aplikacji sieci web przy użyciu Kestrel odbywa się przy użyciu systemd, scentralizowane dziennika są rejestrowane wszystkie zdarzenia i procesy. Jednak ten dziennik zawiera wszystkie wpisy dla wszystkich usług i procesów zarządza systemd. Aby wyświetlić `kestrel-hellomvc.service` określone elementy, użyj następującego polecenia.

```bash
    sudo journalctl -fu kestrel-hellomvc.service
```

Dla dalszego filtrowania, opcje czasu takich jak `--since today`, `--until 1 hour ago` lub kombinacji tych można zmniejszyć liczbę wpisów zwracane.

```bash
    sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a>Zabezpieczanie aplikacji

### <a name="configure-firewall"></a>Konfigurowanie zapory

*Firewalld* jest demon dynamiczne zarządzanie zaporą z obsługą stref sieci, mimo że iptables można nadal używać do zarządzania portów i filtrowanie pakietów. Domyślnie, należy zainstalować Firewalld `yum` może służyć do zainstalowania pakietu lub weryfikacji.

```bash
    sudo yum install firewalld -y
```

Przy użyciu `firewalld` można otworzyć tylko te porty, które są wymagane dla aplikacji. W takim przypadku portu 80 i 443 są używane. Następujące polecenia ustawia trwale je otworzyć.

```bash
    sudo firewall-cmd --add-port=80/tcp --permanent
    sudo firewall-cmd --add-port=443/tcp --permanent
```

Załaduj ponownie ustawienia zapory i sprawdź dostępnych usług i portów w strefie domyślnej. Opcje są dostępne, sprawdzając`firewall-cmd -h`

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

### <a name="ssl-configuration"></a>Konfiguracja protokołu SSL

Aby skonfigurować Apache dla protokołu SSL, moduł mod_ssl jest używany.  To zainstalowano początkowo możemy zainstalowany `httpd` modułu. Jeżeli zostało pominięte lub nie jest zainstalowany, należy dodać ją do konfiguracji za pomocą yum.

```bash
    sudo yum install mod_ssl
```
Aby wymusić protokołu SSL, należy zainstalować`mod_rewrite`

```bash
    sudo yum install mod_rewrite
```

`hellomvc.conf` Pliku, który został utworzony dla w tym przykładzie musi zostać zmodyfikowane w celu włączenia poprawione, a także dodawanie nowej **VirtualHost** sekcji dla protokołu HTTPS.

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
> W tym przykładzie używa certyfikatu, wygenerowaną lokalnie. **SSLCertificateFile** powinien być pliku podstawowego certyfikatu dla nazwy domeny. **SSLCertificateKeyFile** powinien być generowany po utworzeniu obsługi pliku klucza. **SSLCertificateChainFile** powinien być pliku certyfikatu pośredniego (jeśli istnieją) podane przez urzędu certyfikacji

Zapisz plik i przetestowania konfiguracji.

```bash
    sudo service httpd configtest
```

Uruchom ponownie Apache.

```bash
    sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Dodatkowe sugestie Apache

### <a name="additional-headers"></a>Dodatkowych nagłówków 
Aby zabezpieczyć przed złośliwymi atakami są kilka nagłówki, które powinny być zmodyfikowany lub dodać. Upewnij się, że `mod_headers` moduł jest zainstalowany.

```bash
    sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking"></a>Bezpieczny Apache z porywaniu kliknięć
Porywaniu kliknięć to technika złośliwego zbierać zainfekowane użytkownik klika polecenie. Porywaniu kliknięć sztuczki ofiara (użytkownik) do kliknięcia zainfekowane w witrynie. Użyj X-FRAME-OPTIONS do zabezpieczenia witryny.

Edytuj plik httpd.conf.

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

Dodaj wiersz `Header append X-FRAME-OPTIONS "SAMEORIGIN"` i Zapisz plik, a następnie uruchom ponownie Apache.

#### <a name="mime-type-sniffing"></a>Wykrywanie typ MIME

Ten nagłówek uniemożliwia programowi Internet Explorer z wykrywanie MIME odpowiedzi od deklarowanego typu zawartości jako nagłówek powoduje, że przeglądarka nie, aby zastąpić typ zawartości odpowiedzi. Z opcją nosniff Jeśli na serwerze mówi się, że zawartość jest text/html, przeglądarka wyświetli go jako tekst i html.

Edytuj plik httpd.conf.

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

Dodaj wiersz `Header set X-Content-Type-Options "nosniff"` i Zapisz plik, a następnie uruchom ponownie Apache.

### <a name="load-balancing"></a>Równoważenie obciążenia 

W tym przykładzie przedstawiono sposób instalacji i konfiguracji Apache CentOS 7 i Kestrel na tym samym komputerze wystąpienia.  Jednak, aby nie mieć pojedynczy punkt awarii; przy użyciu *mod_proxy_balancer* i modyfikowanie VirtualHost czy umożliwiają zarządzanie wiele wystąpień aplikacji sieci web za serwerem proxy Apache.

```bash
    sudo yum install mod_proxy_balancer
```

W pliku konfiguracyjnym dodatkowego wystąpienia `hellomvc` aplikacji została skonfigurowana do uruchamiania na porcie 5001 i *Proxy* sekcja została ustawiona w konfiguracji usługi równoważenia z dwóch członków na potrzeby równoważenia obciążenia *byrequests* .

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

### <a name="rate-limits"></a>Limity szybkości
Przy użyciu `mod_ratelimit`, który znajduje się w `htttpd` modułu można ograniczyć przepustowość klientów. 

```bash
    sudo nano /etc/httpd/conf.d/ratelimit.conf
```
Przykładowy plik ogranicza przepustowość jako 600 KB/s w lokalizacji głównej.

```text
    <IfModule mod_ratelimit.c>
        <Location />
            SetOutputFilter RATE_LIMIT
            SetEnv rate-limit 600
        </Location>
    </IfModule>
```
