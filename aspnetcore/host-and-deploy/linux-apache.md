---
title: Host platformy ASP.NET Core w systemie Linux z Apache
description: Dowiedz się, jak skonfigurować przekierowywanie ruchu HTTP do aplikacji sieci web platformy ASP.NET Core uruchomionych na Kestrel Apache jako zwrotny serwer proxy serwera na CentOS.
author: spboyer
ms.author: spboyer
ms.custom: mvc
ms.date: 10/23/2018
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 1d303fbde2a398b4628d3390aea80957a59f711b
ms.sourcegitcommit: 4a6bbe84db24c2f3dd2de065de418fde952c8d40
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50253146"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a>Host platformy ASP.NET Core w systemie Linux z Apache

Przez [Shayne Boyer](https://github.com/spboyer)

Za pomocą tego przewodnika, Dowiedz się, jak skonfigurować [Apache](https://httpd.apache.org/) jako zwrotny serwer proxy serwera na [CentOS 7](https://www.centos.org/) przekierowywania ruchu HTTP do aplikacji sieci web platformy ASP.NET Core uruchomionej na [Kestrel](xref:fundamentals/servers/kestrel). [Rozszerzenia mod_proxy](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) i powiązane moduły Utwórz zwrotny serwer proxy serwera.

## <a name="prerequisites"></a>Wymagania wstępne

1. Serwer z systemem CentOS 7 przy użyciu konta użytkownika standardowego przy użyciu uprawnień "sudo".
1. Na serwerze, należy zainstalować środowisko uruchomieniowe platformy .NET Core.
   1. Odwiedź stronę [.NET Core wszystkie strony plików do pobrania](https://www.microsoft.com/net/download/all).
   1. Z listy w obszarze wybierz najnowsze środowisko uruchomieniowe — wersja zapoznawcza **środowiska uruchomieniowego**.
   1. Wybierz, a następnie postępuj zgodnie z instrukcjami na oprogramowanie Oracle, CentOS /.
1. Istniejącą aplikację ASP.NET Core.

## <a name="publish-and-copy-over-the-app"></a>Publikowanie i skopiuj aplikacji

Konfigurowanie aplikacji na potrzeby [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd).

Uruchom [publikowania dotnet](/dotnet/core/tools/dotnet-publish) ze środowiska projektowego, aby utworzyć pakiet aplikacji do katalogu (na przykład *bin/wydawania/&lt;target_framework_moniker&gt;/ publish*), można Uruchom na serwerze:

```console
dotnet publish --configuration Release
```

Aplikację można także publikować jako [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd) Jeśli wolisz nie zachować środowisko uruchomieniowe platformy .NET Core na serwerze.

Skopiuj aplikacji ASP.NET Core na serwer przy użyciu narzędzia, która integruje się z przepływu pracy w organizacji, (na przykład punkt połączenia usługi, SFTP). Często do lokalizowania aplikacji sieci web w obszarze *var* katalog (na przykład *www/var/helloapp*).

> [!NOTE]
> W przypadku wdrożenia produkcyjnego przepływu pracy ciągłej integracji działa publikowania aplikacji i kopiowanie zasobów do serwera.

## <a name="configure-a-proxy-server"></a>Konfiguracja serwera proxy

Zwrotny serwer proxy jest wspólne dla aplikacji sieci web dynamicznego obsługująca. Zwrotny serwer proxy kończy żądanie HTTP i przekazuje je do aplikacji platformy ASP.NET.

Serwer proxy jest jedną, która przekazuje żądania klienta do innego serwera, a nie sam wypełniania żądań. Zwrotny serwer proxy przekazuje do środka miejsca docelowego, zazwyczaj w imieniu dowolnego klientów. W tym przewodniku Apache jest skonfigurowany jako zwrotny serwer proxy, uruchomione na tym samym serwerze, że Kestrel działa jako aplikacja platformy ASP.NET Core.

Ponieważ żądania są przekazywane przez zwrotny serwer proxy, należy użyć [przekazywane oprogramowania pośredniczącego nagłówki](xref:host-and-deploy/proxy-load-balancer) z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pakietu. Aktualizacje oprogramowania pośredniczącego `Request.Scheme`przy użyciu `X-Forwarded-Proto` nagłówka, więc działanie tego identyfikatory URI przekierowań i innych zasad zabezpieczeń.

Dowolny składnik, który jest zależny od systemu, takie jak uwierzytelnianie, generowanie konsolidacji, przekierowań i geolokalizacja, muszą być umieszczone po wywołaniu oprogramowanie pośredniczące przekazane nagłówków. Zgodnie z ogólną zasadą przekazywane oprogramowania pośredniczącego nagłówki należy uruchomić przed innym oprogramowaniu pośredniczącym, z wyjątkiem diagnostyki i obsługi oprogramowania pośredniczącego błędów. Ta kolejność gwarantuje, że oprogramowanie pośredniczące, opierając się na nagłówki przekazywane informacje mogą wykorzystywać wartości nagłówka do przetworzenia.

::: moniker range=">= aspnetcore-2.0"

Wywoływanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in Class metoda `Startup.Configure` przed wywołaniem [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) lub podobne oprogramowanie pośredniczące schematu uwierzytelniania. Konfigurowanie oprogramowania pośredniczącego, aby przekazywać `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Wywoływanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in Class metoda `Startup.Configure` przed wywołaniem [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) i [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) lub podobne schematu uwierzytelniania oprogramowanie pośredniczące. Konfigurowanie oprogramowania pośredniczącego, aby przekazywać `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków:

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

::: moniker-end

Jeśli nie [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) są określone oprogramowanie pośredniczące, są domyślne nagłówki do przekazywania `None`.

Tylko serwery proxy uruchomiona na hoście lokalnym (127.0.0.1, [:: 1]) są zaufane domyślnie. Jeśli innych zaufanych serwerów proxy lub sieci w obrębie organizacji uchwyt żądania między Internetem a serwer sieci web Dodaj je do listy <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> lub <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> z <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>. Poniższy przykład dodaje serwer proxy zaufanych pod adresem IP 10.0.0.100 z oprogramowaniem pośredniczącym nagłówki przekazywane `KnownProxies` w `Startup.ConfigureServices`:

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.

### <a name="install-apache"></a>Zainstaluj Apache

Aktualizowanie pakietów CentOS do ich najnowszej stabilnej wersji:

```bash
sudo yum update -y
```

Instalowanie serwera internetowego Apache na CentOS, za pomocą jednego `yum` polecenia:

```bash
sudo yum -y install httpd mod_ssl
```

Przykładowe dane wyjściowe po uruchomieniu polecenia:

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
> W tym przykładzie dane wyjściowe odzwierciedla httpd.86_64, ponieważ wersja CentOS 7 jest 64-bitowych. Aby sprawdzić, w którym zainstalowano Apache, uruchom `whereis httpd` z poziomu wiersza polecenia.

### <a name="configure-apache"></a>Skonfigurowania serwera Apache

Pliki konfiguracji Apache znajdują się w `/etc/httpd/conf.d/` katalogu. Każdy plik z *.conf* rozszerzenia są przetwarzane w kolejności alfabetycznej, oprócz plików konfiguracji modułu w `/etc/httpd/conf.modules.d/`, który zawiera żadnej konfiguracji pliki niezbędne do ładowania modułów.

Utwórz plik konfiguracji o nazwie *helloapp.conf*, dla aplikacji:

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

`VirtualHost` Bloku mogą pojawić się wiele razy w jeden lub więcej plików na serwerze. W poprzednim pliku konfiguracji Apache akceptuje publicznych ruch na porcie 80. Domena `www.example.com` obsługiwanych danych, a `*.example.com` aliasu jest rozpoznawany jako ten sam witryny sieci Web. Zobacz [obsługi na podstawie nazwy hosta wirtualnego](https://httpd.apache.org/docs/current/vhosts/name-based.html) Aby uzyskać więcej informacji. Żądania są przekierowywane w katalogu głównym na porcie 5000 serwer pod adresem 127.0.0.1. Do komunikacji dwukierunkowej `ProxyPass` i `ProxyPassReverse` są wymagane. Aby zmienić port adresu IP firmy Kestrel, zobacz [Kestrel: Konfiguracja punktu końcowego](xref:fundamentals/servers/kestrel#endpoint-configuration).

> [!WARNING]
> Błąd, aby określić poprawną [dyrektywy ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername) w **VirtualHost** bloku ujawnia luki w zabezpieczeniach aplikacji. Powiązanie symbol wieloznaczny domeny podrzędnej (na przykład `*.example.com`) nie stanowić to zagrożenie bezpieczeństwa, jeśli możesz kontrolować domenę nadrzędną całego (w przeciwieństwie do `*.com`, który jest narażony). Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.

Można skonfigurować rejestrowanie `VirtualHost` przy użyciu `ErrorLog` i `CustomLog` dyrektywy. `ErrorLog` jest to lokalizacja, w których dzienniki błędów serwera i `CustomLog` ustawia nazwę pliku i format pliku dziennika. W tym przypadku jest to, gdzie jest rejestrowane informacje o żądaniu. Istnieje jeden wiersz dla każdego żądania.

Zapisz plik i wykonaj test konfiguracji. Jeśli wszystko przebiegnie pomyślnie, odpowiedź powinna wyglądać `Syntax [OK]`.

```bash
sudo service httpd configtest
```

Uruchom ponownie usługę Apache:

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a>Monitorowanie aplikacji

Apache jest teraz Instalatora w celu przekazywania żądań kierowanych do `http://localhost:80` do aplikacji platformy ASP.NET Core uruchomionych na Kestrel na `http://127.0.0.1:5000`.  Apache skonfigurować nie jest jednak do zarządzania procesem Kestrel. Użyj *systemd* i Utwórz plik usługi, aby uruchomić i monitorować podstawowej aplikacji sieci web. *systemd* to system init, który zapewnia wiele funkcji zaawansowanych uruchamianie, zatrzymywanie oraz zarządzanie procesami. 

### <a name="create-the-service-file"></a>Utwórz plik usługi

Tworzenie pliku definicji usługi:

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

Przykładowy plik usługi dla aplikacji:

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

Jeśli użytkownik *apache* nie jest używany przez tę konfigurację, użytkownik musi najpierw utworzyć i biorąc pod uwagę odpowiednie własności plików.

Użyj `TimeoutStopSec` skonfigurować czas oczekiwania na aplikację, aby zamknięty po odebraniu sygnału przerwania początkowej. Jeśli aplikacja nie zamknięty w tym okresie, aby zakończyć aplikację zgłaszany jest SIGKILL. Podaj wartość jako unitless sekund (na przykład `150`), czas span wartości (na przykład `2min 30s`), lub `infinity` wyłączyć limit czasu. `TimeoutStopSec` Wartość domyślna to wartość `DefaultTimeoutStopSec` w pliku konfiguracji Menedżera (*systemd system.conf*, *system.conf.d*, *systemd user.conf*,  *User.conf.d*). Domyślna wartość limitu czasu dla większości dystrybucji wynosi 90 s.

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

Niektóre wartości (na przykład parametry połączenia SQL), należy użyć znaków ucieczki dla dostawców konfiguracji można odczytać zmienne środowiskowe. Użyj następującego polecenia do generowania prawidłowo o zmienionym znaczeniu wartości do użycia w pliku konfiguracji:

```console
systemd-escape "<value-to-escape>"
```

Zapisz plik i włączyć usługę:

```bash
sudo systemctl enable kestrel-helloapp.service
```

Uruchom usługę i sprawdź, czy jest uruchomiona:

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

Przy użyciu zwrotnego serwera proxy, skonfigurowane i Kestrel zarządzane za pośrednictwem *systemd*, aplikacji sieci web jest w pełni skonfigurowane i dostępne za pomocą przeglądarki na komputerze lokalnym w `http://localhost`. Inspekcja nagłówki odpowiedzi **serwera** nagłówek wskazuje, że aplikacja platformy ASP.NET Core jest obsługiwana przez Kestrel:

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Wyświetlanie dzienników

Ponieważ aplikacja sieci web przy użyciu Kestrel odbywa się przy użyciu *systemd*, scentralizowane dziennika są rejestrowane zdarzenia i procesów. Jednak ten dziennik zawiera wpisy dla wszystkich usług i procesów, które zarządza *systemd*. Aby wyświetlić `kestrel-helloapp.service`— określone elementy, użyj następującego polecenia:

```bash
sudo journalctl -fu kestrel-helloapp.service
```

Filtrowanie czasu, określ opcje czasu za pomocą polecenia. Na przykład użyć `--since today` do filtrowania dla bieżącego dnia lub `--until 1 hour ago` Aby wyświetlić wpisy poprzedniej godziny. Aby uzyskać więcej informacji, zobacz [man strona journalctl](https://www.unix.com/man-page/centos/1/journalctl/).

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a>Ochrona danych

[Stosu ochrony danych programu ASP.NET Core](xref:security/data-protection/introduction) jest używana przez kilka platformy ASP.NET Core [middlewares](xref:fundamentals/middleware/index), w tym oprogramowania pośredniczącego uwierzytelniania (na przykład, oprogramowaniu pośredniczącym pliku cookie) i fałszerstwo żądania międzywitrynowego (CSRF) zabezpieczenia. Nawet wtedy, gdy interfejsów API ochrony danych nie są wywoływane przez kod użytkownika, ochrony danych należy skonfigurować tak, aby utworzyć trwałe kryptograficznych [magazynu kluczy](xref:security/data-protection/implementation/key-management). Jeśli nie jest skonfigurowana ochrona danych, klucze są przechowywane w pamięci i odrzucone po ponownym uruchomieniu aplikacji.

Jeśli pierścień klucz jest przechowywany w pamięci, po ponownym uruchomieniu aplikacji:

* Wszystkie tokeny na podstawie plików cookie uwierzytelniania są unieważniane.
* Użytkownicy muszą ponownie zaloguj się na ich następnego żądania.
* Wszystkie dane chronione za pomocą pierścień klucz może już nie mogły być odszyfrowane. Może to obejmować [tokenów CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) i [plików cookie programu ASP.NET Core MVC TempData](xref:fundamentals/app-state#tempdata).

Aby skonfigurować ochronę danych na zostaną zachowane, a pierścień klucz szyfrowania, zobacz:

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="securing-the-app"></a>Zabezpieczanie aplikacji

### <a name="configure-firewall"></a>Konfigurowanie zapory

*Firewalld* jest dynamiczna demona się zarządzać zaporą z obsługą stref sieci. Porty i filtrowanie pakietów nadal mogą być zarządzane przez iptables. *Firewalld* powinny być instalowane domyślnie. `yum` może służyć do zainstalowania pakietu lub upewnij się, że jest ona zainstalowana.

```bash
sudo yum install firewalld -y
```

Użyj `firewalld` można otworzyć tylko te porty, które są potrzebne dla aplikacji. W tym przypadku port 80 i 443 są używane. Porty 80 i 443, aby otworzyć można trwale ustawione w następujących poleceń:

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

Załaduj ponownie ustawienia zapory. Sprawdź dostępnych usług i portów w strefie domyślnej. Opcje są dostępne, sprawdzając `firewall-cmd -h`.

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

Do skonfigurowania serwera Apache dla protokołu SSL, *mod_ssl* moduł jest używany. Gdy *host z wieloma adresami* moduł został zainstalowany, *mod_ssl* również został zainstalowany moduł. Jeśli nie została zainstalowana za pomocą `yum` Aby dodać go do konfiguracji.

```bash
sudo yum install mod_ssl
```

Aby Wymuszanie protokołu SSL, należy zainstalować `mod_rewrite` modułu, aby umożliwić ponownego zapisywania adresów URL:

```bash
sudo yum install mod_rewrite
```

Modyfikowanie *helloapp.conf* plik, aby włączyć ponownego zapisywania adresów URL i zabezpieczają komunikację na porcie 443:

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
> W tym przykładzie używa lokalnie wygenerowany certyfikat. **SSLCertificateFile** powinien być plik certyfikatu podstawowego dla nazwy domeny. **SSLCertificateKeyFile** powinny być plik klucza generowane podczas tworzenia żądania CSR. **SSLCertificateChainFile** powinien być pliku pośredniego certyfikatu (jeśli istnieje) który został dostarczony przez urząd certyfikacji.

Zapisz plik i wykonaj test konfiguracji:

```bash
sudo service httpd configtest
```

Uruchom ponownie usługę Apache:

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Dodatkowe sugestie Apache

### <a name="additional-headers"></a>Dodatkowe nagłówki

Aby zabezpieczyć się przed złośliwymi atakami, istnieje kilka nagłówki, które powinny być zmodyfikowany lub dodane. Upewnij się, że `mod_headers` zainstalowany moduł:

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a>Zabezpieczanie Apache przed atakami porywaniu kliknięć

[Porywaniu kliknięć](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), znane również jako *interfejsu użytkownika odszkodowania ataku*, jest złośliwymi atakami, gdzie zwiódł już obiekt odwiedzający witrynę sieci Web do kliknięcia łącza lub przycisku na innej stronie nie są one obecnie odwiedzający. Użyj `X-FRAME-OPTIONS` na zabezpieczenie witryny.

Aby uniknąć porywaniu kliknięć ataków:

1. Edytuj *httpd.conf* pliku:

   ```bash
   sudo nano /etc/httpd/conf/httpd.conf
   ```

   Dodaj wiersz `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.
1. Zapisz plik.
1. Uruchom ponownie Apache.

#### <a name="mime-type-sniffing"></a>Wykrywanie typu MIME

`X-Content-Type-Options` Nagłówka uniemożliwia programowi Internet Explorer z *wykrywanie MIME* (Określanie pliku `Content-Type` z zawartości pliku). Jeśli serwer ustawia `Content-Type` nagłówka do `text/html` z `nosniff` zestaw opcji, program Internet Explorer renderuje zawartość jako `text/html` niezależnie od zawartości pliku.

Edytuj *httpd.conf* pliku:

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Dodaj wiersz `Header set X-Content-Type-Options "nosniff"`. Zapisz plik. Uruchom ponownie Apache.

### <a name="load-balancing"></a>Równoważenie obciążenia

W tym przykładzie pokazano, jak zainstalować i skonfigurować Apache CentOS 7 i Kestrel na tym samym komputerze wystąpienia. Aby można było ma pojedynczy punkt awarii; za pomocą *mod_proxy_balancer* i modyfikowanie **VirtualHost** pozwalają na zarządzanie wielu wystąpień funkcji web apps za serwerem proxy Apache.

```bash
sudo yum install mod_proxy_balancer
```

W pliku konfiguracyjnym pokazano poniżej, dodatkowe wystąpienia `helloapp` zdefiniowano działają na portu 5001. *Proxy* sekcji została ustawiona za pomocą konfiguracji usługi równoważenia, za pomocą dwóch członków do równoważenia obciążenia *byrequests*.

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

### <a name="rate-limits"></a>Limity szybkości

Za pomocą *mod_ratelimit*, który znajduje się w *host z wieloma adresami* modułu, może być ograniczona przepustowość klientów:

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
Przykładowy plik ogranicza przepustowość jako 600 KB/s, w obszarze Katalog główny:

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wymagania wstępne dla platformy .NET Core w systemie Linux](/dotnet/core/linux-prerequisites)
* [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer)
