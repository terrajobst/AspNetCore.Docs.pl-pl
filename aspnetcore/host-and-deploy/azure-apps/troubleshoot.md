---
title: Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service
author: guardrex
description: Dowiedz się, jak diagnozować problemy z wdrożeniami platformy ASP.NET Core usługi Azure App Service.
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2019
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: d78499c1a82a011239f6b62b546f304a5d5017e2
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313759"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a>Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service

Przez [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

Ten artykuł zawiera instrukcje na temat platformy ASP.NET Core zdiagnozować problem uruchamiania aplikacji za pomocą narzędzi diagnostycznych w usłudze Azure App Service. Aby uzyskać dodatkowe porady dotyczące rozwiązywania problemów, zobacz [Omówienie diagnostyki usługi Azure App Service](/azure/app-service/app-service-diagnostics) i [jak: Monitorowanie aplikacji w usłudze Azure App Service](/azure/app-service/web-sites-monitor) w dokumentacji platformy Azure.

Dodatkowe tematy dotyczące rozwiązywania problemów:

* Używa również usług IIS [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) do hostowania aplikacji. Aby uzyskać porady, które odnoszą się do usług IIS dotyczące rozwiązywania problemów, zobacz <xref:host-and-deploy/iis/troubleshoot>.
* <xref:fundamentals/error-handling> opisano sposób obsługi błędów w aplikacji platformy ASP.NET Core podczas programowania w systemie lokalnym.
* [Naucz się debugować przy użyciu programu Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) wprowadza nowe funkcje debugera programu Visual Studio.
* [Debugowanie za pomocą programu Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) w tym artykule opisano obsługę debugowania wbudowana w programie Visual Studio Code.

[!INCLUDE[](~/includes/azure-iis-startup-errors.md)]

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
