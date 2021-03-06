---
title: Moduł ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak skonfigurować modułu ASP.NET Core do hostowania aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 298d424557600735668217e1ef07ace606dac60b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667301"
---
# <a name="aspnet-core-module"></a>Moduł ASP.NET Core

Autorzy [Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Krzysztof Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [sourabh Shirhatti](https://twitter.com/sshirhatti)i [Justin Kotalik](https://github.com/jkotalik)

::: moniker range=">= aspnetcore-3.0"

Moduł ASP.NET Core jest natywnym modułem usług IIS, który jest podłączany do potoku usług IIS:

* Hostowanie aplikacji ASP.NET Core w procesie roboczym usług IIS (`w3wp.exe`), nazywanym [modelem hostingu w procesie](#in-process-hosting-model).
* Przekazuj żądania sieci Web do zaplecza ASP.NET Core aplikacji, na której uruchomiono [serwer Kestrel](xref:fundamentals/servers/kestrel), nazywany [modelem hostingu poza procesem](#out-of-process-hosting-model).

Obsługiwane wersje systemu Windows:

* Windows 7 lub nowszy
* Windows Server 2008 R2 lub nowszy

W przypadku hostingu w procesie moduł używa implementacji serwera w procesie dla usług IIS o nazwie serwer HTTP IIS (`IISHttpServer`).

Podczas hostingu poza procesem moduł działa tylko z Kestrel. Moduł nie działa w przypadku [protokołu HTTP. sys](xref:fundamentals/servers/httpsys).

## <a name="hosting-models"></a>Modele hostingu

### <a name="in-process-hosting-model"></a>Model hostingu w procesie

Domyślnie ASP.NET Core aplikacje do modelu hostingu w procesie.

Następujące właściwości mają zastosowanie w przypadku hostowania w procesie:

* Serwer HTTP IIS (`IISHttpServer`) jest używany zamiast serwera [Kestrel](xref:fundamentals/servers/kestrel) . W przypadku wywołań [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> do:

  * Zarejestruj `IISHttpServer`.
  * Skonfiguruj port i ścieżkę bazową, na której serwer powinien nasłuchiwać przy uruchomionym za modułem ASP.NET Core.
  * Skonfiguruj hosta do przechwytywania błędów uruchamiania.

* [Atrybut requestTimeout](#attributes-of-the-aspnetcore-element) nie ma zastosowania do hostingu w procesie.

* Udostępnianie puli aplikacji między aplikacjami nie jest obsługiwane. Użycie jednej puli aplikacji na aplikację.

* W przypadku korzystania z [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) lub ręcznego umieszczania [pliku app_offline. htm we wdrożeniu](xref:host-and-deploy/iis/index#locked-deployment-files)aplikacja może nie zostać wyłączona natychmiast, jeśli istnieje otwarte połączenie. Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.

* Architektura (liczba bitów) zainstalowanego środowiska uruchomieniowego (x64 lub x86) i aplikacji musi być zgodna z architekturą puli aplikacji.

* Rozłącza klienta są wykrywane. Token anulowania [HttpContext. RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) został anulowany po rozłączeniu klienta.

* W ASP.NET Core 2.2.1 lub wcześniejszym, <xref:System.IO.Directory.GetCurrentDirectory*> zwraca katalog procesów roboczych procesu uruchomionego przez usługi IIS, a nie katalog aplikacji (na przykład *C:\Windows\System32\inetsrv* for *w3wp. exe*).

  Przykładowy kod, który ustawia bieżący katalog aplikacji, znajduje się w [klasie CurrentDirectoryHelpers](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs). Wywołaj metodę `SetCurrentDirectory`. Kolejne wywołania <xref:System.IO.Directory.GetCurrentDirectory*> podania katalogu aplikacji.

* Podczas hostingu w procesie <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nie jest wywoływana wewnętrznie w celu zainicjowania użytkownika. W związku z tym, implementacja <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> używana do przekształcania oświadczeń po każdym uwierzytelnieniu nie jest domyślnie aktywowana. Podczas przekształcania oświadczeń z implementacją <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> Wywołaj <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, aby dodać usługi uwierzytelniania:

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
  
  * [Wdrożenia pakietów sieci Web (jednego pliku)](/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages) nie są obsługiwane.

### <a name="out-of-process-hosting-model"></a>Model hostingu poza procesem

Aby skonfigurować aplikację do hostingu poza procesem, ustaw wartość właściwości `<AspNetCoreHostingModel>` na `OutOfProcess` w pliku projektu ( *. csproj*):

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

Hosting w procesie jest ustawiany przy użyciu `InProcess`, który jest wartością domyślną.

W wartości `<AspNetCoreHostingModel>` wielkość liter nie jest rozróżniana, dlatego `inprocess` i `outofprocess` są prawidłowymi wartościami.

Serwer [Kestrel](xref:fundamentals/servers/kestrel) jest używany zamiast serwera http usług IIS (`IISHttpServer`).

W przypadku pozaprocesowych [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) wywołań <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> do:

* Skonfiguruj port i ścieżkę bazową, na której serwer powinien nasłuchiwać przy uruchomionym za modułem ASP.NET Core.
* Skonfiguruj hosta do przechwytywania błędów uruchamiania.

### <a name="hosting-model-changes"></a>Zmiany modelu hostingu

Jeśli ustawienie `hostingModel` zostanie zmienione w pliku *Web. config* (wyjaśniono w sekcji [Konfiguracja z plikiem Web. config](#configuration-with-webconfig) ), moduł odtwarza proces roboczy dla usług IIS.

Dla usług IIS Express moduł nie odtworzyć proces roboczy, ale zamiast tego wyzwala łagodne zamykanie bieżący proces usług IIS Express. Kolejne żądanie aplikacji spowoduje utworzenie nowych procesów usług IIS Express.

### <a name="process-name"></a>Nazwa procesu

`Process.GetCurrentProcess().ProcessName` raporty `w3wp`/`iisexpress` (w procesie) lub `dotnet` (pozaprocesowe).

Wiele modułów macierzystych, takich jak uwierzytelnianie systemu Windows, pozostaje aktywnych. Aby dowiedzieć się więcej na temat modułów usług IIS aktywnych przy użyciu modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/modules>.

Moduł ASP.NET Core może również:

* Ustaw zmienne środowiskowe dla procesu roboczego.
* Rejestruj dane wyjściowe stdout do magazynu plików w celu rozwiązywania problemów z uruchamianiem.
* Przekazuj tokeny uwierzytelniania systemu Windows.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>Jak zainstalować moduł ASP.NET Core i korzystać z niego

Aby uzyskać instrukcje dotyczące sposobu instalowania modułu ASP.NET Core, zobacz [installing the .NET Core hosting](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

## <a name="configuration-with-webconfig"></a>Konfiguracja z pliku web.config

Moduł ASP.NET Core jest skonfigurowany z sekcją `aspNetCore` węzła `system.webServer` w pliku *Web. config* witryny.

Następujący plik *Web. config* jest publikowany dla [wdrożenia zależnego od platformy](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje moduł ASP.NET Core do obsługi żądań lokacji:

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
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

Następująca *plik Web. config* jest publikowana dla [samodzielnego wdrożenia](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

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
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

Właściwość <xref:System.Configuration.SectionInformation.InheritInChildApplications*> jest ustawiona na `false`, aby wskazać, że ustawienia określone w [\<lokalizacji >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) elementu nie są dziedziczone przez aplikacje, które znajdują się w podkatalogu aplikacji.

Po wdrożeniu aplikacji do [Azure App Service](https://azure.microsoft.com/services/app-service/)ścieżka `stdoutLogFile` jest ustawiona na `\\?\%home%\LogFiles\stdout`. Ścieżka zapisuje dzienniki stdout do folderu *LogFiles* , który jest lokalizacją automatycznie utworzoną przez usługę.

Aby uzyskać informacje na temat konfiguracji aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications>.

### <a name="attributes-of-the-aspnetcore-element"></a>Atrybuty elementu aspNetCore

| Atrybut | Opis | Domyślne |
| --------- | ----------- | :-----: |
| `arguments` | <p>Atrybut opcjonalny ciąg.</p><p>Argumenty do pliku wykonywalnego określonego w **processPath**.</p> | |
| `disableStartUpErrorPage` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku wartości true strona **błędu 502,5 procesu** jest pomijana, a strona kodowa stanu 502 skonfigurowana w *pliku Web. config* ma pierwszeństwo.</p> | `false` |
| `forwardWindowsAuthToken` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie. Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</p> | `true` |
| `hostingModel` | <p>Atrybut opcjonalny ciąg.</p><p>Określa model hostingu jako proces (`InProcess`/`inprocess`) lub pozaprocesowe (`OutOfProcess`/`outofprocess`).</p> | `InProcess`<br>`inprocess` |
| `processesPerApplication` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Określa liczbę wystąpień procesu określonego w ustawieniu **processPath** , które może być przypadające na aplikację.</p><p>&dagger;w przypadku hostingu w procesie wartość jest ograniczona do `1`.</p><p>Nie zaleca się ustawienia `processesPerApplication`. Ten atrybut zostanie usunięty w przyszłych wydaniach.</p> | Wartość domyślna: `1`<br>Minimum: `1`<br>Maks.: `100`&dagger; |
| `processPath` | <p>Atrybut wymagany ciąg.</p><p>Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP. Obsługiwane są ścieżki względne. Jeśli ścieżka rozpoczyna się od `.`, ścieżka jest uznawana za względną względem katalogu głównego witryny.</p> | |
| `rapidFailsPerMinute` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Określa, ile razy proces określony w **processPath** może ulec awarii na minutę. W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</p><p>Nieobsługiwane za pomocą wewnątrzprocesowego hostingu.</p> | Wartość domyślna: `10`<br>Minimum: `0`<br>Maks.: `100` |
| `requestTimeout` | <p>Atrybut opcjonalny przedziału czasu.</p><p>Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</p><p>W wersjach modułu ASP.NET Core, który został dostarczony z wersją ASP.NET Core 2,1 lub nowszą, `requestTimeout` jest określony w godzinach, minutach i sekundach.</p><p>Nie ma zastosowania do hostowania w procesie. Dla hostingu w procesie modułu czeka na aplikację, aby przetworzyć żądanie.</p><p>Prawidłowe wartości segmentów minut i sekund ciągu mieszczą się w zakresie 0-59. Użycie **60** w wartości minut lub sekund skutkuje *błędem wewnętrznego serwera 500*.</p> | Wartość domyślna: `00:02:00`<br>Minimum: `00:00:00`<br>Maks.: `360:00:00` |
| `shutdownTimeLimit` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Czas w sekundach, przez który moduł czeka, aż plik wykonywalny zostanie bezpiecznie zamknięty po wykryciu pliku *app_offline. htm* .</p> | Wartość domyślna: `10`<br>Minimum: `0`<br>Maks.: `600` |
| `startupTimeLimit` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie. Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu. Moduł podejmuje próbę ponownego uruchomienia procesu, gdy odbierze nowe żądanie i kontynuuje ponowne uruchomienie procesu na kolejnych żądaniach przychodzących, chyba że aplikacja nie będzie mogła uruchomić **rapidFailsPerMinute** liczbę razy w ostatniej minucie.</p><p>Wartość 0 (zero) **nie** jest uważana za nieskończony limit czasu.</p> | Wartość domyślna: `120`<br>Minimum: `0`<br>Maks.: `3600` |
| `stdoutLogEnabled` | <p>Opcjonalny logiczny atrybut.</p><p>Jeśli wartość jest równa true, **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Atrybut opcjonalny ciąg.</p><p>Określa względną lub bezwzględną ścieżkę do pliku, dla którego jest rejestrowany **stdout** i **stderr** z procesu określonego w **processPath** . Są ścieżki względne względem katalogu głównego witryny. Każda ścieżka rozpoczynająca się od `.` jest określana względem katalogu głównego witryny, a wszystkie inne ścieżki są traktowane jako ścieżki bezwzględne. Wszystkie foldery podane w ścieżce są tworzone przez moduł po utworzeniu pliku dziennika. Przy użyciu ograniczników podkreślenia, sygnatury czasowej, identyfikatora procesu i rozszerzenia pliku (*log*) są dodawane do ostatniego segmentu ścieżki **stdoutLogFile** . Jeśli `.\logs\stdout` jest podana jako wartość, przykładowy dziennik stdout jest zapisywany jako *stdout_20180205194132_1934. log* w folderze *Logs* , gdy Zapisano na 2/5/2018 o GODZINie 19:41:32 z identyfikatorem procesu 1934.</p> | `aspnetcore-stdout` |

### <a name="set-environment-variables"></a>Ustawianie zmiennych środowiskowych

Zmienne środowiskowe można określić dla procesu w atrybucie `processPath`. Określ zmienną środowiskową z `<environmentVariable>` elementem podrzędnym elementu `<environmentVariables>` kolekcji. Zmienne środowiskowe ustawione w tej sekcji pierwszeństwo systemowe zmienne środowiskowe.

W poniższym przykładzie są ustawiane dwie zmienne środowiskowe w *pliku Web. config*. `ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji do `Development`. Deweloper może tymczasowo ustawić tę wartość w pliku *Web. config* w celu wymuszenia załadowania [strony wyjątku dewelopera](xref:fundamentals/error-handling) podczas debugowania wyjątku aplikacji. `CONFIG_DIR` jest przykładem zmiennej środowiskowej zdefiniowanej przez użytkownika, w której programista zapisał kod, który odczytuje wartość przy uruchamianiu, aby utworzyć ścieżkę do ładowania pliku konfiguracji aplikacji.

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!NOTE]
> Alternatywą dla ustawienia środowiska bezpośrednio w pliku *Web. config* jest uwzględnienie właściwości `<EnvironmentName>` w [profilu publikacji (. pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) lub plik projektu. To podejście ustawia środowisko w *pliku Web. config* po opublikowaniu projektu:
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> Ustaw zmienną środowiskową `ASPNETCORE_ENVIRONMENT` na `Development` na serwerach przejściowych i testowych, które nie są dostępne dla niezaufanych sieci, takich jak Internet.

## <a name="app_offlinehtm"></a>app_offline.htm

Jeśli plik o nazwie *app_offline. htm* zostanie wykryty w katalogu głównym aplikacji, moduł ASP.NET Core próbuje bezpiecznie zamknąć aplikację i zatrzymać przetwarzanie żądań przychodzących. Jeśli aplikacja nadal działa po upływie liczby sekund zdefiniowanej w `shutdownTimeLimit`, moduł ASP.NET Core kasuje uruchomiony proces.

Gdy plik *app_offline. htm* jest obecny, moduł ASP.NET Core reaguje na żądania, wysyłając z powrotem zawartość pliku *app_offline. htm* . Po usunięciu pliku *app_offline. htm* następnym żądaniu zostanie uruchomiona aplikacja.

Korzystając z modelu hostingu poza procesem, aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia. Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.

## <a name="start-up-error-page"></a>Strona błędu uruchamiania

Zarówno w trakcie, jak i spoza procesu hostingu generuje strony błędów niestandardowych, gdy mogą zakończyć się niepowodzeniem uruchomić aplikację.

Jeśli moduł ASP.NET Core nie może odnaleźć programu obsługi żądania w procesie lub out-of-process, zostanie wyświetlona strona kodowa stanu *niepowodzenia ładowania 500,0-procesowego/wyjściowego* .

W przypadku hostingu w procesie, Jeśli uruchomienie aplikacji przez moduł ASP.NET Core nie powiedzie się, zostanie wyświetlona strona kod stanu *niepowodzenia 500,30* .

W przypadku hostingu poza procesem, jeśli moduł ASP.NET Core nie może uruchomić procesu zaplecza lub proces zaplecza zostanie uruchomiony, ale nie nasłuchuje na skonfigurowanym porcie, zostanie wyświetlona strona kod stanu *niepowodzenia procesu 502,5* .

Aby pominąć tę stronę i przywrócić domyślną stronę kodową stanu 5xx usługi IIS, Użyj atrybutu `disableStartUpErrorPage`. Aby uzyskać więcej informacji o konfigurowaniu niestandardowych komunikatów o błędach, zobacz [Błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).

## <a name="log-creation-and-redirection"></a>Tworzenia dziennika i Przekierowanie

Moduł ASP.NET Core przekierowuje dane wyjściowe z konsoli stdout i stderr do dysku, jeśli są ustawione atrybuty `stdoutLogEnabled` i `stdoutLogFile` elementu `aspNetCore`. Wszystkie foldery w ścieżce `stdoutLogFile` są tworzone przez moduł po utworzeniu pliku dziennika. Pula aplikacji musi mieć dostęp do zapisu w lokalizacji, w której zapisano dzienniki (Użyj `IIS AppPool\<app_pool_name>`, aby zapewnić uprawnienia do zapisu).

Dzienniki nie są obracane, chyba że wystąpi procesu recykling/ponowne uruchomienie. Jest odpowiedzialny za dostawcy usług hostingowych w celu ograniczenia ilości miejsca na dysku przez dzienniki.

Użycie dziennika stdout jest zalecane tylko w przypadku rozwiązywania problemów z uruchamianiem aplikacji w usługach IIS lub w przypadku korzystania z [obsługi usług IIS w czasie projektowania w programie Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), a nie podczas debugowania lokalnego i uruchamiania aplikacji przy użyciu IIS Express.

Nie używaj dziennika stdout do celów rejestrowania głównej aplikacji. Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje. Aby uzyskać więcej informacji, zobacz [dostawców rejestrowania innych](xref:fundamentals/logging/index#third-party-logging-providers)firm.

Rozszerzenie sygnatur czasowych i pliku są automatycznie dodawane, gdy plik dziennika jest tworzony. Nazwa pliku dziennika składa się z dołączania sygnatury czasowej, identyfikatora procesu i rozszerzenia pliku (*log*) do ostatniego segmentu ścieżki `stdoutLogFile` (zazwyczaj *stdout*) rozdzielanej znakami podkreślenia. Jeśli ścieżka `stdoutLogFile` kończy się na *stdout*, dziennik aplikacji o identyfikatorze PID 1934 utworzony w dniu 2/5/2018 o 19:42:32 ma nazwę pliku *stdout_20180205194132_1934. log*.

Jeśli `stdoutLogEnabled` ma wartość false, błędy występujące podczas uruchamiania aplikacji są przechwytywane i emitowane do dziennika zdarzeń do 30 KB. Po uruchomieniu wszystkie dodatkowe dzienniki są odrzucane.

Poniższy przykład `aspNetCore` element konfiguruje rejestrowanie stdout w ścieżce względnej `.\log\`. Upewnij się, że tożsamość puli aplikacji ma uprawnienia do zapisu w ścieżce podanej.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

W przypadku publikowania aplikacji na potrzeby wdrożenia Azure App Service zestaw SDK sieci Web ustawia wartość `stdoutLogFile` na `\\?\%home%\LogFiles\stdout`. Zmienna środowiskowa `%home` jest wstępnie zdefiniowana dla aplikacji hostowanych przez Azure App Service.

Aby utworzyć reguły filtru rejestrowania, zobacz sekcje [Konfiguracja](xref:fundamentals/logging/index#log-filtering) i [filtrowanie dzienników](xref:fundamentals/logging/index#log-filtering) w dokumentacji rejestrowania ASP.NET Core.

Aby uzyskać więcej informacji na temat formatów ścieżki, zobacz [formaty ścieżki plików w systemach Windows](/dotnet/standard/io/file-path-formats).

## <a name="enhanced-diagnostic-logs"></a>Rozszerzone dzienników diagnostycznych

Moduł ASP.NET Core można skonfigurować w celu udostępnienia dzienników diagnostyki rozszerzonej. Dodaj element `<handlerSettings>` do elementu `<aspNetCore>` w *pliku Web. config*. Ustawienie `debugLevel` na `TRACE` uwidacznia większą wierność informacji diagnostycznych:

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

Wszystkie foldery w ścieżce (*dzienniki* w poprzednim przykładzie) są tworzone przez moduł po utworzeniu pliku dziennika. Pula aplikacji musi mieć dostęp do zapisu w lokalizacji, w której zapisano dzienniki (Użyj `IIS AppPool\<app_pool_name>`, aby zapewnić uprawnienia do zapisu).

Wartości poziomu debugowania (`debugLevel`) mogą zawierać zarówno poziom, jak i lokalizację.

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

* `ASPNETCORE_MODULE_DEBUG_FILE` &ndash; ścieżkę do pliku dziennika debugowania. (Domyślnie: *aspnetcore-Debug. log*)
* `ASPNETCORE_MODULE_DEBUG` &ndash; ustawienia poziomu debugowania.

> [!WARNING]
> **Nie** pozostawiaj włączonej rejestracji debugowania w ramach wdrożenia dłużej niż jest to wymagane, aby rozwiązać problem. Rozmiar dziennika nie jest ograniczona. Pozostawienie dziennik debugowania włączone może wyczerpać dostępne miejsce na dysku i awarii serwera lub usługi app service.

Zobacz temat [Konfiguracja z plikiem Web. config](#configuration-with-webconfig) , aby zapoznać się z przykładem elementu `aspNetCore` w pliku *Web. config* .

## <a name="modify-the-stack-size"></a>Modyfikowanie rozmiaru stosu

*Stosuje się tylko w przypadku korzystania z modelu hostingu w procesie.*

Skonfiguruj rozmiar zarządzanego stosu przy użyciu ustawienia `stackSize` w bajtach w *pliku Web. config*. Domyślny rozmiar to `1048576` bajtów (1 MB).

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="stackSize" value="2097152" />
  </handlerSettings>
</aspNetCore>
```

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Konfiguracja serwera proxy korzysta z protokołu HTTP i parowania token

*Dotyczy tylko hostingu poza procesem.*

Serwer proxy między modułu ASP.NET Core i Kestrel korzysta z protokołu HTTP. Istnieje ryzyko ochronę ruchu między moduł i Kestrel z lokalizacji poza serwerem.

Parowania tokenu jest używany w celu zagwarantowania, że przekazywane przez usługi IIS żądań odebranych przez Kestrel i nie pochodzi z innego źródła. Token parowania jest tworzony i ustawiany w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez moduł. Token parowania jest również ustawiany w nagłówku (`MS-ASPNETCORE-TOKEN`) na każdym żądaniu z serwerem proxy. Oprogramowanie pośredniczące IIS sprawdzanie żądań odbierze, aby upewnić się, że wartość tokenu nagłówka parowania odpowiada wartości zmiennej środowiskowej. Jeśli token wartości są niezgodne, żądanie jest rejestrowane i odrzucone. Zmienna środowiskowa tokenu parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem. Bez znajomości parowania wartość tokenu, osoba atakująca nie może przesłać żądania, które obchodzą wyboru w oprogramowaniu pośredniczącym usługi IIS.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Moduł ASP.NET Core z programem IIS konfigurację udostępnioną

Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami konta **TrustedInstaller** . Ponieważ konto systemu lokalnego nie ma uprawnień do modyfikowania dla ścieżki udziału używanej przez udostępnioną konfigurację usług IIS, Instalator zgłasza błąd odmowy dostępu podczas próby skonfigurowania ustawień modułu w pliku *ApplicationHost. config* w udziale.

W przypadku korzystania z konfiguracji udostępnionej przez usługi IIS na tym samym komputerze, na którym znajduje się instalacja usług IIS, uruchom Instalatora pakietu ASP.NET Core hostowania z parametrem `OPT_NO_SHARED_CONFIG_CHECK` ustawionym na `1`:

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

Jeśli ścieżka do konfiguracji udostępnionej nie znajduje się na tym samym komputerze, na którym znajduje się instalacja usług IIS, wykonaj następujące czynności:

1. Wyłącz konfiguracji udostępnionej usług IIS.
1. Uruchom Instalatora.
1. Wyeksportuj zaktualizowany plik *ApplicationHost. config* do udziału.
1. Ponownie włączyć konfiguracji udostępnionej usług IIS.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Wersja modułu oraz Dzienniki Instalatora pakietu hostingu

Aby określić wersję zainstalowanego modułu ASP.NET Core:

1. W systemie hostingu przejdź do *%windir%\System32\inetsrv*.
1. Znajdź plik *aspnetcore. dll* .
1. Kliknij prawym przyciskiem myszy plik i wybierz polecenie **Właściwości** z menu kontekstowego.
1. Wybierz kartę **szczegóły** . **Wersja pliku** i **Wersja produktu** reprezentują zainstalowaną wersję modułu.

Dzienniki Instalatora pakietu hostingu dla modułu znajdują się pod adresem *C:\\użytkownicy\\% username%\\AppData\\lokalnego\\temp*. Plik ma nazwę *dd_DotNetCoreWinSvrHosting__\<sygnatura czasowa > _000_AspNetCoreModule_x64. log*.

## <a name="module-schema-and-configuration-file-locations"></a>Lokalizacje pliku modułu, schematu i konfiguracji

### <a name="module"></a>Moduł

**IIS (x86/amd64):**

* %windir%\System32\inetsrv\aspnetcore.dll

* %windir%\SysWOW64\inetsrv\aspnetcore.dll

* %ProgramFiles%\IIS\Asp.NET core Module\V2\aspnetcorev2.dll

* % ProgramFiles (x86) %\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll

**IIS Express (x86/amd64):**

* %ProgramFiles%\IIS Express\aspnetcore.dll

* %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

* %ProgramFiles%\iis Express\Asp.Net Core Module\V2\aspnetcorev2.dll

* % ProgramFiles (x86) %\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll

### <a name="schema"></a>Schemat

**IIS**

* %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

* %Windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML

**IIS Express**

* %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

* %ProgramFiles%\iis Express\config\schema\aspnetcore_schema_v2.xml

### <a name="configuration"></a>Konfiguracja

**IIS**

* %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

* Visual Studio: {Aplikacja główna}\\. vs\config\applicationHost.config

* Interfejs wiersza polecenia *iisexpress. exe* :%USERPROFILE%\Documents\IISExpress\config\applicationhost.config

Pliki można znaleźć, wyszukując *aspnetcore* w pliku *ApplicationHost. config* .

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Moduł ASP.NET Core jest natywnym modułem usług IIS, który jest podłączany do potoku usług IIS:

* Hostowanie aplikacji ASP.NET Core w procesie roboczym usług IIS (`w3wp.exe`), nazywanym [modelem hostingu w procesie](#in-process-hosting-model).
* Przekazuj żądania sieci Web do zaplecza ASP.NET Core aplikacji, na której uruchomiono [serwer Kestrel](xref:fundamentals/servers/kestrel), nazywany [modelem hostingu poza procesem](#out-of-process-hosting-model).

Obsługiwane wersje systemu Windows:

* Windows 7 lub nowszy
* Windows Server 2008 R2 lub nowszy

W przypadku hostingu w procesie moduł używa implementacji serwera w procesie dla usług IIS o nazwie serwer HTTP IIS (`IISHttpServer`).

Podczas hostingu poza procesem moduł działa tylko z Kestrel. Moduł nie działa w przypadku [protokołu HTTP. sys](xref:fundamentals/servers/httpsys).

## <a name="hosting-models"></a>Modele hostingu

### <a name="in-process-hosting-model"></a>Model hostingu w procesie

Aby skonfigurować aplikację do hostingu w procesie, Dodaj właściwość `<AspNetCoreHostingModel>` do pliku projektu aplikacji o wartości `InProcess` (hosting poza procesem jest ustawiany z `OutOfProcess`):

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

Model hostingu w procesie nie jest obsługiwany w przypadku aplikacji ASP.NET Core przeznaczonych dla .NET Framework.

W wartości `<AspNetCoreHostingModel>` wielkość liter nie jest rozróżniana, dlatego `inprocess` i `outofprocess` są prawidłowymi wartościami.

Jeśli właściwość `<AspNetCoreHostingModel>` nie jest obecna w pliku, wartość domyślna to `OutOfProcess`.

Następujące właściwości mają zastosowanie w przypadku hostowania w procesie:

* Serwer HTTP IIS (`IISHttpServer`) jest używany zamiast serwera [Kestrel](xref:fundamentals/servers/kestrel) . W przypadku wywołań [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> do:

  * Zarejestruj `IISHttpServer`.
  * Skonfiguruj port i ścieżkę bazową, na której serwer powinien nasłuchiwać przy uruchomionym za modułem ASP.NET Core.
  * Skonfiguruj hosta do przechwytywania błędów uruchamiania.

* [Atrybut requestTimeout](#attributes-of-the-aspnetcore-element) nie ma zastosowania do hostingu w procesie.

* Udostępnianie puli aplikacji między aplikacjami nie jest obsługiwane. Użycie jednej puli aplikacji na aplikację.

* W przypadku korzystania z [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) lub ręcznego umieszczania [pliku app_offline. htm we wdrożeniu](xref:host-and-deploy/iis/index#locked-deployment-files)aplikacja może nie zostać wyłączona natychmiast, jeśli istnieje otwarte połączenie. Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.

* Architektura (liczba bitów) zainstalowanego środowiska uruchomieniowego (x64 lub x86) i aplikacji musi być zgodna z architekturą puli aplikacji.

* Rozłącza klienta są wykrywane. Token anulowania [HttpContext. RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) został anulowany po rozłączeniu klienta.

* W ASP.NET Core 2.2.1 lub wcześniejszym, <xref:System.IO.Directory.GetCurrentDirectory*> zwraca katalog procesów roboczych procesu uruchomionego przez usługi IIS, a nie katalog aplikacji (na przykład *C:\Windows\System32\inetsrv* for *w3wp. exe*).

  Przykładowy kod, który ustawia bieżący katalog aplikacji, znajduje się w [klasie CurrentDirectoryHelpers](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs). Wywołaj metodę `SetCurrentDirectory`. Kolejne wywołania <xref:System.IO.Directory.GetCurrentDirectory*> podania katalogu aplikacji.

* Podczas hostingu w procesie <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nie jest wywoływana wewnętrznie w celu zainicjowania użytkownika. W związku z tym, implementacja <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> używana do przekształcania oświadczeń po każdym uwierzytelnieniu nie jest domyślnie aktywowana. Podczas przekształcania oświadczeń z implementacją <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> Wywołaj <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, aby dodać usługi uwierzytelniania:

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

* Nie określaj właściwości `<AspNetCoreHostingModel>`. Jeśli właściwość `<AspNetCoreHostingModel>` nie jest obecna w pliku, wartość domyślna to `OutOfProcess`.
* Ustaw wartość właściwości `<AspNetCoreHostingModel>` na `OutOfProcess` (hosting w procesie jest ustawiany przy użyciu `InProcess`):

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

W wartości jest uwzględniana wielkość liter, dlatego `inprocess` i `outofprocess` są prawidłowymi wartościami.

Serwer [Kestrel](xref:fundamentals/servers/kestrel) jest używany zamiast serwera http usług IIS (`IISHttpServer`).

W przypadku pozaprocesowych [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) wywołań <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> do:

* Skonfiguruj port i ścieżkę bazową, na której serwer powinien nasłuchiwać przy uruchomionym za modułem ASP.NET Core.
* Skonfiguruj hosta do przechwytywania błędów uruchamiania.

### <a name="hosting-model-changes"></a>Zmiany modelu hostingu

Jeśli ustawienie `hostingModel` zostanie zmienione w pliku *Web. config* (wyjaśniono w sekcji [Konfiguracja z plikiem Web. config](#configuration-with-webconfig) ), moduł odtwarza proces roboczy dla usług IIS.

Dla usług IIS Express moduł nie odtworzyć proces roboczy, ale zamiast tego wyzwala łagodne zamykanie bieżący proces usług IIS Express. Kolejne żądanie aplikacji spowoduje utworzenie nowych procesów usług IIS Express.

### <a name="process-name"></a>Nazwa procesu

`Process.GetCurrentProcess().ProcessName` raporty `w3wp`/`iisexpress` (w procesie) lub `dotnet` (pozaprocesowe).

Wiele modułów macierzystych, takich jak uwierzytelnianie systemu Windows, pozostaje aktywnych. Aby dowiedzieć się więcej na temat modułów usług IIS aktywnych przy użyciu modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/modules>.

Moduł ASP.NET Core może również:

* Ustaw zmienne środowiskowe dla procesu roboczego.
* Rejestruj dane wyjściowe stdout do magazynu plików w celu rozwiązywania problemów z uruchamianiem.
* Przekazuj tokeny uwierzytelniania systemu Windows.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>Jak zainstalować moduł ASP.NET Core i korzystać z niego

Aby uzyskać instrukcje dotyczące sposobu instalowania modułu ASP.NET Core, zobacz [installing the .NET Core hosting](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

## <a name="configuration-with-webconfig"></a>Konfiguracja z pliku web.config

Moduł ASP.NET Core jest skonfigurowany z sekcją `aspNetCore` węzła `system.webServer` w pliku *Web. config* witryny.

Następujący plik *Web. config* jest publikowany dla [wdrożenia zależnego od platformy](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje moduł ASP.NET Core do obsługi żądań lokacji:

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
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

Następująca *plik Web. config* jest publikowana dla [samodzielnego wdrożenia](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

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
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

Właściwość <xref:System.Configuration.SectionInformation.InheritInChildApplications*> jest ustawiona na `false`, aby wskazać, że ustawienia określone w [\<lokalizacji >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) elementu nie są dziedziczone przez aplikacje, które znajdują się w podkatalogu aplikacji.

Po wdrożeniu aplikacji do [Azure App Service](https://azure.microsoft.com/services/app-service/)ścieżka `stdoutLogFile` jest ustawiona na `\\?\%home%\LogFiles\stdout`. Ścieżka zapisuje dzienniki stdout do folderu *LogFiles* , który jest lokalizacją automatycznie utworzoną przez usługę.

Aby uzyskać informacje na temat konfiguracji aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications>.

### <a name="attributes-of-the-aspnetcore-element"></a>Atrybuty elementu aspNetCore

| Atrybut | Opis | Domyślne |
| --------- | ----------- | :-----: |
| `arguments` | <p>Atrybut opcjonalny ciąg.</p><p>Argumenty do pliku wykonywalnego określonego w **processPath**.</p> | |
| `disableStartUpErrorPage` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku wartości true strona **błędu 502,5 procesu** jest pomijana, a strona kodowa stanu 502 skonfigurowana w *pliku Web. config* ma pierwszeństwo.</p> | `false` |
| `forwardWindowsAuthToken` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie. Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</p> | `true` |
| `hostingModel` | <p>Atrybut opcjonalny ciąg.</p><p>Określa model hostingu jako proces (`InProcess`/`inprocess`) lub pozaprocesowe (`OutOfProcess`/`outofprocess`).</p> | `OutOfProcess`<br>`outofprocess` |
| `processesPerApplication` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Określa liczbę wystąpień procesu określonego w ustawieniu **processPath** , które może być przypadające na aplikację.</p><p>&dagger;w przypadku hostingu w procesie wartość jest ograniczona do `1`.</p><p>Nie zaleca się ustawienia `processesPerApplication`. Ten atrybut zostanie usunięty w przyszłych wydaniach.</p> | Wartość domyślna: `1`<br>Minimum: `1`<br>Maks.: `100`&dagger; |
| `processPath` | <p>Atrybut wymagany ciąg.</p><p>Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP. Obsługiwane są ścieżki względne. Jeśli ścieżka rozpoczyna się od `.`, ścieżka jest uznawana za względną względem katalogu głównego witryny.</p> | |
| `rapidFailsPerMinute` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Określa, ile razy proces określony w **processPath** może ulec awarii na minutę. W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</p><p>Nieobsługiwane za pomocą wewnątrzprocesowego hostingu.</p> | Wartość domyślna: `10`<br>Minimum: `0`<br>Maks.: `100` |
| `requestTimeout` | <p>Atrybut opcjonalny przedziału czasu.</p><p>Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</p><p>W wersjach modułu ASP.NET Core, który został dostarczony z wersją ASP.NET Core 2,1 lub nowszą, `requestTimeout` jest określony w godzinach, minutach i sekundach.</p><p>Nie ma zastosowania do hostowania w procesie. Dla hostingu w procesie modułu czeka na aplikację, aby przetworzyć żądanie.</p><p>Prawidłowe wartości segmentów minut i sekund ciągu mieszczą się w zakresie 0-59. Użycie **60** w wartości minut lub sekund skutkuje *błędem wewnętrznego serwera 500*.</p> | Wartość domyślna: `00:02:00`<br>Minimum: `00:00:00`<br>Maks.: `360:00:00` |
| `shutdownTimeLimit` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Czas w sekundach, przez który moduł czeka, aż plik wykonywalny zostanie bezpiecznie zamknięty po wykryciu pliku *app_offline. htm* .</p> | Wartość domyślna: `10`<br>Minimum: `0`<br>Maks.: `600` |
| `startupTimeLimit` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie. Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu. Moduł podejmuje próbę ponownego uruchomienia procesu, gdy odbierze nowe żądanie i kontynuuje ponowne uruchomienie procesu na kolejnych żądaniach przychodzących, chyba że aplikacja nie będzie mogła uruchomić **rapidFailsPerMinute** liczbę razy w ostatniej minucie.</p><p>Wartość 0 (zero) **nie** jest uważana za nieskończony limit czasu.</p> | Wartość domyślna: `120`<br>Minimum: `0`<br>Maks.: `3600` |
| `stdoutLogEnabled` | <p>Opcjonalny logiczny atrybut.</p><p>Jeśli wartość jest równa true, **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Atrybut opcjonalny ciąg.</p><p>Określa względną lub bezwzględną ścieżkę do pliku, dla którego jest rejestrowany **stdout** i **stderr** z procesu określonego w **processPath** . Są ścieżki względne względem katalogu głównego witryny. Każda ścieżka rozpoczynająca się od `.` jest określana względem katalogu głównego witryny, a wszystkie inne ścieżki są traktowane jako ścieżki bezwzględne. Wszystkie foldery podane w ścieżce są tworzone przez moduł po utworzeniu pliku dziennika. Przy użyciu ograniczników podkreślenia, sygnatury czasowej, identyfikatora procesu i rozszerzenia pliku (*log*) są dodawane do ostatniego segmentu ścieżki **stdoutLogFile** . Jeśli `.\logs\stdout` jest podana jako wartość, przykładowy dziennik stdout jest zapisywany jako *stdout_20180205194132_1934. log* w folderze *Logs* , gdy Zapisano na 2/5/2018 o GODZINie 19:41:32 z identyfikatorem procesu 1934.</p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a>Ustawianie zmiennych środowiskowych

Zmienne środowiskowe można określić dla procesu w atrybucie `processPath`. Określ zmienną środowiskową z `<environmentVariable>` elementem podrzędnym elementu `<environmentVariables>` kolekcji. Zmienne środowiskowe ustawione w tej sekcji pierwszeństwo systemowe zmienne środowiskowe.

W poniższym przykładzie ustawiono dwóch zmiennych środowiskowych. `ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji do `Development`. Deweloper może tymczasowo ustawić tę wartość w pliku *Web. config* w celu wymuszenia załadowania [strony wyjątku dewelopera](xref:fundamentals/error-handling) podczas debugowania wyjątku aplikacji. `CONFIG_DIR` jest przykładem zmiennej środowiskowej zdefiniowanej przez użytkownika, w której programista zapisał kod, który odczytuje wartość przy uruchamianiu, aby utworzyć ścieżkę do ładowania pliku konfiguracji aplikacji.

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!NOTE]
> Alternatywą dla ustawienia środowiska bezpośrednio w pliku *Web. config* jest uwzględnienie właściwości `<EnvironmentName>` w [profilu publikacji (. pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) lub plik projektu. To podejście ustawia środowisko w *pliku Web. config* po opublikowaniu projektu:
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> Ustaw zmienną środowiskową `ASPNETCORE_ENVIRONMENT` na `Development` na serwerach przejściowych i testowych, które nie są dostępne dla niezaufanych sieci, takich jak Internet.

## <a name="app_offlinehtm"></a>app_offline.htm

Jeśli plik o nazwie *app_offline. htm* zostanie wykryty w katalogu głównym aplikacji, moduł ASP.NET Core próbuje bezpiecznie zamknąć aplikację i zatrzymać przetwarzanie żądań przychodzących. Jeśli aplikacja nadal działa po upływie liczby sekund zdefiniowanej w `shutdownTimeLimit`, moduł ASP.NET Core kasuje uruchomiony proces.

Gdy plik *app_offline. htm* jest obecny, moduł ASP.NET Core reaguje na żądania, wysyłając z powrotem zawartość pliku *app_offline. htm* . Po usunięciu pliku *app_offline. htm* następnym żądaniu zostanie uruchomiona aplikacja.

Korzystając z modelu hostingu poza procesem, aplikacja może nie natychmiast zamknie w przypadku otwarcia połączenia. Na przykład połączenie websocket może opóźnić zamknięcia aplikacji.

## <a name="start-up-error-page"></a>Strona błędu uruchamiania

Zarówno w trakcie, jak i spoza procesu hostingu generuje strony błędów niestandardowych, gdy mogą zakończyć się niepowodzeniem uruchomić aplikację.

Jeśli moduł ASP.NET Core nie może odnaleźć programu obsługi żądania w procesie lub out-of-process, zostanie wyświetlona strona kodowa stanu *niepowodzenia ładowania 500,0-procesowego/wyjściowego* .

W przypadku hostingu w procesie, Jeśli uruchomienie aplikacji przez moduł ASP.NET Core nie powiedzie się, zostanie wyświetlona strona kod stanu *niepowodzenia 500,30* .

W przypadku hostingu poza procesem, jeśli moduł ASP.NET Core nie może uruchomić procesu zaplecza lub proces zaplecza zostanie uruchomiony, ale nie nasłuchuje na skonfigurowanym porcie, zostanie wyświetlona strona kod stanu *niepowodzenia procesu 502,5* .

Aby pominąć tę stronę i przywrócić domyślną stronę kodową stanu 5xx usługi IIS, Użyj atrybutu `disableStartUpErrorPage`. Aby uzyskać więcej informacji o konfigurowaniu niestandardowych komunikatów o błędach, zobacz [Błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).

## <a name="log-creation-and-redirection"></a>Tworzenia dziennika i Przekierowanie

Moduł ASP.NET Core przekierowuje dane wyjściowe z konsoli stdout i stderr do dysku, jeśli są ustawione atrybuty `stdoutLogEnabled` i `stdoutLogFile` elementu `aspNetCore`. Wszystkie foldery w ścieżce `stdoutLogFile` są tworzone przez moduł po utworzeniu pliku dziennika. Pula aplikacji musi mieć dostęp do zapisu w lokalizacji, w której zapisano dzienniki (Użyj `IIS AppPool\<app_pool_name>`, aby zapewnić uprawnienia do zapisu).

Dzienniki nie są obracane, chyba że wystąpi procesu recykling/ponowne uruchomienie. Jest odpowiedzialny za dostawcy usług hostingowych w celu ograniczenia ilości miejsca na dysku przez dzienniki.

Użycie dziennika stdout jest zalecane tylko w przypadku rozwiązywania problemów z uruchamianiem aplikacji w usługach IIS lub w przypadku korzystania z [obsługi usług IIS w czasie projektowania w programie Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), a nie podczas debugowania lokalnego i uruchamiania aplikacji przy użyciu IIS Express.

Nie używaj dziennika stdout do celów rejestrowania głównej aplikacji. Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje. Aby uzyskać więcej informacji, zobacz [dostawców rejestrowania innych](xref:fundamentals/logging/index#third-party-logging-providers)firm.

Rozszerzenie sygnatur czasowych i pliku są automatycznie dodawane, gdy plik dziennika jest tworzony. Nazwa pliku dziennika składa się z dołączania sygnatury czasowej, identyfikatora procesu i rozszerzenia pliku (*log*) do ostatniego segmentu ścieżki `stdoutLogFile` (zazwyczaj *stdout*) rozdzielanej znakami podkreślenia. Jeśli ścieżka `stdoutLogFile` kończy się na *stdout*, dziennik aplikacji o identyfikatorze PID 1934 utworzony w dniu 2/5/2018 o 19:42:32 ma nazwę pliku *stdout_20180205194132_1934. log*.

Jeśli `stdoutLogEnabled` ma wartość false, błędy występujące podczas uruchamiania aplikacji są przechwytywane i emitowane do dziennika zdarzeń do 30 KB. Po uruchomieniu wszystkie dodatkowe dzienniki są odrzucane.

Poniższy przykład `aspNetCore` element konfiguruje rejestrowanie stdout w ścieżce względnej `.\log\`. Upewnij się, że tożsamość puli aplikacji ma uprawnienia do zapisu w ścieżce podanej.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

W przypadku publikowania aplikacji na potrzeby wdrożenia Azure App Service zestaw SDK sieci Web ustawia wartość `stdoutLogFile` na `\\?\%home%\LogFiles\stdout`. Zmienna środowiskowa `%home` jest wstępnie zdefiniowana dla aplikacji hostowanych przez Azure App Service.

Aby uzyskać więcej informacji na temat formatów ścieżki, zobacz [formaty ścieżki plików w systemach Windows](/dotnet/standard/io/file-path-formats).

## <a name="enhanced-diagnostic-logs"></a>Rozszerzone dzienników diagnostycznych

Moduł ASP.NET Core można skonfigurować w celu udostępnienia dzienników diagnostyki rozszerzonej. Dodaj element `<handlerSettings>` do elementu `<aspNetCore>` w *pliku Web. config*. Ustawienie `debugLevel` na `TRACE` uwidacznia większą wierność informacji diagnostycznych:

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

Foldery w ścieżce przekazane do wartości `<handlerSetting>` (*dzienniki* w powyższym przykładzie) nie są automatycznie tworzone przez moduł i powinny być wcześniej dostępne we wdrożeniu. Pula aplikacji musi mieć dostęp do zapisu w lokalizacji, w której zapisano dzienniki (Użyj `IIS AppPool\<app_pool_name>`, aby zapewnić uprawnienia do zapisu).

Wartości poziomu debugowania (`debugLevel`) mogą zawierać zarówno poziom, jak i lokalizację.

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

* `ASPNETCORE_MODULE_DEBUG_FILE` &ndash; ścieżkę do pliku dziennika debugowania. (Domyślnie: *aspnetcore-Debug. log*)
* `ASPNETCORE_MODULE_DEBUG` &ndash; ustawienia poziomu debugowania.

> [!WARNING]
> **Nie** pozostawiaj włączonej rejestracji debugowania w ramach wdrożenia dłużej niż jest to wymagane, aby rozwiązać problem. Rozmiar dziennika nie jest ograniczona. Pozostawienie dziennik debugowania włączone może wyczerpać dostępne miejsce na dysku i awarii serwera lub usługi app service.

Zobacz temat [Konfiguracja z plikiem Web. config](#configuration-with-webconfig) , aby zapoznać się z przykładem elementu `aspNetCore` w pliku *Web. config* .

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Konfiguracja serwera proxy korzysta z protokołu HTTP i parowania token

*Dotyczy tylko hostingu poza procesem.*

Serwer proxy między modułu ASP.NET Core i Kestrel korzysta z protokołu HTTP. Istnieje ryzyko ochronę ruchu między moduł i Kestrel z lokalizacji poza serwerem.

Parowania tokenu jest używany w celu zagwarantowania, że przekazywane przez usługi IIS żądań odebranych przez Kestrel i nie pochodzi z innego źródła. Token parowania jest tworzony i ustawiany w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez moduł. Token parowania jest również ustawiany w nagłówku (`MS-ASPNETCORE-TOKEN`) na każdym żądaniu z serwerem proxy. Oprogramowanie pośredniczące IIS sprawdzanie żądań odbierze, aby upewnić się, że wartość tokenu nagłówka parowania odpowiada wartości zmiennej środowiskowej. Jeśli token wartości są niezgodne, żądanie jest rejestrowane i odrzucone. Zmienna środowiskowa tokenu parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem. Bez znajomości parowania wartość tokenu, osoba atakująca nie może przesłać żądania, które obchodzą wyboru w oprogramowaniu pośredniczącym usługi IIS.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Moduł ASP.NET Core z programem IIS konfigurację udostępnioną

Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami konta **TrustedInstaller** . Ponieważ konto systemu lokalnego nie ma uprawnień do modyfikowania dla ścieżki udziału używanej przez udostępnioną konfigurację usług IIS, Instalator zgłasza błąd odmowy dostępu podczas próby skonfigurowania ustawień modułu w pliku *ApplicationHost. config* w udziale.

W przypadku korzystania z konfiguracji udostępnionej przez usługi IIS na tym samym komputerze, na którym znajduje się instalacja usług IIS, uruchom Instalatora pakietu ASP.NET Core hostowania z parametrem `OPT_NO_SHARED_CONFIG_CHECK` ustawionym na `1`:

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

Jeśli ścieżka do konfiguracji udostępnionej nie znajduje się na tym samym komputerze, na którym znajduje się instalacja usług IIS, wykonaj następujące czynności:

1. Wyłącz konfiguracji udostępnionej usług IIS.
1. Uruchom Instalatora.
1. Wyeksportuj zaktualizowany plik *ApplicationHost. config* do udziału.
1. Ponownie włączyć konfiguracji udostępnionej usług IIS.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Wersja modułu oraz Dzienniki Instalatora pakietu hostingu

Aby określić wersję zainstalowanego modułu ASP.NET Core:

1. W systemie hostingu przejdź do *%windir%\System32\inetsrv*.
1. Znajdź plik *aspnetcore. dll* .
1. Kliknij prawym przyciskiem myszy plik i wybierz polecenie **Właściwości** z menu kontekstowego.
1. Wybierz kartę **szczegóły** . **Wersja pliku** i **Wersja produktu** reprezentują zainstalowaną wersję modułu.

Dzienniki Instalatora pakietu hostingu dla modułu znajdują się pod adresem *C:\\użytkownicy\\% username%\\AppData\\lokalnego\\temp*. Plik ma nazwę *dd_DotNetCoreWinSvrHosting__\<sygnatura czasowa > _000_AspNetCoreModule_x64. log*.

## <a name="module-schema-and-configuration-file-locations"></a>Lokalizacje pliku modułu, schematu i konfiguracji

### <a name="module"></a>Moduł

**IIS (x86/amd64):**

* %windir%\System32\inetsrv\aspnetcore.dll

* %windir%\SysWOW64\inetsrv\aspnetcore.dll

* %ProgramFiles%\IIS\Asp.NET core Module\V2\aspnetcorev2.dll

* % ProgramFiles (x86) %\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll

**IIS Express (x86/amd64):**

* %ProgramFiles%\IIS Express\aspnetcore.dll

* %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

* %ProgramFiles%\iis Express\Asp.Net Core Module\V2\aspnetcorev2.dll

* % ProgramFiles (x86) %\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll

### <a name="schema"></a>Schemat

**IIS**

* %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

* %Windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML

**IIS Express**

* %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

* %ProgramFiles%\iis Express\config\schema\aspnetcore_schema_v2.xml

### <a name="configuration"></a>Konfiguracja

**IIS**

* %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

* Visual Studio: {Aplikacja główna}\\. vs\config\applicationHost.config

* Interfejs wiersza polecenia *iisexpress. exe* :%USERPROFILE%\Documents\IISExpress\config\applicationhost.config

Pliki można znaleźć, wyszukując *aspnetcore* w pliku *ApplicationHost. config* .

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

Moduł określa port za pośrednictwem zmiennej środowiskowej podczas uruchamiania, a [oprogramowanie pośredniczące integracji usług IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) konfiguruje serwer do nasłuchiwania na `http://localhost:{port}`. Dodatkowe sprawdzenia są wykonywane, a żądania, które nie pochodzą z modułu, są odrzucane. Moduł nie obsługuje przekazywania HTTPS, dlatego żądania są przekazywane przez protokół HTTP nawet wtedy, gdy są odbierane przez usługę IIS przez protokół HTTPS.

Po podaniu przez Kestrel żądania z modułu żądanie jest wypychane do potoku ASP.NET Core pośredniczącego. Potok oprogramowania pośredniczącego obsługuje żądanie i przekazuje go jako wystąpienie `HttpContext` do logiki aplikacji. Oprogramowanie pośredniczące dodane przez integrację usług IIS aktualizuje schemat, zdalny adres IP i pathbase, aby można było przesłać żądanie do Kestrel. Odpowiedź aplikacji jest przesyłana z powrotem do usług IIS, która wypycha ją z powrotem do klienta HTTP, który zainicjował żądanie.

Wiele modułów macierzystych, takich jak uwierzytelnianie systemu Windows, pozostaje aktywnych. Aby dowiedzieć się więcej na temat modułów usług IIS aktywnych przy użyciu modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/modules>.

Moduł ASP.NET Core może również:

* Ustaw zmienne środowiskowe dla procesu roboczego.
* Rejestruj dane wyjściowe stdout do magazynu plików w celu rozwiązywania problemów z uruchamianiem.
* Przekazuj tokeny uwierzytelniania systemu Windows.

## <a name="how-to-install-and-use-the-aspnet-core-module"></a>Jak zainstalować moduł ASP.NET Core i korzystać z niego

Aby uzyskać instrukcje dotyczące sposobu instalowania modułu ASP.NET Core, zobacz [installing the .NET Core hosting](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

## <a name="configuration-with-webconfig"></a>Konfiguracja z pliku web.config

Moduł ASP.NET Core jest skonfigurowany z sekcją `aspNetCore` węzła `system.webServer` w pliku *Web. config* witryny.

Następujący plik *Web. config* jest publikowany dla [wdrożenia zależnego od platformy](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje moduł ASP.NET Core do obsługi żądań lokacji:

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

Następująca *plik Web. config* jest publikowana dla [samodzielnego wdrożenia](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

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

Po wdrożeniu aplikacji do [Azure App Service](https://azure.microsoft.com/services/app-service/)ścieżka `stdoutLogFile` jest ustawiona na `\\?\%home%\LogFiles\stdout`. Ścieżka zapisuje dzienniki stdout do folderu *LogFiles* , który jest lokalizacją automatycznie utworzoną przez usługę.

Aby uzyskać informacje na temat konfiguracji aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications>.

### <a name="attributes-of-the-aspnetcore-element"></a>Atrybuty elementu aspNetCore

| Atrybut | Opis | Domyślne |
| --------- | ----------- | :-----: |
| `arguments` | <p>Atrybut opcjonalny ciąg.</p><p>Argumenty do pliku wykonywalnego określonego w **processPath**.</p>| |
| `disableStartUpErrorPage` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku wartości true strona **błędu 502,5 procesu** jest pomijana, a strona kodowa stanu 502 skonfigurowana w *pliku Web. config* ma pierwszeństwo.</p> | `false` |
| `forwardWindowsAuthToken` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie. Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</p> | `true` |
| `processesPerApplication` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Określa liczbę wystąpień procesu określonego w ustawieniu **processPath** , które może być przypadające na aplikację.</p><p>Nie zaleca się ustawienia `processesPerApplication`. Ten atrybut zostanie usunięty w przyszłych wydaniach.</p> | Wartość domyślna: `1`<br>Minimum: `1`<br>Maks.: `100` |
| `processPath` | <p>Atrybut wymagany ciąg.</p><p>Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP. Obsługiwane są ścieżki względne. Jeśli ścieżka rozpoczyna się od `.`, ścieżka jest uznawana za względną względem katalogu głównego witryny.</p> | |
| `rapidFailsPerMinute` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Określa, ile razy proces określony w **processPath** może ulec awarii na minutę. W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</p> | Wartość domyślna: `10`<br>Minimum: `0`<br>Maks.: `100` |
| `requestTimeout` | <p>Atrybut opcjonalny przedziału czasu.</p><p>Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</p><p>W wersjach modułu ASP.NET Core, który został dostarczony z wersją ASP.NET Core 2,1 lub nowszą, `requestTimeout` jest określony w godzinach, minutach i sekundach.</p> | Wartość domyślna: `00:02:00`<br>Minimum: `00:00:00`<br>Maks.: `360:00:00` |
| `shutdownTimeLimit` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Czas w sekundach, przez który moduł czeka, aż plik wykonywalny zostanie bezpiecznie zamknięty po wykryciu pliku *app_offline. htm* .</p> | Wartość domyślna: `10`<br>Minimum: `0`<br>Maks.: `600` |
| `startupTimeLimit` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie. Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu. Moduł podejmuje próbę ponownego uruchomienia procesu, gdy odbierze nowe żądanie i kontynuuje ponowne uruchomienie procesu na kolejnych żądaniach przychodzących, chyba że aplikacja nie będzie mogła uruchomić **rapidFailsPerMinute** liczbę razy w ostatniej minucie.</p><p>Wartość 0 (zero) **nie** jest uważana za nieskończony limit czasu.</p> | Wartość domyślna: `120`<br>Minimum: `0`<br>Maks.: `3600` |
| `stdoutLogEnabled` | <p>Opcjonalny logiczny atrybut.</p><p>Jeśli wartość jest równa true, **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Atrybut opcjonalny ciąg.</p><p>Określa względną lub bezwzględną ścieżkę do pliku, dla którego jest rejestrowany **stdout** i **stderr** z procesu określonego w **processPath** . Są ścieżki względne względem katalogu głównego witryny. Każda ścieżka rozpoczynająca się od `.` jest określana względem katalogu głównego witryny, a wszystkie inne ścieżki są traktowane jako ścieżki bezwzględne. Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika. Przy użyciu ograniczników podkreślenia, sygnatury czasowej, identyfikatora procesu i rozszerzenia pliku (*log*) są dodawane do ostatniego segmentu ścieżki **stdoutLogFile** . Jeśli `.\logs\stdout` jest podana jako wartość, przykładowy dziennik stdout jest zapisywany jako *stdout_20180205194132_1934. log* w folderze *Logs* , gdy Zapisano na 2/5/2018 o GODZINie 19:41:32 z identyfikatorem procesu 1934.</p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a>Ustawianie zmiennych środowiskowych

Zmienne środowiskowe można określić dla procesu w atrybucie `processPath`. Określ zmienną środowiskową z `<environmentVariable>` elementem podrzędnym elementu `<environmentVariables>` kolekcji.

> [!WARNING]
> Zmienne środowiskowe ustawione w tej sekcji powodują konflikt z systemowymi zmiennymi środowiskowymi ustawionymi z tą samą nazwą. Jeśli zmienna środowiskowa jest ustawiona zarówno w pliku *Web. config* , jak i na poziomie systemu w systemie Windows, wartość z pliku *Web. config* zostanie dołączona do wartości zmiennej środowiskowej systemowej (na przykład `ASPNETCORE_ENVIRONMENT: Development;Development`), co uniemożliwi uruchomienie aplikacji.

W poniższym przykładzie ustawiono dwóch zmiennych środowiskowych. `ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji do `Development`. Deweloper może tymczasowo ustawić tę wartość w pliku *Web. config* w celu wymuszenia załadowania [strony wyjątku dewelopera](xref:fundamentals/error-handling) podczas debugowania wyjątku aplikacji. `CONFIG_DIR` jest przykładem zmiennej środowiskowej zdefiniowanej przez użytkownika, w której programista zapisał kod, który odczytuje wartość przy uruchamianiu, aby utworzyć ścieżkę do ładowania pliku konfiguracji aplikacji.

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
> Ustaw zmienną środowiskową `ASPNETCORE_ENVIRONMENT` na `Development` na serwerach przejściowych i testowych, które nie są dostępne dla niezaufanych sieci, takich jak Internet.

## <a name="app_offlinehtm"></a>app_offline.htm

Jeśli plik o nazwie *app_offline. htm* zostanie wykryty w katalogu głównym aplikacji, moduł ASP.NET Core próbuje bezpiecznie zamknąć aplikację i zatrzymać przetwarzanie żądań przychodzących. Jeśli aplikacja nadal działa po upływie liczby sekund zdefiniowanej w `shutdownTimeLimit`, moduł ASP.NET Core kasuje uruchomiony proces.

Gdy plik *app_offline. htm* jest obecny, moduł ASP.NET Core reaguje na żądania, wysyłając z powrotem zawartość pliku *app_offline. htm* . Po usunięciu pliku *app_offline. htm* następnym żądaniu zostanie uruchomiona aplikacja.

## <a name="start-up-error-page"></a>Strona błędu uruchamiania

Jeśli moduł ASP.NET Core nie może uruchomić procesu zaplecza lub proces zaplecza zostanie uruchomiony, ale nie nasłuchuje na skonfigurowanym porcie, zostanie wyświetlona strona kod stanu *niepowodzenia procesu 502,5* . Aby pominąć tę stronę i przywrócić domyślną stronę kodową stanu 502 usług IIS, Użyj atrybutu `disableStartUpErrorPage`. Aby uzyskać więcej informacji o konfigurowaniu niestandardowych komunikatów o błędach, zobacz [Błędy HTTP \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).

![502.5 strona kodowa stan niepowodzenia procesu](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Tworzenia dziennika i Przekierowanie

Moduł ASP.NET Core przekierowuje dane wyjściowe z konsoli stdout i stderr do dysku, jeśli są ustawione atrybuty `stdoutLogEnabled` i `stdoutLogFile` elementu `aspNetCore`. Wszystkie foldery w ścieżce `stdoutLogFile` są tworzone przez moduł po utworzeniu pliku dziennika. Pula aplikacji musi mieć dostęp do zapisu w lokalizacji, w której zapisano dzienniki (Użyj `IIS AppPool\<app_pool_name>`, aby zapewnić uprawnienia do zapisu).

Dzienniki nie są obracane, chyba że wystąpi procesu recykling/ponowne uruchomienie. Jest odpowiedzialny za dostawcy usług hostingowych w celu ograniczenia ilości miejsca na dysku przez dzienniki.

Użycie dziennika stdout jest zalecane tylko w przypadku rozwiązywania problemów z uruchamianiem aplikacji w usługach IIS lub w przypadku korzystania z [obsługi usług IIS w czasie projektowania w programie Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), a nie podczas debugowania lokalnego i uruchamiania aplikacji przy użyciu IIS Express.

Nie używaj dziennika stdout do celów rejestrowania głównej aplikacji. Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje. Aby uzyskać więcej informacji, zobacz [dostawców rejestrowania innych](xref:fundamentals/logging/index#third-party-logging-providers)firm.

Rozszerzenie sygnatur czasowych i pliku są automatycznie dodawane, gdy plik dziennika jest tworzony. Nazwa pliku dziennika składa się z dołączania sygnatury czasowej, identyfikatora procesu i rozszerzenia pliku (*log*) do ostatniego segmentu ścieżki `stdoutLogFile` (zazwyczaj *stdout*) rozdzielanej znakami podkreślenia. Jeśli ścieżka `stdoutLogFile` kończy się na *stdout*, dziennik aplikacji o identyfikatorze PID 1934 utworzony w dniu 2/5/2018 o 19:42:32 ma nazwę pliku *stdout_20180205194132_1934. log*.

Poniższy przykład `aspNetCore` element konfiguruje rejestrowanie stdout w ścieżce względnej `.\log\`. Upewnij się, że tożsamość puli aplikacji ma uprawnienia do zapisu w ścieżce podanej.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout">
</aspNetCore>
```

W przypadku publikowania aplikacji na potrzeby wdrożenia Azure App Service zestaw SDK sieci Web ustawia wartość `stdoutLogFile` na `\\?\%home%\LogFiles\stdout`. Zmienna środowiskowa `%home` jest wstępnie zdefiniowana dla aplikacji hostowanych przez Azure App Service.

Aby utworzyć reguły filtru rejestrowania, zobacz sekcje [Konfiguracja](xref:fundamentals/logging/index#log-filtering) i [filtrowanie dzienników](xref:fundamentals/logging/index#log-filtering) w dokumentacji rejestrowania ASP.NET Core.

Aby uzyskać więcej informacji na temat formatów ścieżki, zobacz [formaty ścieżki plików w systemach Windows](/dotnet/standard/io/file-path-formats).

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Konfiguracja serwera proxy korzysta z protokołu HTTP i parowania token

Serwer proxy między modułu ASP.NET Core i Kestrel korzysta z protokołu HTTP. Istnieje ryzyko ochronę ruchu między moduł i Kestrel z lokalizacji poza serwerem.

Parowania tokenu jest używany w celu zagwarantowania, że przekazywane przez usługi IIS żądań odebranych przez Kestrel i nie pochodzi z innego źródła. Token parowania jest tworzony i ustawiany w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez moduł. Token parowania jest również ustawiany w nagłówku (`MS-ASPNETCORE-TOKEN`) na każdym żądaniu z serwerem proxy. Oprogramowanie pośredniczące IIS sprawdzanie żądań odbierze, aby upewnić się, że wartość tokenu nagłówka parowania odpowiada wartości zmiennej środowiskowej. Jeśli token wartości są niezgodne, żądanie jest rejestrowane i odrzucone. Zmienna środowiskowa tokenu parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem. Bez znajomości parowania wartość tokenu, osoba atakująca nie może przesłać żądania, które obchodzą wyboru w oprogramowaniu pośredniczącym usługi IIS.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Moduł ASP.NET Core z programem IIS konfigurację udostępnioną

Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami konta **TrustedInstaller** . Ponieważ konto systemu lokalnego nie ma uprawnień do modyfikowania dla ścieżki udziału używanej przez udostępnioną konfigurację usług IIS, Instalator zgłasza błąd odmowy dostępu podczas próby skonfigurowania ustawień modułu w pliku *ApplicationHost. config* w udziale.

Korzystając z konfiguracji udostępnionej usług IIS, wykonaj następujące kroki:

1. Wyłącz konfiguracji udostępnionej usług IIS.
1. Uruchom Instalatora.
1. Wyeksportuj zaktualizowany plik *ApplicationHost. config* do udziału.
1. Ponownie włączyć konfiguracji udostępnionej usług IIS.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Wersja modułu oraz Dzienniki Instalatora pakietu hostingu

Aby określić wersję zainstalowanego modułu ASP.NET Core:

1. W systemie hostingu przejdź do *%windir%\System32\inetsrv*.
1. Znajdź plik *aspnetcore. dll* .
1. Kliknij prawym przyciskiem myszy plik i wybierz polecenie **Właściwości** z menu kontekstowego.
1. Wybierz kartę **szczegóły** . **Wersja pliku** i **Wersja produktu** reprezentują zainstalowaną wersję modułu.

Dzienniki Instalatora pakietu hostingu dla modułu znajdują się pod adresem *C:\\użytkownicy\\% username%\\AppData\\lokalnego\\temp*. Plik ma nazwę *dd_DotNetCoreWinSvrHosting__\<sygnatura czasowa > _000_AspNetCoreModule_x64. log*.

## <a name="module-schema-and-configuration-file-locations"></a>Lokalizacje pliku modułu, schematu i konfiguracji

### <a name="module"></a>Moduł

**IIS (x86/amd64):**

* %windir%\System32\inetsrv\aspnetcore.dll

* %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (x86/amd64):**

* %ProgramFiles%\IIS Express\aspnetcore.dll

* %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

### <a name="schema"></a>Schemat

**IIS**

* %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

**IIS Express**

* %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>Konfiguracja

**IIS**

* %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

* Visual Studio: {Aplikacja główna}\\. vs\config\applicationHost.config

* Interfejs wiersza polecenia *iisexpress. exe* :%USERPROFILE%\Documents\IISExpress\config\applicationhost.config

Pliki można znaleźć, wyszukując *aspnetcore* w pliku *ApplicationHost. config* .

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* [Źródło odwołania do modułu ASP.NET Core (gałąź główna)](https://github.com/dotnet/aspnetcore/tree/master/src/Servers/IIS/AspNetCoreModuleV2) &ndash; Użyj listy rozwijanej **rozgałęzienie** , aby wybrać konkretną wersję (na przykład `release/3.1`).
* <xref:host-and-deploy/iis/modules>
