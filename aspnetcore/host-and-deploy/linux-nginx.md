---
title: Hostowanie ASP.NET Core w systemie Linux za pomocą Nginx
author: guardrex
description: Dowiedz się, jak skonfigurować Nginx jako zwrotny serwer proxy w Ubuntu 16,04 do przesyłania dalej ruchu HTTP do aplikacji internetowej ASP.NET Core działającej na Kestrel.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/31/2019
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: b71bc0464892f15ef8db0324a8e66a28a6192577
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080872"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a>Hostowanie ASP.NET Core w systemie Linux za pomocą Nginx

Autor [sourabh Shirhatti](https://twitter.com/sshirhatti)

W tym przewodniku wyjaśniono Konfigurowanie środowiska ASP.NET Core gotowego do produkcji na serwerze z systemem Ubuntu 16,04. Te instrukcje będą działały z nowszymi wersjami Ubuntu, ale instrukcje nie zostały przetestowane z nowszymi wersjami.

Aby uzyskać informacje dotyczące innych dystrybucji systemu Linux obsługiwanych przez ASP.NET Core, zobacz [wymagania wstępne dotyczące platformy .NET Core w systemie Linux](/dotnet/core/linux-prerequisites).

> [!NOTE]
> W przypadku Ubuntu 14,04 zaleca się, *Aby program był* monitorowany jako rozwiązanie do monitorowania procesu Kestrel. *system* nie jest dostępny w systemie Ubuntu 14,04. Instrukcje dotyczące programu Ubuntu 14,04 znajdują się w [poprzedniej wersji tego tematu](https://github.com/aspnet/AspNetCore.Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).

Ten przewodnik:

* Umieszcza istniejącą aplikację ASP.NET Core za odwrotnym serwerem proxy.
* Konfiguruje odwrotny serwer proxy do przesyłania żądań do serwera sieci Web Kestrel.
* Zapewnia, że aplikacja sieci Web będzie uruchamiana przy uruchamianiu jako demon.
* Konfiguruje narzędzie do zarządzania procesami, aby pomóc w ponownym uruchomieniu aplikacji sieci Web.

## <a name="prerequisites"></a>Wymagania wstępne

1. Dostęp do serwera z systemem Ubuntu 16,04 przy użyciu konta użytkownika standardowego z uprawnieniami sudo.
1. Zainstaluj środowisko uruchomieniowe platformy .NET Core na serwerze.
   1. Odwiedź [stronę wszystkie pliki do pobrania w programie .NET Core](https://www.microsoft.com/net/download/all).
   1. Wybierz najnowszą wersję środowiska uruchomieniowego bez podglądu z listy w obszarze **środowisko uruchomieniowe**.
   1. Wybierz i postępuj zgodnie z instrukcjami dla Ubuntu, które pasują do wersji Ubuntu serwera.
1. Istniejąca aplikacja ASP.NET Core.

## <a name="publish-and-copy-over-the-app"></a>Publikowanie i kopiowanie aplikacji

Skonfiguruj aplikację dla [wdrożenia zależnego od platformy](/dotnet/core/deploying/#framework-dependent-deployments-fdd).

Jeśli aplikacja jest uruchamiana lokalnie i nie jest skonfigurowana do nawiązywania bezpiecznych połączeń (HTTPS), należy zastosować jedną z następujących metod:

* Skonfiguruj aplikację do obsługi bezpiecznych połączeń lokalnych. Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja protokołu HTTPS](#https-configuration) .
* Usuń `https://localhost:5001` (jeśli istnieje) `applicationUrl` z właściwości w pliku *Properties/profilu launchsettings. JSON* .

Uruchom [dotnet Publish](/dotnet/core/tools/dotnet-publish) ze środowiska programistycznego, aby spakować aplikację do katalogu (na przykład *bin/Release&lt;/target_framework_moniker&gt;/Publish*), które można uruchomić na serwerze:

```dotnetcli
dotnet publish --configuration Release
```

Aplikację można również opublikować jako [samodzielne wdrożenie](/dotnet/core/deploying/#self-contained-deployments-scd) , jeśli wolisz, aby nie obsługiwać środowiska uruchomieniowego .NET Core na serwerze.

Skopiuj aplikację ASP.NET Core na serwer przy użyciu narzędzia, które integruje się z przepływem pracy organizacji (na przykład SCP, SFTP). Często można zlokalizować aplikacje sieci Web w katalogu *var* (na przykład *var/www/helloapp*).

> [!NOTE]
> W obszarze scenariusza wdrożenia produkcyjnego przepływ pracy ciągłej integracji wykonuje zadania publikowania aplikacji i kopiowania zasobów na serwer.

Testowanie aplikacji:

1. W wierszu polecenia Uruchom aplikację: `dotnet <app_assembly>.dll`.
1. W przeglądarce przejdź do `http://<serveraddress>:<port>` , aby sprawdzić, czy aplikacja działa lokalnie w systemie Linux.

## <a name="configure-a-reverse-proxy-server"></a>Konfigurowanie zwrotnego serwera proxy

Zwrotny serwer proxy to typowa konfiguracja służąca do obsługi dynamicznych aplikacji sieci Web. Zwrotny serwer proxy przerywa żądanie HTTP i przekazuje go do aplikacji ASP.NET Core.

### <a name="use-a-reverse-proxy-server"></a>Korzystanie z serwera zwrotnego serwera proxy

Kestrel doskonale nadaje się do obsługi zawartości dynamicznej z ASP.NET Core. Jednak funkcje obsługujące sieci Web nie są tak bogate jak takie jak serwery usług IIS, Apache lub nginx. Odwrotny serwer proxy może odciążać działania, takie jak obsługa zawartości statycznej, buforowanie żądań, kompresowanie żądań i zakończenie protokołu HTTPS z serwera HTTP. Odwrotny serwer proxy może znajdować się na dedykowanym komputerze lub może zostać wdrożony razem z serwerem HTTP.

Na potrzeby tego przewodnika jest używane pojedyncze wystąpienie Nginx. Działa na tym samym serwerze, obok serwera HTTP. W zależności od wymagań można wybrać inną konfigurację.

Ze względu na to, że żądania są przekazywane przez zwrotny serwer proxy, należy użyć [oprogramowania pośredniczącego "przesłane nagłówki](xref:host-and-deploy/proxy-load-balancer) " z pakietu [Microsoft. AspNetCore. HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) . Oprogramowanie pośredniczące aktualizuje `Request.Scheme`, `X-Forwarded-Proto` używając nagłówka, tak aby identyfikatory URI przekierowania i inne zasady zabezpieczeń działały prawidłowo.

Wszelkie składniki, które są zależne od schematu, takie jak uwierzytelnianie, generowanie linków, przekierowania i geolokalizacja, muszą być umieszczone po wywołaniu bezpośrednich nagłówków. Zgodnie z ogólną zasadą przekazane nagłówki oprogramowania pośredniczącego powinny zostać uruchomione przed innymi oprogramowania pośredniczącego, z wyjątkiem diagnostyki i błędów obsługi oprogramowania pośredniczącego. Takie porządkowanie zapewnia, że oprogramowanie pośredniczące polegające na informacjach o przekazanych nagłówkach może zużywać wartości nagłówka do przetworzenia.

Wywołaj `Startup.Configure` <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> metodę w przed wywołaniem lub podobnym schematem uwierzytelniania oprogramowania pośredniczącego. <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> Skonfiguruj oprogramowanie pośredniczące do przesyłania dalej `X-Forwarded-For` nagłówków `X-Forwarded-Proto` i:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

Jeśli wartość <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> nie jest określona dla oprogramowania pośredniczącego, domyślne nagłówki są `None`do przodu.

Serwery proxy uruchomione na adresach sprzężenia zwrotnego (127.0.0.0/8, [:: 1]), w tym standardowy adres localhost (127.0.0.1), są domyślnie zaufane. Jeśli inne zaufane serwery proxy lub sieci w organizacji obsługują żądania między Internetem a serwerem sieci Web, należy dodać je do listy <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> lub <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> z <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>. Poniższy przykład dodaje zaufany serwer proxy pod adresem IP 10.0.0.100 do przesyłanych nagłówków pośredniczących `KnownProxies` w programie: `Startup.ConfigureServices`

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.

### <a name="install-nginx"></a>Zainstaluj Nginx

Służy `apt-get` do instalowania Nginx. Instalator tworzy *systemowy* skrypt inicjujący, który uruchamia Nginx jako demon przy uruchamianiu systemu. Postępuj zgodnie z instrukcjami dotyczącymi [instalacji Ubuntu na Nginx: Oficjalne pakiety](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)Debian/Ubuntu.

> [!NOTE]
> Jeśli Opcjonalne moduły Nginx są wymagane, może być wymagane skompilowanie Nginx ze źródła.

Ponieważ Nginx został zainstalowany po raz pierwszy, należy jawnie uruchomić go, uruchamiając polecenie:

```bash
sudo service nginx start
```

Sprawdź, czy w przeglądarce jest wyświetlana domyślna strona docelowa dla Nginx. Strona docelowa jest dostępna `http://<server_IP_address>/index.nginx-debian.html`pod adresem.

### <a name="configure-nginx"></a>Konfigurowanie Nginx

Aby skonfigurować Nginx jako zwrotny serwer proxy do przesyłania dalej żądań do aplikacji ASP.NET Core, zmodyfikuj */etc/nginx/sites-available/default*. Otwórz go w edytorze tekstów i Zastąp zawartość następującym:

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

Gdy nie `server_name` są zgodne, Nginx używa serwera domyślnego. W przypadku braku zdefiniowanego serwera domyślnego pierwszy serwer w pliku konfiguracji jest domyślnym serwerem. Najlepszym rozwiązaniem jest dodanie określonego serwera domyślnego, który zwraca kod stanu 444 w pliku konfiguracji. Domyślnym przykładem konfiguracji serwera jest:

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

W przypadku powyższego pliku konfiguracji i domyślnego serwera Nginx akceptuje publiczny ruch na porcie 80 z `example.com` nagłówkiem `*.example.com`hosta lub. Żądania niepasujące do tych hostów nie zostaną przekazane do Kestrel. Nginx przekazuje pasujące żądania do Kestrel w `http://localhost:5000`. Zobacz [, jak Nginx przetwarza żądanie,](https://nginx.org/docs/http/request_processing.html) Aby uzyskać więcej informacji. Aby zmienić Kestrel IP/port, zobacz [Kestrel: Konfiguracja](xref:fundamentals/servers/kestrel#endpoint-configuration)punktu końcowego.

> [!WARNING]
> Nie można określić odpowiedniej [dyrektywy nazwa_serwera](https://nginx.org/docs/http/server_names.html) , aby uwidocznić aplikację pod kątem luk w zabezpieczeniach. Powiązanie symboli wieloznacznych z poddomeną (na przykład `*.example.com`) nie ma znaczenia dla tego zagrożenia bezpieczeństwa, jeśli kontrolujesz całą domenę nadrzędną (w przeciwieństwie do `*.com`, który jest narażony). Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.

Po nawiązaniu konfiguracji Nginx Uruchom `sudo nginx -t` polecenie, aby zweryfikować składnię plików konfiguracji. Jeśli test pliku konfiguracji zakończy się pomyślnie, Wymuś Nginx aby pobrać zmiany, uruchamiając `sudo nginx -s reload`polecenie.

Aby bezpośrednio uruchomić aplikację na serwerze:

1. Przejdź do katalogu aplikacji.
1. Uruchom aplikację: `dotnet <app_assembly.dll>`, gdzie `app_assembly.dll` to nazwa pliku zestawu aplikacji.

Jeśli aplikacja działa na serwerze, ale nie odpowiada za pośrednictwem Internetu, sprawdź zaporę serwera i upewnij się, że port 80 jest otwarty. W przypadku korzystania z maszyny wirtualnej usługi Azure Ubuntu Dodaj regułę sieciowej grupy zabezpieczeń (sieciowej grupy zabezpieczeń), która umożliwia ruch przychodzący portu 80. Nie ma potrzeby włączania reguły portu 80 dla ruchu wychodzącego, ponieważ ruch wychodzący jest automatycznie udzielany, gdy reguła ruchu przychodzącego jest włączona.

Po zakończeniu testowania aplikacji Zamknij aplikację z `Ctrl+C` poziomu wiersza polecenia.

## <a name="monitor-the-app"></a>Monitorowanie aplikacji

Serwer jest skonfigurowany do przesyłania dalej żądań wysyłanych `http://<serveraddress>:80` do usługi ASP.NET Core aplikacji uruchomionej w systemie `http://127.0.0.1:5000`Kestrel. Nginx nie jest jednak skonfigurowany do zarządzania procesem Kestrel. *system* może służyć do tworzenia pliku usługi do uruchamiania i monitorowania podstawowej aplikacji sieci Web. *system* to system inicjujący, który udostępnia wiele zaawansowanych funkcji uruchamiania, zatrzymywania i zarządzania procesami. 

### <a name="create-the-service-file"></a>Utwórz plik usługi

Utwórz plik definicji usługi:

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

Poniżej znajduje się przykładowy plik usługi dla aplikacji:

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/www/helloapp
ExecStart=/usr/bin/dotnet /var/www/helloapp/helloapp.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

Jeśli użytkownik nie korzysta z *sieci www — dane* nie są używane przez tę konfigurację, należy najpierw utworzyć użytkownika i nadać mu właściwy własność plików.

Użyj `TimeoutStopSec` , aby skonfigurować czas oczekiwania na wyłączenie aplikacji po odebraniu początkowego sygnału przerwania. Jeśli aplikacja nie zostanie zamknięta w tym okresie, SIGKILL jest wystawiony, aby zakończyć działanie aplikacji. Podaj wartość jako bezjednostkowe sekundy (na przykład `150`), wartość przedziału czasu (na `2min 30s`przykład) lub `infinity` aby wyłączyć limit czasu. `TimeoutStopSec`Wartością domyślną jest `DefaultTimeoutStopSec` wartość w pliku konfiguracji Menedżera (*systemd-system. conf*, *System. conf. d*, *systemed-User. conf*, *User. conf. d*). Domyślny limit czasu dla większości dystrybucji wynosi 90 sekund.

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

W systemie Linux istnieje system plików z uwzględnieniem wielkości liter. Ustawienie ASPNETCORE_ENVIRONMENT na "Product" skutkuje wyszukiwaniem pliku konfiguracji *appSettings. Production. JSON*, a nie *appSettings. produkcja. JSON*.

Niektóre wartości (na przykład parametry połączenia SQL) muszą zostać zmienione dla dostawców konfiguracji, aby odczytywać zmienne środowiskowe. Użyj poniższego polecenia, aby wygenerować poprawną wartość ucieczki do użycia w pliku konfiguracji:

```console
systemd-escape "<value-to-escape>"
```

Separatory`:`dwukropek () nie są obsługiwane w nazwach zmiennych środowiskowych. Użyj podwójnego podkreślenia (`__`) zamiast dwukropka. [Dostawca konfiguracji zmiennych środowiskowych](xref:fundamentals/configuration/index#environment-variables-configuration-provider) konwertuje podwójne podkreślenie na dwukropek, gdy zmienne środowiskowe są odczytywane w konfiguracji. W poniższym przykładzie klucz `ConnectionStrings:DefaultConnection` parametrów połączenia jest ustawiany na plik definicji usługi jako: `ConnectionStrings__DefaultConnection`

```
Environment=ConnectionStrings__DefaultConnection={Connection String}
```

Zapisz plik i Włącz usługę.

```bash
sudo systemctl enable kestrel-helloapp.service
```

Uruchom usługę i sprawdź, czy jest uruchomiona.

```
sudo systemctl start kestrel-helloapp.service
sudo systemctl status kestrel-helloapp.service

● kestrel-helloapp.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-helloapp.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-helloapp.service
            └─9021 /usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
```

Gdy zwrotny serwer proxy został skonfigurowany i Kestrel zarządzany za pomocą systemu, aplikacja sieci Web jest w pełni skonfigurowana i można uzyskać do niej dostęp z przeglądarki na `http://localhost`komputerze lokalnym pod adresem. Jest ona również dostępna z komputera zdalnego, blokując zaporę, która może być zablokowana. Inspekcja nagłówków `Server` odpowiedzi w nagłówku zostanie wyświetlona aplikacja ASP.NET Core obsługiwana przez Kestrel.

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="view-logs"></a>Wyświetlanie dzienników

Ponieważ aplikacja internetowa korzystająca z usługi Kestrel `systemd`jest zarządzana przy użyciu programu, wszystkie zdarzenia i procesy są rejestrowane w scentralizowanym dzienniku. Jednak ten dziennik zawiera wszystkie wpisy dla wszystkich usług i procesów zarządzanych przez `systemd`program. Aby wyświetlić `kestrel-helloapp.service`konkretne elementy, użyj następującego polecenia:

```bash
sudo journalctl -fu kestrel-helloapp.service
```

Aby można było kontynuować filtrowanie, opcje czasu `--since today`, `--until 1 hour ago` takie jak, lub ich kombinacje mogą zmniejszyć liczbę zwróconych wpisów.

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a>Ochrona danych

[ASP.NET Core stosu ochrony danych](xref:security/data-protection/introduction) jest używany przez kilka ASP.NET Core [middlewares](xref:fundamentals/middleware/index), w tym uwierzytelnianie pośredniczące uwierzytelniania (np. Oprogramowanie pośredniczące plików cookie) i ochrona za żądania między lokacjami (CSRF). Nawet jeśli interfejsy API ochrony danych nie są wywoływane przez kod użytkownika, należy skonfigurować ochronę danych w celu utworzenia trwałego [magazynu kluczy](xref:security/data-protection/implementation/key-management)kryptograficznych. Jeśli nie jest skonfigurowana ochrona danych, klucze są przechowywane w pamięci i odrzucone po ponownym uruchomieniu aplikacji.

Jeśli pierścień klucz jest przechowywany w pamięci, po ponownym uruchomieniu aplikacji:

* Wszystkie tokeny na podstawie plików cookie uwierzytelniania są unieważniane.
* Użytkownicy muszą ponownie zaloguj się na ich następnego żądania.
* Wszystkie dane chronione za pomocą pierścień klucz może już nie mogły być odszyfrowane. Może to obejmować [tokenów CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) i [plików cookie programu ASP.NET Core MVC TempData](xref:fundamentals/app-state#tempdata).

Aby skonfigurować ochronę danych w celu utrwalenia i szyfrowania pierścienia kluczy, zobacz:

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="long-request-header-fields"></a>Długie pola nagłówka żądania

Jeśli aplikacja wymaga pól nagłówka żądania dłużej niż jest to dozwolone przez domyślne ustawienia serwera proxy (zazwyczaj 4K lub 8K w zależności od platformy), następujące dyrektywy wymagają korekty. Wartości, które mają zostać zastosowane, są zależne od scenariusza. Aby uzyskać więcej informacji, zapoznaj się z dokumentacją serwera.

* [proxy_buffer_size](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_buffer_size)
* [proxy_buffers](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_buffers)
* [proxy_busy_buffers_size](https://nginx.org/docs/http/ngx_http_proxy_module.html#proxy_busy_buffers_size)
* [large_client_header_buffers](https://nginx.org/docs/http/ngx_http_core_module.html#large_client_header_buffers)

> [!WARNING]
> Nie należy zwiększać wartości domyślnych buforów serwerów proxy, chyba że jest to konieczne. Zwiększenie tych wartości zwiększa ryzyko ataków przepełnienia buforu (przepełnienie) i ataki typu "odmowa usługi" (DoS) przez złośliwych użytkowników.

## <a name="secure-the-app"></a>Zabezpieczanie aplikacji

### <a name="enable-apparmor"></a>Włącz AppArmor

Moduły zabezpieczeń systemu Linux (LSM) to struktura, która jest częścią jądra systemu Linux od systemu Linux 2,6. LSM obsługuje różne implementacje modułów zabezpieczeń. [AppArmor](https://wiki.ubuntu.com/AppArmor) to LSM, który implementuje obowiązkowy System Access Control, który umożliwia confining programu z ograniczonym zestawem zasobów. Upewnij się, że AppArmor jest włączona i prawidłowo skonfigurowana.

### <a name="configure-the-firewall"></a>Konfigurowanie zapory

Zamknij wszystkie porty zewnętrzne, które nie są używane. Nieskomplikowana Zapora (UFW) zapewnia fronton dla programu `iptables` , dostarczając interfejs wiersza polecenia do konfigurowania zapory.

> [!WARNING]
> Zapora uniemożliwi dostęp do całego systemu, jeśli nie zostanie prawidłowo skonfigurowany. Niepowodzenie określenia poprawnego portu SSH spowoduje, że w przypadku korzystania z protokołu SSH w celu nawiązania połączenia z systemem zostanie on zablokowany. Domyślnym portem jest 22. Aby uzyskać więcej informacji, zobacz [wprowadzenie do UFW](https://help.ubuntu.com/community/UFW) i [Podręcznik](https://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).

Zainstaluj `ufw` i skonfiguruj ją, aby zezwalać na ruch na wszystkich portach.

```bash
sudo apt-get install ufw

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

sudo ufw enable
```

### <a name="secure-nginx"></a>Zabezpiecz Nginx

#### <a name="change-the-nginx-response-name"></a>Zmień nazwę odpowiedzi Nginx

Edit *src/http/ngx_http_header_filter_module.c*:

```
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a>Konfiguruj opcje

Skonfiguruj serwer przy użyciu dodatkowych wymaganych modułów. Rozważ użycie zapory aplikacji sieci Web, takiej jak [zapory ModSecurity](https://www.modsecurity.org/), aby zabezpieczyć aplikację.

#### <a name="https-configuration"></a>Konfiguracja protokołu HTTPS

**Konfigurowanie aplikacji do połączeń lokalnych (HTTPS)**

Polecenie [dotnet Run](/dotnet/core/tools/dotnet-run) używa pliku *właściwości/profilu launchsettings. JSON* aplikacji, który konfiguruje aplikację do nasłuchiwania na adresach URL `applicationUrl` dostarczonych przez właściwość (na przykład `https://localhost:5001; http://localhost:5000`).

Skonfiguruj aplikację do korzystania z certyfikatu podczas opracowywania dla `dotnet run` polecenia lub środowiska programistycznego (F5 lub CTRL + F5 w Visual Studio Code), korzystając z jednej z następujących metod:

* [Zastąp domyślny certyfikat z konfiguracji](xref:fundamentals/servers/kestrel#configuration) (*Zalecane*)
* [KestrelServerOptions.ConfigureHttpsDefaults](xref:fundamentals/servers/kestrel#configurehttpsdefaultsactionhttpsconnectionadapteroptions)

**Konfigurowanie zwrotnego serwera proxy dla połączeń zabezpieczonych za pośrednictwem protokołu HTTPS**

* Skonfiguruj serwer do nasłuchiwania ruchu https na porcie `443` , określając prawidłowy certyfikat wystawiony przez zaufany urząd certyfikacji (CA).

* Ochrona zabezpieczeń poprzez zastosowanie niektórych praktyk przedstawionych w następującym pliku */etc/nginx/Nginx.conf* . Przykłady obejmują wybranie silniejszego szyfru i przekierowanie całego ruchu przez protokół HTTP do protokołu HTTPS.

* Dodanie nagłówka `HTTP Strict-Transport-Security` (HSTS) gwarantuje, że wszystkie kolejne żądania wysyłane przez klienta korzystają z protokołu HTTPS.

* Nie dodawaj nagłówka HSTS ani nie wybieraj `max-age` odpowiedniego elementu, jeśli https zostanie wyłączony w przyszłości.

Dodaj plik konfiguracji */etc/nginx/proxy.conf* :

[!code-nginx[](linux-nginx/proxy.conf)]

Edytuj plik konfiguracji */etc/nginx/Nginx.conf* . Przykład zawiera obie `http` sekcje i `server` w jednym pliku konfiguracyjnym.

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Zabezpiecz Nginx z clickjacking

[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), znana także jako *atak polegająca na zaskarżeniu interfejsu użytkownika*, to złośliwy atak polegający na tym, że odwiedzanie witryny sieci Web jest trudne do kliknięcia linku lub przycisku na innej stronie niż aktualnie odwiedzane. Użyj `X-FRAME-OPTIONS` , aby zabezpieczyć lokację.

Aby wyeliminować ataki clickjacking:

1. Edytuj plik *Nginx. conf* :

   ```bash
   sudo nano /etc/nginx/nginx.conf
   ```

   Dodaj wiersz `add_header X-Frame-Options "SAMEORIGIN";`.
1. Zapisz plik.
1. Uruchom ponownie Nginx.

#### <a name="mime-type-sniffing"></a>Wykrywanie typu MIME

Ten nagłówek zapobiega większości przeglądarek z wykrywaniem MIME odpowiedzi w odniesieniu do zadeklarowanego typu zawartości, ponieważ nagłówek instruuje przeglądarkę, aby nie przesłaniał typu zawartości odpowiedzi. `nosniff` Jeśli na serwerze jest wyświetlana zawartość "text/html", przeglądarka renderuje ją jako "text/html".

Edytuj plik *Nginx. conf* :

```bash
sudo nano /etc/nginx/nginx.conf
```

Dodaj wiersz `add_header X-Content-Type-Options "nosniff";` i Zapisz plik, a następnie uruchom ponownie Nginx.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wymagania wstępne dotyczące programu .NET Core w systemie Linux](/dotnet/core/linux-prerequisites)
* [Nginx Wersje binarne: Oficjalne pakiety Debian/Ubuntu](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* <xref:test/troubleshoot>
* <xref:host-and-deploy/proxy-load-balancer>
* [NGINX Korzystanie z przekazanego nagłówka](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
