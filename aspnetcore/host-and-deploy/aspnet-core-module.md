---
title: Moduł ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować modułu ASP.NET Core do hostowania aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/17/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: d5392ff6b15eeb3a4502df578665538b936aae6f
ms.sourcegitcommit: 28a2874765cefe9eaa068dceb989a978ba2096aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67167067"
---
# <a name="aspnet-core-module"></a>Moduł ASP.NET Core

Przez [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [ Justin Kotalik](https://github.com/jkotalik), i [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-2.2"

Modułu ASP.NET Core jest moduł macierzysty usług IIS, które podłącza się do potoku usług IIS albo:

* Hostowanie aplikacji ASP.NET Core wewnątrz proces roboczy usług IIS (`w3wp.exe`), co jest nazywane [modelu hostingu w trakcie](#in-process-hosting-model).
* Przesyła żądania sieci web do uruchamiania aplikacji ASP.NET Core zaplecza [serwera Kestrel](xref:fundamentals/servers/kestrel), co jest nazywane [modelu hostingu poza procesem](#out-of-process-hosting-model).

Obsługiwane wersje Windows:

* Windows 7 lub nowszy
* Windows Server 2008 R2 lub nowszy

W przypadku hostowania w procesie, moduł używa implementacji w procesie serwera dla usług IIS, nazywany serwerem HTTP usług IIS (`IISHttpServer`).

Gdy w hostingu poza procesem, moduł działa tylko z Kestrel. Moduł nie jest zgodna z [HTTP.sys](xref:fundamentals/servers/httpsys).

## <a name="hosting-models"></a>Modele hostingu

### <a name="in-process-hosting-model"></a>W trakcie modelu hostingu

Aby skonfigurować aplikację do obsługi w procesie, Dodaj `<AspNetCoreHostingModel>` właściwość do pliku projektu aplikacji o wartości `InProcess` (spoza procesu hostingu została ustawiona za pomocą `OutOfProcess`):

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

Model hostingu w trakcie nie jest obsługiwana dla aplikacji platformy ASP.NET Core, które obsługują program .NET Framework.

Jeśli `<AspNetCoreHostingModel>` właściwość nie jest obecny w pliku, wartością domyślną jest `OutOfProcess`.

Następujące właściwości mają zastosowanie w przypadku hostowania w procesie:

* Serwer HTTP usług IIS (`IISHttpServer`) jest używany zamiast [Kestrel](xref:fundamentals/servers/kestrel) serwera. W procesie [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) wywołania <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> do:

  * Zarejestruj `IISHttpServer`.
  * Skonfiguruj port i ścieżka podstawowa serwera powinien nasłuchiwać podczas uruchamiania za modułu ASP.NET Core.
  * Konfigurowanie hosta do przechwytywania błędów uruchamiania.

* [Atrybut requestTimeout](#attributes-of-the-aspnetcore-element) nie ma zastosowania do hostowania w procesie.

* Udostępnianie puli aplikacji między aplikacjami nie jest obsługiwane. Użycie jednej puli aplikacji na aplikację.

* Korzystając z [narzędzia Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) lub ręczne wprowadzanie [plik app_offline.htm we wdrożeniu](xref:host-and-deploy/iis/index#locked-deployment-files), aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia. Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.

* Architektura (liczba bitów) zainstalowanego środowiska uruchomieniowego (x64 lub x86) i aplikacji musi być zgodna z architekturą puli aplikacji.

* Ustawiając hosta aplikacji ręcznie za pomocą `WebHostBuilder` (nie używa [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) i nigdy nie uruchomienia aplikacji bezpośrednio na serwerze Kestrel (Self-Hosted), wywołanie `UseKestrel` przed wywołaniem `UseIISIntegration`. Jeśli kolejność została odwrócona, host nie można uruchomić.

* Rozłącza klienta są wykrywane. [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) odwołano token anulowania, gdy klient odłączy się.

* W programie ASP.NET Core 2.2.1 lub wcześniej, <xref:System.IO.Directory.GetCurrentDirectory*> zwraca katalogu roboczego proces rozpoczęty przez usługi IIS, a nie w katalogu aplikacji (na przykład *C:\Windows\System32\inetsrv* dla *w3wp.exe*) .

  Przykładowy kod, który ustawia bieżący katalog aplikacji, zobacz [klasy CurrentDirectoryHelpers](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs). Wywołaj `SetCurrentDirectory` metody. Kolejne wywołania <xref:System.IO.Directory.GetCurrentDirectory*> zapewniają katalogu aplikacji.

* W przypadku hostowania w trakcie <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nie jest wewnętrznie wywoływana w celu zainicjowania przez użytkownika. W związku z tym <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementacji używanego do przekształcania oświadczeń, po każdym uwierzytelniania nie jest aktywowana domyślnie. Podczas transformowania oświadczenia, za pomocą <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementacji, wywołanie <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> nad dodaniem usług uwierzytelniania:

  ```csharp
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddTransient<IClaimsTransformation, ClaimsTransformer>();
      services.AddAuthentication(IISServerDefaults.AuthenticationScheme);
  }

  public void Configure(IApplicationBuilder app)
  {
      app.UseAuthentication();
  }
  ```

### <a name="out-of-process-hosting-model"></a>Model hostingu poza procesem

Aby skonfigurować aplikację dla hostingu poza procesem, użyj jednej z poniższych metod w pliku projektu:

* Nie określaj `<AspNetCoreHostingModel>` właściwości. Jeśli `<AspNetCoreHostingModel>` właściwość nie jest obecny w pliku, wartością domyślną jest `OutOfProcess`.
* Ustaw wartość `<AspNetCoreHostingModel>` właściwości `OutOfProcess` (hostowanie wewnątrzprocesowe została ustawiona za pomocą `InProcess`):

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

[Kestrel](xref:fundamentals/servers/kestrel) serwer jest używany zamiast serwera HTTP usług IIS (`IISHttpServer`).

Dla procesu,- [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) wywołania <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> do:

* Skonfiguruj port i ścieżka podstawowa serwera powinien nasłuchiwać podczas uruchamiania za modułu ASP.NET Core.
* Konfigurowanie hosta do przechwytywania błędów uruchamiania.

### <a name="hosting-model-changes"></a>Zmiany modelu hostingu

Jeśli `hostingModel` ustawienie to ulegnie zmianie w *web.config* pliku (wyjaśnione w [konfiguracji z pliku web.config](#configuration-with-webconfig) sekcji), moduł odtwarzania procesu roboczego programu IIS.

Dla usług IIS Express moduł nie odtworzyć proces roboczy, ale zamiast tego wyzwala łagodne zamykanie bieżący proces usług IIS Express. Kolejne żądanie aplikacji spowoduje utworzenie nowych procesów usług IIS Express.

### <a name="process-name"></a>Nazwa procesu

`Process.GetCurrentProcess().ProcessName` Raporty `w3wp` / `iisexpress` (w procesie) lub `dotnet` (poza procesem).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Modułu ASP.NET Core jest moduł macierzysty usług IIS, które podłącza się do potoku usług IIS do przekazywania żądań sieci web z zapleczem platformy ASP.NET Core z aplikacji.

Obsługiwane wersje Windows:

* Windows 7 lub nowszy
* Windows Server 2008 R2 lub nowszy

Moduł działa tylko z Kestrel. Moduł nie jest zgodna z [HTTP.sys](xref:fundamentals/servers/httpsys).

Ponieważ aplikacje platformy ASP.NET Core, uruchom w procesie oddzielić od proces roboczy usług IIS, moduł obsługuje także zarządzanie procesem. Moduł rozpoczyna się proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie dociera i ponownie uruchamia aplikację w przypadku jej awarii. Jest to zasadniczo takie samo zachowanie, ponieważ aplikacje ASP.NET 4.x, działających w trakcie w usługach IIS, które są zarządzane przez [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Na poniższym diagramie przedstawiono relację między usług IIS, modułu ASP.NET Core i aplikacji:

![Moduł ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

Żądania pojawić się w sieci Web w trybie jądra sterownik HTTP.sys. Sterownik kieruje żądania do usługi IIS w witrynie sieci Web skonfigurowanego portu, zwykle 80 (HTTP) lub 443 (HTTPS). Moduł przekazuje żądania do Kestrel na losowy port aplikacji, która nie jest port 80 i 443.

Moduł Określa port, za pośrednictwem zmiennej środowiskowej w momencie uruchamiania i [oprogramowania pośredniczącego integracji usługi IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) konfiguruje serwer do nasłuchiwania `http://localhost:{port}`. Wykonywane są dodatkowe czynności kontrolne i żądania, które nie pochodzą z modułu są odrzucane. Moduł nie obsługuje przekazywanie protokołu HTTPS, dlatego żądania są przekazywane za pośrednictwem protokołu HTTP, nawet wtedy, gdy odbierane przez usługi IIS przy użyciu protokołu HTTPS.

Po Kestrel przejmuje żądania z modułu, żądania są przesyłane do potoku oprogramowania pośredniczącego platformy ASP.NET Core. Potoku oprogramowania pośredniczącego obsługuje żądanie i przekazuje ją jako `HttpContext` wystąpienie aplikacji logiki. Oprogramowanie pośredniczące dodane przez usługi IIS integracji aktualizuje schemat, zdalny adres IP i pathbase w celu uwzględnienia przekazywania żądania do Kestrel. Odpowiedź aplikacji jest przekazywany z powrotem do usług IIS, wypchnięć, które go wycofać do klienta HTTP, który zainicjował żądanie.

::: moniker-end

Wiele modułów macierzystych, takie jak uwierzytelnianie Windows pozostają aktywne. Aby dowiedzieć się, jak aktywne moduły usług IIS przy użyciu modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/modules>.

Modułu ASP.NET Core może również:

* Ustaw zmienne środowiskowe dla procesu roboczego.
* Rejestrowanie strumienia stdout, dane wyjściowe do usługi file storage dotyczące rozwiązywania problemów, uruchamiania.
* Przekazuj tokeny uwierzytelniania Windows.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>Jak zainstalować i używać modułu ASP.NET Core

Aby uzyskać instrukcje dotyczące sposobu instalowania modułu ASP.NET Core, zobacz [jest instalowany pakiet hostingu platformy .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

## <a name="configuration-with-webconfig"></a>Konfiguracja z pliku web.config

Skonfigurowano modułu ASP.NET Core `aspNetCore` części `system.webServer` węzeł w tej witrynie *web.config* pliku.

Następujące *web.config* pliku została opublikowana na potrzeby [wdrożenia zależny od struktury](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje modułu ASP.NET Core do obsługi żądań w lokacji:

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet"
                  arguments=".\MyApp.dll"
                  stdoutLogEnabled="false"
                  stdoutLogFile=".\logs\stdout"
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet"
                arguments=".\MyApp.dll"
                stdoutLogEnabled="false"
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

Następujące *web.config* została opublikowana na potrzeby [niezależna wdrożenia](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe"
                  stdoutLogEnabled="false"
                  stdoutLogFile=".\logs\stdout"
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

<xref:System.Configuration.SectionInformation.InheritInChildApplications*> Właściwość jest ustawiona na `false` do wskazania, że ustawienia określone w [ \<lokalizacja >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) elementu nie są dziedziczone przez aplikacje, które znajdują się w podkatalogu aplikacji.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe"
                stdoutLogEnabled="false"
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

Gdy aplikacja jest wdrażana na [usługi Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` ścieżka jest równa `\\?\%home%\LogFiles\stdout`. Ścieżka zapisuje dzienniki stdout *LogFiles* folderu, w którym znajduje się automatycznie utworzone przez usługę.

Aby uzyskać informacji na temat konfigurowania aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications>.

### <a name="attributes-of-the-aspnetcore-element"></a>Atrybuty elementu aspNetCore

::: moniker range=">= aspnetcore-2.2"

| Atrybut | Opis | Domyślny |
| --------- | ----------- | :-----: |
| `arguments` | <p>Atrybut opcjonalny ciąg.</p><p>Argumenty do pliku wykonywalnego, określony w **processPath**.</p> | |
| `disableStartUpErrorPage` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</p> | `false` |
| `forwardWindowsAuthToken` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie. Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</p> | `true` |
| `hostingModel` | <p>Atrybut opcjonalny ciąg.</p><p>Określa modelu hostingu w trakcie (`InProcess`) lub spoza procesu (`OutOfProcess`).</p> | `OutOfProcess` |
| `processesPerApplication` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</p><p>&dagger;W trakcie hostingu, wartość jest ograniczona do `1`.</p><p>Ustawienie `processesPerApplication` jest niezalecane. Ten atrybut zostanie usunięta w przyszłej wersji.</p> | Wartość domyślna: `1`<br>Minimalna: `1`<br>Maks.: `100`&dagger; |
| `processPath` | <p>Atrybut wymagany ciąg.</p><p>Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP. Obsługiwane są ścieżki względne. Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</p> | |
| `rapidFailsPerMinute` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę. W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</p><p>Nieobsługiwane za pomocą wewnątrzprocesowego hostingu.</p> | Wartość domyślna: `10`<br>Minimalna: `0`<br>Maks.: `100` |
| `requestTimeout` | <p>Atrybut opcjonalny przedziału czasu.</p><p>Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</p><p>W wersjach modułu ASP.NET Core, dołączonej do wersji platformy ASP.NET Core 2.1 lub nowszej `requestTimeout` jest określona w godzinach, minutach i sekundach.</p><p>Nie ma zastosowania do hostowania w procesie. Dla hostingu w procesie modułu czeka na aplikację, aby przetworzyć żądanie.</p><p>Prawidłowe wartości minut i sekund segmentów ciągu należą do zakresu 0-59. Korzystanie z **60** wartości dla wyników w ciągu kilku minut i sekund w *500 — Wewnętrzny błąd serwera*.</p> | Wartość domyślna: `00:02:00`<br>Minimalna: `00:00:00`<br>Maks.: `360:00:00` |
| `shutdownTimeLimit` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</p> | Wartość domyślna: `10`<br>Minimalna: `0`<br>Maks.: `600` |
| `startupTimeLimit` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie. Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu. Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</p><p>Wartość 0 (zero) jest **nie** uważane za nieskończony limit czasu.</p> | Wartość domyślna: `120`<br>Minimalna: `0`<br>Maks.: `3600` |
| `stdoutLogEnabled` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Atrybut opcjonalny ciąg.</p><p>Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane. Są ścieżki względne względem katalogu głównego witryny. Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych. Wszystkie foldery w ścieżce są tworzone przez moduł, gdy plik dziennika jest tworzony. Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku ( *.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki. Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| Atrybut | Opis | Domyślny |
| --------- | ----------- | :-----: |
| `arguments` | <p>Atrybut opcjonalny ciąg.</p><p>Argumenty do pliku wykonywalnego, określony w **processPath**.</p>| |
| `disableStartUpErrorPage` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</p> | `false` |
| `forwardWindowsAuthToken` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie. Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</p> | `true` |
| `processesPerApplication` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</p><p>Ustawienie `processesPerApplication` jest niezalecane. Ten atrybut zostanie usunięta w przyszłej wersji.</p> | Wartość domyślna: `1`<br>Minimalna: `1`<br>Maks.: `100` |
| `processPath` | <p>Atrybut wymagany ciąg.</p><p>Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP. Obsługiwane są ścieżki względne. Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</p> | |
| `rapidFailsPerMinute` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę. W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</p> | Wartość domyślna: `10`<br>Minimalna: `0`<br>Maks.: `100` |
| `requestTimeout` | <p>Atrybut opcjonalny przedziału czasu.</p><p>Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</p><p>W wersjach modułu ASP.NET Core, dołączonej do wersji platformy ASP.NET Core 2.1 lub nowszej `requestTimeout` jest określona w godzinach, minutach i sekundach.</p> | Wartość domyślna: `00:02:00`<br>Minimalna: `00:00:00`<br>Maks.: `360:00:00` |
| `shutdownTimeLimit` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</p> | Wartość domyślna: `10`<br>Minimalna: `0`<br>Maks.: `600` |
| `startupTimeLimit` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie. Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu. Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</p><p>Wartość 0 (zero) jest **nie** uważane za nieskończony limit czasu.</p> | Wartość domyślna: `120`<br>Minimalna: `0`<br>Maks.: `3600` |
| `stdoutLogEnabled` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Atrybut opcjonalny ciąg.</p><p>Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane. Są ścieżki względne względem katalogu głównego witryny. Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych. Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika. Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku ( *.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki. Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a>Ustawianie zmiennych środowiskowych

::: moniker range=">= aspnetcore-3.0"

Zmienne środowiskowe można określić dla tego procesu w `processPath` atrybutu. Określ zmienną środowiskową `<environmentVariable>` element podrzędny elementu `<environmentVariables>` elementu kolekcji. Zmienne środowiskowe ustawione w tej sekcji pierwszeństwo systemowe zmienne środowiskowe.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Zmienne środowiskowe można określić dla tego procesu w `processPath` atrybutu. Określ zmienną środowiskową `<environmentVariable>` element podrzędny elementu `<environmentVariables>` elementu kolekcji.

> [!WARNING]
> Ustaw zmienne środowiskowe ustawione w tej sekcji wystąpienie konfliktu, za pomocą zmiennych środowiskowych systemu o takiej samej nazwie. Jeśli zmienna środowiskowa jest ustawiana w obu *web.config* plik, a w systemie poziomu w Windows, wartość *web.config* plik staje się dołączany do wartości zmiennej środowiskowej systemu (na potrzeby przykład `ASPNETCORE_ENVIRONMENT: Development;Development`), który uniemożliwia uruchomienie przez aplikację.

::: moniker-end

W poniższym przykładzie ustawiono dwóch zmiennych środowiskowych. `ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji `Development`. Deweloper może tymczasowo ustawić tę wartość w *web.config* pliku, aby wymusić [stronie wyjątków deweloperów](xref:fundamentals/error-handling) załadować podczas debugowania aplikacji wyjątek. `CONFIG_DIR` to przykład zmiennej środowiskowej zdefiniowanej przez użytkownika, gdzie deweloper ma napisany kod, który odczytuje wartość przy uruchamianiu w celu utworzenia ścieżki do ładowania pliku konfiguracji aplikacji.

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> Zamiast bezpośrednio w konfiguracji środowiska *web.config* ma zawierać `<EnvironmentName>` właściwości w profilu publikowania ( *.pubxml*) lub pliku projektu. To podejście ustawia środowisko *web.config* po opublikowaniu projektu:
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

::: moniker-end

> [!WARNING]
> Ustawić tylko `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej, aby `Development` przejściowym i testowania serwerów, które nie są dostępne do niezaufanych sieci, takich jak Internet.

## <a name="appofflinehtm"></a>app_offline.htm

Jeśli plik o nazwie *app_offline.htm* wykryte w katalogu głównym aplikacji, modułu ASP.NET Core próbuje łagodne zamykanie aplikacji i zatrzymanie przetwarzania przychodzących żądań. Jeśli aplikacja jest nadal uruchomione po upływie liczby sekund zdefiniowane w `shutdownTimeLimit`, modułu ASP.NET Core to złe rozwiązanie uruchomionego procesu.

Gdy *app_offline.htm* występuje pliku modułu ASP.NET Core ma odpowiadać na żądania, wysyłając ponownie zawartość *app_offline.htm* pliku. Gdy *app_offline.htm* plik jest usuwany, kolejne żądanie uruchamia aplikację.

::: moniker range=">= aspnetcore-2.2"

Korzystając z modelu hostingu poza procesem, aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia. Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.

::: moniker-end

## <a name="start-up-error-page"></a>Strona błędu uruchamiania

::: moniker range=">= aspnetcore-2.2"

Zarówno w trakcie, jak i spoza procesu hostingu generuje strony błędów niestandardowych, gdy mogą zakończyć się niepowodzeniem uruchomić aplikację.

Jeśli modułu ASP.NET Core nie uda się znaleźć albo w trakcie lub poza procesem programu obsługi żądania, *500.0 — Błąd ładowania obsługi w-procesu/Out-Of-Process* zostanie wyświetlona strona kodu stanu.

W trakcie obsługi w przypadku niepowodzenia modułu ASP.NET Core uruchomić aplikację, *500.30 - Start błąd* zostanie wyświetlona strona kodu stanu.

Dla hostingu poza procesem, jeśli modułu ASP.NET Core nie uda się uruchomić procesu wewnętrznej bazy danych lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 - niepowodzenia procesu* zostanie wyświetlona strona kodu stanu.

Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 5xx usług IIS, należy użyć `disableStartUpErrorPage` atrybutu. Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Jeśli modułu ASP.NET Core nie uda się uruchomić procesu wewnętrznej bazy danych lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 - niepowodzenia procesu* zostanie wyświetlona strona kodu stanu. Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 502 usług IIS, należy użyć `disableStartUpErrorPage` atrybutu. Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).

![502.5 strona kodowa stan niepowodzenia procesu](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a>Tworzenia dziennika i Przekierowanie

Modułu ASP.NET Core przekierowuje dane wyjściowe stdout i stderr konsoli do dysku, jeśli `stdoutLogEnabled` i `stdoutLogFile` atrybuty `aspNetCore` elementu są ustawione. Wszystkie foldery w `stdoutLogFile` ścieżki są tworzone przez moduł, gdy plik dziennika jest tworzony. Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).

Dzienniki nie są obracane, chyba że wystąpi procesu recykling/ponowne uruchomienie. Jest odpowiedzialny za dostawcy usług hostingowych w celu ograniczenia ilości miejsca na dysku przez dzienniki.

Przy użyciu dziennika stdout jest zalecane tylko na potrzeby rozwiązywania problemów z uruchamianiem aplikacji. Nie używaj dziennika stdout do celów rejestrowania głównej aplikacji. Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje. Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).

Rozszerzenie sygnatur czasowych i pliku są automatycznie dodawane, gdy plik dziennika jest tworzony. Przez dodanie sygnatury czasowej, identyfikator procesu i rozszerzenie pliku składa się nazwa pliku dziennika ( *.log*) do ostatni segment `stdoutLogFile` ścieżki (zazwyczaj *stdout*) rozdzielane znakami podkreślenia. Jeśli `stdoutLogFile` ścieżki kończy się *stdout*, dziennika dla aplikacji za pomocą identyfikatora PID 1934 utworzono 19:42:32 2/5/2018 o nazwie pliku *stdout_20180205194132_1934.log*.

::: moniker range=">= aspnetcore-2.2"

Jeśli `stdoutLogEnabled` ma wartość FAŁSZ, błędów występujących podczas uruchamiania aplikacji są przechwytywane i wysyłanego do dziennika zdarzeń do 30 KB. Po uruchomieniu wszystkie dodatkowe dzienniki są odrzucane.

::: moniker-end

Poniższy przykład `aspNetCore` element konfiguruje rejestrowanie strumienia stdout dla aplikacji hostowanej w usłudze Azure App Service. Ścieżka lokalna lub ścieżkę do udziału sieciowego jest możliwa do logowania lokalnego. Upewnij się, że tożsamość puli aplikacji ma uprawnienia do zapisu w ścieżce podanej.

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="enhanced-diagnostic-logs"></a>Rozszerzone dzienników diagnostycznych

Modułu ASP.NET Core jest konfigurowane w celu zapewnienia dzienniki diagnostyki rozszerzonej. Dodaj `<handlerSettings>` elementu `<aspNetCore>` element *web.config*. Ustawienie `debugLevel` do `TRACE` udostępnia pozwala uzyskać większą wierność informacje diagnostyczne:

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Wszystkie foldery w ścieżce (*dzienniki* w powyższym przykładzie) są tworzone przez moduł, gdy plik dziennika jest tworzony. Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Foldery w ścieżce podanej do `<handlerSetting>` wartość (*dzienniki* w powyższym przykładzie) nie są tworzone automatycznie przez moduł i wstępnie powinno istnieć we wdrożeniu. Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Poziom debugowania (`debugLevel`) wartości mogą obejmować zarówno na poziomie, jak i lokalizację.

Poziomy (w kolejności od najmniejszej do najbardziej szczegółowy):

* BŁĄD
* OSTRZEŻENIE
* INFORMACJE O
* TRACE

Lokalizacje (wiele lokalizacji są dozwolone):

* KONSOLA
* DZIENNIK ZDARZEŃ
* PLIK

Ustawienia obsługi można również podać za pośrednictwem zmienne środowiskowe:

* `ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Ścieżka do pliku dziennika debugowania. (Domyślnie: *czy aspnetcore*)
* `ASPNETCORE_MODULE_DEBUG` &ndash; Ustawienie poziomie debugowania.

> [!WARNING]
> Czy **nie** Pozostaw włączone we wdrożeniu na dłużej, niż jest to wymagane, aby rozwiązać problem rejestrowanie debugowania. Rozmiar dziennika nie jest ograniczona. Pozostawienie dziennik debugowania włączone może wyczerpać dostępne miejsce na dysku i awarii serwera lub usługi app service.

::: moniker-end

Zobacz [konfiguracji z pliku web.config](#configuration-with-webconfig) przykład `aspNetCore` element *web.config* pliku.

::: moniker range=">= aspnetcore-3.0"

## <a name="modify-the-stack-size"></a>Zmodyfikuj rozmiar stosu

Konfigurowanie przy użyciu rozmiaru zarządzanego stosu `stackSize` ustawienie w bajtach. Domyślny rozmiar to `1048576` bajtów (1 MB).

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="stackSize" value="2097152" />
  </handlerSettings>
</aspNetCore>
```

::: moniker-end

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Konfiguracja serwera proxy korzysta z protokołu HTTP i parowania token

::: moniker range=">= aspnetcore-2.2"

*Dotyczy tylko hostingu poza procesem.*

::: moniker-end

Serwer proxy między modułu ASP.NET Core i Kestrel korzysta z protokołu HTTP. Przy użyciu protokołu HTTP jest Optymalizacja wydajności, gdzie ruch między modułem i Kestrel odbywa się na adres sprzężenia zwrotnego zniżki w stosunku do interfejsu sieciowego. Istnieje ryzyko ochronę ruchu między moduł i Kestrel z lokalizacji poza serwerem.

Parowania tokenu jest używany w celu zagwarantowania, że przekazywane przez usługi IIS żądań odebranych przez Kestrel i nie pochodzi z innego źródła. Parowania token jest utworzona i ustawiona w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez moduł. Token parowania jest również ustawiona w nagłówku (`MS-ASPNETCORE-TOKEN`) na każde żądanie serwerem proxy. Oprogramowanie pośredniczące IIS sprawdzanie żądań odbierze, aby upewnić się, że wartość tokenu nagłówka parowania odpowiada wartości zmiennej środowiskowej. Jeśli token wartości są niezgodne, żądanie jest rejestrowane i odrzucone. Zmienna środowiskowa tokenu parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem. Bez znajomości parowania wartość tokenu, osoba atakująca nie może przesłać żądania, które obchodzą wyboru w oprogramowaniu pośredniczącym usługi IIS.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Moduł ASP.NET Core z programem IIS konfigurację udostępnioną

Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami **zaufany Instalator** konta. Ponieważ lokalne konto systemowe nie ma uprawnienia do modyfikowania dla ścieżki udziału konfiguracji udostępnionej usług IIS, Instalator zgłasza odmowa dostępu błąd podczas próby skonfigurowania ustawień modułu w *applicationHost.config*  pliku w udziale.

::: moniker range=">= aspnetcore-2.2"

Podczas korzystania z konfiguracji udostępnionej usług IIS, w tym samym komputerze co instalacji usług IIS, uruchom Instalatora programu ASP.NET Core hostingu pakietu z `OPT_NO_SHARED_CONFIG_CHECK` parametr `1`:

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

Gdy na tym samym komputerze co instalacji usług IIS nie jest ścieżką do konfiguracji udostępnionej, wykonaj następujące kroki:

1. Wyłącz konfiguracji udostępnionej usług IIS.
1. Uruchom Instalatora.
1. Eksportuj zaktualizowanego *applicationHost.config* plików do udziału.
1. Ponownie włączyć konfiguracji udostępnionej usług IIS.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Korzystając z konfiguracji udostępnionej usług IIS, wykonaj następujące kroki:

1. Wyłącz konfiguracji udostępnionej usług IIS.
1. Uruchom Instalatora.
1. Eksportuj zaktualizowanego *applicationHost.config* plików do udziału.
1. Ponownie włączyć konfiguracji udostępnionej usług IIS.

::: moniker-end

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Wersja modułu oraz Dzienniki Instalatora pakietu hostingu

Aby określić wersję zainstalowanego modułu ASP.NET Core:

1. W systemie hostingu, przejdź do *%windir%\System32\inetsrv*.
1. Znajdź *aspnetcore.dll* pliku.
1. Kliknij prawym przyciskiem myszy plik i wybierz **właściwości** z menu kontekstowego.
1. Wybierz **szczegóły** kartę. **Wersja pliku** i **wersji produktu** reprezentują zainstalowanej wersji modułu.

Dzienniki Instalatora pakietu hostingu dla modułu znajdują się w *C:\\użytkowników\\% UserName %\\AppData\\lokalnego\\Temp*. Plik *dd_DotNetCoreWinSvrHosting__\<sygnatura czasowa > _000_AspNetCoreModule_x64.log*.

## <a name="module-schema-and-configuration-file-locations"></a>Lokalizacje pliku modułu, schematu i konfiguracji

### <a name="module"></a>Moduł

**Usługi IIS (x86/amd64):**

* %windir%\System32\inetsrv\aspnetcore.dll

* %windir%\SysWOW64\inetsrv\aspnetcore.dll

::: moniker range=">= aspnetcore-2.2"

* %ProgramFiles%\IIS\Asp.NET core Module\V2\aspnetcorev2.dll

* % ProgramFiles (x86) %\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll

::: moniker-end

**Usługi IIS Express (x86/amd64):**

* %ProgramFiles%\IIS Express\aspnetcore.dll

* %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

::: moniker range=">= aspnetcore-2.2"

* %ProgramFiles%\iis Express\Asp.Net Core Module\V2\aspnetcorev2.dll

* % ProgramFiles (x86) %\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll

::: moniker-end

### <a name="schema"></a>Schemat

**IIS**

* %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

::: moniker range=">= aspnetcore-2.2"

* %Windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML

::: moniker-end

**Usługi IIS Express**

* %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

::: moniker range=">= aspnetcore-2.2"

* %ProgramFiles%\iis Express\config\schema\aspnetcore_schema_v2.xml

::: moniker-end

### <a name="configuration"></a>Konfiguracja

**IIS**

* %windir%\System32\inetsrv\config\applicationHost.config

**Usługi IIS Express**

* Visual Studio: {Katalog główny aplikacji}\\.vs\config\applicationHost.config

* *iisexpress.exe* interfejsu wiersza polecenia: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config

Pliki można znaleźć, wyszukując *aspnetcore* w *applicationHost.config* pliku.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:host-and-deploy/iis/index>
* [Repozytorium GitHub modułów Core ASP.NET (źródło odwołania)](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
