---
title: Host platformy ASP.NET Core w systemie Linux z Nginx
author: rick-anderson
description: Dowiedz się, jak można skonfigurować jako zwrotny serwer proxy na 16.04 Ubuntu, aby przesyłał dalej ruch HTTP dla aplikacji sieci web platformy ASP.NET Core systemem Kestrel Nginx.
ms.author: riande
ms.custom: mvc
ms.date: 05/22/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 374b13e0851cd171a7d8500a4965851a3a0eb49c
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277377"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a>Host platformy ASP.NET Core w systemie Linux z Nginx

Przez [Sourabh Shirhatti](https://twitter.com/sshirhatti)

W tym przewodniku opisano konfigurowanie środowiska ASP.NET Core gotowe do produkcji na serwerze Ubuntu 16.04. Te instrukcje, które mogą działać z nowszymi wersjami systemu Ubuntu, ale nie zostały przetestowane zgodnie z instrukcjami z nowszymi wersjami.

> [!NOTE]
> Dla Ubuntu 14.04 *supervisord* zaleca się rozwiązanie do monitorowania procesu Kestrel. *systemd* nie jest dostępna w Ubuntu 14.04. Aby uzyskać instrukcje Ubuntu 14.04, zobacz [poprzednią wersję tego tematu](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).

Ten przewodnik:

* Umieszcza istniejącej aplikacji platformy ASP.NET Core za zwrotnego serwera proxy.
* Konfiguruje zwrotnego serwera proxy do przekazywania żądań do serwera sieci web Kestrel.
* Gwarantuje, że aplikacja sieci web uruchamiany podczas uruchamiania jako demon.
* Konfiguruje narzędzie do zarządzania procesu, aby ponownie uruchomić aplikacji sieci web.

## <a name="prerequisites"></a>Wymagania wstępne

1. Dostęp do serwera Ubuntu 16.04 przy użyciu konta użytkowników standardowych z uprawnieniem sudo.
1. Zainstaluj środowisko uruchomieniowe .NET Core na serwerze.
   1. Odwiedź stronę [.NET Core wszystkie pliki do pobrania strony](https://www.microsoft.com/net/download/all).
   1. Wybierz z listy w obszarze najnowsze środowisko uruchomieniowe-preview **środowiska uruchomieniowego**.
   1. Wybierz, a następnie postępuj zgodnie z instrukcjami Ubuntu odpowiadających Ubuntu wersji serwera.
1. Istniejącej aplikacji platformy ASP.NET Core.

## <a name="publish-and-copy-over-the-app"></a>Publikowanie i skopiuj przez aplikację

Konfigurowanie aplikacji na potrzeby [wdrożenia zależne od framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).

Uruchom [publikowania dotnet](/dotnet/core/tools/dotnet-publish) z Środowisko deweloperskie do tworzenia pakietów aplikacji w katalogu (na przykład *bin i wersji/&lt;target_framework_moniker&gt;/ publish*) który można Uruchom na serwerze:

```console
dotnet publish --configuration Release
```

Aplikacja również mogą być publikowane jako [niezależne wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd) nie, aby obsługa środowiska uruchomieniowego .NET Core na serwerze.

Skopiuj aplikacji platformy ASP.NET Core do serwera przy użyciu narzędzia, która integruje się z przepływu organizacji (na przykład punkt połączenia usługi, SFTP). Często można znaleźć aplikacji sieci web w obszarze *var* katalog (na przykład *var/aspnetcore/hellomvc*).

> [!NOTE]
> W przypadku wdrożenia produkcyjnego ciągłej integracji przepływu pracy wykonuje pracę publikowania aplikacji i kopiowanie zasoby na serwerze.

Testowanie aplikacji:

1. W wierszu polecenia Uruchom aplikację: `dotnet <app_assembly>.dll`.
1. W przeglądarce przejdź do `http://<serveraddress>:<port>` można zweryfikować aplikacja działa w systemie Linux lokalnie.

## <a name="configure-a-reverse-proxy-server"></a>Skonfiguruj serwer zwrotnego serwera proxy

Zwrotny serwer proxy jest typowe dla obsługi aplikacji sieci web dynamicznych. Zwrotny serwer proxy kończy żądanie HTTP i przekazuje je do aplikacji platformy ASP.NET Core.

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> Albo konfiguracji&mdash;z lub bez zwrotnego serwera proxy&mdash;jest prawidłowa i obsługiwanych konfiguracji hostingu dla platformy ASP.NET Core w wersji 2.0 lub nowszej aplikacji. Aby uzyskać więcej informacji, zobacz [użycie Kestrel z zwrotny serwer proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a>Użyj serwera proxy wstecznego

Kestrel stanowi doskonałe rozwiązanie do obsługi zawartości dynamicznej z platformy ASP.NET Core. Funkcji obsługi sieci web nie są jednak jako funkcja sformatowany jako serwery usług IIS, Apache lub Nginx. Zwrotnego serwera proxy można odciążyć pracy, takie jak obsługę zawartości statycznej, buforowanie żądań kompresowania żądań i kończenia żądań SSL z serwera HTTP. Zwrotnego serwera proxy może znajdować się na dedykowanym komputerze lub mogą można wdrożyć obok serwera HTTP.

Na potrzeby tego przewodnika jest używany przez pojedyncze wystąpienie Nginx. Uruchamia go na tym samym serwerze, z serwera HTTP. Na podstawie wymagań, można wybrać różne ustawienia.

Ponieważ żądania są przekazywane przez zwrotny serwer proxy, należy użyć [przekazywane oprogramowanie pośredniczące nagłówki](xref:host-and-deploy/proxy-load-balancer) z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pakietu. Aktualizacje oprogramowania pośredniczącego `Request.Scheme`za pomocą `X-Forwarded-Proto` nagłówka, więc poprawne działanie tego przekierowania URI i innymi zasadami zabezpieczeń.

Każdego składnika, który jest zależny od systemu, takich jak uwierzytelnianie, generowanie konsolidacji przekierowania i używanie funkcji geolokalizacji, muszą znajdować się po wywołaniu przez oprogramowanie pośredniczące przekazane nagłówków. Ogólną zasadą przekazywane oprogramowanie pośredniczące nagłówków należy uruchomić przed innym oprogramowaniu pośredniczącym, z wyjątkiem diagnostyki i obsługi oprogramowania pośredniczącego błędów. Ta kolejność zapewnia, że oprogramowanie pośredniczące polegania na informacje przekazywane nagłówków może używać wartości nagłówka do przetwarzania.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Wywołanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metody w `Startup.Configure` przed wywołaniem [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) lub podobne oprogramowanie pośredniczące schematu uwierzytelniania. Konfigurowanie oprogramowania pośredniczącego do przekazywania `X-Forwarded-For` i `X-Forwarded-Proto` nagłówki:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Wywołanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metody w `Startup.Configure` przed wywołaniem [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) i [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) lub podobne schemat uwierzytelniania oprogramowanie pośredniczące. Konfigurowanie oprogramowania pośredniczącego do przekazywania `X-Forwarded-For` i `X-Forwarded-Proto` nagłówki:

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

Jeśli nie [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) są określone przez oprogramowanie pośredniczące są domyślne nagłówki do przekazywania `None`.

Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych serwerów proxy i moduły równoważenia obciążenia. Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).

### <a name="install-nginx"></a>Zainstaluj Nginx

Użyj `apt-get` do zainstalowania Nginx. Instalator tworzy *systemd* init skrypt uruchamiany Nginx jako demon na uruchamianie systemu. 

```bash
sudo -s
nginx=stable # use nginx=development for latest development version
add-apt-repository ppa:nginx/$nginx
apt-get update
apt-get install nginx
```

Ubuntu osobistych pakietu archiwum (PPA) obsługiwany przez ochotników, nie zostało przekazane przez [nginx.org](https://nginx.org/). Aby uzyskać więcej informacji, zobacz [Nginx: binarny wersjach: pakiety oficjalnego Debian/Ubuntu](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).

> [!NOTE]
> Jeśli wymagane są opcjonalne moduły Nginx, może być wymagane tworzenie Nginx ze źródła.

Po zainstalowaniu Nginx po raz pierwszy, jawnie uruchomić:

```bash
sudo service nginx start
```

Sprawdź, czy przeglądarka wyświetla domyślna strona początkowa dla Nginx. Strona docelowa jest dostępny w `http://<server_IP_address>/index.nginx-debian.html`.

### <a name="configure-nginx"></a>Skonfiguruj Nginx

Aby skonfigurować Nginx jako zwrotny serwer proxy do przesyłania żądań do aplikacji platformy ASP.NET Core, zmodyfikuj */etc/nginx/sites-available/default*. Otwórz go w edytorze tekstów i Zastąp zawartość z następujących czynności:

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

Gdy nie `server_name` dopasowania, Nginx korzysta z domyślnego serwera. Jeśli żaden serwer domyślny jest zdefiniowany, pierwszy serwer w pliku konfiguracji jest serwerem domyślnym. Najlepszym rozwiązaniem jest dodanie określonego domyślnego serwera, która zwraca kod stanu 444 w pliku konfiguracji. To jest przykład domyślny serwer konfiguracji:

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

Z poprzedniego pliku i domyślnego serwera konfiguracji, Nginx akceptuje publicznego ruch na porcie 80 z nagłówkiem hosta `example.com` lub `*.example.com`. Żądania niezgodni te hosty nie pobrać przekazywane do Kestrel. Nginx przekazuje żądania pasujące do Kestrel na `http://localhost:5000`. Zobacz [jak nginx przetwarza żądanie](https://nginx.org/docs/http/request_processing.html) Aby uzyskać więcej informacji.

> [!WARNING]
> Błąd w celu określenia odpowiedniego [dyrektywy nazwa_serwera](https://nginx.org/docs/http/server_names.html) przedstawia aplikacji luk w zabezpieczeniach. Powiązanie symbolu wieloznacznego domeny podrzędnej (na przykład `*.example.com`) nie stanowić to zagrożenie bezpieczeństwa, jeśli kontrolować domeny nadrzędnej cały (w przeciwieństwie do `*.com`, której występuje). Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.

Po ustanowieniu konfiguracji Nginx, uruchom `sudo nginx -t` Aby sprawdzić składnię plików konfiguracji. Jeśli test konfiguracji w pliku zakończy się pomyślnie, wymusić Nginx do zastosowania zmian, uruchamiając `sudo nginx -s reload`.

Aby bezpośrednio uruchamiać aplikacji na serwerze:

1. Przejdź do katalogu aplikacji.
1. Uruchom plik wykonywalny aplikacji: `./<app_executable>`.

Jeśli wystąpi błąd uprawnień, Zmień uprawnienia:

```console
chmod u+x <app_executable>
```

Jeśli aplikacja działa na serwerze, ale nie odpowiada za pośrednictwem Internetu, sprawdź ustawienia zapory serwera i upewnij się, że port 80 jest otwarty. Jeśli przy użyciu maszyny Wirtualnej systemu Ubuntu Azure, Dodaj regułę grupy zabezpieczeń sieci (NSG), która umożliwia przychodzący port 80 ruchu. Jak włączenie reguły ruchu przychodzącego ruchu wychodzącego jest automatycznie przyznawane jest niepotrzebna można włączyć reguły wychodzące port 80.

Po zakończeniu testowania aplikacji, zamknij aplikację z `Ctrl+C` w wierszu polecenia.

## <a name="monitoring-the-app"></a>Monitorowanie aplikacji

Serwer jest skonfigurowany do przekazywania żądań wysyłanych do `http://<serveraddress>:80` się do aplikacji platformy ASP.NET Core systemem Kestrel na `http://127.0.0.1:5000`. Nginx Konfigurowanie nie jest jednak do zarządzania procesem Kestrel. *systemd* może służyć do tworzenia pliku usługi, aby uruchomić i monitorować podstawowej aplikacji sieci web. *systemd* to system init zapewnia wiele zaawansowanych funkcji uruchamianie, zatrzymywanie oraz procesy zarządzania. 

### <a name="create-the-service-file"></a>Tworzenie pliku usługi

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
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

Jeśli użytkownik *danych www* nie jest używany przez tę konfigurację użytkownika zdefiniowane w tym miejscu musi być najpierw utworzyć i podane odpowiednie własność plików.

Linux ma system plików z uwzględnieniem wielkości liter. Ustawienie "Production" spowoduje wyszukiwanie w pliku konfiguracyjnym ASPNETCORE_ENVIRONMENT *appsettings. Production.JSON*, a nie *appsettings.production.json*.

> [!NOTE]
> Niektóre wartości (na przykład parametry połączenia SQL), należy użyć znaków ucieczki dla dostawców konfiguracji można odczytać zmiennych środowiskowych. Użyj następującego polecenia, aby wygenerować prawidłowo zmienionym wartość do użycia w pliku konfiguracji:
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

Zapisz plik i włączyć usługę.

```bash
systemctl enable kestrel-hellomvc.service
```

Uruchom usługę i sprawdź, czy jest uruchomiona.

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

Zwrotny serwer proxy skonfigurowane i zarządzanych za pomocą systemd Kestrel, aplikacji sieci web jest w pełni skonfigurowane i dostępne w przeglądarce na komputerze lokalnym w `http://localhost`. Jest również dostępny z komputera zdalnego, blokowanie wszelkich zaporą, która może być blokowane. Zapoznanie się nagłówki odpowiedzi `Server` nagłówek zawiera obsługiwanej przez Kestrel aplikacji platformy ASP.NET Core.

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Przeglądanie dzienników

Ponieważ aplikacja sieci web przy użyciu Kestrel odbywa się przy użyciu `systemd`, scentralizowane dziennika są rejestrowane wszystkie zdarzenia i procesów. Jednak ten dziennik zawiera wszystkie wpisy dla wszystkich usług i procesów zarządza `systemd`. Aby wyświetlić `kestrel-hellomvc.service`— określone elementy, użyj następującego polecenia:

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Dla dalszego filtrowania, opcje czasu takich jak `--since today`, `--until 1 hour ago` lub kombinacji tych można zmniejszyć liczbę wpisów zwracane.

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>Zabezpieczanie aplikacji

### <a name="enable-apparmor"></a>Włącz AppArmor

Linux zabezpieczeń modułów (LSM) tak, to platforma, która jest częścią jądra systemu Linux od 2.6 systemu Linux. LSM obsługuje różne implementacje modułów zabezpieczeń. [AppArmor](https://wiki.ubuntu.com/AppArmor) jest LSM, implementujący systemu obowiązkowe kontroli dostępu, który umożliwia ograniczenie program do określonych zasobów. Upewnij się, AppArmor jest włączony i poprawnie skonfigurowane.

### <a name="configuring-the-firewall"></a>Konfigurowanie zapory

Zamknij Wyłącz wszystkie porty zewnętrznych, które nie są używane. Zapora prostotę (ufw) zapewnia frontonu dla `iptables` korzystanie z interfejsu wiersza polecenia dla konfiguracji zapory. Sprawdź, czy `ufw` zezwalają na ruch na dowolne porty.

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a>Zabezpieczanie Nginx

#### <a name="change-the-nginx-response-name"></a>Zmień nazwę Nginx odpowiedzi

Edit *src/http/ngx_http_header_filter_module.c*:

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a>Skonfiguruj opcje

Skonfiguruj serwer z dodatkowe wymagane moduły. Należy rozważyć użycie zapory aplikacji sieci web, takich jak [ModSecurity](https://www.modsecurity.org/), w celu ograniczenia funkcjonalności aplikacji.

#### <a name="configure-ssl"></a>Konfigurowanie protokołu SSL

* Konfigurowanie serwera do nasłuchiwania na ruch HTTPS na porcie `443` , określając prawidłowy certyfikat wystawiony przez zaufany urząd certyfikacji.

* Ograniczenia funkcjonalności zabezpieczeń poprzez zastosowanie niektórych rozwiązań przedstawione poniżej */etc/nginx/nginx.conf* pliku. Przykłady obejmują wybranie silniejszego szyfrowania i przekierowywania całego ruchu za pośrednictwem protokołu HTTP, HTTPS.

* Dodawanie `HTTP Strict-Transport-Security` nagłówka (HSTS) zapewnia wszystkie kolejne żądania przez klienta są tylko za pośrednictwem protokołu HTTPS.

* Nie dodawaj nagłówka TLS Strict lub wybrać odpowiednie `max-age` Jeśli SSL zostanie wyłączone w przyszłości.

Dodaj */etc/nginx/proxy.conf* pliku konfiguracji:

[!code-nginx[](linux-nginx/proxy.conf)]

Edytuj */etc/nginx/nginx.conf* pliku konfiguracji. Przykład zawiera zarówno `http` i `server` sekcji w pliku konfiguracji jednego.

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Bezpieczny Nginx z porywaniu kliknięć
Porywaniu kliknięć to technika złośliwego zbierać zainfekowane użytkownik klika polecenie. Porywaniu kliknięć sztuczki ofiara (użytkownik) do kliknięcia zainfekowane w witrynie. Użyj X-FRAME-OPTIONS do zabezpieczenia witryny.

Edytuj *nginx.conf* pliku:

```bash
sudo nano /etc/nginx/nginx.conf
```

Dodaj wiersz `add_header X-Frame-Options "SAMEORIGIN";` i Zapisz plik, a następnie uruchom ponownie Nginx.

#### <a name="mime-type-sniffing"></a>Wykrywanie typ MIME

Ten nagłówek zapobiega w większości przeglądarek z wykrywanie MIME odpowiedzi od deklarowanego typu zawartości, jak nagłówka powoduje, że przeglądarka nie, aby zastąpić typ zawartości odpowiedzi. Z `nosniff` opcję, jeśli serwer jest wyświetlany komunikat "text/html" jest zawartość przeglądarki renderuje go jako "text/html".

Edytuj *nginx.conf* pliku:

```bash
sudo nano /etc/nginx/nginx.conf
```

Dodaj wiersz `add_header X-Content-Type-Options "nosniff";` i Zapisz plik, a następnie uruchom ponownie Nginx.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Nginx: Wersje binarne: oficjalny Debian/Ubuntu pakietów](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* [Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer)
* [NGINX: Nagłówek przekazane za pomocą](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
