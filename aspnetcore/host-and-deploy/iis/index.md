---
title: Host platformy ASP.NET Core w systemie Windows z programem IIS
author: guardrex
description: Dowiedz się, jak udostępniać aplikacje platformy ASP.NET Core na systemu Windows Server Internet informacji usług (IIS).
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/index
ms.openlocfilehash: 6b2c3334798861ebdb14787205480422d7d536ea
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/24/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Host platformy ASP.NET Core w systemie Windows z programem IIS

Przez [Luke Latham](https://github.com/guardrex) i [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="supported-operating-systems"></a>Supported operating systems

Obsługiwane są następujące systemy operacyjne:

* Windows 7 lub nowszy
* Windows Server 2008 R2 lub nowszy

[Serwer HTTP.sys](xref:fundamentals/servers/httpsys) (wcześniej nazywanych [WebListener](xref:fundamentals/servers/weblistener)) nie działa w konfiguracji zwrotny serwer proxy z usługami IIS. Użyj [serwera Kestrel](xref:fundamentals/servers/kestrel).

## <a name="application-configuration"></a>Konfiguracja aplikacji

### <a name="enable-the-iisintegration-components"></a>Włącz składniki IISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Typowe *Program.cs* wywołania [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) aby rozpocząć konfigurowanie hosta. `CreateDefaultBuilder` Konfiguruje [Kestrel](xref:fundamentals/servers/kestrel) jako sieci web Integracja serwera i umożliwia usług IIS przez skonfigurowanie ścieżki podstawowej i port [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module):

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

Moduł platformy ASP.NET Core generuje portów dynamicznych do przypisania do procesu zaplecza. `UseIISIntegration` Metoda przejmuje portów dynamicznych i konfiguruje Kestrel do nasłuchiwania `http://localhost:{dynamicPort}/`. Przesłania inne konfiguracje adresu URL, takie jak wywołania `UseUrls` lub [API nasłuchiwania na Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration). W związku z tym wywołań `UseUrls` lub jego Kestrel `Listen` interfejsu API nie są wymagane, gdy za pomocą modułu. Jeśli `UseUrls` lub `Listen` jest nazywany wykrywa Kestrel na port określony podczas uruchamiania aplikacji bez usług IIS.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Obejmują zależności na [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) pakietu w zależnościach aplikacji. Używanie oprogramowania pośredniczącego integracji usług IIS przez dodanie [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) metodę rozszerzenie [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

Zarówno [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) i [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) są wymagane. Wywoływanie kodu `UseIISIntegration` nie ma wpływu na przenośność kodu. Jeśli aplikacja nie jest uruchamiana za usług IIS (na przykład aplikacja jest uruchamiana bezpośrednio na Kestrel), `UseIISIntegration` nie działa.

Moduł platformy ASP.NET Core generuje portów dynamicznych do przypisania do procesu zaplecza. `UseIISIntegration` Metoda przejmuje portów dynamicznych i konfiguruje Kestrel do nasłuchiwania `http://locahost:{dynamicPort}/`. Przesłania inne konfiguracje adresu URL, takie jak wywołania `UseUrls`. W związku z tym wywołaniu `UseUrls` nie jest wymagane, gdy za pomocą modułu. Jeśli `UseUrls` jest nazywany wykrywa Kestrel na port określony podczas uruchamiania aplikacji bez usług IIS.

Jeśli `UseUrls` jest wywoływana w aplikacji platformy ASP.NET Core 1.0, wywołać ją **przed** wywoływania `UseIISIntegration` tak, aby nie jest zastąpione przez moduł skonfigurowany port. To zamówienie wywołujący nie jest wymagane w przypadku platformy ASP.NET Core 1.1, ponieważ zastępuje ustawienia modułu `UseUrls`.

---

Aby uzyskać więcej informacji dotyczących obsługi, zobacz [hosta w ASP.NET Core](xref:fundamentals/host/index).

### <a name="iis-options"></a>Opcje usług IIS

Aby skonfigurować opcje usług IIS, obejmują konfigurację usługi [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) w [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices). W poniższym przykładzie przekazywania certyfikatów klientów do aplikacji, aby wypełnić `HttpContext.Connection.ClientCertificate` jest wyłączone:

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| Opcja                         | Domyślny | Ustawienie |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | Jeśli `true`, oprogramowanie pośredniczące integracji usług IIS ustawia `HttpContext.User` uwierzytelnione przez [uwierzytelniania systemu Windows](xref:security/authentication/windowsauth). Jeśli `false`, oprogramowanie pośredniczące udostępnia tylko tożsamości dla `HttpContext.User` i sprostać wymaganiom jawnie żądanie `AuthenticationScheme`. Należy włączyć uwierzytelnianie systemu Windows w usługach IIS dla `AutomaticAuthentication` funkcji. Aby uzyskać więcej informacji, zobacz [uwierzytelniania systemu Windows](xref:security/authentication/windowsauth) tematu. |
| `AuthenticationDisplayName`    | `null`  | Ustawia nazwę wyświetlaną pokazywana użytkownikom na stronach logowania. |
| `ForwardClientCertificate`     | `true`  | Jeśli `true` i `MS-ASPNETCORE-CLIENTCERT` nagłówek żądania jest obecny, `HttpContext.Connection.ClientCertificate` jest wypełnione. |

### <a name="proxy-server-and-load-balancer-scenarios"></a>Serwer proxy i scenariuszy usługi równoważenia obciążenia

IIS integracji oprogramowania pośredniczącego, który konfiguruje przekazywane oprogramowanie pośredniczące nagłówki i moduł platformy ASP.NET Core są skonfigurowane do przekazywania schemat (HTTP/HTTPS) oraz adres IP zdalnego, którego pochodzi żądanie. Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych serwerów proxy dodatkowe i moduły równoważenia obciążenia. Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).

### <a name="webconfig-file"></a>plik Web.config

*Web.config* konfiguruje pliku [platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module). Tworzenie, przekształcanie i publikowanie *web.config* jest obsługiwany przez zestaw SDK programu .NET Core sieci Web (`Microsoft.NET.Sdk.Web`). Zestaw SDK jest ustawiona na początku pliku projektu:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

Jeśli *web.config* plik nie znajduje się w projekcie, plik jest tworzony z prawidłowym *processPath* i *argumenty* skonfigurować [platformy ASP.NET Core Moduł](xref:fundamentals/servers/aspnet-core-module) i przenieść do [opublikowane dane wyjściowe](xref:host-and-deploy/directory-structure).

Jeśli *web.config* plik znajduje się w projekcie, plik jest przekształcana z prawidłowym *processPath* i *argumenty* Konfigurowanie modułu platformy ASP.NET Core i przeniesiony do opublikowane dane wyjściowe. Transformacja nie zmodyfikować ustawień konfiguracji usług IIS w pliku.

*Web.config* pliku może zapewnić dodatkowe ustawienia konfiguracji usług IIS, które kontrolują active modułów usług IIS. Aby uzyskać informacje na modułów usług IIS, które są w stanie przetwarzania żądań w aplikacjach ASP.NET Core, zobacz [moduły IIS](xref:host-and-deploy/iis/modules) tematu.

Aby zapobiec przekształcania zestawu SDK sieci Web *web.config* plików, użyj  **\<IsTransformWebConfigDisabled >** właściwość w pliku projektu:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Przy wyłączaniu zestawu SDK sieci Web z transformacji pliku *processPath* i *argumenty* należy ręcznie ustawić przez dewelopera. Aby uzyskać więcej informacji, zobacz [odwołania konfiguracji platformy ASP.NET Core modułu](xref:host-and-deploy/aspnet-core-module).

### <a name="webconfig-file-location"></a>Lokalizacja pliku Web.config

Aby można było utworzyć zwrotnego serwera proxy między usługami IIS a serwerem Kestrel *web.config* plik musi znajdować się w ścieżce zawartości katalogu głównego (zazwyczaj ścieżki podstawowej aplikacji) wdrożonej aplikacji. To jest tej samej lokalizacji co ścieżkę fizyczną witryny sieci Web do usług IIS. *Web.config* plik jest wymagany w katalogu głównym aplikacji, aby umożliwić publikowanie wielu aplikacji za pomocą narzędzia Web Deploy.

Poufne pliki istnieją na ścieżkę fizyczną aplikacji, takich jak  *\<zestawu >. runtimeconfig.json*,  *\<zestawu > .xml* (komentarze dokumentacji XML), a  *\<zestawu >. deps.json*. Gdy *web.config* plik istnieje i i zwykle uruchamiania witryny, usługi IIS nie obsługiwać te poufnych plików, jeśli są one wymagane. Jeśli *web.config* brakuje pliku, niepoprawnie o nazwie lub nie można skonfigurować lokacji podczas normalnego uruchamiania, usługi IIS mogą służyć poufnych plików publicznie.

***Web.config* pliku musi być obecny we wdrożeniu przez cały czas, o nazwie prawidłowo i możliwe jest skonfigurowanie lokacji w celu rozpoczęcia normalnego się. Nigdy nie usunąć *web.config* pliku z wdrożenia produkcyjnego.**

## <a name="iis-configuration"></a>Konfiguracja usług IIS

**Systemy operacyjne Windows Server**

Włącz **serwer sieci Web (IIS)** roli serwera i ustanowienia usług ról.

1. Użyj **Dodaj role i funkcje** kreatora z **Zarządzaj** menu lub link w **Menedżera serwera**. Na **ról serwera** kroku, pole wyboru dla **serwer sieci Web (IIS)**.

   ![W kroku role serwera wybierz wybrano roli Serwer sieci Web IIS.](index/_static/server-roles-ws2016.png)

1. Po **funkcje** kroku **usług ról** krok ładuje dla serwera sieci Web (IIS). Wybieranie usług ról usług IIS konieczne lub zaakceptuj domyślną rolę usług pod warunkiem.

   ![Domyślne usługi ról są zaznaczone w kroku wybierz rolę usług.](index/_static/role-services-ws2016.png)

   **Uwierzytelnianie systemu Windows (opcjonalnie)**  
   Aby włączyć uwierzytelnianie systemu Windows, rozwiń następujące węzły: **serwera sieci Web** > **zabezpieczeń**. Wybierz **uwierzytelniania systemu Windows** funkcji. Aby uzyskać więcej informacji, zobacz [uwierzytelniania systemu Windows \<windowsAuthentication >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) i [uwierzytelniania skonfigurować systemu Windows](xref:security/authentication/windowsauth).

   **Protokół WebSockets (opcjonalnie)**  
   Obiekty Websocket jest obsługiwana z platformy ASP.NET Core 1.1 lub nowszej. Aby włączyć protokół WebSockets, rozwiń następujące węzły: **serwera sieci Web** > **projektowanie aplikacji**. Wybierz **protokół WebSocket** funkcji. Aby uzyskać więcej informacji, zobacz [Websocket](xref:fundamentals/websockets).

1. Postępuj zgodnie z instrukcjami **potwierdzenie** krok w celu zainstalowania roli serwera sieci web i usług. Ponowne uruchomienie serwera i IIS nie jest wymagane po zainstalowaniu **serwer sieci Web (IIS)** roli.

**Systemy operacyjne Windows**

Włącz **Konsola zarządzania usługami IIS** i **usługi sieci World Wide Web**.

1. Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **wyłączyć funkcje systemu Windows na lub wyłącz** (po lewej stronie ekranu).

1. Otwórz **Internetowe usługi informacyjne** węzła. Otwórz **narzędzia zarządzania siecią Web** węzła.

1. Pole wyboru dla **Konsola zarządzania usługami IIS**.

1. Pole wyboru dla **usługi sieci World Wide Web**.

1. Zaakceptuj domyślne funkcje dla **usługi sieci World Wide Web** lub dostosować funkcje usług IIS.

   **Uwierzytelnianie systemu Windows (opcjonalnie)**  
   Aby włączyć uwierzytelnianie systemu Windows, rozwiń następujące węzły: **usługi sieci World Wide Web** > **zabezpieczeń**. Wybierz **uwierzytelniania systemu Windows** funkcji. Aby uzyskać więcej informacji, zobacz [uwierzytelniania systemu Windows \<windowsAuthentication >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) i [uwierzytelniania skonfigurować systemu Windows](xref:security/authentication/windowsauth).

   **Protokół WebSockets (opcjonalnie)**  
   Obiekty Websocket jest obsługiwana z platformy ASP.NET Core 1.1 lub nowszej. Aby włączyć protokół WebSockets, rozwiń następujące węzły: **usługi sieci World Wide Web** > **funkcje tworzenia aplikacji**. Wybierz **protokół WebSocket** funkcji. Aby uzyskać więcej informacji, zobacz [Websocket](xref:fundamentals/websockets).

1. Jeśli instalacja usług IIS wymaga ponownego uruchomienia komputera, należy ponownie uruchomić system.

![Konsola zarządzania usługami IIS i usługi sieci World Wide Web są zaznaczone w funkcji systemu Windows.](index/_static/windows-features-win10.png)

---

## <a name="install-the-net-core-hosting-bundle"></a>Zainstaluj oprogramowanie .NET Core Hosting pakietu

1. Zainstaluj *.NET Core Hosting pakietu* przez system operacyjny. Pakiet instaluje wykonawczym .NET Core .NET Core biblioteki, a [platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module). Moduł tworzy zwrotny serwer proxy między usługami IIS a Kestrel serwera. Jeśli system nie ma połączenia internetowego, Uzyskaj i zainstaluj [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) przed zainstalowaniem pakietu Hosting .NET Core.

   1. Przejdź do [.NET wszystkie pliki do pobrania strony](https://www.microsoft.com/net/download/all).
   1. Wybierz z listy najnowsze środowisko uruchomieniowe .NET Core-preview (**.NET Core** > **środowiska uruchomieniowego** > **x.y.z środowisko uruchomieniowe platformy .NET Core**). Jeśli nie zamierzasz pracować z oprogramowaniem w wersji zapoznawczej, uniknąć środowiska uruchomieniowego od słowa "w wersji zapoznawczej" lub "rc" (Wersja Release Candidate) tekst łącza.
   1. Na środowiska uruchomieniowego .NET Core strony w obszarze pobierania **Windows**, wybierz pozycję **Hosting Instalatora pakietu** łącze, aby pobrać *.NET Core Hosting pakietu*.

   **Ważne!** Po zainstalowaniu pakietu Hosting przed zainstalowaniem usług IIS instalacji pakietu musi zostać naprawiony. Uruchom Instalatora pakietu Hosting ponownie po zainstalowaniu usług IIS.
   
   Aby zapobiec zainstalowaniu x86 Instalator pakietów na x64 systemu operacyjnego, uruchom Instalatora z wiersza polecenia administratora z przełącznikiem `OPT_NO_X86=1`.

1. Ponowne uruchamianie systemu lub wykonać **net stop została /y** następuje **net start w3svc** z wiersza polecenia. Ponowne uruchomienie usług IIS przejmuje zmianę systemowej PATH wprowadzone przez Instalator.

> [!NOTE]
> Aby uzyskać informacje o konfiguracji udostępnionej usług IIS, zobacz [platformy ASP.NET Core modułu z konfiguracji udostępnionej usług IIS](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Zainstaluj pakiet Webdeploy podczas publikowania z programem Visual Studio

W przypadku wdrażania aplikacji na serwerach z [narzędzia Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), zainstaluj najnowszą wersję narzędzia Web Deploy na serwerze. Aby zainstalować narzędzie Web Deploy, użyj [Instalatora platformy sieci Web (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) lub uzyskać Instalatora bezpośrednio z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717). Preferowaną metodą jest używać WebPI. WebPI oferuje autonomicznego Instalatora i konfiguracja dla dostawców hostingu.

## <a name="create-the-iis-site"></a>Tworzenie witryny usług IIS

1. Przez system operacyjny należy utworzyć folder do zawierać plików i folderów opublikowanych aplikacji. Układ wdrożenia aplikacji jest opisany w [struktury katalogów](xref:host-and-deploy/directory-structure) tematu.

1. W ramach nowego folderu, Utwórz *dzienniki* folder do przechowywania dzienników stdout moduł platformy ASP.NET Core po włączeniu rejestrowania stdout. Jeśli aplikacja jest wdrażana z *dzienniki* folderu w ładunku, Pomiń ten krok. Aby uzyskać instrukcje dotyczące włączania MSBuild tworzenia *dzienniki* folderu automatycznie, gdy projekt jest budowany lokalnie, zobacz [struktury katalogów](xref:host-and-deploy/directory-structure) tematu.

   > [!IMPORTANT]
   > Należy używać tylko w dzienniku stdout rozwiązywać problemy z uruchamianiem aplikacji. Nigdy nie używaj stdout rejestrowania do rejestrowania procedury aplikacji. Brak limitu rozmiaru pliku dziennika lub liczba pliki dziennika utworzone. Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki. Wszystkie foldery w ścieżce do lokalizacja dziennika musi istnieć. Aby uzyskać więcej informacji w dzienniku stdout, zobacz [tworzenia i Przekierowanie dziennika](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection). Informacje dotyczące rejestrowania w aplikacji platformy ASP.NET Core znajdują się w temacie [rejestrowanie](xref:fundamentals/logging/index) tematu.

1. W **Menedżera usług IIS**, otwórz węzeł serwera w **połączeń** panelu. Kliknij prawym przyciskiem myszy **witryny** folderu. Wybierz **Dodawanie witryny sieci Web** z menu kontekstowego.

1. Podaj **nazwa witryny** i ustaw **ścieżka fizyczna** do folderu wdrożenia aplikacji. Podaj **powiązanie** konfiguracji i tworzyć witryny sieci Web, wybierając **OK**:

   ![Podaj nazwę lokacji, ścieżka fizyczna i nazwy hosta w kroku Dodawanie witryny sieci Web.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > Powiązania najwyższego poziomu symbolu wieloznacznego (`http://*:80/` i `http://+:80`) powinien **nie** można użyć. Powiązania wieloznaczny najwyższego poziomu można otwarcie luk w zabezpieczeniach aplikacji. Dotyczy to zarówno silne i słabe symboli wieloznacznych. Użyj nazwy hostów jawne zamiast symboli wieloznacznych. Powiązanie symbolu wieloznacznego domeny podrzędnej (na przykład `*.mysub.com`) nie ma to zagrożenie bezpieczeństwa, jeśli kontrolować domeny nadrzędnej całego (w przeciwieństwie do `*.com`, której występuje). Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.

1. W węźle serwera, wybierz **pul aplikacji**.

1. Kliknij prawym przyciskiem myszy pulę aplikacji witryny i wybierz **podstawowe ustawienia** z menu kontekstowego.

1. W **edytowanie puli aplikacji** ustaw **wersja środowiska .NET CLR** do **bez kodu zarządzanego**:

   ![Ustaw bez kodu zarządzanego dla wersji środowiska .NET CLR.](index/_static/edit-apppool-ws2016.png)

    Platformy ASP.NET Core działa w oddzielnych procesach i zarządza środowiska uruchomieniowego. Platformy ASP.NET Core nie zależą od ładowania CLR pulpitu. Ustawienie **wersja środowiska .NET CLR** do **bez kodu zarządzanego** jest opcjonalna.

1. Upewnij się, że tożsamość procesu modelu ma odpowiednie uprawnienia.

   Jeśli domyślna tożsamość puli aplikacji (**Model procesu** > **tożsamości**) została zmieniona z **puli** tożsamość, sprawdź, czy nową tożsamość ma wymagane uprawnienia dostępu do folderu instalacji aplikacji, bazy danych i innych wymaganych zasobów. Pula aplikacji wymaga na przykład odczytu i zapisu do folderów, gdzie odczytuje i zapisuje pliki aplikacji.

**Konfiguracja uwierzytelniania systemu Windows (opcjonalnie)**  
Aby uzyskać więcej informacji, zobacz [uwierzytelniania skonfigurować systemu Windows](xref:security/authentication/windowsauth).

## <a name="deploy-the-app"></a>Wdrażanie aplikacji

Wdróż aplikację na folder utworzony przez system operacyjny. [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) to mechanizm zalecane dla wdrożenia.

### <a name="web-deploy-with-visual-studio"></a>Narzędzie Web Deploy w programie Visual Studio

Zobacz [profilów publikowania programu Visual Studio dla wdrożenia aplikacji platformy ASP.NET Core](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) tematu, aby dowiedzieć się, jak utworzyć profil publikowania do użytku z narzędzia Web Deploy. Jeśli dostawcy hostingu zapewnia profilu publikacji lub pomocy technicznej dla utworzenie, Pobierz swój profil i zaimportuj go za pomocą programu Visual Studio **publikowania** okna dialogowego.

![Okno dialogowe strony publikacji](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Narzędzie Web Deploy poza programu Visual Studio

[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) można również poza Visual Studio z wiersza polecenia. Aby uzyskać więcej informacji, zobacz [narzędzie Web Deployment](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Wdrażanie rozwiązań alternatywnych w sieci Web

Przenieś aplikację do hostingu systemu, takie jak ręczne kopiowanie, Xcopy, Robocopy lub programu PowerShell przy użyciu jednej z kilku metod.

Aby uzyskać więcej informacji dotyczących wdrażania platformy ASP.NET Core w usługach IIS, zobacz [zasoby dotyczące wdrażania dla administratorów usług IIS](#deployment-resources-for-iis-administrators) sekcji.

## <a name="browse-the-website"></a>Przeglądaj witryny sieci Web

![Przeglądarka Microsoft Edge załadował strony początkowej usług IIS.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a>Pliki zablokowanego wdrożenia

Pliki w folderze wdrożenia są zablokowane, gdy aplikacja jest uruchomiona. Nie można zastąpić pliki zablokowane podczas wdrażania. Aby zwolnić zablokowane pliki we wdrożeniu, Zatrzymaj pulę aplikacji w programie **jeden** z następujących metod:

* Użyj narzędzia Web Deploy i odwołanie `Microsoft.NET.Sdk.Web` w pliku projektu. *App_offline.htm* plik znajduje się w głównym katalogu aplikacji sieci web. Gdy plik jest obecny, moduł platformy ASP.NET Core bezpiecznie zamyka aplikację i służy *app_offline.htm* plików podczas wdrażania. Aby uzyskać więcej informacji, zobacz [odwołania konfiguracji platformy ASP.NET Core modułu](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).
* Ręcznie zatrzymaj pulę aplikacji w Menedżerze usług IIS na serwerze.
* Zatrzymanie i ponowne uruchomienie puli aplikacji (wymaga programu PowerShell, 5 lub nowszy) przy użyciu programu PowerShell:

  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

## <a name="data-protection"></a>Ochrona danych

[Stosu ochrony danych platformy ASP.NET Core](xref:security/data-protection/index) jest używana przez kilka platformy ASP.NET Core [middlewares](xref:fundamentals/middleware/index), w tym oprogramowanie pośredniczące używane podczas uwierzytelniania. Nawet jeśli interfejsy API ochrony danych nie jest wywoływany przez kod użytkownika, ochrony danych można skonfigurować przy użyciu skryptu wdrożenia lub w kodzie użytkownika, aby utworzyć trwały kryptograficznych [magazynu kluczy](xref:security/data-protection/implementation/key-management). Jeśli nie jest skonfigurowany do ochrony danych, klucze są przechowywane w pamięci i odrzucone po ponownym uruchomieniu aplikacji.

Jeśli pierścień klucza jest przechowywana w pamięci po uruchomieniu aplikacji:

* Wszystkie tokeny na podstawie plików cookie uwierzytelniania są unieważniona. 
* Użytkownicy muszą ponownie zaloguj się na ich następnego żądania. 
* Wszystkie dane chronione za pomocą pierścień klucza nie mogły być odszyfrowane. Może to obejmować [tokenów CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) i [plików cookie platformy ASP.NET Core MVC TempData](xref:fundamentals/app-state#tempdata).

Aby skonfigurować ochronę danych w środowisku usług IIS, aby utrwalić pierścień klucza, użyj **jeden** z następujących metod:

* **Utwórz klucze rejestru ochrony danych**

  Dane ochrony kluczy używanych przez aplikacje platformy ASP.NET Core są przechowywane w rejestrze zewnętrzne do aplikacji. Aby zachować kluczy dla danej aplikacji, Utwórz klucze rejestru dla puli aplikacji.

  Dla autonomicznej, w przypadku instalacji usług IIS z systemem innym niż farma sieci Web [skrypt programu PowerShell należy AutoGenKeys.ps1 ochrony danych](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) można używać dla każdej puli aplikacji używane w aplikacji platformy ASP.NET Core. Ten skrypt tworzy klucz rejestru w rejestrze HKLM, który jest dostępny tylko dla konta procesu roboczego puli aplikacji w aplikacji. Klucze są szyfrowane, gdy przy użyciu DPAPI za pomocą klucza komputera.

  W scenariuszach kolektywu serwerów sieci web aplikację można skonfigurować do użyć ścieżki UNC do przechowywania jego pierścień klucza ochrony danych. Domyślnie nie są szyfrowane klucze ochrony danych. Upewnij się, że uprawnienia do udziału sieciowego są ograniczone do konta systemu Windows jest uruchamiana aplikacja. Certyfikat może służyć do ochrony kluczy magazynowane X509. Należy wziąć pod uwagę mechanizm Zezwalaj użytkownikom na przekazywanie certyfikaty: miejsce certyfikatów do zaufanego certyfikatu użytkownika przechowywania i upewnij się, są one dostępne na wszystkich komputerach, którym jest uruchamiany aplikacji użytkownika. Zobacz [konfiguracji ochrony danych ASP.NET Core](xref:security/data-protection/configuration/overview) szczegółowe informacje.

* **Konfigurowanie puli aplikacji usług IIS do załadowania profilu użytkownika**

  To ustawienie jest **Model procesu** w obszarze **Zaawansowane ustawienia** dla puli aplikacji. Ustaw Załaduj profil użytkownika `True`. To są przechowywane klucze w katalogu profilu użytkownika i ich ochrony za pomocą DPAPI kluczem określone konto użytkownika używane przez pulę aplikacji.

* **System plików jako pierścień klucza magazynu**

  Kod aplikacji, aby dopasować [system plików jako magazyn pierścień klucza](xref:security/data-protection/configuration/overview). Użyj X509 certyfikatu do ochrony klucza pierścień i upewnij się, certyfikat jest zaufany certyfikat. Jeśli certyfikat jest certyfikatem z podpisem, należy umieścić certyfikatu w magazynie zaufanych głównych.

  Korzystając z usług IIS w farmie sieci web:

  * Użyj udziału plików, które mogą uzyskiwać dostęp do wszystkich maszyn.
  * Wdrażanie X509 certyfikatów na każdym komputerze. Skonfiguruj [ochrony danych w kodzie](xref:security/data-protection/configuration/overview).

* **Ustawienie zasad komputera w przypadku ochrony danych**

  System ochrony danych ma ograniczoną obsługę ustawienie domyślne [komputera zasad](xref:security/data-protection/configuration/machine-wide-policy) dla wszystkich aplikacji, które korzystanie z interfejsów API ochrony danych. Zobacz [dokumentacji dotyczącej ochrony danych](xref:security/data-protection/index) szczegółowe informacje.

## <a name="sub-application-configuration"></a>Konfiguracja aplikacji podrzędna

Aplikacje podrzędne dodany w obszarze katalogu głównego aplikacji nie powinna zawierać moduł platformy ASP.NET Core jako program obsługi. Jeśli moduł jest dodawany jako program obsługi w sub-app *web.config* pliku *500.19 wewnętrzny błąd serwera* odwołuje się do pliku konfiguracji błędny odebraniu podczas próby przeglądania aplikacji sub.

W poniższym przykładzie przedstawiono opublikowanej *web.config* pliku dla platformy ASP.NET Core sub aplikacji:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Odnośnie do hostowania - platformy ASP.NET Core sub aplikacji w kontenerze aplikacji platformy ASP.NET Core, jawnie usunąć dziedziczonej obsługi w aplikacji sub *web.config* pliku:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Aby uzyskać więcej informacji na temat konfigurowania moduł platformy ASP.NET Core, zobacz [wprowadzenie do platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module) tematu i [odwołania konfiguracji platformy ASP.NET Core modułu](xref:host-and-deploy/aspnet-core-module).

## <a name="configuration-of-iis-with-webconfig"></a>Konfiguracja usług IIS z pliku web.config

Konfiguracja usług IIS ma wpływ  **\<system.webServer >** sekcji *web.config* dla tych funkcji usług IIS, które mają zastosowanie do konfiguracji zwrotnego serwera proxy. Jeśli usługi IIS zostały skonfigurowane na poziomie serwera, aby użyć kompresji dynamicznej  **\<urlCompression >** element w aplikacji *web.config* pliku można go wyłączyć.

Aby uzyskać więcej informacji, zobacz [konfiguracji odwołania dla \<system.webServer >](/iis/configuration/system.webServer/), [odwołania do konfiguracji modułu platformy ASP.NET Core](xref:host-and-deploy/aspnet-core-module), i [modułów usług IIS z wykorzystaniem technologii ASP.NET Podstawowe](xref:host-and-deploy/iis/modules). Ustawianie zmiennych środowiskowych dla poszczególnych aplikacji uruchomionych w pulach aplikacji izolowanych (obsługiwane dla programu IIS 10.0 lub nowszego), zobacz *polecenia AppCmd.exe* sekcji [zmiennych środowiskowych \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) dokumentacji odwołania tematu w usługach IIS.

## <a name="configuration-sections-of-webconfig"></a>Sekcji konfiguracyjnych w pliku Web.config

ASP.NET 4.x aplikacji w sekcji konfiguracji *web.config* nie są używane przez aplikacje platformy ASP.NET Core dla konfiguracji:

* **\<system.web>**
* **\<appSettings>**
* **\<connectionStrings>**
* **\<Lokalizacja >**

Aplikacje platformy ASP.NET Core są skonfigurowane przy użyciu innych dostawców konfiguracji. Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Pule aplikacji

Odnośnie do hostowania wielu witryn sieci Web na serwerze, izolowania aplikacji od siebie, uruchamiając każdej aplikacji w odrębnej puli aplikacji. Usługi IIS **Dodawanie witryny sieci Web** domyślnie okno dialogowe tej konfiguracji. Gdy **nazwa witryny** została podana, tekst jest automatycznie przenoszona do **puli aplikacji** pola tekstowego. Jest tworzona nowa pula aplikacji przy użyciu nazwy lokacji po dodaniu lokacji.

## <a name="application-pool-identity"></a>Tożsamość puli aplikacji

Konto tożsamości puli aplikacji dzięki czemu aplikacja może działać w ramach unikatowe konto bez konieczności tworzenia i zarządzania nimi domeny lub lokalnego konta. W programie IIS 8.0 lub nowszym procesów roboczych administratora usług IIS (WAS) tworzy konto wirtualnego o nazwie nowej puli aplikacji i aplikacji w puli procesów roboczych na tym koncie są domyślnie uruchamiane. W konsoli zarządzania usług IIS w obszarze **Zaawansowane ustawienia** dla puli aplikacji, upewnij się, że **tożsamości** jest skonfigurowany do używania **puli**:

![Okno dialogowe Zaawansowane ustawienia puli aplikacji](index/_static/apppool-identity.png)

Proces zarządzania usług IIS tworzy bezpiecznego identyfikatora o nazwie puli aplikacji w systemie zabezpieczeń systemu Windows. Zasoby mogą być chronione przy użyciu tej tożsamości. Jednak ta tożsamość nie jest rzeczywistego konta użytkownika i nie pojawiają się w konsoli zarządzania użytkownika systemu Windows.

Jeśli proces roboczy usług IIS wymaga podwyższonego poziomu dostępu do aplikacji, należy zmodyfikować listy kontroli dostępu (ACL) dla katalogu zawierającej aplikację:

1. Otwórz Eksploratora Windows i przejdź do katalogu.

1. Kliknij prawym przyciskiem myszy na katalog i wybierz **właściwości**.

1. W obszarze **zabezpieczeń** wybierz opcję **Edytuj** przycisk, a następnie **Dodaj** przycisku.

1. Wybierz **lokalizacje** przycisk i upewnij się, że system jest zaznaczone.

1. Wprowadź **IIS AppPool\\< app_pool_name >** w **wprowadź nazwy obiektów do wybrania** obszaru. Wybierz **Sprawdź nazwy** przycisku. Aby uzyskać *DefaultAppPool* sprawdzanie nazw przy użyciu **IIS AppPool\DefaultAppPool**. Gdy **Sprawdź nazwy** przycisk jest zaznaczony, wartość **DefaultAppPool** podane w obszarze nazwy obiektu. Nie można wprowadzić nazwę puli aplikacji bezpośrednio do obszaru nazw obiektów. Użyj **IIS AppPool\\< app_pool_name >** formatowania podczas sprawdzania dostępności nazwy obiektu.

   ![Wybierz użytkowników lub grup okna dialogowego folderu aplikacji: Nazwa puli aplikacji "Domyślna pula aplikacji" jest dołączany do "IIS AppPool\" w obszarze nazw obiektu przed wybraniem"Sprawdź nazwy".](index/_static/select-users-or-groups-1.png)

1. Wybierz **OK**.

   ![Wybierz użytkowników lub grup okna dialogowego folderu aplikacji: po wybraniu "Sprawdź nazwy", nazwa obiektu "Domyślna pula aplikacji" jest wyświetlany w obiekcie nazwy obszaru.](index/_static/select-users-or-groups-2.png)

1. Odczyt &amp; wykonania uprawnienia domyślne. Podaj dodatkowe uprawnienia, zgodnie z potrzebami.

Można również przyznany dostęp za pomocą wiersza polecenia **ICACLS** narzędzia. Przy użyciu *DefaultAppPool* na przykład służy następujące polecenie:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

Aby uzyskać więcej informacji, zobacz [icacls](/windows-server/administration/windows-commands/icacls) tematu.

## <a name="deployment-resources-for-iis-administrators"></a>Zasoby dotyczące wdrażania dla administratorów usług IIS

Więcej informacji na temat usług IIS szczegółowe w dokumentacji usług IIS.  
[Dokumentacja usług IIS](/iis)

Więcej informacji na temat modeli wdrażania aplikacji .NET Core.  
[Wdrażanie aplikacji .NET core](/dotnet/core/deploying/)

Dowiedz się, jak moduł platformy ASP.NET Core umożliwia Kestrel serwer sieci web dla usług IIS lub usług IIS Express jako serwera zwrotnego serwera proxy.  
[Moduł ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)

Dowiedz się, jak skonfigurować moduł platformy ASP.NET Core do hostowania aplikacji platformy ASP.NET Core.  
[Odwołania do konfiguracji modułu platformy ASP.NET Core](xref:host-and-deploy/aspnet-core-module)

Więcej informacji na temat struktury katalogów opublikowanych aplikacji platformy ASP.NET Core.  
[Struktura katalogów](xref:host-and-deploy/directory-structure)

Wykryj aktywną i nieaktywną modułów usług IIS dla aplikacji platformy ASP.NET Core i zarządzanie modułów usług IIS.  
[Moduły IIS](xref:host-and-deploy/iis/troubleshoot)

Dowiedz się, jak diagnozować problemy z wdrożeniami usług IIS aplikacji platformy ASP.NET Core.  
[Rozwiązywanie problemów](xref:host-and-deploy/iis/troubleshoot)

Rozróżnianie typowe błędy hosting aplikacji platformy ASP.NET Core w usługach IIS.  
[Dokumentacja typowych błędów dla usług Azure App Service i IIS](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wprowadzenie do platformy ASP.NET Core](xref:index)
* [Witryna oficjalnego Microsoft IIS](https://www.iis.net/)
* [Biblioteka zawartości technicznej systemu Windows Server](/windows-server/windows-server)
