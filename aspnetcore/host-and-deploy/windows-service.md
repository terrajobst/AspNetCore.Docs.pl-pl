---
title: Host platformy ASP.NET Core w usłudze Windows
author: guardrex
description: Dowiedz się, jak udostępnić aplikację ASP.NET Core w usłudze Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/30/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 11913019bfe5d06c259b806fce9cc580a8280ad5
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758196"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Host platformy ASP.NET Core w usłudze Windows

Przez [Luke Latham](https://github.com/guardrex) i [Tom Dykstra](https://github.com/tdykstra)

Aplikacji ASP.NET Core może być hostowana na Windows bez korzystania z usług IIS jako [usługi Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Po hostowany jako usługa Windows, aplikacja zostanie automatycznie uruchomiona po ponownym uruchomieniu.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="convert-a-project-into-a-windows-service"></a>Konwertuj projekt w usłudze Windows

Następujące minimalne zmiany są wymagane do skonfigurowania istniejący projekt ASP.NET Core, można uruchomić jako usługę:

1. W pliku projektu:

   * Potwierdzić obecność Windows [identyfikator środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog) lub dodać ją do `<PropertyGroup>` zawierający platforma docelowa:

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.2</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      Aby opublikować dla wielu identyfikatorów RID:

      * Podaj identyfikatorów RID w liście rozdzielanej średnikami.
      * Użyj nazwy właściwości `<RuntimeIdentifiers>` (w liczbie mnogiej).

      Aby uzyskać więcej informacji, zobacz [.NET Core RID katalogu](/dotnet/core/rid-catalog).

   * Dodaj odwołania do pakietu dla [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).

1. Wprowadź następujące zmiany w `Program.Main`:

   * Wywołaj [hosta. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) zamiast `host.Run`.

   * Wywołaj [UseContentRoot](xref:fundamentals/host/web-host#content-root) i ścieżka do aplikacji — opublikowane lokalizacji zamiast używania `Directory.GetCurrentDirectory()`.

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

1. Publikowanie aplikacji za pomocą [publikowania dotnet](/dotnet/articles/core/tools/dotnet-publish), [profilu publikowania w programie Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles), lub Visual Studio Code. Jeśli używasz programu Visual Studio, wybierz opcję **FolderProfile** i skonfigurować **lokalizacji docelowej** przed wybraniem **Publikuj** przycisku.

   Aby opublikować przykładową aplikację przy użyciu narzędzi interfejsu wiersza polecenia (CLI), uruchom [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie w wierszu polecenia z folderu projektu. Identyfikator RID musi być określona w `<RuntimeIdenfifier>` (lub `<RuntimeIdentifiers>`) właściwości pliku projektu. W poniższym przykładzie aplikacja zostanie opublikowana w konfiguracji wydania dla `win7-x64` środowiska uruchomieniowego, aby utworzyć folder w *c:\\svc*:

   ```console
   dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
   ```

1. Utwórz konto użytkownika dla usługi przy użyciu `net user` polecenia:

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

1. Udzielanie zapisu/odczytu/wykonania dostępu do folderu aplikacji przy użyciu [icacls](/windows-server/administration/windows-commands/icacls) polecenia:

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

1. Użyj [sc.exe](https://technet.microsoft.com/library/bb490995) narzędzie wiersza polecenia, aby utworzyć usługę. `binPath` Wartość jest ścieżką do pliku wykonywalnego aplikacji, która zawiera nazwę pliku wykonywalnego. **Odstęp między równości i znaku cudzysłowu każdego parametru i wartość jest wymagana.**

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * `{SERVICE NAME}` &ndash; Nazwa do przypisania do usługi w [Menedżera sterowania usługami](/windows/desktop/services/service-control-manager).
   * `{PATH}` &ndash; Ścieżka do pliku wykonywalnego usługi.
   * `{DOMAIN}` (lub jeśli komputer nie jest domeny dołączonych, nazwy komputera lokalnego) i `{USER ACCOUNT}` &ndash; domeny (lub nazwy komputera lokalnego) i konto użytkownika w ramach którego działa usługa. Czy **nie** pominąć `obj` parametru. Wartością domyślną dla `obj` jest [konta LocalSystem](/windows/desktop/services/localsystem-account) konta. Uruchamianie usługi w obszarze `LocalSystem` konto stanowi znaczące zagrożenie bezpieczeństwa. Zawsze uruchamiaj usługi przy użyciu konta użytkownika z ograniczonymi uprawnieniami na serwerze.
   * `{PASSWORD}` &ndash; Hasło konta użytkownika.

   W poniższym przykładzie:

   * Usługa jest o nazwie **Moja_usługa**.
   * Opublikowana usługa znajduje się w *c:\\svc* folderu. Nosi nazwę pliku wykonywalnego aplikacji *AspNetCoreService.exe*. `binPath` Wartość jest ujęta w cudzysłowy proste (").
   * Usługa jest uruchamiana w ramach `ServiceUser` konta. Zastąp `{DOMAIN}` przy użyciu konta użytkownika domeny lub nazwy komputera lokalnego. Ujmij `obj` wartość w cudzysłowy proste ("). Przykład: W przypadku hostowania systemu komputera lokalnego, o nazwie `MairaPC`ustaw `obj` do `"MairaPC\ServiceUser"`.
   * Zastąp `{PASSWORD}` przy użyciu hasła konta użytkownika. `password` Wartość jest ujęta w cudzysłowy proste (").

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > Upewnij się, że istnieją spacji między znakami równości parametrów i wartości parametrów.

1. Uruchom usługę za pomocą `sc start {SERVICE NAME}` polecenia.

   Aby uruchomić usługę aplikacji przykładowej, użyj następującego polecenia:

   ```console
   sc start MyService
   ```

   Polecenie zajmuje kilka sekund, aby uruchomić usługę.

1. Aby sprawdzić stan usługi, użyj `sc query {SERVICE NAME}` polecenia. Stan jest zgłaszany jako jeden z następujących wartości:

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   Użyj następującego polecenia, aby sprawdzić stan usługi aplikacji przykładowej:

   ```console
   sc query MyService
   ```

1. Kiedy usługa jest w `RUNNING` stanu i usługi w przypadku aplikacji sieci web, przeglądanie aplikacji w ścieżce (domyślnie `http://localhost:5000`, który przekierowuje do `https://localhost:5001` przy użyciu [HTTPS przekierowanie w oprogramowaniu pośredniczącym](xref:security/enforcing-ssl)).

   Usługa app service przykładowego, można przeglądać w tej aplikacji w `http://localhost:5000`.

1. Zatrzymaj usługę za pomocą `sc stop {SERVICE NAME}` polecenia.

   Następujące polecenie zatrzymuje usługę aplikacji przykładowej:

   ```console
   sc stop MyService
   ```

1. Po krótkiej chwili zatrzymania usługi, odinstaluj usługę za pomocą `sc delete {SERVICE NAME}` polecenia.

   Sprawdź stan usługi aplikacji przykładowej:

   ```console
   sc query MyService
   ```

   Gdy usługa app service przykładowego jest w `STOPPED` stanu, użyj następującego polecenia, aby odinstalować usługę aplikacji przykładowej:

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a>Uruchom aplikację poza usługą

Znacznie łatwiej testować i debugować podczas uruchamiania spoza niej, więc zwyczajowego, aby dodać kod, który wywołuje `RunAsService` tylko pod pewnymi warunkami. Na przykład aplikacja może działać jako aplikacji konsoli, za pomocą `--console` argument wiersza polecenia lub jeśli jest dołączony debuger:

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

Ponieważ konfiguracja platformy ASP.NET Core wymaga pary nazwa wartość dla argumentów wiersza polecenia `--console` przełącznik został usunięty, zanim argumenty są przekazywane do [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

> [!NOTE]
> `isService` nie jest przekazywane z `Main` do `CreateWebHostBuilder` ponieważ podpis `CreateWebHostBuilder` musi być `CreateWebHostBuilder(string[])` aby [Testowanie integracji](xref:test/integration-tests) działało poprawnie.

## <a name="handle-stopping-and-starting-events"></a>Obsługa zatrzymanie i uruchomienie zdarzenia

Aby obsłużyć [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), i [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) zdarzenia, należy wprowadzić następujące zmiany dodatkowe:

1. Utwórz klasę, która pochodzi od klasy [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. Tworzenie metody rozszerzenia dla [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) niestandardowego, który przekazuje `WebHostService` do [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. W `Program.Main`, wywołanie nowej metody rozszerzenia `RunAsCustomService`, zamiast [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > `isService` nie jest przekazywane z `Main` do `CreateWebHostBuilder` ponieważ podpis `CreateWebHostBuilder` musi być `CreateWebHostBuilder(string[])` aby [Testowanie integracji](xref:test/integration-tests) działało poprawnie.

Jeśli niestandardowa `WebHostService` kod wymaga usługi z wstrzykiwanie zależności (np. Rejestrator), Uzyskaj ją z [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) właściwości:

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Serwer proxy i scenariuszy usługi równoważenia obciążenia

Usługi wchodzić w interakcje z żądaniami z Internetu lub sieci firmowej i znajdują się za serwerem proxy lub moduł równoważenia obciążenia, które mogą wymagać dodatkowej konfiguracji. Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>Konfigurowanie protokołu HTTPS

Aby skonfigurować usługę z bezpiecznego punktu końcowego:

1. Utwórz certyfikat X.509 dla systemu macierzystego przy użyciu pozyskiwania certyfikatu używanej platformy i mechanizmy wdrażania.
1. Określ [konfiguracji punktu końcowego HTTPS serwera Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) przy użyciu certyfikatu.

Użycie certyfikatu deweloperskiego platformy ASP.NET Core HTTPS, aby zabezpieczyć punkt końcowy usługi nie jest obsługiwane.

## <a name="current-directory-and-content-root"></a>Bieżący katalog i katalog główny zawartości

Bieżący katalog roboczy zwracany przez wywołanie metody `Directory.GetCurrentDirectory()` dla usługi Windows jest *C:\\WINDOWS\\system32* folderu. *System32* folder nie jest odpowiednią lokalizację do przechowywania plików usługi (na przykład pliki ustawień). Użyj jednej z następujących metod do utrzymywania i uzyskać dostęp do zasobów i plików ustawień za pomocą usługi [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) przy użyciu [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):

* Użyj ścieżki katalogu głównego zawartości. `IHostingEnvironment.ContentRootPath` Tej samej ścieżki, które są udostępniane `binPath` argumentów podczas tworzenia usługi. Zamiast używania `Directory.GetCurrentDirectory()` Tworzenie ścieżki do plików ustawień, użyj ścieżki katalogu głównego zawartości i przechowuje pliki w katalogu głównym zawartości aplikacji.
* Store pliki w odpowiedniej lokalizacji na dysku. Określ ścieżkę bezwzględną z `SetBasePath` do folderu zawierającego pliki.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Konfiguracja punktu końcowego kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (w tym konfiguracja protokołu HTTPS i obsługa SNI)
* <xref:fundamentals/host/web-host>
