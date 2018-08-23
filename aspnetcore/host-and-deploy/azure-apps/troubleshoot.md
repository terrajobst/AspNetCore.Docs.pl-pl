---
title: Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service
author: guardrex
description: Dowiedz się, jak diagnozować problemy z wdrożeniami platformy ASP.NET Core usługi Azure App Service.
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2018
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: a995c743b4e43be8bea5329affb3f2c736b1d016
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/22/2018
ms.locfileid: "41902557"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a>Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service

Przez [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

Ten artykuł zawiera instrukcje na temat platformy ASP.NET Core zdiagnozować problem uruchamiania aplikacji za pomocą narzędzi diagnostycznych w usłudze Azure App Service. Aby uzyskać dodatkowe porady dotyczące rozwiązywania problemów, zobacz [Omówienie diagnostyki usługi Azure App Service](/azure/app-service/app-service-diagnostics) i [jak: monitorowanie aplikacji w usłudze Azure App Service](/azure/app-service/web-sites-monitor) w dokumentacji platformy Azure.

## <a name="app-startup-errors"></a>Błędy uruchamiania aplikacji

**502.5 niepowodzenie procesu**  
Proces roboczy kończy się niepowodzeniem. Nie zaczyna się aplikacja.

[Modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) próby uruchomienia procesu roboczego, ale nie została uruchomiona. Badanie w dzienniku zdarzeń aplikacji często pomaga rozwiązać tego rodzaju problemów. Dostęp do dziennika zostało wyjaśnione w [dziennik zdarzeń aplikacji](#application-event-log) sekcji.

*502.5 niepowodzenia procesu* strony błędu jest zwracany, jeśli niepoprawnie skonfigurowany aplikacji powoduje, że proces roboczy nie powiedzie się:

![Okno przeglądarki, przedstawiający 502.5 stronę niepowodzenia procesu](troubleshoot/_static/process-failure-page.png)

**500 Wewnętrzny błąd serwera**  
Uruchamia aplikację, ale błąd uniemożliwia spełnienie żądania przez serwer.

Ten błąd występuje w kodzie aplikacji, podczas uruchamiania lub podczas tworzenia odpowiedzi. Odpowiedź może zawierać żadnej zawartości lub odpowiedzi może być wyświetlana jako *500 Wewnętrzny błąd serwera* w przeglądarce. W dzienniku zdarzeń aplikacji stwierdza, zwykle uruchomiona aplikacja. Z perspektywy serwera, który jest poprawna. Aplikacja została uruchomiona, ale nie może wygenerować prawidłowej odpowiedzi. [Uruchamianie aplikacji w konsoli Kudu](#run-the-app-in-the-kudu-console) lub [Włącz dziennik stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log) do rozwiązania problemu.

**Resetowanie połączenia**

Jeśli błąd wystąpi po nagłówki są wysyłane, jest za późno serwera wysłać **500 Wewnętrzny błąd serwera** po wystąpieniu błędu. Dzieje się tak często, gdy wystąpi błąd podczas serializacji obiektów złożonych na odpowiedź. Tego typu błędu jest wyświetlany jako *resetowania połączenia* błąd na komputerze klienckim. [Rejestrowanie aplikacji](xref:fundamentals/logging/index) mogą pomóc rozwiązać tego rodzaju błędów.

## <a name="default-startup-limits"></a>Domyślne limity uruchamiania

Modułu ASP.NET Core jest skonfigurowany z domyślną *startupTimeLimit* 120 sekund. Gdy pozostawić wartość domyślną, aplikacja może potrwać do dwóch minut przed moduł dzienniki awarii procesu. Aby uzyskać informacje na temat konfigurowania modułu, zobacz [atrybuty elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Rozwiązywanie problemów z błędami uruchamiania aplikacji

### <a name="application-event-log"></a>Dziennik zdarzeń aplikacji

Aby uzyskać dostęp do dziennika zdarzeń aplikacji, należy użyć **diagnozowanie i rozwiązywanie problemów** bloku w witrynie Azure portal:

1. W witrynie Azure portal Otwórz blok aplikacji w **App Services** bloku.
1. Wybierz **diagnozowanie i rozwiązywanie problemów** bloku.
1. W obszarze **wybierz kategorię problemu**, wybierz opcję **niedziałającej aplikacji internetowej** przycisku.
1. W obszarze **sugerowane rozwiązania**, otwórz okienko **otwórz dzienniki zdarzeń aplikacji**. Wybierz **otwórz dzienniki zdarzeń aplikacji** przycisku.
1. Zapoznaj się z błędem najnowsze dostarczone przez *IIS AspNetCoreModule* w **źródła** kolumny.

Alternatywa dla użycia **diagnozowanie i rozwiązywanie problemów** bloku jest zbadanie pliku dziennika zdarzeń aplikacji bezpośrednio za pomocą [Kudu](https://github.com/projectkudu/kudu/wiki):

1. Wybierz **Narzędzia zaawansowane** bloku **narzędzia PROGRAMISTYCZNE** obszaru. Wybierz **Przejdź&rarr;**  przycisku. W nowej karcie przeglądarki lub w oknie zostanie otwarta konsola Kudu.
1. Za pomocą paska nawigacji u góry strony, otwórz **konsoli debugowania** i wybierz **CMD**.
1. Otwórz **LogFiles** folderu.
1. Wybierz ikonę ołówka obok *eventlog.xml* pliku.
1. Przeanalizuj dziennik. Przewiń do dołu najnowszych zdarzeń w dzienniku.

### <a name="run-the-app-in-the-kudu-console"></a>Uruchamianie aplikacji w konsoli programu Kudu

Wiele błędów uruchamiania przestaną generować przydatne informacje w dzienniku zdarzeń aplikacji. Aplikację można uruchomić [Kudu](https://github.com/projectkudu/kudu/wiki) konsoli zdalnej wykonywania, aby dowiedzieć się, błąd:

1. Wybierz **Narzędzia zaawansowane** bloku **narzędzia PROGRAMISTYCZNE** obszaru. Wybierz **Przejdź&rarr;**  przycisku. W nowej karcie przeglądarki lub w oknie zostanie otwarta konsola Kudu.
1. Za pomocą paska nawigacji u góry strony, otwórz **konsoli debugowania** i wybierz **CMD**.
1. Otwieranie folderów w ścieżce **witryny** > **wwwroot**.
1. W konsoli należy uruchomić tę aplikację, wykonując zestawu aplikacji.
   * Jeśli aplikacja jest [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd), uruchom zestaw aplikacji za pomocą *dotnet.exe*. W poniższym poleceniu zastąp nazwę zestawu aplikacji dla `<assembly_name>`: `dotnet .\<assembly_name>.dll`
   * Jeśli aplikacja jest [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd)Uruchom pliku wykonywalnym aplikacji. W poniższym poleceniu zastąp nazwę zestawu aplikacji dla `<assembly_name>`: `<assembly_name>.exe`
1. Dane wyjściowe z aplikacji, przedstawiający wszystkie błędy z konsoli jest przekazywany w potoku do konsoli Kudu.

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

**Ważne!** Wyłącz rejestrowanie strumienia stdout, po zakończeniu rozwiązywania problemów.

1. W Kudu **konsoli diagnostyki**, wróć do ścieżki **witryny** > **wwwroot** aby ujawnić *web.config* pliku. Otwórz **web.config** plik ponownie, wybierając ikonę ołówka.
1. Ustaw **stdoutLogEnabled** do `false`.
1. Wybierz **Zapisz** można zapisać pliku.

> [!WARNING]
> Nie można wyłączyć dziennika stdout może prowadzić do awarii aplikacji lub serwera. Brak brak limitu rozmiaru pliku dziennika lub liczba pliki dziennika utworzone. Należy używać tylko stdout, rejestrowanie, aby rozwiązać problemy z uruchamianiem aplikacji.
>
> Ogólne logujesz się w aplikacji ASP.NET Core po uruchomieniu, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje. Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="common-startup-errors"></a>Typowe błędy uruchamiania 

Zobacz [dokumentacja typowych błędów platformy ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference). Najbardziej typowe problemy, które uniemożliwiają uruchamianie aplikacji znajdują się w temacie odwołania.

## <a name="slow-or-hanging-app"></a>Wolne lub zwisa aplikacji

Gdy aplikacja będzie odpowiadać wolno lub zawiesza się na żądanie, zobacz [problemów z wydajnością aplikacji sieci web powolne Rozwiązywanie problemów w usłudze Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation) wskazówki dotyczące debugowania.

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

**Ważne!** Pamiętaj wyłączyć rejestrowanie strumienia stdout, po zakończeniu rozwiązywania problemów. Zapoznaj się z instrukcjami w [dziennika stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log) sekcji.

Aby wyświetlić dzienniki śledzenia nieudanych żądań (Dzienniki FREB):

1. Przejdź do **diagnozowanie i rozwiązywanie problemów** bloku w witrynie Azure portal.
1. Wybierz **nie powiodło się dzienniki śledzenia żądań** z **SUPPORT TOOLS** obszar na pasku bocznym.

Zobacz [żądania zakończone niepowodzeniem śledzi sekcji Włącz rejestrowanie diagnostyki dla aplikacji sieci web w usłudze Azure App Service temacie](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) i [wydajność aplikacji — często zadawane pytania dla aplikacji sieci Web na platformie Azure: jak włączyć śledzenie nieudanych żądań?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) Aby uzyskać więcej informacji.

Aby uzyskać więcej informacji, zobacz [Włączanie rejestrowania diagnostycznego dla aplikacji sieci web w usłudze Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).

> [!WARNING]
> Nie można wyłączyć dziennika stdout może prowadzić do awarii aplikacji lub serwera. Brak brak limitu rozmiaru pliku dziennika lub liczba pliki dziennika utworzone.
>
> Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje. Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wprowadzenie do obsługi błędów w platformy ASP.NET Core](xref:fundamentals/error-handling)
* [Dokumentacja typowych błędów dla usługi Azure App Service i IIS za pomocą programu ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Rozwiązywanie problemów z aplikacją sieci web w usłudze Azure App Service przy użyciu programu Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Rozwiązywanie problemów z błędami HTTP "502 — Zła brama" i "503 Usługa niedostępna" w aplikacjach sieci web platformy Azure](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [Powolne sieci web app Rozwiązywanie problemów z wydajnością w usłudze Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Wydajność aplikacji — często zadawane pytania dla aplikacji sieci Web na platformie Azure](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Azure piaskownica aplikacji sieci Web (ograniczenia wykonywania środowiska uruchomieniowego usługi App Service)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [Azure Friday: Diagnostyki usługi aplikacji Azure i środowisko rozwiązywania problemów (12-minutowy klip wideo)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
