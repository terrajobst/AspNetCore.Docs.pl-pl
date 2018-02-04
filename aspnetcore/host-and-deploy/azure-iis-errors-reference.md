---
title: "Typowe błędy odwołania dla usługi Azure App Service i IIS z platformy ASP.NET Core"
author: guardrex
description: "Rozróżnianie typowe błędy hosting aplikacji platformy ASP.NET Core w usłudze aplikacji Azure i usług IIS."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 53b59045751153cd858e13769b5b42d5700e26d4
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/03/2018
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>Typowe błędy odwołania dla usługi Azure App Service i IIS z platformy ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

Następujące nie znajduje się pełna lista błędów. Jeśli wystąpi błąd niewymienione w tym [Otwórz nowy problem](https://github.com/aspnet/Docs/issues/new) z szczegółowe instrukcje dotyczące odtwarzania błędu.

Zbierz wymienione poniżej informacje.

* Zachowanie przeglądarki
* Wpisy w dzienniku zdarzeń aplikacji
* Wpisy dziennika stdout Module głównym programu ASP.NET

Porównywanie informacji do poniższych typowych błędów. Jeśli zostanie znaleziony dopasowanie, wykonaj porady dotyczące rozwiązywania problemów.

## <a name="installer-unable-to-obtain-vc-redistributable"></a>Nie można uzyskać VC ++ pakiet redystrybucyjny Instalatora

* **Instalator wyjątek:** 0x80072efd lub 0x80072f76 — nieokreślony błąd

* **Wyjątek dziennika Instalatora &#8224;:** błąd 0x80072efd lub 0x80072f76: nie można wykonać EXE pakietu

  &#8224; Dziennik znajduje się pod adresem C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.

Rozwiązywanie problemów:

* Jeśli system nie ma dostępu do Internetu, podczas instalowania serwera obsługującego pakietu, ten wyjątek występuje, gdy Instalator będzie mógł uzyskiwania *Microsoft Visual C++ 2015 Redistributable*. Uzyskać Instalatora z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840). Jeśli Instalator zakończy się niepowodzeniem, serwer nie może otrzymywać środowiska uruchomieniowego .NET Core wymaganego do obsługi wdrożenia framework zależne (stacje). Jeśli hosting Dyskietki, upewnij się, że środowisko wykonawcze jest instalowana w programach &amp; funkcji. W razie potrzeby Uzyskaj Instalatora środowiska uruchomieniowego, z [pobiera .NET](https://www.microsoft.com/net/download/core). Po zainstalowaniu środowiska uruchomieniowego, ponownie uruchom system, lub ponownego uruchomienia usług IIS, wykonując **net stop została /y** następuje **net start w3svc** z wiersza polecenia.

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>Uaktualnienie systemu operacyjnego usunąć moduł 32-bitowej platformy ASP.NET Core

* **Dziennik aplikacji:** modułu DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** nie można załadować. Dane to kod błędu.

Rozwiązywanie problemów:

* Pliki systemu operacyjnego bez **C:\Windows\SysWOW64\inetsrv** katalogu nie są zachowywane podczas OS uaktualnienia. Jeśli zainstalowano modułu platformy ASP.NET Core przed uaktualnienia systemu operacyjnego, a następnie wszystkie puli aplikacji jest uruchomiony w trybie 32-bitowej, po uaktualnieniu systemu operacyjnego, ten problem. Po uaktualnieniu systemu operacyjnego Napraw moduł platformy ASP.NET Core. Zobacz [instalacji pakietu .NET Core systemu Windows serwer obsługujący](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle). Wybierz **naprawy** po uruchomieniu Instalatora.

## <a name="platform-conflicts-with-rid"></a>Platforma w konflikcie z identyfikatorów RID

* **Przeglądarki:** błąd HTTP 502.5 — błąd procesu

* **Dziennik aplikacji:** aplikacji "MACHINE/WEBROOT/APPHOST / {zestawu}" z fizyczny katalog główny "C:\{ścieżki}\' nie można uruchomić procesu z wiersza polecenia" "C:\\{PATH} {zestawu}. { exe | dll} "", kod błędu = "0x80004005: ff.

* **Dziennik modułu platformy ASP.NET Core:** nieobsługiwany wyjątek: System.BadImageFormatException: nie można załadować pliku lub zestawu "dll {zestawu}". Próbowano załadować program w niepoprawnym formacie.

Rozwiązywanie problemów:

* Upewnij się, że aplikacja działa lokalnie na Kestrel. Błąd procesu może być wynikiem problemu w aplikacji. Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów](xref:host-and-deploy/iis/troubleshoot).

* Upewnij się, że `<PlatformTarget>` w *.csproj* nie koliduje to z RID. Na przykład określić nie `<PlatformTarget>` z `x86` i publikowanie za pomocą RID `win10-x64`, przy użyciu *dotnet publikowania - c - r wersji win10-x64* lub przez ustawienie `<RuntimeIdentifiers>` w *.csproj*  do `win10-x64`. Projekt publikuje bez ostrzeżenia lub błędu, ale kończy się niepowodzeniem z powyższych wyjątki zarejestrowane w systemie.

* Jeśli ten wyjątek występuje wdrożenia aplikacji Azure, podczas uaktualniania aplikacji i wdrożenie nowszej zestawy, ręcznie usuń wszystkie pliki z poprzedniego wdrożenia. Pokutujące niezgodne zestawy może spowodować `System.BadImageFormatException` wyjątków w przypadku wdrażania uaktualnionego aplikacji.

## <a name="uri-endpoint-wrong-or-stopped-website"></a>Identyfikator URI punktu końcowego nieprawidłowe lub zatrzymania witryny sieci Web

* **Przeglądarki:** ERR_CONNECTION_REFUSED

* **Dziennik aplikacji:** wpisu

* **Dziennik modułu platformy ASP.NET Core:** nie utworzono pliku dziennika

Rozwiązywanie problemów:

* Potwierdź, że poprawne identyfikatora URI punktu końcowego dla aplikacji jest używany. Sprawdź powiązania.

* Upewnij się, że witryna sieci Web usług IIS nie jest w *zatrzymane* stanu.

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>Serwer CoreWebEngine lub W3SVC funkcje wyłączone

* **Wyjątek systemu operacyjnego:** funkcje usług IIS 7.0 CoreWebEngine i W3SVC musi być zainstalowany, aby użyć modułu programu ASP.NET Core.

Rozwiązywanie problemów:

* Upewnij się, że odpowiednie role i funkcje są włączone. Zobacz [konfiguracji usług IIS](xref:host-and-deploy/iis/index#iis-configuration).

## <a name="incorrect-website-physical-path-or-app-missing"></a>Ścieżka fizyczna witryny sieci Web nieprawidłowe lub brakujące aplikacji

* **Przeglądarki:** 403 Zabroniony — odmowa dostępu **--lub--** zabroniony 403.14 — serwer sieci Web jest skonfigurowana do nie listy zawartości tego katalogu.

* **Dziennik aplikacji:** wpisu

* **Dziennik modułu platformy ASP.NET Core:** nie utworzono pliku dziennika

Rozwiązywanie problemów:

* Witrynie sieci Web usług IIS **podstawowych ustawień** i folder aplikacji fizycznych. Upewnij się, że aplikacja znajduje się w folderze w witrynie sieci Web usług IIS **ścieżka fizyczna**.

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>Niepoprawne roli, nie jest zainstalowany moduł lub nieprawidłowe uprawnienia

* **Przeglądarki:** 500.19 wewnętrzny błąd serwera — nie można uzyskać dostępu do żądanej strony, ponieważ jej odpowiednie dane konfiguracyjne dla strony jest nieprawidłowy.

* **Dziennik aplikacji:** wpisu

* **Dziennik modułu platformy ASP.NET Core:** nie utworzono pliku dziennika

Rozwiązywanie problemów:

* Upewnij się, że właściwej roli jest włączona. Zobacz [konfiguracji usług IIS](xref:host-and-deploy/iis/index#iis-configuration).

* Sprawdź **programy &amp; funkcje** i upewnij się, że **modułu programu Microsoft ASP.NET Core** został zainstalowany. Jeśli **modułu programu Microsoft ASP.NET Core** nie występuje na liście zainstalowanych programów, instalowania modułu. Zobacz [instalacji pakietu .NET Core systemu Windows serwer obsługujący](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).

* Upewnij się, że **puli aplikacji** > **Model procesu** > **tożsamości** ustawiono **puli** lub tożsamość niestandardowa ma odpowiednie uprawnienia dostępu do folderu wdrożenia aplikacji.

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>Niepoprawne processPath, Brak zmiennej PATH, pakiet hostingu nie jest zainstalowany, system/IIS ponowne uruchomienie nie, VC ++ Redistributable nie jest zainstalowany lub dotnet.exe naruszenie zasad dostępu

* **Przeglądarki:** błąd HTTP 502.5 — błąd procesu

* **Dziennik aplikacji:** aplikacji "MACHINE/WEBROOT/APPHOST / {zestawu}" z fizyczny katalog główny "C:\\{PATH}\' nie można uruchomić procesu z wiersza polecenia" ".\{ .exe zestawu}"", kod błędu = "0x80070002: 0.

* **Dziennik modułu platformy ASP.NET Core:** utworzony plik dziennika, ale pusty

Rozwiązywanie problemów:

* Upewnij się, że aplikacja działa lokalnie na Kestrel. Błąd procesu może być wynikiem problemu w aplikacji. Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów](xref:host-and-deploy/iis/troubleshoot).

* Sprawdź *processPath* atrybutu `<aspNetCore>` element *web.config* aby upewnić się, że jest *dotnet* wdrożenia framework zależne (stacje) lub *. \{zestawu} .exe* niezależne wdrożenia (SCD).

* Dla Dyskietki *dotnet.exe* może nie być dostępny za pośrednictwem ustawienia ścieżki. Upewnij się, że * C:\Program Files\dotnet\* istnieje w ŚCIEŻCE systemowej ustawienia.

* Dla Dyskietki *dotnet.exe* mogą nie być dostępne dla tożsamości puli aplikacji. Upewnij się, że tożsamość puli aplikacji ma dostęp do *C:\Program Files\dotnet* katalogu. Upewnij się, że nie istnieją żadne reguły odmowy skonfigurowane dla tożsamości użytkownika puli aplikacji na *C:\Program Files\dotnet* i katalogów aplikacji.

* STACJE zostały wdrożone, oraz .NET Core zainstalowany bez ponownego uruchomienia usług IIS. Uruchom ponownie serwer lub ponownego uruchomienia usług IIS, wykonując **net stop została /y** następuje **net start w3svc** z wiersza polecenia.

* STACJE mogą wdrożyć bez instalowania środowiska uruchomieniowego .NET Core przez system operacyjny. Jeśli nie zainstalowano środowiska wykonawczego platformy .NET Core, uruchom **.NET Core systemu Windows serwer obsługujący Instalatora pakietu** w systemie. Zobacz [instalacji pakietu .NET Core systemu Windows serwer obsługujący](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle). Jeśli próba instalowanie środowiska uruchomieniowego .NET Core w systemie bez połączenia z Internetem, uzyskać środowiska uruchomieniowego z [pobiera .NET](https://www.microsoft.com/net/download/core) i uruchom Instalatora pakietu hostingu, aby zainstalować moduł platformy ASP.NET Core. Ukończenie instalacji przez ponowne uruchomienie systemu lub ponowne uruchomienie usług IIS, wykonując **net stop została /y** następuje **net start w3svc** z wiersza polecenia.

* STACJE zostały wdrożone i *Microsoft Visual C++ 2015 Redistributable (x64)* nie jest zainstalowany w systemie. Uzyskać Instalatora z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).

## <a name="incorrect-arguments-of-aspnetcore-element"></a>Nieprawidłowe argumenty \<aspNetCore\> — element

* **Przeglądarki:** błąd HTTP 502.5 — błąd procesu

* **Dziennik aplikacji:** aplikacji "MACHINE/WEBROOT/APPHOST / {zestawu}" z fizyczny katalog główny "C:\\{PATH}\' nie można uruchomić procesu z wiersza polecenia" "dotnet".\{ DLL zestawu} ", kod błędu =" 0x80004005: 80008081.

* **Program ASP.NET Core modułu dziennika:** wykonywanie aplikacji nie istnieje: "ŚCIEŻKA\{zestawu} .dll"

Rozwiązywanie problemów:

* Upewnij się, że aplikacja działa lokalnie na Kestrel. Błąd procesu może być wynikiem problemu w aplikacji. Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów](xref:host-and-deploy/iis/troubleshoot).

* Sprawdź *argumenty* atrybutu `<aspNetCore>` element *web.config* aby upewnić się, że jest ona () *.\{ DLL zestawu}* we wdrożeniu (stacje); zależne od framework lub brak pustego ciągu (b) (*argumenty = ""*), lub listę argumentów aplikacji (*argumenty = "arg1, arg2..."*) Samodzielne wdrożenia (SCD).

## <a name="missing-net-framework-version"></a>Brak wersji .NET Framework

* **Przeglądarki:** 502.3 Zła brama — wystąpił błąd połączenia podczas próby trasy żądania.

* **Dziennik aplikacji:** ErrorCode = aplikacji "MACHINE/WEBROOT/APPHOST / {zestawu}" z fizyczny katalog główny "C:\\{PATH}\' nie można uruchomić procesu z wiersza polecenia" "dotnet".\{ DLL zestawu} ", kod błędu =" 0x80004005: 80008081.

* **Dziennik modułu platformy ASP.NET Core:** Brak metody, pliku lub zestawu wyjątku. Metoda, pliku lub zestawu określonego w wyjątek jest metoda, pliku lub zestawu .NET Framework.

Rozwiązywanie problemów:

* Zainstaluj wersję .NET Framework w systemie.

* Zależne od framework wdrożenia (stacje) Potwierdź, że poprawne środowiska uruchomieniowego zainstalowane w systemie. Jeśli projekt jest uaktualniana 1.1 2.0 wdrożony system obsługujący i wyniki tego wyjątku, upewnij się, że 2.0 framework jest przez system operacyjny.

## <a name="stopped-application-pool"></a>Zatrzymanej puli aplikacji

* **Przeglądarki:** 503 Usługa jest niedostępna

* **Dziennik aplikacji:** wpisu

* **Dziennik modułu platformy ASP.NET Core:** nie utworzono pliku dziennika

Rozwiązywanie problemów

* Upewnij się, że pula aplikacji nie znajduje się w *zatrzymane* stanu.

## <a name="iis-integration-middleware-not-implemented"></a>Oprogramowanie pośredniczące integracji usług IIS nie jest zaimplementowana

* **Przeglądarki:** błąd HTTP 502.5 — błąd procesu

* **Dziennik aplikacji:** aplikacji "MACHINE/WEBROOT/APPHOST / {zestawu}" z fizyczny katalog główny "C:\\{PATH}\' utworzyć procesu z wiersza polecenia" "C:\\{PATH}\{zestawu}. { exe | dll} "" ale awaria lub odpowiedzi nie albo Nasłuchuj na danego portu "{PORT}", kod błędu = "0x800705b4"

* **Dziennik modułu platformy ASP.NET Core:** plik dziennika utworzony i przedstawia normalnego działania.

Rozwiązywanie problemów

* Upewnij się, że aplikacja działa lokalnie na Kestrel. Błąd procesu może być wynikiem problemu w aplikacji. Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów](xref:host-and-deploy/iis/troubleshoot).

* Upewnij się, że to:
  * Oprogramowanie pośredniczące integracji usług IIS jest referencedby wywoływania `UseIISIntegration` metody w aplikacji `WebHostBuilder` (platformy ASP.NET Core 1.x)
  * Używa aplikacji `CreateDefaultBuilder` — metoda (platformy ASP.NET Core 2.x).
  
  Zobacz [Hosting w ASP.NET Core](xref:fundamentals/hosting) szczegółowe informacje.

## <a name="sub-application-includes-a-handlers-section"></a>Podrzędne aplikacja zawiera \<obsługi\> sekcji

* **Przeglądarki:** błąd HTTP 500.19 — wewnętrzny błąd serwera

* **Dziennik aplikacji:** wpisu

* **Dziennik modułu platformy ASP.NET Core:** plik dziennika utworzony i pokazuje normalnego działania głównego aplikacji. Plik dziennika nie został utworzony dla aplikacji sub.

Rozwiązywanie problemów

* Upewnij się, że aplikacja sub *web.config* nie zawiera pliku `<handlers>` sekcji.

## <a name="application-configuration-general-issue"></a>Ogólne problem z konfiguracją aplikacji

* **Przeglądarki:** błąd HTTP 502.5 — błąd procesu

* **Dziennik aplikacji:** aplikacji "MACHINE/WEBROOT/APPHOST / {zestawu}" z fizyczny katalog główny "C:\\{PATH}\' utworzyć procesu z wiersza polecenia" "C:\\{PATH}\{zestawu}. { exe | dll} "" ale awaria lub odpowiedzi nie albo Nasłuchuj na danego portu "{PORT}", kod błędu = "0x800705b4"

* **Dziennik modułu platformy ASP.NET Core:** utworzony plik dziennika, ale pusty

Rozwiązywanie problemów

* Ten wyjątek ogólny wskazuje, że proces nie mógł uruchomić najprawdopodobniej ze względu na problem z konfiguracją aplikacji. Odwołanie do [struktury katalogów](xref:host-and-deploy/directory-structure), upewnij się, że aplikacja wdrożona pliki i foldery są odpowiednie i że pliki konfiguracji aplikacji są obecne i zawiera poprawne ustawienia dla aplikacji i środowiska. Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów](xref:host-and-deploy/iis/troubleshoot).
