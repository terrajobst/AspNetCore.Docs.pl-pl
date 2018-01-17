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
ms.openlocfilehash: cc15efc25abbfb5bfc9b748b49802afebc75bfb2
ms.sourcegitcommit: 87168cdc409e7a7257f92a0f48f9c5ab320b5b28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a>Host platformy ASP.NET Core w systemie Linux z nginx

Przez [Sourabh Shirhatti](https://twitter.com/sshirhatti)

W tym przewodniku opisano konfigurowanie środowiska ASP.NET Core gotowe do produkcji na 16.04 Ubuntu Server.

**Uwaga:** dla Ubuntu 14.04 *supervisord* zaleca się rozwiązanie do monitorowania procesu Kestrel. *systemd* nie jest dostępna w Ubuntu 14.04. [Zobacz poprzednią wersję tego dokumentu](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

Ten przewodnik:

* Umieszcza istniejącej aplikacji platformy ASP.NET Core za zwrotnego serwera proxy.
* Konfiguruje zwrotnego serwera proxy do przekazywania żądań do serwera sieci web Kestrel.
* Gwarantuje, że aplikacja sieci web uruchamiany podczas uruchamiania jako demon.
* Konfiguruje narzędzie do zarządzania procesu, aby ponownie uruchomić aplikacji sieci web.

## <a name="prerequisites"></a>Wymagania wstępne

1. Dostęp do serwera 16.04 Ubuntu przy użyciu konta użytkowników standardowych z uprawnieniami sudo
1. Istniejącej aplikacji platformy ASP.NET Core

## <a name="copy-over-the-app"></a>Skopiuj przez aplikację

Uruchom `dotnet publish` ze środowiska deweloperskiego do pakietu aplikacji do katalogu autonomiczną, która może działać na serwerze.

Skopiuj aplikacji platformy ASP.NET Core na serwerze przy użyciu dowolnego narzędzia integruje przepływu organizacji (na przykład punkt połączenia usługi, FTP). Testowanie aplikacji, na przykład:

* W wierszu polecenia Uruchom `dotnet <app_assembly>.dll`.
* W przeglądarce przejdź do `http://<serveraddress>:<port>` można zweryfikować aplikacja działa w systemie Linux. 
 
## <a name="configure-a-reverse-proxy-server"></a>Skonfiguruj serwer zwrotnego serwera proxy

Zwrotny serwer proxy jest typowe dla obsługi aplikacji sieci web dynamicznych. Zwrotny serwer proxy kończy żądanie HTTP i przekazuje je do aplikacji platformy ASP.NET Core.

### <a name="why-use-a-reverse-proxy-server"></a>Dlaczego warto używać zwrotnego serwera proxy?

Kestrel stanowi doskonałe rozwiązanie do obsługi zawartości dynamicznej z platformy ASP.NET Core; jednak części usług sieci web nie są jako funkcja sformatowany jako serwery, takimi jak usługi IIS, Apache lub nginx. Zwrotnego serwera proxy można odciążyć pracy, takich jak obsługę zawartości statycznej, buforowanie żądań, kompresowania żądań i kończenia żądań SSL z serwera HTTP. Zwrotnego serwera proxy może znajdować się na dedykowanym komputerze lub mogą można wdrożyć obok serwera HTTP.

Na potrzeby tego przewodnika jest używany przez pojedyncze wystąpienie nginx. Uruchamia go na tym samym serwerze, z serwera HTTP. Na podstawie wymagań, różnych konfiguracji może być wybrana opcja.

Ponieważ żądania są przekazywane przez zwrotny serwer proxy, należy użyć `ForwardedHeaders` oprogramowania pośredniczącego z `Microsoft.AspNetCore.HttpOverrides` pakietu. Aktualizacje tego oprogramowania pośredniczącego `Request.Scheme`za pomocą `X-Forwarded-Proto` nagłówka, więc poprawne działanie tego przekierowania URI i innymi zasadami zabezpieczeń.

Podczas konfigurowania serwera zwrotnego serwera proxy, oprogramowanie pośredniczące uwierzytelniania musi `UseForwardedHeaders` ma być uruchomiony. Ta kolejność zapewnia, że oprogramowanie pośredniczące uwierzytelniania może wykorzystywać odpowiednich wartości i generowanie poprawne przekierowania URI.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Wywołanie `UseForwardedHeaders` — metoda (w `Configure` metody *Startup.cs*) przed wywołaniem `UseAuthentication` lub podobne oprogramowanie pośredniczące schemat uwierzytelniania:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Wywołanie `UseForwardedHeaders` — metoda (w `Configure` metody *Startup.cs*) przed wywołaniem `UseIdentity` i `UseFacebookAuthentication` lub podobne oprogramowanie pośredniczące schemat uwierzytelniania:

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

### <a name="install-nginx"></a>Zainstaluj nginx

```bash
sudo apt-get install nginx
```

> [!NOTE]
> Jeśli zostaną zainstalowane moduły opcjonalne nginx, może być wymagane tworzenie nginx ze źródła.

Użyj `apt-get` do zainstalowania nginx. Instalator tworzy skrypt init System V uruchamiany nginx jako demon podczas uruchamiania systemu. Po zainstalowaniu nginx po raz pierwszy, jawnie uruchomić:

```bash
sudo service nginx start
```

Sprawdź, czy przeglądarka wyświetla domyślna strona dla nginx początkowa.

### <a name="configure-nginx"></a>Skonfiguruj nginx

Aby skonfigurować nginx jako zwrotny serwer proxy do przesyłania żądań do aplikacji platformy ASP.NET Core, zmodyfikuj `/etc/nginx/sites-available/default`. Otwórz go w edytorze tekstów i Zastąp zawartość z następujących czynności:

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

Ten plik konfiguracji nginx przekazuje ruch przychodzący publicznych z portu `80` do portu `5000`.

Po ustanowieniu konfiguracji nginx, uruchom `sudo nginx -t` Aby sprawdzić składnię plików konfiguracji. Jeśli test konfiguracji w pliku zakończy się pomyślnie, wymusić nginx do zastosowania zmian, uruchamiając `sudo nginx -s reload`.

## <a name="monitoring-the-app"></a>Monitorowanie aplikacji

Serwer jest skonfigurowany do przekazywania żądań wysyłanych do `http://<serveraddress>:80` się do aplikacji platformy ASP.NET Core systemem Kestrel na `http://127.0.0.1:5000`. Jednak nginx nie skonfigurowano do zarządzania procesem Kestrel. *systemd* może służyć do tworzenia pliku usługi, aby uruchomić i monitorować podstawowej aplikacji sieci web. *systemd* to system init zapewnia wiele zaawansowanych funkcji uruchamianie, zatrzymywanie oraz procesy zarządzania. 

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

**Uwaga:** Jeśli użytkownik *danych www* nie jest używany przez tę konfigurację użytkownika zdefiniowane w tym miejscu musi być najpierw utworzyć i podane odpowiednie własność plików.
**Uwaga:** Linux ma system plików z uwzględnieniem wielkości liter. Ustawienie "Production" spowoduje wyszukiwanie w pliku konfiguracyjnym ASPNETCORE_ENVIRONMENT *appsettings. Production.JSON*, a nie *appsettings.production.json*.

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

### <a name="securing-nginx"></a>Zabezpieczanie nginx

Rozkład domyślne nginx nie Włącz protokół SSL. Aby włączyć dodatkowe funkcje zabezpieczeń, kompilacji ze źródła.

#### <a name="download-the-source-and-install-the-build-dependencies"></a>Pobierz źródła i zainstaluj zależności kompilacji

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a>Zmień nazwę nginx odpowiedzi

Edit *src/http/ngx_http_header_filter_module.c*:

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a>Skonfiguruj opcje i kompilacji

Biblioteka PCRE jest wymagany dla wyrażeń regularnych. Wyrażenia regularne są używane w dyrektywie lokalizacji dla ngx_http_rewrite_module. Http_ssl_module dodaje obsługę protokołu HTTPS.

Należy rozważyć użycie zapory aplikacji sieci web, takich jak *ModSecurity* celu ograniczenia funkcjonalności aplikacji.

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a>Konfigurowanie protokołu SSL

* Konfigurowanie serwera do nasłuchiwania na ruch HTTPS na porcie `443` , określając prawidłowy certyfikat wystawiony przez zaufany urząd certyfikacji.

* Ograniczenia funkcjonalności zabezpieczeń poprzez zastosowanie niektórych rozwiązań przedstawione poniżej */etc/nginx/nginx.conf* pliku. Przykłady obejmują wybranie silniejszego szyfrowania i przekierowywania całego ruchu za pośrednictwem protokołu HTTP, HTTPS.

* Dodawanie `HTTP Strict-Transport-Security` nagłówka (HSTS) zapewnia wszystkie kolejne żądania przez klienta są tylko za pośrednictwem protokołu HTTPS.

* Nie dodawaj nagłówka TLS Strict lub wybrać odpowiednie `max-age` Jeśli SSL zostanie wyłączone w przyszłości.

Dodaj */etc/nginx/proxy.conf* pliku konfiguracji:

[!code-nginx[Main](linux-nginx/proxy.conf)]

Edytuj */etc/nginx/nginx.conf* pliku konfiguracji. Przykład zawiera zarówno `http` i `server` sekcji w pliku konfiguracji jednego.

[!code-nginx[Main](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Bezpieczne nginx z porywaniu kliknięć
Porywaniu kliknięć to technika złośliwego zbierać zainfekowane użytkownik klika polecenie. Porywaniu kliknięć sztuczki ofiara (użytkownik) do kliknięcia zainfekowane w witrynie. Użyj X-FRAME-OPTIONS do zabezpieczenia witryny.

Edytuj *nginx.conf* pliku:

```bash
sudo nano /etc/nginx/nginx.conf
```

Dodaj wiersz `add_header X-Frame-Options "SAMEORIGIN";` i Zapisz plik, a następnie uruchom ponownie nginx.

#### <a name="mime-type-sniffing"></a>Wykrywanie typ MIME

Ten nagłówek zapobiega w większości przeglądarek z wykrywanie MIME odpowiedzi od deklarowanego typu zawartości, jak nagłówka powoduje, że przeglądarka nie, aby zastąpić typ zawartości odpowiedzi. Z `nosniff` opcję, jeśli serwer jest wyświetlany komunikat "text/html" jest zawartość przeglądarki renderuje go jako "text/html".

Edytuj *nginx.conf* pliku:

```bash
sudo nano /etc/nginx/nginx.conf
```

Dodaj wiersz `add_header X-Content-Type-Options "nosniff";` i Zapisz plik, a następnie uruchom ponownie nginx.
