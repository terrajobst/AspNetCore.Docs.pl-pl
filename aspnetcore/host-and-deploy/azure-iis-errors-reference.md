---
title: Dokumentacja typowych błędów dla usługi Azure App Service i IIS za pomocą programu ASP.NET Core
author: guardrex
description: Uzyskaj porady dotyczące rozwiązywania problemów dla typowych błędów, odnośnie do hostowania aplikacji platformy ASP.NET Core na usługi aplikacji Azure i usług IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/12/2019
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 0191460f8c3dab98e6f977a29eacf0396b6789d8
ms.sourcegitcommit: b4ef2b00f3e1eb287138f8b43c811cb35a100d3e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970073"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>Dokumentacja typowych błędów dla usługi Azure App Service i IIS za pomocą programu ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

W tym temacie oferuje porady dotyczące rozwiązywania problemów dla typowych błędów, odnośnie do hostowania aplikacji platformy ASP.NET Core na usługi aplikacji Azure i usług IIS.

Zbierz wymienione poniżej informacje.

* Zachowanie przeglądarki (stan kodu i komunikatu o błędzie)
* Wpisy dziennika zdarzeń aplikacji
  * Usługa Azure App Service &ndash; zobacz <xref:host-and-deploy/azure-apps/troubleshoot>.
  * IIS
    1. Wybierz **Start** na **Windows** menu, typ *Podgląd zdarzeń*i naciśnij klawisz **Enter**.
    1. Po **Podgląd zdarzeń** zostanie otwarta, rozwiń węzeł **Dzienniki Windows** > **aplikacji** na pasku bocznym.
* Wpisy dziennika platformy ASP.NET Core modułu stdout i debugowania
  * Usługa Azure App Service &ndash; zobacz <xref:host-and-deploy/azure-apps/troubleshoot>.
  * Usługi IIS &ndash; postępuj zgodnie z instrukcjami w [tworzenia i Przekierowanie dziennika](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) i [rozszerzone dzienniki diagnostyczne](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sekcjach tematu modułu ASP.NET Core.

Porównywanie informacji o błędzie do następujących typowych błędów. Jeśli zostanie znalezione dopasowanie, wykonaj porady dotyczące rozwiązywania problemów.

Lista błędów, w tym temacie nie jest wyczerpująca. Jeśli wystąpi błąd niewymienione w tym Otwórz nowy problem za pomocą **zawartości opinii** przycisk w dolnej części tego tematu zawiera szczegółowe instrukcje dotyczące sposobu odtwarzania błędu.

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a>Instalator nie może uzyskać VC ++ do dystrybucji

* **Instalator wyjątek:** 0x80072EFD **--lub--** 0x80072f76 — nieokreślony błąd

* **Wyjątek dziennika Instalatora&#8224;:** Błąd 0x80072efd **--lub--** 0x80072f76: Nie można uruchomić pakietu EXE

  &#8224;Dziennik znajduje się w *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.

Rozwiązywanie problemów:

Jeśli system nie ma dostępu do Internetu podczas [Instalowanie pakietu hostingu platformy .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), ten wyjątek występuje w przypadku uzyskiwania Instalator będzie mógł *Microsoft Visual C++ 2015 Redistributable*. Uzyskaj Instalator [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840). Jeśli Instalator zakończy się niepowodzeniem, serwer może nie otrzymać wymagane do obsługi środowiska uruchomieniowego .NET Core [zależny od struktury wdrożenia (stacje)](/dotnet/core/deploying/#framework-dependent-deployments-fdd). Jeśli hostingu Dyskietki, upewnij się, że środowisko uruchomieniowe jest zainstalowany w **programy i funkcje** lub **aplikacje i funkcje**. Jeśli wymagana jest określonego środowiska uruchomieniowego, Pobierz środowisko uruchomieniowe z [archiwa Pobierz .NET](https://dotnet.microsoft.com/download/archives) i zainstaluj go na system. Po zainstalowaniu środowiska uruchomieniowego, uruchom ponownie komputer lub uruchom ponownie usługi IIS, wykonując **net stop został /y** następuje **net start w3svc** z poziomu wiersza polecenia.

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>Uaktualnienie systemu operacyjnego usunięte 32-bitowych modułu ASP.NET Core

**Dziennik aplikacji:** Biblioteka DLL modułu **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** udało się załadować. Dane są błędne.

Rozwiązywanie problemów:

Pliki systemu operacyjnego bez **C:\Windows\SysWOW64\inetsrv** katalogu nie są zachowywane w trakcie systemu operacyjnego uaktualnienia. Jeśli zainstalowano modułu ASP.NET Core przed uaktualnienie systemu operacyjnego, a następnie każdej puli aplikacji jest uruchamiana w trybie 32-bitowych po uaktualnieniu systemu operacyjnego, ten problem zostanie osiągnięty. Po uaktualnieniu systemu operacyjnego napraw modułu ASP.NET Core. Zobacz [instalacji pakietu .NET Core hostingu](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle). Wybierz **naprawy** po uruchomieniu Instalatora.

## <a name="missing-site-extension-32-bit-x86-and-64-bit-x64-site-extensions-installed-or-wrong-process-bitness-set"></a>Brak rozszerzenia usługi site, (x86) 32-bitowych i 64-bitowych (x64) zainstalowane rozszerzenia witryny lub ustawić liczbę bitów procesu problem

*Dotyczy aplikacji hostowanych przez usługi Azure App Service.*

* **Przeglądarka:** Błąd HTTP 500.0 - ANCM w procesie programu obsługi błędu ładowania

* **Dziennik aplikacji:** Wywoływanie hostfxr można znaleźć programu obsługi żądania inprocess nie powiodła się bez znajdowanie wszelkie zależności natywnych. Nie można odnaleźć inprocess żądania obsługi. Przechwycone dane wyjściowe z wywołaniem hostfxr: Nie można odnaleźć żadnych zgodnych framework w wersji. Określony framework "Microsoft.AspNetCore.App", wersja "{VERSION} - preview -\*" nie został znaleziony. Nie można uruchomić aplikacji "/ LM/W3SVC/1416782824/ROOT", kod błędu "0x8000ffff".

* **ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Nie można odnaleźć żadnych zgodnych framework w wersji. Określony framework "Microsoft.AspNetCore.App", wersja "{VERSION} - preview -\*" nie został znaleziony.

::: moniker range=">= aspnetcore-2.2"

* **Moduł ASP.NET Core dziennik debugowania:** Wywoływanie hostfxr można znaleźć programu obsługi żądania inprocess nie powiodła się bez znajdowanie wszelkie zależności natywnych. To najprawdopodobniej oznacza, że aplikacja jest nieprawidłowo skonfigurowane, sprawdź, czy wersje pakietów Microsoft.NetCore.App i Microsoft.AspNetCore.App są objęci aplikacji, które są zainstalowane na komputerze. Zwrócone HRESULT nie powiodło się: 0x8000ffff. Nie można odnaleźć inprocess żądania obsługi. Nie można odnaleźć żadnych zgodnych framework w wersji. Określony framework "Microsoft.AspNetCore.App", wersja "{VERSION} - preview -\*" nie został znaleziony.

::: moniker-end

Rozwiązywanie problemów:

* Jeśli uruchamianie aplikacji w środowisku uruchomieniowym w wersji zapoznawczej, należy zainstalować (x86) 32-bitowych **lub** (x64) 64-bitowych lokacji rozszerzeniem odpowiadającym typowi bitowość wersji środowiska uruchomieniowego aplikacji i aplikacji. **Nie należy instalować zarówno rozszerzenia lub wielu wersji środowiska uruchomieniowego rozszerzenia.**

  * ASP.NET Core {RUNTIME VERSION} (x86) Runtime
  * Platforma ASP.NET Core {wersja środowiska URUCHOMIENIOWEGO} (x 64) środowiska uruchomieniowego

  Uruchom ponownie aplikację. Odczekaj kilka sekund dla aplikacji, aby ponownie uruchomić.

* Jeśli aplikacja jest uruchomiona na runtime (wersja zapoznawcza) i (x86) 32-bitowych i 64-bitowych (x64) [rozszerzeń witryny](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) są zainstalowane, odinstaluj rozszerzenie witryny, która nie odpowiada liczbie bitów aplikacji. Po usunięciu rozszerzenia witryny, należy ponownie uruchomić aplikację. Odczekaj kilka sekund dla aplikacji, aby ponownie uruchomić.

* Jeśli aplikacja uruchomiona w środowisku uruchomieniowym w wersji zapoznawczej i rozszerzenia bitowości dopasowań, które aplikacji, upewnij się, które korzystania z wersji zapoznawczej witryny rozszerzenia *wersji środowiska uruchomieniowego* jest zgodna wersja środowiska uruchomieniowego aplikacji.

* Upewnij się, że aplikacja **platformy** w **ustawienia aplikacji** odpowiada liczbie bitów w aplikacji.

Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a>X86 aplikacja jest wdrożona, ale pula aplikacji nie jest włączony dla aplikacji 32-bitowych

* **Przeglądarka:** Błąd HTTP 500.30 — błąd w trakcie uruchamiania ANCM

* **Dziennik aplikacji:** Aplikacja "/ LM/W3SVC/5/ROOT" za pomocą fizycznych głównego {PATH} trafień nieoczekiwany wyjątek zarządzany kod wyjątku = "0xe0434352". Sprawdź dzienniki stderr, aby uzyskać więcej informacji. Aplikacja "/ LM/W3SVC/5/ROOT" za pomocą fizycznych głównego "{PATH}" nie można załadować środowiska clr i zarządzanych aplikacji. Wątek roboczy CLR przedwcześnie zakończył działanie

* **ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika jest utworzony, ale puste.

::: moniker range=">= aspnetcore-2.2"

* **Moduł ASP.NET Core dziennik debugowania:** Zwrócone HRESULT nie powiodło się: 0x8007023e

::: moniker-end

W tym scenariuszu jest to spowodowane zestawu SDK podczas publikowania aplikacja samodzielna. Zestaw SDK generuje błąd, jeśli RID nie jest zgodny platformy docelowej (na przykład `win10-x64` RID z `<PlatformTarget>x86</PlatformTarget>` w pliku projektu).

Rozwiązywanie problemów:

Dla x86 zależny od struktury wdrożenia (`<PlatformTarget>x86</PlatformTarget>`), Włącz puli aplikacji usług IIS dla aplikacji 32-bitowych. W Menedżerze usług IIS, otwórz puli aplikacji **Zaawansowane ustawienia** i ustaw **Włącz 32-bitowych aplikacji** do **True**.

## <a name="platform-conflicts-with-rid"></a>Platforma jest w konflikcie z identyfikatorów RID

* **Przeglądarka:** Błąd HTTP 502.5 — niepowodzenia procesu

* **Dziennik aplikacji:** Aplikacja "APPHOST-MACHINE/WEBROOT / {zestawu}" z certyfikatem głównym fizycznych "C:\{ścieżki}\' nie można uruchomić procesu przy użyciu wiersza polecenia" "C:\{ścieżki} {zestawu}. { plik exe | dll} "", kod błędu = "0x80004005: ff.

* **ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Nieobsługiwany wyjątek: System.BadImageFormatException: Nie można załadować pliku lub zestawu "{zestawu} .dll". Próbowano załadować program w niepoprawnym formacie.

Rozwiązywanie problemów:

* Upewnij się, że aplikacja działa lokalnie na Kestrel. Niepowodzenia procesu może wynikać z problemu w aplikacji. Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów z (IIS)](xref:host-and-deploy/iis/troubleshoot) lub [rozwiązywania problemów (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).

* Jeśli ten wyjątek występuje podczas wdrażania aplikacji platformy Azure, podczas uaktualniania aplikacji i wdrażanie zestawów nowszej ręcznie usunąć wszystkie pliki z poprzedniego wdrożenia. Lingering niezgodne zestawów może spowodować `System.BadImageFormatException` wyjątku w przypadku wdrażania uaktualnionego aplikacji.

## <a name="uri-endpoint-wrong-or-stopped-website"></a>Identyfikator URI punktu końcowego problem lub zatrzymana witryny sieci Web

* **Przeglądarka:** ERR_CONNECTION_REFUSED **--lub--** nie można połączyć z

* **Dziennik aplikacji:** Brak wpisu

* **ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.

::: moniker range=">= aspnetcore-2.2"

* **Moduł ASP.NET Core dziennik debugowania:** Plik dziennika nie jest tworzony.

::: moniker-end

Rozwiązywanie problemów:

* Upewnij się, że właściwego punktu końcowego identyfikator URI aplikacji jest używana. Sprawdź wiązania.

* Upewnij się, że witryny sieci Web usług IIS nie znajduje się w *zatrzymane* stanu.

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>Serwer CoreWebEngine lub W3SVC funkcje wyłączone

**Wyjątek systemu operacyjnego:** Użyj modułu ASP.NET Core można zainstalować funkcje usług IIS 7.0 CoreWebEngine i W3SVC.

Rozwiązywanie problemów:

Upewnij się, czy są włączone odpowiednie role i funkcje. Zobacz [konfiguracji programu IIS](xref:host-and-deploy/iis/index#iis-configuration).

## <a name="incorrect-website-physical-path-or-app-missing"></a>Ścieżka fizyczna niepoprawna witryna sieci Web lub Brak aplikacji

* **Przeglądarka:** 403 Zabroniony — odmowa dostępu **--lub--** zabronione 403.14 — serwer sieci Web jest skonfigurowany, aby nie wyświetlać listę zawartości tego katalogu.

* **Dziennik aplikacji:** Brak wpisu

* **ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.

::: moniker range=">= aspnetcore-2.2"

* **Moduł ASP.NET Core dziennik debugowania:** Plik dziennika nie jest tworzony.

::: moniker-end

Rozwiązywanie problemów:

Sprawdź witrynę sieci Web usług IIS **podstawowych ustawień** i folderu fizycznego aplikacji. Upewnij się, że aplikacja znajduje się w folderze w witrynie sieci Web usług IIS **ścieżkę fizyczną**.

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a>Nieprawidłowa rola, nie zainstalowano modułu ASP.NET Core lub niepoprawne uprawnienia

* **Przeglądarka:** 500.19 — wewnętrzny błąd serwera — żądana strona nie są dostępne, ponieważ odpowiednie dane konfiguracyjne dla strony jest nieprawidłowy. **--LUB--** nie można wyświetlić tej strony

* **Dziennik aplikacji:** Brak wpisu

* **ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.

::: moniker range=">= aspnetcore-2.2"

* **Moduł ASP.NET Core dziennik debugowania:** Plik dziennika nie jest tworzony.

::: moniker-end

Rozwiązywanie problemów:

* Upewnij się, że włączono właściwej roli. Zobacz [konfiguracji programu IIS](xref:host-and-deploy/iis/index#iis-configuration).

* Otwórz **programy i funkcje** lub **aplikacje i funkcje** i upewnij się, że **systemu Windows serwer obsługujący** jest zainstalowany. Jeśli **systemu Windows serwer obsługujący** nie ma na liście zainstalowanych programów, Pobierz i zainstaluj pakiet hostingu platformy .NET Core.

  [Bieżący Instalatora pakietu hostingu programu .NET Core (pobieranie bezpośrednie)](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  Aby uzyskać więcej informacji, zobacz [jest instalowany pakiet hostingu platformy .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

* Upewnij się, że **puli aplikacji** > **Model procesu** > **tożsamości** ustawiono **ApplicationPoolIdentity** lub tożsamość niestandardowa ma odpowiednie uprawnienia dostępu do folderu wdrożenia aplikacji.

* Jeśli odinstalowanie pakietu hostingu platformy ASP.NET Core i zainstalować starszą wersję pakietu hostingu *applicationHost.config* plik nie zawiera sekcji dla modułu ASP.NET Core. Otwórz *applicationHost.config* na *%windir%/System32/inetsrv/config* i Znajdź `<configuration><configSections><sectionGroup name="system.webServer">` grupy sekcji. Jeśli grupy sekcji brakuje sekcji dla modułu ASP.NET Core, Dodaj element sekcji:

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```

  Alternatywnie zainstalować najnowszą wersję pakietu hostingu platformy ASP.NET Core. Najnowsza wersja jest wstecznie zgodny z obsługiwane aplikacje platformy ASP.NET Core.

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>Niepoprawne processPath, Brak zmiennej PATH, hostingu pakietu nie jest zainstalowany, nie uruchomiono ponownie system/IIS, VC ++ Redistributable nie jest zainstalowany lub naruszenie zasad dostępu dotnet.exe

::: moniker range=">= aspnetcore-2.2"

* **Przeglądarka:** Błąd HTTP 500.0 - ANCM w procesie programu obsługi błędu ładowania

* **Dziennik aplikacji:** Aplikacja "MACHINE/WEBROOT/APPHOST / {zestawu}" z certyfikatem głównym fizycznych "C:\{ścieżki}\' nie można uruchomić procesu przy użyciu wiersza polecenia" "{...}" ', ErrorCode = '0x80070002 : 0. Aplikacja {PATH} nie był w stanie uruchomić. Nie można odnaleźć pliku wykonywalnego w lokalizacji {PATH}. Nie można uruchomić aplikacji "/ LM/W3SVC/2/ROOT", kod błędu "0x8007023e".

* **ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.

* **Moduł ASP.NET Core dziennik debugowania:** Dziennik zdarzeń: "Aplikacja"{PATH}"nie był w stanie uruchomić. Nie można odnaleźć pliku wykonywalnego w lokalizacji {PATH}. Zwrócone HRESULT nie powiodło się: 0x8007023e

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* **Przeglądarka:** Błąd HTTP 502.5 — niepowodzenia procesu

* **Dziennik aplikacji:** Aplikacja "MACHINE/WEBROOT/APPHOST / {zestawu}" z certyfikatem głównym fizycznych "C:\{ścieżki}\' nie można uruchomić procesu przy użyciu wiersza polecenia" "{...}" ', ErrorCode = '0x80070002 : 0.

* **ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika jest utworzony, ale puste.

::: moniker-end

Rozwiązywanie problemów:

* Upewnij się, że aplikacja działa lokalnie na Kestrel. Niepowodzenia procesu może wynikać z problemu w aplikacji. Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów z (IIS)](xref:host-and-deploy/iis/troubleshoot) lub [rozwiązywania problemów (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).

* Sprawdź *processPath* atrybutu na `<aspNetCore>` element *web.config* aby upewnić się, że jest `dotnet` wdrożenia zależny od struktury (stacje) lub `.\{ASSEMBLY}.exe` dla [niezależna wdrożenia (— SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).

* Dla Dyskietki *dotnet.exe* może nie być dostępne za pośrednictwem ustawienia ścieżki. Upewnij się, że *C:\Program Files\dotnet\\*  istnieje w ustawieniach ścieżki systemowej.

* Dla Dyskietki *dotnet.exe* może nie być dostępny dla tożsamości puli aplikacji. Upewnij się, że tożsamość puli aplikacji ma dostęp do *C:\Program Files\dotnet* katalogu. Upewnij się, że nie istnieją żadne reguły odmowy skonfigurowane dla tożsamości użytkownika puli aplikacji na *C:\Program Files\dotnet* i katalogów aplikacji.

* STACJE zostały wdrożone, oraz platformy .NET Core zainstalowane bez ponownego uruchamiania usług IIS. Uruchom ponownie serwer, albo ponownego uruchomienia usług IIS, wykonując **net stop został /y** następuje **net start w3svc** z poziomu wiersza polecenia.

* Dyskietki mogą wdrożyć bez konieczności instalowania środowiska uruchomieniowego .NET Core w systemie hostingu. Jeśli nie zainstalowano środowisko uruchomieniowe platformy .NET Core, uruchom **Instalatora programu .NET Core hostingu pakietu** w systemie.

  [Bieżący Instalatora pakietu hostingu programu .NET Core (pobieranie bezpośrednie)](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  Aby uzyskać więcej informacji, zobacz [jest instalowany pakiet hostingu platformy .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

  Jeśli wymagana jest określonego środowiska uruchomieniowego, Pobierz środowisko uruchomieniowe z [archiwa Pobierz .NET](https://dotnet.microsoft.com/download/archives) i zainstaluj go na system. Zakończ instalację, ponowne uruchamianie systemu lub ponowne uruchomienie usług IIS, wykonując **net stop został /y** następuje **net start w3svc** z poziomu wiersza polecenia.

* STACJE zostały wdrożone i *Microsoft Visual C++ 2015 Redistributable (x64)* nie jest zainstalowany w systemie. Uzyskaj Instalator [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).

## <a name="incorrect-arguments-of-aspnetcore-element"></a>Nieprawidłowe argumenty \<aspNetCore > element

::: moniker range=">= aspnetcore-2.2"

* **Przeglądarka:** Błąd HTTP 500.0 - ANCM w procesie programu obsługi błędu ładowania

* **Dziennik aplikacji:** Wywoływanie hostfxr można znaleźć programu obsługi żądania inprocess nie powiodła się bez znajdowanie wszelkie zależności natywnych. To najprawdopodobniej oznacza, że aplikacja jest nieprawidłowo skonfigurowane, sprawdź, czy wersje pakietów Microsoft.NetCore.App i Microsoft.AspNetCore.App są objęci aplikacji, które są zainstalowane na komputerze. Nie można odnaleźć inprocess żądania obsługi. Przechwycone dane wyjściowe z wywołaniem hostfxr: Czy chodziło Ci o polecenia zestawu SDK platformy dotnet? Zainstaluj zestaw SDK platformy dotnet z: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Nie można uruchomić aplikacji "/ LM/W3SVC/3/ROOT", kod błędu "0x8000ffff".

* **ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Czy chodziło Ci o polecenia zestawu SDK platformy dotnet? Zainstaluj zestaw SDK platformy dotnet z: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409

* **Moduł ASP.NET Core dziennik debugowania:** Wywoływanie hostfxr można znaleźć programu obsługi żądania inprocess nie powiodła się bez znajdowanie wszelkie zależności natywnych. To najprawdopodobniej oznacza, że aplikacja jest nieprawidłowo skonfigurowane, sprawdź, czy wersje pakietów Microsoft.NetCore.App i Microsoft.AspNetCore.App są objęci aplikacji, które są zainstalowane na komputerze. Zwrócone HRESULT nie powiodło się: 0x8000ffff nie można odnaleźć inprocess żądania obsługi. Przechwycone dane wyjściowe z wywołaniem hostfxr: Czy chodziło Ci o polecenia zestawu SDK platformy dotnet? Zainstaluj zestaw SDK platformy dotnet z: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Zwrócone HRESULT nie powiodło się: 0x8000ffff

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* **Przeglądarka:** Błąd HTTP 502.5 — niepowodzenia procesu

* **Dziennik aplikacji:** Aplikacja "MACHINE/WEBROOT/APPHOST / {zestawu}" z certyfikatem głównym fizycznych "C:\{ścieżki}\' nie można uruchomić procesu przy użyciu wiersza polecenia" "dotnet".\{ Plik .dll zestawu} ", kod błędu =" 0x80004005: 80008081.

* **ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Wykonywanie aplikacji nie istnieje: 'PATH\{ASSEMBLY}.dll'

::: moniker-end

Rozwiązywanie problemów:

* Upewnij się, że aplikacja działa lokalnie na Kestrel. Niepowodzenia procesu może wynikać z problemu w aplikacji. Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów z (IIS)](xref:host-and-deploy/iis/troubleshoot) lub [rozwiązywania problemów (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).

* Sprawdź *argumenty* atrybutu na `<aspNetCore>` elementu w *web.config* aby upewnić się, że jest ona () `.\{ASSEMBLY}.dll` wdrożenia zależny od struktury (stacje); lub (b) nie istnieje pusty ciąg (`arguments=""`), lub Podaj listę aplikacji argumentów (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) niezależna wdrożenia (— SCD).

::: moniker range=">= aspnetcore-2.2"

## <a name="missing-net-core-shared-framework"></a>Brak udostępnionej platformy .NET Core

* **Przeglądarka:** Błąd HTTP 500.0 - ANCM w procesie programu obsługi błędu ładowania

* **Dziennik aplikacji:** Wywoływanie hostfxr można znaleźć programu obsługi żądania inprocess nie powiodła się bez znajdowanie wszelkie zależności natywnych. To najprawdopodobniej oznacza, że aplikacja jest nieprawidłowo skonfigurowane, sprawdź, czy wersje pakietów Microsoft.NetCore.App i Microsoft.AspNetCore.App są objęci aplikacji, które są zainstalowane na komputerze. Nie można odnaleźć inprocess żądania obsługi. Przechwycone dane wyjściowe z wywołaniem hostfxr: Nie można odnaleźć żadnych zgodnych framework w wersji. Określony framework "Microsoft.AspNetCore.App", wersja {VERSION} nie został znaleziony.

Nie można uruchomić aplikacji "/ LM/W3SVC/5/ROOT", kod błędu "0x8000ffff".

* **ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Nie można odnaleźć żadnych zgodnych framework w wersji. Określony framework "Microsoft.AspNetCore.App", wersja {VERSION} nie został znaleziony.

* **Moduł ASP.NET Core dziennik debugowania:** Zwrócone HRESULT nie powiodło się: 0x8000ffff

::: moniker-end

Rozwiązywanie problemów:

W przypadku wdrożenia zależny od struktury (stacje) upewnij się, że poprawne środowisko uruchomieniowe zainstalowane w systemie.

## <a name="stopped-application-pool"></a>Zatrzymanej puli aplikacji

* **Przeglądarka:** 503 Usługa niedostępna

* **Dziennik aplikacji:** Brak wpisu

* **ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.

::: moniker range=">= aspnetcore-2.2"

* **Moduł ASP.NET Core dziennik debugowania:** Plik dziennika nie jest tworzony.

::: moniker-end

Rozwiązywanie problemów:

Upewnij się, że pula aplikacji nie znajduje się w *zatrzymane* stanu.

## <a name="sub-application-includes-a-handlers-section"></a>Aplikacja podrzędnych zawiera \<obsługi > sekcji

* **Przeglądarka:** Błąd HTTP 500.19 — wewnętrzny błąd serwera

* **Dziennik aplikacji:** Brak wpisu

* **ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Aplikacja główny plik dziennika jest tworzony i pokazuje normalnego działania. Nie jest tworzony plik dziennika aplikacji podrzędnej.

::: moniker range=">= aspnetcore-2.2"

* **Moduł ASP.NET Core dziennik debugowania:** Aplikacja główny plik dziennika jest tworzony i pokazuje normalnego działania. Nie jest tworzony plik dziennika aplikacji podrzędnej.

::: moniker-end

Rozwiązywanie problemów:

::: moniker range=">= aspnetcore-2.2"

Upewnij się, że aplikacja sub *web.config* file nie dołącza `<handlers>` sekcji lub że aplikacji podrzędnej nie dziedziczy obsługi aplikacji nadrzędnej.

Aplikacja nadrzędna `<system.webServer>` części *web.config* znajduje się wewnątrz `<location>` elementu. <xref:System.Configuration.SectionInformation.InheritInChildApplications*> Właściwość jest ustawiona na `false` do wskazania, że ustawienia określone w [ \<lokalizacja >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) elementu nie są dziedziczone przez aplikacje, które znajdują się w podkatalogu aplikacja nadrzędna. Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/aspnet-core-module>.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Upewnij się, że aplikacja sub *web.config* file nie dołącza `<handlers>` sekcji.

::: moniker-end

## <a name="stdout-log-path-incorrect"></a>Nieprawidłowa ścieżka dziennika stdout

* **Przeglądarka:** Aplikacja reaguje, normalnie.

::: moniker range=">= aspnetcore-2.2"

* **Dziennik aplikacji:** Nie można uruchomić przekierowania stdout C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll. Komunikat o wyjątku: HRESULT 0x80070005 zwracanych w {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84. Nie można zatrzymać przekierowanie stdout w C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll. Komunikat o wyjątku: HRESULT: 0x80070002 zwrócił w lokalizacji {PATH}. Nie można uruchomić przekierowanie stdout w {PATH}\aspnetcorev2_inprocess.dll.

* **ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.

* **Moduł ASP.NET Core dziennik debugowania:** Nie można uruchomić przekierowania stdout C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll. Komunikat o wyjątku: HRESULT 0x80070005 zwracanych w {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84. Nie można zatrzymać przekierowanie stdout w C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll. Komunikat o wyjątku: HRESULT: 0x80070002 zwrócił w lokalizacji {PATH}. Nie można uruchomić przekierowanie stdout w {PATH}\aspnetcorev2_inprocess.dll.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* **Dziennik aplikacji:** Ostrzeżenie: Nie można utworzyć stdoutLogFile \\?\{ ŚCIEŻKA} \path_doesnt_exist\stdout_ {identyfikator procesu} _ {sygnatura CZASOWA} .log, kod błędu =-2147024893.

* **ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.

::: moniker-end

Rozwiązywanie problemów:

* `stdoutLogFile` Ścieżce określonej w `<aspNetCore>` elementu *web.config* nie istnieje. Aby uzyskać więcej informacji, zobacz [modułu ASP.NET Core: Tworzenie i Przekierowanie dziennika](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).

* Użytkownik puli aplikacji nie ma uprawnienia do zapisu w ścieżce dziennika stdout.

## <a name="application-configuration-general-issue"></a>Ogólny problem z konfiguracją aplikacji

::: moniker range=">= aspnetcore-2.2"

* **Przeglądarka:** Błąd HTTP 500.0 - ANCM w procesie programu obsługi błędu ładowania **--lub--** błąd HTTP 500.30 — błąd w trakcie uruchamiania ANCM

* **Dziennik aplikacji:** Zmienna

* **ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika został utworzony, ale puste lub utworzony przy użyciu normalnych pozycji aż do punktu niepowodzenie aplikacji.

* **Moduł ASP.NET Core dziennik debugowania:** Zmienna

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* **Przeglądarka:** Błąd HTTP 502.5 — niepowodzenia procesu

* **Dziennik aplikacji:** Aplikacji "MACHINE/WEBROOT/APPHOST / {zestawu}" z certyfikatem głównym fizycznej "C:\{ścieżki}\' utworzone procesu przy użyciu wiersza polecenia" "C:\{ścieżki}\{zestawu}. { plik exe | dll} "" ale wystąpiła awaria lub nie odpowiada lub Nasłuchuj na podany port "{PORT}", kod błędu = "{Kod błędu:}"

* **ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika jest utworzony, ale puste.

::: moniker-end

Rozwiązywanie problemów:

Proces nie mógł uruchomić, najprawdopodobniej z powodu konfiguracji aplikacji lub programowania problem.

Więcej informacji znajduje się w następujących tematach:

* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:test/troubleshoot>
