---
title: Rozwiązywanie problemów z platformy ASP.NET Core w usłudze aplikacji Azure
author: guardrex
description: Dowiedz się, jak diagnozować problemy z wdrożeniami platformy ASP.NET Core usłudze Azure App Service.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/troubleshoot
ms.openlocfilehash: 47056c80c7abf5dd5ad5ae96af7b821d31b21b8b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service"></a>Rozwiązywanie problemów z platformy ASP.NET Core w usłudze aplikacji Azure

Przez [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

Ten artykuł zawiera instrukcje na temat diagnozowania platformy ASP.NET Core problem uruchamiania aplikacji za pomocą narzędzi diagnostycznych w usłudze Azure App Service. Aby uzyskać dodatkowe porady dotyczące rozwiązywania problemów, zobacz [Omówienie diagnostyki Azure App Service](/azure/app-service/app-service-diagnostics) i [porady: monitorowanie aplikacji w usłudze Azure App Service](/azure/app-service/web-sites-monitor) w dokumentacji platformy Azure.

## <a name="app-startup-errors"></a>Błędy uruchamiania aplikacji

**502.5 niepowodzenie procesu**  
Proces roboczy kończy się niepowodzeniem. Nie uruchamia aplikację.

[Moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) próbuje uruchomić proces roboczy, ale nie powiedzie się. Badanie często w dzienniku zdarzeń aplikacji pomaga w rozwiązywaniu problemów tego typu. Dostęp do dziennika zostało wyjaśnione w dokumencie [w dzienniku zdarzeń aplikacji](#application-event-log) sekcji.

*502.5 awarii procesu* strony błędu jest zwracany, jeśli aplikacja nieprawidłowo powoduje niepowodzenie procesu roboczego:

![Niepowodzenie procesu wyświetlana 502.5 w oknie przeglądarki](troubleshoot/_static/process-failure-page.png)

**500 Wewnętrzny błąd serwera**  
Uruchamia aplikację, ale wystąpił błąd uniemożliwia spełnienie żądania przez serwer.

Ten błąd występuje w ciągu kodu aplikacji, podczas uruchamiania lub podczas tworzenia odpowiedzi. Odpowiedź nie może zawierać żadnej zawartości lub odpowiedzi może być wyświetlana jako *500 Wewnętrzny błąd serwera* w przeglądarce. Dziennik zdarzeń aplikacji określają, zwykle uruchomić aplikacji. Z perspektywy serwera, który jest poprawna. Aplikacja została uruchomiona, ale nie można wygenerować poprawnej odpowiedzi. [Uruchamianie aplikacji w konsoli Kudu](#run-the-app-in-the-kudu-console) lub [Włącz dziennik stdout moduł platformy ASP.NET Core](#aspnet-core-module-stdout-log) do rozwiązania problemu.

**Resetowania połączenia**

Jeśli błąd wystąpi po wysłaniu nagłówków, jest za późno na serwerze wysłać **500 Wewnętrzny błąd serwera** po wystąpieniu błędu. Dzieje się tak często, gdy wystąpi błąd podczas serializacji obiektów złożonych na odpowiedź. Tego typu błędu jest wyświetlany jako *resetowania połączenia* błąd na komputerze klienckim. [Rejestrowanie aplikacji](xref:fundamentals/logging/index) ułatwiają rozwiązywanie problemów z tych typów błędów.

## <a name="default-startup-limits"></a>Domyślne limity uruchamiania

Moduł platformy ASP.NET Core jest skonfigurowany z domyślną *startupTimeLimit* 120 sekund. Po lewej, wartość domyślną, aplikacja może zająć do dwóch minut przed modułu dzienniki awarii procesu. Aby uzyskać informacje na temat konfigurowania modułu, zobacz [atrybuty elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Rozwiązywanie problemów z uruchamianiem aplikacji

### <a name="application-event-log"></a>Dziennik zdarzeń aplikacji

Aby uzyskać dostęp do dziennika zdarzeń aplikacji, należy użyć **diagnozowanie i rozwiązywanie problemów** bloku w portalu Azure:

1. W portalu Azure, otwórz blok aplikacji w **usługi aplikacji** bloku.
1. Wybierz **diagnozowanie i rozwiązywanie problemów** bloku.
1. W obszarze **wybierz kategorię problemu**, wybierz pozycję **aplikacji sieci Web w dół** przycisku.
1. W obszarze **sugerowane rozwiązania**, Otwórz w okienku **otwórz dzienniki zdarzeń aplikacji**. Wybierz **otwórz dzienniki zdarzeń aplikacji** przycisku.
1. Zapoznaj się z błędem najnowsze dostarczonych przez *IIS AspNetCoreModule* w **źródła** kolumny.

Zamiast **diagnozowanie i rozwiązywanie problemów** ma zapoznaj się z plikiem dziennika zdarzeń aplikacji bezpośrednio za pomocą bloku [Kudu](https://github.com/projectkudu/kudu/wiki):

1. Wybierz **zaawansowane narzędzia** bloku w **narzędzi PROGRAMISTYCZNYCH** obszaru. Wybierz **Przejdź&rarr;**  przycisku. W nowej karty przeglądarki lub w oknie zostanie otwarta konsola Kudu.
1. Za pomocą paska nawigacji w górnej części strony, otwórz **konsoli debugowania** i wybierz **CMD**.
1. Otwórz **LogFiles** folderu.
1. Wybierz ikonę ołówka obok *eventlog.xml* pliku.
1. Sprawdź w dzienniku. Przewiń w dół najnowszych zdarzeń w dzienniku.

### <a name="run-the-app-in-the-kudu-console"></a>Uruchamianie aplikacji w konsoli Kudu

Wiele błędów uruchamiania nie dają przydatnych informacji w dzienniku zdarzeń aplikacji. Aplikację można uruchomić [Kudu](https://github.com/projectkudu/kudu/wiki) konsoli zdalnej wykonywania, aby dowiedzieć się, kod błędu:

1. Wybierz **zaawansowane narzędzia** bloku w **narzędzi PROGRAMISTYCZNYCH** obszaru. Wybierz **Przejdź&rarr;**  przycisku. W nowej karty przeglądarki lub w oknie zostanie otwarta konsola Kudu.
1. Za pomocą paska nawigacji w górnej części strony, otwórz **konsoli debugowania** i wybierz **CMD**.
1. Otwórz foldery do ścieżki **lokacji** > **wwwroot**.
1. W konsoli Uruchom aplikację, wykonując zestawu aplikacji.
   * Jeśli aplikacja jest [wdrożenia zależne od framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd), uruchom zestaw aplikacji z *dotnet.exe*. W poniższym poleceniu zastąp nazwę zestawu aplikacji dla `<assembly_name>`: `dotnet .\<assembly_name>.dll`
   * Jeśli aplikacja jest [niezależne wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd)Uruchom aplikację do pliku wykonywalnego. W poniższym poleceniu zastąp nazwę zestawu aplikacji dla `<assembly_name>`: `<assembly_name>.exe`
1. Dane wyjściowe z aplikacji, przedstawiający wszelkie błędy z konsoli jest przekazywany w potoku do konsoli Kudu.

### <a name="aspnet-core-module-stdout-log"></a>Program ASP.NET Core modułu stdout dziennika

Moduł platformy ASP.NET Core dziennika stdout rejestruje często przydatne komunikaty o błędach nie można odnaleźć w dzienniku zdarzeń aplikacji. Aby włączyć i sprawdź dzienniki stdout:

1. Przejdź do **diagnozowanie i rozwiązywanie problemów** bloku w portalu Azure.
1. W obszarze **wybierz kategorię problemu**, wybierz pozycję **aplikacji sieci Web w dół** przycisku.
1. W obszarze **sugerowane rozwiązania** > **Włącz przekierowanie dziennika Stdout**, kliknij przycisk, aby **Otwórz konsolę Kudu do edycji pliku Web.Config**.
1. W Kudu **konsoli diagnostyki**, należy otworzyć foldery do ścieżki **lokacji** > **wwwroot**. Przewiń w dół do ujawniania *web.config* pliku w dolnej części listy.
1. Kliknij ikonę ołówka *web.config* pliku.
1. Ustaw **stdoutLogEnabled** do `true` i zmienić **stdoutLogFile** ścieżkę do: `\\?\%home%\LogFiles\stdout`.
1. Wybierz **zapisać** zapisać zaktualizowanego *web.config* pliku.
1. Wyślij żądanie do aplikacji.
1. Wróć do portalu Azure. Wybierz **zaawansowane narzędzia** bloku w **narzędzi PROGRAMISTYCZNYCH** obszaru. Wybierz **Przejdź&rarr;**  przycisku. W nowej karty przeglądarki lub w oknie zostanie otwarta konsola Kudu.
1. Za pomocą paska nawigacji w górnej części strony, otwórz **konsoli debugowania** i wybierz **CMD**.
1. Wybierz **LogFiles** folderu.
1. Sprawdź **zmodyfikowane** kolumny i wybierz ikonę ołówka do edycji obiektu stdout logowania daty ostatniej modyfikacji.
1. Po otwarciu pliku dziennika został wyświetlony błąd.

**Ważne!** Wyłącz rejestrowanie stdout po zakończeniu rozwiązywania problemów.

1. W Kudu **konsoli diagnostyki**, zwróć się do ścieżki **lokacji** > **wwwroot** ujawniasz *web.config* pliku. Otwórz **web.config** plik ponownie, wybierając ikonę ołówka.
1. Ustaw **stdoutLogEnabled** do `false`.
1. Wybierz **zapisać** można zapisać pliku.

> [!WARNING]
> Aby wyłączyć dziennik stdout może doprowadzić do awarii aplikacji lub serwera. Brak limitu rozmiaru pliku dziennika lub liczba pliki dziennika utworzone. Należy używać tylko stdout rejestrowania rozwiązywać problemy z uruchamianiem aplikacji.
>
> Ogólne rejestrowanie w aplikacji platformy ASP.NET Core po uruchomieniu, użyj biblioteki rejestrowania, który ogranicza rozmiar pliku dziennika i dzienników obraca. Aby uzyskać więcej informacji, zobacz [dostawców innych firm rejestrowania](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="common-startup-errors"></a>Typowe błędy uruchamiania 

Zobacz [częsta błędów platformy ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference). Większość typowych problemów, które uniemożliwiają uruchamianie aplikacji znajdują się w temacie.

## <a name="slow-or-hanging-app"></a>Wolne lub zwisa aplikacji

Gdy aplikacja reaguje powoli lub zawiesza się na żądanie, zobacz [rozwiązywanie powolne web app problemy z wydajnością w usłudze Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation) do debugowania wskazówki.

## <a name="remote-debugging"></a>Debugowanie zdalne

Zobacz następujące tematy:

* [Zdalne debugowanie aplikacji sieci web części Rozwiązywanie problemów aplikacji sieci web w usłudze Azure App Service przy użyciu programu Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) (dokumentacji platformy Azure)
* [Zdalne debugowanie platformy ASP.NET Core w usługach IIS na platformie Azure w programie Visual Studio 2017](/visualstudio/debugger/remote-debugging-azure) (Dokumentacja programu Visual Studio)

## <a name="application-insights"></a>Application Insights

[Usługa Application Insights](https://azure.microsoft.com/services/application-insights/) zawiera dane telemetryczne z aplikacji hostowanych w usłudze Azure App Service, w tym błąd rejestrowania i funkcje raportowania. Usługi Application Insights można tylko raporty dotyczące błędów występujących po uruchomieniu aplikacji funkcji rejestrowania aplikacji stają się dostępne. Aby uzyskać więcej informacji, zobacz [usługi Application Insights dla platformy ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="monitoring-blades"></a>Monitorowanie bloków

Bloki monitorowania alternatywną metodą Rozwiązywanie problemów do metod opisanych wcześniej w temacie. Te bloki umożliwia diagnozowanie błędów 500 serii.

Upewnij się, czy są zainstalowane rozszerzenia platformy ASP.NET Core. Jeśli nie są zainstalowane rozszerzenia, zainstaluj je ręcznie:

1. W **narzędzi PROGRAMISTYCZNYCH** sekcji bloku, wybierz opcję **rozszerzenia** bloku.
1. **Platformy ASP.NET Core rozszerzenia** powinien zostać wyświetlony na liście.
1. Jeśli nie są zainstalowane rozszerzenia, wybierz **Dodaj** przycisku.
1. Wybierz **platformy ASP.NET Core rozszerzenia** z listy.
1. Wybierz **OK** zaakceptować postanowienia prawne.
1. Wybierz **OK** na **dodanie rozszerzenia** bloku.
1. Komunikat informacyjny wyskakujących wskazuje, jeśli rozszerzenia są pomyślnie zainstalowane.

Jeśli rejestrowanie stdout nie jest włączone, wykonaj następujące kroki:

1. W portalu Azure wybierz **zaawansowane narzędzia** bloku w **narzędzi PROGRAMISTYCZNYCH** obszaru. Wybierz **Przejdź&rarr;**  przycisku. W nowej karty przeglądarki lub w oknie zostanie otwarta konsola Kudu.
1. Za pomocą paska nawigacji w górnej części strony, otwórz **konsoli debugowania** i wybierz **CMD**.
1. Otwórz foldery do ścieżki **lokacji** > **wwwroot** i przewiń w dół do ujawniania *web.config* pliku w dolnej części listy.
1. Kliknij ikonę ołówka *web.config* pliku.
1. Ustaw **stdoutLogEnabled** do `true` i zmienić **stdoutLogFile** ścieżkę do: `\\?\%home%\LogFiles\stdout`.
1. Wybierz **zapisać** zapisać zaktualizowanego *web.config* pliku.

Przejdź do aktywowania rejestrowania diagnostycznego:

1. W portalu Azure wybierz **dzienników diagnostycznych** bloku.
1. Wybierz **na** przełączać **rejestrowania aplikacji (systemu plików)** i **szczegółowe komunikaty o błędach**. Wybierz **zapisać** u góry bloku.
1. Aby dołączyć dane śledzenia nieudanych żądań, rejestrowanie nie powiodło się żądanie zdarzenia buforowania (FREB), nazywany również zaznaczyć **na** przełączać **śledzenie nieudanych żądań**. 
1. Wybierz **strumienia dziennika** bloku znajduje się bezpośrednio pod **dzienników diagnostycznych** bloku w portalu.
1. Wyślij żądanie do aplikacji.
1. W dzienniku transmisji danych wskazuje przyczynę błędu.

**Ważne!** Pamiętaj wyłączyć rejestrowanie stdout po zakończeniu rozwiązywania problemów. Zapoznaj się z instrukcjami w [dziennika stdout moduł platformy ASP.NET Core](#aspnet-core-module-stdout-log) sekcji.

Aby wyświetlić dzienniki śledzenia nieudanych żądań (Dzienniki FREB):

1. Przejdź do **diagnozowanie i rozwiązywanie problemów** bloku w portalu Azure.
1. Wybierz **nie powiodło się dzienników śledzenia żądań** z **narzędzia obsługi** obszar paska bocznego.

Zobacz [żądania śledzi sekcji Włączanie rejestrowania diagnostyki dla aplikacji sieci web w usłudze Azure App Service temacie](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) i [wydajność aplikacji — często zadawane pytania dotyczące aplikacji sieci Web na platformie Azure: jak włączyć śledzenie nieudanych żądań?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) Aby uzyskać więcej informacji.

Aby uzyskać więcej informacji, zobacz [Włączanie rejestrowania diagnostyki dla aplikacji sieci web w usłudze Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).

> [!WARNING]
> Aby wyłączyć dziennik stdout może doprowadzić do awarii aplikacji lub serwera. Brak limitu rozmiaru pliku dziennika lub liczba pliki dziennika utworzone.
>
> Użyć biblioteki rejestrowania, który ogranicza rozmiar pliku dziennika i dzienników obraca rutynowych rejestrowania w aplikacji platformy ASP.NET Core. Aby uzyskać więcej informacji, zobacz [dostawców innych firm rejestrowania](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wprowadzenie do obsługi błędów w platformy ASP.NET Core](xref:fundamentals/error-handling)
* [Typowe błędy odwołania dla usługi Azure App Service i IIS z platformy ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Rozwiązywanie problemów z aplikacji sieci web w usłudze Azure App Service przy użyciu programu Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Rozwiązywanie błędów HTTP "502 Niewłaściwa brama" i "503 Usługa niedostępna" w aplikacjach sieci web platformy Azure](/app-service/app-service-web-troubleshoot-http-502-http-503)
* [Powolne web app Rozwiązywanie problemów z wydajnością w usłudze Azure App Service](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [Wydajność aplikacji — często zadawane pytania dotyczące aplikacji sieci Web na platformie Azure](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [Piaskownica aplikacji sieci Web platformy Azure (ograniczenia wykonywania środowiska uruchomieniowego usługi App Service)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [Piątek z Azure: Diagnostycznych usługi aplikacji Azure i rozwiązywanie problemów z doświadczenia (12-minutowy film wideo)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
