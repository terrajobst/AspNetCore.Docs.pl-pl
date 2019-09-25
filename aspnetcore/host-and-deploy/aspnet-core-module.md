---
title: Moduł ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować modułu ASP.NET Core do hostowania aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/24/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 811aafce6686b446440b146efd7449b598ed1722
ms.sourcegitcommit: e54672f5c493258dc449fac5b98faf47eb123b28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2019
ms.locfileid: "71248345"
---
# <a name="aspnet-core-module"></a>Moduł ASP.NET Core

Autorzy [Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Krzysztof Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik)i [Luke](https://github.com/guardrex) Latham

::: moniker range=">= aspnetcore-3.0"

Moduł ASP.NET Core jest natywnym modułem usług IIS, który jest podłączany do potoku usług IIS:

* Hostowanie aplikacji ASP.NET Core w procesie roboczym usług IIS (`w3wp.exe`), nazywanym [modelem hostingu w procesie](#in-process-hosting-model).
* Przekazuj żądania sieci Web do zaplecza ASP.NET Core aplikacji, na której uruchomiono [serwer Kestrel](xref:fundamentals/servers/kestrel), nazywany [modelem hostingu poza procesem](#out-of-process-hosting-model).

Obsługiwane wersje systemu Windows:

* Windows 7 lub nowszy
* Windows Server 2008 R2 lub nowszy

Podczas hostingu w procesie moduł używa implementacji serwera w procesie dla usług IIS, nazywanego serwerem HTTP IIS (`IISHttpServer`).

Podczas hostingu poza procesem moduł działa tylko z Kestrel. Moduł nie działa w przypadku [protokołu HTTP. sys](xref:fundamentals/servers/httpsys).

## <a name="hosting-models"></a>Modele hostingu

### <a name="in-process-hosting-model"></a>Model hostingu w procesie

Domyślnie ASP.NET Core aplikacje do modelu hostingu w procesie.

Następujące właściwości mają zastosowanie w przypadku hostowania w procesie:

* Serwer http IIS (`IISHttpServer`) jest używany zamiast serwera [Kestrel](xref:fundamentals/servers/kestrel) . W przypadku [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) wywołań <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> do:

  * `IISHttpServer`Zarejestruj.
  * Skonfiguruj port i ścieżkę bazową, na której serwer powinien nasłuchiwać przy uruchomionym za modułem ASP.NET Core.
  * Skonfiguruj hosta do przechwytywania błędów uruchamiania.

* [Atrybut requestTimeout](#attributes-of-the-aspnetcore-element) nie ma zastosowania do hostowania w procesie.

* Udostępnianie puli aplikacji między aplikacjami nie jest obsługiwane. Użycie jednej puli aplikacji na aplikację.

* Korzystając z [narzędzia Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) lub ręczne wprowadzanie [plik app_offline.htm we wdrożeniu](xref:host-and-deploy/iis/index#locked-deployment-files), aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia. Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.

* Architektura (liczba bitów) zainstalowanego środowiska uruchomieniowego (x64 lub x86) i aplikacji musi być zgodna z architekturą puli aplikacji.

* Rozłącza klienta są wykrywane. [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) odwołano token anulowania, gdy klient odłączy się.

* W ASP.NET Core 2.2.1 lub wcześniejszym <xref:System.IO.Directory.GetCurrentDirectory*> zwraca katalog procesów roboczych procesu uruchomionego przez usługi IIS, a nie katalog aplikacji (na przykład *C:\Windows\System32\inetsrv* for *w3wp. exe*).

  Przykładowy kod, który ustawia bieżący katalog aplikacji, zobacz [klasy CurrentDirectoryHelpers](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs). Wywołaj `SetCurrentDirectory` metody. Kolejne wywołania <xref:System.IO.Directory.GetCurrentDirectory*> zapewniają katalogu aplikacji.

* Podczas hostingu w procesie <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nie jest wywoływana wewnętrznie w celu zainicjowania użytkownika. W związku z <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> tym, implementacja użyta do przekształcenia oświadczeń po każdym uwierzytelnieniu nie jest domyślnie aktywowana. Podczas przekształcania oświadczeń z <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementacją, wywołaj <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> polecenie Dodaj usługi uwierzytelniania:

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

Aby skonfigurować aplikację do hostingu poza procesem, ustaw wartość `<AspNetCoreHostingModel>` właściwości na `OutOfProcess` (hosting w procesie jest ustawiony za pomocą `InProcess`, która jest wartością domyślną):

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

Serwer [Kestrel](xref:fundamentals/servers/kestrel) jest używany zamiast serwera http usług IIS (`IISHttpServer`).

W przypadku [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) połączeń <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> z:

* Skonfiguruj port i ścieżkę bazową, na której serwer powinien nasłuchiwać przy uruchomionym za modułem ASP.NET Core.
* Skonfiguruj hosta do przechwytywania błędów uruchamiania.

### <a name="hosting-model-changes"></a>Zmiany modelu hostingu

Jeśli `hostingModel` ustawienie to ulegnie zmianie w *web.config* pliku (wyjaśnione w [konfiguracji z pliku web.config](#configuration-with-webconfig) sekcji), moduł odtwarzania procesu roboczego programu IIS.

Dla usług IIS Express moduł nie odtworzyć proces roboczy, ale zamiast tego wyzwala łagodne zamykanie bieżący proces usług IIS Express. Kolejne żądanie aplikacji spowoduje utworzenie nowych procesów usług IIS Express.

### <a name="process-name"></a>Nazwa procesu

`Process.GetCurrentProcess().ProcessName` Raporty `w3wp` / `iisexpress` (w procesie) lub `dotnet` (poza procesem).

Wiele modułów macierzystych, takich jak uwierzytelnianie systemu Windows, pozostaje aktywnych. Aby dowiedzieć się więcej na temat modułów usług IIS aktywnych przy użyciu <xref:host-and-deploy/iis/modules>modułu ASP.NET Core, zobacz.

Moduł ASP.NET Core może również:

* Ustaw zmienne środowiskowe dla procesu roboczego.
* Rejestruj dane wyjściowe stdout do magazynu plików w celu rozwiązywania problemów z uruchamianiem.
* Przekazuj tokeny uwierzytelniania systemu Windows.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>Jak zainstalować moduł ASP.NET Core i korzystać z niego

Aby uzyskać instrukcje dotyczące sposobu instalowania modułu ASP.NET Core, zobacz [installing the .NET Core hosting](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

## <a name="configuration-with-webconfig"></a>Konfiguracja z pliku web.config

Skonfigurowano modułu ASP.NET Core `aspNetCore` części `system.webServer` węzeł w tej witrynie *web.config* pliku.

Następujące *web.config* pliku została opublikowana na potrzeby [wdrożenia zależny od struktury](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje modułu ASP.NET Core do obsługi żądań w lokacji:

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

Następujące *web.config* została opublikowana na potrzeby [niezależna wdrożenia](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

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

Gdy aplikacja jest wdrażana na [usługi Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` ścieżka jest równa `\\?\%home%\LogFiles\stdout`. Ścieżka zapisuje dzienniki stdout *LogFiles* folderu, w którym znajduje się automatycznie utworzone przez usługę.

Aby uzyskać informacji na temat konfigurowania aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications>.

### <a name="attributes-of-the-aspnetcore-element"></a>Atrybuty elementu aspNetCore

| Atrybut | Opis | Domyślny |
| --------- | ----------- | :-----: |
| `arguments` | <p>Atrybut opcjonalny ciąg.</p><p>Argumenty do pliku wykonywalnego, określony w **processPath**.</p> | |
| `disableStartUpErrorPage` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</p> | `false` |
| `forwardWindowsAuthToken` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie. Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</p> | `true` |
| `hostingModel` | <p>Atrybut opcjonalny ciąg.</p><p>Określa modelu hostingu w trakcie (`InProcess`) lub spoza procesu (`OutOfProcess`).</p> | `InProcess` |
| `processesPerApplication` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</p><p>&dagger;W trakcie hostingu, wartość jest ograniczona do `1`.</p><p>Ustawienie `processesPerApplication` jest niezalecane. Ten atrybut zostanie usunięty w przyszłych wydaniach.</p> | Wartość domyślna: `1`<br>Minimalna: `1`<br>Maks.: `100`&dagger; |
| `processPath` | <p>Atrybut wymagany ciąg.</p><p>Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP. Obsługiwane są ścieżki względne. Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</p> | |
| `rapidFailsPerMinute` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę. W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</p><p>Nieobsługiwane za pomocą wewnątrzprocesowego hostingu.</p> | Wartość domyślna: `10`<br>Minimalna: `0`<br>Maks.: `100` |
| `requestTimeout` | <p>Atrybut opcjonalny przedziału czasu.</p><p>Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</p><p>W wersjach modułu ASP.NET Core, dołączonej do wersji platformy ASP.NET Core 2.1 lub nowszej `requestTimeout` jest określona w godzinach, minutach i sekundach.</p><p>Nie ma zastosowania do hostowania w procesie. Dla hostingu w procesie modułu czeka na aplikację, aby przetworzyć żądanie.</p><p>Prawidłowe wartości segmentów minut i sekund ciągu mieszczą się w zakresie 0-59. Użycie **60** w wartości minut lub sekund skutkuje *błędem wewnętrznego serwera 500*.</p> | Wartość domyślna: `00:02:00`<br>Minimalna: `00:00:00`<br>Maks.: `360:00:00` |
| `shutdownTimeLimit` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</p> | Wartość domyślna: `10`<br>Minimalna: `0`<br>Maks.: `600` |
| `startupTimeLimit` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie. Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu. Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</p><p>Wartość 0 (zero) jest **nie** uważane za nieskończony limit czasu.</p> | Wartość domyślna: `120`<br>Minimalna: `0`<br>Maks.: `3600` |
| `stdoutLogEnabled` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Atrybut opcjonalny ciąg.</p><p>Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane. Są ścieżki względne względem katalogu głównego witryny. Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych. Wszystkie foldery podane w ścieżce są tworzone przez moduł po utworzeniu pliku dziennika. Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku ( *.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki. Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a>Ustawianie zmiennych środowiskowych

Zmienne środowiskowe można określić dla tego procesu w `processPath` atrybutu. Określ zmienną środowiskową `<environmentVariable>` element podrzędny elementu `<environmentVariables>` elementu kolekcji. Zmienne środowiskowe ustawione w tej sekcji pierwszeństwo systemowe zmienne środowiskowe.

W poniższym przykładzie ustawiono dwóch zmiennych środowiskowych. `ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji `Development`. Deweloper może tymczasowo ustawić tę wartość w *web.config* pliku, aby wymusić [stronie wyjątków deweloperów](xref:fundamentals/error-handling) załadować podczas debugowania aplikacji wyjątek. `CONFIG_DIR` to przykład zmiennej środowiskowej zdefiniowanej przez użytkownika, gdzie deweloper ma napisany kod, który odczytuje wartość przy uruchamianiu w celu utworzenia ścieżki do ładowania pliku konfiguracji aplikacji.

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

> [!NOTE]
> Alternatywą dla ustawienia środowiska bezpośrednio w pliku *Web. config* jest uwzględnienie `<EnvironmentName>` właściwości w profilu publikacji ( *. pubxml*) lub plik projektu. To podejście ustawia środowisko w *pliku Web. config* po opublikowaniu projektu:
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> Ustawić tylko `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej, aby `Development` przejściowym i testowania serwerów, które nie są dostępne do niezaufanych sieci, takich jak Internet.

## <a name="app_offlinehtm"></a>app_offline.htm

Jeśli plik o nazwie *app_offline.htm* wykryte w katalogu głównym aplikacji, modułu ASP.NET Core próbuje łagodne zamykanie aplikacji i zatrzymanie przetwarzania przychodzących żądań. Jeśli aplikacja jest nadal uruchomione po upływie liczby sekund zdefiniowane w `shutdownTimeLimit`, modułu ASP.NET Core to złe rozwiązanie uruchomionego procesu.

Gdy *app_offline.htm* występuje pliku modułu ASP.NET Core ma odpowiadać na żądania, wysyłając ponownie zawartość *app_offline.htm* pliku. Gdy *app_offline.htm* plik jest usuwany, kolejne żądanie uruchamia aplikację.

Korzystając z modelu hostingu poza procesem, aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia. Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.

## <a name="start-up-error-page"></a>Strona błędu uruchamiania

Zarówno w trakcie, jak i spoza procesu hostingu generuje strony błędów niestandardowych, gdy mogą zakończyć się niepowodzeniem uruchomić aplikację.

Jeśli modułu ASP.NET Core nie uda się znaleźć albo w trakcie lub poza procesem programu obsługi żądania, *500.0 — Błąd ładowania obsługi w-procesu/Out-Of-Process* zostanie wyświetlona strona kodu stanu.

W trakcie obsługi w przypadku niepowodzenia modułu ASP.NET Core uruchomić aplikację, *500.30 - Start błąd* zostanie wyświetlona strona kodu stanu.

Dla hostingu poza procesem, jeśli modułu ASP.NET Core nie uda się uruchomić procesu wewnętrznej bazy danych lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 - niepowodzenia procesu* zostanie wyświetlona strona kodu stanu.

Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 5xx usług IIS, należy użyć `disableStartUpErrorPage` atrybutu. Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).

## <a name="log-creation-and-redirection"></a>Tworzenia dziennika i Przekierowanie

Modułu ASP.NET Core przekierowuje dane wyjściowe stdout i stderr konsoli do dysku, jeśli `stdoutLogEnabled` i `stdoutLogFile` atrybuty `aspNetCore` elementu są ustawione. Wszystkie foldery w `stdoutLogFile` ścieżce są tworzone przez moduł po utworzeniu pliku dziennika. Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).

Dzienniki nie są obracane, chyba że wystąpi procesu recykling/ponowne uruchomienie. Jest odpowiedzialny za dostawcy usług hostingowych w celu ograniczenia ilości miejsca na dysku przez dzienniki.

Przy użyciu dziennika stdout jest zalecane tylko na potrzeby rozwiązywania problemów z uruchamianiem aplikacji. Nie używaj dziennika stdout do celów rejestrowania głównej aplikacji. Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje. Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).

Rozszerzenie sygnatur czasowych i pliku są automatycznie dodawane, gdy plik dziennika jest tworzony. Przez dodanie sygnatury czasowej, identyfikator procesu i rozszerzenie pliku składa się nazwa pliku dziennika ( *.log*) do ostatni segment `stdoutLogFile` ścieżki (zazwyczaj *stdout*) rozdzielane znakami podkreślenia. Jeśli `stdoutLogFile` ścieżki kończy się *stdout*, dziennika dla aplikacji za pomocą identyfikatora PID 1934 utworzono 19:42:32 2/5/2018 o nazwie pliku *stdout_20180205194132_1934.log*.

Jeśli `stdoutLogEnabled` ma wartość FAŁSZ, błędów występujących podczas uruchamiania aplikacji są przechwytywane i wysyłanego do dziennika zdarzeń do 30 KB. Po uruchomieniu wszystkie dodatkowe dzienniki są odrzucane.

Poniższy przykład `aspNetCore` element konfiguruje rejestrowanie strumienia stdout dla aplikacji hostowanej w usłudze Azure App Service. Ścieżka lokalna lub ścieżkę do udziału sieciowego jest możliwa do logowania lokalnego. Upewnij się, że tożsamość puli aplikacji ma uprawnienia do zapisu w ścieżce podanej.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

## <a name="enhanced-diagnostic-logs"></a>Rozszerzone dzienników diagnostycznych

Moduł ASP.NET Core można skonfigurować w celu udostępnienia dzienników diagnostyki rozszerzonej. Dodaj `<handlerSettings>` elementu `<aspNetCore>` element *web.config*. Ustawienie `debugLevel` do `TRACE` udostępnia pozwala uzyskać większą wierność informacje diagnostyczne:

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

Wszystkie foldery w ścieżce (*dzienniki* w poprzednim przykładzie) są tworzone przez moduł po utworzeniu pliku dziennika. Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).

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

Zobacz [konfiguracji z pliku web.config](#configuration-with-webconfig) przykład `aspNetCore` element *web.config* pliku.

## <a name="modify-the-stack-size"></a>Modyfikowanie rozmiaru stosu

*Stosuje się tylko w przypadku korzystania z modelu hostingu w procesie.*

Skonfiguruj rozmiar zarządzanego stosu przy użyciu `stackSize` ustawienia w bajtach. Domyślny rozmiar to `1048576` bajty (1 MB).

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

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Konfiguracja serwera proxy korzysta z protokołu HTTP i parowania token

*Dotyczy tylko hostingu poza procesem.*

Serwer proxy między modułu ASP.NET Core i Kestrel korzysta z protokołu HTTP. Istnieje ryzyko ochronę ruchu między moduł i Kestrel z lokalizacji poza serwerem.

Parowania tokenu jest używany w celu zagwarantowania, że przekazywane przez usługi IIS żądań odebranych przez Kestrel i nie pochodzi z innego źródła. Parowania token jest utworzona i ustawiona w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez moduł. Token parowania jest również ustawiona w nagłówku (`MS-ASPNETCORE-TOKEN`) na każde żądanie serwerem proxy. Oprogramowanie pośredniczące IIS sprawdzanie żądań odbierze, aby upewnić się, że wartość tokenu nagłówka parowania odpowiada wartości zmiennej środowiskowej. Jeśli token wartości są niezgodne, żądanie jest rejestrowane i odrzucone. Zmienna środowiskowa tokenu parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem. Bez znajomości parowania wartość tokenu, osoba atakująca nie może przesłać żądania, które obchodzą wyboru w oprogramowaniu pośredniczącym usługi IIS.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Moduł ASP.NET Core z programem IIS konfigurację udostępnioną

Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami konta **TrustedInstaller** . Ponieważ konto systemu lokalnego nie ma uprawnień do modyfikowania dla ścieżki udziału używanej przez udostępnioną konfigurację usług IIS, Instalator zgłasza błąd odmowy dostępu podczas próby skonfigurowania ustawień modułu w pliku *ApplicationHost. config* na udział.

W przypadku korzystania z konfiguracji udostępnionej przez usługi IIS na tym samym komputerze, na którym znajduje się instalacja usług IIS, `OPT_NO_SHARED_CONFIG_CHECK` Uruchom Instalatora pakietu `1`ASP.NET Core hostowania z parametrem ustawionym na:

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

Jeśli ścieżka do konfiguracji udostępnionej nie znajduje się na tym samym komputerze, na którym znajduje się instalacja usług IIS, wykonaj następujące czynności:

1. Wyłącz konfiguracji udostępnionej usług IIS.
1. Uruchom Instalatora.
1. Eksportuj zaktualizowanego *applicationHost.config* plików do udziału.
1. Ponownie włączyć konfiguracji udostępnionej usług IIS.

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

* %ProgramFiles%\IIS\Asp.NET core Module\V2\aspnetcorev2.dll

* % ProgramFiles (x86) %\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll

**Usługi IIS Express (x86/amd64):**

* %ProgramFiles%\IIS Express\aspnetcore.dll

* %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

* %ProgramFiles%\iis Express\Asp.Net Core Module\V2\aspnetcorev2.dll

* % ProgramFiles (x86) %\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll

### <a name="schema"></a>Schemat

**IIS**

* %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

* %Windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML

**Usługi IIS Express**

* %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

* %ProgramFiles%\iis Express\config\schema\aspnetcore_schema_v2.xml

### <a name="configuration"></a>Konfiguracja

**IIS**

* %windir%\System32\inetsrv\config\applicationHost.config

**Usługi IIS Express**

* Visual Studio: {Aplikacja główna}\\. vs\config\applicationHost.config

* Interfejs wiersza polecenia *iisexpress. exe* :%USERPROFILE%\Documents\IISExpress\config\applicationhost.config

Pliki można znaleźć, wyszukując *aspnetcore* w *applicationHost.config* pliku.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Moduł ASP.NET Core jest natywnym modułem usług IIS, który jest podłączany do potoku usług IIS:

* Hostowanie aplikacji ASP.NET Core w procesie roboczym usług IIS (`w3wp.exe`), nazywanym [modelem hostingu w procesie](#in-process-hosting-model).
* Przekazuj żądania sieci Web do zaplecza ASP.NET Core aplikacji, na której uruchomiono [serwer Kestrel](xref:fundamentals/servers/kestrel), nazywany [modelem hostingu poza procesem](#out-of-process-hosting-model).

Obsługiwane wersje systemu Windows:

* Windows 7 lub nowszy
* Windows Server 2008 R2 lub nowszy

Podczas hostingu w procesie moduł używa implementacji serwera w procesie dla usług IIS, nazywanego serwerem HTTP IIS (`IISHttpServer`).

Podczas hostingu poza procesem moduł działa tylko z Kestrel. Moduł nie działa w przypadku [protokołu HTTP. sys](xref:fundamentals/servers/httpsys).

## <a name="hosting-models"></a>Modele hostingu

### <a name="in-process-hosting-model"></a>Model hostingu w procesie

Aby skonfigurować aplikację do hostingu w procesie, należy dodać `<AspNetCoreHostingModel>` właściwość do pliku projektu aplikacji z `InProcess` wartością (hosting poza procesem jest ustawiony z `OutOfProcess`):

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

Model hostingu w procesie nie jest obsługiwany w przypadku aplikacji ASP.NET Core przeznaczonych dla .NET Framework.

Jeśli właściwość nie jest obecna w pliku, wartość domyślna to `OutOfProcess`. `<AspNetCoreHostingModel>`

Następujące właściwości mają zastosowanie w przypadku hostowania w procesie:

* Serwer http IIS (`IISHttpServer`) jest używany zamiast serwera [Kestrel](xref:fundamentals/servers/kestrel) . W przypadku [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) wywołań <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> do:

  * `IISHttpServer`Zarejestruj.
  * Skonfiguruj port i ścieżkę bazową, na której serwer powinien nasłuchiwać przy uruchomionym za modułem ASP.NET Core.
  * Skonfiguruj hosta do przechwytywania błędów uruchamiania.

* [Atrybut requestTimeout](#attributes-of-the-aspnetcore-element) nie ma zastosowania do hostowania w procesie.

* Udostępnianie puli aplikacji między aplikacjami nie jest obsługiwane. Użycie jednej puli aplikacji na aplikację.

* Korzystając z [narzędzia Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) lub ręczne wprowadzanie [plik app_offline.htm we wdrożeniu](xref:host-and-deploy/iis/index#locked-deployment-files), aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia. Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.

* Architektura (liczba bitów) zainstalowanego środowiska uruchomieniowego (x64 lub x86) i aplikacji musi być zgodna z architekturą puli aplikacji.

* Rozłącza klienta są wykrywane. [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) odwołano token anulowania, gdy klient odłączy się.

* W ASP.NET Core 2.2.1 lub wcześniejszym <xref:System.IO.Directory.GetCurrentDirectory*> zwraca katalog procesów roboczych procesu uruchomionego przez usługi IIS, a nie katalog aplikacji (na przykład *C:\Windows\System32\inetsrv* for *w3wp. exe*).

  Przykładowy kod, który ustawia bieżący katalog aplikacji, zobacz [klasy CurrentDirectoryHelpers](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs). Wywołaj `SetCurrentDirectory` metody. Kolejne wywołania <xref:System.IO.Directory.GetCurrentDirectory*> zapewniają katalogu aplikacji.

* Podczas hostingu w procesie <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nie jest wywoływana wewnętrznie w celu zainicjowania użytkownika. W związku z <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> tym, implementacja użyta do przekształcenia oświadczeń po każdym uwierzytelnieniu nie jest domyślnie aktywowana. Podczas przekształcania oświadczeń z <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementacją, wywołaj <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> polecenie Dodaj usługi uwierzytelniania:

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

Aby skonfigurować aplikację do hostingu poza procesem, użyj jednego z następujących metod w pliku projektu:

* Nie określaj `<AspNetCoreHostingModel>` właściwości. Jeśli właściwość nie jest obecna w pliku, wartość domyślna to `OutOfProcess`. `<AspNetCoreHostingModel>`
* Ustaw wartość `<AspNetCoreHostingModel>` właściwości na `OutOfProcess` (hosting w procesie jest ustawiany z `InProcess`):

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

Serwer [Kestrel](xref:fundamentals/servers/kestrel) jest używany zamiast serwera http usług IIS (`IISHttpServer`).

W przypadku [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) połączeń <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> z:

* Skonfiguruj port i ścieżkę bazową, na której serwer powinien nasłuchiwać przy uruchomionym za modułem ASP.NET Core.
* Skonfiguruj hosta do przechwytywania błędów uruchamiania.

### <a name="hosting-model-changes"></a>Zmiany modelu hostingu

Jeśli `hostingModel` ustawienie to ulegnie zmianie w *web.config* pliku (wyjaśnione w [konfiguracji z pliku web.config](#configuration-with-webconfig) sekcji), moduł odtwarzania procesu roboczego programu IIS.

Dla usług IIS Express moduł nie odtworzyć proces roboczy, ale zamiast tego wyzwala łagodne zamykanie bieżący proces usług IIS Express. Kolejne żądanie aplikacji spowoduje utworzenie nowych procesów usług IIS Express.

### <a name="process-name"></a>Nazwa procesu

`Process.GetCurrentProcess().ProcessName` Raporty `w3wp` / `iisexpress` (w procesie) lub `dotnet` (poza procesem).

Wiele modułów macierzystych, takich jak uwierzytelnianie systemu Windows, pozostaje aktywnych. Aby dowiedzieć się więcej na temat modułów usług IIS aktywnych przy użyciu <xref:host-and-deploy/iis/modules>modułu ASP.NET Core, zobacz.

Moduł ASP.NET Core może również:

* Ustaw zmienne środowiskowe dla procesu roboczego.
* Rejestruj dane wyjściowe stdout do magazynu plików w celu rozwiązywania problemów z uruchamianiem.
* Przekazuj tokeny uwierzytelniania systemu Windows.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>Jak zainstalować moduł ASP.NET Core i korzystać z niego

Aby uzyskać instrukcje dotyczące sposobu instalowania modułu ASP.NET Core, zobacz [installing the .NET Core hosting](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

## <a name="configuration-with-webconfig"></a>Konfiguracja z pliku web.config

Skonfigurowano modułu ASP.NET Core `aspNetCore` części `system.webServer` węzeł w tej witrynie *web.config* pliku.

Następujące *web.config* pliku została opublikowana na potrzeby [wdrożenia zależny od struktury](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje modułu ASP.NET Core do obsługi żądań w lokacji:

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

Następujące *web.config* została opublikowana na potrzeby [niezależna wdrożenia](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

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

Gdy aplikacja jest wdrażana na [usługi Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` ścieżka jest równa `\\?\%home%\LogFiles\stdout`. Ścieżka zapisuje dzienniki stdout *LogFiles* folderu, w którym znajduje się automatycznie utworzone przez usługę.

Aby uzyskać informacji na temat konfigurowania aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications>.

### <a name="attributes-of-the-aspnetcore-element"></a>Atrybuty elementu aspNetCore

| Atrybut | Opis | Domyślny |
| --------- | ----------- | :-----: |
| `arguments` | <p>Atrybut opcjonalny ciąg.</p><p>Argumenty do pliku wykonywalnego, określony w **processPath**.</p> | |
| `disableStartUpErrorPage` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</p> | `false` |
| `forwardWindowsAuthToken` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie. Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</p> | `true` |
| `hostingModel` | <p>Atrybut opcjonalny ciąg.</p><p>Określa modelu hostingu w trakcie (`InProcess`) lub spoza procesu (`OutOfProcess`).</p> | `OutOfProcess` |
| `processesPerApplication` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</p><p>&dagger;W trakcie hostingu, wartość jest ograniczona do `1`.</p><p>Ustawienie `processesPerApplication` jest niezalecane. Ten atrybut zostanie usunięty w przyszłych wydaniach.</p> | Wartość domyślna: `1`<br>Minimalna: `1`<br>Maks.: `100`&dagger; |
| `processPath` | <p>Atrybut wymagany ciąg.</p><p>Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP. Obsługiwane są ścieżki względne. Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</p> | |
| `rapidFailsPerMinute` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę. W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</p><p>Nieobsługiwane za pomocą wewnątrzprocesowego hostingu.</p> | Wartość domyślna: `10`<br>Minimalna: `0`<br>Maks.: `100` |
| `requestTimeout` | <p>Atrybut opcjonalny przedziału czasu.</p><p>Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</p><p>W wersjach modułu ASP.NET Core, dołączonej do wersji platformy ASP.NET Core 2.1 lub nowszej `requestTimeout` jest określona w godzinach, minutach i sekundach.</p><p>Nie ma zastosowania do hostowania w procesie. Dla hostingu w procesie modułu czeka na aplikację, aby przetworzyć żądanie.</p><p>Prawidłowe wartości segmentów minut i sekund ciągu mieszczą się w zakresie 0-59. Użycie **60** w wartości minut lub sekund skutkuje *błędem wewnętrznego serwera 500*.</p> | Wartość domyślna: `00:02:00`<br>Minimalna: `00:00:00`<br>Maks.: `360:00:00` |
| `shutdownTimeLimit` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</p> | Wartość domyślna: `10`<br>Minimalna: `0`<br>Maks.: `600` |
| `startupTimeLimit` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie. Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu. Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</p><p>Wartość 0 (zero) jest **nie** uważane za nieskończony limit czasu.</p> | Wartość domyślna: `120`<br>Minimalna: `0`<br>Maks.: `3600` |
| `stdoutLogEnabled` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Atrybut opcjonalny ciąg.</p><p>Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane. Są ścieżki względne względem katalogu głównego witryny. Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych. Wszystkie foldery podane w ścieżce są tworzone przez moduł po utworzeniu pliku dziennika. Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku ( *.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki. Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a>Ustawianie zmiennych środowiskowych

Zmienne środowiskowe można określić dla tego procesu w `processPath` atrybutu. Określ zmienną środowiskową `<environmentVariable>` element podrzędny elementu `<environmentVariables>` elementu kolekcji. Zmienne środowiskowe ustawione w tej sekcji pierwszeństwo systemowe zmienne środowiskowe.

W poniższym przykładzie ustawiono dwóch zmiennych środowiskowych. `ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji `Development`. Deweloper może tymczasowo ustawić tę wartość w *web.config* pliku, aby wymusić [stronie wyjątków deweloperów](xref:fundamentals/error-handling) załadować podczas debugowania aplikacji wyjątek. `CONFIG_DIR` to przykład zmiennej środowiskowej zdefiniowanej przez użytkownika, gdzie deweloper ma napisany kod, który odczytuje wartość przy uruchamianiu w celu utworzenia ścieżki do ładowania pliku konfiguracji aplikacji.

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

> [!NOTE]
> Alternatywą dla ustawienia środowiska bezpośrednio w pliku *Web. config* jest uwzględnienie `<EnvironmentName>` właściwości w profilu publikacji ( *. pubxml*) lub plik projektu. To podejście ustawia środowisko w *pliku Web. config* po opublikowaniu projektu:
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> Ustawić tylko `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej, aby `Development` przejściowym i testowania serwerów, które nie są dostępne do niezaufanych sieci, takich jak Internet.

## <a name="app_offlinehtm"></a>app_offline.htm

Jeśli plik o nazwie *app_offline.htm* wykryte w katalogu głównym aplikacji, modułu ASP.NET Core próbuje łagodne zamykanie aplikacji i zatrzymanie przetwarzania przychodzących żądań. Jeśli aplikacja jest nadal uruchomione po upływie liczby sekund zdefiniowane w `shutdownTimeLimit`, modułu ASP.NET Core to złe rozwiązanie uruchomionego procesu.

Gdy *app_offline.htm* występuje pliku modułu ASP.NET Core ma odpowiadać na żądania, wysyłając ponownie zawartość *app_offline.htm* pliku. Gdy *app_offline.htm* plik jest usuwany, kolejne żądanie uruchamia aplikację.

Korzystając z modelu hostingu poza procesem, aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia. Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.

## <a name="start-up-error-page"></a>Strona błędu uruchamiania

Zarówno w trakcie, jak i spoza procesu hostingu generuje strony błędów niestandardowych, gdy mogą zakończyć się niepowodzeniem uruchomić aplikację.

Jeśli modułu ASP.NET Core nie uda się znaleźć albo w trakcie lub poza procesem programu obsługi żądania, *500.0 — Błąd ładowania obsługi w-procesu/Out-Of-Process* zostanie wyświetlona strona kodu stanu.

W trakcie obsługi w przypadku niepowodzenia modułu ASP.NET Core uruchomić aplikację, *500.30 - Start błąd* zostanie wyświetlona strona kodu stanu.

Dla hostingu poza procesem, jeśli modułu ASP.NET Core nie uda się uruchomić procesu wewnętrznej bazy danych lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 - niepowodzenia procesu* zostanie wyświetlona strona kodu stanu.

Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 5xx usług IIS, należy użyć `disableStartUpErrorPage` atrybutu. Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).

## <a name="log-creation-and-redirection"></a>Tworzenia dziennika i Przekierowanie

Modułu ASP.NET Core przekierowuje dane wyjściowe stdout i stderr konsoli do dysku, jeśli `stdoutLogEnabled` i `stdoutLogFile` atrybuty `aspNetCore` elementu są ustawione. Wszystkie foldery w `stdoutLogFile` ścieżce są tworzone przez moduł po utworzeniu pliku dziennika. Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).

Dzienniki nie są obracane, chyba że wystąpi procesu recykling/ponowne uruchomienie. Jest odpowiedzialny za dostawcy usług hostingowych w celu ograniczenia ilości miejsca na dysku przez dzienniki.

Przy użyciu dziennika stdout jest zalecane tylko na potrzeby rozwiązywania problemów z uruchamianiem aplikacji. Nie używaj dziennika stdout do celów rejestrowania głównej aplikacji. Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje. Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).

Rozszerzenie sygnatur czasowych i pliku są automatycznie dodawane, gdy plik dziennika jest tworzony. Przez dodanie sygnatury czasowej, identyfikator procesu i rozszerzenie pliku składa się nazwa pliku dziennika ( *.log*) do ostatni segment `stdoutLogFile` ścieżki (zazwyczaj *stdout*) rozdzielane znakami podkreślenia. Jeśli `stdoutLogFile` ścieżki kończy się *stdout*, dziennika dla aplikacji za pomocą identyfikatora PID 1934 utworzono 19:42:32 2/5/2018 o nazwie pliku *stdout_20180205194132_1934.log*.

Jeśli `stdoutLogEnabled` ma wartość FAŁSZ, błędów występujących podczas uruchamiania aplikacji są przechwytywane i wysyłanego do dziennika zdarzeń do 30 KB. Po uruchomieniu wszystkie dodatkowe dzienniki są odrzucane.

Poniższy przykład `aspNetCore` element konfiguruje rejestrowanie strumienia stdout dla aplikacji hostowanej w usłudze Azure App Service. Ścieżka lokalna lub ścieżkę do udziału sieciowego jest możliwa do logowania lokalnego. Upewnij się, że tożsamość puli aplikacji ma uprawnienia do zapisu w ścieżce podanej.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

## <a name="enhanced-diagnostic-logs"></a>Rozszerzone dzienników diagnostycznych

Moduł ASP.NET Core można skonfigurować w celu udostępnienia dzienników diagnostyki rozszerzonej. Dodaj `<handlerSettings>` elementu `<aspNetCore>` element *web.config*. Ustawienie `debugLevel` do `TRACE` udostępnia pozwala uzyskać większą wierność informacje diagnostyczne:

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

Foldery w ścieżce przekazanej do `<handlerSetting>` wartości (*dzienniki* w powyższym przykładzie) nie są automatycznie tworzone przez moduł i powinny być wcześniej dostępne we wdrożeniu. Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).

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

Zobacz [konfiguracji z pliku web.config](#configuration-with-webconfig) przykład `aspNetCore` element *web.config* pliku.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Konfiguracja serwera proxy korzysta z protokołu HTTP i parowania token

*Dotyczy tylko hostingu poza procesem.*

Serwer proxy między modułu ASP.NET Core i Kestrel korzysta z protokołu HTTP. Istnieje ryzyko ochronę ruchu między moduł i Kestrel z lokalizacji poza serwerem.

Parowania tokenu jest używany w celu zagwarantowania, że przekazywane przez usługi IIS żądań odebranych przez Kestrel i nie pochodzi z innego źródła. Parowania token jest utworzona i ustawiona w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez moduł. Token parowania jest również ustawiona w nagłówku (`MS-ASPNETCORE-TOKEN`) na każde żądanie serwerem proxy. Oprogramowanie pośredniczące IIS sprawdzanie żądań odbierze, aby upewnić się, że wartość tokenu nagłówka parowania odpowiada wartości zmiennej środowiskowej. Jeśli token wartości są niezgodne, żądanie jest rejestrowane i odrzucone. Zmienna środowiskowa tokenu parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem. Bez znajomości parowania wartość tokenu, osoba atakująca nie może przesłać żądania, które obchodzą wyboru w oprogramowaniu pośredniczącym usługi IIS.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Moduł ASP.NET Core z programem IIS konfigurację udostępnioną

Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami konta **TrustedInstaller** . Ponieważ konto systemu lokalnego nie ma uprawnień do modyfikowania dla ścieżki udziału używanej przez udostępnioną konfigurację usług IIS, Instalator zgłasza błąd odmowy dostępu podczas próby skonfigurowania ustawień modułu w pliku *ApplicationHost. config* na udział.

W przypadku korzystania z konfiguracji udostępnionej przez usługi IIS na tym samym komputerze, na którym znajduje się instalacja usług IIS, `OPT_NO_SHARED_CONFIG_CHECK` Uruchom Instalatora pakietu `1`ASP.NET Core hostowania z parametrem ustawionym na:

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

Jeśli ścieżka do konfiguracji udostępnionej nie znajduje się na tym samym komputerze, na którym znajduje się instalacja usług IIS, wykonaj następujące czynności:

1. Wyłącz konfiguracji udostępnionej usług IIS.
1. Uruchom Instalatora.
1. Eksportuj zaktualizowanego *applicationHost.config* plików do udziału.
1. Ponownie włączyć konfiguracji udostępnionej usług IIS.

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

* %ProgramFiles%\IIS\Asp.NET core Module\V2\aspnetcorev2.dll

* % ProgramFiles (x86) %\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll

**Usługi IIS Express (x86/amd64):**

* %ProgramFiles%\IIS Express\aspnetcore.dll

* %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

* %ProgramFiles%\iis Express\Asp.Net Core Module\V2\aspnetcorev2.dll

* % ProgramFiles (x86) %\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll

### <a name="schema"></a>Schemat

**IIS**

* %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

* %Windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML

**Usługi IIS Express**

* %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

* %ProgramFiles%\iis Express\config\schema\aspnetcore_schema_v2.xml

### <a name="configuration"></a>Konfiguracja

**IIS**

* %windir%\System32\inetsrv\config\applicationHost.config

**Usługi IIS Express**

* Visual Studio: {Aplikacja główna}\\. vs\config\applicationHost.config

* Interfejs wiersza polecenia *iisexpress. exe* :%USERPROFILE%\Documents\IISExpress\config\applicationhost.config

Pliki można znaleźć, wyszukując *aspnetcore* w *applicationHost.config* pliku.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Moduł ASP.NET Core jest natywnym modułem usług IIS, który jest podłączany do potoku usług IIS do przesyłania dalej żądań sieci Web do zaplecza ASP.NET Core aplikacji.

Obsługiwane wersje systemu Windows:

* Windows 7 lub nowszy
* Windows Server 2008 R2 lub nowszy

Moduł działa tylko z Kestrel. Moduł jest niezgodny z [protokołem HTTP. sys](xref:fundamentals/servers/httpsys).

Ponieważ ASP.NET Core aplikacje działają w procesie innym niż proces roboczy usług IIS, moduł obsługuje również zarządzanie procesami. Moduł uruchamia proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie zostanie odebrane i ponownie uruchomiony, jeśli wystąpi awaria. Jest to zasadniczo takie samo zachowanie jak w przypadku aplikacji ASP.NET 4. x, które działają w procesie w usługach IIS, które są zarządzane przez [usługę aktywacji procesów systemu Windows (was)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).

Na poniższym diagramie przedstawiono relację między usługami IIS, modułem ASP.NET Core i aplikacją:

![Moduł ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

Żądania docierają do sieci Web do sterownika HTTP. sys trybu jądra. Sterownik kieruje żądania do usług IIS na skonfigurowanym porcie witryny sieci Web, zwykle 80 (HTTP) lub 443 (HTTPS). Moduł przekazuje żądania do Kestrel na losowo wybranym porcie dla aplikacji, która nie jest portem 80 lub 443.

Moduł określa port za pośrednictwem zmiennej środowiskowej podczas uruchamiania, a [oprogramowanie pośredniczące integracji usług IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) konfiguruje serwer do nasłuchiwania `http://localhost:{port}`. Dodatkowe sprawdzenia są wykonywane, a żądania, które nie pochodzą z modułu, są odrzucane. Moduł nie obsługuje przekazywania HTTPS, dlatego żądania są przekazywane przez protokół HTTP nawet wtedy, gdy są odbierane przez usługę IIS przez protokół HTTPS.

Po podaniu przez Kestrel żądania z modułu żądanie jest wypychane do potoku ASP.NET Core pośredniczącego. Potok oprogramowania pośredniczącego obsługuje żądanie i przekazuje go jako `HttpContext` wystąpienie do logiki aplikacji. Oprogramowanie pośredniczące dodane przez integrację usług IIS aktualizuje schemat, zdalny adres IP i pathbase, aby można było przesłać żądanie do Kestrel. Odpowiedź aplikacji jest przesyłana z powrotem do usług IIS, która wypycha ją z powrotem do klienta HTTP, który zainicjował żądanie.

Wiele modułów macierzystych, takich jak uwierzytelnianie systemu Windows, pozostaje aktywnych. Aby dowiedzieć się więcej na temat modułów usług IIS aktywnych przy użyciu <xref:host-and-deploy/iis/modules>modułu ASP.NET Core, zobacz.

Moduł ASP.NET Core może również:

* Ustaw zmienne środowiskowe dla procesu roboczego.
* Rejestruj dane wyjściowe stdout do magazynu plików w celu rozwiązywania problemów z uruchamianiem.
* Przekazuj tokeny uwierzytelniania systemu Windows.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>Jak zainstalować moduł ASP.NET Core i korzystać z niego

Aby uzyskać instrukcje dotyczące sposobu instalowania modułu ASP.NET Core, zobacz [installing the .NET Core hosting](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

## <a name="configuration-with-webconfig"></a>Konfiguracja z pliku web.config

Skonfigurowano modułu ASP.NET Core `aspNetCore` części `system.webServer` węzeł w tej witrynie *web.config* pliku.

Następujące *web.config* pliku została opublikowana na potrzeby [wdrożenia zależny od struktury](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje modułu ASP.NET Core do obsługi żądań w lokacji:

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

Następujące *web.config* została opublikowana na potrzeby [niezależna wdrożenia](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

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

Gdy aplikacja jest wdrażana na [usługi Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` ścieżka jest równa `\\?\%home%\LogFiles\stdout`. Ścieżka zapisuje dzienniki stdout *LogFiles* folderu, w którym znajduje się automatycznie utworzone przez usługę.

Aby uzyskać informacji na temat konfigurowania aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications>.

### <a name="attributes-of-the-aspnetcore-element"></a>Atrybuty elementu aspNetCore

| Atrybut | Opis | Domyślny |
| --------- | ----------- | :-----: |
| `arguments` | <p>Atrybut opcjonalny ciąg.</p><p>Argumenty do pliku wykonywalnego, określony w **processPath**.</p>| |
| `disableStartUpErrorPage` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</p> | `false` |
| `forwardWindowsAuthToken` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie. Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</p> | `true` |
| `processesPerApplication` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Określa liczbę wystąpień procesu określone w **processPath** ustawienia mogą być przetworzyliśmy na aplikację.</p><p>Ustawienie `processesPerApplication` jest niezalecane. Ten atrybut zostanie usunięty w przyszłych wydaniach.</p> | Wartość domyślna: `1`<br>Minimalna: `1`<br>Maks.: `100` |
| `processPath` | <p>Atrybut wymagany ciąg.</p><p>Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP. Obsługiwane są ścieżki względne. Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</p> | |
| `rapidFailsPerMinute` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę. W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</p> | Wartość domyślna: `10`<br>Minimalna: `0`<br>Maks.: `100` |
| `requestTimeout` | <p>Atrybut opcjonalny przedziału czasu.</p><p>Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</p><p>W wersjach modułu ASP.NET Core, dołączonej do wersji platformy ASP.NET Core 2.1 lub nowszej `requestTimeout` jest określona w godzinach, minutach i sekundach.</p> | Wartość domyślna: `00:02:00`<br>Minimalna: `00:00:00`<br>Maks.: `360:00:00` |
| `shutdownTimeLimit` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</p> | Wartość domyślna: `10`<br>Minimalna: `0`<br>Maks.: `600` |
| `startupTimeLimit` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie. Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu. Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</p><p>Wartość 0 (zero) jest **nie** uważane za nieskończony limit czasu.</p> | Wartość domyślna: `120`<br>Minimalna: `0`<br>Maks.: `3600` |
| `stdoutLogEnabled` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Atrybut opcjonalny ciąg.</p><p>Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane. Są ścieżki względne względem katalogu głównego witryny. Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych. Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika. Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku ( *.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki. Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a>Ustawianie zmiennych środowiskowych

Zmienne środowiskowe można określić dla tego procesu w `processPath` atrybutu. Określ zmienną środowiskową `<environmentVariable>` element podrzędny elementu `<environmentVariables>` elementu kolekcji.

> [!WARNING]
> Zmienne środowiskowe ustawione w tej sekcji powodują konflikt z systemowymi zmiennymi środowiskowymi ustawionymi z tą samą nazwą. Jeśli zmienna środowiskowa jest ustawiona zarówno w pliku *Web. config* , jak i na poziomie systemu w systemie Windows, wartość z pliku *Web. config* zostanie dołączona do systemowej wartości zmiennej środowiskowej ( `ASPNETCORE_ENVIRONMENT: Development;Development`na przykład), która uniemożliwia aplikację od początku.

W poniższym przykładzie ustawiono dwóch zmiennych środowiskowych. `ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji `Development`. Deweloper może tymczasowo ustawić tę wartość w *web.config* pliku, aby wymusić [stronie wyjątków deweloperów](xref:fundamentals/error-handling) załadować podczas debugowania aplikacji wyjątek. `CONFIG_DIR` to przykład zmiennej środowiskowej zdefiniowanej przez użytkownika, gdzie deweloper ma napisany kod, który odczytuje wartość przy uruchamianiu w celu utworzenia ścieżki do ładowania pliku konfiguracji aplikacji.

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

> [!WARNING]
> Ustawić tylko `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej, aby `Development` przejściowym i testowania serwerów, które nie są dostępne do niezaufanych sieci, takich jak Internet.

## <a name="app_offlinehtm"></a>app_offline.htm

Jeśli plik o nazwie *app_offline.htm* wykryte w katalogu głównym aplikacji, modułu ASP.NET Core próbuje łagodne zamykanie aplikacji i zatrzymanie przetwarzania przychodzących żądań. Jeśli aplikacja jest nadal uruchomione po upływie liczby sekund zdefiniowane w `shutdownTimeLimit`, modułu ASP.NET Core to złe rozwiązanie uruchomionego procesu.

Gdy *app_offline.htm* występuje pliku modułu ASP.NET Core ma odpowiadać na żądania, wysyłając ponownie zawartość *app_offline.htm* pliku. Gdy *app_offline.htm* plik jest usuwany, kolejne żądanie uruchamia aplikację.

## <a name="start-up-error-page"></a>Strona błędu uruchamiania

Jeśli modułu ASP.NET Core nie uda się uruchomić procesu wewnętrznej bazy danych lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 - niepowodzenia procesu* zostanie wyświetlona strona kodu stanu. Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 502 usług IIS, należy użyć `disableStartUpErrorPage` atrybutu. Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).

![502.5 strona kodowa stan niepowodzenia procesu](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Tworzenia dziennika i Przekierowanie

Modułu ASP.NET Core przekierowuje dane wyjściowe stdout i stderr konsoli do dysku, jeśli `stdoutLogEnabled` i `stdoutLogFile` atrybuty `aspNetCore` elementu są ustawione. Wszystkie foldery w `stdoutLogFile` ścieżce są tworzone przez moduł po utworzeniu pliku dziennika. Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).

Dzienniki nie są obracane, chyba że wystąpi procesu recykling/ponowne uruchomienie. Jest odpowiedzialny za dostawcy usług hostingowych w celu ograniczenia ilości miejsca na dysku przez dzienniki.

Przy użyciu dziennika stdout jest zalecane tylko na potrzeby rozwiązywania problemów z uruchamianiem aplikacji. Nie używaj dziennika stdout do celów rejestrowania głównej aplikacji. Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje. Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).

Rozszerzenie sygnatur czasowych i pliku są automatycznie dodawane, gdy plik dziennika jest tworzony. Przez dodanie sygnatury czasowej, identyfikator procesu i rozszerzenie pliku składa się nazwa pliku dziennika ( *.log*) do ostatni segment `stdoutLogFile` ścieżki (zazwyczaj *stdout*) rozdzielane znakami podkreślenia. Jeśli `stdoutLogFile` ścieżki kończy się *stdout*, dziennika dla aplikacji za pomocą identyfikatora PID 1934 utworzono 19:42:32 2/5/2018 o nazwie pliku *stdout_20180205194132_1934.log*.

Poniższy przykład `aspNetCore` element konfiguruje rejestrowanie strumienia stdout dla aplikacji hostowanej w usłudze Azure App Service. Ścieżka lokalna lub ścieżkę do udziału sieciowego jest możliwa do logowania lokalnego. Upewnij się, że tożsamość puli aplikacji ma uprawnienia do zapisu w ścieżce podanej.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

Foldery w ścieżce przekazanej do `<handlerSetting>` wartości (*dzienniki* w powyższym przykładzie) nie są automatycznie tworzone przez moduł i powinny być wcześniej dostępne we wdrożeniu. Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).

Zobacz [konfiguracji z pliku web.config](#configuration-with-webconfig) przykład `aspNetCore` element *web.config* pliku.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Konfiguracja serwera proxy korzysta z protokołu HTTP i parowania token

Serwer proxy między modułu ASP.NET Core i Kestrel korzysta z protokołu HTTP. Istnieje ryzyko ochronę ruchu między moduł i Kestrel z lokalizacji poza serwerem.

Parowania tokenu jest używany w celu zagwarantowania, że przekazywane przez usługi IIS żądań odebranych przez Kestrel i nie pochodzi z innego źródła. Parowania token jest utworzona i ustawiona w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez moduł. Token parowania jest również ustawiona w nagłówku (`MS-ASPNETCORE-TOKEN`) na każde żądanie serwerem proxy. Oprogramowanie pośredniczące IIS sprawdzanie żądań odbierze, aby upewnić się, że wartość tokenu nagłówka parowania odpowiada wartości zmiennej środowiskowej. Jeśli token wartości są niezgodne, żądanie jest rejestrowane i odrzucone. Zmienna środowiskowa tokenu parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem. Bez znajomości parowania wartość tokenu, osoba atakująca nie może przesłać żądania, które obchodzą wyboru w oprogramowaniu pośredniczącym usługi IIS.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Moduł ASP.NET Core z programem IIS konfigurację udostępnioną

Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami konta **TrustedInstaller** . Ponieważ konto systemu lokalnego nie ma uprawnień do modyfikowania dla ścieżki udziału używanej przez udostępnioną konfigurację usług IIS, Instalator zgłasza błąd odmowy dostępu podczas próby skonfigurowania ustawień modułu w pliku *ApplicationHost. config* na udział.

Korzystając z konfiguracji udostępnionej usług IIS, wykonaj następujące kroki:

1. Wyłącz konfiguracji udostępnionej usług IIS.
1. Uruchom Instalatora.
1. Eksportuj zaktualizowanego *applicationHost.config* plików do udziału.
1. Ponownie włączyć konfiguracji udostępnionej usług IIS.

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

**Usługi IIS Express (x86/amd64):**

* %ProgramFiles%\IIS Express\aspnetcore.dll

* %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

### <a name="schema"></a>Schemat

**IIS**

* %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

**Usługi IIS Express**

* %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>Konfiguracja

**IIS**

* %windir%\System32\inetsrv\config\applicationHost.config

**Usługi IIS Express**

* Visual Studio: {Aplikacja główna}\\. vs\config\applicationHost.config

* Interfejs wiersza polecenia *iisexpress. exe* :%USERPROFILE%\Documents\IISExpress\config\applicationhost.config

Pliki można znaleźć, wyszukując *aspnetcore* w *applicationHost.config* pliku.

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:host-and-deploy/iis/index>
* [Repozytorium GitHub modułu ASP.NET Core (Źródło odwołania)](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
