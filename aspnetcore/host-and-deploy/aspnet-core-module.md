---
title: Informacje o konfiguracji ASP.NET Core modułu
author: guardrex
description: Dowiedz się, jak skonfigurować modułu ASP.NET Core do hostowania aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: bf7a60b67b1ea78bb346e6dd5eeef38b54bfdbe4
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010952"
---
# <a name="aspnet-core-module-configuration-reference"></a>Informacje o konfiguracji ASP.NET Core modułu

Przez [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), i [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Niniejszy dokument zawiera instrukcje dotyczące sposobu konfigurowania modułu ASP.NET Core do hostowania aplikacji platformy ASP.NET Core. Aby zapoznać się z wprowadzeniem do modułu ASP.NET Core i instrukcje dotyczące instalacji, zobacz [Przegląd modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

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

Zobacz [konfiguracji podrzędnych aplikacji](xref:host-and-deploy/iis/index#sub-application-configuration) ważne uwagi dotyczące konfiguracji *web.config* plików w aplikacji podrzędnych.

### <a name="attributes-of-the-aspnetcore-element"></a>Atrybuty elementu aspNetCore

::: moniker range="<= aspnetcore-2.0"

| Atrybut | Opis | Domyślny |
| --------- | ----------- | :-----: |
| `arguments` | <p>Atrybut opcjonalny ciąg.</p><p>Argumenty do pliku wykonywalnego, określony w **processPath**.</p>| |
| `disableStartUpErrorPage` | prawda lub fałsz.</p><p>W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</p> | `false` |
| `forwardWindowsAuthToken` | prawda lub fałsz.</p><p>W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie. Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</p> | `true` |
| `processPath` | <p>Atrybut wymagany ciąg.</p><p>Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP. Obsługiwane są ścieżki względne. Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</p> | |
| `rapidFailsPerMinute` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę. W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</p> | `10` |
| `requestTimeout` | <p>Atrybut opcjonalny przedziału czasu.</p><p>Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</p><p>W wersjach modułu ASP.NET Core, dostarczonej wraz z wydaniem programu ASP.NET Core 2.0 lub wcześniej `requestTimeout` musi być określona w pełnych minut, w przeciwnym razie jego wartość domyślna to 2 minuty.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</p> | `10` |
| `startupTimeLimit` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie. Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu. Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</p> | `120` |
| `stdoutLogEnabled` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Atrybut opcjonalny ciąg.</p><p>Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane. Są ścieżki względne względem katalogu głównego witryny. Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych. Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika. Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku (*.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki. Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

| Atrybut | Opis | Domyślny |
| --------- | ----------- | :-----: |
| `arguments` | <p>Atrybut opcjonalny ciąg.</p><p>Argumenty do pliku wykonywalnego, określony w **processPath**.</p>| |
| `disableStartUpErrorPage` | prawda lub fałsz.</p><p>W przypadku opcji true **502.5 - niepowodzenia procesu** strony jest pominięty, a strona kodowa 502 stan skonfigurowane w *web.config* ma pierwszeństwo.</p> | `false` |
| `forwardWindowsAuthToken` | prawda lub fałsz.</p><p>W przypadku opcji true token są przekazywane do procesu podrzędnego nasłuchiwać ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie. Jest odpowiedzialny za ten proces może wywołać funkcja CloseHandle tego tokenu na żądanie.</p> | `true` |
| `processPath` | <p>Atrybut wymagany ciąg.</p><p>Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP. Obsługiwane są ścieżki względne. Jeśli ścieżka zaczyna się od `.`, ścieżka jest uważany za względem katalogu głównego witryny.</p> | |
| `rapidFailsPerMinute` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Określa liczbę powtórzeń proces w **processPath** może być awaria na minutę. W przypadku przekroczenia tego limitu moduł zatrzymuje uruchamiania procesu na pozostałą część tej minuty kończą.</p> | `10` |
| `requestTimeout` | <p>Atrybut opcjonalny przedziału czasu.</p><p>Określa czas, dla którego modułu ASP.NET Core czeka na odpowiedź z procesu nasłuchiwać ASPNETCORE_PORT %.</p><p>W wersjach modułu ASP.NET Core, dołączonej do wersji platformy ASP.NET Core 2.1 lub nowszej `requestTimeout` jest określona w godzinach, minutach i sekundach.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Czas trwania w sekundach module pliku wykonywalnego, który jest bezpiecznie zamknąć podczas *app_offline.htm* Wykryto plik.</p> | `10` |
| `startupTimeLimit` | <p>Atrybut opcjonalną liczbą całkowitą.</p><p>Czas trwania w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchuje na porcie. Jeśli ten limit czasu zostanie przekroczony, moduł kasuje procesu. Moduł spróbuje ponownie uruchomić proces, gdy otrzymuje nowe żądanie oraz podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu **rapidFailsPerMinute** liczbę razy w ciągu ostatnich stopniowe minuta.</p> | `120` |
| `stdoutLogEnabled` | <p>Opcjonalny logiczny atrybut.</p><p>W przypadku opcji true **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Atrybut opcjonalny ciąg.</p><p>Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane. Są ścieżki względne względem katalogu głównego witryny. Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżek bezwzględnych. Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika. Za pomocą ograniczniki podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku (*.log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki. Jeśli `.\logs\stdout` jest dostarczany jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folderu po zapisaniu 2/5/2018 o 19:41:32 przy użyciu procesu o identyfikatorze 1934.</p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a>Ustawianie zmiennych środowiskowych

Zmienne środowiskowe można określić dla tego procesu w `processPath` atrybutu. Określ zmienną środowiskową `environmentVariable` element podrzędny elementu `environmentVariables` elementu kolekcji. Zmienne środowiskowe ustawione w tej sekcji pierwszeństwo systemowe zmienne środowiskowe.

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
> Ustawić tylko `ASPNETCORE_ENVIRONMENT` zmienną envirnonment `Development` przejściowym i testowania serwerów, które nie są dostępne do niezaufanych sieci, takich jak Internet.

## <a name="appofflinehtm"></a>app_offline.htm

Jeśli plik o nazwie *app_offline.htm* wykryte w katalogu głównym aplikacji, modułu ASP.NET Core próbuje łagodne zamykanie aplikacji i zatrzymanie przetwarzania przychodzących żądań. Jeśli aplikacja jest nadal uruchomione po upływie liczby sekund zdefiniowane w `shutdownTimeLimit`, modułu ASP.NET Core to złe rozwiązanie uruchomionego procesu.

Gdy *app_offline.htm* występuje pliku modułu ASP.NET Core ma odpowiadać na żądania, wysyłając ponownie zawartość *app_offline.htm* pliku. Gdy *app_offline.htm* plik jest usuwany, kolejne żądanie uruchamia aplikację.

## <a name="start-up-error-page"></a>Strona błędu uruchamiania

Jeśli modułu ASP.NET Core nie uda się uruchomić procesu wewnętrznej bazy danych lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 niepowodzenia procesu* zostanie wyświetlona strona kodu stanu. Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 502 usług IIS, należy użyć `disableStartUpErrorPage` atrybutu. Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).

![502.5 strona kodowa stan niepowodzenia procesu](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Tworzenia dziennika i Przekierowanie

Modułu ASP.NET Core przekierowuje dane wyjściowe stdout i stderr konsoli do dysku, jeśli `stdoutLogEnabled` i `stdoutLogFile` atrybuty `aspNetCore` elementu są ustawione. Wszystkie foldery w `stdoutLogFile` ścieżka musi istnieć w kolejności dla modułu, można utworzyć pliku dziennika. Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewnienie uprawnienia do zapisu).

Dzienniki nie są obracane, chyba że wystąpi procesu recykling/ponowne uruchomienie. Jest odpowiedzialny za dostawcy usług hostingowych w celu ograniczenia ilości miejsca na dysku przez dzienniki.

Przy użyciu dziennika stdout jest zalecane tylko na potrzeby rozwiązywania problemów z uruchamianiem aplikacji. Nie używaj dziennika stdout do celów rejestrowania głównej aplikacji. Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje. Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).

Rozszerzenie sygnatur czasowych i pliku są automatycznie dodawane, gdy plik dziennika jest tworzony. Przez dodanie sygnatury czasowej, identyfikator procesu i rozszerzenie pliku składa się nazwa pliku dziennika (*.log*) do ostatni segment `stdoutLogFile` ścieżki (zazwyczaj *stdout*) rozdzielane znakami podkreślenia. Jeśli `stdoutLogFile` ścieżki kończy się *stdout*, dziennika dla aplikacji za pomocą identyfikatora PID 1934 utworzono 19:42:32 2/5/2018 o nazwie pliku *stdout_20180205194132_1934.log*.

Poniższy przykład `aspNetCore` element konfiguruje rejestrowanie strumienia stdout dla aplikacji hostowanej w usłudze Azure App Service. Ścieżka lokalna lub ścieżkę do udziału sieciowego jest możliwa do logowania lokalnego. Upewnij się, że tożsamość puli aplikacji ma uprawnienia do zapisu w ścieżce podanej.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

Zobacz [konfiguracji z pliku web.config](#configuration-with-webconfig) przykład `aspNetCore` element *web.config* pliku.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Konfiguracja serwera proxy korzysta z protokołu HTTP i parowania token

Serwer proxy między modułu ASP.NET Core i Kestrel korzysta z protokołu HTTP. Przy użyciu protokołu HTTP jest Optymalizacja wydajności, gdzie ruch między modułem i Kestrel odbywa się na adres sprzężenia zwrotnego zniżki w stosunku do interfejsu sieciowego. Istnieje ryzyko ochronę ruchu między moduł i Kestrel z lokalizacji poza serwerem.

Parowania tokenu jest używany w celu zagwarantowania, że przekazywane przez usługi IIS żądań odebranych przez Kestrel i nie pochodzi z innego źródła. Parowania token jest utworzona i ustawiona w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez moduł. Token parowania jest również ustawiona w nagłówku (`MSAspNetCoreToken`) na każde żądanie serwerem proxy. Oprogramowanie pośredniczące IIS sprawdzanie żądań odbierze, aby upewnić się, że wartość tokenu nagłówka parowania odpowiada wartości zmiennej środowiskowej. Jeśli token wartości są niezgodne, żądanie jest rejestrowane i odrzucone. Zmienna środowiskowa tokenu parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem. Bez znajomości parowania wartość tokenu, osoba atakująca nie może przesłać żądania, które obchodzą wyboru w oprogramowaniu pośredniczącym usługi IIS.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Moduł ASP.NET Core z programem IIS konfigurację udostępnioną

Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami **systemu** konta. Ponieważ lokalne konto systemowe nie ma uprawnienia do modyfikowania dla ścieżki udziału konfiguracji udostępnionej usług IIS, Instalator trafienia odmowa dostępu błąd podczas próby skonfigurowania ustawień modułu w *applicationHost.config* w udziale. Korzystając z konfiguracji udostępnionej usług IIS, wykonaj następujące kroki:

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

   * .vs\config\applicationHost.config

Pliki można znaleźć, wyszukując *aspnetcore.dll* w *applicationHost.config* pliku. Dla usług IIS Express *applicationHost.config* plik nie istnieje domyślnie. Plik jest tworzony w  *\<application_root >\\.vs\\config* podczas uruchamiania dowolnego projektu aplikacji sieci web w rozwiązaniu Visual Studio.
