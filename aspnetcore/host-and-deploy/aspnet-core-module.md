---
title: Konfiguracja modułu Core programu ASP.NET
author: guardrex
description: Dowiedz się, jak skonfigurować moduł platformy ASP.NET Core do hostowania aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 954841a1b1465c80e60d5745ad9e22294a88fdf4
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2018
ms.locfileid: "31483750"
---
# <a name="aspnet-core-module-configuration-reference"></a>Konfiguracja modułu Core programu ASP.NET

Przez [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), i [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Ten dokument zawiera instrukcje dotyczące sposobu konfigurowania modułu Core ASP.NET do obsługi aplikacji platformy ASP.NET Core. Aby obejrzeć wprowadzenie do platformy ASP.NET Core moduł i instrukcje dotyczące instalacji, zobacz [Przegląd platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module).

## <a name="configuration-with-webconfig"></a>Konfiguracja z pliku web.config

Moduł Core ASP.NET jest skonfigurowany z `aspNetCore` sekcji `system.webServer` węzła w tej witrynie *web.config* pliku.

Następujące *web.config* pliku została opublikowana na potrzeby [wdrożenia zależne od framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje moduł Core ASP.NET do obsługi żądań lokacji:

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

Następujące *web.config* została opublikowana na potrzeby [niezależne wdrożenia](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

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

Gdy aplikacja jest wdrażana na [usłudze Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` ustawiono ścieżkę `\\?\%home%\LogFiles\stdout`. Ścieżka zapisuje dzienniki stdout *LogFiles* folderu, w którym znajduje się automatycznie utworzone przez usługę.

Zobacz [podrzędna aplikacji konfiguracji](xref:host-and-deploy/iis/index#sub-application-configuration) ważne uwagi dotyczące konfiguracji *web.config* plików w aplikacjach podrzędnych.

### <a name="attributes-of-the-aspnetcore-element"></a>Atrybuty elementu aspNetCore

::: moniker range="<= aspnetcore-2.0"
| Atrybut | Opis | Domyślny |
| --------- | ----------- | :-----: |
| `arguments` | <p>Opcjonalny atrybut ciągu.</p><p>Argumenty plik wykonywalny określony w **processPath**.</p>| |
| `disableStartUpErrorPage` | prawda lub fałsz.</p><p>Jeśli PRAWDA, **502.5 — niepowodzenie procesu** strony jest pomijane, a strona kodowa 502 stan skonfigurowane w *web.config* pierwszeństwo.</p> | `false` |
| `forwardWindowsAuthToken` | prawda lub fałsz.</p><p>Jeśli PRAWDA, token jest przekazywane do procesu podrzędnego nasłuchiwanie ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie. Jest odpowiedzialny za ten proces może wywołać funkcji CloseHandle: ten token na żądanie.</p> | `true` |
| `processPath` | <p>Atrybut wymaganych parametrów.</p><p>Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP. Ścieżki względne są obsługiwane. Jeśli ścieżka zaczyna się od `.`, ścieżka jest traktowany jako względem katalogu głównego witryny.</p> | |
| `rapidFailsPerMinute` | <p>Opcjonalny atrybut całkowity.</p><p>Określa liczbę powtórzeń procesu w **processPath** może awarii na minutę. Po przekroczeniu tego limitu modułu zatrzymuje uruchamiania procesu w pozostałej części minutę.</p> | `10` |
| `requestTimeout` | <p>Atrybut opcjonalny timespan.</p><p>Określa okres czasu, dla którego moduł platformy ASP.NET Core czeka na odpowiedź z procesu nasłuchiwanie ASPNETCORE_PORT %.</p><p>W wersjach programu ASP.NET moduł Core dostarczonej wraz z wydaniem programu ASP.NET 2.0 rdzeni lub wcześniej `requestTimeout` muszą być określone w pełnych minutach, w przeciwnym razie domyślne 2 minuty.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>Opcjonalny atrybut całkowity.</p><p>Czas w sekundach modułu dla pliku wykonywalnego jest bezpiecznie zamknąć po *app_offline.htm* Wykryto plik.</p> | `10` |
| `startupTimeLimit` | <p>Opcjonalny atrybut całkowity.</p><p>Czas w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchiwanie na porcie. Po przekroczeniu tego limitu czasu modułu kasuje procesu. Moduł próbuje uruchomić proces, kiedy odbierze żądanie nowej i podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu się **rapidFailsPerMinute** razy w ciągu ostatnich wycofanie minutę.</p> | `120` |
| `stdoutLogEnabled` | <p>Opcjonalny logiczny atrybut.</p><p>Jeśli PRAWDA, **stdout** i **stderr** dla określonym w procesie **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Opcjonalny atrybut ciągu.</p><p>Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane. Ścieżki względne są względem katalogu głównego witryny. Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżki bezwzględne. Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu utworzyć plik dziennika. Przy użyciu ograniczników podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku (*log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki. Jeśli `.\logs\stdout` jest podana jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folder po zapisaniu na 2/5/2018 na 19:41:32 z procesu o identyfikatorze 1934.</p> | `aspnetcore-stdout` |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| Atrybut | Opis | Domyślny |
| --------- | ----------- | :-----: |
| `arguments` | <p>Opcjonalny atrybut ciągu.</p><p>Argumenty plik wykonywalny określony w **processPath**.</p>| |
| `disableStartUpErrorPage` | prawda lub fałsz.</p><p>Jeśli PRAWDA, **502.5 — niepowodzenie procesu** strony jest pomijane, a strona kodowa 502 stan skonfigurowane w *web.config* pierwszeństwo.</p> | `false` |
| `forwardWindowsAuthToken` | prawda lub fałsz.</p><p>Jeśli PRAWDA, token jest przekazywane do procesu podrzędnego nasłuchiwanie ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie. Jest odpowiedzialny za ten proces może wywołać funkcji CloseHandle: ten token na żądanie.</p> | `true` |
| `processPath` | <p>Atrybut wymaganych parametrów.</p><p>Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP. Ścieżki względne są obsługiwane. Jeśli ścieżka zaczyna się od `.`, ścieżka jest traktowany jako względem katalogu głównego witryny.</p> | |
| `rapidFailsPerMinute` | <p>Opcjonalny atrybut całkowity.</p><p>Określa liczbę powtórzeń procesu w **processPath** może awarii na minutę. Po przekroczeniu tego limitu modułu zatrzymuje uruchamiania procesu w pozostałej części minutę.</p> | `10` |
| `requestTimeout` | <p>Atrybut opcjonalny timespan.</p><p>Określa okres czasu, dla którego moduł platformy ASP.NET Core czeka na odpowiedź z procesu nasłuchiwanie ASPNETCORE_PORT %.</p><p>W wersjach moduł Core platformy ASP.NET, dostarczonej wraz z wydaniem programu ASP.NET Core 2.1 lub nowszego `requestTimeout` jest określona w godziny, minuty i sekundy.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>Opcjonalny atrybut całkowity.</p><p>Czas w sekundach modułu dla pliku wykonywalnego jest bezpiecznie zamknąć po *app_offline.htm* Wykryto plik.</p> | `10` |
| `startupTimeLimit` | <p>Opcjonalny atrybut całkowity.</p><p>Czas w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchiwanie na porcie. Po przekroczeniu tego limitu czasu modułu kasuje procesu. Moduł próbuje uruchomić proces, kiedy odbierze żądanie nowej i podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu się **rapidFailsPerMinute** razy w ciągu ostatnich wycofanie minutę.</p> | `120` |
| `stdoutLogEnabled` | <p>Opcjonalny logiczny atrybut.</p><p>Jeśli PRAWDA, **stdout** i **stderr** dla określonym w procesie **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Opcjonalny atrybut ciągu.</p><p>Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane. Ścieżki względne są względem katalogu głównego witryny. Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżki bezwzględne. Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu utworzyć plik dziennika. Przy użyciu ograniczników podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku (*log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki. Jeśli `.\logs\stdout` jest podana jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folder po zapisaniu na 2/5/2018 na 19:41:32 z procesu o identyfikatorze 1934.</p> | `aspnetcore-stdout` |
::: moniker-end

### <a name="setting-environment-variables"></a>Ustawianie zmiennych środowiskowych

Zmienne środowiskowe można określić dla tego procesu w `processPath` atrybutu. Określ zmienną środowiskową o `environmentVariable` elementem podrzędnym `environmentVariables` elementu kolekcji. Zmienne środowiskowe w tej sekcji mają pierwszeństwo względem systemu zmiennych środowiskowych.

Poniższy przykład przedstawia dwa zmiennych środowiskowych. `ASPNETCORE_ENVIRONMENT` Konfiguruje aplikację w środowisku `Development`. Deweloper może tymczasowo Ustaw tę wartość *web.config* pliku, aby wymusić [strona wyjątek dewelopera](xref:fundamentals/error-handling) załadować podczas debugowania aplikacji wyjątek. `CONFIG_DIR` jest przykład zmiennej środowiska użytkownika, gdy projektanta został zapisany kod odczytuje wartość przy uruchamianiu do utworzenia ścieżka pliku konfiguracji aplikacji.

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
> Ustawić tylko `ASPNETCORE_ENVIRONMENT` zmiennej envirnonment `Development` przemieszczania i testowanie serwerów, które nie są dostępne z niezaufanymi sieciami, na przykład Internetem.

## <a name="appofflinehtm"></a>app_offline.htm

Jeśli plik o nazwie *app_offline.htm* została wykryta w katalogu głównym aplikacji platformy ASP.NET Core moduł próbuje bezpiecznie zamknięcia aplikacji i Zatrzymaj przetwarzania przychodzących żądań. Jeśli aplikacja nadal działa po upływie liczby sekund zdefiniowane w `shutdownTimeLimit`, moduł platformy ASP.NET Core kasuje uruchomionego procesu.

Gdy *app_offline.htm* plik jest obecny, moduł platformy ASP.NET Core odpowiada na żądania odsyła zawartość *app_offline.htm* pliku. Gdy *app_offline.htm* plik zostanie usunięty, przy następnym żądaniu uruchomienia aplikacji.

## <a name="start-up-error-page"></a>Strona błędu uruchamiania

Jeśli moduł platformy ASP.NET Core nie można uruchomić procesu zaplecza lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 awarii procesu* zostanie wyświetlona strona kodu stanu. Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 502 usług IIS, należy użyć `disableStartUpErrorPage` atrybutu. Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).

![502.5 strona kodowa stan niepowodzenia procesu](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Tworzenie dziennika i Przekierowanie

Moduł platformy ASP.NET Core przekierowuje dzienników stdout i stderr na dysku, jeśli `stdoutLogEnabled` i `stdoutLogFile` atrybuty `aspNetCore` ustawiono element. Wszystkie foldery w `stdoutLogFile` ścieżka musi istnieć w kolejności dla modułu utworzyć plik dziennika. Pula aplikacji musi mieć dostęp do zapisu do lokalizacji, w którym zapisywane są dzienniki (Użyj `IIS AppPool\<app_pool_name>` zapewniające uprawnienia do zapisu).

Dzienniki nie są obracane, chyba że występuje procesu recykling/ponowne uruchomienie. Jest odpowiedzialny za dostawcy usług hostingowych, aby ograniczyć miejsce na dysku korzystać z dzienników.

Przy użyciu dzienników stdout jest zalecane tylko dla rozwiązywania problemów uruchomienia aplikacji. Dziennik stdout nie jest używany do rejestrowania aplikacji ogólnych celów. Użyć biblioteki rejestrowania, który ogranicza rozmiar pliku dziennika i dzienników obraca rutynowych rejestrowania w aplikacji platformy ASP.NET Core. Aby uzyskać więcej informacji, zobacz [dostawców innych firm rejestrowania](xref:fundamentals/logging/index#third-party-logging-providers).

Rozszerzenia znaczników czasu i pliku są dodawane automatycznie podczas tworzenia pliku dziennika. Nazwa pliku dziennika składa się przez dodanie sygnatury czasowej ID procesu i rozszerzenie pliku (*log*) do ostatni segment `stdoutLogFile` ścieżki (zazwyczaj *stdout*) rozdzielane znakami podkreślenia. Jeśli `stdoutLogFile` ścieżka kończy się *stdout*, nazwa pliku ma dziennika dla aplikacji za pomocą identyfikatora PID 1934 utworzony na 2/5/2018 w 19:42:32 *stdout_20180205194132_1934.log*.

Poniższy przykład `aspNetCore` element konfiguruje stdout rejestrowania dla aplikacji hostowanej w usłudze Azure App Service. Ścieżkę lokalną lub ścieżkę do udziału sieciowego jest dopuszczalne do lokalnego logowania. Upewnij się, że tożsamość puli aplikacji ma uprawnienia do zapisu w podanej ścieżce.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

Zobacz [konfiguracji z pliku web.config](#configuration-with-webconfig) przykład `aspNetCore` element *web.config* pliku.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Konfiguracja serwera proxy używa protokołu HTTP i token parowania

Serwer proxy między moduł platformy ASP.NET Core i Kestrel używa protokołu HTTP. Przy użyciu protokołu HTTP jest zoptymalizować wydajność, których ruch między moduł i Kestrel odbywa się na adres sprzężenia zwrotnego wylogowuje interfejsu sieciowego. Nie istnieje ryzyko z podsłuchiwaniu ruch między moduł i Kestrel z lokalizacji wylogowuje na serwerze.

Token parowania służy do zagwarantowania, że żądań odebranych przez Kestrel były przekazywane przez serwer proxy przez usługi IIS i nie pochodzą z innego źródła. Token parowania zostanie utworzona i ustawiona w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez moduł. Token parowania zostanie również ustawiona w nagłówku (`MSAspNetCoreToken`) na każde żądanie przekazywane przez serwer proxy. Oprogramowanie pośredniczące IIS sprawdzanie żądań odbierze upewnij się, że parowania wartość tokenu nagłówka odpowiada wartości zmiennej środowiskowej. Jeśli wartości tokenów są niezgodne, żądanie jest rejestrowane i odrzucone. Parowania zmiennej środowiskowej tokenu i komunikacji między modułu i Kestrel nie są dostępne z lokalizacji wylogowuje na serwerze. Bez wiedzy o parowania wartość tokenu, osoba atakująca nie może przesłać żądania, które pomijają wyboru w oprogramowaniu pośredniczącym usługi IIS.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Moduł platformy ASP.NET Core z programem IIS konfiguracji udostępnionej

Instalator platformy ASP.NET Core modułu jest uruchamiany z uprawnieniami **systemu** konta. Ponieważ lokalne konto systemowe nie ma uprawnienia do modyfikowania dla ścieżki udziału konfiguracji udostępnionej usług IIS, Instalator trafienia dostępu błąd podczas próby skonfigurowania ustawienia modułu w *applicationHost.config* w udziale. Podczas korzystania z konfiguracji udostępnionej usług IIS, wykonaj następujące kroki:

1. Wyłącz konfiguracji udostępnionej usług IIS.
1. Uruchom Instalatora.
1. Eksportuj zaktualizowanego *applicationHost.config* plików do udziału.
1. Ponownie włączyć konfiguracji udostępnionej usług IIS.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Dzienniki Instalatora Hosting pakietu i wersja modułu

Aby określić wersję zainstalowanego modułu Core ASP.NET:

1. W systemie hostingu, przejdź do *%windir%\System32\inetsrv*.
1. Zlokalizuj *aspnetcore.dll* pliku.
1. Kliknij prawym przyciskiem myszy plik i wybierz **właściwości** z menu kontekstowego.
1. Wybierz **szczegóły** kartę. **Wersja pliku** i **wersji produktu** reprezentują zainstalowanej wersji modułu.

Dzienniki Instalatora pakietu Hosting dla modułu znajdują się w *C:\\użytkowników\\% UserName %\\AppData\\lokalnego\\Temp*. Plik ma nazwę *dd_DotNetCoreWinSvrHosting__\<sygnatury czasowej > _000_AspNetCoreModule_x64.log*.

## <a name="module-schema-and-configuration-file-locations"></a>Moduł, schematów i konfiguracji lokalizacje plików

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

Pliki można znaleźć, wyszukując *aspnetcore.dll* w *applicationHost.config* pliku. Dla usług IIS Express *applicationHost.config* plik nie istnieje domyślnie. Plik jest tworzony w  *\<application_root >\\.vs\\config* podczas uruchamiania żadnego projektu aplikacji sieci web w rozwiązaniu Visual Studio.
