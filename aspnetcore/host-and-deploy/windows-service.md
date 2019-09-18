---
title: ASP.NET Core hosta w usłudze systemu Windows
author: guardrex
description: Dowiedz się, jak hostować aplikację ASP.NET Core w usłudze systemu Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/09/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 995fdd2bbba30ff983bc2055fcb97c14541e2ac6
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081483"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>ASP.NET Core hosta w usłudze systemu Windows

Autorzy [Luke Latham](https://github.com/guardrex) i [Tomasz Dykstra](https://github.com/tdykstra)

Aplikacja ASP.NET Core może być hostowana w systemie Windows jako [Usługa systemu Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) bez korzystania z usług IIS. Gdy usługa jest hostowana w systemie Windows, aplikacja jest uruchamiana automatycznie po ponownym uruchomieniu serwera.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

* [ASP.NET Core zestaw SDK 2,1 lub nowszy](https://dotnet.microsoft.com/download)
* [Program PowerShell 6,2 lub nowszy](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a>Szablon usługi procesu roboczego

Szablon usługi ASP.NET Core Worker zapewnia punkt początkowy do pisania długotrwałych aplikacji usługi. Aby użyć szablonu jako podstawy dla aplikacji usługi systemu Windows:

1. Utwórz aplikację usługi procesu roboczego na podstawie szablonu .NET Core.
1. Postępuj zgodnie ze wskazówkami w sekcji [Konfiguracja aplikacji](#app-configuration) , aby zaktualizować aplikację usługi procesu roboczego tak, aby mogła ona działać jako usługa systemu Windows.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Utwórz nowy projekt.
1. Wybierz **aplikacji sieci Web platformy ASP.NET Core**. Wybierz opcję **Dalej**.
1. Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu. Wybierz pozycję **Utwórz**.
1. W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 3,0** .
1. Wybierz szablon **usługi procesu roboczego** . Wybierz pozycję **Utwórz**.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Użyj szablonu usługi procesu roboczego`worker`() z poleceniem [dotnet New](/dotnet/core/tools/dotnet-new) z powłoki poleceń. W poniższym przykładzie jest tworzona aplikacja usługi Worker o nazwie `ContosoWorkerService`. Folder dla `ContosoWorkerService` aplikacji jest tworzony automatycznie podczas wykonywania polecenia.

```dotnetcli
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

## <a name="app-configuration"></a>Konfiguracja aplikacji

::: moniker range=">= aspnetcore-3.0"

`IHostBuilder.UseWindowsService`dostarczone przez pakiet [Microsoft. Extensions. hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) jest wywoływany podczas kompilowania hosta. Jeśli aplikacja działa jako usługa systemu Windows, Metoda:

* Ustawia okres istnienia hosta `WindowsServiceLifetime`na.
* Ustawia katalog główny zawartości.
* Umożliwia rejestrowanie w dzienniku zdarzeń przy użyciu nazwy aplikacji jako domyślnej nazwy źródła.
  * Poziom dziennika można skonfigurować przy użyciu `Logging:LogLevel:Default` klucza w pliku *appSettings. Plik Product. JSON* .
  * Tylko Administratorzy mogą tworzyć nowe źródła zdarzeń. Gdy nie można utworzyć źródła zdarzeń przy użyciu nazwy aplikacji, w źródle *aplikacji* jest rejestrowane ostrzeżenie, a dzienniki zdarzeń są wyłączone.

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Aplikacja wymaga odwołania do pakietów dla elementu [Microsoft. AspNetCore. hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) i [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).

Aby przetestować i debugować w przypadku uruchamiania poza usługą, należy dodać kod, aby określić, czy aplikacja działa jako usługa, czy Aplikacja konsolowa. Sprawdź, czy debuger jest dołączony lub czy `--console` jest obecny przełącznik. Jeśli którykolwiek z warunków jest spełniony (aplikacja nie jest uruchomiona jako usługa), wywołaj metodę <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>. Jeśli warunki są fałszywe (aplikacja jest uruchamiana jako usługa):

* Wywołaj <xref:System.IO.Directory.SetCurrentDirectory*> i użyj ścieżki do opublikowanej lokalizacji aplikacji. Nie wywołuj <xref:System.IO.Directory.GetCurrentDirectory*> , aby uzyskać ścieżkę, ponieważ aplikacja usługi systemu Windows zwraca folder *C\\:\\Windows system32* , <xref:System.IO.Directory.GetCurrentDirectory*> gdy jest wywoływana. Aby uzyskać więcej informacji, zapoznaj się z sekcją [Current Directory i root Content](#current-directory-and-content-root) . Ten krok jest wykonywany przed skonfigurowaniem aplikacji w programie `CreateWebHostBuilder`.
* Wywołaj <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> , aby uruchomić aplikację jako usługę.

Ponieważ [dostawca konfiguracji wiersza polecenia](xref:fundamentals/configuration/index#command-line-configuration-provider) wymaga par nazwa-wartość dla argumentów wiersza polecenia, `--console` przełącznik jest usuwany z argumentów przed <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> odebraniem argumentów.

Aby zapisać w dzienniku zdarzeń systemu Windows, Dodaj dostawcę EventLog do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>programu. Ustaw poziom rejestrowania przy użyciu `Logging:LogLevel:Default` klucza w pliku *appSettings. Plik Product. JSON* .

W poniższym przykładzie z przykładowej aplikacji `RunAsCustomService` jest wywoływana <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> zamiast w celu obsługi zdarzeń okresu istnienia w aplikacji. Aby uzyskać więcej informacji, zobacz sekcję [Obsługa zdarzeń dotyczących uruchamiania i zatrzymywania](#handle-starting-and-stopping-events) .

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a>Typ wdrożenia

Aby uzyskać informacje i porady dotyczące scenariuszy wdrażania, zobacz [wdrażanie aplikacji .NET Core](/dotnet/core/deploying/).

### <a name="framework-dependent-deployment-fdd"></a>Wdrożenie zależne od platformy (FDD)

Wdrożenie zależne od platformy (FDD) zależy od obecności udostępnionej systemowej wersji platformy .NET Core w systemie docelowym. Po przyjęciu scenariusza FDD zgodnie ze wskazówkami zawartymi w tym artykule zestaw SDK tworzy plik wykonywalny ( *. exe*), nazywany *plik wykonywalny zależny od platformy*.

::: moniker range=">= aspnetcore-3.0"

Dodaj następujące elementy właściwości do pliku projektu:

* `<OutputType>`Typ danych wyjściowych aplikacji (`Exe` dla pliku wykonywalnego). &ndash;
* `<LangVersion>`Wersja językowa (`latest` lub`preview`). &ndash; C#

Plik *Web. config* , który jest zwykle tworzony podczas publikowania aplikacji ASP.NET Core, nie jest konieczny dla aplikacji usług systemu Windows. Aby wyłączyć tworzenie pliku *Web. config* , należy dodać `<IsTransformWebConfigDisabled>` Właściwość ustawioną na `true`.

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <OutputType>Exe</OutputType>
  <LangVersion>preview</LangVersion>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[Identyfikator środowiska uruchomieniowego systemu Windows (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)) zawiera platformę docelową. W poniższym przykładzie identyfikator RID jest ustawiony na `win7-x64`. Właściwość jest ustawiona na `false`. `<SelfContained>` Te właściwości instruują zestaw SDK, aby wygenerował plik wykonywalny (*exe*) dla systemu Windows i aplikację, która zależy od współużytkowanej platformy .NET Core.

Plik *Web. config* , który jest zwykle tworzony podczas publikowania aplikacji ASP.NET Core, nie jest konieczny dla aplikacji usług systemu Windows. Aby wyłączyć tworzenie pliku *Web. config* , należy dodać `<IsTransformWebConfigDisabled>` Właściwość ustawioną na `true`.

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

[Identyfikator środowiska uruchomieniowego systemu Windows (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)) zawiera platformę docelową. W poniższym przykładzie identyfikator RID jest ustawiony na `win7-x64`. Właściwość jest ustawiona na `false`. `<SelfContained>` Te właściwości instruują zestaw SDK, aby wygenerował plik wykonywalny (*exe*) dla systemu Windows i aplikację, która zależy od współużytkowanej platformy .NET Core.

Właściwość jest ustawiona na `true`. `<UseAppHost>` Ta właściwość zapewnia usługę ze ścieżką aktywacji (plik wykonywalny, *exe*) dla FDD.

Plik *Web. config* , który jest zwykle tworzony podczas publikowania aplikacji ASP.NET Core, nie jest konieczny dla aplikacji usług systemu Windows. Aby wyłączyć tworzenie pliku *Web. config* , należy dodać `<IsTransformWebConfigDisabled>` Właściwość ustawioną na `true`.

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

### <a name="self-contained-deployment-scd"></a>Wdrażanie samodzielne (SCD)

Wdrożenie samodzielne (SCD) nie polega na obecności struktury udostępnionej w systemie hosta. Środowisko uruchomieniowe i zależności aplikacji są wdrażane przy użyciu aplikacji.

[Identyfikator środowiska uruchomieniowego systemu Windows (RID)](/dotnet/core/rid-catalog) znajduje się `<PropertyGroup>` w, który zawiera platformę docelową:

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

Aby opublikować dla wielu identyfikatorów RID:

* Podaj identyfikatory RID na liście rozdzielanej średnikami.
* Użyj nazwy [ \<właściwości RuntimeIdentifiers >](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).

Aby uzyskać więcej informacji, zobacz [katalog .NET Core RID Catalog](/dotnet/core/rid-catalog).

::: moniker range="< aspnetcore-3.0"

Właściwość jest ustawiona na `true`: `<SelfContained>`

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a>Konto użytkownika usługi

Aby utworzyć konto użytkownika dla usługi, należy użyć polecenia cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) w powłoce poleceń administracyjnych programu PowerShell 6.

W systemie Windows 10 października 2018 Update (wersja 1809/Kompilacja 10.0.17763) lub nowsza:

```PowerShell
New-LocalUser -Name {NAME}
```

W systemie operacyjnym Windows w wersji starszej niż aktualizacja 2018 systemu Windows 10 października (wersja 1809/Kompilacja 10.0.17763):

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

Podaj [silne hasło](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) po wyświetleniu monitu.

Jeśli parametr nie zostanie dostarczony do polecenia cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) z wygaśnięciem <xref:System.DateTime>, konto nie wygaśnie. `-AccountExpires`

Aby uzyskać więcej informacji, zobacz [Microsoft. PowerShell. LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) i [konta użytkowników usług](/windows/desktop/services/service-user-accounts).

Alternatywna metoda zarządzania użytkownikami podczas korzystania z Active Directory polega na użyciu zarządzanych kont usług. Aby uzyskać więcej informacji, zobacz [Omówienie kont usług zarządzanych przez grupę](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).

## <a name="log-on-as-a-service-rights"></a>Logowanie jako prawa usługi

Aby nawiązać *Logowanie jako prawa usługi* dla konta użytkownika usługi:

1. Otwórz Edytor lokalnych zasad zabezpieczeń, uruchamiając program *secpol. msc*.
1. Rozwiń węzeł **Zasady lokalne** , a następnie wybierz pozycję **Przypisywanie praw użytkownika**.
1. Otwórz okno **Logowanie jako usługa** .
1. Wybierz pozycję **Dodaj użytkownika lub grupę**.
1. Podaj nazwę obiektu (konto użytkownika) przy użyciu jednej z następujących metod:
   1. Wpisz konto użytkownika (`{DOMAIN OR COMPUTER NAME\USER}`) w polu Nazwa obiektu, a następnie wybierz **przycisk OK** , aby dodać użytkownika do zasad.
   1. Wybierz pozycję **Zaawansowane**. Wybierz pozycję **Znajdź teraz**. Wybierz z listy konto użytkownika. Kliknij przycisk **OK**. Ponownie wybierz **przycisk OK** , aby dodać użytkownika do zasad.
1. Wybierz **przycisk OK** lub **Zastosuj** , aby zaakceptować zmiany.

## <a name="create-and-manage-the-windows-service"></a>Tworzenie usługi systemu Windows i zarządzanie nią

### <a name="create-a-service"></a>Tworzenie usługi

Zarejestrowanie usługi za pomocą poleceń programu PowerShell. Z poziomu powłoki poleceń administracyjnych programu PowerShell 6 wykonaj następujące polecenia:

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* `{EXE PATH}`Ścieżka do folderu aplikacji na hoście (na `d:\myservice`przykład). &ndash; Nie dołączaj pliku wykonywalnego aplikacji do ścieżki. Końcowy ukośnik nie jest wymagany.
* `{DOMAIN OR COMPUTER NAME\USER}`Konto użytkownika usługi (na `Contoso\ServiceUser`przykład). &ndash;
* `{NAME}`Nazwa usługi (na `MyService`przykład). &ndash;
* `{EXE FILE PATH}`Ścieżka pliku wykonywalnego aplikacji (na `d:\myservice\myservice.exe`przykład). &ndash; Uwzględnij nazwę pliku wykonywalnego z rozszerzeniem.
* `{DESCRIPTION}`Opis usługi (na `My sample service`przykład). &ndash;
* `{DISPLAY NAME}`Nazwa wyświetlana usługi (na `My Service`przykład). &ndash;

### <a name="start-a-service"></a>Uruchom usługę

Uruchom usługę za pomocą następującego polecenia programu PowerShell 6:

```powershell
Start-Service -Name {NAME}
```

Uruchomienie usługi może potrwać kilka sekund.

### <a name="determine-a-services-status"></a>Określanie stanu usługi

Aby sprawdzić stan usługi, użyj następującego polecenia programu PowerShell 6:

```powershell
Get-Service -Name {NAME}
```

Stan jest raportowany jako jedna z następujących wartości:

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a>Zatrzymaj usługę

Zatrzymaj usługę za pomocą następującego polecenia programu PowerShell 6:

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a>Usuwanie usługi

Po krótkim opóźnieniu na zatrzymanie usługi Usuń usługę za pomocą następującego polecenia programu PowerShell 6:

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a>Obsługa zdarzeń uruchamiania i zatrzymywania

Aby obsłużyć <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>zdarzenia <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> , <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>i:

1. Utwórz klasę, która dziedziczy z <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> `OnStarting`metody, `OnStarted`, i `OnStopping` :

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. Utwórz metodę rozszerzenia dla <xref:Microsoft.AspNetCore.Hosting.IWebHost> , która `CustomWebHostService` przekazuje do <xref:System.ServiceProcess.ServiceBase.Run*>:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. W `Program.Main` <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> ,`RunAsCustomService` Wywołaj metodę rozszerzenia zamiast:

   ```csharp
   host.RunAsCustomService();
   ```

   Aby zobaczyć lokalizację <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> programu w programie `Program.Main`, zapoznaj się z przykładem kodu pokazanym w sekcji [typ wdrożenia](#deployment-type) .

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Serwer proxy i scenariuszy usługi równoważenia obciążenia

Usługi, które współdziałają z żądaniami z Internetu lub sieci firmowej i znajdują się za serwerem proxy lub modułem równoważenia obciążenia, mogą wymagać dodatkowej konfiguracji. Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-endpoints"></a>Konfigurowanie punktów końcowych

Domyślnie ASP.NET Core wiąże się z `http://localhost:5000`. Skonfiguruj adres URL i port przez ustawienie `ASPNETCORE_URLS` zmiennej środowiskowej.

Dodatkowe podejścia do konfiguracji adresów URL i portów, w tym obsługa punktów końcowych HTTPS, można znaleźć w następujących tematach:

* <xref:fundamentals/servers/kestrel#endpoint-configuration>Kestrel
* <xref:fundamentals/servers/httpsys#configure-windows-server>(HTTP. sys)

> [!NOTE]
> Korzystanie z ASP.NET Core certyfikatu deweloperskiego HTTPS w celu zabezpieczenia punktu końcowego usługi nie jest obsługiwane.

## <a name="current-directory-and-content-root"></a>Bieżący katalog i główna Zawartość

Bieżącym katalogiem roboczym zwróconym <xref:System.IO.Directory.GetCurrentDirectory*> przez wywołanie dla usługi systemu Windows jest folder *\\C\\: Windows system32* . Folder *system32* nie jest odpowiednią lokalizacją do przechowywania plików usługi (na przykład plików ustawień). Użyj jednego z poniższych metod, aby zachować i uzyskać dostęp do plików ustawień i zasobów usługi.

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a>Użyj ContentRootPath lub ContentRootFileProvider

Użyj [IHostEnvironment. ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) lub <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> do lokalizowania zasobów aplikacji.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a>Ustawianie ścieżki katalogu głównego zawartości do folderu aplikacji

Jest to ta sama ścieżka `binPath` do argumentu podczas tworzenia usługi. <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> Zamiast wywoływania `GetCurrentDirectory` funkcji tworzenia ścieżek do plików ustawień, należy wywołać <xref:System.IO.Directory.SetCurrentDirectory*> ścieżkę do katalogu głównego zawartości aplikacji.

W `Program.Main`programie określ ścieżkę do folderu wykonywalnego usługi i użyj ścieżki, aby określić katalog główny zawartości aplikacji:

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a>Przechowywanie plików usługi w odpowiedniej lokalizacji na dysku

Określ ścieżkę <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> bezwzględną przy <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> użyciu do folderu zawierającego pliki.

## <a name="additional-resources"></a>Dodatkowe zasoby

::: moniker range=">= aspnetcore-3.0"

* [Konfiguracja punktu końcowego Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (obejmuje konfigurację protokołu HTTPS i pomoc techniczną SNI)
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* [Konfiguracja punktu końcowego Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (obejmuje konfigurację protokołu HTTPS i pomoc techniczną SNI)
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
