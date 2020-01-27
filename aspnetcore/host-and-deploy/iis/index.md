---
title: Host platformy ASP.NET Core na Windows za pomocą programu IIS
author: guardrex
description: Dowiedz się, jak hostować aplikacje platformy ASP.NET Core na systemu Windows serwera Internet Information Services (IIS).
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
uid: host-and-deploy/iis/index
ms.openlocfilehash: 146a204509856186a2696b770cae2249d348fa34
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726837"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Host platformy ASP.NET Core na Windows za pomocą programu IIS

Przez [Luke Latham](https://github.com/guardrex)

Aby zapoznać się z samouczkiem dotyczącym publikowania aplikacji ASP.NET Core na serwerze usług IIS, zobacz <xref:tutorials/publish-to-iis>.

[Zainstaluj program .NET Core hostingu pakietu](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a>Supported operating systems

Obsługiwane są następujące systemy operacyjne:

::: moniker range=">= aspnetcore-3.0"

* Windows 7 lub nowszy
* System Windows Server 2012 R2 lub nowszy

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Windows 7 lub nowszy
* Windows Server 2008 R2 lub nowszy

::: moniker-end

[Serwer http. sys](xref:fundamentals/servers/httpsys) (znany wcześniej jako webListener) nie działa w konfiguracji zwrotnego serwera proxy z usługami IIS. Użyj [serwera Kestrel](xref:fundamentals/servers/kestrel).

Aby uzyskać informacji na temat obsługi na platformie Azure, zobacz <xref:host-and-deploy/azure-apps/index>.

Aby uzyskać wskazówki dotyczące rozwiązywania problemów, zobacz <xref:test/troubleshoot>.

## <a name="supported-platforms"></a>Obsługiwane platformy

Obsługiwane są aplikacje opublikowane dla wdrożenia 32-bitowego (x86) lub 64-bitowego (x64). Wdróż aplikację 32-bitową z 32-bitową (x86) zestaw .NET Core SDK, chyba że aplikacja:

* Wymagana jest większa przestrzeń adresów pamięci wirtualnej dla aplikacji 64-bitowej.
* Wymaga większego rozmiaru stosu IIS.
* Ma 64-bitowe zależności natywne.

Aby opublikować aplikację 64-bitową, należy użyć 64-bitowej (x64) zestaw .NET Core SDK. 64-bitowy środowisko uruchomieniowe musi być obecny w systemie hosta.

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-models"></a>Modele hostingu

### <a name="in-process-hosting-model"></a>Model hostingu w procesie

Korzystając z hostingu w procesie, aplikacja ASP.NET Core jest uruchamiana w tym samym procesie co proces roboczy usług IIS. Hosting w procesie zapewnia lepszą wydajność w porównaniu z obsługą hostingu, ponieważ żądania nie są kierowane do serwera proxy za pośrednictwem karty sprzężenia zwrotnego, czyli interfejsu sieciowego, który zwraca wychodzący ruch sieciowy z powrotem do tego samego komputera. Usługi IIS obsługują zarządzanie procesami przy użyciu [usługi aktywacji procesów systemu Windows (was)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

[Moduł ASP.NET Core](xref:host-and-deploy/aspnet-core-module):

* Wykonuje inicjalizację aplikacji.
  * Ładuje [CoreCLR](/dotnet/standard/glossary#coreclr).
  * Wywołania `Program.Main`.
* Obsługuje okres istnienia żądania natywnego usług IIS.

Model hostingu w procesie nie jest obsługiwany w przypadku aplikacji ASP.NET Core przeznaczonych dla .NET Framework.

Na poniższym diagramie przedstawiono relację między usługami IIS, modułem ASP.NET Core i hostowaną w procesie aplikacją:

![Moduł ASP.NET Core w scenariuszu hostingu w procesie](index/_static/ancm-inprocess.png)

Żądanie dociera do sieci Web do sterownika HTTP. sys trybu jądra. Sterownik kieruje natywne żądanie do usług IIS na skonfigurowanym porcie witryny sieci Web, zwykle 80 (HTTP) lub 443 (HTTPS). Moduł ASP.NET Core odbiera żądanie natywne i przekazuje go do serwera HTTP usług IIS (`IISHttpServer`). Serwer HTTP IIS jest implementacją serwera w procesie dla usług IIS, która konwertuje żądanie z natywnego na zarządzane.

Po przetworzeniu żądania przez serwer HTTP IIS żądanie jest wypychane do potoku ASP.NET Core pośredniczącego. Potok oprogramowania pośredniczącego obsługuje żądanie i przekazuje go jako wystąpienie `HttpContext` do logiki aplikacji. Odpowiedź aplikacji jest przesyłana z powrotem do usług IIS za pośrednictwem serwera HTTP IIS. Program IIS wysyła odpowiedź do klienta, który zainicjował żądanie.

Hosting w procesie jest nieobecny w przypadku istniejących aplikacji, ale w przypadku wszystkich scenariuszy z usługami w ramach programu z obsługą IIS Express usług w toku są domyślnie obsługiwane [nowe](/dotnet/core/tools/dotnet-new) szablony.

`CreateDefaultBuilder` dodaje wystąpienie <xref:Microsoft.AspNetCore.Hosting.Server.IServer>, wywołując metodę <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> w celu rozruchu [CoreCLR](/dotnet/standard/glossary#coreclr) i hostowania aplikacji w procesie roboczym usług IIS (*w3wp. exe* lub *iisexpress. exe*). Testy wydajności wskazują, że hostowanie platformy .NET Core app w procesie zapewnia znacznie większą przepływność żądania, w porównaniu do hostowania aplikacji spoza procesu i pośredniczenie żądania do [Kestrel](xref:fundamentals/servers/kestrel) serwera.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

> [!NOTE]
> Aplikacje publikowane jako pojedynczy plik wykonywalny nie mogą zostać załadowane przez model hostingu w procesie.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="out-of-process-hosting-model"></a>Model hostingu poza procesem

Ponieważ ASP.NET Core aplikacje działają w procesie innym niż proces roboczy usług IIS, moduł ASP.NET Core obsługuje zarządzanie procesami. Moduł uruchamia proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie zostanie odebrane i ponownie uruchomiony, jeśli zostanie zamknięty lub ulegnie awarii. Jest to zasadniczo takie samo zachowanie jak w przypadku aplikacji uruchamianych w procesie, które są zarządzane przez [usługę aktywacji procesów systemu Windows (was)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Na poniższym diagramie przedstawiono relację między usługami IIS, modułem ASP.NET Core i hostowanym przez aplikację aplikacją:

![Moduł ASP.NET Core w scenariuszu hostingu poza procesem](index/_static/ancm-outofprocess.png)

Żądania docierają do sieci Web do sterownika HTTP. sys trybu jądra. Sterownik kieruje żądania do usług IIS na skonfigurowanym porcie witryny sieci Web, zwykle 80 (HTTP) lub 443 (HTTPS). Moduł przekazuje żądania do Kestrel na losowo wybranym porcie dla aplikacji, która nie jest portem 80 lub 443.

Moduł określa port za pośrednictwem zmiennej środowiskowej podczas uruchamiania, a rozszerzenie <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> konfiguruje serwer do nasłuchiwania na `http://localhost:{PORT}`. Dodatkowe sprawdzenia są wykonywane, a żądania, które nie pochodzą z modułu, są odrzucane. Moduł nie obsługuje przekazywania HTTPS, dlatego żądania są przekazywane przez protokół HTTP nawet wtedy, gdy są odbierane przez usługę IIS przez protokół HTTPS.

Po podaniu przez Kestrel żądania z modułu żądanie jest wypychane do potoku ASP.NET Core pośredniczącego. Potok oprogramowania pośredniczącego obsługuje żądanie i przekazuje go jako wystąpienie `HttpContext` do logiki aplikacji. Oprogramowanie pośredniczące dodane przez integrację usług IIS aktualizuje schemat, zdalny adres IP i pathbase, aby można było przesłać żądanie do Kestrel. Odpowiedź aplikacji jest przesyłana z powrotem do usług IIS, która wypycha ją z powrotem do klienta HTTP, który zainicjował żądanie.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

ASP.NET Core jest dostarczany z [serwerem Kestrel](xref:fundamentals/servers/kestrel), a domyślnym serwerem HTTP na wielu platformach.

W przypadku korzystania z [usług IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) lub [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)aplikacja działa w procesie innym niż proces roboczy usług IIS (*poza procesem*) z [serwerem Kestrel](xref:fundamentals/servers/index#kestrel).

Ponieważ ASP.NET Core aplikacje działają w procesie innym niż proces roboczy usług IIS, moduł obsługuje zarządzanie procesami. Moduł uruchamia proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie zostanie odebrane i ponownie uruchomiony, jeśli zostanie zamknięty lub ulegnie awarii. Jest to zasadniczo takie samo zachowanie jak w przypadku aplikacji uruchamianych w procesie, które są zarządzane przez [usługę aktywacji procesów systemu Windows (was)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Na poniższym diagramie przedstawiono relację między usługami IIS, modułem ASP.NET Core i hostowanym przez aplikację aplikacją:

![Moduł ASP.NET Core](index/_static/ancm-outofprocess.png)

Żądania docierają do sieci Web do sterownika HTTP. sys trybu jądra. Sterownik kieruje żądania do usług IIS na skonfigurowanym porcie witryny sieci Web, zwykle 80 (HTTP) lub 443 (HTTPS). Moduł przekazuje żądania do Kestrel na losowo wybranym porcie dla aplikacji, która nie jest portem 80 lub 443.

Moduł określa port za pośrednictwem zmiennej środowiskowej podczas uruchamiania, a [oprogramowanie pośredniczące integracji usług IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) konfiguruje serwer do nasłuchiwania na `http://localhost:{port}`. Dodatkowe sprawdzenia są wykonywane, a żądania, które nie pochodzą z modułu, są odrzucane. Moduł nie obsługuje przekazywania HTTPS, dlatego żądania są przekazywane przez protokół HTTP nawet wtedy, gdy są odbierane przez usługę IIS przez protokół HTTPS.

Po podaniu przez Kestrel żądania z modułu żądanie jest wypychane do potoku ASP.NET Core pośredniczącego. Potok oprogramowania pośredniczącego obsługuje żądanie i przekazuje go jako wystąpienie `HttpContext` do logiki aplikacji. Oprogramowanie pośredniczące dodane przez integrację usług IIS aktualizuje schemat, zdalny adres IP i pathbase, aby można było przesłać żądanie do Kestrel. Odpowiedź aplikacji jest przesyłana z powrotem do usług IIS, która wypycha ją z powrotem do klienta HTTP, który zainicjował żądanie.

`CreateDefaultBuilder` Konfiguruje [Kestrel](xref:fundamentals/servers/kestrel) serwera jako serwera sieci web i umożliwia integrację usług IIS, konfigurując ścieżki podstawowej i port [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

Modułu ASP.NET Core generuje portów dynamicznych do przypisania do procesu zaplecza. `CreateDefaultBuilder` wywołania <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> metody. `UseIISIntegration` Konfiguruje usługi Kestrel do nasłuchiwania na port dynamiczny adres IP hosta lokalnego (`127.0.0.1`). Jeśli port dynamiczny jest 1234, Kestrel nasłuchuje na `127.0.0.1:1234`. Ta konfiguracja zastępuje inne konfiguracje adresu URL, dostarczone przez:

* `UseUrls`
* [Interfejs API nasłuchiwania kestrel firmy](xref:fundamentals/servers/kestrel#endpoint-configuration)
* [Konfiguracja](xref:fundamentals/configuration/index) (lub [opcji wiersza polecenia — adresy URL](xref:fundamentals/host/web-host#override-configuration))

Wywołania `UseUrls` lub jego Kestrel `Listen` interfejsu API nie są wymagane w przypadku korzystania z modułu. Jeśli `UseUrls` lub `Listen` jest wywoływane Kestrel nasłuchuje na porcie określony tylko podczas uruchamiania aplikacji bez usług IIS.

::: moniker-end

Aby uzyskać wskazówki dotyczące konfiguracji modułu ASP.NET Core, zobacz <xref:host-and-deploy/aspnet-core-module>.

Aby uzyskać więcej informacji o hostingu, zobacz [hostów w programie ASP.NET Core](xref:fundamentals/index#host).

## <a name="application-configuration"></a>Konfiguracja aplikacji

### <a name="enable-the-iisintegration-components"></a>Włącz składniki IISIntegration

::: moniker range=">= aspnetcore-3.0"

Podczas kompilowania hosta w `CreateHostBuilder` (*program.cs*) wywołaj <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, aby włączyć INTEGRACJĘ usług IIS:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        ...
```

Aby uzyskać więcej informacji na temat `CreateDefaultBuilder`, zobacz <xref:fundamentals/host/generic-host#default-builder-settings>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Podczas kompilowania hosta w `CreateWebHostBuilder` (*program.cs*) wywołaj <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, aby włączyć INTEGRACJĘ usług IIS:

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

Aby uzyskać więcej informacji na temat `CreateDefaultBuilder`, zobacz <xref:fundamentals/host/web-host#set-up-a-host>.

::: moniker-end

### <a name="iis-options"></a>Opcje programu IIS

::: moniker range=">= aspnetcore-2.2"

**W trakcie modelu hostingu**

Aby skonfigurować opcje serwera usług IIS, należy uwzględnić konfigurację usługi dla <xref:Microsoft.AspNetCore.Builder.IISServerOptions> w <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>. Poniższy przykład wyłącza AutomaticAuthentication:

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

| Opcja                         | Domyślny | Ustawienie |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | Jeśli `true`, ustawia serwer IIS `HttpContext.User` uwierzytelnione przez [uwierzytelniania Windows](xref:security/authentication/windowsauth). Jeśli `false`, serwer tylko zapewnia usługi tożsamości dla `HttpContext.User` i sprostać wymaganiom, gdy wyraźnie żąda przez `AuthenticationScheme`. Należy włączyć uwierzytelnianie Windows w usługach IIS dla `AutomaticAuthentication` funkcji. Aby uzyskać więcej informacji, zobacz [uwierzytelniania Windows](xref:security/authentication/windowsauth). |
| `AuthenticationDisplayName`    | `null`  | Określa nazwę wyświetlaną, widocznym dla użytkowników na stronach logowania. |
| `AllowSynchronousIO`           | `false` | Czy synchroniczna operacja we/wy jest dozwolona dla `HttpContext.Request` i `HttpContext.Response`. |
| `MaxRequestBodySize`           | `30000000`  | Pobiera lub ustawia maksymalny rozmiar treści żądania dla `HttpRequest`. Należy zauważyć, że sama usługa IIS ma limit `maxAllowedContentLength`, który zostanie przetworzony przed ustawieniem `MaxRequestBodySize` w `IISServerOptions`. Zmiana `MaxRequestBodySize` nie wpłynie na `maxAllowedContentLength`. Aby zwiększyć `maxAllowedContentLength`, Dodaj wpis w *pliku Web. config* , aby ustawić `maxAllowedContentLength` wyższą wartość. Aby uzyskać więcej informacji, zobacz [Konfiguracja](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration). |

**Model hostingu poza procesem**

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| Opcja                         | Domyślny | Ustawienie |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | Jeśli `true`, ustawia serwer IIS `HttpContext.User` uwierzytelnione przez [uwierzytelniania Windows](xref:security/authentication/windowsauth). Jeśli `false`, serwer tylko zapewnia usługi tożsamości dla `HttpContext.User` i sprostać wymaganiom, gdy wyraźnie żąda przez `AuthenticationScheme`. Należy włączyć uwierzytelnianie Windows w usługach IIS dla `AutomaticAuthentication` funkcji. Aby uzyskać więcej informacji, zobacz [uwierzytelniania Windows](xref:security/authentication/windowsauth). |
| `AuthenticationDisplayName`    | `null`  | Określa nazwę wyświetlaną, widocznym dla użytkowników na stronach logowania. |

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

**Model hostingu poza procesem**

::: moniker-end

Aby skonfigurować opcje usług IIS, należy uwzględnić konfigurację usługi dla <xref:Microsoft.AspNetCore.Builder.IISOptions> w <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>. Poniższy przykład uniemożliwia jej wypełnianie `HttpContext.Connection.ClientCertificate`:

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| Opcja                         | Domyślny | Ustawienie |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | Jeśli `true`, [oprogramowanie pośredniczące integracji usług IIS](#enable-the-iisintegration-components) ustawia `HttpContext.User` uwierzytelniane przy użyciu [uwierzytelniania systemu Windows](xref:security/authentication/windowsauth). Jeśli `false`, oprogramowanie pośredniczące tylko zapewnia usługi tożsamości dla `HttpContext.User` i sprostać wymaganiom, gdy wyraźnie żąda przez `AuthenticationScheme`. Należy włączyć uwierzytelnianie Windows w usługach IIS dla `AutomaticAuthentication` funkcji. Aby uzyskać więcej informacji, zobacz [uwierzytelniania Windows](xref:security/authentication/windowsauth) tematu. |
| `AuthenticationDisplayName`    | `null`  | Określa nazwę wyświetlaną, widocznym dla użytkowników na stronach logowania. |
| `ForwardClientCertificate`     | `true`  | Jeśli `true` i `MS-ASPNETCORE-CLIENTCERT` nagłówek żądania jest obecny, `HttpContext.Connection.ClientCertificate` jest wypełnione. |

### <a name="proxy-server-and-load-balancer-scenarios"></a>Serwer proxy i scenariuszy usługi równoważenia obciążenia

[Oprogramowanie pośredniczące integracji usług IIS](#enable-the-iisintegration-components), które konfiguruje przekazane nagłówki oprogramowania pośredniczącego, a moduł ASP.NET Core jest skonfigurowany do przesyłania dalej schematu (http/https) i zdalnego adresu IP, z którego pochodzi żądanie. Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych za serwery proxy dodatkowe i moduły równoważenia obciążenia. Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).

### <a name="webconfig-file"></a>plik Web.config

*Web.config* plik konfiguruje [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module). Tworzenie, przekształcania i publikowania *web.config* pliku jest obsługiwane przez obiekt docelowy programu MSBuild (`_TransformWebConfig`) po opublikowaniu projektu. Ten element docelowy znajduje się w elementy docelowe zestawu SDK sieci Web (`Microsoft.NET.Sdk.Web`). Zestaw SDK jest ustawiony w górnej części pliku projektu:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

Jeśli plik *Web. config* nie jest obecny w projekcie, plik zostanie utworzony przy użyciu poprawnych *processPath* i *argumentów* w celu skonfigurowania modułu ASP.NET Core i przesunięcia do [publikowanych danych wyjściowych](xref:host-and-deploy/directory-structure).

Jeśli *web.config* plik znajduje się w projekcie, plik jest przekształcana z prawidłowymi *processPath* i *argumenty* do skonfigurowania modułu ASP.NET Core przeniesiona do ikony opublikowane dane wyjściowe. Przekształcenie nie zmodyfikować ustawień konfiguracji usług IIS w pliku.

*Web.config* plik mogą dostarczać dodatkowych ustawień konfiguracji usług IIS, które kontrolują aktywne moduły usług IIS. Aby uzyskać informacji na temat moduły usług IIS, które są w stanie przetwarzania żądań z aplikacji platformy ASP.NET Core, zobacz [moduły usług IIS](xref:host-and-deploy/iis/modules) tematu.

Aby zapobiec przekształcania zestawu SDK sieci Web *web.config* pliku, użyj **\<IsTransformWebConfigDisabled >** właściwość w pliku projektu:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Zestaw SDK sieci Web z transformacji pliku, wyłączając *processPath* i *argumenty* należy ręcznie ustawić przez dewelopera. Aby uzyskać więcej informacji, zobacz temat <xref:host-and-deploy/aspnet-core-module>.

### <a name="webconfig-file-location"></a>Lokalizacja pliku Web.config

Aby poprawnie skonfigurować [moduł ASP.NET Core](xref:host-and-deploy/aspnet-core-module) , plik *Web. config* musi znajdować się w ścieżce [katalogu głównego zawartości](xref:fundamentals/index#content-root) (zazwyczaj ścieżka podstawowa aplikacji) wdrożonej aplikacji. Jest to tej samej lokalizacji co ścieżka fizyczna witryny sieci Web dostarczone do usług IIS. *Web.config* plik jest wymagany w katalogu głównym aplikacji, aby umożliwić publikowanie wielu aplikacji za pomocą narzędzia Web Deploy.

Poufne pliki istnieją na ścieżkę fizyczną aplikacji, takich jak  *\<zestawu >. runtimeconfig.json*,  *\<zestawu > .xml* (komentarze dokumentacji XML), a  *\<zestawu >. deps.json*. Gdy plik *Web. config* jest obecny, a lokacja jest uruchamiana normalnie, usługi IIS nie będą obsługiwały tych poufnych plików, jeśli są żądane. Jeśli *web.config* brakuje pliku, niepoprawnie o nazwie lub nie można skonfigurować witrynę podczas normalnego uruchamiania, usług IIS może obsługiwać poufnych plików publicznie.

**Plik *Web. config* musi być obecny we wdrożeniu przez cały czas, prawidłowo nazwany i można skonfigurować lokację pod kątem normalnego uruchamiania. Nigdy nie należy usuwać pliku *Web. config* z wdrożenia produkcyjnego.**

### <a name="transform-webconfig"></a>Przekształcanie pliku web.config

Jeśli musisz przekształcić *plik Web. config* przy publikowaniu (na przykład ustawić zmienne środowiskowe na podstawie konfiguracji, profilu lub środowiska), zobacz <xref:host-and-deploy/iis/transform-webconfig>.

## <a name="iis-configuration"></a>Konfiguracja programu IIS

**Systemy operacyjne Windows Server**

Włącz **serwer sieci Web (IIS)** roli serwera i ustanowić usług ról.

1. Użyj **Dodaj role i funkcje** kreatora z **Zarządzaj** menu lub linku w **Menedżera serwera**. Na **ról serwera** kroku, pole wyboru dla **serwer sieci Web (IIS)** .

   ![W kroku role serwera wybierz zaznaczona została rola Serwer sieci Web IIS.](index/_static/server-roles-ws2016.png)

1. Po **funkcji** kroku **usług ról** kroku ładuje dla serwera sieci Web (IIS). Wybierz potrzebnych usług roli usług IIS lub zaakceptuj domyślną rolę usług pod warunkiem.

   ![Domyślne usługi ról są wybrane w kroku wybierz rolę usług.](index/_static/role-services-ws2016.png)

   **Windows Authentication (opcjonalnie)**  
   Aby włączyć uwierzytelnianie Windows, rozwiń następujące węzły: **serwera sieci Web** > **zabezpieczeń**. Wybierz **uwierzytelniania Windows** funkcji. Aby uzyskać więcej informacji, zobacz [uwierzytelniania Windows \<windowsAuthentication >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) i [uwierzytelniania Windows skonfiguruj](xref:security/authentication/windowsauth).

   **Gniazda Websocket (opcjonalnie)**  
   Funkcja WebSockets jest obsługiwana przy użyciu platformy ASP.NET Core 1.1 lub nowszej. Aby włączyć protokół WebSockets, rozwiń następujące węzły: **serwera sieci Web** > **opracowywanie aplikacji**. Wybierz **protokołu WebSocket** funkcji. Aby uzyskać więcej informacji, zobacz [WebSockets](xref:fundamentals/websockets).

1. Postępuj zgodnie z instrukcjami **potwierdzenie** krok w celu zainstalowania roli Serwer sieci web i usług. Ponowne uruchomienie serwera/IIS nie jest wymagane po zainstalowaniu **serwer sieci Web (IIS)** roli.

**Systemy operacyjne Windows**

Włącz **Konsola zarządzania usługami IIS** i **usługi World Wide Web**.

1. Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **Windows Włącz funkcje w lub wyłącz** (po lewej stronie ekranu).

1. Otwórz **Internetowe usługi informacyjne** węzła. Otwórz **narzędzia zarządzania siecią Web** węzła.

1. Pole wyboru dla **Konsola zarządzania usługami IIS**.

1. Pole wyboru dla **usługi World Wide Web**.

1. Zaakceptuj domyślne funkcje dla **usługi World Wide Web** lub dostosowywanie funkcji usług IIS.

   **Windows Authentication (opcjonalnie)**  
   Aby włączyć uwierzytelnianie Windows, rozwiń następujące węzły: **usługi World Wide Web** > **zabezpieczeń**. Wybierz **uwierzytelniania Windows** funkcji. Aby uzyskać więcej informacji, zobacz [uwierzytelniania Windows \<windowsAuthentication >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) i [uwierzytelniania Windows skonfiguruj](xref:security/authentication/windowsauth).

   **Gniazda Websocket (opcjonalnie)**  
   Funkcja WebSockets jest obsługiwana przy użyciu platformy ASP.NET Core 1.1 lub nowszej. Aby włączyć protokół WebSockets, rozwiń następujące węzły: **usługi World Wide Web** > **funkcje tworzenia aplikacji**. Wybierz **protokołu WebSocket** funkcji. Aby uzyskać więcej informacji, zobacz [WebSockets](xref:fundamentals/websockets).

1. Jeśli instalacja usług IIS wymaga ponownego uruchomienia komputera, uruchom ponownie system.

![Konsola zarządzania usługami IIS oraz usługi sieci Web są zaznaczone w funkcji Windows.](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a>Zainstaluj program .NET Core hostingu pakietu

Zainstaluj *hostingu pakietu programu .NET Core* przez system operacyjny. Pakiet instaluje .NET Core środowisko uruchomieniowe, biblioteki platformy .NET Core i [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module). Moduł umożliwia platformy ASP.NET Core w aplikacji do uruchamiania w tle usług IIS.

> [!IMPORTANT]
> Po zainstalowaniu pakietu hostowanie usług IIS wcześniejsze instalacji pakietu musi zostać naprawiony. Uruchom Instalatora pakietu hostingu ponownie po zainstalowaniu usług IIS.
>
> Jeśli pakiet hostingu jest instalowany po zainstalowaniu 64-bitowej (x64) wersji programu .NET Core, prawdopodobnie brakuje zestawów SDK ([nie wykryto żadnych zestawów SDK dla platformy .NET Core](xref:test/troubleshoot#no-net-core-sdks-were-detected)). Aby rozwiązać ten problem, zobacz <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.

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
> Niektóre instalatory zawierają wersje, osiągnęły ich koniec cyklu życia (ma), które nie są już obsługiwane przez firmę Microsoft. Aby uzyskać więcej informacji, zobacz [zasady pomocy technicznej](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).

### <a name="install-the-hosting-bundle"></a>Zainstaluj pakiet hostingu

1. Uruchom Instalatora na serwerze. Następujące parametry są dostępne podczas uruchamiania Instalatora z powłoki poleceń administratora:

   * `OPT_NO_ANCM=1` &ndash; Pomiń instalację modułu ASP.NET Core.
   * `OPT_NO_RUNTIME=1` &ndash; Pomiń instalację środowiska uruchomieniowego .NET Core. Używany, gdy serwer obsługuje tylko [wdrożenia samodzielne (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).
   * `OPT_NO_SHAREDFX=1` &ndash; Pomiń instalację struktury programu ASP.NET udostępnione (środowisko uruchomieniowe programu ASP.NET). Używany, gdy serwer obsługuje tylko [wdrożenia samodzielne (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).
   * `OPT_NO_X86=1` &ndash; Pomiń instalację x86 środowisk uruchomieniowych. Użyj tego parametru, Jeśli wiesz, że nie będziesz hostować aplikacji 32-bitowych. Jeśli w przyszłości będziesz hostować zarówno aplikacje 32-bitowe, jak i 64-bitowe, nie używaj tego parametru i zainstaluj oba środowiska uruchomieniowe.
   * `OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; wyłączyć sprawdzanie przy użyciu konfiguracji udostępnionej usług IIS, gdy konfiguracja udostępniona (*ApplicationHost. config*) znajduje się na tym samym komputerze, na którym zainstalowano program IIS. *Dostępne tylko w przypadku ASP.NET Core 2,2 lub nowszych instalatorów pakietu do obsługi.* Aby uzyskać więcej informacji, zobacz temat <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.
1. Uruchom ponownie system lub wykonaj następujące polecenia w powłoce poleceń:

   ```console
   net stop was /y
   net start w3svc
   ```
   Ponowne uruchomienie usług IIS przejmuje zmiany w systemie ścieżki, która jest zmienną środowiskową, wprowadzone przez Instalatora.

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core nie przyjmuje zachowania z przekazaniem do przodu dla wersji poprawki współużytkowanych pakietów platformy. Po uaktualnieniu udostępnionej platformy, instalując nowy pakiet hostingu, ponownie uruchom system lub wykonaj następujące polecenia w powłoce poleceń:

```console
net stop was /y
net start w3svc
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Nie jest konieczne ręczne zatrzymanie poszczególnych lokacji w usługach IIS podczas instalowania pakietu hostingu. Aplikacje hostowane (lokacje IIS) są ponownie uruchamiane po ponownym uruchomieniu usług IIS. Aplikacje są uruchamiane ponownie po otrzymaniu pierwszego żądania, w tym od [modułu inicjalizacji aplikacji](#application-initialization-module-and-idle-timeout).

ASP.NET Core przyjmuje zachowanie funkcji przekazywania do przodu dla wydań poprawek udostępnionych pakietów platformy. Po ponownym uruchomieniu aplikacji hostowanych przez usługi IIS przy użyciu usług IIS aplikacje są ładowane z najnowszymi wersjami poprawki pakietów, do których się odwołują, gdy otrzymają swoje pierwsze żądanie. Jeśli usługi IIS nie zostaną ponownie uruchomione, aplikacje ponownie uruchamiają się i wykazują zachowanie przekazujące, gdy procesy robocze są odtwarzane i otrzymują swoje pierwsze żądanie.

::: moniker-end

> [!NOTE]
> Aby uzyskać informacji na temat konfiguracji udostępnionej usług IIS, zobacz [modułu ASP.NET Core przy użyciu konfiguracji udostępnionej usług IIS](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Zainstaluj pakiet Webdeploy podczas publikowania za pomocą programu Visual Studio

W przypadku wdrażania aplikacji na serwerach z [narzędzia Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later), zainstaluj najnowszą wersję narzędzia Web Deploy na serwerze. Aby zainstalować narzędzie Web Deploy, należy użyć [Instalatora platformy sieci Web (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) lub uzyskać bezpośrednio z poziomu Instalatora [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717). Jest to preferowana metoda do użycia Instalatora WebPI. WebPI oferuje instalację autonomiczną i konfiguracji dla dostawców hostingu.

## <a name="create-the-iis-site"></a>Tworzenie witryny usług IIS

1. W systemie hostingu utworzyć folderu zawierającego pliki i foldery opublikowanych aplikacji. W poniższym kroku ścieżka folderu jest dostarczana do usług IIS jako ścieżka fizyczna do aplikacji. Aby uzyskać więcej informacji na temat folderu wdrożenia aplikacji i układu pliku, zobacz <xref:host-and-deploy/directory-structure>.

1. W Menedżerze usług IIS Otwórz węzeł serwera w panelu **połączenia** . Kliknij prawym przyciskiem myszy **witryn** folderu. Wybierz **Dodawanie witryny sieci Web** z menu kontekstowego.

1. Podaj **Nazwa lokacji** i ustaw **ścieżkę fizyczną** do folderu wdrożenia aplikacji. Podaj **powiązanie** konfiguracji i tworzyć witryny sieci Web, wybierając **OK**:

   ![Podaj nazwę lokacji, ścieżkę fizyczną i nazwy hosta w kroku Dodawanie witryny sieci Web.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > Powiązania najwyższego poziomu symbolu wieloznacznego (`http://*:80/` i `http://+:80`) powinien **nie** można użyć. Powiązania najwyższego poziomu symboli wieloznacznych otworzyć aplikację w celu luk w zabezpieczeniach. Dotyczy to silnych i słabych symboli wieloznacznych. Użyj nazwy hostów jawne, a nie symboli wieloznacznych. Powiązanie symbol wieloznaczny domeny podrzędnej (na przykład `*.mysub.com`) nie ma to zagrożenie bezpieczeństwa, jeśli możesz kontrolować domenę nadrzędną całego (w przeciwieństwie do `*.com`, który jest narażony). Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.

1. W obszarze węzła serwera wybierz **pul aplikacji**.

1. Kliknij prawym przyciskiem myszy pulę aplikacji witryny i wybierz **podstawowych ustawień** z menu kontekstowego.

1. W **edytowanie puli aplikacji** oknie **wersja środowiska .NET CLR** do **bez kodu zarządzanego**:

   ![Ustaw bez kodu zarządzanego dla wersji środowiska .NET CLR.](index/_static/edit-apppool-ws2016.png)

    Platforma ASP.NET Core działa w oddzielnym procesie i zarządza środowiska uruchomieniowego. ASP.NET Core nie bazują na ładowaniu środowiska CLR programu Desktop (.NET CLR),&mdash;rdzeń podstawowego środowiska uruchomieniowego języka wspólnego (CoreCLR) dla platformy .NET Core jest uruchamiany w celu hostowania aplikacji w procesie roboczym. Ustawienie **wersji środowiska .NET CLR** na **Brak kodu zarządzanego** jest opcjonalne, ale zalecane.

1. *Platforma ASP.NET Core 2,2 lub nowszej*: dla 64-bitowych (x 64) [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd) , który używa [modelu hostingu w trakcie](#in-process-hosting-model), Wyłącz pulę aplikacji dla procesów 32-bitowych (x 86).

   Na pasku bocznym **Akcje** Menedżera usług IIS > **Pule aplikacji**wybierz pozycję **Ustaw ustawienia domyślne puli aplikacji** lub **Zaawansowane**. Znajdź **Włącz 32-bitowych aplikacji** i ustaw wartość `False`. To ustawienie nie ma wpływu na aplikacje wdrożone dla [hostingu poza procesem](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).

1. Upewnij się, że tożsamość modelu procesu ma odpowiednie uprawnienia.

   Jeśli domyślna tożsamość puli aplikacji (**Model procesu** > **tożsamości**) została zmieniona z **ApplicationPoolIdentity** tożsamość, upewnij się, że nową tożsamość ma wymagane uprawnienia dostępu do folderu aplikacji, bazy danych i inne wymagane zasoby. Na przykład pula aplikacji wymaga dostępu odczytu i zapisu do folderów, gdzie aplikacja odczytuje i zapisuje pliki.

**Konfiguracja uwierzytelniania Windows (opcjonalnie)**  
Aby uzyskać więcej informacji, zobacz [uwierzytelniania Windows skonfiguruj](xref:security/authentication/windowsauth).

## <a name="deploy-the-app"></a>Wdrażanie aplikacji

Wdróż aplikację w folderze **ścieżki fizycznej** usług IIS, który został ustanowiony w sekcji [Tworzenie witryny usług IIS](#create-the-iis-site) . [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) jest zalecanym mechanizmem wdrażania, ale istnieje kilka opcji przeniesienia aplikacji z folderu *publikowania* projektu do folderu wdrożenia systemu hostingu.

### <a name="web-deploy-with-visual-studio"></a>Narzędzie Web Deploy w programie Visual Studio

Zobacz [profilów publikowania programu Visual Studio dla wdrożenia aplikacji platformy ASP.NET Core](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) tematu, aby dowiedzieć się, jak utworzyć profil publikowania do użycia za pomocą narzędzia Web Deploy. Jeśli dostawca hostingu udostępnia profil publikowania lub obsługę tworzenia aplikacji, pobierz swój profil i zaimportuj go za pomocą okna dialogowego **publikowania** programu Visual Studio:

![Strona okna dialogowego publikowania](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Narzędzie Web Deploy poza programem Visual Studio

[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) można również poza programem Visual Studio z poziomu wiersza polecenia. Aby uzyskać więcej informacji, zobacz [narzędzie Web Deployment](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Wdrażanie rozwiązania alternatywne w sieci Web

Użyj dowolnej z kilku metod, aby przenieść aplikację do systemu hostingu, takich jak ręczna kopia, [xcopy](/windows-server/administration/windows-commands/xcopy), [Robocopy](/windows-server/administration/windows-commands/robocopy)lub [PowerShell](/powershell/).

Aby uzyskać więcej informacji na temat wdrażania programu ASP.NET Core w usługach IIS, zobacz [zasoby dotyczące wdrażania dla administratorów usług IIS](#deployment-resources-for-iis-administrators) sekcji.

## <a name="browse-the-website"></a>Przeglądanie witryny sieci Web

Po wdrożeniu aplikacji w systemie hostingu należy wysłać żądanie do jednej z publicznych punktów końcowych aplikacji.

W poniższym przykładzie lokacja jest powiązana z **nazwą hosta** usług IIS `www.mysite.com` na **porcie** `80`. Wykonano żądanie `http://www.mysite.com`:

![Przeglądarka Microsoft Edge został załadowany strony uruchamiania usług IIS.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a>Pliki wdrożenia zablokowane

Pliki w folderze wdrażania są zablokowane, gdy aplikacja jest uruchomiona. Nie można zastąpić pliki zablokowane podczas wdrażania. Aby zwolnić pliki zablokowane w danym wdrożeniu, Zatrzymaj pulę aplikację za pomocą **jeden** z następujących metod:

* Użyj narzędzia Web Deploy i odwołania `Microsoft.NET.Sdk.Web` w pliku projektu. *App_offline.htm* plik zostanie umieszczony w folderze głównym katalogu aplikacji sieci web. Gdy plik jest obecny, modułu ASP.NET Core bez problemu zmieniała zamyka aplikację i służy *app_offline.htm* pliku podczas wdrażania. Aby uzyskać więcej informacji, zobacz [informacje o konfiguracji modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).
* Ręcznie zatrzymaj pulę aplikacji w Menedżerze usług IIS na serwerze.
* Porzuć *app_offline. htm* przy użyciu programu PowerShell (wymaga programu PowerShell 5 lub nowszego):

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

  Dla autonomicznej, bez webfarm instalacji usług IIS, [skrypt programu PowerShell do aprowizacji AutoGenKeys.ps1 ochrony danych](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) może służyć do każdej puli aplikacji używana z aplikacji ASP.NET Core. Ten skrypt tworzy klucz rejestru w rejestrze HKLM, który jest dostępny tylko dla konta procesu roboczego puli aplikacji w aplikacji. Klucze są szyfrowane za pomocą DPAPI za pomocą klucza komputera.

  W scenariuszach z farmami internetowymi można skonfigurować aplikację można użyć ścieżki UNC do przechowywania jego pierścień klucz ochrony danych. Domyślnie klucze ochrony danych nie są szyfrowane. Upewnij się, że uprawnienia do udziału sieciowego są ograniczone do konta Windows, którego aplikacja działa. X509 certyfikatu może służyć do ochrony kluczy w stanie spoczynku. Należy wziąć pod uwagę mechanizmu, aby zezwolić użytkownikom na przekazywanie certyfikatów: miejsce certyfikatów do zaufanego certyfikatu przez użytkownika, przechowywania i upewnij się, są one dostępne na wszystkich komputerach, którym jest uruchamiany aplikacji użytkownika. Zobacz [konfiguracji ochrony danych platformy ASP.NET Core](xref:security/data-protection/configuration/overview) Aby uzyskać szczegółowe informacje.

* **Konfigurowanie puli aplikacji usług IIS, aby załadować profil użytkownika**

  To ustawienie znajduje się w **Model procesu** sekcji **Zaawansowane ustawienia** dla puli aplikacji. Ustaw wartość opcji **Załaduj profil użytkownika** na `True`. Po ustawieniu na `True`klucze są przechowywane w katalogu profilu użytkownika i chronione za pomocą interfejsu DPAPI przy użyciu klucza specyficznego dla konta użytkownika. Klucze są utrwalane w folderze *% LocalAppData%/ASP.NET/dataprotection-Keys* .

  [Atrybut setProfileEnvironment](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) puli aplikacji również musi być włączony. Wartość domyślna `setProfileEnvironment` jest `true`. W niektórych scenariuszach (na przykład systemu operacyjnego Windows) `setProfileEnvironment` jest ustawiony na `false`. Jeśli klucze nie są przechowywane w katalogu profilu użytkownika zgodnie z oczekiwaniami:

  1. Przejdź do folderu *% windir%/system32/inetsrv/config*
  1. Otwórz plik *ApplicationHost. config* .
  1. Znajdź element `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>`.
  1. Upewnij się, że atrybut `setProfileEnvironment` nie istnieje, który jest wartością domyślną `true`, lub jawnie ustaw wartość atrybutu na `true`.

* **System plików do przechowywania klucza pierścienia**

  Kod aplikacji, aby dopasować [przy użyciu systemu plików do przechowywania klucza pierścień](xref:security/data-protection/configuration/overview). X509 należy używać certyfikatu do ochrony klucza pierścienia i upewnij się, certyfikat jest zaufany certyfikat. Jeśli certyfikat jest certyfikatem z podpisem własnym, umieść certyfikat w magazynie zaufanego certyfikatu głównego.

  Korzystając z usług IIS w ramach farmy sieci web:

  * Użyj udziału plików, które mogą uzyskiwać dostęp do wszystkich komputerów.
  * Wdrażanie X509 certyfikatu do każdej maszyny. Konfigurowanie [ochrony danych w kodzie](xref:security/data-protection/configuration/overview).

* **Ustaw zasady dla komputera w celu ochrony danych**

  System ochrony danych ma ograniczoną obsługę ustawiania domyślnych [komputera zasad](xref:security/data-protection/configuration/machine-wide-policy) dla wszystkich aplikacji korzystających z interfejsów API ochrony danych. Aby uzyskać więcej informacji, zobacz temat <xref:security/data-protection/introduction>.

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

Jeśli statycznych zasobów `src` atrybut jest ustawiony na ścieżkę bezwzględną (na przykład `src="/image.png"`), łącze jest renderowane bez pathbase aplikacji podrzędnej. Oprogramowanie pośredniczące pliku statycznego aplikacji głównej próbuje obsłużyć zasób z poziomu [głównego katalogu sieci Web](xref:fundamentals/index#web-root)aplikacji głównej, co spowoduje, że odpowiedź na *404 nie zostanie znaleziona* , chyba że statyczny zasób jest dostępny z poziomu aplikacji głównej.

Do hostowania aplikacji ASP.NET Core jako aplikację podrzędne w ramach innej aplikacji platformy ASP.NET Core:

1. Ustanów pulę aplikacji do aplikacji podrzędnej. Ustaw **wersję środowiska .NET CLR** na **Brak kodu zarządzanego** , ponieważ podstawowe środowisko uruchomieniowe języka wspólnego (CoreCLR) dla platformy .NET Core jest uruchomione w celu hostowania aplikacji w procesie roboczym, a nie na pulpicie CLR (.NET CLR).

1. Dodawanie katalogu głównego witryny w Menedżerze usług IIS przy użyciu aplikacji podrzędne w folderze w katalogu głównego witryny.

1. Kliknij prawym przyciskiem myszy folder aplikacji podrzędnej w Menedżerze usług IIS, a następnie wybierz pozycję **Konwertuj na aplikację**.

1. W **Add Application** okno dialogowe, użyj **wybierz** przycisku **puli aplikacji** można przypisać puli aplikacji, który został utworzony dla aplikacji podrzędnej. Wybierz **OK**.

Przypisanie puli osobnych aplikacji do aplikacji podrzędnej jest wymagana, korzystając z modelu hostingu w procesie.

Aby uzyskać więcej informacji na temat modelu hostingu w procesie i konfigurowania modułu ASP.NET Core, zobacz <xref:host-and-deploy/aspnet-core-module>.

## <a name="configuration-of-iis-with-webconfig"></a>Konfiguracja programu IIS z pliku web.config

Ma wpływ konfiguracji programu IIS `<system.webServer>` części *web.config* do scenariuszy dotyczących usług IIS, które będą funkcjonalne dla aplikacji platformy ASP.NET Core przy użyciu modułu ASP.NET Core. Na przykład konfiguracji programu IIS działa dla kompresji dynamicznej. Jeśli usługi IIS zostały skonfigurowane na poziomie serwera, aby skorzystać z kompresji dynamicznej `<urlCompression>` elementu w aplikacji *web.config* pliku ją wyłączyć dla aplikacji ASP.NET Core.

Więcej informacji znajduje się w następujących tematach:

* [Dokumentacja konfiguracyjna \<system. WebServer >](/iis/configuration/system.webServer/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>

Aby ustawić zmienne środowiskowe dla poszczególnych aplikacji, działających w pulach aplikacji izolowanej (obsługiwane dla usługi IIS 10.0 lub nowszy), zobacz *polecenia AppCmd.exe* części [zmienne środowiskowe \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) dokumentację referencyjną tematu w usługach IIS.

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

   ![Użytkownicy lub grupy, okno dialogowe wyboru folderu aplikacji: Nazwa puli aplikacji "DefaultAppPool" jest dołączany do "puli aplikacji IIS\" w obszarze nazwy obiektu przed wybraniem opcji"Sprawdź nazwy".](index/_static/select-users-or-groups-1.png)

1. Wybierz **OK**.

   ![Użytkownicy lub grupy, okno dialogowe wyboru folderu aplikacji: po wybraniu pozycji "Sprawdź nazwy", nazwa obiektu "DefaultAppPool" jest wyświetlany w obiekcie nazwy obszaru.](index/_static/select-users-or-groups-2.png)

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

Aby uzyskać więcej informacji na temat modeli hostingu w procesie i poza procesami, zobacz <xref:host-and-deploy/aspnet-core-module>.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[Protokołu HTTP/2](https://httpwg.org/specs/rfc7540.html) jest obsługiwana w przypadku wdrożeń poza procesem, które spełniają następujące wymagania podstawowy:

* Windows Server 2016 i Windows 10 lub nowszym; Usługi IIS 10 lub nowszym
* Połączenia z serwerem usługi edge publicznego służy połączenia zwrotnego serwera proxy protokołu HTTP/2 [serwera Kestrel](xref:fundamentals/servers/kestrel) korzysta z protokołu HTTP/1.1.
* Platforma docelowa: nie dotyczy wdrożeń spoza procesu, ponieważ połączenie HTTP/2 jest obsługiwane wyłącznie przez usługi IIS.
* Protokół TLS 1.2 lub nowszej połączenia

Jeśli zostanie nawiązane połączenie HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporty `HTTP/1.1`.

::: moniker-end

Protokołu HTTP/2 jest domyślnie włączona. Jeśli nie jest nawiązane połączenie HTTP/2, połączenia wrócić do protokołu HTTP/1.1. Aby uzyskać więcej informacji na temat konfiguracji protokołu HTTP/2 z wdrożeniami usług IIS, zobacz [protokołu HTTP/2 w programie IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).

## <a name="cors-preflight-requests"></a>Żądania inspekcji wstępnej CORS

*Ta sekcja dotyczy tylko ASP.NET Core aplikacji przeznaczonych dla .NET Framework.*

W przypadku aplikacji ASP.NET Core, która jest przeznaczona dla .NET Framework, żądania opcji nie są domyślnie przesyłane do aplikacji w usługach IIS. Aby dowiedzieć się, jak skonfigurować obsługę usług IIS aplikacji w *pliku Web. config* w celu przekazywania żądań dotyczących opcji, zobacz [Włączanie żądań między źródłami w programie ASP.NET Web API 2: jak działa mechanizm CORS](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).

::: moniker range=">= aspnetcore-2.2"

## <a name="application-initialization-module-and-idle-timeout"></a>Moduł inicjalizacji aplikacji i limit czasu bezczynności

Hostowane w usługach IIS przez moduł ASP.NET Core w wersji 2:

* [Moduł inicjalizacji aplikacji](#application-initialization-module) [&ndash; hostowanej lub pozaprocesowej](#in-process-hosting-model) aplikacji [można skonfigurować](#out-of-process-hosting-model) do automatycznego uruchamiania na potrzeby ponownego uruchomienia procesu roboczego lub ponownego uruchomienia serwera.
* [Limit czasu bezczynności](#idle-timeout) &ndash; hostowanej aplikacji można skonfigurować [w](#in-process-hosting-model) taki sposób, aby nie przekroczyć limitu czasu podczas okresów braku aktywności.

### <a name="application-initialization-module"></a>Moduł inicjalizacji aplikacji

*Dotyczy aplikacji hostowanych w procesie i pozaprocesowe.*

[Inicjowanie aplikacji IIS](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) to funkcja usług IIS, która wysyła do aplikacji żądanie HTTP, gdy pula aplikacji zostanie uruchomiona lub zostanie odtworzona. Żądanie wyzwala uruchomienie aplikacji. Domyślnie usługi IIS wysyłają żądanie do głównego adresu URL aplikacji (`/`) w celu zainicjowania aplikacji (zobacz [dodatkowe zasoby](#application-initialization-module-and-idle-timeout-additional-resources) , aby uzyskać więcej informacji na temat konfiguracji).

Upewnij się, że funkcja inicjowania roli inicjalizacji aplikacji IIS jest włączona:

Na komputerach z systemem Windows 7 lub nowszym w przypadku lokalnego korzystania z usług IIS:

1. Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **włączać lub wyłączać funkcje systemu Windows** (po lewej stronie ekranu).
1. Otwórz **Internet Information Services** > **World Wide Web Services** > **funkcje projektowania aplikacji**.
1. Zaznacz pole wyboru dla **inicjowania aplikacji**.

W systemie Windows Server 2008 R2 lub nowszym:

1. Otwórz **Kreatora dodawania ról i funkcji**.
1. W panelu **Wybierz usługi ról** Otwórz węzeł **Programowanie aplikacji** .
1. Zaznacz pole wyboru dla **inicjowania aplikacji**.

Użyj jednego z poniższych metod, aby włączyć moduł inicjowania aplikacji dla lokacji:

* Za pomocą Menedżera usług IIS:

  1. W panelu **połączenia** wybierz pozycję **Pule aplikacji** .
  1. Kliknij prawym przyciskiem myszy pulę aplikacji aplikacji na liście i wybierz pozycję **Ustawienia zaawansowane**.
  1. Domyślny **tryb uruchamiania** to **OnDemand**. Ustaw **tryb uruchamiania** na **AlwaysRunning**. Wybierz **OK**.
  1. Otwórz węzeł **Lokacje** w panelu **połączenia** .
  1. Kliknij prawym przyciskiem myszy aplikację i wybierz pozycję **Zarządzaj witryną sieci web** > **Ustawienia zaawansowane**.
  1. Domyślnym ustawieniem **wstępnego ładowania** jest **wartość false**. Ustaw dla opcji **wstępnego ładowania** **wartość true**. Wybierz **OK**.

* Korzystając z pliku *Web. config*, dodaj element `<applicationInitialization>` z `doAppInitAfterRestart` ustawionym na `true` do elementów `<system.webServer>` w plikach *Web. config* aplikacji:

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <location path="." inheritInChildApplications="false">
      <system.webServer>
        <applicationInitialization doAppInitAfterRestart="true" />
      </system.webServer>
    </location>
  </configuration>
  ```

### <a name="idle-timeout"></a>Limit czasu bezczynności

*Dotyczy tylko aplikacji hostowanych w procesie.*

Aby zapobiec przekroczeniu przez aplikację, należy ustawić limit czasu bezczynności puli aplikacji przy użyciu Menedżera usług IIS:

1. W panelu **połączenia** wybierz pozycję **Pule aplikacji** .
1. Kliknij prawym przyciskiem myszy pulę aplikacji aplikacji na liście i wybierz pozycję **Ustawienia zaawansowane**.
1. Domyślny **limit czasu bezczynności (w minutach)** wynosi **20** minut. Ustaw **limit czasu bezczynności (w minutach)** na **0** (zero). Wybierz **OK**.
1. Odtwórz proces roboczy.

Aby zapobiec przekroczeniu limitu [czasu hostowanych przez aplikacje](#out-of-process-hosting-model) aplikacji, użyj jednej z następujących metod:

* Wyślij polecenie ping do aplikacji z zewnętrznej usługi, aby było ono uruchomione.
* Jeśli aplikacja obsługuje tylko usługi w tle, należy unikać hostingu usług IIS i używać [usługi systemu Windows do hostowania aplikacji ASP.NET Core](xref:host-and-deploy/windows-service).

### <a name="application-initialization-module-and-idle-timeout-additional-resources"></a>Dodatkowe zasoby dotyczące modułu inicjalizacji aplikacji i limitu czasu bezczynności

* [Inicjowanie aplikacji usług IIS 8,0](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)
* [Inicjowanie aplikacji \<> applicationInitialization](/iis/configuration/system.webserver/applicationinitialization/).
* [Ustawienia modelu procesu dla puli aplikacji \<processModel >](/iis/configuration/system.applicationhost/applicationpools/add/processmodel).

::: moniker-end

## <a name="deployment-resources-for-iis-administrators"></a>Zasoby dotyczące wdrażania dla administratorów usług IIS

* [Dokumentacja usług IIS](/iis)
* [Wprowadzenie z menedżerem usług IIS w usługach IIS](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [Wdrażanie aplikacji .NET core](/dotnet/core/deploying/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:host-and-deploy/iis/modules>
* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:test/troubleshoot>
* <xref:index>
* [Witryna oficjalne Microsoft IIS](https://www.iis.net/)
* [Windows Server Technical Preview biblioteki zawartości](/windows-server/windows-server)
* [Protokołu HTTP/2 w programie IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>
