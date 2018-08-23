---
title: Rozwiązywanie problemów z platformą ASP.NET Core w usługach IIS
author: guardrex
description: Dowiedz się, jak diagnozować problemy z wdrożeniami usług Internet Information Services (IIS) w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2018
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: f22914c9b0d6d1902dd37c9b21b80a18894c97e7
ms.sourcegitcommit: d1c4580f56656b503cf528ec9f5ba570d790b57d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "41752256"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Rozwiązywanie problemów z platformą ASP.NET Core w usługach IIS

Przez [Luke Latham](https://github.com/guardrex)

Ten artykuł zawiera instrukcje na temat platformy ASP.NET Core zdiagnozować problem uruchamiania aplikacji w przypadku hostowania za pomocą [Internet Information Services (IIS)](/iis). Informacje przedstawione w tym artykule mają zastosowanie do hostowania w usługach IIS w systemie Windows Server i Windows Desktop.

W programie Visual Studio ma domyślnie wartość projektu ASP.NET Core [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hostingu podczas debugowania. A *502.5 niepowodzenia procesu* występuje, gdy debugowanie lokalne może być troubleshooted przy użyciu porady w tym temacie.

Dodatkowe tematy dotyczące rozwiązywania problemów:

[Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)  
Mimo że usługa App Service używa [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) i usługi IIS do hostowania aplikacji, zobacz temat dedykowanych instrukcje specyficzne dla usługi App Service.

[Obsługa błędów](xref:fundamentals/error-handling)  
Dowiedz się, jak do obsługi błędów w aplikacji platformy ASP.NET Core podczas programowania w systemie lokalnym.

[Naucz się debugować przy użyciu programu Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger)  
W tym temacie przedstawiono funkcje debugera programu Visual Studio.

[Debugowanie za pomocą programu Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging)  
Więcej informacji na temat debugowania wbudowaną w programie Visual Studio Code.

## <a name="app-startup-errors"></a>Błędy uruchamiania aplikacji

**502.5 niepowodzenie procesu**  
Proces roboczy kończy się niepowodzeniem. Nie zaczyna się aplikacja.

Modułu ASP.NET Core podejmowana jest próba uruchomienia procesu roboczego, ale nie została uruchomiona. Zazwyczaj można ustalić przyczyny niepowodzenia uruchamiania procesu na podstawie wpisów w [dziennik zdarzeń aplikacji](#application-event-log) i [dziennika stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log).

*502.5 niepowodzenia procesu* strony błędu jest zwracany, jeśli aplikacji lub obsługującego błędnej konfiguracji powoduje niepowodzenie procesu roboczego:

![Okno przeglądarki, przedstawiający 502.5 stronę niepowodzenia procesu](troubleshoot/_static/process-failure-page.png)

**500 Wewnętrzny błąd serwera**  
Uruchamia aplikację, ale błąd uniemożliwia spełnienie żądania przez serwer.

Ten błąd występuje w kodzie aplikacji, podczas uruchamiania lub podczas tworzenia odpowiedzi. Odpowiedź może zawierać żadnej zawartości lub odpowiedzi może być wyświetlana jako *500 Wewnętrzny błąd serwera* w przeglądarce. W dzienniku zdarzeń aplikacji stwierdza, zwykle uruchomiona aplikacja. Z perspektywy serwera, który jest poprawna. Aplikacja została uruchomiona, ale nie może wygenerować prawidłowej odpowiedzi. [Uruchamianie aplikacji w wierszu polecenia](#run-the-app-at-a-command-prompt) na serwerze lub [Włącz dziennik stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log) do rozwiązania problemu.

**Resetowanie połączenia**

Jeśli błąd wystąpi po nagłówki są wysyłane, jest za późno serwera wysłać **500 Wewnętrzny błąd serwera** po wystąpieniu błędu. Dzieje się tak często, gdy wystąpi błąd podczas serializacji obiektów złożonych na odpowiedź. Tego typu błędu jest wyświetlany jako *resetowania połączenia* błąd na komputerze klienckim. [Rejestrowanie aplikacji](xref:fundamentals/logging/index) mogą pomóc rozwiązać tego rodzaju błędów.

## <a name="default-startup-limits"></a>Domyślne limity uruchamiania

Modułu ASP.NET Core jest skonfigurowany z domyślną *startupTimeLimit* 120 sekund. Gdy pozostawić wartość domyślną, aplikacja może potrwać do dwóch minut przed moduł dzienniki awarii procesu. Aby uzyskać informacje na temat konfigurowania modułu, zobacz [atrybuty elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Rozwiązywanie problemów z błędami uruchamiania aplikacji

### <a name="application-event-log"></a>Dziennik zdarzeń aplikacji

Dostęp do dziennika zdarzeń aplikacji:

1. Otwieranie Start menu, wyszukaj **Podgląd zdarzeń**, a następnie wybierz pozycję **Podgląd zdarzeń** aplikacji.
1. W **Podgląd zdarzeń**, otwórz **Dzienniki Windows** węzła.
1. Wybierz **aplikacji** można otworzyć dziennika zdarzeń aplikacji.
1. Wyszukaj błędy skojarzone z aplikacją się niepowodzeniem. Błędy mają wartość *moduł AspNetCore IIS* lub *usług IIS Express AspNetCore modułu* w *źródła* kolumny.

### <a name="run-the-app-at-a-command-prompt"></a>Uruchamianie aplikacji w wierszu polecenia

Wiele błędów uruchamiania przestaną generować przydatne informacje w dzienniku zdarzeń aplikacji. Przyczyną niektórych błędów można znaleźć, uruchamiając aplikację w wierszu polecenia w systemie hostingu.

**Wdrożenie zależny od struktury**

Jeśli aplikacja jest [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

1. W wierszu polecenia przejdź do folderu wdrożenia i uruchomienia aplikacji, wykonując zestaw aplikacji za pomocą *dotnet.exe*. W poniższym poleceniu zastąp nazwę zestawu aplikacji dla \<assembly_name >: `dotnet .\<assembly_name>.dll`.
1. Dane wyjściowe z aplikacji, przedstawiający wszystkie błędy z konsoli są zapisywane w oknie konsoli.
1. Jeśli wystąpią błędy, gdy kieruje żądanie do aplikacji, należy wysłać żądanie do hosta i portu, na którym nasłuchuje Kestrel. Przy użyciu domyślnego hosta i post, zgłosić wniosek o `http://localhost:5000/`. Jeśli aplikacja reaguje, zwykle pod adresem punktu końcowego Kestrel, problem najprawdopodobniej związanych z konfiguracją zwrotny serwer proxy i mniej prawdopodobne w aplikacji.

**Niezależne wdrożenia**

Jeśli aplikacja jest [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd):

1. W wierszu polecenia przejdź do folderu wdrożenia i uruchomienia pliku wykonywalnego aplikacji. W poniższym poleceniu zastąp nazwę zestawu aplikacji dla \<assembly_name >: `<assembly_name>.exe`.
1. Dane wyjściowe z aplikacji, przedstawiający wszystkie błędy z konsoli są zapisywane w oknie konsoli.
1. Jeśli wystąpią błędy, gdy kieruje żądanie do aplikacji, należy wysłać żądanie do hosta i portu, na którym nasłuchuje Kestrel. Przy użyciu domyślnego hosta i post, zgłosić wniosek o `http://localhost:5000/`. Jeśli aplikacja reaguje, zwykle pod adresem punktu końcowego Kestrel, problem najprawdopodobniej związanych z konfiguracją zwrotny serwer proxy i mniej prawdopodobne w aplikacji.

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

**Ważne!** Wyłącz rejestrowanie strumienia stdout, po zakończeniu rozwiązywania problemów.

1. Edytuj *web.config* pliku.
1. Ustaw **stdoutLogEnabled** do `false`.
1. Zapisz plik.

> [!WARNING]
> Nie można wyłączyć dziennika stdout może prowadzić do awarii aplikacji lub serwera. Brak brak limitu rozmiaru pliku dziennika lub liczba pliki dziennika utworzone.
>
> Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje. Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="enabling-the-developer-exception-page"></a>Włączanie na stronie wyjątków dla deweloperów

`ASPNETCORE_ENVIRONMENT` [Zmiennej środowiskowej, można dodać do pliku web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) do uruchomienia aplikacji w środowisku programistycznym. Tak długo, jak środowisko nie jest zastąpione przy uruchamianiu aplikacji przez `UseEnvironment` umożliwia ustawienie zmiennej środowiskowej w Konstruktorze hosta [stronie wyjątków deweloperów](xref:fundamentals/error-handling) się pojawiać po uruchomieniu aplikacji.

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

Ustawienie zmiennej środowiskowej, aby uzyskać `ASPNETCORE_ENVIRONMENT` jest zalecane tylko dla używane w przejściowym i testowania serwerów, które nie są połączone z Internetem. Usuń zmienną środowiskową z *web.config* plik po rozwiązywania problemów. Aby uzyskać informacje na temat ustawiania zmiennych środowiskowych *web.config*, zobacz [environmentVariables element podrzędny elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

## <a name="common-startup-errors"></a>Typowe błędy uruchamiania 

Zobacz [dokumentacja typowych błędów platformy ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference). Najbardziej typowe problemy, które uniemożliwiają uruchamianie aplikacji znajdują się w temacie odwołania.

## <a name="slow-or-hanging-app"></a>Wolne lub zwisa aplikacji

Gdy aplikacja będzie odpowiadać wolno lub zawiesza się na żądanie, Uzyskaj i analizowanie [plik zrzutu](/visualstudio/debugger/using-dump-files). Pliki zrzutu można uzyskać przy użyciu dowolnej z następujących narzędzi:

* [ProcDump](/sysinternals/downloads/procdump)
* [DebugDiag](https://www.microsoft.com/download/details.aspx?id=49924)
* WinDbg: [Pobierz narzędzia debugowania dla Windows](https://developer.microsoft.com/windows/hardware/download-windbg), [debugowania za pomocą narzędzia WinDbg](/windows-hardware/drivers/debugger/debugging-using-windbg)

## <a name="remote-debugging"></a>Debugowanie zdalne

Zobacz [zdalne debugowanie platformy ASP.NET Core na komputerze zdalnym usług IIS w programie Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) w dokumentacji programu Visual Studio.

## <a name="application-insights"></a>Application Insights

[Usługa Application Insights](/azure/application-insights/) udostępnia dane telemetryczne z aplikacji hostowanych przez usługi IIS, w tym rejestrowanie i funkcji raportowania błędów. Usługa Application Insights można tylko raporty dotyczące błędów występujących po uruchomieniu aplikacji, gdy staną się dostępne funkcje rejestrowania aplikacji. Aby uzyskać więcej informacji, zobacz [usługi Application Insights dla platformy ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="additional-troubleshooting-advice"></a>Dodatkowe porady dotyczące rozwiązywania problemów

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

* [Wprowadzenie do obsługi błędów w platformy ASP.NET Core](xref:fundamentals/error-handling)
* [Dokumentacja typowych błędów dla usługi Azure App Service i IIS za pomocą programu ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Odwołania do konfiguracji modułu platformy ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)
