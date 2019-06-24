---
title: Rozwiązywanie problemów z platformą ASP.NET Core w usługach IIS
author: guardrex
description: Dowiedz się, jak diagnozować problemy z wdrożeniami usług Internet Information Services (IIS) w aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 4df370dd9b1a5a651bcf767b8b9ace4220bdc345
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313656"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Rozwiązywanie problemów z platformą ASP.NET Core w usługach IIS

Przez [Luke Latham](https://github.com/guardrex)

Ten artykuł zawiera instrukcje na temat platformy ASP.NET Core zdiagnozować problem uruchamiania aplikacji w przypadku hostowania za pomocą [Internet Information Services (IIS)](/iis). Informacje przedstawione w tym artykule mają zastosowanie do hostowania w usługach IIS w systemie Windows Server i Windows Desktop.

Dodatkowe tematy dotyczące rozwiązywania problemów:

* Usługa Azure App Service używa również [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) i usługi IIS do hostowania aplikacji. Aby uzyskać porady, które odnoszą się do usługi Azure App Service dotyczące rozwiązywania problemów, zobacz <xref:host-and-deploy/azure-apps/troubleshoot>.
* <xref:fundamentals/error-handling> opisano sposób obsługi błędów w aplikacji platformy ASP.NET Core podczas programowania w systemie lokalnym.
* [Naucz się debugować przy użyciu programu Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) wprowadza nowe funkcje debugera programu Visual Studio.
* [Debugowanie za pomocą programu Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) w tym artykule opisano obsługę debugowania wbudowana w programie Visual Studio Code.

[!INCLUDE[](~/includes/azure-iis-startup-errors.md)]

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
