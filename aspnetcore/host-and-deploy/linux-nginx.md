---
title: Host platformy ASP.NET Core w systemie Linux przy użyciu serwera Nginx
author: rick-anderson
description: Dowiedz się, jak skonfigurować serwer Nginx jako zwrotny serwer proxy w systemie Ubuntu 16.04 do przesyłania ruchu HTTP do aplikacji sieci web ASP.NET Core uruchomionych na Kestrel.
ms.author: riande
ms.custom: mvc
ms.date: 09/08/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 3891251b585ba0fd7f4ce58d824f1d82f40ae928
ms.sourcegitcommit: c684eb6c0999d11d19e15e65939e5c7f99ba47df
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46292352"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a>Host platformy ASP.NET Core w systemie Linux przy użyciu serwera Nginx

Przez [Sourabh Shirhatti](https://twitter.com/sshirhatti)

W tym przewodniku wyjaśniono Konfigurowanie środowiska ASP.NET Core gotowe do produkcji na serwerze z systemem Ubuntu 16.04. Te instrukcje, które mogą działać z nowszymi wersjami systemu Ubuntu, ale instrukcje nie zostały przetestowane z nowszymi wersjami.

Aby uzyskać informacje na inne dystrybucje systemu Linux obsługiwane przez program ASP.NET Core, zobacz [wymagania wstępne dla platformy .NET Core w systemie Linux](/dotnet/core/linux-prerequisites).

> [!NOTE]
> Aby uzyskać Ubuntu 14.04 *supervisord* jest zalecane jako rozwiązanie do monitorowania procesu Kestrel. *systemd* nie jest dostępna w systemie Ubuntu 14.04. Ubuntu 14.04 instrukcje można znaleźć [poprzednią wersję tego tematu](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).

Ten przewodnik:

* Umieszcza istniejącej aplikacji platformy ASP.NET Core za serwerem proxy odwrotnej.
* Konfiguruje zwrotnego serwera proxy do przekazywania żądań do serwera sieci web Kestrel.
* Gwarantuje, że aplikacja sieci web jest uruchamiana podczas uruchamiania jako demon.
* Konfiguruje narzędzia do zarządzania procesami, aby pomóc, uruchom ponownie aplikację internetową.

## <a name="prerequisites"></a>Wymagania wstępne

1. Dostęp do serwera Ubuntu 16.04 przy użyciu konta użytkownika standardowego przy użyciu uprawnień "sudo".
1. Na serwerze, należy zainstalować środowisko uruchomieniowe platformy .NET Core.
   1. Odwiedź stronę [.NET Core wszystkie strony plików do pobrania](https://www.microsoft.com/net/download/all).
   1. Z listy w obszarze wybierz najnowsze środowisko uruchomieniowe — wersja zapoznawcza **środowiska uruchomieniowego**.
   1. Wybierz, a następnie postępuj zgodnie z instrukcjami dotyczącymi Ubuntu, które są zgodne z wersją systemu Ubuntu server.
1. Istniejącą aplikację ASP.NET Core.

## <a name="publish-and-copy-over-the-app"></a>Publikowanie i skopiuj aplikacji

Konfigurowanie aplikacji na potrzeby [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd).

Uruchom [publikowania dotnet](/dotnet/core/tools/dotnet-publish) ze środowiska projektowego, aby utworzyć pakiet aplikacji do katalogu (na przykład *bin/wydawania/&lt;target_framework_moniker&gt;/ publish*), można Uruchom na serwerze:

```console
dotnet publish --configuration Release
```

Aplikację można także publikować jako [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd) Jeśli wolisz nie zachować środowisko uruchomieniowe platformy .NET Core na serwerze.

Skopiuj aplikacji ASP.NET Core na serwer przy użyciu narzędzia, która integruje się z przepływu pracy w organizacji, (na przykład punkt połączenia usługi, SFTP). Często do lokalizowania aplikacji sieci web w obszarze *var* katalog (na przykład *aspnetcore/var/hellomvc*).

> [!NOTE]
> W przypadku wdrożenia produkcyjnego przepływu pracy ciągłej integracji działa publikowania aplikacji i kopiowanie zasobów do serwera.

Testowanie aplikacji:

1. W wierszu polecenia Uruchom aplikację: `dotnet <app_assembly>.dll`.
1. W przeglądarce przejdź do `http://<serveraddress>:<port>` Aby sprawdzić, aplikacja działa lokalnie w systemie Linux.

## <a name="configure-a-reverse-proxy-server"></a>Konfigurowanie zwrotnego serwera proxy

Zwrotny serwer proxy jest wspólne dla aplikacji sieci web dynamicznego obsługująca. Zwrotny serwer proxy kończy żądanie HTTP i przekazuje je do aplikacji ASP.NET Core.

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> Każda konfiguracja&mdash;z lub bez serwera proxy odwrotnej&mdash;jest prawidłowy i obsługiwanych konfiguracji hostingu dla platformy ASP.NET Core 2.0 lub nowszej aplikacje. Aby uzyskać więcej informacji, zobacz [kiedy należy używać Kestrel przy użyciu zwrotnego serwera proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a>Użyj serwera proxy odwrotnej

Kestrel nadaje się doskonale dla obsługujących zawartość dynamiczną z platformą ASP.NET Core. Jednak możliwości usług sieci web nie są jako wyposażonym jako serwerów, takich jak usługi IIS, Apache i Nginx. Serwer proxy odwrotnej można odciążyć pracy, takich jak obsługująca zawartość statyczną, buforowanie żądań kompresowania żądań i kończenie żądań SSL z serwerem HTTP. Zwrotnego serwera proxy mogą znajdować się na dedykowanym komputerze lub można wdrażać wraz z serwerem HTTP.

Na potrzeby tego przewodnika pojedyncze wystąpienie serwera Nginx jest używany. Działa na tym samym serwerze, wraz z serwerem HTTP. Na podstawie wymagań, można wybrać różne ustawienia.

Ponieważ żądania są przekazywane przez zwrotny serwer proxy, należy użyć [przekazywane oprogramowania pośredniczącego nagłówki](xref:host-and-deploy/proxy-load-balancer) z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pakietu. Aktualizacje oprogramowania pośredniczącego `Request.Scheme`przy użyciu `X-Forwarded-Proto` nagłówka, więc działanie tego identyfikatory URI przekierowań i innych zasad zabezpieczeń.

Dowolny składnik, który jest zależny od systemu, takie jak uwierzytelnianie, generowanie konsolidacji, przekierowań i geolokalizacja, muszą być umieszczone po wywołaniu oprogramowanie pośredniczące przekazane nagłówków. Zgodnie z ogólną zasadą przekazywane oprogramowania pośredniczącego nagłówki należy uruchomić przed innym oprogramowaniu pośredniczącym, z wyjątkiem diagnostyki i obsługi oprogramowania pośredniczącego błędów. Ta kolejność gwarantuje, że oprogramowanie pośredniczące, opierając się na nagłówki przekazywane informacje mogą wykorzystywać wartości nagłówka do przetworzenia.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Wywoływanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in Class metoda `Startup.Configure` przed wywołaniem [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) lub podobne oprogramowanie pośredniczące schematu uwierzytelniania. Konfigurowanie oprogramowania pośredniczącego, aby przekazywać `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

---

Jeśli nie [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) są określone oprogramowanie pośredniczące, są domyślne nagłówki do przekazywania `None`.

Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych za serwery proxy i moduły równoważenia obciążenia. Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).

### <a name="install-nginx"></a>Zainstalować rozwiązanie Nginx

Użyj `apt-get` do zainstalowania serwera Nginx. Instalator tworzy *systemd* skryptu init, który uruchamia serwer Nginx jako demon przy uruchamianiu systemu. Postępuj zgodnie z instrukcjami instalacji dla systemu Ubuntu na [Nginx: pakiety oficjalne Debian/Ubuntu](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).

> [!NOTE]
> Jeśli wymagane są opcjonalne modułów serwera Nginx, może być wymagane tworzenia Nginx ze źródła.

Po zainstalowaniu serwera Nginx po raz pierwszy, jawnie uruchom ją, uruchamiając:

```bash
sudo service nginx start
```

Sprawdź, czy przeglądarka wyświetla wartość domyślna strona docelowa dla kontenera Nginx. Strona docelowa jest dostępny na `http://<server_IP_address>/index.nginx-debian.html`.

### <a name="configure-nginx"></a>Konfigurowanie serwera Nginx

Aby skonfigurować serwer Nginx jako zwrotny serwer proxy, aby przekazywać żądania do aplikacji platformy ASP.NET Core, zmodyfikuj */etc/nginx/sites-available/default*. Otwórz go w edytorze tekstów i Zastąp zawartość następujących czynności:

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

Gdy nie `server_name` dopasowania, Nginx korzysta z domyślnego serwera. Jeśli żaden serwer domyślny jest zdefiniowany, pierwszy serwer w pliku konfiguracji jest domyślny serwer. Najlepszym rozwiązaniem jest dodanie określonych domyślnego serwera, która zwraca kod stanu 444 w pliku konfiguracji. Przedstawiono przykładową konfigurację serwera domyślnego:

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

Za pomocą poprzedniego pliku i domyślnego serwera konfiguracji Nginx akceptuje publicznych ruch na porcie 80 z nagłówkiem hosta `example.com` lub `*.example.com`. Żądania, które nie pasują te hosty nie uzyskać przekazywane do Kestrel. Serwer Nginx przekazuje żądania pasujących do Kestrel w `http://localhost:5000`. Zobacz [przetwarzaniu żądania przez serwer nginx](https://nginx.org/docs/http/request_processing.html) Aby uzyskać więcej informacji. Aby zmienić port adresu IP firmy Kestrel, zobacz [Kestrel: Konfiguracja punktu końcowego](xref:fundamentals/servers/kestrel#endpoint-configuration).

> [!WARNING]
> Nie można określić poprawną [dyrektywy nazwa_serwera](https://nginx.org/docs/http/server_names.html) ujawnia luki w zabezpieczeniach aplikacji. Powiązanie symbol wieloznaczny domeny podrzędnej (na przykład `*.example.com`) nie stanowić to zagrożenie bezpieczeństwa, jeśli możesz kontrolować domenę nadrzędną całego (w przeciwieństwie do `*.com`, który jest narażony). Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.

Po ustanowieniu konfigurację serwera Nginx, uruchom `sudo nginx -t` Aby sprawdzić składnię plików konfiguracyjnych. Jeśli test konfiguracji w pliku zakończy się pomyślnie, wymusić Nginx, aby wczytać zmiany, uruchamiając `sudo nginx -s reload`.

Aby bezpośrednio uruchamiać aplikację na serwerze:

1. Przejdź do katalogu aplikacji.
1. Uruchom plik wykonywalny aplikacji: `./<app_executable>`.

Jeśli wystąpi błąd uprawnień, Zmień uprawnienia:

```console
chmod u+x <app_executable>
```

Jeśli aplikacja działa na serwerze, ale nie odpowiada za pośrednictwem Internetu, sprawdź ustawienia zapory serwera i upewnij się, że port 80 jest otwarty. Jeśli używasz systemu Ubuntu Maszynie wirtualnej platformy Azure, Dodaj regułę sieciowej grupy zabezpieczeń (NSG), która włącza port przychodzący ruch 80. Nie ma potrzeby można włączyć reguły ruchu wychodzącego portu 80, jak ruch wychodzący jest udzielany automatycznie po włączeniu reguły dla ruchu przychodzącego.

Po zakończeniu testowania aplikacji, zamknij aplikację za pomocą `Ctrl+C` w wierszu polecenia.

## <a name="monitoring-the-app"></a>Monitorowanie aplikacji

Serwer jest skonfigurowany do przekazywania żądań kierowanych do `http://<serveraddress>:80` się do aplikacji platformy ASP.NET Core uruchomionych na Kestrel na `http://127.0.0.1:5000`. Jednak serwer Nginx nie jest skonfigurować do zarządzania procesem Kestrel. *systemd* może służyć do tworzenia pliku usługi, aby uruchomić i monitorować podstawowej aplikacji sieci web. *systemd* to system init, który zapewnia wiele funkcji zaawansowanych uruchamianie, zatrzymywanie oraz zarządzanie procesami. 

### <a name="create-the-service-file"></a>Utwórz plik usługi

Tworzenie pliku definicji usługi:

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Poniżej przedstawiono przykładowy plik usługi dla aplikacji:

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
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

Jeśli użytkownik *danych www* nie jest używany przez tę konfigurację użytkownika zdefiniowane w tym miejscu należy najpierw utworzyć i podane odpowiednie prawa własności plików.

Użyj `TimeoutStopSec` skonfigurować czas oczekiwania na aplikację, aby zamknięty po odebraniu sygnału przerwania początkowej. Jeśli aplikacja nie zamknięty w tym okresie, aby zakończyć aplikację zgłaszany jest SIGKILL. Podaj wartość jako unitless sekund (na przykład `150`), czas span wartości (na przykład `2min 30s`), lub `infinity` wyłączyć limit czasu. `TimeoutStopSec` Wartość domyślna to wartość `DefaultTimeoutStopSec` w pliku konfiguracji Menedżera (*systemd system.conf*, *system.conf.d*, *systemd user.conf*,  *User.conf.d*). Domyślna wartość limitu czasu dla większości dystrybucji wynosi 90 s.

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

System plików rozróżniana wielkość liter jest systemu Linux. Ustawienie ASPNETCORE_ENVIRONMENT "Produkcyjne" wyniki wyszukiwania dla pliku konfiguracji w *appsettings. Production.JSON*, a nie *appsettings.production.json*.

Niektóre wartości (na przykład parametry połączenia SQL), należy użyć znaków ucieczki dla dostawców konfiguracji można odczytać zmienne środowiskowe. Użyj następującego polecenia do generowania prawidłowo o zmienionym znaczeniu wartości do użycia w pliku konfiguracji:

```console
systemd-escape "<value-to-escape>"
```

Zapisz plik i włączyć usługę.

```bash
sudo systemctl enable kestrel-hellomvc.service
```

Uruchom usługę i sprawdź, czy jest uruchomiona.

```
sudo systemctl start kestrel-hellomvc.service
sudo systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

Zwrotny serwer proxy, skonfigurowane i Kestrel zarządzane za pośrednictwem systemd, aplikacji sieci web jest w pełni skonfigurowane i dostępne za pomocą przeglądarki na komputerze lokalnym w `http://localhost`. Jest także dostępny z komputera zdalnego, wyłączając wszelkie zapory, która może blokować. Inspekcja nagłówki odpowiedzi `Server` nagłówka przedstawiono aplikację ASP.NET Core, obsługiwanej przez Kestrel.

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Wyświetlanie dzienników

Ponieważ aplikacja sieci web przy użyciu Kestrel odbywa się przy użyciu `systemd`, scentralizowane dziennika są rejestrowane wszystkie zdarzenia i procesów. Jednak ten dziennik zawiera wszystkie wpisy dla wszystkich usług i procesów, które zarządza `systemd`. Aby wyświetlić `kestrel-hellomvc.service`— określone elementy, użyj następującego polecenia:

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Do dalszego filtrowania, opcje czasu takie jak `--since today`, `--until 1 hour ago` lub kombinację tych można zmniejszyć liczbę zwróconych pozycji.

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a>Ochrona danych

[Stosu ochrony danych programu ASP.NET Core](xref:security/data-protection/index) jest używana przez kilka platformy ASP.NET Core [middlewares](xref:fundamentals/middleware/index), w tym oprogramowania pośredniczącego uwierzytelniania (na przykład, oprogramowaniu pośredniczącym pliku cookie) i fałszerstwo żądania międzywitrynowego (CSRF) zabezpieczenia. Nawet wtedy, gdy interfejsów API ochrony danych nie są wywoływane przez kod użytkownika, ochrony danych należy skonfigurować tak, aby utworzyć trwałe kryptograficznych [magazynu kluczy](xref:security/data-protection/implementation/key-management). Jeśli nie jest skonfigurowana ochrona danych, klucze są przechowywane w pamięci i odrzucone po ponownym uruchomieniu aplikacji.

Jeśli pierścień klucz jest przechowywany w pamięci, po ponownym uruchomieniu aplikacji:

* Wszystkie tokeny na podstawie plików cookie uwierzytelniania są unieważniane.
* Użytkownicy muszą ponownie zaloguj się na ich następnego żądania.
* Wszystkie dane chronione za pomocą pierścień klucz może już nie mogły być odszyfrowane. Może to obejmować [tokenów CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) i [plików cookie programu ASP.NET Core MVC TempData](xref:fundamentals/app-state#tempdata).

Aby skonfigurować ochronę danych na zostaną zachowane, a pierścień klucz szyfrowania, zobacz:

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="securing-the-app"></a>Zabezpieczanie aplikacji

### <a name="enable-apparmor"></a>Włącz AppArmor

Linux zabezpieczeń modułów (LSM) tak, to struktura, która jest częścią jądra systemu Linux od systemu Linux w wersji 2.6. LSM obsługuje różne implementacje modułach zabezpieczeń. [AppArmor](https://wiki.ubuntu.com/AppArmor) jest LSM, implementujący systemu obowiązkowe kontroli dostępu, który umożliwia ograniczenie program ma ograniczony zestaw zasobów. Upewnij się, AppArmor jest włączone i skonfigurowane prawidłowo.

### <a name="configuring-the-firewall"></a>Konfigurowanie zapory

Zamknij wyłączanie wszystkich portów zewnętrznych, które nie są używane. Zapora prostotę (ufw) zawiera frontonu na potrzeby `iptables` , zapewniając interfejs wiersza polecenia dla konfiguracji zapory.

> [!WARNING]
> Zapora uniemożliwi dostęp do całego systemu Jeśli nie zostały skonfigurowane prawidłowo. Nie można określić prawidłowy port SSH będzie efektywnie zablokowania systemu korzystania z protokołu SSH do nawiązania połączenia. Domyślny port to 22. Aby uzyskać więcej informacji, zobacz [wprowadzenie do ufw](https://help.ubuntu.com/community/UFW) i [ręczne](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).

Zainstaluj `ufw` i skonfiguruj ją tak, aby zezwolić na ruch na portach, wszelkie potrzebne.

```bash
sudo apt-get install ufw

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

sudo ufw enable
```

### <a name="securing-nginx"></a>Zabezpieczanie serwera Nginx

#### <a name="change-the-nginx-response-name"></a>Zmień nazwę odpowiedzi serwera Nginx

Edit *src/http/ngx_http_header_filter_module.c*:

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a>Skonfiguruj opcje

Konfigurowanie serwera przy użyciu dodatkowych wymaganych modułów. Należy wziąć pod uwagę przy użyciu zapory aplikacji sieci web, takich jak [zapory ModSecurity](https://www.modsecurity.org/), w celu ograniczenia funkcjonalności aplikacji.

#### <a name="configure-ssl"></a>Konfigurowanie certyfikatu SSL

* Konfigurowanie serwera do nasłuchiwania ruchu HTTPS na porcie `443` , określając prawidłowy certyfikat wystawiony przez zaufany urząd certyfikacji (CA).

* Wzmocnić zabezpieczenia przez wprowadzenie niektórych rozwiązań przedstawiony w następującym */etc/nginx/nginx.conf* pliku. Przykłady obejmują wybranie silniejszego szyfrowania i przekierowania całego ruchu za pośrednictwem protokołu HTTP do HTTPS.

* Dodawanie `HTTP Strict-Transport-Security` nagłówka (HSTS) zapewnia wszystkie kolejne żądania wysłane przez klienta są tylko za pośrednictwem protokołu HTTPS.

* Nie dodawaj nagłówka zabezpieczeń w przypadku transportu Strict, lub wybrać odpowiednią `max-age` Jeśli SSL zostanie wyłączona w przyszłości.

Dodaj */etc/nginx/proxy.conf* pliku konfiguracji:

[!code-nginx[](linux-nginx/proxy.conf)]

Edytuj */etc/nginx/nginx.conf* pliku konfiguracji. Przykład zawiera zarówno `http` i `server` sekcji w pliku w jednej konfiguracji.

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Zabezpieczenia serwera Nginx z porywaniu kliknięć
Porywaniu kliknięć jest techniki złośliwego Zbieraj zainfekowane użytkownik klika polecenie. Porywaniu kliknięć wskazówki ofiarą (gości) do kliknięcia w witrynie zainfekowane. Użyj X-FRAME-OPTIONS na zabezpieczenie witryny.

Edytuj *nginx.conf* pliku:

```bash
sudo nano /etc/nginx/nginx.conf
```

Dodaj wiersz `add_header X-Frame-Options "SAMEORIGIN";` i Zapisz plik, a następnie uruchom ponownie serwer Nginx.

#### <a name="mime-type-sniffing"></a>Wykrywanie typu MIME

Tego pliku nagłówkowego zapobiega w większości przeglądarek z wykrywanie MIME odpowiedzi od deklarowanej typu zawartości jako nagłówek powoduje, że przeglądarka nie, aby zastąpić typ zawartości odpowiedzi. Za pomocą `nosniff` opcji, jeśli serwer jest w stanie "text/html" jest zawartość, Przeglądarka renderuje je jako "text/html".

Edytuj *nginx.conf* pliku:

```bash
sudo nano /etc/nginx/nginx.conf
```

Dodaj wiersz `add_header X-Content-Type-Options "nosniff";` i Zapisz plik, a następnie uruchom ponownie serwer Nginx.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wymagania wstępne dla platformy .NET Core w systemie Linux](/dotnet/core/linux-prerequisites)
* [Nginx: Wersje binarne: pakiety oficjalne Debian/Ubuntu](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer)
* [Serwer NGINX: Przy użyciu nagłówka przekazane](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
