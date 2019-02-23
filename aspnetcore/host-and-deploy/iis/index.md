---
title: Host platformy ASP.NET Core na Windows za pomocą programu IIS
author: guardrex
description: 'Dowiedz się, jak hostować aplikacje platformy ASP.NET Core na systemu Windows serwera Internet Information Services (IIS).'
ms.author: riande
ms.custom: mvc
ms.date: 02/19/2019
uid: host-and-deploy/iis/index
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Host platformy ASP.NET Core na Windows za pomocą programu IIS

Przez [Luke Latham](https://github.com/guardrex)

[Zainstaluj program .NET Core hostingu pakietu](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a>Supported operating systems

Obsługiwane są następujące systemy operacyjne:

* Windows 7 lub nowszy
* Windows Server 2008 R2 lub nowszy

[Serwer HTTP.sys](xref:fundamentals/servers/httpsys) (dawniej o nazwie WebListener) nie działa w konfiguracji zwrotny serwer proxy z usługami IIS. Użyj [serwera Kestrel](xref:fundamentals/servers/kestrel).

Aby uzyskać informacji na temat obsługi na platformie Azure, zobacz <xref:host-and-deploy/azure-apps/index>.

## <a name="supported-platforms"></a>Obsługiwane platformy

Aplikacje opublikowane (x86) 32-bitowych i 64-bitowych (x 64) wdrożenia są obsługiwane. Wdrażanie aplikacji 32-bitowych, chyba że aplikacja:

* Wymaga większych pamięci wirtualnej przestrzeni adresowej dostępne dla aplikacji 64-bitowych.
* Wymaga większy rozmiar stosu usług IIS.
* Ma zależności natywnych 64-bitowych.

## <a name="application-configuration"></a>Konfiguracja aplikacji

### <a name="enable-the-iisintegration-components"></a>Włącz składniki IISIntegration

::: moniker range=">= aspnetcore-2.1"

Typowa *Program.cs* wywołania <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> aby rozpocząć konfigurowanie hosta:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Typowa *Program.cs* wywołania <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> aby rozpocząć konfigurowanie hosta:

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

**W trakcie modelu hostingu**

`CreateDefaultBuilder` wywołania `UseIIS` metoda rozruchu [CoreCLR](/dotnet/standard/glossary#coreclr) i hostowania aplikacji wewnątrz proces roboczy usług IIS (*w3wp.exe* lub *iisexpress.exe*). Testy wydajności wskazują, że hostowanie platformy .NET Core app w procesie zapewnia znacznie większą przepływność żądania, w porównaniu do hostowania aplikacji spoza procesu i pośredniczenie żądania do [Kestrel](xref:fundamentals/servers/kestrel) serwera.

Model hostingu w trakcie nie jest obsługiwana dla aplikacji platformy ASP.NET Core, które obsługują program .NET Framework.

**Model hostingu poza procesem**

Dla hostingu poza procesem, za pomocą programu IIS, `CreateDefaultBuilder` konfiguruje [Kestrel](xref:fundamentals/servers/kestrel) serwera jako serwera sieci web i umożliwia integrację usług IIS, konfigurując ścieżki podstawowej i port [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

Modułu ASP.NET Core generuje portów dynamicznych do przypisania do procesu zaplecza. `CreateDefaultBuilder` wywołania <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> metody. `UseIISIntegration` Konfiguruje usługi Kestrel do nasłuchiwania na port dynamiczny adres IP hosta lokalnego (`127.0.0.1`). Jeśli port dynamiczny jest 1234, Kestrel nasłuchuje na `127.0.0.1:1234`. Ta konfiguracja zastępuje inne konfiguracje adresu URL, dostarczone przez:

* `UseUrls`
* [Interfejs API nasłuchiwania kestrel firmy](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Konfiguracja](xref:fundamentals/configuration/index) (lub [opcji wiersza polecenia — adresy URL](xref:fundamentals/host/web-host#override-configuration))

Wywołania `UseUrls` lub jego Kestrel `Listen` interfejsu API nie są wymagane w przypadku korzystania z modułu. Jeśli `UseUrls` lub `Listen` jest wywoływane Kestrel nasłuchuje na określone porty tylko podczas uruchamiania aplikacji bez usług IIS.

Aby uzyskać więcej informacji o modelach hostingu w procesie i poza procesem, zobacz [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) i [informacje o konfiguracji modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

::: moniker-end

::: moniker range="= aspnetcore-2.1"

`CreateDefaultBuilder` Konfiguruje [Kestrel](xref:fundamentals/servers/kestrel) serwera jako serwera sieci web i umożliwia integrację usług IIS, konfigurując ścieżki podstawowej i port [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

Modułu ASP.NET Core generuje portów dynamicznych do przypisania do procesu zaplecza. `CreateDefaultBuilder` wywołania <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> metody. `UseIISIntegration` Konfiguruje usługi Kestrel do nasłuchiwania na port dynamiczny adres IP hosta lokalnego (`127.0.0.1`). Jeśli port dynamiczny jest 1234, Kestrel nasłuchuje na `127.0.0.1:1234`. Ta konfiguracja zastępuje inne konfiguracje adresu URL, dostarczone przez:

* `UseUrls`
* [Interfejs API nasłuchiwania kestrel firmy](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Konfiguracja](xref:fundamentals/configuration/index) (lub [opcji wiersza polecenia — adresy URL](xref:fundamentals/host/web-host#override-configuration))

Wywołania `UseUrls` lub jego Kestrel `Listen` interfejsu API nie są wymagane w przypadku korzystania z modułu. Jeśli `UseUrls` lub `Listen` jest wywoływane Kestrel nasłuchuje na porcie określony tylko podczas uruchamiania aplikacji bez usług IIS.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

`CreateDefaultBuilder` Konfiguruje [Kestrel](xref:fundamentals/servers/kestrel) serwera jako serwera sieci web i umożliwia integrację usług IIS, konfigurując ścieżki podstawowej i port [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

Modułu ASP.NET Core generuje portów dynamicznych do przypisania do procesu zaplecza. `CreateDefaultBuilder` wywołania <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> metody. `UseIISIntegration` Konfiguruje usługi Kestrel do nasłuchiwania na port dynamiczny adres IP hosta lokalnego (`localhost`). Jeśli port dynamiczny jest 1234, Kestrel nasłuchuje na `localhost:1234`. Ta konfiguracja zastępuje inne konfiguracje adresu URL, dostarczone przez:

* `UseUrls`
* [Interfejs API nasłuchiwania kestrel firmy](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Konfiguracja](xref:fundamentals/configuration/index) (lub [opcji wiersza polecenia — adresy URL](xref:fundamentals/host/web-host#override-configuration))

Wywołania `UseUrls` lub jego Kestrel `Listen` interfejsu API nie są wymagane w przypadku korzystania z modułu. Jeśli `UseUrls` lub `Listen` jest wywoływane Kestrel nasłuchuje na porcie określony tylko podczas uruchamiania aplikacji bez usług IIS.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Obejmują zależności na [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) pakietu w zależnościach aplikacji. Używanie oprogramowania pośredniczącego integracji usług IIS przez dodanie <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> metodę rozszerzenia, aby <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

Zarówno <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> i <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> są wymagane. Wywoływanie kodu `UseIISIntegration` nie ma wpływu na przenoszenie kodu. Jeśli aplikacja nie jest uruchamiana za usług IIS (na przykład, aplikacja jest uruchamiana bezpośrednio na Kestrel), `UseIISIntegration` nie działa.

Modułu ASP.NET Core generuje portów dynamicznych do przypisania do procesu zaplecza. `UseIISIntegration` Konfiguruje usługi Kestrel do nasłuchiwania na port dynamiczny adres IP hosta lokalnego (`localhost`). Jeśli port dynamiczny jest 1234, Kestrel nasłuchuje na `localhost:1234`. Ta konfiguracja zastępuje inne konfiguracje adresu URL, dostarczone przez:

* `UseUrls`
* [Konfiguracja](xref:fundamentals/configuration/index) (lub [opcji wiersza polecenia — adresy URL](xref:fundamentals/host/web-host#override-configuration))

Wywołanie `UseUrls` nie jest wymagana podczas korzystania z modułu. Jeśli `UseUrls` jest wywoływane Kestrel nasłuchuje na porcie określony tylko podczas uruchamiania aplikacji bez usług IIS.

Jeśli `UseUrls` jest wywoływana w aplikacji ASP.NET Core 1.0, wywołaj ją **przed** wywoływania `UseIISIntegration` tak, aby moduł skonfigurowany port nie są zastępowane. Ta kolejność wywoływania nie jest wymagana przy użyciu platformy ASP.NET Core 1.1, ponieważ zastępuje ustawienia modułu `UseUrls`.

::: moniker-end

Aby uzyskać więcej informacji o hostingu, zobacz [hostów w programie ASP.NET Core](xref:fundamentals/index#host).

### <a name="iis-options"></a>Opcje programu IIS

::: moniker range=">= aspnetcore-2.2"

**W trakcie modelu hostingu**

Aby skonfigurować opcje serwera usług IIS, obejmują konfigurację usługi <xref:Microsoft.AspNetCore.Builder.IISServerOptions> w <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>. Poniższy przykład wyłącza AutomaticAuthentication:

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

| Opcja                         | Domyślny | Ustawienie |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | Jeśli `true`, ustawia serwer IIS `HttpContext.User` uwierzytelnione przez [uwierzytelniania Windows](xref:security/authentication/windowsauth). Jeśli `false`, serwer tylko zapewnia usługi tożsamości dla `HttpContext.User` i sprostać wymaganiom, gdy wyraźnie żąda przez `AuthenticationScheme`. Należy włączyć uwierzytelnianie Windows w usługach IIS dla `AutomaticAuthentication` funkcji. Aby uzyskać więcej informacji, zobacz [uwierzytelniania Windows](xref:security/authentication/windowsauth). |
| `AuthenticationDisplayName`    | `null`  | Określa nazwę wyświetlaną, widocznym dla użytkowników na stronach logowania. |

**Model hostingu poza procesem**

::: moniker-end

Aby skonfigurować opcje programu IIS, obejmują konfigurację usługi <xref:Microsoft.AspNetCore.Builder.IISOptions> w <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>. Poniższy przykład uniemożliwia jej wypełnianie `HttpContext.Connection.ClientCertificate`:

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| Opcja                         | Domyślny | Ustawienie |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | Jeśli `true`, ustawia oprogramowania pośredniczącego integracji usługi IIS `HttpContext.User` uwierzytelnione przez [uwierzytelniania Windows](xref:security/authentication/windowsauth). Jeśli `false`, oprogramowanie pośredniczące tylko zapewnia usługi tożsamości dla `HttpContext.User` i sprostać wymaganiom, gdy wyraźnie żąda przez `AuthenticationScheme`. Należy włączyć uwierzytelnianie Windows w usługach IIS dla `AutomaticAuthentication` funkcji. Aby uzyskać więcej informacji, zobacz [uwierzytelniania Windows](xref:security/authentication/windowsauth) tematu. |
| `AuthenticationDisplayName`    | `null`  | Określa nazwę wyświetlaną, widocznym dla użytkowników na stronach logowania. |
| `ForwardClientCertificate`     | `true`  | Jeśli `true` i `MS-ASPNETCORE-CLIENTCERT` nagłówek żądania jest obecny, `HttpContext.Connection.ClientCertificate` jest wypełnione. |

### <a name="proxy-server-and-load-balancer-scenarios"></a>Serwer proxy i scenariuszy usługi równoważenia obciążenia

Usługi IIS oprogramowania pośredniczącego integracji, który konfiguruje przekazany oprogramowania pośredniczącego nagłówków i modułu ASP.NET Core są skonfigurowane do przekazywania schemat (HTTP/HTTPS) i zdalny adres IP, skąd pochodzi żądanie. Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych za serwery proxy dodatkowe i moduły równoważenia obciążenia. Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).

### <a name="webconfig-file"></a>plik Web.config

*Web.config* plik konfiguruje [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module). Tworzenie, przekształcania i publikowania *web.config* pliku jest obsługiwane przez obiekt docelowy programu MSBuild (`_TransformWebConfig`) po opublikowaniu projektu. Ten element docelowy znajduje się w elementy docelowe zestawu SDK sieci Web (`Microsoft.NET.Sdk.Web`). Zestaw SDK jest ustawiony w górnej części pliku projektu:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

Jeśli *web.config* pliku nie jest obecny w projekcie, plik jest tworzony z prawidłowymi *processPath* i *argumenty* skonfigurować [platformy ASP.NET Core Moduł](xref:host-and-deploy/aspnet-core-module) przeniesiona do ikony [opublikowane dane wyjściowe](xref:host-and-deploy/directory-structure).

Jeśli *web.config* plik znajduje się w projekcie, plik jest przekształcana z prawidłowymi *processPath* i *argumenty* do skonfigurowania modułu ASP.NET Core przeniesiona do ikony opublikowane dane wyjściowe. Przekształcenie nie zmodyfikować ustawień konfiguracji usług IIS w pliku.

*Web.config* plik mogą dostarczać dodatkowych ustawień konfiguracji usług IIS, które kontrolują aktywne moduły usług IIS. Aby uzyskać informacji na temat moduły usług IIS, które są w stanie przetwarzania żądań z aplikacji platformy ASP.NET Core, zobacz [moduły usług IIS](xref:host-and-deploy/iis/modules) tematu.

Aby zapobiec przekształcania zestawu SDK sieci Web *web.config* pliku, użyj  **\<IsTransformWebConfigDisabled >** właściwość w pliku projektu:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Zestaw SDK sieci Web z transformacji pliku, wyłączając *processPath* i *argumenty* należy ręcznie ustawić przez dewelopera. Aby uzyskać więcej informacji, zobacz [informacje o konfiguracji modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

### <a name="webconfig-file-location"></a>Lokalizacja pliku Web.config

Aby skonfigurować [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) poprawnie, *web.config* plik musi znajdować się w ścieżce głównej zawartości (zwykle ścieżki podstawowej aplikacji) wdrożonej aplikacji. Jest to tej samej lokalizacji co ścieżka fizyczna witryny sieci Web dostarczone do usług IIS. *Web.config* plik jest wymagany w katalogu głównym aplikacji, aby umożliwić publikowanie wielu aplikacji za pomocą narzędzia Web Deploy.

Poufne pliki istnieją na ścieżkę fizyczną aplikacji, takich jak  *\<zestawu >. runtimeconfig.json*,  *\<zestawu > .xml* (komentarze dokumentacji XML), a  *\<zestawu >. deps.json*. Gdy *web.config* plik jest obecny i i lokacji uruchamia się normalnie, usługi IIS nie obsługuje tych poufnych plików, jeśli są one wymagane. Jeśli *web.config* brakuje pliku, niepoprawnie o nazwie lub nie można skonfigurować witrynę podczas normalnego uruchamiania, usług IIS może obsługiwać poufnych plików publicznie.

***Web.config* plik musi być obecny w ramach wdrożenia przez cały czas nazwane poprawnie i możliwe jest skonfigurowanie lokacji do rozpoczęcia normalnego się. Nigdy nie należy usunąć *web.config* plik z wdrożenia produkcyjnego.**

### <a name="transform-webconfig"></a>Przekształcanie pliku web.config

Jeśli potrzebujesz do przekształcania *web.config* przy publikowaniu (na przykład ustawienie zmiennych środowiskowych na podstawie konfiguracji, profilu lub środowiska), zobacz <xref:host-and-deploy/iis/transform-webconfig>.

## <a name="iis-configuration"></a>Konfiguracja programu IIS

**Systemy operacyjne Windows Server**

Włącz **serwer sieci Web (IIS)** roli serwera i ustanowić usług ról.

1. Użyj **Dodaj role i funkcje** kreatora z **Zarządzaj** menu lub linku w **Menedżera serwera**. Na **ról serwera** kroku, pole wyboru dla **serwer sieci Web (IIS)**.

   ![W kroku role serwera wybierz zaznaczona została rola Serwer sieci Web IIS.](index/_static/server-roles-ws2016.png)

1. Po **funkcji** kroku **usług ról** kroku ładuje dla serwera sieci Web (IIS). Wybierz potrzebnych usług roli usług IIS lub zaakceptuj domyślną rolę usług pod warunkiem.

   ![Domyślne usługi ról są wybrane w kroku wybierz rolę usług.](index/_static/role-services-ws2016.png)

   **Windows Authentication (opcjonalnie)**  
   Aby włączyć uwierzytelnianie Windows, rozwiń następujące węzły: **Serwer sieci Web** > **zabezpieczeń**. Wybierz **uwierzytelniania Windows** funkcji. Aby uzyskać więcej informacji, zobacz [uwierzytelniania Windows \<windowsAuthentication >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) i [uwierzytelniania Windows skonfiguruj](xref:security/authentication/windowsauth).

   **Gniazda Websocket (opcjonalnie)**  
   Funkcja WebSockets jest obsługiwana przy użyciu platformy ASP.NET Core 1.1 lub nowszej. Aby włączyć protokół WebSockets, rozwiń następujące węzły: **Serwer sieci Web** > **opracowywanie aplikacji**. Wybierz **protokołu WebSocket** funkcji. Aby uzyskać więcej informacji, zobacz [WebSockets](xref:fundamentals/websockets).

1. Postępuj zgodnie z instrukcjami **potwierdzenie** krok w celu zainstalowania roli Serwer sieci web i usług. Ponowne uruchomienie serwera/IIS nie jest wymagane po zainstalowaniu **serwer sieci Web (IIS)** roli.

**Systemy operacyjne Windows**

Włącz **Konsola zarządzania usługami IIS** i **usługi World Wide Web**.

1. Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **Windows Włącz funkcje w lub wyłącz** (po lewej stronie ekranu).

1. Otwórz **Internetowe usługi informacyjne** węzła. Otwórz **narzędzia zarządzania siecią Web** węzła.

1. Pole wyboru dla **Konsola zarządzania usługami IIS**.

1. Pole wyboru dla **usługi World Wide Web**.

1. Zaakceptuj domyślne funkcje dla **usługi World Wide Web** lub dostosowywanie funkcji usług IIS.

   **Windows Authentication (opcjonalnie)**  
   Aby włączyć uwierzytelnianie Windows, rozwiń następujące węzły: **Usługi World Wide Web** > **zabezpieczeń**. Wybierz **uwierzytelniania Windows** funkcji. Aby uzyskać więcej informacji, zobacz [uwierzytelniania Windows \<windowsAuthentication >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) i [uwierzytelniania Windows skonfiguruj](xref:security/authentication/windowsauth).

   **Gniazda Websocket (opcjonalnie)**  
   Funkcja WebSockets jest obsługiwana przy użyciu platformy ASP.NET Core 1.1 lub nowszej. Aby włączyć protokół WebSockets, rozwiń następujące węzły: **Usługi World Wide Web** > **funkcje tworzenia aplikacji**. Wybierz **protokołu WebSocket** funkcji. Aby uzyskać więcej informacji, zobacz [WebSockets](xref:fundamentals/websockets).

1. Jeśli instalacja usług IIS wymaga ponownego uruchomienia komputera, uruchom ponownie system.

![Konsola zarządzania usługami IIS oraz usługi sieci Web są zaznaczone w funkcji Windows.](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a>Zainstaluj program .NET Core hostingu pakietu

Zainstaluj *hostingu pakietu programu .NET Core* przez system operacyjny. Pakiet instaluje .NET Core środowisko uruchomieniowe, biblioteki platformy .NET Core i [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module). Moduł umożliwia platformy ASP.NET Core w aplikacji do uruchamiania w tle usług IIS. Jeśli system nie ma dostępu do Internetu, należy uzyskać i zainstalować [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) przed zainstalowaniem pakietu hostingu platformy .NET Core.

> [!IMPORTANT]
> Po zainstalowaniu pakietu hostowanie usług IIS wcześniejsze instalacji pakietu musi zostać naprawiony. Uruchom Instalatora pakietu hostingu ponownie po zainstalowaniu usług IIS.

### <a name="direct-download-current-version"></a>Pobieranie bezpośrednie (bieżąca wersja)

Pobierz Instalatora, korzystając z następującego linku:

[Bieżący Instalatora pakietu hostingu programu .NET Core (pobieranie bezpośrednie)](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a>Wcześniejszych wersjach Instalator

Aby uzyskać starszej wersji Instalatora:

1. Przejdź do [.NET Pobierz archiwa](https://www.microsoft.com/net/download/archives).
1. W obszarze **platformy .NET Core**, wybierz wersję platformy .NET Core.
1. W **uruchamiać aplikacje — środowisko uruchomieniowe** kolumny, znajdź wiersz żądanego wersji środowiska uruchomieniowego .NET Core.
1. Pobierz Instalatora przy użyciu **środowisko uruchomieniowe i pakiet hostingu** łącza.

> [!WARNING]
> Niektóre instalatory zawierają wersje, osiągnęły ich koniec cyklu życia (ma), które nie są już obsługiwane przez firmę Microsoft. Aby uzyskać więcej informacji, zobacz [zasady pomocy technicznej](https://www.microsoft.com/net/download/dotnet-core/2.0).

### <a name="install-the-hosting-bundle"></a>Zainstaluj pakiet hostingu

1. Uruchom Instalatora na serwerze. Podczas uruchamiania Instalatora jako administrator z powłoki poleceń, dostępne są następujące parametry:

   * `OPT_NO_ANCM=1` &ndash; Pomiń instalację modułu ASP.NET Core.
   * `OPT_NO_RUNTIME=1` &ndash; Pomiń instalację środowiska uruchomieniowego .NET Core.
   * `OPT_NO_SHAREDFX=1` &ndash; Pomiń instalację struktury programu ASP.NET udostępnione (środowisko uruchomieniowe programu ASP.NET).
   * `OPT_NO_X86=1` &ndash; Pomiń instalację x86 środowisk uruchomieniowych. Użyj tego parametru, gdy wiadomo, że użytkownik nie będzie hostingu aplikacji 32-bitowych. W przypadku każdej okazji, że zarówno 32-bitowych i 64-bitowych aplikacji będzie obsługiwać w przyszłości, nie za pomocą tego parametru i zainstaluj obydwu środowisk uruchomieniowych.
   * `OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; Wyłącz sprawdzanie przy użyciu konfiguracji udostępnionej usług IIS podczas konfiguracji udostępnionej (*applicationHost.config*) znajduje się na tym samym komputerze co instalacji usług IIS. *Dostępne tylko w przypadku platformy ASP.NET Core 2.2 lub nowszej instalatory obsługującego program instalujący niezamówione pakiety.* Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.
1. Ponowne uruchamianie systemu lub wykonać **net stop został /y** następuje **net start w3svc** z powłoki poleceń. Ponowne uruchomienie usług IIS przejmuje zmiany w systemie ścieżki, która jest zmienną środowiskową, wprowadzone przez Instalatora.

Jeśli Instalator Windows obsługującego pakietu wykryje, że usługi IIS wymaga zresetowania, aby dokończyć instalację, Instalator resetuje usług IIS. Jeśli Instalator wyzwala Resetowanie usług IIS, zostaną uruchomione ponownie wszystkie pule aplikacji usług IIS i witryn sieci Web.

> [!NOTE]
> Aby uzyskać informacji na temat konfiguracji udostępnionej usług IIS, zobacz [modułu ASP.NET Core przy użyciu konfiguracji udostępnionej usług IIS](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Zainstaluj pakiet Webdeploy podczas publikowania za pomocą programu Visual Studio

W przypadku wdrażania aplikacji na serwerach z [narzędzia Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), zainstaluj najnowszą wersję narzędzia Web Deploy na serwerze. Aby zainstalować narzędzie Web Deploy, należy użyć [Instalatora platformy sieci Web (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) lub uzyskać bezpośrednio z poziomu Instalatora [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717). Jest to preferowana metoda do użycia Instalatora WebPI. WebPI oferuje instalację autonomiczną i konfiguracji dla dostawców hostingu.

## <a name="create-the-iis-site"></a>Tworzenie witryny usług IIS

1. W systemie hostingu utworzyć folderu zawierającego pliki i foldery opublikowanych aplikacji. Układ wdrożenia aplikacji jest opisana w [strukturę katalogów](xref:host-and-deploy/directory-structure) tematu.

1. W Menedżerze usług IIS otwórz węzeł serwera w **połączeń** panelu. Kliknij prawym przyciskiem myszy **witryn** folderu. Wybierz **Dodawanie witryny sieci Web** z menu kontekstowego.

1. Podaj **Nazwa lokacji** i ustaw **ścieżkę fizyczną** do folderu wdrożenia aplikacji. Podaj **powiązanie** konfiguracji i tworzyć witryny sieci Web, wybierając **OK**:

   ![Podaj nazwę lokacji, ścieżkę fizyczną i nazwy hosta w kroku Dodawanie witryny sieci Web.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > Powiązania najwyższego poziomu symbolu wieloznacznego (`http://*:80/` i `http://+:80`) powinien **nie** można użyć. Powiązania najwyższego poziomu symboli wieloznacznych otworzyć aplikację w celu luk w zabezpieczeniach. Dotyczy to silnych i słabych symboli wieloznacznych. Użyj nazwy hostów jawne, a nie symboli wieloznacznych. Powiązanie symbol wieloznaczny domeny podrzędnej (na przykład `*.mysub.com`) nie ma to zagrożenie bezpieczeństwa, jeśli możesz kontrolować domenę nadrzędną całego (w przeciwieństwie do `*.com`, który jest narażony). Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.

1. W obszarze węzła serwera wybierz **pul aplikacji**.

1. Kliknij prawym przyciskiem myszy pulę aplikacji witryny i wybierz **podstawowych ustawień** z menu kontekstowego.

1. W **edytowanie puli aplikacji** oknie **wersja środowiska .NET CLR** do **bez kodu zarządzanego**:

   ![Ustaw bez kodu zarządzanego dla wersji środowiska .NET CLR.](index/_static/edit-apppool-ws2016.png)

    Platforma ASP.NET Core działa w oddzielnym procesie i zarządza środowiska uruchomieniowego. Platforma ASP.NET Core nie jest zależny od ładowanie klasycznych CLR. Ustawienie **wersja środowiska .NET CLR** do **bez kodu zarządzanego** jest opcjonalne.

1. *Platforma ASP.NET Core 2,2 lub nowszej*: Dla (x64) 64-bitowych [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd) , który używa [modelu hostingu w trakcie](xref:fundamentals/servers/index#in-process-hosting-model), Wyłącz pulę aplikacji dla procesów 32-bitowych (x 86).

   W **akcje** paska bocznego Menedżera usług IIS > **pul aplikacji**, wybierz opcję **ustawienia domyślne puli aplikacji** lub **Zaawansowane ustawienia**. Znajdź **Włącz 32-bitowych aplikacji** i ustaw wartość `False`. To ustawienie nie ma wpływu na aplikacje wdrożone dla [hostingu poza procesem](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).

1. Upewnij się, że tożsamość modelu procesu ma odpowiednie uprawnienia.

   Jeśli domyślna tożsamość puli aplikacji (**Model procesu** > **tożsamości**) została zmieniona z **ApplicationPoolIdentity** tożsamość, upewnij się, że nową tożsamość ma wymagane uprawnienia dostępu do folderu aplikacji, bazy danych i inne wymagane zasoby. Na przykład pula aplikacji wymaga dostępu odczytu i zapisu do folderów, gdzie aplikacja odczytuje i zapisuje pliki.

**Konfiguracja uwierzytelniania Windows (opcjonalnie)**  
Aby uzyskać więcej informacji, zobacz [uwierzytelniania Windows skonfiguruj](xref:security/authentication/windowsauth).

## <a name="deploy-the-app"></a>Wdrażanie aplikacji

Wdróż aplikację w folderze, który został utworzony przez system operacyjny. [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) zalecane mechanizm dla wdrożenia.

### <a name="web-deploy-with-visual-studio"></a>Narzędzie Web Deploy w programie Visual Studio

Zobacz [profilów publikowania programu Visual Studio dla wdrożenia aplikacji platformy ASP.NET Core](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) tematu, aby dowiedzieć się, jak utworzyć profil publikowania do użycia za pomocą narzędzia Web Deploy. Jeśli dostawca hostingu oferuje pomocy technicznej lub profilu publikacji tworzenia jeden, Pobierz swój profil i zaimportuj go za pomocą programu Visual Studio **Publikuj** okna dialogowego.

![Strona okna dialogowego publikowania](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Narzędzie Web Deploy poza programem Visual Studio

[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) można również poza programem Visual Studio z poziomu wiersza polecenia. Aby uzyskać więcej informacji, zobacz [narzędzie Web Deployment](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Wdrażanie rozwiązania alternatywne w sieci Web

Przenoszenie aplikacji do hostowania systemu, takie jak ręcznego kopiowania, polecenia Xcopy, Robocopy lub programu PowerShell przy użyciu jednej z kilku metod.

Aby uzyskać więcej informacji na temat wdrażania programu ASP.NET Core w usługach IIS, zobacz [zasoby dotyczące wdrażania dla administratorów usług IIS](#deployment-resources-for-iis-administrators) sekcji.

## <a name="browse-the-website"></a>Przeglądanie witryny sieci Web

![Przeglądarka Microsoft Edge został załadowany strony uruchamiania usług IIS.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a>Pliki wdrożenia zablokowane

Pliki w folderze wdrażania są zablokowane, gdy aplikacja jest uruchomiona. Nie można zastąpić pliki zablokowane podczas wdrażania. Aby zwolnić pliki zablokowane w danym wdrożeniu, Zatrzymaj pulę aplikację za pomocą **jeden** z następujących metod:

* Użyj narzędzia Web Deploy i odwołania `Microsoft.NET.Sdk.Web` w pliku projektu. *App_offline.htm* plik zostanie umieszczony w folderze głównym katalogu aplikacji sieci web. Gdy plik jest obecny, modułu ASP.NET Core bez problemu zmieniała zamyka aplikację i służy *app_offline.htm* pliku podczas wdrażania. Aby uzyskać więcej informacji, zobacz [informacje o konfiguracji modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).
* Ręcznie zatrzymaj pulę aplikacji w Menedżerze usług IIS na serwerze.
* Użyj programu PowerShell, aby porzucić *app_offline.html* (wymaga programu PowerShell 5 lub nowszy):

  ```PowerShell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a>Ochrona danych

[Stosu ochrony danych programu ASP.NET Core](xref:security/data-protection/introduction) jest używana przez kilka platformy ASP.NET Core [middlewares](xref:fundamentals/middleware/index), w tym oprogramowanie pośredniczące używane w przypadku uwierzytelniania. Nawet wtedy, gdy interfejsów API ochrony danych nie są wywoływane przez kod użytkownika, należy skonfigurować ochronę danych, za pomocą skryptu wdrożenia lub w kodzie użytkownika, aby utworzyć trwałe kryptograficznych [magazynu kluczy](xref:security/data-protection/implementation/key-management). Jeśli nie jest skonfigurowana ochrona danych, klucze są przechowywane w pamięci i odrzucone po ponownym uruchomieniu aplikacji.

Jeśli pierścień klucz jest przechowywany w pamięci, po ponownym uruchomieniu aplikacji:

* Wszystkie tokeny na podstawie plików cookie uwierzytelniania są unieważniane. 
* Użytkownicy muszą ponownie zaloguj się na ich następnego żądania. 
* Wszystkie dane chronione za pomocą pierścień klucz może już nie mogły być odszyfrowane. Może to obejmować [tokenów CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) i [plików cookie programu ASP.NET Core MVC TempData](xref:fundamentals/app-state#tempdata).

Aby skonfigurować ochronę danych w środowisku usług IIS, aby utrwalić pierścień klucza, należy użyć **jeden** z następujących metod:

* **Utworzyć klucze rejestru, ochrony danych**

  Klucze ochrony danych używane przez aplikacje platformy ASP.NET Core są przechowywane w rejestrze systemu zewnętrznego do aplikacji. Aby zachować klucze dla danej aplikacji, należy utworzyć klucze rejestru dla puli aplikacji.

  Dla autonomicznej, bez webfarm instalacji usług IIS, [skrypt programu PowerShell do aprowizacji AutoGenKeys.ps1 ochrony danych](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) może służyć do każdej puli aplikacji używana z aplikacji ASP.NET Core. Ten skrypt tworzy klucz rejestru w rejestrze HKLM, który jest dostępny tylko dla konta procesu roboczego puli aplikacji w aplikacji. Klucze są szyfrowane za pomocą DPAPI za pomocą klucza komputera.

  W scenariuszach z farmami internetowymi można skonfigurować aplikację można użyć ścieżki UNC do przechowywania jego pierścień klucz ochrony danych. Domyślnie klucze ochrony danych nie są szyfrowane. Upewnij się, że uprawnienia do udziału sieciowego są ograniczone do konta Windows, którego aplikacja działa. X509 certyfikatu może służyć do ochrony kluczy w stanie spoczynku. Należy wziąć pod uwagę mechanizmu, aby zezwolić użytkownikom na przekazywanie certyfikatów: Miejsce certyfikatów do zaufanego certyfikatu przez użytkownika, Przechowuj i upewnij się, że są one dostępne na wszystkich komputerach, którym jest uruchamiany aplikacji użytkownika. Zobacz [konfiguracji ochrony danych platformy ASP.NET Core](xref:security/data-protection/configuration/overview) Aby uzyskać szczegółowe informacje.

* **Konfigurowanie puli aplikacji usług IIS, aby załadować profil użytkownika**

  To ustawienie znajduje się w **Model procesu** sekcji **Zaawansowane ustawienia** dla puli aplikacji. Ustaw **Załaduj profil użytkownika** do `True`. Po ustawieniu `True`, klucze są przechowywane w katalogu profilu użytkownika i chronione przy użyciu interfejsu DPAPI za pomocą klucza, które są specyficzne dla konta użytkownika. Klucze są zachowywane do *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* folderu.

  Pula aplikacji [atrybut setProfileEnvironment](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) musi być także włączona. Wartość domyślna `setProfileEnvironment` jest `true`. W niektórych przypadkach (na przykład Windows System operacyjny) `setProfileEnvironment` ustawiono `false`. Jeśli klucze nie są przechowywane w katalogu profilu użytkownika, co Oczekiwano:

  1. Przejdź do *%windir%/system32/inetsrv/config* folderu.
  1. Otwórz *applicationHost.config* pliku.
  1. Znajdź `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` elementu.
  1. Upewnij się, że `setProfileEnvironment` atrybut nie jest obecny, które domyślnie używa wartości do `true`, lub jawnie ustawić wartość atrybutu `true`.

* **System plików do przechowywania klucza pierścienia**

  Kod aplikacji, aby dopasować [przy użyciu systemu plików do przechowywania klucza pierścień](xref:security/data-protection/configuration/overview). X509 należy używać certyfikatu do ochrony klucza pierścienia i upewnij się, certyfikat jest zaufany certyfikat. Jeśli certyfikat jest certyfikatem z podpisem własnym, umieść certyfikat w magazynie zaufanego certyfikatu głównego.

  Korzystając z usług IIS w ramach farmy sieci web:

  * Użyj udziału plików, które mogą uzyskiwać dostęp do wszystkich komputerów.
  * Wdrażanie X509 certyfikatu do każdej maszyny. Konfigurowanie [ochrony danych w kodzie](xref:security/data-protection/configuration/overview).

* **Ustaw zasady dla komputera w celu ochrony danych**

  System ochrony danych ma ograniczoną obsługę ustawiania domyślnych [komputera zasad](xref:security/data-protection/configuration/machine-wide-policy) dla wszystkich aplikacji korzystających z interfejsów API ochrony danych. Aby uzyskać więcej informacji, zobacz <xref:security/data-protection/introduction>.

## <a name="virtual-directories"></a>Katalogi wirtualne

[Wirtualne katalogi IIS](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) nie są obsługiwane przez aplikacje platformy ASP.NET Core. Aplikacja może być obsługiwany jako [podrzędnych aplikacji](#sub-applications).

## <a name="sub-applications"></a>Aplikacje podrzędne

Może być hostowana aplikacji ASP.NET Core jako [aplikacji podrzędnych usług IIS (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications). Ścieżka do aplikacji podrzędnej staje się częścią adresu URL aplikacji głównej.

::: moniker range="< aspnetcore-2.2"

Sub — aplikacja nie powinna zawierać modułu ASP.NET Core jako program obsługi. Jeśli moduł jest dodawany jako program obsługi w sub-app *web.config* pliku *500.19 wewnętrzny błąd serwera* odwołuje się do pliku błędnej konfiguracji jest zgłaszany, jeśli próba przeglądania aplikacji podrzędnej.

W poniższym przykładzie pokazano publikowania *web.config* sub aplikacji ASP.NET Core w pliku:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Odnośnie do hostowania aplikacji — ASP.NET Core sub-poniżej aplikacji ASP.NET Core, jawnie usunąć dziedziczonych obsługi w aplikacji podrzędnej *web.config* pliku:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

Łączy statycznych zasobów w obrębie aplikacji podrzędnej należy używać ukośnika tyldy (`~/`) notacji. Wyzwalacze notacji ukośnika tylda [Pomocnik tagu](xref:mvc/views/tag-helpers/intro) można poprzedzić pathbase aplikacji podrzędnych do renderowanej względnego linku. Sub-aplikacji na `/subapp_path`, obraz połączony z `src="~/image.png"` jest renderowane jako `src="/subapp_path/image.png"`. Oprogramowanie pośredniczące plików statycznych aplikacji głównego nie przetwarza żądanie plików statycznych. Żądanie jest przetwarzane przez oprogramowanie pośredniczące plików statycznych aplikacji podrzędnej.

Jeśli statycznych zasobów `src` atrybut jest ustawiony na ścieżkę bezwzględną (na przykład `src="/image.png"`), łącze jest renderowane bez pathbase aplikacji podrzędnej. Oprogramowanie pośredniczące plików statycznych aplikacji głównej próbuje obsługiwać zasobów z poziomu katalogu głównego aplikacji [katalog główny sieci web](xref:fundamentals/index#web-root), które powoduje *404 — Nie można odnaleźć* odpowiedzi, chyba że statyczny element zawartości jest dostępny z poziomu katalogu głównego aplikacji.

Do hostowania aplikacji ASP.NET Core jako aplikację podrzędne w ramach innej aplikacji platformy ASP.NET Core:

1. Ustanów pulę aplikacji do aplikacji podrzędnej. Ustaw **wersja środowiska .NET CLR** do **bez kodu zarządzanego**.

1. Dodawanie katalogu głównego witryny w Menedżerze usług IIS przy użyciu aplikacji podrzędne w folderze w katalogu głównego witryny.

1. Kliknij prawym przyciskiem myszy folder aplikacji podrzędnej w Menedżerze usług IIS, a następnie wybierz pozycję **Konwertuj na aplikację**.

1. W **Add Application** okno dialogowe, użyj **wybierz** przycisku **puli aplikacji** można przypisać puli aplikacji, który został utworzony dla aplikacji podrzędnej. Wybierz **OK**.

Przypisanie puli osobnych aplikacji do aplikacji podrzędnej jest wymagana, korzystając z modelu hostingu w procesie.

Aby uzyskać więcej informacji na temat w procesie model hostingu i konfigurowania modułu ASP.NET Core, zobacz <xref:host-and-deploy/aspnet-core-module> i <xref:host-and-deploy/aspnet-core-module>.

## <a name="configuration-of-iis-with-webconfig"></a>Konfiguracja programu IIS z pliku web.config

Ma wpływ konfiguracji programu IIS `<system.webServer>` części *web.config* do scenariuszy dotyczących usług IIS, które będą funkcjonalne dla aplikacji platformy ASP.NET Core przy użyciu modułu ASP.NET Core. Na przykład konfiguracji programu IIS działa dla kompresji dynamicznej. Jeśli usługi IIS zostały skonfigurowane na poziomie serwera, aby skorzystać z kompresji dynamicznej `<urlCompression>` elementu w aplikacji *web.config* pliku ją wyłączyć dla aplikacji ASP.NET Core.

Aby uzyskać więcej informacji, zobacz [informacje o konfiguracji dla \<system.webServer >](/iis/configuration/system.webServer/), [odwołania do konfiguracji modułu platformy ASP.NET Core](xref:host-and-deploy/aspnet-core-module), i [moduły usług IIS za pomocą platformy ASP.NET Podstawowe](xref:host-and-deploy/iis/modules). Aby ustawić zmienne środowiskowe dla poszczególnych aplikacji, działających w pulach aplikacji izolowanej (obsługiwane dla usługi IIS 10.0 lub nowszy), zobacz *polecenia AppCmd.exe* części [zmienne środowiskowe \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) dokumentację referencyjną tematu w usługach IIS.

## <a name="configuration-sections-of-webconfig"></a>Sekcji konfiguracyjnych w pliku Web.config

Konfiguracja części aplikacji w technologii ASP.NET 4.x w *web.config* nie są używane przez aplikacje platformy ASP.NET Core dla konfiguracji:

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

Aplikacje platformy ASP.NET Core są skonfigurowane przy użyciu innych dostawców konfiguracji. Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Pule aplikacji

::: moniker range=">= aspnetcore-2.2"

Izolacja puli aplikacji jest określany przez model hostowania:

* Hosting w trakcie &ndash; aplikacje są wymagane do uruchamiania w aplikacji w osobnych pulach.
* Spoza procesu hostingu &ndash; zalecamy Izolowanie aplikacji ze sobą, uruchamiając każdej aplikacji w puli aplikacji.

Usługi IIS **Dodawanie witryny sieci Web** okno dialogowe Ustawienia domyślne puli jednej aplikacji na aplikację. Gdy **Nazwa lokacji** zostanie podana, tekst jest automatycznie przenoszona do **puli aplikacji** pola tekstowego. Tworzona jest nowa pula aplikacji, przy użyciu nazwy lokacji po dodaniu lokacji.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

W przypadku hostowania wielu witryn sieci Web na serwerze, firma Microsoft zaleca izolowania aplikacji od siebie nawzajem, uruchamiając każdej aplikacji w puli aplikacji. Usługi IIS **Dodawanie witryny sieci Web** okno dialogowe Ustawienia domyślne w tej konfiguracji. Gdy **Nazwa lokacji** zostanie podana, tekst jest automatycznie przenoszona do **puli aplikacji** pola tekstowego. Tworzona jest nowa pula aplikacji, przy użyciu nazwy lokacji po dodaniu lokacji.

::: moniker-end

## <a name="application-pool-identity"></a>Tożsamość puli aplikacji

Tożsamość konta puli aplikacji umożliwia aplikację do uruchamiania w ramach unikatowe konto bez konieczności tworzenia i zarządzania nimi, domeny lub lokalnego konta. W programie IIS 8.0 lub nowszym procesu roboczego administratora usług IIS (WAS) tworzy konto wirtualnego o nazwie nowej puli aplikacji i aplikacji w puli procesów roboczych w ramach tego konta są domyślnie uruchamiane. W konsoli zarządzania usług IIS w obszarze **Zaawansowane ustawienia** dla puli aplikacji, upewnij się, że **tożsamości** jest skonfigurowany do używania **ApplicationPoolIdentity**:

![Okno dialogowe Zaawansowane ustawienia puli aplikacji](index/_static/apppool-identity.png)

Proces zarządzania usług IIS tworzy bezpieczny identyfikator, nazwę puli aplikacji w systemie Windows zabezpieczeń. Zasoby mogą być chronione przy użyciu tej tożsamości. Jednak ta tożsamość nie ma konta użytkowników i nie jest wyświetlane w konsoli zarządzania użytkownika Windows.

Jeśli proces roboczy usług IIS wymaga podwyższonego poziomu dostępu do aplikacji, należy zmodyfikować listę kontroli dostępu (ACL) do katalogu zawierającego aplikację:

1. Otwórz Eksploratora Windows i przejdź do katalogu.

1. Kliknij prawym przyciskiem myszy katalog i wybierz pozycję **właściwości**.

1. W obszarze **zabezpieczeń** zaznacz **Edytuj** przycisku i następnie **Dodaj** przycisku.

1. Wybierz **lokalizacje** przycisk i upewnij się, że wybrano system.

1. Wprowadź **puli aplikacji IIS\\< app_pool_name >** w **wprowadź nazwy obiektów do wybrania** obszaru. Wybierz **Sprawdź nazwy** przycisku. Aby uzyskać *DefaultAppPool* Sprawdź nazwy przy użyciu **IIS AppPool\DefaultAppPool**. Gdy **Sprawdź nazwy** przycisk jest zaznaczony, wartość **DefaultAppPool** podane w obszarze nazwy obiektu. Nie można wprowadzić nazwę puli aplikacji bezpośrednio do obszaru nazw obiektów. Użyj **puli aplikacji IIS\\< app_pool_name >** formatowania podczas sprawdzania dostępności nazwy obiektu.

   ![Wybierz użytkowników lub grup okno dialogowe folderu aplikacji: Nazwa puli aplikacji "DefaultAppPool" jest dołączany do "puli aplikacji IIS\" w obszarze nazwy obiektu przed wybraniem opcji"Sprawdź nazwy".](index/_static/select-users-or-groups-1.png)

1. Kliknij przycisk **OK**.

   ![Wybierz użytkowników lub grup okno dialogowe folderu aplikacji: Po wybraniu pozycji "Sprawdź nazwy", nazwa obiektu "DefaultAppPool" jest wyświetlany w obszarze nazwy obiektu.](index/_static/select-users-or-groups-2.png)

1. Odczyt &amp; wykonania uprawnienia domyślne. Podaj dodatkowe uprawnienia, stosownie do potrzeb.

Można również przyznawać dostęp przy użyciu wiersza polecenia **ICACLS** narzędzia. Za pomocą *DefaultAppPool* przykład służy następujące polecenie:

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

Aby uzyskać więcej informacji, zobacz [icacls](/windows-server/administration/windows-commands/icacls) tematu.

## <a name="http2-support"></a>Obsługa protokołu HTTP/2

::: moniker range=">= aspnetcore-2.2"

[Protokołu HTTP/2](https://httpwg.org/specs/rfc7540.html) jest obsługiwana za pomocą programu ASP.NET Core w następujących scenariuszach wdrażania usług IIS:

* W trakcie
  * Windows Server 2016 i Windows 10 lub nowszym; Usługi IIS 10 lub nowszym
  * Protokół TLS 1.2 lub nowszej połączenia
* Spoza procesu
  * Windows Server 2016 i Windows 10 lub nowszym; Usługi IIS 10 lub nowszym
  * Połączenia z serwerem usługi edge publicznego służy połączenia zwrotnego serwera proxy protokołu HTTP/2 [serwera Kestrel](xref:fundamentals/servers/kestrel) korzysta z protokołu HTTP/1.1.
  * Protokół TLS 1.2 lub nowszej połączenia

W procesie wdrożenia po nawiązaniu połączenia protokołu HTTP/2 [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporty `HTTP/2`. Spoza procesu wdrożenia po nawiązaniu połączenia protokołu HTTP/2 [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporty `HTTP/1.1`.

Aby uzyskać więcej informacji o modelach hostingu w procesie i poza procesem, zobacz <xref:host-and-deploy/aspnet-core-module> tematu i <xref:host-and-deploy/aspnet-core-module>.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[Protokołu HTTP/2](https://httpwg.org/specs/rfc7540.html) jest obsługiwana w przypadku wdrożeń poza procesem, które spełniają następujące wymagania podstawowy:

* Windows Server 2016 i Windows 10 lub nowszym; Usługi IIS 10 lub nowszym
* Połączenia z serwerem usługi edge publicznego służy połączenia zwrotnego serwera proxy protokołu HTTP/2 [serwera Kestrel](xref:fundamentals/servers/kestrel) korzysta z protokołu HTTP/1.1.
* Lokalizacja docelowa: Nie dotyczy wdrożeń spoza procesu, ponieważ połączenie HTTP/2 jest obsługiwane wyłącznie przez usługi IIS.
* Protokół TLS 1.2 lub nowszej połączenia

Jeśli zostanie nawiązane połączenie HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporty `HTTP/1.1`.

::: moniker-end

Protokołu HTTP/2 jest domyślnie włączona. Jeśli nie jest nawiązane połączenie HTTP/2, połączenia wrócić do protokołu HTTP/1.1. Aby uzyskać więcej informacji na temat konfiguracji protokołu HTTP/2 z wdrożeniami usług IIS, zobacz [protokołu HTTP/2 w programie IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).

## <a name="cors-preflight-requests"></a>Żądania wstępnego CORS

*Ta sekcja dotyczy tylko aplikacji platformy ASP.NET Core środowiska .NET Framework.*

Dla aplikacji ASP.NET Core przeznaczonego programu .NET Framework, opcje żądania nie są przekazywane do aplikacji domyślnie w usługach IIS. Informacje na temat konfigurowania obsługi usług IIS aplikacji w *web.config* do przekazania żądania OPTIONS, zobacz [Włączanie żądań cross-origin w programie ASP.NET Web API 2: Jak działa CORS](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).

## <a name="deployment-resources-for-iis-administrators"></a>Zasoby dotyczące wdrażania dla administratorów usług IIS

Więcej informacji na temat usług IIS szczegółowe w dokumentacji usług IIS.  
[Dokumentacja usług IIS](/iis)

Dowiedz się więcej na temat modeli wdrażania aplikacji .NET Core.  
[Wdrażanie aplikacji .NET core](/dotnet/core/deploying/)

Dowiedz się, jak modułu ASP.NET Core zezwala na serwerze sieci web Kestrel do używania usług IIS lub IIS Express jako serwera zwrotny serwer proxy.  
[Moduł ASP.NET Core](xref:host-and-deploy/aspnet-core-module)

Dowiedz się, jak skonfigurować modułu ASP.NET Core do hostowania aplikacji platformy ASP.NET Core.  
[Odwołania do konfiguracji modułu platformy ASP.NET Core](xref:host-and-deploy/aspnet-core-module)

Więcej informacji na temat struktury katalogów opublikowane aplikacje platformy ASP.NET Core.  
[Struktura katalogów](xref:host-and-deploy/directory-structure)

Dowiedz się, aktywnych i nieaktywnych moduły IIS dla aplikacji platformy ASP.NET Core oraz jak zarządzać moduły usług IIS.  
[Moduły usług IIS](xref:host-and-deploy/iis/troubleshoot)

Dowiedz się, jak diagnozować problemy z wdrożeniami usług IIS aplikacji platformy ASP.NET Core.  
[Rozwiązywanie problemów](xref:host-and-deploy/iis/troubleshoot)

Rozróżnia typowych błędów, odnośnie do hostowania aplikacji platformy ASP.NET Core w usługach IIS.  
[Dokumentacja typowych błędów dla usług Azure App Service i IIS](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:test/troubleshoot>
* [Wprowadzenie do platformy ASP.NET Core](xref:index)
* [Witryna oficjalne Microsoft IIS](https://www.iis.net/)
* [Windows Server Technical Preview biblioteki zawartości](/windows-server/windows-server)
* [Protokołu HTTP/2 w programie IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>
