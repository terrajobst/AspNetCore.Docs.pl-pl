## <a name="app-startup-errors"></a>Błędy uruchamiania aplikacji

::: moniker range=">= aspnetcore-2.2"

W programie Visual Studio ma domyślnie wartość projektu ASP.NET Core [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hostingu podczas debugowania. A *502.5 - niepowodzenia procesu* lub *500.30 - Start błąd* występuje, gdy debugowanie lokalne może być troubleshooted przy użyciu porady w tym temacie.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

W programie Visual Studio ma domyślnie wartość projektu ASP.NET Core [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hostingu podczas debugowania. A *502.5 niepowodzenia procesu* występuje, gdy debugowanie lokalne może być troubleshooted przy użyciu porady w tym temacie.

::: moniker-end

### <a name="500-internal-server-error"></a>500 Wewnętrzny błąd serwera

Uruchamia aplikację, ale błąd uniemożliwia spełnienie żądania przez serwer.

Ten błąd występuje w kodzie aplikacji, podczas uruchamiania lub podczas tworzenia odpowiedzi. Odpowiedź może zawierać żadnej zawartości lub odpowiedzi może być wyświetlana jako *500 Wewnętrzny błąd serwera* w przeglądarce. W dzienniku zdarzeń aplikacji stwierdza, zwykle uruchomiona aplikacja. Z perspektywy serwera, który jest poprawna. Aplikacja została uruchomiona, ale nie może wygenerować prawidłowej odpowiedzi. Uruchom aplikację w wierszu polecenia na serwerze lub [Włącz dziennik stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log) do rozwiązania problemu.

::: moniker range="= aspnetcore-2.2"

### <a name="5000-in-process-handler-load-failure"></a>500.0 w procesie programu obsługi błędu ładowania

Proces roboczy kończy się niepowodzeniem. Nie zaczyna się aplikacja.

[Modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) nie powiedzie się znaleźć programu .NET Core CLR i Znajdź program obsługi żądania w trakcie (*aspnetcorev2_inprocess.dll*). Sprawdź, czy:

* Aplikacja jest przeznaczona na albo [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) pakietu NuGet lub [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).
* Wersja udostępnionej platformy ASP.NET Core jest zainstalowanie aplikacji jest przeznaczony dla na komputerze docelowym.

### <a name="5000-out-of-process-handler-load-failure"></a>500.0 Błąd ładowania poza procesem programu obsługi

Proces roboczy kończy się niepowodzeniem. Nie zaczyna się aplikacja.

[Modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) umożliwi znalezienia spoza procesu hostingu programu obsługi żądania. Upewnij się, że *aspnetcorev2_outofprocess.dll* znajduje się w podfolderze obok *aspnetcorev2.dll*.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="5000-in-process-handler-load-failure"></a>500.0 w procesie programu obsługi błędu ładowania

Proces roboczy kończy się niepowodzeniem. Nie zaczyna się aplikacja.

Wystąpił nieznany błąd podczas ładowania [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) składników. Wykonaj jedną z następujących czynności:

* Skontaktuj się z pomocą [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (wybierz **narzędzi deweloperskich** następnie **platformy ASP.NET Core**).
* Zadaj pytanie w witrynie Stack Overflow.
* Prześlij zgłoszenie na naszych [repozytorium GitHub](https://github.com/aspnet/AspNetCore).

### <a name="50030-in-process-startup-failure"></a>500.30 w procesie Niepowodzenie uruchamiania

Proces roboczy kończy się niepowodzeniem. Nie zaczyna się aplikacja.

[Modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) próby uruchomienia .NET Core CLR w procesie, ale nie została uruchomiona. Zazwyczaj można ustalić przyczyny niepowodzenia uruchamiania procesu na podstawie wpisów w [dziennik zdarzeń aplikacji](#application-event-log) i [dziennika stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log).

Aplikacja jest błędnie skonfigurowane z powodu przeznaczony dla wersji udostępnionej platformy ASP.NET Core, która nie jest obecny jest jakiś wspólny warunek błędu. Sprawdź, które wersje udostępnionej platformy ASP.NET Core są zainstalowane na komputerze docelowym.

### <a name="50031-ancm-failed-to-find-native-dependencies"></a>500.31 nie można odnaleźć zależności natywnych ANCM

Proces roboczy kończy się niepowodzeniem. Nie zaczyna się aplikacja.

[Modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) próby uruchomienia .NET Core środowiska uruchomieniowego w procesie, ale nie została uruchomiona. Najczęstszą przyczyną tego błędu uruchamiania jest, gdy `Microsoft.NETCore.App` lub `Microsoft.AspNetCore.App` środowisko uruchomieniowe nie jest zainstalowany. Jeśli aplikacja jest wdrażana na docelowej platformy ASP.NET Core 3.0, a ta wersja nie istnieje na maszynie, ten błąd występuje. Przykładowy komunikat o błędzie:

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

Nie odwoływać się do aplikacji `Microsoft.AspNetCore.App` framework. Tylko aplikacje przeznaczone dla `Microsoft.AspNetCore.App` framework może być obsługiwany przez [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

Aby naprawić ten błąd, upewnij się, że aplikacja jest przeznaczony dla `Microsoft.AspNetCore.App` framework. Sprawdź `.runtimeconfig.json` Aby zweryfikować framework docelowe przez aplikację.

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a>500.34 ANCM mieszane modelach hostingu nie jest obsługiwane

Proces roboczy nie może uruchomić aplikację spoza procesu i aplikacji w trakcie tego samego procesu.

Aby naprawić ten błąd, należy uruchamiać aplikacje w oddzielnych pul aplikacji usług IIS.

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a>500.35 wiele aplikacji w trakcie ANCM w tym samym procesie

Proces roboczy nie może uruchomić aplikację spoza procesu i aplikacji w trakcie tego samego procesu.

Aby naprawić ten błąd, należy uruchamiać aplikacje w oddzielnych pul aplikacji usług IIS.

### <a name="50036-ancm-out-of-process-handler-load-failure"></a>500.36 błąd ładowania spoza procesu obsługi ANCM

Obsługa żądań poza procesem, *aspnetcorev2_outofprocess.dll*, obok pozycji nie jest *aspnetcorev2.dll* pliku. Oznacza to uszkodzenie instalacji [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

Aby naprawić ten błąd, napraw instalację [hostingu pakietu programu .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (dla usług IIS) lub Visual Studio (dla usług IIS Express).

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a>500.37 ANCM uruchomienie nie powiodło się przed upływem limitu czasu uruchamiania

ANCM nie można uruchomić w ramach limitu czasu uruchamiania dostarczają. Domyślnie limit czasu wynosi 120 sekund.

Ten błąd może wystąpić podczas uruchamiania dużej liczby aplikacji na tym samym komputerze. Sprawdź, czy wzrostów użycia Procesora/pamięci na serwerze podczas uruchamiania. Może być konieczne przesunąć procesu uruchamiania wielu aplikacji.

::: moniker-end

### <a name="5025-process-failure"></a>502.5 niepowodzenie procesu

Proces roboczy kończy się niepowodzeniem. Nie zaczyna się aplikacja.

[Modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) próby uruchomienia procesu roboczego, ale nie została uruchomiona. Zazwyczaj można ustalić przyczyny niepowodzenia uruchamiania procesu na podstawie wpisów w [dziennik zdarzeń aplikacji](#application-event-log) i [dziennika stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log).

Aplikacja jest błędnie skonfigurowane z powodu przeznaczony dla wersji udostępnionej platformy ASP.NET Core, która nie jest obecny jest jakiś wspólny warunek błędu. Sprawdź, które wersje udostępnionej platformy ASP.NET Core są zainstalowane na komputerze docelowym. *Udostępnionej platformy* to zbiór zestawów ( *.dll* plików), są zainstalowane na komputerze i odwołuje się meta Microsoft.aspnetcore.all, takich jak `Microsoft.AspNetCore.App`. Dokumentacja meta Microsoft.aspnetcore.all można określić minimalnej wymaganej wersji. Aby uzyskać więcej informacji, zobacz [udostępnionej platformy](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).

*502.5 niepowodzenia procesu* strony błędu jest zwracany, jeśli aplikacji lub obsługującego błędnej konfiguracji powoduje niepowodzenie procesu roboczego:

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

[Modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) jest skonfigurowany z domyślną *startupTimeLimit* 120 sekund. Gdy pozostawić wartość domyślną, aplikacja może potrwać do dwóch minut przed moduł dzienniki awarii procesu. Aby uzyskać informacje na temat konfigurowania modułu, zobacz [atrybuty elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).
