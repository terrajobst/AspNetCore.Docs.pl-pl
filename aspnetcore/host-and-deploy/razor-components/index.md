---
title: Hostowanie i wdrażanie składników Razor
author: guardrex
description: 'Dowiedz się, jak hostowanie i wdrażanie składników Razor i Blazor aplikacje przy użyciu platformy ASP.NET Core, sieci dostarczania zawartości (CDN), serwery plików i stron w witrynie GitHub.'
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/22/2019
uid: host-and-deploy/razor-components/index
---
# <a name="host-and-deploy-razor-components"></a>Hostowanie i wdrażanie składników Razor

Przez [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), i [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

## <a name="publish-the-app"></a>Publikowanie aplikacji

Aplikacje są publikowane do wdrożenia w konfiguracji wydania przy użyciu [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenia. Środowisko IDE może obsługiwać wykonywanie `dotnet publish` polecenia automatycznie przy użyciu jego wbudowanych funkcji publikowania, dzięki czemu może nie być konieczne ręczne wykonaj polecenie w wierszu polecenia, w zależności od poziomu narzędzi deweloperskich w użyciu.

```console
dotnet publish -c Release
```

`dotnet publish` Wyzwalacze [przywrócić](/dotnet/core/tools/dotnet-restore) zależności projektu i [kompilacje](/dotnet/core/tools/dotnet-build) projekcie przed utworzeniem zasobów dla wdrożenia. Jako część procesu kompilacji Aby zmniejszyć rozmiar pobierania aplikacji i czasów ładowania są usuwane nieużywane metod i zestawów. Wdrożenie jest tworzony w */bin/wersji/&lt;platformę docelową&gt;/ publish* folderu.

Zasoby w *publikowania* folderu wdrożonych na serwerze sieci web. Wdrożenie może być ręcznego lub zautomatyzowanego procesu w zależności od poziomu narzędzi deweloperskich w użyciu.

## <a name="host-configuration-values"></a>Wartości konfiguracji hosta

Razor składniki aplikacje, które używają [po stronie serwera modelu hostingu](xref:razor-components/hosting-models#server-side-hosting-model) może akceptować [wartości konfiguracji hosta sieci Web](xref:fundamentals/host/web-host#host-configuration-values).

Blazor aplikacje, które używają [modelu hostingu po stronie klienta](xref:razor-components/hosting-models#client-side-hosting-model) może akceptować następujące wartości konfiguracji hosta jako argumenty wiersza polecenia w czasie wykonywania w środowisku programistycznym.

### <a name="content-root"></a>Zawartość katalogu głównego

`--contentroot` Argument ustawia ścieżkę bezwzględną do katalogu, który zawiera pliki zawartości aplikacji.

* Przekazać argument podczas uruchamiania aplikacji lokalnie w wierszu polecenia. W katalogu aplikacji wykonaj polecenie:

  ```console
  dotnet run --contentroot=/<content-root>
  ```

* Dodawanie wpisu do aplikacji *launchSettings.json* w pliku **usług IIS Express** profilu. To ustawienie jest pobierane podczas uruchamiania aplikacji przy użyciu debugera programu Visual Studio, a podczas uruchamiania aplikacji z poziomu wiersza polecenia przy użyciu `dotnet run`.

  ```json
  "commandLineArgs": "--contentroot=/<content-root>"
  ```

* W programie Visual Studio, należy określić argument w **właściwości** > **debugowania** > **argumenty aplikacji**. Ustawienie argumentu na stronie właściwości programu Visual Studio dodaje argument *launchSettings.json* pliku.

  ```console
  --contentroot=/<content-root>
  ```

### <a name="path-base"></a>Podstawa ścieżki

`--pathbase` Argument określa podstawową ścieżkę aplikacji dla aplikacji, które działają lokalnie z innego niż główny ścieżki wirtualnej ( `<base>` tag `href` ustawiono ścieżki innego niż `/` dla środowisk przejściowych i produkcyjnych). Aby uzyskać więcej informacji, zobacz [ścieżki podstawowej aplikacji](#app-base-path) sekcji.

> [!IMPORTANT]
> W odróżnieniu od podana ścieżka do `href` z `<base>` tag, nie dołączaj końcowy ukośnik (`/`) podczas przekazywania `--pathbase` wartość argumentu. Jeśli ścieżka podstawowa aplikacja znajduje się w `<base>` otaguj jako `<base href="/CoolApp/" />` (w tym znakiem ukośnika), Przekaż wartość argumentu wiersza polecenia jako `--pathbase=/CoolApp` (nie ukośnikiem końcowym).

* Przekazać argument podczas uruchamiania aplikacji lokalnie w wierszu polecenia. W katalogu aplikacji wykonaj polecenie:

  ```console
  dotnet run --pathbase=/<virtual-path>
  ```

* Dodawanie wpisu do aplikacji *launchSettings.json* w pliku **usług IIS Express** profilu. To ustawienie jest pobierane podczas uruchamiania aplikacji przy użyciu debugera programu Visual Studio, a podczas uruchamiania aplikacji z poziomu wiersza polecenia przy użyciu `dotnet run`.

  ```json
  "commandLineArgs": "--pathbase=/<virtual-path>"
  ```

* W programie Visual Studio, należy określić argument w **właściwości** > **debugowania** > **argumenty aplikacji**. Ustawienie argumentu na stronie właściwości programu Visual Studio dodaje argument *launchSettings.json* pliku.

  ```console
  --pathbase=/<virtual-path>
  ```

### <a name="urls"></a>adresy URL

`--urls` Argument wskazuje adresy IP lub adresy hostów, porty i protokoły do nasłuchiwania żądań.

* Przekazać argument podczas uruchamiania aplikacji lokalnie w wierszu polecenia. W katalogu aplikacji wykonaj polecenie:

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* Dodawanie wpisu do aplikacji *launchSettings.json* w pliku **usług IIS Express** profilu. To ustawienie jest pobierane podczas uruchamiania aplikacji przy użyciu debugera programu Visual Studio, a podczas uruchamiania aplikacji z poziomu wiersza polecenia przy użyciu `dotnet run`.

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* W programie Visual Studio, należy określić argument w **właściwości** > **debugowania** > **argumenty aplikacji**. Ustawienie argumentu na stronie właściwości programu Visual Studio dodaje argument *launchSettings.json* pliku.

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deploy-a-client-side-blazor-app"></a>Wdrażanie aplikacji Blazor po stronie klienta

Za pomocą [modelu hostingu po stronie klienta](xref:razor-components/hosting-models#client-side-hosting-model):

* Aplikacja Blazor, jego zależności i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.
* Aplikacja jest wykonywane bezpośrednio w przeglądarce wątku interfejsu użytkownika. Obsługiwany jest jedną z następujących strategii:
  * Aplikacja Blazor jest obsługiwany przez aplikację ASP.NET Core. Objęte [Blazor po stronie klienta hostowane wdrażanie za pomocą platformy ASP.NET Core](#client-side-blazor-hosted-deployment-with-aspnet-core) sekcji.
  * Aplikacji Blazor jest umieszczany na statyczne hostingu serwera sieci web lub usługi, której platformy .NET nie jest używany do obsługi aplikacji Blazor. Objęte [wdrażania autonomicznego Blazor po stronie klienta](#client-side-blazor-standalone-deployment) sekcji.

### <a name="configure-the-linker"></a>Konfigurowanie konsolidatora

Blazor wykonuje języka pośredniego (IL) łączenia każdego kompilacji, aby usunąć niepotrzebne IL z zestawów danych wyjściowych. Można kontrolować łączenie podczas kompilowania zestawu. Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/razor-components/configure-linker>.

### <a name="rewrite-urls-for-correct-routing"></a>Ponowne zapisywanie adresów URL poprawne routingu

Routing żądań dla elementów strony w aplikacji po stronie klienta nie jest tak proste, jak kierowania żądań do aplikacji po stronie serwera, hostowaną. Należy wziąć pod uwagę aplikacji po stronie klienta, zawierający dwie strony:

* **_Main.cshtml_**  &ndash; obciążeń w katalogu głównym aplikacji i zawiera łącza do strony informacje (`href="About"`).
* **_About.cshtml_**  &ndash; o stronie.

Gdy dokument domyślny aplikacji przy użyciu paska adresu w przeglądarce (na przykład `https://www.contoso.com/`):

1. Żąda przeglądarki.
1. Domyślna strona ma zostać zwrócona, co jest zazwyczaj *index.html*.
1. *index.HTML* używa do ładowania aplikacji.
1. Router firmy Blazor obciążeń i strony Razor Main (*Main.cshtml*) jest wyświetlana.

Na stronie głównej wybierając link do strony informacje ładuje stronę informacje. Wybierając link do strony informacje działa na komputerze klienckim, ponieważ Blazor router zatrzymuje przeglądarki z żądania w Internecie, aby `www.contoso.com` dla `About` i służy sama strona informacje. Wszystkie żądania dla wewnętrznej stron *w aplikacji po stronie klienta* działają tak samo: Żądania nie wyzwalacza opartego na przeglądarce żądania hostowany serwer zasobów w Internecie. Wewnętrznie obsługuje żądania przez router.

Jeśli żądanie zostało nawiązane za pomocą paska adresu w przeglądarce dla `www.contoso.com/About`, żądanie kończy się niepowodzeniem. Żaden z tych zasobów istnieje na hoście Internet aplikacji, więc *404 Nie znaleziono* zwróceniem odpowiedzi.

Ponieważ przeglądarki wysyłać żądania do internetowego hostów dla stron po stronie klienta, serwery sieci web i usług hostingu należy przepisać wszystkie żądania dotyczące zasobów bez fizycznego na serwerze, aby *index.html* strony. Gdy *index.html* jest zwracany przez aplikację klienta routera przejmuje i odpowiada za pomocą odpowiedniego zasobu.

### <a name="app-base-path"></a>Podstawowa ścieżka aplikacji

Ścieżka podstawowa aplikacja jest ścieżka katalogu głównego aplikacji wirtualnej na serwerze. Na przykład aplikację, która znajduje się na serwerze Contoso folder wirtualny o `/CoolApp/` zostanie osiągnięty w `https://www.contoso.com/CoolApp` i ma wirtualnej ścieżki podstawowej z `/CoolApp/`. Ustawiając ścieżki podstawowej aplikacji `CoolApp/`, aplikacja jest powiadamianym o którym praktycznie znajduje się na serwerze. Aplikacja może używać ścieżki podstawowej aplikacji do tworzenia adresów URL, względem katalogu głównego aplikacji ze składnika, który nie znajduje się w katalogu głównym. Dzięki temu składniki, które istnieją na różnych poziomach struktury katalogów, tworzenie łączy do innych zasobów w lokalizacjach w całej aplikacji. Ścieżka podstawowa aplikacja jest również używane do przechwycenia hiperłącze kliknie gdzie `href` miejsce docelowe łącza jest w ramach ścieżki podstawowej aplikacji identyfikator URI przestrzeni&mdash;routera Blazor obsługuje wewnętrzny nawigacji.

W wielu scenariuszach hostingu serwer ścieżka wirtualna do aplikacji jest głównym aplikacji. W takich przypadkach ścieżki podstawowej aplikacji jest ukośnikiem (`<base href="/" />`), która jest domyślna konfiguracja dla aplikacji. W innych scenariuszach hostingu, takich jak katalogi wirtualne stronach GitHub oraz usług IIS lub aplikacje podrzędne ścieżki podstawowej aplikacji należy określić ścieżkę wirtualną serwera aplikacji. Aby ustawić ścieżki podstawowej aplikacji, dodać lub zaktualizować `<base>` tagów w *index.html* można znaleźć w `<head>` tagów elementów. Ustaw `href` wartość do atrybutu `<virtual-path>/` (wymagane jest podanie końcowy ukośnik), gdzie `<virtual-path>/` to ścieżka katalogu głównego aplikacji pełna wirtualna na serwerze aplikacji. W powyższym przykładzie, ścieżka wirtualna jest ustawiona na `CoolApp/`: `<base href="CoolApp/" />`.

Dla aplikacji za pomocą innego niż główny ścieżki wirtualnej skonfigurowane (na przykład `<base href="CoolApp/" />`), aplikacja nie może odnaleźć swoich zasobów *uruchomienia lokalnie*. Aby rozwiązać ten problem, podczas lokalnego opracowywania i testowania, możesz podać *podstawa ścieżki* argument, który odpowiada `href` wartość `<base>` tagu w czasie wykonywania.

Aby przekazać argument podstawowa ścieżka o ścieżce katalogu głównego (`/`) podczas uruchamiania aplikacji lokalnie, uruchom następujące polecenie z katalogu aplikacji:

```console
dotnet run --pathbase=/CoolApp
```

Aplikacja reaguje lokalnie na `http://localhost:port/CoolApp`.

Aby uzyskać więcej informacji, zobacz [wartość konfiguracji podstawowej hosta ścieżki](#path-base) sekcji.

> [!IMPORTANT]
> Jeśli aplikacja używa [modelu hostingu po stronie klienta](xref:razor-components/hosting-models#client-side-hosting-model) (na podstawie **Blazor** szablon projektu) i jest hostowany jako aplikacja podrzędnych usług IIS w aplikacji ASP.NET Core, ważne jest, aby wyłączyć dziedziczone platformy ASP.NET Core Obsługi modułu lub upewnić się, że aplikacja głównego (nadrzędnego) `<handlers>` sekcji *web.config* pliku nie jest dziedziczona przez jej podrzędnych.
>
> Usuń procedurę obsługi w aplikacji — opublikowane *web.config* pliku, dodając `<handlers>` sekcji w pliku:
>
> ```xml
> <handlers>
>   <remove name="aspNetCore" />
> </handlers>
> ```
>
> Alternatywnie wyłączyć funkcję dziedziczenia aplikacji głównej (nadrzędnej) `<system.webServer>` sekcji przy użyciu `<location>` element z `inheritInChildApplications` równa `false`:
>
> ```xml
> <?xml version="1.0" encoding="utf-8"?>
> <configuration>
>  <location path="." inheritInChildApplications="false">
>    <system.webServer>
>      <handlers>
>        <add name="aspNetCore" ... />
>      </handlers>
>      <aspNetCore ... />
>    </system.webServer>
>  </location>
> </configuration>
> ```
>
> Usuwanie obsługi lub wyłączając dziedziczenie jest wykonywane Oprócz konfigurowania ścieżki podstawowej aplikacji, zgodnie z opisem w tej sekcji. Ustawianie ścieżki podstawowej aplikacji w aplikacji *index.html* pliku do aliasu usług IIS, używane podczas konfigurowania aplikacji podrzędnej w usługach IIS.

### <a name="client-side-blazor-hosted-deployment-with-aspnet-core"></a>Wdrażanie Blazor hostowanej po stronie klienta za pomocą platformy ASP.NET Core

A *hostowanych wdrożenia* służy aplikacja Blazor po stronie klienta do przeglądarki z [aplikacji ASP.NET Core](xref:index) , które jest uruchamiane na serwerze.

Aplikacja Blazor jest dołączone do aplikacji platformy ASP.NET Core w opublikowanych danych wyjściowych, więc, że dwie aplikacje wdrażane razem. Wymagany jest serwer sieci web, który jest zdolny do obsługi aplikacji ASP.NET Core. W przypadku wdrożenia hostowanego obejmuje program Visual Studio **Blazor (platformy ASP.NET Core, obsługiwane)** szablonu projektu (`blazorhosted` szablon, korzystając z [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia).

Aby uzyskać więcej informacji na temat hosting aplikacji platformy ASP.NET Core i wdrażanie, zobacz <xref:host-and-deploy/index>.

Aby uzyskać informacje na temat wdrażania w usłudze Azure App Service zobacz następujące tematy:

<xref:tutorials/publish-to-azure-webapp-using-vs>

Dowiedz się, jak opublikować aplikację ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio.

### <a name="client-side-blazor-standalone-deployment"></a>Wdrażania autonomicznego Blazor po stronie klienta

A *wdrażania autonomicznego* służy aplikacja Blazor po stronie klienta jako zbiór plików statycznych, które są żądane bezpośrednio przez klientów. Serwer sieci web nie jest używany do obsługi aplikacji Blazor.

#### <a name="client-side-blazor-standalone-hosting-with-iis"></a>Po stronie klienta Blazor autonomiczne hostowanie za pomocą programu IIS

Program IIS jest serwerem plików statycznych z możliwością Blazor aplikacji. Aby skonfigurować usługi IIS do hosta Blazor, zobacz [Tworzenie statycznej witryny sieci Web w usługach IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).

Opublikowane zasoby są tworzone w  *\\bin\\wersji\\&lt;platformę docelową&gt;\\publikowania* folderu. Hostowanie zawartości *publikowania* folderu na serwerze sieci web lub innej usługi hostingu.

**web.config**

Po opublikowaniu projektu Blazor *web.config* plik jest tworzony przy użyciu następującej konfiguracji usług IIS:

* Typy MIME są ustawiane dla następujących rozszerzeń pliku:
  * *\*.dll*: `application/octet-stream`
  * *\*.json*: `application/json`
  * *\*.wasm*: `application/wasm`
  * *\*.woff*: `application/font-woff`
  * *\*.woff2*: `application/font-woff`
* Kompresja protokołu HTTP jest włączone dla następujących typów MIME:
  * `application/octet-stream`
  * `application/wasm`
* Ustanawiane są reguły moduł ponowne zapisywanie adresów URL:
  * Obsługiwać podkatalogu, gdzie znajdują się zasoby statyczne aplikacji (*&lt;assembly_name&gt;\\dist\\&lt;path_requested&gt;*).
  * Utwórz SPA rezerwowego routingu, tak aby żądania innego niż plik zasobów są przekierowywane do aplikacji dokumentu domyślnego w folderze statycznych zasobów (*&lt;assembly_name&gt;\\dist\\index.html*).

**Zainstaluj moduł ponowne zapisywanie adresów URL**

[Moduł ponowne zapisywanie adresów URL](https://www.iis.net/downloads/microsoft/url-rewrite) jest wymagany do ponownego zapisywania adresów URL. Moduł nie jest instalowany domyślnie i nie jest dostępna do zainstalowania jako funkcja usługi roli Serwer sieci Web (IIS). Moduł musi zostać pobrany z witryny sieci Web usług IIS. Aby zainstalować moduł, należy użyć Instalatora platformy sieci Web:

1. Lokalnie, przejdź do [moduł ponowne zapisywanie adresów URL strony pobierania](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads). W wersji angielskiej, wybierz **WebPI** do pobierania Instalatora WebPI. W przypadku języków wybierz odpowiednią architekturę dla serwera — x86/x64 64 do pobierania Instalatora.
1. Skopiuj Instalatora na serwerze. Uruchom Instalatora. Wybierz **zainstalować** przycisk i zaakceptuj postanowienia licencyjne. Ponowne uruchomienie serwera nie jest wymagane po ukończeniu instalacji.

**Konfigurowanie witryny sieci Web**

Ustaw witrynę sieci Web **ścieżkę fizyczną** do folderu aplikacji. Folder zawiera:

* *Web.config* pliku wykorzystywanym przez usługi IIS do konfigurowania witryny sieci Web, łącznie z regułami wymagane przekierowania i typy zawartości plików.
* Folder statycznych zasobów aplikacji.

**Rozwiązywanie problemów**

Jeśli *500 Wewnętrzny błąd serwera* odebraniu i Menedżera usług IIS zgłasza błędy podczas próby dostępu do konfiguracji witryny sieci Web, upewnij się, że zainstalowano moduł ponowne zapisywanie adresów URL. Jeśli moduł nie jest zainstalowany, *web.config* nie można przeanalizować pliku przez usługi IIS. Zapobiega to ładowania konfiguracji witryny sieci Web i witryny sieci Web z dostarczania plików statycznych dla Blazor Menedżera usług IIS.

Aby uzyskać więcej informacji na temat Rozwiązywanie problemów z wdrożeniami usług IIS, zobacz <xref:host-and-deploy/iis/troubleshoot>.

#### <a name="client-side-blazor-standalone-hosting-with-nginx"></a>Po stronie klienta Blazor autonomiczne hostowanie przy użyciu serwera Nginx

Następujące *nginx.conf* pliku została uproszczona w celu pokazują, jak skonfigurować serwer Nginx, aby wysłać *Index.html* plików zawsze wtedy, gdy nie można odnaleźć odpowiedniego pliku na dysku.

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /Index.html =404;
        }
    }
}
```

Aby uzyskać więcej informacji na temat konfiguracji serwera sieci web Nginx produkcji, zobacz [tworzenia serwer NGINX Plus i pliki konfiguracji NGINX](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).

#### <a name="client-side-blazor-standalone-hosting-with-nginx-in-docker"></a>Po stronie klienta Blazor autonomiczne hostowanie przy użyciu serwera Nginx na platformie Docker

Aby hostować Blazor na platformie Docker przy użyciu serwera Nginx, Konfiguracja pliku Dockerfile, aby użyć obrazu Alpine na podstawie serwera Nginx. Aktualizacja pliku Dockerfile, aby skopiować *nginx.config* pliku do kontenera.

Dodaj jeden wiersz do pliku Dockerfile, jak pokazano w poniższym przykładzie:

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

#### <a name="client-side-blazor-standalone-hosting-with-github-pages"></a>Po stronie klienta Blazor autonomiczne hostowanie strony GitHub

Aby obsłużyć ponownego adresu URL, Dodaj *404. html* pliku ze skryptem, który obsługuje żądanie przekierowania *index.html* strony. Przykładem implementacji dostarczane przez społeczność, można zobaczyć [pojedynczej aplikacji strony dla strony GitHub](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github strony w witrynie GitHub](https://github.com/rafrex/spa-github-pages#readme)). Przykładem dotyczącym używania podejścia społeczności są widoczne w [blazor-demo/blazor-demo.github.io w serwisie GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([aktywnej witryny](https://blazor-demo.github.io/)).

Korzystając z witryny projektu zamiast witryny organizacji, Dodaj lub zaktualizuj `<base>` tagów w *index.html*. Ustaw `href` wartość do atrybutu `<repository-name>/`, gdzie `<repository-name>/` jest nazwa repozytorium GitHub.

## <a name="deploy-a-server-side-razor-components-app"></a>Wdrażanie aplikacji Razor składniki po stronie serwera

Za pomocą [po stronie serwera modelu hostingu](xref:razor-components/hosting-models#server-side-hosting-model), składniki Razor jest wykonywane na serwerze z poziomu aplikacji ASP.NET Core. Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane za pośrednictwem połączenia SignalR.

Aplikacja jest dołączone do aplikacji platformy ASP.NET Core w opublikowanych danych wyjściowych, więc, że dwie aplikacje wdrażane razem. Wymagany jest serwer sieci web, który jest zdolny do obsługi aplikacji ASP.NET Core. Wdrożenia po stronie serwera, program Visual Studio obejmuje **składniki Razor** szablonu projektu (`razorcomponents` szablon, korzystając z [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia).

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

Po opublikowaniu aplikacji ASP.NET Core Razor składniki aplikacji znajduje w opublikowanych danych wyjściowych, dzięki czemu aplikacja platformy ASP.NET Core i składniki Razor aplikacji można wdrożyć ze sobą. Aby uzyskać więcej informacji na temat hosting aplikacji platformy ASP.NET Core i wdrażanie, zobacz <xref:host-and-deploy/index>.

Aby uzyskać informacje na temat wdrażania w usłudze Azure App Service zobacz następujące tematy:

<xref:tutorials/publish-to-azure-webapp-using-vs>

Dowiedz się, jak opublikować aplikację ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio.
