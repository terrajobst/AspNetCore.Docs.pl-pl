---
title: "Rozwiązywanie problemów z platformy ASP.NET Core w usługach IIS"
author: guardrex
description: "Dowiedz się, jak diagnozować problemy z wdrożeniami usług Internet Information Services (IIS) w aplikacji platformy ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 65173e0101a17c64f4cde583e5bbb9fb0a9c7718
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/11/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Rozwiązywanie problemów z platformy ASP.NET Core w usługach IIS

Przez [Luke Latham](https://github.com/guardrex)

Ten artykuł zawiera instrukcje na temat platformy ASP.NET Core zdiagnozować problem uruchamiania aplikacji odnośnie do hostowania z [Internet Information Services (IIS)](/iis). Informacje przedstawione w tym artykule dotyczą hosting w usługach IIS w systemie Windows Server i Windows Desktop.

W programie Visual Studio, projekt platformy ASP.NET Core domyślnie [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hostingu podczas debugowania. A *502.5 awarii procesu* występuje, gdy debugowanie lokalnie może być troubleshooted przy użyciu zalecenia w tym temacie.

Dodatkowe tematy dotyczące rozwiązywania problemów:

[Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)  
Chociaż korzysta z usługi aplikacji [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) i usług IIS do aplikacji hosta, zobacz temat dedykowanych instrukcje specyficzne dla aplikacji usługi.

[Obsługa błędów](xref:fundamentals/error-handling)  
Wykryj sposób obsługi błędów w aplikacji platformy ASP.NET Core podczas tworzenia w systemie lokalnym.

[Naucz się debugować przy użyciu programu Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger)  
W tym temacie przedstawiono funkcje debugera programu Visual Studio.

## <a name="app-startup-errors"></a>Błędy uruchamiania aplikacji

**502.5 niepowodzenie procesu**  
Proces roboczy kończy się niepowodzeniem. Nie uruchamia aplikację.

Moduł platformy ASP.NET Core próbuje uruchomić proces roboczy, ale nie powiedzie się uruchomienie. Zazwyczaj można ustalić przyczyny niepowodzenia uruchomienia procesu na podstawie wpisów w [w dzienniku zdarzeń aplikacji](#application-event-log) i [dziennika stdout moduł platformy ASP.NET Core](#aspnet-core-module-stdout-log).

*502.5 awarii procesu* strony błędu jest zwracany, gdy hosting aplikacji lub błędnej konfiguracji powoduje niepowodzenie procesu roboczego:

![Niepowodzenie procesu wyświetlana 502.5 w oknie przeglądarki](troubleshoot/_static/process-failure-page.png)

**500 Wewnętrzny błąd serwera**  
Uruchamia aplikację, ale wystąpił błąd uniemożliwia spełnienie żądania przez serwer.

Ten błąd występuje w ciągu kodu aplikacji, podczas uruchamiania lub podczas tworzenia odpowiedzi. Odpowiedź nie może zawierać żadnej zawartości lub odpowiedzi może być wyświetlana jako *500 Wewnętrzny błąd serwera* w przeglądarce. Dziennik zdarzeń aplikacji określają, zwykle uruchomić aplikacji. Z perspektywy serwera, który jest poprawna. Aplikacja została uruchomiona, ale nie można wygenerować poprawnej odpowiedzi. [Uruchom aplikację w wierszu polecenia](#run-the-app-at-a-command-prompt) na serwerze lub [Włącz dziennik stdout moduł platformy ASP.NET Core](#aspnet-core-module-stdout-log) do rozwiązania problemu.

**Resetowania połączenia**

Jeśli błąd wystąpi po wysłaniu nagłówków, jest za późno na serwerze wysłać **500 Wewnętrzny błąd serwera** po wystąpieniu błędu. Dzieje się tak często, gdy wystąpi błąd podczas serializacji obiektów złożonych na odpowiedź. Tego typu błędu jest wyświetlany jako *resetowania połączenia* błąd na komputerze klienckim. [Rejestrowanie aplikacji](xref:fundamentals/logging/index) ułatwiają rozwiązywanie problemów z tych typów błędów.

## <a name="default-startup-limits"></a>Domyślne limity uruchamiania

Moduł platformy ASP.NET Core jest skonfigurowany z domyślną *startupTimeLimit* 120 sekund. Po lewej, wartość domyślną, aplikacja może zająć do dwóch minut przed modułu dzienniki awarii procesu. Aby uzyskać informacje na temat konfigurowania modułu, zobacz [atrybuty elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Rozwiązywanie problemów z uruchamianiem aplikacji

### <a name="application-event-log"></a>Dziennik zdarzeń aplikacji

Dostęp do dziennika zdarzeń aplikacji:

1. Otwórz Start menu, wyszukaj **Podgląd zdarzeń**, a następnie wybierz **Podgląd zdarzeń** aplikacji.
1. W **Podgląd zdarzeń**, otwórz **dzienniki systemu Windows** węzła.
1. Wybierz **aplikacji** otworzyć dziennika zdarzeń aplikacji.
1. Wyszukaj błędy skojarzone z aplikacją się niepowodzeniem. Błędy mają wartość *moduł AspNetCore IIS* lub *usług IIS Express AspNetCore modułu* w *źródła* kolumny.

### <a name="run-the-app-at-a-command-prompt"></a>Uruchom aplikację w wierszu polecenia

Wiele błędów uruchamiania nie dają przydatnych informacji w dzienniku zdarzeń aplikacji. Możesz znaleźć przyczynę błędy przez uruchomienie aplikacji w wierszu polecenia przez system operacyjny.

**Zależne od Framework wdrożenia**

Jeśli aplikacja jest [wdrożenia zależne od framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

1. W wierszu polecenia przejdź do folderu wdrożenia i uruchamianie aplikacji, wykonując zestaw aplikacji z *dotnet.exe*. W poniższym poleceniu zastąp nazwę zestawu aplikacji dla \<nazwa_zestawu >: `dotnet .\<assembly_name>.dll`.
1. Dane wyjściowe z aplikacji, przedstawiający wszelkie błędy z konsoli są zapisywane w oknie konsoli.
1. Jeśli wystąpi błąd podczas tworzenia żądania do aplikacji, należy wysłać żądanie do hosta i portu, na którym nasłuchuje Kestrel. Przy użyciu domyślnego hosta i post, wykona żądanie `http://localhost:5000/`. Jeśli aplikacja zazwyczaj odpowiada pod adresem punktu końcowego Kestrel, problem jest bardziej prawdopodobne, związane z konfiguracją zwrotnego serwera proxy i mniej prawdopodobne w aplikacji.

**Samodzielne wdrożenia**

Jeśli aplikacja jest [niezależne wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd):

1. W wierszu polecenia przejdź do folderu wdrożenia i uruchom plik wykonywalny aplikacji. W poniższym poleceniu zastąp nazwę zestawu aplikacji dla \<nazwa_zestawu >: `<assembly_name>.exe`.
1. Dane wyjściowe z aplikacji, przedstawiający wszelkie błędy z konsoli są zapisywane w oknie konsoli.
1. Jeśli wystąpi błąd podczas tworzenia żądania do aplikacji, należy wysłać żądanie do hosta i portu, na którym nasłuchuje Kestrel. Przy użyciu domyślnego hosta i post, wykona żądanie `http://localhost:5000/`. Jeśli aplikacja zazwyczaj odpowiada pod adresem punktu końcowego Kestrel, problem jest bardziej prawdopodobne, związane z konfiguracją zwrotnego serwera proxy i mniej prawdopodobne w aplikacji.

### <a name="aspnet-core-module-stdout-log"></a>Program ASP.NET Core modułu stdout dziennika

Aby włączyć i sprawdź dzienniki stdout:

1. Przejdź do folderu wdrożenia dla lokacji przez system operacyjny.
1. Jeśli *dzienniki* folder nie istnieje, Utwórz folder. Aby uzyskać instrukcje dotyczące włączania MSBuild tworzenia *dzienniki* folderu we wdrożeniu automatycznie, zobacz [struktury katalogów](xref:host-and-deploy/directory-structure) tematu.
1. Edytuj *web.config* pliku. Ustaw **stdoutLogEnabled** do `true` i zmienić **stdoutLogFile** ścieżkę, aby wskazywał *dzienniki* folder (na przykład `.\logs\stdout`). `stdout`w ścieżce jest prefiks nazwy pliku dziennika. Sygnatura czasowa, identyfikator procesu i rozszerzenie pliku są dodawane automatycznie podczas tworzenia dziennika. Przy użyciu `stdout` jako prefiksu nazwy pliku, nosi nazwę pliku dziennika typowe *stdout_20180205184032_5412.log*. 
1. Zapisz zaktualizowane *web.config* pliku.
1. Wyślij żądanie do aplikacji.
1. Przejdź do *dzienniki* folderu. Znajdowanie i otwieranie najnowszych dziennika stdout.
1. Badanie w dzienniku błędów.

**Ważne!** Wyłącz rejestrowanie stdout po zakończeniu rozwiązywania problemów.

1. Edytuj *web.config* pliku.
1. Ustaw **stdoutLogEnabled** do `false`.
1. Zapisz plik.

> [!WARNING]
> Aby wyłączyć dziennik stdout może doprowadzić do awarii aplikacji lub serwera. Brak limitu rozmiaru pliku dziennika lub liczba pliki dziennika utworzone.
>
> Użyć biblioteki rejestrowania, który ogranicza rozmiar pliku dziennika i dzienników obraca rutynowych rejestrowania w aplikacji platformy ASP.NET Core. Aby uzyskać więcej informacji, zobacz [dostawców innych firm rejestrowania](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="enabling-the-developer-exception-page"></a>Włączanie strony wyjątek Developer

`ASPNETCORE_ENVIRONMENT` [Zmiennej środowiskowej można dodać do pliku web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) do uruchomienia aplikacji w środowisku programistycznym. Dopóki środowisko nie jest zastąpione w przypadku uruchamiania aplikacji przez `UseEnvironment` umożliwia ustawienie zmiennej środowiskowej na konstruktorze hosta [strona wyjątek dewelopera](xref:fundamentals/error-handling) pojawią się po uruchomieniu aplikacji.

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

Ustawienie zmiennej środowiskowej dla `ASPNETCORE_ENVIRONMENT` jest zalecane tylko dla przemieszczania i testowanie serwerów, które nie są dostępne w Internecie. Usuń zmienną środowiskową z *web.config* plik po rozwiązywania problemów. Aby uzyskać informacje na temat ustawiania zmiennych środowiskowych *web.config*, zobacz [environmentVariables elementem podrzędnym aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

## <a name="common-startup-errors"></a>Typowe błędy uruchamiania 

Zobacz [częsta błędów platformy ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference). Większość typowych problemów, które uniemożliwiają uruchamianie aplikacji znajdują się w temacie.

## <a name="slow-or-hanging-app"></a>Wolne lub zwisa aplikacji

Gdy aplikacja reaguje powoli lub zawiesza się na żądanie, uzyskać i analizować [plik zrzutu](/visualstudio/debugger/using-dump-files). Pliki zrzutu można uzyskać przy użyciu dowolnego z następujących narzędzi:

* [ProcDump](/sysinternals/downloads/procdump)
* [DebugDiag](https://www.microsoft.com/download/details.aspx?id=49924)
* WinDbg: [Pobierz narzędzia debugowania dla systemu Windows](https://developer.microsoft.com/windows/hardware/download-windbg), [WinDbg za pomocą debugowania](/windows-hardware/drivers/debugger/debugging-using-windbg)

## <a name="remote-debugging"></a>Debugowanie zdalne

Zobacz [zdalnego debugowania ASP.NET Core na zdalnym komputerze usług IIS w programie Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) w dokumentacji programu Visual Studio.

## <a name="application-insights"></a>Application Insights

[Usługa Application Insights](/azure/application-insights/) zawiera dane telemetryczne z aplikacji hostowanych przez usługi IIS, w tym błąd rejestrowania i funkcje raportowania. Usługi Application Insights można tylko raporty dotyczące błędów występujących po uruchomieniu aplikacji funkcji rejestrowania aplikacji stają się dostępne. Aby uzyskać więcej informacji, zobacz [usługi Application Insights dla platformy ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="additional-troubleshooting-advice"></a>Dodatkowe porady dotyczące rozwiązywania problemów

Czasami funkcjonalnej aplikacji nie powiedzie się natychmiast, po uaktualnieniu albo zestawu .NET Core SDK w wersjach programowanie maszyny lub pakietu w aplikacji. W niektórych przypadkach niespójne pakiety mogą być dzielone aplikacji podczas przeprowadzania uaktualnienia głównych. Większość tych problemów można naprawić, wykonując następujące instrukcje:

1. Usuń *bin* i *obj* folderów.
1. Wyczyść buforuje pakietu na *% UserProfile %\\.nuget\\pakiety* i *LocalAppData %\\Nuget\\pamięci podręcznej v3*.
1. Przywracanie i skompiluj ponownie projekt.
1. Upewnij się, że poprzedniego wdrożenia na serwerze zostaną całkowicie usunięte przed ponownego wdrażania aplikacji.

> [!TIP]
> Jest to wygodny sposób czyszczenia pamięci podręcznych pakietu do wykonania `dotnet nuget locals all --clear` z wiersza polecenia.
> 
> Wyczyszczenie pamięci podręcznej pakiet może być również wykonywane przy użyciu [nuget.exe](https://www.nuget.org/downloads) narzędzie i wykonywania polecenia `nuget locals all -clear`. *nuget.exe* nie jest powiązane instalacji z pulpitu systemu operacyjnego i należy je uzyskać oddzielnie od [NuGet witryny sieci Web](https://www.nuget.org/downloads).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wprowadzenie do obsługi błędów w platformy ASP.NET Core](xref:fundamentals/error-handling)
* [Typowe błędy odwołania dla usługi Azure App Service i IIS z platformy ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Odwołania do konfiguracji modułu platformy ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)
