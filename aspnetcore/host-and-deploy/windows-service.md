---
title: Host platformy ASP.NET Core w usłudze Windows
author: guardrex
description: Dowiedz się, jak udostępnić aplikację ASP.NET Core w usłudze Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/22/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: eedaf64710506f2a2aac65c178a9888d2ab33d38
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837484"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Host platformy ASP.NET Core w usłudze Windows

Przez [Luke Latham](https://github.com/guardrex) i [Tom Dykstra](https://github.com/tdykstra)

Aplikacji ASP.NET Core może być hostowana na Windows jako [usługi Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) bez korzystania z usług IIS. Po hostowany jako usługa Windows, aplikacja zostanie automatycznie uruchomiona po ponownym uruchomieniu.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="deployment-type"></a>Typ wdrożenia

Można utworzyć albo zależny od struktury lub niezależna Windows wdrożenia usługi. Aby uzyskać informacje i wskazówki dotyczące scenariuszy wdrażania, zobacz [wdrażanie aplikacji .NET Core](/dotnet/core/deploying/).

### <a name="framework-dependent-deployment"></a>Wdrożenie zależny od struktury

Zależny od struktury wdrożenia (stacje) zależy od obecności udostępnione całego systemu w wersji programu .NET Core w systemie docelowym. Stosowania w scenariuszu z stacje za pomocą aplikacji platformy ASP.NET Core Windows Service SDK tworzy plik wykonywalny (*\*.exe*), co jest nazywane *zależny od struktury pliku wykonywalnego*.

### <a name="self-contained-deployment"></a>Niezależne wdrożenia

Niezależne wdrożenia (— SCD) nie jest zależny od obecności składniki współużytkowane w systemie docelowym. Środowisko uruchomieniowe i zależności aplikacji są wdrażane za pomocą aplikacji do systemu macierzystego.

## <a name="convert-a-project-into-a-windows-service"></a>Konwertuj projekt w usłudze Windows

Do istniejącego projektu platformy ASP.NET Core do uruchomienia aplikacji jako usługi, należy wprowadzić następujące zmiany:

### <a name="project-file-updates"></a>Aktualizacje plików projektu

Na podstawie wybranego elementu [typu wdrożenia](#deployment-type), należy zaktualizować plik projektu:

#### <a name="framework-dependent-deployment-fdd"></a>Framework-dependent Deployment (FDD)

Dodaj Windows [identyfikator środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog) do `<PropertyGroup>` zawierający platformę docelową. W poniższym przykładzie ustawiono RID `win7-x64`. Dodaj `<SelfContained>` właściwością `false`. Te właściwości poinstruować zestawu SDK w celu wygenerowania pliku wykonywalnego (*.exe*) pliku Windows.

A *web.config* pliku, który zazwyczaj jest generowany podczas publikowania aplikacji ASP.NET Core, nie jest konieczne w przypadku aplikacji usługi Windows. Aby wyłączyć tworzenie *web.config* Dodaj `<IsTransformWebConfigDisabled>` właściwością `true`.

::: moniker range=">= aspnetcore-2.2"

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

Dodaj `<UseAppHost>` właściwością `true`. Ta właściwość zapewnia usługi ze ścieżką aktywacji (plik wykonywalny *.exe*) dla Dyskietki.

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

#### <a name="self-contained-deployment-scd"></a>Niezależne wdrożenia (— SCD)

Potwierdzić obecność Windows [identyfikator środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog) lub identyfikatorów RID, aby dodać `<PropertyGroup>` zawierający platformę docelową. Wyłączyć tworzenie *web.config* pliku, dodając `<IsTransformWebConfigDisabled>` właściwością `true`.

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Aby opublikować dla wielu identyfikatorów RID:

* Podaj identyfikatorów RID w liście rozdzielanej średnikami.
* Użyj nazwy właściwości `<RuntimeIdentifiers>` (w liczbie mnogiej).

  Aby uzyskać więcej informacji, zobacz [.NET Core RID katalogu](/dotnet/core/rid-catalog).

Dodaj odwołania do pakietu dla [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).

Aby włączyć rejestrowanie w dzienniku zdarzeń Windows, należy dodać odwołania do pakietu dla [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).

Aby uzyskać więcej informacji, zobacz [obsługi, uruchamianie i zatrzymywanie wydarzeń](#handle-starting-and-stopping-events) sekcji.

### <a name="programmain-updates"></a>Aktualizacje Program.Main

Wprowadź następujące zmiany w `Program.Main`:

* Do testowania i debugowania, gdy działają poza usługą, Dodaj kod, aby ustalić, czy aplikacja jest uruchomiona jako usługę lub aplikację konsoli. Sprawdź, czy jest dołączony debuger a `--console` argument wiersza polecenia jest obecny.

  Jeśli tych warunków jest spełniony, (aplikacja nie jest uruchamiana jako usługa), należy wywołać <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> na hoście w sieci Web.

  Jeśli warunki są fałszywe, (aplikacja jest uruchamiana jako usługa):

  * Wywołaj <xref:System.IO.Directory.SetCurrentDirectory*> i użyj ścieżki do lokalizacji publikowania aplikacji. Nie wywołuj <xref:System.IO.Directory.GetCurrentDirectory*> można uzyskać ścieżki, ponieważ aplikacji usługi Windows, która zwraca *C:\\WINDOWS\\system32* folderu podczas `GetCurrentDirectory` nosi nazwę. Aby uzyskać więcej informacji, zobacz [bieżący katalog i katalog główny zawartości](#current-directory-and-content-root) sekcji.
  * Wywołaj <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> do uruchomienia aplikacji jako usługi.

  Ponieważ [dostawcę konfiguracji wiersza polecenia](xref:fundamentals/configuration/index#command-line-configuration-provider) wymaga pary nazwa wartość dla argumentów wiersza poleceń `--console` przełącznik został usunięty z argumentów przed <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> otrzymuje je.

* Można zapisać w dzienniku zdarzeń Windows, należy dodać dostawcę dziennika zdarzeń <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>. Poziom rejestrowania za pomocą `Logging:LogLevel:Default` w *appsettings. Production.JSON* pliku. Pokaz i testowania pliku ustawień produkcji przykładową aplikację ustawia poziom rejestrowania `Information`. W środowisku produkcyjnym, wartość jest zazwyczaj równa `Error`. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/logging/index#windows-eventlog-provider>.

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

### <a name="publish-the-app"></a>Publikowanie aplikacji

Publikowanie aplikacji za pomocą [publikowania dotnet](/dotnet/articles/core/tools/dotnet-publish), [profilu publikowania w programie Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles), lub Visual Studio Code. Jeśli używasz programu Visual Studio, wybierz opcję **FolderProfile** i skonfigurować **lokalizacji docelowej** przed wybraniem **Publikuj** przycisku.

Aby opublikować przykładową aplikację przy użyciu narzędzi interfejsu wiersza polecenia (CLI), uruchom [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie w wierszu polecenia z folderu projektu z konfiguracją wydania, przekazana do [- c |--konfiguracji](/dotnet/core/tools/dotnet-publish#options)opcji. Użyj [-o |--dane wyjściowe](/dotnet/core/tools/dotnet-publish#options) opcji ze ścieżką do publikowania do folderu poza aplikacją.

#### <a name="publish-a-framework-dependent-deployment-fdd"></a>Publikowanie wdrożenia zależny od struktury (stacje)

W poniższym przykładzie aplikacja została opublikowana do *c:\\svc* folderu:

```console
dotnet publish --configuration Release --output c:\svc
```

#### <a name="publish-a-self-contained-deployment-scd"></a>Publikowanie niezależne wdrożenia (— SCD)

Identyfikator RID musi być określona w `<RuntimeIdenfifier>` (lub `<RuntimeIdentifiers>`) właściwości pliku projektu. Podaj środowisko uruchomieniowe [- r | — środowisko uruchomieniowe](/dotnet/core/tools/dotnet-publish#options) opcji `dotnet publish` polecenia.

W poniższym przykładzie aplikacja została opublikowana na potrzeby `win7-x64` środowiska uruchomieniowego *c:\\svc* folderu:

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

### <a name="create-a-user-account"></a>Utwórz konto użytkownika

Utwórz konto użytkownika dla usługi przy użyciu `net user` polecenia:

```console
net user {USER ACCOUNT} {PASSWORD} /add
```

Dla przykładowej aplikacji, należy utworzyć konto użytkownika o nazwie `ServiceUser` i hasła. W poniższym poleceniu zastąp `{PASSWORD}` z [silne hasło](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).

```console
net user ServiceUser {PASSWORD} /add
```

Jeśli potrzebujesz dodać użytkownika do grupy, użyj `net localgroup` polecenie, gdzie `{GROUP}` to nazwa grupy:

```console
net localgroup {GROUP} {USER ACCOUNT} /add
```

Aby uzyskać więcej informacji, zobacz [kont użytkowników usług](/windows/desktop/services/service-user-accounts).

### <a name="set-permissions"></a>Ustawianie uprawnień

Udzielanie zapisu/odczytu/wykonania dostępu do folderu aplikacji przy użyciu [icacls](/windows-server/administration/windows-commands/icacls) polecenia:

```console
icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
```

* `{PATH}` &ndash; Ścieżka do folderu aplikacji.
* `{USER ACCOUNT}` &ndash; Konto użytkownika (SID).
* `(OI)` &ndash; Obiekt dziedziczenia flagi propaguje uprawnienia do podrzędnych plików.
* `(CI)` &ndash; Flaga Dziedziczenie kontenera propaguje uprawnienia do folderów podrzędnych.
* `{PERMISSION FLAGS}` &ndash; Ustawia uprawnienia dostępu do aplikacji.
  * Zapis (`W`)
  * Odczyt (`R`)
  * Wykonaj (`X`)
  * Pełne (`F`)
  * Modyfikowanie (`M`)
* `/t` &ndash; Rekursywnie dotyczą plików i folderów podrzędnych istniejących.

Dla przykładowej aplikacji opublikowany *c:\\svc* folder i `ServiceUser` konto z uprawnieniami do zapisu/odczytu/wykonania, użyj następującego polecenia:

```console
icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
```

Aby uzyskać więcej informacji, zobacz [icacls](/windows-server/administration/windows-commands/icacls).

## <a name="manage-the-service"></a>Zarządzanie usługą

### <a name="create-the-service"></a>Tworzenie usługi

Użyj [sc.exe](https://technet.microsoft.com/library/bb490995) narzędzie wiersza polecenia, aby utworzyć usługę. `binPath` Wartość jest ścieżką do pliku wykonywalnego aplikacji, która zawiera nazwę pliku wykonywalnego. **Odstęp między równości i znaku cudzysłowu każdego parametru i wartość jest wymagana.**

```console
sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
```

* `{SERVICE NAME}` &ndash; Nazwa do przypisania do usługi w [Menedżera sterowania usługami](/windows/desktop/services/service-control-manager).
* `{PATH}` &ndash; Ścieżka do pliku wykonywalnego usługi.
* `{DOMAIN}` &ndash; Domena komputerze przyłączonym do domeny. Jeśli komputer nie jest przyłączone do domeny, nazwa komputera lokalnego.
* `{USER ACCOUNT}` &ndash; Konto użytkownika, pod którym działa usługa.
* `{PASSWORD}` &ndash; Hasło konta użytkownika.

> [!WARNING]
> Czy **nie** pominąć `obj` parametru. Wartością domyślną dla `obj` jest [konta LocalSystem](/windows/desktop/services/localsystem-account) konta. Uruchamianie usługi w obszarze `LocalSystem` konto stanowi znaczące zagrożenie bezpieczeństwa. Usługi są zawsze uruchamiane przy użyciu konta użytkownika, które ma ograniczone uprawnienia.

W poniższym przykładzie przykładowej aplikacji:

* Usługa jest o nazwie **Moja_usługa**.
* Opublikowana usługa znajduje się w *c:\\svc* folderu. Nosi nazwę pliku wykonywalnego aplikacji *SampleApp.exe*. Ujmij `binPath` wartość w znaki cudzysłowu (").
* Usługa jest uruchamiana w ramach `ServiceUser` konta. Zastąp `{DOMAIN}` przy użyciu konta użytkownika domeny lub nazwy komputera lokalnego. Ujmij `obj` wartość w znaki cudzysłowu ("). Przykład: W przypadku hostowania systemu komputera lokalnego, o nazwie `MairaPC`ustaw `obj` do `"MairaPC\ServiceUser"`.
* Zastąp `{PASSWORD}` przy użyciu hasła konta użytkownika. Ujmij `password` wartość w znaki cudzysłowu (").

```console
sc create MyService binPath= "c:\svc\sampleapp.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
```

> [!IMPORTANT]
> Upewnij się, że istnieją spacji między znakami równości parametrów i wartości parametrów.

### <a name="start-the-service"></a>Uruchom usługę

Uruchom usługę za pomocą `sc start {SERVICE NAME}` polecenia.

Aby uruchomić usługę aplikacji przykładowej, użyj następującego polecenia:

```console
sc start MyService
```

Polecenie zajmuje kilka sekund, aby uruchomić usługę.

### <a name="determine-the-service-status"></a>Sprawdź stan usługi

Aby sprawdzić stan usługi, użyj `sc query {SERVICE NAME}` polecenia. Stan jest zgłaszany jako jeden z następujących wartości:

* `START_PENDING`
* `RUNNING`
* `STOP_PENDING`
* `STOPPED`

Użyj następującego polecenia, aby sprawdzić stan usługi aplikacji przykładowej:

```console
sc query MyService
```

### <a name="browse-a-web-app-service"></a>Przeglądaj, usługi aplikacji sieci web

Kiedy usługa jest w `RUNNING` stanu i usługi w przypadku aplikacji sieci web, przeglądanie aplikacji w ścieżce (domyślnie `http://localhost:5000`, który przekierowuje do `https://localhost:5001` przy użyciu [HTTPS przekierowanie w oprogramowaniu pośredniczącym](xref:security/enforcing-ssl)).

Usługa app service przykładowego, można przeglądać w tej aplikacji w `http://localhost:5000`.

### <a name="stop-the-service"></a>Zatrzymaj usługę

Zatrzymaj usługę za pomocą `sc stop {SERVICE NAME}` polecenia.

Następujące polecenie zatrzymuje usługę aplikacji przykładowej:

```console
sc stop MyService
```

### <a name="delete-the-service"></a>Usuń usługę

Po krótkiej chwili zatrzymania usługi, odinstaluj usługę za pomocą `sc delete {SERVICE NAME}` polecenia.

Sprawdź stan usługi aplikacji przykładowej:

```console
sc query MyService
```

Gdy usługa app service przykładowego jest w `STOPPED` stanu, użyj następującego polecenia, aby odinstalować usługę aplikacji przykładowej:

```console
sc delete MyService
```

## <a name="handle-starting-and-stopping-events"></a>Obsługa uruchamianie i zatrzymywanie wydarzeń

Aby obsłużyć <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, i <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> zdarzeń, wykonaj następujące dodatkowe zmiany:

1. Utwórz klasę, która pochodzi od klasy <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> z `OnStarting`, `OnStarted`, i `OnStopping` metody:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. Tworzenie metody rozszerzenia dla <xref:Microsoft.AspNetCore.Hosting.IWebHost> , który przekazuje `CustomWebHostService` do <xref:System.ServiceProcess.ServiceBase.Run*>:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. W `Program.Main`, wywołaj `RunAsCustomService` rozszerzenia zamiast metody <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:

   ```csharp
   host.RunAsCustomService();
   ```

   Aby wyświetlić lokalizację <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> w `Program.Main`, można znaleźć w przykładzie kodu pokazano w [konwertować projekt w usłudze Windows](#convert-a-project-into-a-windows-service) sekcji.

## <a name="proxy-server-and-load-balancer-scenarios"></a>Serwer proxy i scenariuszy usługi równoważenia obciążenia

Usługi wchodzić w interakcje z żądaniami z Internetu lub sieci firmowej i znajdują się za serwerem proxy lub moduł równoważenia obciążenia, które mogą wymagać dodatkowej konfiguracji. Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>Konfigurowanie protokołu HTTPS

Aby skonfigurować usługę z bezpiecznego punktu końcowego:

1. Utwórz certyfikat X.509 dla systemu macierzystego przy użyciu pozyskiwania certyfikatu używanej platformy i mechanizmy wdrażania.

1. Określ [konfiguracji punktu końcowego HTTPS serwera Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) przy użyciu certyfikatu.

Użycie certyfikatu deweloperskiego platformy ASP.NET Core HTTPS, aby zabezpieczyć punkt końcowy usługi nie jest obsługiwane.

## <a name="current-directory-and-content-root"></a>Bieżący katalog i katalog główny zawartości

Bieżący katalog roboczy zwracany przez wywołanie metody <xref:System.IO.Directory.GetCurrentDirectory*> dla usługi Windows jest *C:\\WINDOWS\\system32* folderu. *System32* folder nie jest odpowiednią lokalizację do przechowywania plików usługi (na przykład pliki ustawień). Użyj jednej z następujących metod do utrzymywania i uzyskać dostęp do zasobów i plików ustawień usługi.

### <a name="set-the-content-root-path-to-the-apps-folder"></a>Ustaw ścieżkę katalogu głównego zawartości do folderu aplikacji

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> Tej samej ścieżki, które są udostępniane `binPath` argumentów podczas tworzenia usługi. Zamiast wywoływać metodę `GetCurrentDirectory` do tworzenia ścieżek do plików ustawień, należy wywołać <xref:System.IO.Directory.SetCurrentDirectory*> ze ścieżką do katalogu głównego zawartości aplikacji.

W `Program.Main`, określić ścieżkę do folderu pliku wykonywalnego usługi i ustanowić zawartości katalogu głównego aplikacji za pomocą ścieżki:

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a>Store usługi plików w odpowiedniej lokalizacji na dysku

Określ ścieżkę bezwzględną z <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> przy użyciu <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> do folderu zawierającego pliki.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Konfiguracja punktu końcowego kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (w tym konfiguracja protokołu HTTPS i obsługa SNI)
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
