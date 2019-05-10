---
title: Rejestrowania i diagnostyki w biblioteki SignalR platformy ASP.NET Core
author: anurse
description: Dowiedz się, jak zbieranie diagnostyki aplikacji biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: signalr
ms.date: 02/27/2019
uid: signalr/diagnostics
ms.openlocfilehash: b6bd21314ed183488999bcff3553e53493537a11
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902785"
---
# <a name="logging-and-diagnostics-in-aspnet-core-signalr"></a>Rejestrowania i diagnostyki w biblioteki SignalR platformy ASP.NET Core

Przez [Andrew Stanton pielęgniarki](https://twitter.com/anurse)

Ten artykuł zawiera wskazówki dotyczące zbieranie diagnostyki z Twojej aplikacji biblioteki SignalR platformy ASP.NET Core, aby ułatwić rozwiązywanie problemów.

## <a name="server-side-logging"></a>Rejestrowania po stronie serwera

> [!WARNING]
> Dzienniki po stronie serwera może zawierać poufne informacje z Twojej aplikacji. **Nigdy nie** Publikuj nieprzetworzonych dzienników z aplikacji produkcyjnych na forach publicznych, takich jak GitHub.

Ponieważ SignalR jest częścią platformy ASP.NET Core, używa platformy ASP.NET Core rejestrowania systemu. W domyślnej konfiguracji SignalR rejestruje informacje w niewielkim stopniu, ale może to skonfigurować. Znajdują się w dokumentacji na [platformy ASP.NET Core rejestrowania](xref:fundamentals/logging/index#configuration) Aby uzyskać szczegółowe informacje na temat konfigurowania programu ASP.NET Core rejestrowania.

SignalR używa dwóch kategorii rejestratora:

* `Microsoft.AspNetCore.SignalR` — w przypadku dzienników związane z Centrum protokołów aktywowanie koncentratorów, wywoływanie metod i inne działania związane z Centrum.
* `Microsoft.AspNetCore.Http.Connections` — w przypadku dzienników powiązane z transportami, takich jak funkcja WebSockets, długie sondowania i Server-Sent zdarzeń i niskiego poziomu infrastrukturą SignalR.

Aby włączyć dzienniki z SignalR, skonfiguruj zarówno poprzedniego prefiksów `Debug` poziom w swojej `appsettings.json` pliku, dodając następujące elementy, aby `LogLevel` podsekcji w `Logging`:

[!code-json[Configuring logging](diagnostics/logging-config.json?highlight=7-8)]

Można również skonfigurować ten w kodzie w swojej `CreateWebHostBuilder` metody:

[!code-csharp[Configuring logging in code](diagnostics/logging-config-code.cs?highlight=5-6)]

Jeśli nie używasz konfiguracji opartej na formacie JSON, ustaw następujące wartości konfiguracji w Twoim systemie konfiguracji:

* `Logging:LogLevel:Microsoft.AspNetCore.SignalR` = `Debug`
* `Logging:LogLevel:Microsoft.AspNetCore.Http.Connections` = `Debug`

Sprawdź w dokumentacji systemu konfiguracji określić, jak określić wartości konfiguracji zagnieżdżonych. Na przykład w przypadku używania zmiennych środowiskowych dwóch `_` znaki są używane zamiast `:` (takich jak: `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).

Firma Microsoft zaleca używanie `Debug` poziomu podczas zbierania bardziej szczegółowych diagnostyki dla aplikacji. `Trace` Poziomu powoduje bardzo diagnostyki i rzadko jest potrzebne do diagnozowania problemów w aplikacji.

## <a name="access-server-side-logs"></a>Uzyskiwanie dostępu do dzienników po stronie serwera

Sposób uzyskiwania dostępu do dzienników po stronie serwera, zależy od środowiska, w którym pracujesz.

### <a name="as-a-console-app-outside-iis"></a>Jako aplikację konsoli poza usług IIS

Jeśli pracujesz w aplikacji konsolowej [rejestratora konsoli](xref:fundamentals/logging/index#console-provider) powinno być włączone domyślnie. SignalR dzienników będą wyświetlane w konsoli.

### <a name="within-iis-express-from-visual-studio"></a>W ramach usług IIS Express z programu Visual Studio

Visual Studio wyświetla dane wyjściowe dziennika w **dane wyjściowe** okna. Wybierz **serwera sieci Web programu ASP.NET Core** listy rozwijanej opcję.

### <a name="azure-app-service"></a>Usługa Azure App Service

Włącz opcję "Rejestrowanie aplikacji (system plików)" w sekcji "Dzienniki diagnostyczne" w portalu usługi Azure App Service i skonfigurować poziom `Verbose`. Dzienniki powinny być dostępne usługi "Przesyłanie strumieniowe dzienników", a także, jak dzienniki usługi App Service w systemie plików. Aby uzyskać więcej informacji, zobacz dokumentację na [przesyłanie strumieniowe dzienników platformy Azure](xref:fundamentals/logging/index#azure-log-streaming).

### <a name="other-environments"></a>Innych środowisk

Jeśli używasz w innym środowisku (Docker, Kubernetes, usługa Windows itp.), zobacz pełną dokumentację na [rejestrowania programu ASP.NET Core](xref:fundamentals/logging/index) Aby uzyskać więcej informacji na temat konfigurowania rejestrowania dostawców odpowiednie dla danego środowiska.

## <a name="javascript-client-logging"></a>Rejestrowanie klienta JavaScript

> [!WARNING]
> Dzienniki po stronie klienta mogą zawierać poufne informacje z Twojej aplikacji. **Nigdy nie** Publikuj nieprzetworzonych dzienników z aplikacji produkcyjnych na forach publicznych, takich jak GitHub.

Podczas korzystania z klienta JavaScript, można skonfigurować opcje rejestrowania przy użyciu `configureLogging` metody `HubConnectionBuilder`:

[!code-javascript[Configuring logging in the JavaScript client](diagnostics/logging-config-js.js?highlight=3)]

Aby wyłączyć rejestrowanie w całości, należy określić `signalR.LogLevel.None` w `configureLogging` metody.

W poniższej tabeli przedstawiono dostępne poziomy dziennika dla klienta język JavaScript. Ustawienie poziomu dziennika na jedną z następujących wartości umożliwia rejestrowanie na danym poziomie i wszystkie poziomy nad nim w tabeli.

| Poziom | Opis |
| ----- | ----------- |
| `None` | Żadne komunikaty są rejestrowane. |
| `Critical` | Komunikaty, które wskazują błędy w całej aplikacji. |
| `Error` | Komunikaty, które wskazują błędy w bieżącej operacji. |
| `Warning` | Komunikaty, które wskazują na problem krytyczny. |
| `Information` | Komunikaty informacyjne. |
| `Debug` | Komunikaty diagnostyczne przydatne podczas debugowania. |
| `Trace` | Bardzo szczegółowe komunikaty diagnostyczne przeznaczone dla diagnozowanie konkretnych problemów. |

Po skonfigurowaniu poziom szczegółowości, dzienniki będą zapisywane w konsoli przeglądarki (lub standardowe dane wyjściowe w aplikacji node.js).

Jeśli chcesz wysłać dzienniki do systemu rejestrowania niestandardowego, możesz podać implementacja obiektu JavaScript `ILogger` interfejsu. Jest jedyną metodą, która musi zostać wdrożone `log`, który przyjmuje poziom zdarzenia i wiadomości skojarzonej ze zdarzeniem. Na przykład:

[!code-typescript[Creating a custom logger](diagnostics/custom-logger.ts?highlight=3-7,13)]

## <a name="net-client-logging"></a>Rejestrowanie klienta platformy .NET

> [!WARNING]
> Dzienniki po stronie klienta mogą zawierać poufne informacje z Twojej aplikacji. **Nigdy nie** Publikuj nieprzetworzonych dzienników z aplikacji produkcyjnych na forach publicznych, takich jak GitHub.

Aby uzyskać dzienniki z klienta .NET, możesz użyć `ConfigureLogging` metody `HubConnectionBuilder`. To działa tak samo jak `ConfigureLogging` metody `WebHostBuilder` i `HostBuilder`. Można skonfigurować tego samego dostawcy rejestrowania używanej w programie ASP.NET Core. Jednak należy ręcznie zainstalować i włączyć pakietów NuGet dla indywidualnych rejestrowania dostawców.

### <a name="console-logging"></a>Rejestrowanie konsoli

Aby włączyć rejestrowanie konsoli, Dodaj [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) pakietu. Następnie należy użyć `AddConsole` Metoda konfiguracji rejestratora konsoli:

[!code-csharp[Configuring console logging in .NET client](diagnostics/net-client-console-log.cs?highlight=6)]

### <a name="debug-output-window-logging"></a>Rejestrowanie okna dane wyjściowe debugowania

Można również skonfigurować dzienniki, aby przejść do **dane wyjściowe** okna w programie Visual Studio. Zainstaluj [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) pakietu i użyj `AddDebug` metody:

[!code-csharp[Configuring debug output window logging in .NET client](diagnostics/net-client-debug-log.cs?highlight=6)]

### <a name="other-logging-providers"></a>Innych dostawców logowania

SignalR obsługuje innych dostawców logowania, takie jak Serilog, Seq, NLog lub innego systemu rejestrowania, która integruje się z `Microsoft.Extensions.Logging`. Jeśli system rejestrowania udostępnia `ILoggerProvider`, można zarejestrować go za pomocą `AddProvider`:

[!code-csharp[Configuring a custom logging provider in .NET client](diagnostics/net-client-custom-log.cs?highlight=6)]

### <a name="control-verbosity"></a>Szczegółowość kontroli

Jeśli logujesz się w innych miejscach w aplikacji, zmiana domyślnego poziomu do `Debug` może być zbyt pełne. Filtr umożliwia konfigurowanie poziomu rejestrowania dzienników SignalR. Można to zrobić w kodzie, w taki sam sposób, jak na serwerze:

[!code-csharp[Controlling verbosity in .NET client](diagnostics/logging-config-client-code.cs?highlight=9-10)]

## <a name="network-traces"></a>Danych śledzenia sieci

> [!WARNING]
> Śledzenie sieci zawiera całą zawartość każda wiadomość wysłana przez aplikację. **Nigdy nie** publikowanie danych śledzenia sieci pierwotnych z aplikacji produkcyjnych na forach publicznych, takich jak GitHub.

Jeśli wystąpi problem, śledzenia sieci czasami może zapewnić mnóstwo przydatnych informacji. Jest to szczególnie przydatne, jeśli zamierzasz Prześlij zgłoszenie na nasze narzędzia do śledzenia błędów.

## <a name="collect-a-network-trace-with-fiddler-preferred-option"></a>Zbierać dane śledzenia sieci przy użyciu programu Fiddler (preferowaną opcję)

Ta metoda działa w przypadku wszystkich aplikacji.

Jest bardzo zaawansowanego narzędzia do zbierania danych śledzenia protokołu HTTP. Zainstaluj go z [telerik.com/fiddler](https://www.telerik.com/fiddler), uruchom ją, a następnie uruchom aplikację i odtworzyć problem. Jest dostępna dla Windows, a istnieją wersje beta dla systemu macOS i Linux.

Jeśli łączysz się przy użyciu protokołu HTTPS, istnieją pewne dodatkowe kroki, aby upewnić się, że program Fiddler można odszyfrowywanie ruchu protokołu HTTPS. Aby uzyskać więcej informacji, zobacz [dokumentacji programu Fiddler](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).

Zostały zebrane śledzenia można eksportować śledzenia, wybierając **pliku** > **Zapisz** > **wszystkie sesje...**  z paska menu.

![Eksportowanie wszystkich sesji z programu Fiddler](diagnostics/fiddler-export.png)

## <a name="collect-a-network-trace-with-tcpdump-macos-and-linux-only"></a>Zbierać dane śledzenia sieci za pomocą tcpdump (z systemem macOS i Linux tylko)

Ta metoda działa w przypadku wszystkich aplikacji.

Możesz zbierać pierwotne TCP danych śledzenia przy użyciu tcpdump, uruchamiając następujące polecenie z powłoki poleceń. Konieczne może być `root` lub prefiks polecenie `sudo` Jeśli zostanie wyświetlony błąd uprawnień:

```console
tcpdump -i [interface] -w trace.pcap
```

Zastąp `[interface]` z interfejsem sieciowym, które mają być przechwytywane. Zazwyczaj jest podobny do `/dev/eth0` (w przypadku standardowego interfejsu Ethernet) lub `/dev/lo0` (dla ruchu hosta lokalnego). Aby uzyskać więcej informacji, zobacz `tcpdump` strony ataków typu man w systemie hosta.

## <a name="collect-a-network-trace-in-the-browser"></a>Zbierać dane śledzenia sieci w przeglądarce

Ta metoda działa tylko dla aplikacji opartych na przeglądarce.

Większość narzędzi dla deweloperów przeglądarki ma kartę "Sieć", która pozwala na przechwytywanie aktywności sieciowej między przeglądarką a serwerem. Ślady te nie mają jednak komunikaty protokołu WebSocket i Server-Sent zdarzeń. Jeśli używasz tych transportu przy użyciu narzędzia, takiego jak Fiddler lub TcpDump (opisanych poniżej) jest lepszym rozwiązaniem.

### <a name="microsoft-edge-and-internet-explorer"></a>Przeglądarka Microsoft Edge i przeglądarki Internet Explorer

(Instrukcje są takie same dla przeglądarki Edge i Internet Explorer)

1. Naciśnij klawisz F12, aby otworzyć narzędzia programistyczne
2. Kliknij kartę Sieć
3. (Jeśli jest to konieczne), Odśwież stronę i odtworzenia problemu
4. Kliknij ikonę Zapisz na pasku narzędzi, aby wyeksportować śledzenia w formacie "HAR":

![Zapisz ikonę na deweloperów przeglądarki Microsoft Edge narzędzi karty sieciowej](diagnostics/ie-edge-har-export.png)

### <a name="google-chrome"></a>Google Chrome

1. Naciśnij klawisz F12, aby otworzyć narzędzia programistyczne
2. Kliknij kartę Sieć
3. (Jeśli jest to konieczne), Odśwież stronę i odtworzenia problemu
4. Kliknij prawym przyciskiem myszy kliknij w dowolnym miejscu na liście żądań i wybierz polecenie "Zapisz jako plik HAR z zawartością":

![Opcja "Zapisz jako plik HAR z zawartością" Google Chrome Dev Tools sieci karcie](diagnostics/chrome-har-export.png)

### <a name="mozilla-firefox"></a>Mozilla Firefox

1. Naciśnij klawisz F12, aby otworzyć narzędzia programistyczne
2. Kliknij kartę Sieć
3. (Jeśli jest to konieczne), Odśwież stronę i odtworzenia problemu
4. Kliknij prawym przyciskiem myszy kliknij w dowolnym miejscu na liście żądań i wybierz polecenie "Zapisz wszystkie jako plik HAR"

![Opcja "Zapisz wszystko jako HAR" na karcie Narzędzia deweloperskie Mozilla Firefox w sieci](diagnostics/firefox-har-export.png)

## <a name="attach-diagnostics-files-to-github-issues"></a>Dołącz pliki diagnostyki na problemy usługi GitHub

Możesz dołączyć pliki diagnostyki na problemy usługi GitHub, zmieniając ich nazwy, dzięki czemu mają one `.txt` rozszerzenie, a następnie przeciągając i upuszczając je do problemu.

> [!NOTE]
> Problem w usłudze GitHub, nie Wklej zawartość plików dziennika i danych śledzenia sieci. Te dzienniki i dane śledzenia może być dość duży i GitHub zwykle obetnie je.

![Przeciąganie plików dziennika na problem w usłudze GitHub](diagnostics/attaching-diagnostics-files.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:signalr/configuration>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
