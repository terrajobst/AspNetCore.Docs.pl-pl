---
title: Rozwiązywanie problemów z platformą ASP.NET Core w usługach IIS
author: guardrex
description: Dowiedz się, jak diagnozować problemy z wdrożeniami usług Internet Information Services (IIS) w aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/28/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: cb42a262c89c27fa350e936184f8ddb3a02788f0
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034740"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Rozwiązywanie problemów z platformą ASP.NET Core w usługach IIS

Przez [Luke Latham](https://github.com/guardrex)

Ten artykuł zawiera instrukcje na temat platformy ASP.NET Core zdiagnozować problem uruchamiania aplikacji w przypadku hostowania za pomocą [Internet Information Services (IIS)](/iis). Informacje przedstawione w tym artykule mają zastosowanie do hostowania w usługach IIS w systemie Windows Server i Windows Desktop.

::: moniker range=">= aspnetcore-2.2"

W programie Visual Studio ma domyślnie wartość projektu ASP.NET Core [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hostingu podczas debugowania. A *502.5 - niepowodzenia procesu* lub *500.30 - Start błąd* występuje, gdy debugowanie lokalne może być troubleshooted przy użyciu porady w tym temacie.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

W programie Visual Studio ma domyślnie wartość projektu ASP.NET Core [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hostingu podczas debugowania. A *502.5 niepowodzenia procesu* występuje, gdy debugowanie lokalne może być troubleshooted przy użyciu porady w tym temacie.

::: moniker-end

Dodatkowe tematy dotyczące rozwiązywania problemów:

<xref:host-and-deploy/azure-apps/troubleshoot> Mimo że usługa App Service używa [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) i usługi IIS do hostowania aplikacji, zobacz temat dedykowanych instrukcje specyficzne dla usługi App Service.

<xref:fundamentals/error-handling> Dowiedz się, jak do obsługi błędów w aplikacji platformy ASP.NET Core podczas programowania w systemie lokalnym.

[Naucz się debugować przy użyciu programu Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) w tym temacie przedstawiono funkcje debugera programu Visual Studio.

[Debugowanie za pomocą programu Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) więcej informacji na temat debugowania wbudowaną w programie Visual Studio Code.

## <a name="app-startup-errors"></a>Błędy uruchamiania aplikacji

### <a name="5025-process-failure"></a>502.5 niepowodzenie procesu

Proces roboczy kończy się niepowodzeniem. Nie zaczyna się aplikacja.

Modułu ASP.NET Core próbuje uruchomić proces dotnet wewnętrznej bazy danych, ale nie została uruchomiona. Zazwyczaj można ustalić przyczyny niepowodzenia uruchamiania procesu na podstawie wpisów w [dziennik zdarzeń aplikacji](#application-event-log) i [dziennika stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log).

Aplikacja jest błędnie skonfigurowane z powodu przeznaczony dla wersji udostępnionej platformy ASP.NET Core, która nie jest obecny jest jakiś wspólny warunek błędu. Sprawdź, które wersje udostępnionej platformy ASP.NET Core są zainstalowane na komputerze docelowym. *Udostępnionej platformy* to zbiór zestawów ( *.dll* plików), są zainstalowane na komputerze i odwołuje się meta Microsoft.aspnetcore.all, takich jak `Microsoft.AspNetCore.App`. Dokumentacja meta Microsoft.aspnetcore.all można określić minimalnej wymaganej wersji. Aby uzyskać więcej informacji, zobacz [udostępnionej platformy](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).

*502.5 niepowodzenia procesu* strony błędu jest zwracany, jeśli aplikacji lub obsługującego błędnej konfiguracji powoduje niepowodzenie procesu roboczego:

![Okno przeglądarki, przedstawiający 502.5 stronę niepowodzenia procesu](troubleshoot/_static/process-failure-page.png)

::: moniker range="= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a>500.30 w procesie Niepowodzenie uruchamiania

Proces roboczy kończy się niepowodzeniem. Nie zaczyna się aplikacja.

Próbuje uruchomić program .NET Core CLR w procesie modułu ASP.NET Core, ale nie została uruchomiona. Zazwyczaj można ustalić przyczyny niepowodzenia uruchamiania procesu na podstawie wpisów w [dziennik zdarzeń aplikacji](#application-event-log) i [dziennika stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log).

Aplikacja jest błędnie skonfigurowane z powodu przeznaczony dla wersji udostępnionej platformy ASP.NET Core, która nie jest obecny jest jakiś wspólny warunek błędu. Sprawdź, które wersje udostępnionej platformy ASP.NET Core są zainstalowane na komputerze docelowym.

### <a name="5000-in-process-handler-load-failure"></a>500.0 w procesie programu obsługi błędu ładowania

Proces roboczy kończy się niepowodzeniem. Nie zaczyna się aplikacja.

Modułu ASP.NET Core nie powiedzie się znaleźć programu .NET Core CLR i Znajdź program obsługi żądania w trakcie (*aspnetcorev2_inprocess.dll*). Sprawdź, czy:

* Aplikacja jest przeznaczona na albo [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) pakietu NuGet lub [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).
* Wersja udostępnionej platformy ASP.NET Core jest zainstalowanie aplikacji jest przeznaczony dla na komputerze docelowym.

### <a name="5000-out-of-process-handler-load-failure"></a>500.0 Błąd ładowania poza procesem programu obsługi

Proces roboczy kończy się niepowodzeniem. Nie zaczyna się aplikacja.

Modułu ASP.NET Core nie powiedzie się znaleźć spoza procesu hostingu Obsługa żądania. Upewnij się, że *aspnetcorev2_outofprocess.dll* znajduje się w podfolderze obok *aspnetcorev2.dll*.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="50031-ancm-failed-to-find-native-dependencies"></a>500.31 nie można odnaleźć zależności natywnych ANCM

Proces roboczy kończy się niepowodzeniem. Nie zaczyna się aplikacja.

Próbuje uruchomić platformy .NET Core środowiska uruchomieniowego w procesie modułu ASP.NET Core, ale nie została uruchomiona. Najczęstszą przyczyną tego błędu uruchamiania jest, gdy `Microsoft.NETCore.App` lub `Microsoft.AspNetCore.App` środowisko uruchomieniowe nie jest zainstalowany. Jeśli aplikacja jest wdrażana na docelowej platformy ASP.NET Core 3.0, a ta wersja nie istnieje na maszynie, ten błąd występuje. Przykładowy komunikat o błędzie:

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

Komunikat o błędzie zawiera listę wszystkich zainstalowanych wersji platformy .NET Core i wersji, żądane przez aplikację. Aby naprawić ten błąd, to:

* Na komputerze, należy zainstalować odpowiednią wersję programu .NET Core.
* Zmień aplikacji pod kątem określonej wersji programu .NET Core, który znajduje się na komputerze.
* Opublikuj aplikację jako [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd).

Podczas uruchamiania w trakcie opracowywania ( `ASPNETCORE_ENVIRONMENT` ustawiono zmienną środowiskową `Development`), ten błąd jest zapisywany do odpowiedzi HTTP. Przyczyny niepowodzenia uruchamiania procesu znajduje się również w [dziennik zdarzeń aplikacji](#application-event-log).

### <a name="50032-ancm-failed-to-load-dll"></a>500.32 ANCM nie udało się ładowanie biblioteki dll

Proces roboczy kończy się niepowodzeniem. Nie zaczyna się aplikacja.

Najczęstszą przyczyną tego błędu jest to, że aplikacja została opublikowana na potrzeby architektury procesora niezgodne. Jeśli aplikacja została opublikowana do obiektu docelowego w 64-bitowy proces roboczy działa jako aplikacja 32-bitowych, ten błąd występuje.

Aby naprawić ten błąd, to:

* Ponownie opublikować aplikację dla architektury procesorów jako procesu roboczego.
* Opublikuj aplikację jako [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-executables-fde).

### <a name="50033-ancm-request-handler-load-failure"></a>500.33 błąd ładowania program obsługi żądania ANCM

Proces roboczy kończy się niepowodzeniem. Nie zaczyna się aplikacja.

Nie odwoływać się do aplikacji `Microsoft.AspNetCore.App` framework. Tylko aplikacje przeznaczone dla `Microsoft.AspNetCore.App` framework może być obsługiwany przez modułu ASP.NET Core.

Aby naprawić ten błąd, upewnij się, że aplikacja jest przeznaczony dla `Microsoft.AspNetCore.App` framework. Sprawdź `.runtimeconfig.json` Aby zweryfikować framework docelowe przez aplikację.

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a>500.34 ANCM mieszane modelach hostingu nie jest obsługiwane

Proces roboczy nie może uruchomić aplikację spoza procesu i aplikacji w trakcie tego samego procesu.

Aby naprawić ten błąd, należy uruchamiać aplikacje w oddzielnych pul aplikacji usług IIS.

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a>500.35 wiele aplikacji w trakcie ANCM w tym samym procesie

Proces roboczy nie może uruchomić aplikację spoza procesu i aplikacji w trakcie tego samego procesu.

Aby naprawić ten błąd, należy uruchamiać aplikacje w oddzielnych pul aplikacji usług IIS.

### <a name="50036-ancm-out-of-process-handler-load-failure"></a>500.36 błąd ładowania spoza procesu obsługi ANCM

Obsługa żądań poza procesem, *aspnetcorev2_outofprocess.dll*, obok pozycji nie jest *aspnetcorev2.dll* pliku. Oznacza to uszkodzenie instalacji modułu ASP.NET Core.

Aby naprawić ten błąd, napraw instalację [hostingu pakietu programu .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (dla usług IIS) lub Visual Studio (dla usług IIS Express).

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a>500.37 ANCM uruchomienie nie powiodło się przed upływem limitu czasu uruchamiania

ANCM nie można uruchomić w ramach limitu czasu uruchamiania dostarczają. Domyślnie limit czasu wynosi 120 sekund.

Ten błąd może wystąpić podczas uruchamiania dużej liczby aplikacji na tym samym komputerze. Sprawdź, czy wzrostów użycia Procesora/pamięci na serwerze podczas uruchamiania. Może być konieczne przesunąć procesu uruchamiania wielu aplikacji.

### <a name="50030-in-process-startup-failure"></a>500.30 w procesie Niepowodzenie uruchamiania

Proces roboczy kończy się niepowodzeniem. Nie zaczyna się aplikacja.

Próbuje uruchomić platformy .NET Core środowiska uruchomieniowego w procesie modułu ASP.NET Core, ale nie została uruchomiona. Przyczyny niepowodzenia uruchamiania procesu jest zazwyczaj określana na podstawie wpisów w [dziennik zdarzeń aplikacji](#application-event-log) i [dziennika stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log).

### <a name="5000-in-process-handler-load-failure"></a>500.0 w procesie programu obsługi błędu ładowania

Proces roboczy kończy się niepowodzeniem. Nie zaczyna się aplikacja.

Wystąpił nieznany błąd podczas ładowania składników modułu ASP.NET Core. Wykonaj jedną z następujących czynności:

* Skontaktuj się z pomocą [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (wybierz **narzędzi deweloperskich** następnie **platformy ASP.NET Core**).
* Zadaj pytanie w witrynie Stack Overflow.
* Prześlij zgłoszenie na naszych [repozytorium GitHub](https://github.com/aspnet/AspNetCore).

::: moniker-end

### <a name="500-internal-server-error"></a>500 Wewnętrzny błąd serwera

Uruchamia aplikację, ale błąd uniemożliwia spełnienie żądania przez serwer.

Ten błąd występuje w kodzie aplikacji, podczas uruchamiania lub podczas tworzenia odpowiedzi. Odpowiedź może zawierać żadnej zawartości lub odpowiedzi może być wyświetlana jako *500 Wewnętrzny błąd serwera* w przeglądarce. W dzienniku zdarzeń aplikacji stwierdza, zwykle uruchomiona aplikacja. Z perspektywy serwera, który jest poprawna. Aplikacja została uruchomiona, ale nie może wygenerować prawidłowej odpowiedzi. [Uruchamianie aplikacji w wierszu polecenia](#run-the-app-at-a-command-prompt) na serwerze lub [Włącz dziennik stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log) do rozwiązania problemu.

### <a name="failed-to-start-application-errorcode-0x800700c1"></a>Nie można uruchomić aplikację (kod błędu "0x800700c1")

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

Aplikacji nie powiodło się, ponieważ zestaw aplikacji ( *.dll*) nie można go załadować.

Ten błąd występuje, gdy występuje niezgodność liczby bitów opublikowanej aplikacji i procesu w3wp/programu iisexpress.

Upewnij się, że ustawienie 32-bitowych puli aplikacji jest prawidłowy:

1. Wybierz pulę aplikacji w Menedżerze usług IIS w **pul aplikacji**.
1. Wybierz **Zaawansowane ustawienia** w obszarze **edytowanie puli aplikacji** w **akcje** panelu.
1. Ustaw **Włącz aplikacje 32-bitowe**:
   * Jeśli wdrażanie (x86) 32-bitowych aplikacji, ustaw wartość `True`.
   * Jeśli wdrażanie (x64) 64-bitowych aplikacji, ustaw wartość `False`.

### <a name="connection-reset"></a>Resetowanie połączenia

Jeśli błąd wystąpi po nagłówki są wysyłane, jest za późno serwera wysłać **500 Wewnętrzny błąd serwera** po wystąpieniu błędu. Dzieje się tak często, gdy wystąpi błąd podczas serializacji obiektów złożonych na odpowiedź. Tego typu błędu jest wyświetlany jako *resetowania połączenia* błąd na komputerze klienckim. [Rejestrowanie aplikacji](xref:fundamentals/logging/index) mogą pomóc rozwiązać tego rodzaju błędów.

## <a name="default-startup-limits"></a>Domyślne limity uruchamiania

Modułu ASP.NET Core jest skonfigurowany z domyślną *startupTimeLimit* 120 sekund. Gdy pozostawić wartość domyślną, aplikacja może potrwać do dwóch minut przed moduł dzienniki awarii procesu. Aby uzyskać informacje na temat konfigurowania modułu, zobacz [atrybuty elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Rozwiązywanie problemów z błędami uruchamiania aplikacji

### <a name="enable-the-aspnet-core-module-debug-log"></a>Włącz dziennik debugowania modułu ASP.NET Core

Dodaj poniższe ustawienia programu obsługi w aplikacji *web.config* plik, aby włączyć dzienniki debugowania modułu ASP.NET Core:

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

Upewnij się, czy ścieżka określona dla dziennika istnieje i że tożsamość puli aplikacji ma uprawnienia do zapisu do lokalizacji.

### <a name="application-event-log"></a>Dziennik zdarzeń aplikacji

Dostęp do dziennika zdarzeń aplikacji:

1. Otwieranie Start menu, wyszukaj **Podgląd zdarzeń**, a następnie wybierz pozycję **Podgląd zdarzeń** aplikacji.
1. W **Podgląd zdarzeń**, otwórz **Dzienniki Windows** węzła.
1. Wybierz **aplikacji** można otworzyć dziennika zdarzeń aplikacji.
1. Wyszukaj błędy skojarzone z aplikacją się niepowodzeniem. Błędy mają wartość *moduł AspNetCore IIS* lub *usług IIS Express AspNetCore modułu* w *źródła* kolumny.

### <a name="run-the-app-at-a-command-prompt"></a>Uruchamianie aplikacji w wierszu polecenia

Wiele błędów uruchamiania przestaną generować przydatne informacje w dzienniku zdarzeń aplikacji. Przyczyną niektórych błędów można znaleźć, uruchamiając aplikację w wierszu polecenia w systemie hostingu.

#### <a name="framework-dependent-deployment"></a>Wdrożenie zależny od struktury

Jeśli aplikacja jest [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

1. W wierszu polecenia przejdź do folderu wdrożenia i uruchomienia aplikacji, wykonując zestaw aplikacji za pomocą *dotnet.exe*. W poniższym poleceniu zastąp nazwę zestawu aplikacji dla \<assembly_name >: `dotnet .\<assembly_name>.dll`.
1. Dane wyjściowe z aplikacji, przedstawiający wszystkie błędy z konsoli są zapisywane w oknie konsoli.
1. Jeśli wystąpią błędy, gdy kieruje żądanie do aplikacji, należy wysłać żądanie do hosta i portu, na którym nasłuchuje Kestrel. Przy użyciu domyślnego hosta i post, zgłosić wniosek o `http://localhost:5000/`. Jeśli aplikacja reaguje, zwykle pod adresem punktu końcowego Kestrel, problem najprawdopodobniej związanych z konfiguracją hostingu i mniej prawdopodobne w aplikacji.

#### <a name="self-contained-deployment"></a>Niezależne wdrożenia

Jeśli aplikacja jest [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd):

1. W wierszu polecenia przejdź do folderu wdrożenia i uruchomienia pliku wykonywalnego aplikacji. W poniższym poleceniu zastąp nazwę zestawu aplikacji dla \<assembly_name >: `<assembly_name>.exe`.
1. Dane wyjściowe z aplikacji, przedstawiający wszystkie błędy z konsoli są zapisywane w oknie konsoli.
1. Jeśli wystąpią błędy, gdy kieruje żądanie do aplikacji, należy wysłać żądanie do hosta i portu, na którym nasłuchuje Kestrel. Przy użyciu domyślnego hosta i post, zgłosić wniosek o `http://localhost:5000/`. Jeśli aplikacja reaguje, zwykle pod adresem punktu końcowego Kestrel, problem najprawdopodobniej związanych z konfiguracją hostingu i mniej prawdopodobne w aplikacji.

### <a name="aspnet-core-module-stdout-log"></a>ASP.NET Core modułu stdout dziennika

Włączanie i wyświetlanie dzienników stdout:

1. Przejdź do folderu wdrożenia witryny w systemie hostingu.
1. Jeśli *dzienniki* folder nie jest obecny, Utwórz folder. Aby uzyskać instrukcje dotyczące włączania MSBuild tworzenia *dzienniki* folderu we wdrożeniu automatycznie, zobacz [strukturę katalogów](xref:host-and-deploy/directory-structure) tematu.
1. Edytuj *web.config* pliku. Ustaw **stdoutLogEnabled** do `true` i zmień **stdoutLogFile** ścieżki, aby wskazywał *dzienniki* folderu (na przykład `.\logs\stdout`). `stdout` w ścieżce jest prefiks nazwy pliku dziennika. Sygnatura czasowa, identyfikator procesu i rozszerzenie pliku są dodawane automatycznie, gdy zostanie utworzony dziennik. Za pomocą `stdout` jako prefiks nazwy pliku, plik dziennika typowe o nazwie *stdout_20180205184032_5412.log*.
1. Upewnij się, tożsamość puli aplikacji ma uprawnienia do zapisu *dzienniki* folderu.
1. Zapisz zaktualizowany *web.config* pliku.
1. Wysłać żądanie do aplikacji.
1. Przejdź do *dzienniki* folderu. Znajdowanie i otwieranie najnowszych dziennika stdout.
1. Badanie w dzienniku błędów.

> [!IMPORTANT]
> Wyłącz rejestrowanie strumienia stdout, po zakończeniu rozwiązywania problemów.

1. Edytuj *web.config* pliku.
1. Ustaw **stdoutLogEnabled** do `false`.
1. Zapisz plik.

> [!WARNING]
> Nie można wyłączyć dziennika stdout może prowadzić do awarii aplikacji lub serwera. Brak brak limitu rozmiaru pliku dziennika lub liczba pliki dziennika utworzone.
>
> Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje. Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="enable-the-developer-exception-page"></a>Włącz na stronie wyjątków dla deweloperów

`ASPNETCORE_ENVIRONMENT` [Zmiennej środowiskowej, można dodać do pliku web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) do uruchomienia aplikacji w środowisku programistycznym. Tak długo, jak środowisko nie jest zastąpione przy uruchamianiu aplikacji przez `UseEnvironment` umożliwia ustawienie zmiennej środowiskowej w Konstruktorze hosta [stronie wyjątków deweloperów](xref:fundamentals/error-handling) się pojawiać po uruchomieniu aplikacji.

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

Ustawienie zmiennej środowiskowej, aby uzyskać `ASPNETCORE_ENVIRONMENT` jest zalecane tylko dla używane w przejściowym i testowania serwerów, które nie są połączone z Internetem. Usuń zmienną środowiskową z *web.config* plik po rozwiązywania problemów. Aby uzyskać informacje na temat ustawiania zmiennych środowiskowych *web.config*, zobacz [environmentVariables element podrzędny elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

## <a name="common-startup-errors"></a>Typowe błędy uruchamiania

Zobacz <xref:host-and-deploy/azure-iis-errors-reference>. Najbardziej typowe problemy, które uniemożliwiają uruchamianie aplikacji znajdują się w temacie odwołania.

## <a name="obtain-data-from-an-app"></a>Uzyskiwanie danych z aplikacji

Jeśli aplikacja jest w stanie odpowiadać na żądania, żądania, połączenia i dodatkowych danych można uzyskać z aplikację za pomocą oprogramowania pośredniczącego terminalu wbudowanego. Aby uzyskać więcej informacji i przykładowy kod, zobacz <xref:test/troubleshoot#obtain-data-from-an-app>.

## <a name="create-a-dump"></a>Utwórz zrzut

A *zrzutu* jest migawką pamięci systemowej i mogą pomóc w określeniu przyczyny awarii aplikacji, Niepowodzenie uruchamiania lub powolne aplikacji.

### <a name="app-crashes-or-encounters-an-exception"></a>Aplikacja ulegnie awarii lub napotka wyjątek

Uzyskaj i analizowanie zrzutu z [raportowania błędów Windows (WER)](/windows/desktop/wer/windows-error-reporting):

1. Utwórz folder do przechowywania plików zrzutu awaryjnego na `c:\dumps`. Pula aplikacji musi mieć dostęp do zapisu do folderu.
1. Uruchom [skrypt programu EnableDumps PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):
   * Jeśli aplikacja korzysta z [modelu hostingu w trakcie](xref:host-and-deploy/iis/index#in-process-hosting-model), uruchom skrypt *w3wp.exe*:

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * Jeśli aplikacja korzysta z [modelu hostingu poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model), uruchom skrypt *dotnet.exe*:

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. Uruchom aplikację zgodnie z warunkami, które powodują awarię wystąpić.
1. Po przeprowadzeniu awarii Uruchom [skrypt programu DisableDumps PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):
   * Jeśli aplikacja korzysta z [modelu hostingu w trakcie](xref:host-and-deploy/iis/index#in-process-hosting-model), uruchom skrypt *w3wp.exe*:

     ```console
     .\DisableDumps w3wp.exe
     ```

   * Jeśli aplikacja korzysta z [modelu hostingu poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model), uruchom skrypt *dotnet.exe*:

     ```console
     .\DisableDumps dotnet.exe
     ```

Po zakończeniu zbierania zrzutu awarii aplikacji oraz aplikację może zakończyć w zwykły sposób. Skrypt programu PowerShell umożliwia skonfigurowanie raportowania błędów systemu Windows do zbierania zrzutów maksymalnie pięć na aplikację.

> [!WARNING]
> Zrzuty awaryjne może potrwać dużą ilość miejsca na dysku (maksymalnie kilka gigabajtów każdego).

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a>Aplikacja zawiesza się zakończy się niepowodzeniem podczas uruchamiania i działa normalnie

Gdy aplikacja *zawiesza się* (przestaje odpowiadać, ale nie awarii), zakończy się niepowodzeniem podczas uruchamiania lub działa normalnie, zobacz [plików zrzut trybu użytkownika: Wybieranie najlepszych narzędzi](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) do wybrania odpowiedniego narzędzia do tworzenia zrzutu.

### <a name="analyze-the-dump"></a>Analizowanie zrzutu

Zrzut można analizować przy użyciu kilku metod. Aby uzyskać więcej informacji, zobacz [analizowanie plik zrzutu trybu użytkownika](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).

## <a name="remote-debugging"></a>Debugowanie zdalne

Zobacz [zdalne debugowanie platformy ASP.NET Core na komputerze zdalnym usług IIS w programie Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) w dokumentacji programu Visual Studio.

## <a name="application-insights"></a>Application Insights

[Usługa Application Insights](/azure/application-insights/) udostępnia dane telemetryczne z aplikacji hostowanych przez usługi IIS, w tym rejestrowanie i funkcji raportowania błędów. Usługa Application Insights można tylko raporty dotyczące błędów występujących po uruchomieniu aplikacji, gdy staną się dostępne funkcje rejestrowania aplikacji. Aby uzyskać więcej informacji, zobacz [usługi Application Insights dla platformy ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="additional-advice"></a>Dodatkowe porady

Czasami funkcjonalności aplikacji nie powiedzie się natychmiast po uaktualnieniu albo .NET Core SDK w wersjach maszyny lub pakiet rozwoju w aplikacji. W niektórych przypadkach niespójne pakietów może spowodować uszkodzenie aplikacji podczas przeprowadzania uaktualnienia głównych. Większość z tych problemów można naprawić, wykonując następujące instrukcje:

1. Usuń *bin* i *obj* folderów.
1. Wyczyść pakiet zapisuje w pamięci podręcznej w *% UserProfile %\\.nuget\\pakietów* i *% LocalAppData %\\Nuget\\pamięci podręcznej v3*.
1. Przywróć i skompiluj ponownie projekt.
1. Upewnij się, że poprzedniego wdrożenia na serwerze została całkowicie usunięta przed ponownego wdrażania aplikacji.

> [!TIP]
> Wygodnym sposobem Wyczyść pamięć podręczną pakietu ma wykonać `dotnet nuget locals all --clear` z poziomu wiersza polecenia.
>
> Wyczyszczenie pamięci podręcznej pakietu może być również wykonywane przy użyciu [nuget.exe](https://www.nuget.org/downloads) narzędzie i wykonywania polecenia `nuget locals all -clear`. *nuget.exe* nie jest powiązane instalacji z pulpitu systemu operacyjnego Windows i należy uzyskać oddzielnie od [NuGet witryny sieci Web](https://www.nuget.org/downloads).

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
