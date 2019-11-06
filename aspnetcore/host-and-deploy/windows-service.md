---
title: ASP.NET Core hosta w usłudze systemu Windows
author: guardrex
description: Dowiedz się, jak hostować aplikację ASP.NET Core w usłudze systemu Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/30/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 014585cd1e170fc94f7f577e11ec19824e54572f
ms.sourcegitcommit: 6628cd23793b66e4ce88788db641a5bbf470c3c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73659846"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>ASP.NET Core hosta w usłudze systemu Windows

Autor [Luke Latham](https://github.com/guardrex)

Aplikacja ASP.NET Core może być hostowana w systemie Windows jako [Usługa systemu Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) bez korzystania z usług IIS. Gdy usługa jest hostowana w systemie Windows, aplikacja jest uruchamiana automatycznie po ponownym uruchomieniu serwera.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

* [ASP.NET Core zestaw SDK 2,1 lub nowszy](https://dotnet.microsoft.com/download)
* [Program PowerShell 6,2 lub nowszy](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a>Szablon usługi procesu roboczego

Szablon usługi ASP.NET Core Worker zapewnia punkt początkowy do pisania długotrwałych aplikacji usługi. Aby użyć szablonu jako podstawy dla aplikacji usługi systemu Windows:

1. Utwórz aplikację usługi procesu roboczego na podstawie szablonu .NET Core.
1. Postępuj zgodnie ze wskazówkami w sekcji [Konfiguracja aplikacji](#app-configuration) , aby zaktualizować aplikację usługi procesu roboczego tak, aby mogła ona działać jako usługa systemu Windows.

[!INCLUDE[](~/includes/worker-template-instructions.md)]

::: moniker-end

## <a name="app-configuration"></a>Konfiguracja aplikacji

::: moniker range=">= aspnetcore-3.0"

Aplikacja wymaga odwołania do pakietu dla elementu [Microsoft. Extensions. hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices).

`IHostBuilder.UseWindowsService` jest wywoływana podczas kompilowania hosta. Jeśli aplikacja działa jako usługa systemu Windows, Metoda:

* Ustawia okres istnienia hosta na `WindowsServiceLifetime`.
* Ustawia [katalog główny zawartości](xref:fundamentals/index#content-root).
* Umożliwia rejestrowanie w dzienniku zdarzeń przy użyciu nazwy aplikacji jako domyślnej nazwy źródła.
  * Poziom dziennika można skonfigurować przy użyciu klucza `Logging:LogLevel:Default` w pliku *appSettings. Plik Product. JSON* .
  * Tylko Administratorzy mogą tworzyć nowe źródła zdarzeń. Gdy nie można utworzyć źródła zdarzeń przy użyciu nazwy aplikacji, w źródle *aplikacji* jest rejestrowane ostrzeżenie, a dzienniki zdarzeń są wyłączone.

W `CreateHostBuilder` *program.cs*:

```csharp
Host.CreateDefaultBuilder(args)
    .UseWindowsService()
    ...
```

Następujące przykładowe aplikacje zostały dołączone do tego tematu:

* Przykład usługi procesu roboczego w tle &ndash; przykład aplikacji innej niż sieć Web oparta na [szablonie usługi procesu roboczego](#worker-service-template) , który korzysta z [usług hostowanych](xref:fundamentals/host/hosted-services) na potrzeby zadań w tle.
* Przykład App Service sieci Web &ndash; przykład aplikacji internetowej Razor Pages, która działa jako usługa systemu Windows z [usługami hostowanymi](xref:fundamentals/host/hosted-services) dla zadań w tle.

Wskazówki dotyczące MVC można znaleźć w artykułach w obszarze <xref:mvc/overview> i <xref:migration/22-to-30>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Aplikacja wymaga odwołania do pakietów dla elementu [Microsoft. AspNetCore. hosting. WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) i [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).

Aby przetestować i debugować w przypadku uruchamiania poza usługą, należy dodać kod, aby określić, czy aplikacja działa jako usługa, czy Aplikacja konsolowa. Sprawdź, czy debuger jest podłączony lub czy jest dostępny przełącznik `--console`. Jeśli którykolwiek z warunków jest spełniony (aplikacja nie jest uruchomiona jako usługa), wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>. Jeśli warunki są fałszywe (aplikacja jest uruchamiana jako usługa):

* Wywołaj <xref:System.IO.Directory.SetCurrentDirectory*> i użyj ścieżki do lokalizacji opublikowanej aplikacji. Nie wywołuj <xref:System.IO.Directory.GetCurrentDirectory*>, aby uzyskać ścieżkę, ponieważ aplikacja usługi systemu Windows zwraca folder *C:\\Windows\\system32* w przypadku wywołania <xref:System.IO.Directory.GetCurrentDirectory*>. Aby uzyskać więcej informacji, zapoznaj się z sekcją [Current Directory i root Content](#current-directory-and-content-root) . Ten krok jest wykonywany przed skonfigurowaniem aplikacji w `CreateWebHostBuilder`.
* Wywołaj <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>, aby uruchomić aplikację jako usługę.

Ponieważ [dostawca konfiguracji wiersza polecenia](xref:fundamentals/configuration/index#command-line-configuration-provider) wymaga par nazwa-wartość dla argumentów wiersza polecenia, przełącznik `--console` jest usuwany z argumentów przed <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> odbiera argumenty.

Aby zapisać w dzienniku zdarzeń systemu Windows, Dodaj dostawcę EventLog do <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>. Ustaw poziom rejestrowania z kluczem `Logging:LogLevel:Default` w pliku *appSettings. Plik Product. JSON* .

W poniższym przykładzie z przykładowej aplikacji `RunAsCustomService` jest wywoływana zamiast <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> w celu obsługi zdarzeń okresu istnienia w aplikacji. Aby uzyskać więcej informacji, zobacz sekcję [Obsługa zdarzeń dotyczących uruchamiania i zatrzymywania](#handle-starting-and-stopping-events) .

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a>Typ wdrożenia

Aby uzyskać informacje i porady dotyczące scenariuszy wdrażania, zobacz [wdrażanie aplikacji .NET Core](/dotnet/core/deploying/).

### <a name="sdk"></a>SDK

W przypadku usługi opartej na aplikacji sieci Web używającej platform Razor Pages lub MVC należy określić zestaw SDK sieci Web w pliku projektu:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

Jeśli usługa wykonuje tylko zadania w tle (na przykład [usługi hostowane](xref:fundamentals/host/hosted-services)), określ zestaw SDK procesu roboczego w pliku projektu:

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

### <a name="framework-dependent-deployment-fdd"></a>Wdrożenie zależne od platformy (FDD)

Wdrożenie zależne od platformy (FDD) zależy od obecności udostępnionej systemowej wersji platformy .NET Core w systemie docelowym. Po przyjęciu scenariusza FDD zgodnie ze wskazówkami zawartymi w tym artykule zestaw SDK tworzy plik wykonywalny ( *. exe*), nazywany *plik wykonywalny zależny od platformy*.

::: moniker range=">= aspnetcore-3.0"

Jeśli używasz [zestawu SDK sieci Web](#sdk), plik *Web. config* , który jest zwykle tworzony podczas publikowania aplikacji ASP.NET Core, jest zbędny dla aplikacji usług systemu Windows. Aby wyłączyć tworzenie pliku *Web. config* , należy dodać właściwość `<IsTransformWebConfigDisabled>` do `true`.

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[Identyfikator środowiska uruchomieniowego systemu Windows (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)) zawiera platformę docelową. W poniższym przykładzie identyfikator RID jest ustawiony na `win7-x64`. Właściwość `<SelfContained>` ma wartość `false`. Te właściwości instruują zestaw SDK, aby wygenerował plik wykonywalny (*exe*) dla systemu Windows i aplikację, która zależy od współużytkowanej platformy .NET Core.

Plik *Web. config* , który jest zwykle tworzony podczas publikowania aplikacji ASP.NET Core, nie jest konieczny dla aplikacji usług systemu Windows. Aby wyłączyć tworzenie pliku *Web. config* , należy dodać właściwość `<IsTransformWebConfigDisabled>` do `true`.

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

[Identyfikator środowiska uruchomieniowego systemu Windows (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)) zawiera platformę docelową. W poniższym przykładzie identyfikator RID jest ustawiony na `win7-x64`. Właściwość `<SelfContained>` ma wartość `false`. Te właściwości instruują zestaw SDK, aby wygenerował plik wykonywalny (*exe*) dla systemu Windows i aplikację, która zależy od współużytkowanej platformy .NET Core.

Właściwość `<UseAppHost>` ma wartość `true`. Ta właściwość zapewnia usługę ze ścieżką aktywacji (plik wykonywalny, *exe*) dla FDD.

Plik *Web. config* , który jest zwykle tworzony podczas publikowania aplikacji ASP.NET Core, nie jest konieczny dla aplikacji usług systemu Windows. Aby wyłączyć tworzenie pliku *Web. config* , należy dodać właściwość `<IsTransformWebConfigDisabled>` do `true`.

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

### <a name="self-contained-deployment-scd"></a>Wdrażanie samodzielne (SCD)

Wdrożenie samodzielne (SCD) nie polega na obecności struktury udostępnionej w systemie hosta. Środowisko uruchomieniowe i zależności aplikacji są wdrażane przy użyciu aplikacji.

[Identyfikator (RID) środowiska uruchomieniowego](/dotnet/core/rid-catalog) systemu Windows jest zawarty w `<PropertyGroup>`, który zawiera platformę docelową:

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

Aby opublikować dla wielu identyfikatorów RID:

* Podaj identyfikatory RID na liście rozdzielanej średnikami.
* Użyj nazwy właściwości [\<RuntimeIdentifiers >](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).

Aby uzyskać więcej informacji, zobacz [katalog .NET Core RID Catalog](/dotnet/core/rid-catalog).

::: moniker range="< aspnetcore-3.0"

Właściwość `<SelfContained>` ma wartość `true`:

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

Jeśli parametr `-AccountExpires` został dostarczony do polecenia cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) z wygaśnięciem <xref:System.DateTime>, konto nie wygaśnie.

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
   1. Wybierz pozycję **Zaawansowane**. Wybierz pozycję **Znajdź teraz**. Wybierz z listy konto użytkownika. Wybierz **przycisk OK**. Ponownie wybierz **przycisk OK** , aby dodać użytkownika do zasad.
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

* `{EXE PATH}` &ndash; ścieżka do folderu aplikacji na hoście (na przykład `d:\myservice`). Nie dołączaj pliku wykonywalnego aplikacji do ścieżki. Końcowy ukośnik nie jest wymagany.
* konto użytkownika usługi `{DOMAIN OR COMPUTER NAME\USER}` &ndash; (na przykład `Contoso\ServiceUser`).
* `{NAME}` &ndash; Nazwa usługi (na przykład `MyService`).
* `{EXE FILE PATH}` &ndash; ścieżka pliku wykonywalnego aplikacji (na przykład `d:\myservice\myservice.exe`). Uwzględnij nazwę pliku wykonywalnego z rozszerzeniem.
* `{DESCRIPTION}` &ndash; Opis usługi (na przykład `My sample service`).
* `{DISPLAY NAME}` &ndash; Nazwa wyświetlana usługi (na przykład `My Service`).

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

Aby obsłużyć <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> i <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> zdarzeń:

1. Utwórz klasę pochodzącą z <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> z `OnStarting`, `OnStarted` i `OnStopping` metodami:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. Utwórz metodę rozszerzenia dla <xref:Microsoft.AspNetCore.Hosting.IWebHost>, która przekazuje `CustomWebHostService` do <xref:System.ServiceProcess.ServiceBase.Run*>:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. W `Program.Main` Wywołaj metodę rozszerzenia `RunAsCustomService` zamiast <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:

   ```csharp
   host.RunAsCustomService();
   ```

   Aby zobaczyć lokalizację <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> w `Program.Main`, zapoznaj się z przykładem kodu pokazanym w sekcji [typ wdrożenia](#deployment-type) .

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scenariusze serwera proxy i modułu równoważenia obciążenia

Usługi, które współdziałają z żądaniami z Internetu lub sieci firmowej i znajdują się za serwerem proxy lub modułem równoważenia obciążenia, mogą wymagać dodatkowej konfiguracji. Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-endpoints"></a>Konfigurowanie punktów końcowych

Domyślnie ASP.NET Core wiąże się z `http://localhost:5000`. Skonfiguruj adres URL i port, ustawiając zmienną środowiskową `ASPNETCORE_URLS`.

Aby uzyskać dodatkowe podejścia do konfiguracji adresów URL i portów, zobacz artykuł dotyczący odpowiedniego serwera:

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

Powyższe wskazówki obejmują obsługę punktów końcowych HTTPS. Na przykład skonfiguruj aplikację do obsługi protokołu HTTPS, gdy uwierzytelnianie jest używane z usługą systemu Windows.

> [!NOTE]
> Korzystanie z ASP.NET Core certyfikatu deweloperskiego HTTPS w celu zabezpieczenia punktu końcowego usługi nie jest obsługiwane.

## <a name="current-directory-and-content-root"></a>Bieżący katalog i główna Zawartość

Bieżącym katalogiem roboczym zwróconym przez wywoływanie <xref:System.IO.Directory.GetCurrentDirectory*> dla usługi systemu Windows jest folder *C:\\Windows\\system32* . Folder *system32* nie jest odpowiednią lokalizacją do przechowywania plików usługi (na przykład plików ustawień). Użyj jednego z poniższych metod, aby zachować i uzyskać dostęp do plików ustawień i zasobów usługi.

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a>Użyj ContentRootPath lub ContentRootFileProvider

Użyj [IHostEnvironment. ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) lub <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> do lokalizowania zasobów aplikacji.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a>Ustawianie ścieżki katalogu głównego zawartości do folderu aplikacji

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> jest tą samą ścieżką dostarczoną do argumentu `binPath` podczas tworzenia usługi. Zamiast wywoływania `GetCurrentDirectory` Aby utworzyć ścieżki do plików ustawień, wywołaj <xref:System.IO.Directory.SetCurrentDirectory*> ze ścieżką do [katalogu głównego zawartości](xref:fundamentals/index#content-root)aplikacji.

W `Program.Main` określ ścieżkę do folderu wykonywalnego usługi i użyj ścieżki, aby określić katalog główny zawartości aplikacji:

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

Określ ścieżkę bezwzględną z <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> w przypadku używania <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> do folderu zawierającego pliki.

## <a name="additional-resources"></a>Dodatkowe zasoby

::: moniker range=">= aspnetcore-3.0"

* [Konfiguracja punktu końcowego Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (w tym Konfiguracja protokołu HTTPS i obsługa SNI)
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* [Konfiguracja punktu końcowego Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (w tym Konfiguracja protokołu HTTPS i obsługa SNI)
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
