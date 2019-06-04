---
title: Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service
author: guardrex
description: Dowiedz się, jak diagnozować problemy z wdrożeniami platformy ASP.NET Core usługi Azure App Service.
ms.author: riande
ms.custom: mvc
ms.date: 03/06/2019
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: 7a0bb7df27ebbea0eac79771452295846fad563a
ms.sourcegitcommit: a04eb20e81243930ec829a9db5dd5de49f669450
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/03/2019
ms.locfileid: "66470442"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a>Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service

Przez [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

Ten artykuł zawiera instrukcje na temat platformy ASP.NET Core zdiagnozować problem uruchamiania aplikacji za pomocą narzędzi diagnostycznych w usłudze Azure App Service. Aby uzyskać dodatkowe porady dotyczące rozwiązywania problemów, zobacz [Omówienie diagnostyki usługi Azure App Service](/azure/app-service/app-service-diagnostics) i [jak: Monitorowanie aplikacji w usłudze Azure App Service](/azure/app-service/web-sites-monitor) w dokumentacji platformy Azure.

## <a name="app-startup-errors"></a>Błędy uruchamiania aplikacji

**502.5 przetwarzania awarii** proces roboczy kończy się niepowodzeniem. Nie zaczyna się aplikacja.

[Modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) próby uruchomienia procesu roboczego, ale nie została uruchomiona. Badanie w dzienniku zdarzeń aplikacji często pomaga rozwiązać tego rodzaju problemów. Dostęp do dziennika zostało wyjaśnione w [dziennik zdarzeń aplikacji](#application-event-log) sekcji.

*502.5 niepowodzenia procesu* strony błędu jest zwracany, jeśli niepoprawnie skonfigurowany aplikacji powoduje, że proces roboczy nie powiedzie się:

![Okno przeglądarki, przedstawiający 502.5 stronę niepowodzenia procesu](troubleshoot/_static/process-failure-page.png)

**500 Wewnętrzny błąd serwera**

Uruchamia aplikację, ale błąd uniemożliwia spełnienie żądania przez serwer.

Ten błąd występuje w kodzie aplikacji, podczas uruchamiania lub podczas tworzenia odpowiedzi. Odpowiedź może zawierać żadnej zawartości lub odpowiedzi może być wyświetlana jako *500 Wewnętrzny błąd serwera* w przeglądarce. W dzienniku zdarzeń aplikacji stwierdza, zwykle uruchomiona aplikacja. Z perspektywy serwera, który jest poprawna. Aplikacja została uruchomiona, ale nie może wygenerować prawidłowej odpowiedzi. [Uruchamianie aplikacji w konsoli Kudu](#run-the-app-in-the-kudu-console) lub [Włącz dziennik stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log) do rozwiązania problemu.

::: moniker range="= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a>500.30 w procesie Niepowodzenie uruchamiania

Proces roboczy kończy się niepowodzeniem. Nie zaczyna się aplikacja.

Próbuje uruchomić program .NET Core CLR w procesie modułu ASP.NET Core, ale nie została uruchomiona. Przyczyny niepowodzenia uruchamiania procesu jest zazwyczaj określana na podstawie wpisów w [dziennik zdarzeń aplikacji](#application-event-log) i [dziennika stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log).

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

Przyczyny niepowodzenia uruchamiania procesu znajduje się również w [dziennik zdarzeń aplikacji](#application-event-log).

::: moniker-end

**Resetowanie połączenia**

Jeśli błąd wystąpi po nagłówki są wysyłane, jest za późno serwera wysłać **500 Wewnętrzny błąd serwera** po wystąpieniu błędu. Dzieje się tak często, gdy wystąpi błąd podczas serializacji obiektów złożonych na odpowiedź. Tego typu błędu jest wyświetlany jako *resetowania połączenia* błąd na komputerze klienckim. [Rejestrowanie aplikacji](xref:fundamentals/logging/index) mogą pomóc rozwiązać tego rodzaju błędów.

## <a name="default-startup-limits"></a>Domyślne limity uruchamiania

Modułu ASP.NET Core jest skonfigurowany z domyślną *startupTimeLimit* 120 sekund. Gdy pozostawić wartość domyślną, aplikacja może potrwać do dwóch minut przed moduł dzienniki awarii procesu. Aby uzyskać informacje na temat konfigurowania modułu, zobacz [atrybuty elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Rozwiązywanie problemów z błędami uruchamiania aplikacji

### <a name="application-event-log"></a>Dziennik zdarzeń aplikacji

Aby uzyskać dostęp do dziennika zdarzeń aplikacji, należy użyć **diagnozowanie i rozwiązywanie problemów** bloku w witrynie Azure portal:

1. W witrynie Azure portal, należy otworzyć aplikację w **App Services**.
1. Wybierz **diagnozowanie i rozwiązywanie problemów**.
1. Wybierz **narzędzia diagnostyczne** nagłówka.
1. W obszarze **Support Tools**, wybierz opcję **zdarzenia aplikacji** przycisku.
1. Zapoznaj się z błędem najnowsze dostarczone przez *IIS AspNetCoreModule* lub *AspNetCoreModule usług IIS w wersji 2* wpisu w **źródła** kolumny.

Alternatywa dla użycia **diagnozowanie i rozwiązywanie problemów** bloku jest zbadanie pliku dziennika zdarzeń aplikacji bezpośrednio za pomocą [Kudu](https://github.com/projectkudu/kudu/wiki):

1. Otwórz **Narzędzia zaawansowane** w **narzędzia programistyczne** obszaru. Wybierz **Przejdź&rarr;**  przycisku. W nowej karcie przeglądarki lub w oknie zostanie otwarta konsola Kudu.
1. Za pomocą paska nawigacji u góry strony, otwórz **konsoli debugowania** i wybierz **CMD**.
1. Otwórz **LogFiles** folderu.
1. Wybierz ikonę ołówka obok *eventlog.xml* pliku.
1. Przeanalizuj dziennik. Przewiń do dołu najnowszych zdarzeń w dzienniku.

### <a name="run-the-app-in-the-kudu-console"></a>Uruchamianie aplikacji w konsoli programu Kudu

Wiele błędów uruchamiania przestaną generować przydatne informacje w dzienniku zdarzeń aplikacji. Aplikację można uruchomić [Kudu](https://github.com/projectkudu/kudu/wiki) konsoli zdalnej wykonywania, aby dowiedzieć się, błąd:

1. Otwórz **Narzędzia zaawansowane** w **narzędzia programistyczne** obszaru. Wybierz **Przejdź&rarr;**  przycisku. W nowej karcie przeglądarki lub w oknie zostanie otwarta konsola Kudu.
1. Za pomocą paska nawigacji u góry strony, otwórz **konsoli debugowania** i wybierz **CMD**.

#### <a name="test-a-32-bit-x86-app"></a>Testowanie aplikacji 32-bitowych (x 86)

##### <a name="current-release"></a>Bieżąca wersja

1. `cd d:\home\site\wwwroot`
1. Uruchom aplikację:
   * Jeśli aplikacja jest [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

     ```console
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * Jeśli aplikacja jest [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd):

     ```console
     {ASSEMBLY NAME}.exe
     ```

Dane wyjściowe z aplikacji, przedstawiający wszystkie błędy z konsoli jest przekazywany w potoku do konsoli Kudu.

##### <a name="framework-dependent-deployment-running-on-a-preview-release"></a>Wdrożenie zależny od struktury usługi uruchomione w wersji zapoznawczej

*Wymaga zainstalowania platformy ASP.NET Core {VERSION} (x86) rozszerzenia witryny środowiska uruchomieniowego.*

1. `cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` jest wersja środowiska wykonawczego)
1. Uruchom aplikację: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`

Dane wyjściowe z aplikacji, przedstawiający wszystkie błędy z konsoli jest przekazywany w potoku do konsoli Kudu.

#### <a name="test-a-64-bit-x64-app"></a>Testowanie aplikacji 64-bitowych (x 64)

##### <a name="current-release"></a>Bieżąca wersja

* Jeśli aplikacja jest (x64) 64-bitowych [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd):
  1. `cd D:\Program Files\dotnet`
  1. Uruchom aplikację: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`
* Jeśli aplikacja jest [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd):
  1. `cd D:\home\site\wwwroot`
  1. Uruchom aplikację: `{ASSEMBLY NAME}.exe`

Dane wyjściowe z aplikacji, przedstawiający wszystkie błędy z konsoli jest przekazywany w potoku do konsoli Kudu.

##### <a name="framework-dependent-deployment-running-on-a-preview-release"></a>Wdrożenie zależny od struktury usługi uruchomione w wersji zapoznawczej

*Wymaga zainstalowania platformy ASP.NET Core {VERSION} (x64) rozszerzenia witryny środowiska uruchomieniowego.*

1. `cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` jest wersja środowiska wykonawczego)
1. Uruchom aplikację: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`

Dane wyjściowe z aplikacji, przedstawiający wszystkie błędy z konsoli jest przekazywany w potoku do konsoli Kudu.

### <a name="aspnet-core-module-stdout-log"></a>ASP.NET Core modułu stdout dziennika

Plik dziennika stdout modułu ASP.NET Core rejestruje często przydatne komunikaty o błędach nie można odnaleźć w dzienniku zdarzeń aplikacji. Włączanie i wyświetlanie dzienników stdout:

1. Przejdź do **diagnozowanie i rozwiązywanie problemów** bloku w witrynie Azure portal.
1. W obszarze **wybierz kategorię problemu**, wybierz opcję **niedziałającej aplikacji internetowej** przycisku.
1. W obszarze **sugerowane rozwiązania** > **włączyć przekierowywanie dzienników Stdout**, wybierz przycisk, aby **otwartej konsoli Kudu, aby edytować plik Web.Config**.
1. W Kudu **konsoli diagnostyki**, otwieranie folderów w ścieżce **witryny** > **wwwroot**. Przewiń w dół do ujawniania *web.config* pliku w dolnej części listy.
1. Kliknij ikonę ołówka obok *web.config* pliku.
1. Ustaw **stdoutLogEnabled** do `true` i zmień **stdoutLogFile** ścieżka: `\\?\%home%\LogFiles\stdout`.
1. Wybierz **Zapisz** zapisać zaktualizowanego *web.config* pliku.
1. Wysłać żądanie do aplikacji.
1. Wróć do witryny Azure portal. Wybierz **Narzędzia zaawansowane** bloku **narzędzia PROGRAMISTYCZNE** obszaru. Wybierz **Przejdź&rarr;**  przycisku. W nowej karcie przeglądarki lub w oknie zostanie otwarta konsola Kudu.
1. Za pomocą paska nawigacji u góry strony, otwórz **konsoli debugowania** i wybierz **CMD**.
1. Wybierz **LogFiles** folderu.
1. Sprawdzanie **zmodyfikowane** kolumny i wybierz ikonę ołówka, aby edytować stdout dziennika z najnowszą datą modyfikacji.
1. Po otwarciu pliku dziennika jest wyświetlany błąd.

Aby wyłączyć rejestrowanie strumienia stdout, po zakończeniu rozwiązywania problemów:

1. W Kudu **konsoli diagnostyki**, wróć do ścieżki **witryny** > **wwwroot** aby ujawnić *web.config* pliku. Otwórz **web.config** plik ponownie, wybierając ikonę ołówka.
1. Ustaw **stdoutLogEnabled** do `false`.
1. Wybierz **Zapisz** można zapisać pliku.

> [!WARNING]
> Nie można wyłączyć dziennika stdout może prowadzić do awarii aplikacji lub serwera. Brak brak limitu rozmiaru pliku dziennika lub liczba pliki dziennika utworzone. Należy używać tylko stdout, rejestrowanie, aby rozwiązać problemy z uruchamianiem aplikacji.
>
> Ogólne logujesz się w aplikacji ASP.NET Core po uruchomieniu, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje. Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log"></a>Dziennik debugowania modułu platformy ASP.NET Core

Dziennik debugowania modułu ASP.NET Core zapewnia rejestrowanie dodatkowych, lepiej od modułu ASP.NET Core. Włączanie i wyświetlanie dzienników stdout:

1. Aby włączyć rozszerzony dziennik diagnostyczny, wykonaj jedną z następujących czynności:
   * Postępuj zgodnie z instrukcjami w [rozszerzone dzienniki diagnostyczne](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) do konfigurowania aplikacji na potrzeby rozszerzonego rejestrowania diagnostycznego. Ponowne wdrażanie aplikacji.
   * Dodaj `<handlerSettings>` wyświetlane w [rozszerzone dzienniki diagnostyczne](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) do działającej aplikacji *web.config* plików przy użyciu konsoli Kudu:
     1. Otwórz **Narzędzia zaawansowane** w **narzędzia programistyczne** obszaru. Wybierz **Przejdź&rarr;**  przycisku. W nowej karcie przeglądarki lub w oknie zostanie otwarta konsola Kudu.
     1. Za pomocą paska nawigacji u góry strony, otwórz **konsoli debugowania** i wybierz **CMD**.
     1. Otwieranie folderów w ścieżce **witryny** > **wwwroot**. Edytuj *web.config* pliku, wybierając przycisk ołówka. Dodaj `<handlerSettings>` jak pokazano na [rozszerzone dzienniki diagnostyczne](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs). Wybierz ikonę **Zapisz**.
1. Otwórz **Narzędzia zaawansowane** w **narzędzia programistyczne** obszaru. Wybierz **Przejdź&rarr;**  przycisku. W nowej karcie przeglądarki lub w oknie zostanie otwarta konsola Kudu.
1. Za pomocą paska nawigacji u góry strony, otwórz **konsoli debugowania** i wybierz **CMD**.
1. Otwieranie folderów w ścieżce **witryny** > **wwwroot**. Jeśli nie podasz ścieżkę *czy aspnetcore* pliku, plik zostanie wyświetlony na liście. Jeśli podano ścieżkę, przejdź do lokalizacji pliku dziennika.
1. Otwórz plik dziennika, klikając przycisk ołówka obok nazwy pliku.

Aby wyłączyć rejestrowanie debugowania, po zakończeniu rozwiązywania problemów:

1. Aby wyłączyć dziennik debugowania rozszerzone, wykonaj jedną z następujących czynności:
   * Usuń `<handlerSettings>` z *web.config* lokalnie plik i ponownie wdrożyć aplikację.
   * Konsola Kudu umożliwia edytowanie *web.config* pliku i usuwania `<handlerSettings>` sekcji. Zapisz plik.

> [!WARNING]
> Nie można wyłączyć dziennik debugowania może prowadzić do awarii aplikacji lub serwera. Nie ma żadnego limitu rozmiaru pliku dziennika. Tylko umożliwia rejestrowanie debugowania aplikacji rozwiązywania problemów z uruchamianiem.
>
> Ogólne logujesz się w aplikacji ASP.NET Core po uruchomieniu, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje. Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).

::: moniker-end

## <a name="common-startup-errors"></a>Typowe błędy uruchamiania

Zobacz <xref:host-and-deploy/azure-iis-errors-reference>. Najbardziej typowe problemy, które uniemożliwiają uruchamianie aplikacji znajdują się w temacie odwołania.

## <a name="slow-or-hanging-app"></a>Wolne lub zwisa aplikacji

Gdy aplikacja będzie odpowiadać wolno lub zawiesza się na żądanie, zobacz następujące artykuły:

* [Powolne sieci web app Rozwiązywanie problemów z wydajnością w usłudze Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Umożliwia rozszerzenie witryny usługi Crash diagnostyki przechwytywania zrzutu wyjątek sporadyczne problemy lub problemy z wydajnością w usłudze Azure Web App](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)

## <a name="remote-debugging"></a>Debugowanie zdalne

Zobacz następujące tematy:

* [Zdalne debugowanie aplikacji sieci web części Rozwiązywanie problemów aplikacji sieci web w usłudze Azure App Service przy użyciu programu Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (dokumentacja platformy Azure)
* [Zdalne debugowanie platformy ASP.NET Core w usługach IIS na platformie Azure w programie Visual Studio 2017](/visualstudio/debugger/remote-debugging-azure) (Dokumentacja programu Visual Studio)

## <a name="application-insights"></a>Application Insights

[Usługa Application Insights](https://azure.microsoft.com/services/application-insights/) udostępnia dane telemetryczne z aplikacji hostowanych w usłudze Azure App Service, w tym rejestrowanie i funkcji raportowania błędów. Usługa Application Insights można tylko raporty dotyczące błędów występujących po uruchomieniu aplikacji, gdy staną się dostępne funkcje rejestrowania aplikacji. Aby uzyskać więcej informacji, zobacz [usługi Application Insights dla platformy ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="monitoring-blades"></a>Monitorowanie bloków

Bloki monitorowania alternatywną środowisko do metod opisanych wcześniej w temacie dotyczącym rozwiązywania problemów. Te bloki może służyć do diagnozowania błędów 500 serii.

Upewnij się, czy są zainstalowane rozszerzenia programu ASP.NET Core. Rozszerzenia nie są zainstalowane, zainstaluj je ręcznie:

1. W **narzędzia PROGRAMISTYCZNE** bloku zaznacz **rozszerzenia** bloku.
1. **Platformy ASP.NET Core rozszerzenia** powinna zostać wyświetlona na liście.
1. Jeśli są zainstalowane rozszerzenia, wybierz opcję **Dodaj** przycisku.
1. Wybierz **platformy ASP.NET Core rozszerzenia** z listy.
1. Wybierz **OK** aby zaakceptować warunki prawne.
1. Wybierz **OK** na **Dodaj rozszerzenie** bloku.
1. Komunikat informacyjny wyskakującego wskazuje, po pomyślnym zainstalowaniu rozszerzenia.

Jeśli rejestrowanie strumienia wyjściowego stdout nie jest włączone, wykonaj następujące kroki:

1. W witrynie Azure portal wybierz **Narzędzia zaawansowane** bloku **narzędzia PROGRAMISTYCZNE** obszaru. Wybierz **Przejdź&rarr;**  przycisku. W nowej karcie przeglądarki lub w oknie zostanie otwarta konsola Kudu.
1. Za pomocą paska nawigacji u góry strony, otwórz **konsoli debugowania** i wybierz **CMD**.
1. Otwieranie folderów w ścieżce **witryny** > **wwwroot** i przewiń w dół do ujawniania *web.config* pliku w dolnej części listy.
1. Kliknij ikonę ołówka obok *web.config* pliku.
1. Ustaw **stdoutLogEnabled** do `true` i zmień **stdoutLogFile** ścieżka: `\\?\%home%\LogFiles\stdout`.
1. Wybierz **Zapisz** zapisać zaktualizowanego *web.config* pliku.

Przejdź do aktywowania rejestrowania diagnostycznego:

1. W witrynie Azure portal wybierz **dzienniki diagnostyczne** bloku.
1. Wybierz **na** przełączać **rejestrowanie aplikacji (system plików)** i **szczegółowe komunikaty o błędach**. Wybierz **Zapisz** znajdujący się u góry bloku.
1. Aby dołączyć śledzenie nieudanych żądań, tzw. Niepowodzenie żądania zdarzenia buforowania (FREB) rejestrowanie, wybierz **na** przełączać **śledzenie nieudanych żądań**.
1. Wybierz **strumień dziennika** bloku, który znajduje się natychmiast w obszarze **dzienniki diagnostyczne** bloku w portalu.
1. Wysłać żądanie do aplikacji.
1. W ramach transmisji danych dziennika wskazuje przyczynę błędu.

Pamiętaj wyłączyć rejestrowanie strumienia stdout, po zakończeniu rozwiązywania problemów. Zapoznaj się z instrukcjami w [dziennika stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log) sekcji.

Aby wyświetlić dzienniki śledzenia nieudanych żądań (Dzienniki FREB):

1. Przejdź do **diagnozowanie i rozwiązywanie problemów** bloku w witrynie Azure portal.
1. Wybierz **nie powiodło się dzienniki śledzenia żądań** z **SUPPORT TOOLS** obszar na pasku bocznym.

Zobacz [żądania zakończone niepowodzeniem śledzi sekcji Włącz rejestrowanie diagnostyki dla aplikacji sieci web w usłudze Azure App Service temacie](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) i [wydajność aplikacji — często zadawane pytania dla aplikacji sieci Web na platformie Azure: Jak włączyć śledzenie niepomyślnych żądań ](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) Aby uzyskać więcej informacji.

Aby uzyskać więcej informacji, zobacz [Włączanie rejestrowania diagnostycznego dla aplikacji sieci web w usłudze Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).

> [!WARNING]
> Nie można wyłączyć dziennika stdout może prowadzić do awarii aplikacji lub serwera. Brak brak limitu rozmiaru pliku dziennika lub liczba pliki dziennika utworzone.
>
> Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje. Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [Rozwiązywanie problemów z aplikacją sieci web w usłudze Azure App Service przy użyciu programu Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Rozwiązywanie problemów z błędami HTTP "502 — Zła brama" i "503 Usługa niedostępna" w aplikacjach sieci web platformy Azure](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [Powolne sieci web app Rozwiązywanie problemów z wydajnością w usłudze Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Wydajność aplikacji — często zadawane pytania dla aplikacji sieci Web na platformie Azure](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Azure piaskownica aplikacji sieci Web (ograniczenia wykonywania środowiska uruchomieniowego usługi App Service)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [Azure Friday: Azure diagnostyki usługi aplikacji i rozwiązywanie problemów z doświadczenia (12-minutowy klip wideo)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
