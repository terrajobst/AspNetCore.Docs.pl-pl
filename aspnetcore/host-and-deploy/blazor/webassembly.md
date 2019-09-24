---
title: Hostowanie i wdrażanie ASP.NET Core Blazor webassembly
author: guardrex
description: Dowiedz się, jak hostować i wdrażać aplikację Blazor przy użyciu ASP.NET Core, usługi Content Delivery Networks (CDN), serwerów plików i stron usługi GitHub.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: host-and-deploy/blazor/webassembly
ms.openlocfilehash: cdb424137d80b280873347c1352fc43d23b4aec3
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2019
ms.locfileid: "71211626"
---
# <a name="host-and-deploy-aspnet-core-blazor-webassembly"></a>Hostowanie i wdrażanie ASP.NET Core Blazor webassembly

[Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)i [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Z [modelem hostingu Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly):

* Aplikacja Blazor, jej zależności i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.
* Aplikacja jest wykonywana bezpośrednio w wątku interfejsu użytkownika przeglądarki.

Obsługiwane są następujące strategie wdrażania:

* Aplikacja Blazor jest obsługiwana przez aplikację ASP.NET Core. Ta strategia jest objęta [wdrożeniem hostowanym za pomocą ASP.NET Core](#hosted-deployment-with-aspnet-core) sekcji.
* Aplikacja Blazor jest umieszczana na statycznym, hostingowym serwerze sieci Web lub usłudze, w której program .NET nie jest używany do obsługi aplikacji Blazor. Ta strategia została omówiona w sekcji [wdrażanie autonomiczne](#standalone-deployment) , która obejmuje informacje dotyczące hostingu aplikacji Blazor webassembly jako aplikacji PODRZĘDNEJ usług IIS.

## <a name="rewrite-urls-for-correct-routing"></a>Ponownie Napisz adresy URL pod kątem prawidłowego routingu

Żądania routingu dla składników strony w aplikacji Blazor webassembly nie są tak proste jak żądania routingu na serwerze Blazor, hostowanej aplikacji. Rozważmy aplikację webassembly Blazor z dwoma składnikami:

* *Główny. Razor* &ndash; ładuje się w katalogu głównym aplikacji i zawiera link do `About` składnika (`href="About"`).
* *Informacje o* &ndash; składniku Razor `About` .

Gdy zażądano dokumentu domyślnego aplikacji przy użyciu paska adresu przeglądarki (na przykład `https://www.contoso.com/`):

1. Przeglądarka wykonuje żądanie.
1. Zostanie zwrócona strona domyślna, która jest zwykle *index. html*.
1. *index. html* Bootstrap aplikację.
1. Ładowanie routera Blazor, a składnik Razor `Main` jest renderowany.

Na stronie głównej wybranie `About` linku do składnika działa na kliencie, ponieważ router Blazor zatrzyma w przeglądarce żądanie połączenia z `www.contoso.com` Internetem `About` i obsługuje wyrenderowany `About` składnik. Wszystkie żądania dotyczące wewnętrznych punktów końcowych *w aplikacji Blazor webassembly* działają w taki sam sposób: Żądania nie wyzwalają żądań przeglądarki do zasobów hostowanych przez serwer w Internecie. Router obsługuje wewnętrznie żądania.

Żądanie kończy się niepowodzeniem `www.contoso.com/About`, jeśli żądanie zostanie wykonane przy użyciu paska adresu przeglądarki. Ten zasób nie istnieje na hoście internetowym aplikacji, więc zwracana jest odpowiedź *404 — nie znaleziono* .

Ponieważ przeglądarki wysyłają żądania do hostów internetowych dla stron po stronie klienta, serwery sieci Web i usługi hostingu muszą ponownie zapisywać wszystkie żądania dotyczące zasobów, które nie znajdują się fizycznie na serwerze, na stronie *index. html* . Po zwróceniu elementu *index. html* router Blazor aplikacji przejmuje i reaguje na prawidłowy zasób.

Podczas wdrażania na serwerze IIS można użyć modułu ponownego zapisywania adresu URL z opublikowanym plikiem *Web. config* aplikacji. Aby uzyskać więcej informacji, zobacz sekcję [usług IIS](#iis) .

## <a name="hosted-deployment-with-aspnet-core"></a>Hostowane wdrożenie z ASP.NET Core

*Wdrożenie hostowane* służy do obsługi aplikacji Blazor webassembly w przeglądarkach z poziomu [aplikacji ASP.NET Core](xref:index) działającej na serwerze sieci Web.

Aplikacja Blazor jest dołączana do aplikacji ASP.NET Core w publikowanym danych wyjściowych, dzięki czemu dwie aplikacje są wdrażane razem. Wymagany jest serwer sieci Web, który umożliwia hostowanie aplikacji ASP.NET Core. W przypadku wdrożenia hostowanego program Visual Studio zawiera szablon projektu **aplikacji webassembly Blazor** (`blazorwasm` szablon używany przez polecenie [dotnet New](/dotnet/core/tools/dotnet-new) ) z wybraną opcją **hostowaną** .

Aby uzyskać więcej informacji na temat ASP.NET Core hostingu i wdrażania aplikacji <xref:host-and-deploy/index>, zobacz.

Aby uzyskać informacje na temat wdrażania do Azure App Service <xref:tutorials/publish-to-azure-webapp-using-vs>, zobacz.

## <a name="standalone-deployment"></a>Wdrożenie autonomiczne

*Wdrożenie autonomiczne* obsługuje aplikację webassembly Blazor jako zestaw plików statycznych, które są żądane bezpośrednio przez klientów. Każdy statyczny serwer plików jest w stanie obsłużyć aplikację Blazor.

Zasoby wdrażania autonomicznego są publikowane w folderze *bin/Release/{Target Framework}/Publish/{Assembly Name}/dist* .

### <a name="iis"></a>IIS

Usługi IIS to obsługujący statyczny serwer plików dla aplikacji Blazor. Aby skonfigurować usługi IIS do hostowania Blazor, zobacz [Tworzenie statycznej witryny sieci Web w usługach IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).

Opublikowane zasoby są tworzone w folderze */bin/Release/{Target Framework}/Publish* . Hostowanie zawartości folderu *publikowania* na serwerze sieci Web lub w usłudze hostingu.

#### <a name="webconfig"></a>web.config

Po opublikowaniu projektu Blazor zostanie utworzony plik *Web. config* z następującą konfiguracją usług IIS:

* Typy MIME są ustawiane dla następujących rozszerzeń plików:
  * *.dll* &ndash; `application/octet-stream`
  * *.json* &ndash; `application/json`
  * *.wasm* &ndash; `application/wasm`
  * *. WOFF* &ndash;`application/font-woff`
  * *. woff2* &ndash;`application/font-woff`
* Kompresja HTTP jest włączona dla następujących typów MIME:
  * `application/octet-stream`
  * `application/wasm`
* Reguły modułu ponownego zapisywania adresu URL zostały ustanowione:
  * Obsługuj podkatalog, w którym znajdują się zasoby statyczne aplikacji ( *{Assembly Name}/dist/{Path*).
  * Utwórz Routing awaryjny SPA, aby żądania dotyczące zasobów nienależących do pliku zostały przekierowane do domyślnego dokumentu aplikacji w folderze zasobów statycznych ( *{Assembly Name}/dist/index.html*).

#### <a name="install-the-url-rewrite-module"></a>Zainstaluj moduł ponownego zapisywania adresów URL

[Moduł ponownego zapisywania adresu URL](https://www.iis.net/downloads/microsoft/url-rewrite) jest wymagany do ponownego zapisywania adresów URL. Moduł nie jest instalowany domyślnie i nie jest dostępny do zainstalowania jako funkcja usługi roli Serwer sieci Web (IIS). Moduł musi zostać pobrany z witryny sieci Web usług IIS. Zainstaluj moduł przy użyciu Instalatora platformy sieci Web:

1. Lokalnie przejdź do [strony pobierania modułu ponowne zapisywanie adresów URL](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads). W przypadku wersji angielskiej wybierz pozycję **Instalatora WebPI** , aby pobrać Instalatora Instalatora WebPI. W przypadku innych języków wybierz odpowiednią architekturę dla serwera (x86/x64), aby pobrać Instalatora.
1. Skopiuj Instalatora na serwer. Uruchom Instalatora. Wybierz przycisk **Zainstaluj** i zaakceptuj postanowienia licencyjne. Po zakończeniu instalacji nie jest wymagane ponowne uruchomienie serwera.

#### <a name="configure-the-website"></a>Skonfiguruj witrynę sieci Web

Ustaw **ścieżkę fizyczną** witryny sieci Web do folderu aplikacji. Folder zawiera:

* Plik *Web. config* , za pomocą którego usługi IIS konfigurują witrynę sieci Web, w tym wymagane reguły przekierowań i typy zawartości plików.
* Folder elementu zawartości statycznej aplikacji.

#### <a name="host-as-an-iis-sub-app"></a>Host jako podrzędną aplikację usług IIS

Jeśli aplikacja autonomiczna jest hostowana jako podaplikacja usług IIS, wykonaj jedną z następujących czynności:

* Wyłącz procedurę obsługi ASP.NET Core dziedziczonego modułu.

  Usuń program obsługi w opublikowanym pliku *Web. config* aplikacji Blazor przez dodanie `<handlers>` sekcji do pliku:

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* Wyłącz dziedziczenie `<system.webServer>` sekcji głównej (nadrzędnej) aplikacji `<location>` przy użyciu elementu z ustawioną `false`opcją `inheritInChildApplications` :

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <location path="." inheritInChildApplications="false">
      <system.webServer>
        <handlers>
          <add name="aspNetCore" ... />
        </handlers>
        <aspNetCore ... />
      </system.webServer>
    </location>
  </configuration>
  ```

Usuwanie procedury obsługi lub wyłączanie dziedziczenia jest wykonywane poza [konfiguracją ścieżki podstawowej aplikacji](xref:host-and-deploy/blazor/index#app-base-path). Ustaw ścieżkę bazową aplikacji w pliku *index. html* aplikacji na alias IIS używany podczas konfigurowania aplikacji podrzędnej w usługach IIS.

#### <a name="troubleshooting"></a>Rozwiązywanie problemów

W przypadku odebrania *500 — wewnętrzny błąd serwera* , a Menedżer usług IIS zgłasza błędy przy próbie uzyskania dostępu do konfiguracji witryny sieci Web, upewnij się, że zainstalowano moduł ponownego zapisywania adresu URL. Gdy moduł nie jest zainstalowany, nie można przeanalizować pliku *Web. config* przez usługi IIS. Zapobiega to załadowaniu przez Menedżera usług IIS konfiguracji witryny sieci Web i witryny sieci Web do obsługi plików statycznych Blazor.

Aby uzyskać więcej informacji na temat rozwiązywania problemów z <xref:test/troubleshoot-azure-iis>WDROŻENIAMI w usługach IIS, zobacz.

### <a name="azure-storage"></a>Azure Storage

Hosting pliku statycznego [usługi Azure Storage](/azure/storage/) umożliwia hosting aplikacji bezserwerowych Blazor. Obsługiwane są niestandardowe nazwy domen, usługa Azure Content Delivery Network (CDN) i protokół HTTPS.

Gdy usługa BLOB jest włączona dla hostingu statycznej witryny sieci Web na koncie magazynu:

* Ustaw **nazwę dokumentu indeksu** na `index.html`.
* Ustaw `index.html` **ścieżkę do dokumentu błędu** . Składniki Razor i inne punkty końcowe inne niż pliki nie znajdują się w ścieżkach fizycznych w zawartości statycznej przechowywanej przez usługę BLOB. Po otrzymaniu żądania dla jednego z tych zasobów, który powinien zostać obsłużony przez router Blazor, błąd *404-nie znaleziono* przez usługę BLOB Service kieruje żądanie do **ścieżki dokumentu błędu**. Zwracany jest obiekt BLOB *index. html* , a router Blazor ładuje i przetwarza ścieżkę.

Aby uzyskać więcej informacji, zobacz [Obsługa statycznej witryny sieci Web w usłudze Azure Storage](/azure/storage/blobs/storage-blob-static-website).

### <a name="nginx"></a>Nginx

Następujący plik *Nginx. conf* został uproszczony, aby pokazać, jak skonfigurować Nginx do wysyłania pliku *index. html* za każdym razem, gdy nie można znaleźć odpowiedniego pliku na dysku.

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /index.html =404;
        }
    }
}
```

Aby uzyskać więcej informacji na temat konfiguracji serwera sieci Web w środowisku produkcyjnym, zobacz [Tworzenie plików konfiguracji Nginx Plus i Nginx](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).

### <a name="nginx-in-docker"></a>Nginx w Docker

Aby hostować Blazor w platformie Docker przy użyciu Nginx, skonfiguruj pliku dockerfile do korzystania z obrazu Nginx opartego na Alpine. Zaktualizuj pliku dockerfile, aby skopiować plik *Nginx. config* do kontenera.

Dodaj jeden wiersz do pliku dockerfile, jak pokazano w następującym przykładzie:

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="github-pages"></a>Strony serwisu GitHub

Aby obsłużyć ponowne zapisywanie adresów URL, Dodaj plik *404. html* ze skryptem, który obsługuje przekierowywanie żądania do strony *index. html* . Aby zapoznać się z przykładową implementacją dostarczoną przez społeczność, zobacz [aplikacje jednostronicowe dla stron usługi GitHub](https://spa-github-pages.rafrex.com/) ([rafrex/Spa-GitHub-Pages w witrynie GitHub](https://github.com/rafrex/spa-github-pages#readme)). Przykład użycia podejścia społecznościowego można znaleźć w witrynie GitHub[(](https://blazor-demo.github.io/) [blazor — Demonstracja/blazor-Demonstracja](https://github.com/blazor-demo/blazor-demo.github.io) ).

W przypadku korzystania z witryny projektu zamiast witryny organizacji Dodaj lub zaktualizuj `<base>` tag w *pliku index. html*. Ustaw wartość `my-repository/`atrybutu na nazwę repozytorium GitHub z końcowym ukośnikiem (na przykład. `href`

## <a name="host-configuration-values"></a>Wartości konfiguracji hosta

[Blazor aplikacje webassembly](xref:blazor/hosting-models#blazor-webassembly) mogą akceptować następujące wartości konfiguracji hosta jako argumenty wiersza polecenia w czasie wykonywania w środowisku programistycznym.

### <a name="content-root"></a>Katalog główny zawartości

`--contentroot` Argument ustawia ścieżkę bezwzględną do katalogu, który zawiera pliki zawartości aplikacji. W poniższych przykładach `/content-root-path` jest ścieżką katalogu głównego zawartości aplikacji.

* Przekaż argument podczas lokalnego uruchamiania aplikacji w wierszu polecenia. W katalogu aplikacji wykonaj następujące polecenie:

  ```dotnetcli
  dotnet run --contentroot=/content-root-path
  ```

* Dodaj wpis do pliku *profilu launchsettings. JSON* aplikacji w profilu **IIS Express** . To ustawienie jest używane, gdy aplikacja jest uruchamiana z debugerem programu Visual Studio i z wiersza polecenia `dotnet run`z.

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* W programie Visual Studio Określ argument w **właściwościach** > **Debuguj** > **argumenty aplikacji**. Ustawienie argumentu na stronie właściwości programu Visual Studio powoduje dodanie argumentu do pliku *profilu launchsettings. JSON* .

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a>Baza ścieżki

Argument ustawia ścieżkę bazową aplikacji dla aplikacji uruchamianej lokalnie z niegłówną względną ścieżką URL `<base>` (tag `href` jest ustawiony na ścieżkę inną niż `/` w przypadku przemieszczania i produkcji). `--pathbase` W poniższych przykładach `/relative-URL-path` jest podstawą ścieżki aplikacji. Aby uzyskać więcej informacji, zobacz [Ścieżka podstawowa aplikacji](xref:host-and-deploy/blazor/index#app-base-path).

> [!IMPORTANT]
> W przeciwieństwie `href` do ścieżki przekazanej do `<base>` tagu, nie dodawaj końcowego ukośnika `--pathbase` (`/`) podczas przekazywania wartości argumentu. Jeśli ścieżka podstawowa aplikacji jest podana w `<base>` tagu jako `<base href="/CoolApp/">` (zawiera końcowy ukośnik), należy przekazać wartość argumentu wiersza polecenia jako `--pathbase=/CoolApp` (bez ukośnika na końcu).

* Przekaż argument podczas lokalnego uruchamiania aplikacji w wierszu polecenia. W katalogu aplikacji wykonaj następujące polecenie:

  ```dotnetcli
  dotnet run --pathbase=/relative-URL-path
  ```

* Dodaj wpis do pliku *profilu launchsettings. JSON* aplikacji w profilu **IIS Express** . To ustawienie jest używane podczas uruchamiania aplikacji za pomocą debugera programu Visual Studio i z wiersza polecenia w programie `dotnet run`.

  ```json
  "commandLineArgs": "--pathbase=/relative-URL-path"
  ```

* W programie Visual Studio Określ argument w **właściwościach** > **Debuguj** > **argumenty aplikacji**. Ustawienie argumentu na stronie właściwości programu Visual Studio powoduje dodanie argumentu do pliku *profilu launchsettings. JSON* .

  ```console
  --pathbase=/relative-URL-path
  ```

### <a name="urls"></a>adresy URL

`--urls` Argument ustawia adresy IP lub adresy hosta z portami i protokołami, aby nasłuchiwać żądań.

* Przekaż argument podczas lokalnego uruchamiania aplikacji w wierszu polecenia. W katalogu aplikacji wykonaj następujące polecenie:

  ```dotnetcli
  dotnet run --urls=http://127.0.0.1:0
  ```

* Dodaj wpis do pliku *profilu launchsettings. JSON* aplikacji w profilu **IIS Express** . To ustawienie jest używane podczas uruchamiania aplikacji za pomocą debugera programu Visual Studio i z wiersza polecenia w programie `dotnet run`.

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* W programie Visual Studio Określ argument w **właściwościach** > **Debuguj** > **argumenty aplikacji**. Ustawienie argumentu na stronie właściwości programu Visual Studio powoduje dodanie argumentu do pliku *profilu launchsettings. JSON* .

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="configure-the-linker"></a>Konfigurowanie konsolidatora

Blazor wykonuje konsolidację języka pośredniego (IL) dla każdej kompilacji, aby usunąć niepotrzebny kod IL z zestawów wyjściowych. Łączenie zestawu może być kontrolowane podczas kompilacji. Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/blazor/configure-linker>.
