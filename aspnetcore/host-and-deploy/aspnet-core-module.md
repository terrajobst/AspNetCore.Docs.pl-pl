---
title: "Konfiguracja modułu Core programu ASP.NET"
author: guardrex
description: "Jak skonfigurować moduł platformy ASP.NET Core do obsługi aplikacji platformy ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: c1a05c3e40e6aab0f2e4a97c0b3bb9eca8a08a41
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-module-configuration-reference"></a>Konfiguracja modułu Core programu ASP.NET

Przez [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), i [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Ten dokument zawiera szczegółowe informacje na temat konfigurowania modułu Core ASP.NET do obsługi aplikacji platformy ASP.NET Core. Aby obejrzeć wprowadzenie do platformy ASP.NET Core moduł i instrukcje dotyczące instalacji, zobacz [Przegląd platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module).

## <a name="configuration-via-webconfig"></a>Konfiguracji za pomocą pliku web.config

Moduł platformy ASP.NET Core jest skonfigurowana za pośrednictwem witryny lub aplikacji *web.config* plików i ma własną `aspNetCore` sekcji konfiguracji w ramach `system.webServer`. Oto przykład *web.config* pliku `Microsoft.NET.Sdk.Web` zapewni zestawu SDK, gdy projekt zostanie opublikowany dla [wdrożenia zależne od framework](https://docs.microsoft.com/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) z symbole zastępcze `processPath` i `arguments`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="%LAUNCHER_PATH%" 
        arguments="%LAUNCHER_ARGS%" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

*Web.config* w poniższym przykładzie jest przeznaczony dla [niezależne wdrożenia](https://docs.microsoft.com/dotnet/articles/core/deploying/#self-contained-deployments-scd) do [usłudze Azure App Service](https://azure.microsoft.com/services/app-service/). Aby uzyskać więcej informacji, zobacz [hosta w systemie Windows z programem IIS](xref:host-and-deploy/iis/index). Zobacz [konfiguracji aplikacji podrzędne](xref:host-and-deploy/iis/index#configuration-of-sub-applications) ważne uwagi dotyczące konfiguracji *web.config* plików w aplikacjach podrzędnych.

```xml
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
        stdoutLogEnabled="false" 
        stdoutLogFile="\\?\%home%\LogFiles\stdout" />
  </system.webServer>
</configuration>
```

### <a name="attributes-of-the-aspnetcore-element"></a>Atrybuty elementu aspNetCore

| Atrybut | Opis |
| --- | --- |
| processPath | <p>Atrybut wymaganych parametrów.</p><p>Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP. Ścieżki względne są obsługiwane. Jeśli ścieżka zaczyna się od '.', ścieżka jest traktowany jako względem katalogu głównego witryny.</p><p>Brak wartości domyślnej.</p> |
| argumenty | <p>Opcjonalny atrybut ciągu.</p><p>Argumenty plik wykonywalny określony w **processPath**.</p><p>Wartością domyślną jest ciąg pusty.</p> |
| startupTimeLimit | <p>Opcjonalny atrybut całkowity.</p><p>Czas w sekundach, oczekiwania modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchiwanie na porcie. Po przekroczeniu tego limitu czasu, moduł będzie kasowanie procesu. Moduł spróbuje ponownie uruchomić proces, kiedy odbierze żądanie nowej i będzie podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie może uruchomić **rapidFailsPerMinute** numer razy w ciągu ostatniej minuty stopniowego.</p><p>Wartość domyślna to 120.</p> |
| shutdownTimeLimit | <p>Opcjonalny atrybut całkowity.</p><p>Czas w sekundach, dla których moduł będzie oczekiwał na plik wykonywalny jest bezpiecznie zamknąć po *app_offline.htm* Wykryto plik.</p><p>Wartość domyślna to 10.</p> |
| rapidFailsPerMinute | <p>Opcjonalny atrybut całkowity.</p><p>Określa liczbę powtórzeń procesu w **processPath** może awarii na minutę. Po przekroczeniu tego limitu modułu przestanie uruchamiania procesu w pozostałej części minutę.</p><p>Wartość domyślna to 10.</p> |
| requestTimeout | <p>Atrybut opcjonalny timespan.</p><p>Określa okres czasu, dla którego moduł platformy ASP.NET Core będzie oczekiwał na odpowiedź z procesu nasłuchiwanie ASPNETCORE_PORT %.</p><p>Wartość domyślna to "00: 02:00".</p><p>`requestTimeout` Muszą być określone w pełnych minutach, w przeciwnym razie domyślne 2 minuty.</p> |
| stdoutLogEnabled | <p>Opcjonalny logiczny atrybut.</p><p>Jeśli PRAWDA, **stdout** i **stderr** dla określonym w procesie **processPath** nastąpi przekierowanie do określonego w pliku **stdoutLogFile**.</p><p>Wartość domyślna to false.</p> |
| stdoutLogFile | <p>Opcjonalny atrybut ciągu.</p><p>Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** będą rejestrowane. Ścieżki względne są względem katalogu głównego witryny. Dowolną ścieżkę, rozpoczynając od '.' będzie względem katalogu głównego witryny i innych ścieżek będą traktowane jako ścieżki bezwzględne. Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu utworzyć plik dziennika. Identyfikator procesu sygnatury czasowej (*yyyyMdhms*) i rozszerzenie pliku (*log*) podkreślenia ograniczniki są dodawane do ostatniego segment **stdoutLogFile** podane.</p><p>Wartość domyślna to `aspnetcore-stdout`.</p> |
| forwardWindowsAuthToken | prawda lub fałsz.</p><p>Jeśli PRAWDA, token będą przekazywane do procesu podrzędnego nasłuchiwanie ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie. Jest odpowiedzialny za ten proces może wywołać funkcji CloseHandle: ten token na żądanie.</p><p>Wartość domyślna to true.</p> |
| disableStartUpErrorPage | prawda lub fałsz.</p><p>Jeśli PRAWDA, **502.5 — niepowodzenie procesu** strona zostanie pominięta i na stronie kod stanu 502 w Twojej *web.config* mają wyższy priorytet.</p><p>Wartość domyślna to false.</p> |

### <a name="setting-environment-variables"></a>Ustawianie zmiennych środowiskowych

Moduł platformy ASP.NET Core pozwala określić zmiennych środowiskowych dla procesu, który został określony w `processPath` atrybutu, określając je w co najmniej jednej `environmentVariable` elementy podrzędne `environmentVariables` elementu kolekcji, w obszarze `aspNetCore` elementu. Zmienne środowiskowe w tej sekcji mają pierwszeństwo względem systemu zmiennych środowiskowych dla procesu.

W poniższym przykładzie ustawia dwie zmienne środowiskowe. `ASPNETCORE_ENVIRONMENT`konfiguruje środowisko aplikacji `Development`. Deweloper może tymczasowo Ustaw tę wartość *web.config* pliku, aby wymusić [strona wyjątek dewelopera](xref:fundamentals/error-handling) załadować podczas debugowania aplikacji wyjątek. `CONFIG_DIR`jest przykład zmiennej środowiska użytkownika, gdy projektanta został zapisany kod, który odczyta wartość przy uruchamianiu do utworzenia ścieżkę, aby można było załadować pliku konfiguracyjnego aplikacji.

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

## <a name="appofflinehtm"></a>app_offline.htm

Jeśli możesz umieścić plik o nazwie *app_offline.htm* w głównym katalogu aplikacji sieci web, moduł platformy ASP.NET Core będzie próbować bezpiecznie zamknięcia aplikacji i Zatrzymaj przetwarzanie przychodzących żądań. Jeśli aplikacja jest nadal uruchomiona `shutdownTimeLimit` liczbę sekund, moduł platformy ASP.NET Core będzie kill uruchomionego procesu.

Gdy *app_offline.htm* plik jest obecny, moduł platformy ASP.NET Core będzie odpowiadał na żądania przez odsyła zawartość *app_offline.htm* pliku. Raz *app_offline.htm* plik zostanie usunięty, przy następnym żądaniu ładuje aplikacji, która następnie odpowiada na żądania.

## <a name="start-up-error-page"></a>Strona błędu uruchamiania

Jeśli moduł platformy ASP.NET Core nie można uruchomić procesu zaplecza lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym, zostanie wyświetlona strona kod stanu HTTP 502.5. Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 502 usług IIS, należy użyć `disableStartUpErrorPage` atrybutu. Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP `<httpErrors>` ](https://docs.microsoft.com/iis/configuration/system.webServer/httpErrors/).

![502 stanu strony](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Tworzenie dziennika i Przekierowanie

Moduł platformy ASP.NET Core przekierowuje `stdout` i `stderr` dzienniki na dysku, jeśli ustawisz `stdoutLogEnabled` i `stdoutLogFile` atrybuty `aspNetCore` elementu. Wszystkie foldery w `stdoutLogFile` ścieżka musi istnieć w kolejności dla modułu utworzyć plik dziennika. Rozszerzenia znaczników czasu i plików zostaną dodane automatycznie, gdy plik dziennika jest tworzony. Dzienniki nie są obracane, chyba że występuje procesu recykling/ponowne uruchomienie. Jest odpowiedzialny za dostawcy usług hostingowych, aby ograniczyć miejsce na dysku korzystać z dzienników. Przy użyciu `stdout` dziennika jest zalecane tylko uzyskać informacje o rozwiązywaniu problemów z uruchamianiem aplikacji, a nie do celów rejestracji ogólnego zastosowania.

Nazwa pliku dziennika składa się przez dołączenie Identyfikatora PID, sygnatura czasowa procesu (*yyyyMdhms*) i rozszerzenie pliku (*log*) do ostatni segment `stdoutLogFile` ścieżki (zazwyczaj *stdout* ) rozdzielane znakami podkreślenia. Na przykład jeśli `stdoutLogFile` ścieżka kończy się *stdout*, nazwa pliku ma dziennika dla aplikacji za pomocą identyfikatora PID 10652 utworzonego na 8/10/2017 w dniu 12:05:02 *stdout_10652_20178101252.log*.

Poniżej przedstawiono przykładowe `aspNetCore` element, który konfiguruje `stdout` rejestrowania. `stdoutLogFile` Ścieżka pokazano w przykładzie jest odpowiednia dla usługi Azure App Service. Ścieżkę lokalną lub ścieżkę do udziału sieciowego jest dopuszczalne do lokalnego logowania. Upewnij się, że tożsamość puli aplikacji ma uprawnienia do zapisu w podanej ścieżce.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```
Zobacz [konfiguracji za pomocą pliku web.config](#configuration-via-webconfig) przykład `aspNetCore` element *web.config* pliku.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Moduł platformy ASP.NET Core z programem IIS konfiguracji udostępnionej

Instalator platformy ASP.NET Core modułu jest uruchamiany z uprawnieniami **systemu** konta. Ponieważ lokalne konto systemowe nie ma uprawnienia do modyfikowania dla ścieżki udziału, który jest używany przez konfiguracji udostępnionej usług IIS, Instalator nastąpi trafienie dostępu błąd podczas próby skonfigurowania ustawienia modułu w  *applicationHost.config* w udziale.

Nieobsługiwany obejściem jest wyłączenie konfiguracji udostępnionej usług IIS, uruchom Instalatora, eksportowanie zaktualizowanego *applicationHost.config* plików do udziału, a następnie ponownie włącz konfiguracji udostępnionej usług IIS.

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

Możesz wyszukać *aspnetcore.dll* w *applicationHost.config* pliku. Dla usług IIS Express *applicationHost.config* plik nie istnieje domyślnie. Plik jest tworzony w *{katalog główny aplikacji}\.vs\config* po rozpoczęciu żadnego projektu aplikacji sieci web w rozwiązaniu Visual Studio.
