---
title: Host platformy ASP.NET Core w usłudze Windows
author: guardrex
description: Dowiedz się, jak udostępnić aplikację ASP.NET Core w usłudze Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/21/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: ab36bc1b2827c80bb1e7b9e8cee558b346a991f8
ms.sourcegitcommit: b8ed594ab9f47fa32510574f3e1b210cff000967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/28/2019
ms.locfileid: "66251422"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Host platformy ASP.NET Core w usłudze Windows

Przez [Luke Latham](https://github.com/guardrex) i [Tom Dykstra](https://github.com/tdykstra)

Aplikacji ASP.NET Core może być hostowana na Windows jako [usługi Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) bez korzystania z usług IIS. Gdy hostowany jako usługa Windows, aplikacji uruchamiany automatycznie po ponownym uruchomieniu serwera.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

* [Platforma ASP.NET Core SDK 2.1 lub nowszej](https://dotnet.microsoft.com/download)
* [PowerShell 6.2 lub nowszej](https://github.com/PowerShell/PowerShell)

## <a name="app-configuration"></a>Konfiguracja aplikacji

::: moniker range=">= aspnetcore-3.0"

`IHostBuilder.UseWindowsService`, dostarczone przez [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) pakietu, jest wywoływana podczas tworzenia hosta. Jeśli aplikacja jest uruchomiona jako usługa Windows metody:

* Ustawia okres istnienia hosta `WindowsServiceLifetime`.
* Ustawia zawartość katalogu głównego.
* Włącza funkcję rejestrowania w dzienniku zdarzeń, z nazwą aplikacji jako domyślną nazwę źródła.
  * Poziom dziennika można skonfigurować za pomocą `Logging:LogLevel:Default` w *appsettings. Production.JSON* pliku.
  * Tylko administratorzy mogą tworzyć nowe źródła zdarzeń. Gdy nie można utworzyć źródła zdarzeń, przy użyciu nazwy aplikacji, ostrzeżenie jest rejestrowany wpis *aplikacji* źródła i dzienniki zdarzeń są wyłączone.

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Aplikacja wymaga odwołania do pakietu dla [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) i [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).

Do testowania i debugowania, gdy działają poza usługą, Dodaj kod, aby ustalić, czy aplikacja jest uruchomiona jako usługę lub aplikację konsoli. Sprawdź, czy jest dołączony debuger a `--console` przełącznik jest obecny. Jeśli tych warunków jest spełniony, (aplikacja nie jest uruchamiana jako usługa), należy wywołać <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>. Jeśli warunki są fałszywe, (aplikacja jest uruchamiana jako usługa):

* Wywołaj <xref:System.IO.Directory.SetCurrentDirectory*> i użyj ścieżki do lokalizacji publikowania aplikacji. Nie wywołuj <xref:System.IO.Directory.GetCurrentDirectory*> można uzyskać ścieżki, ponieważ aplikacji usługi Windows, która zwraca *C:\\WINDOWS\\system32* folderu podczas <xref:System.IO.Directory.GetCurrentDirectory*> nosi nazwę. Aby uzyskać więcej informacji, zobacz [bieżący katalog i katalog główny zawartości](#current-directory-and-content-root) sekcji. Wykonanie tego kroku, zanim aplikacja jest skonfigurowana w `CreateWebHostBuilder`.
* Wywołaj <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> do uruchomienia aplikacji jako usługi.

Ponieważ [dostawcę konfiguracji wiersza polecenia](xref:fundamentals/configuration/index#command-line-configuration-provider) wymaga pary nazwa wartość dla argumentów wiersza poleceń `--console` przełącznik został usunięty z argumentów przed <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> odbiera argumenty.

Można zapisać w dzienniku zdarzeń Windows, należy dodać dostawcę dziennika zdarzeń <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>. Poziom rejestrowania za pomocą `Logging:LogLevel:Default` w *appsettings. Production.JSON* pliku.

W poniższym przykładzie z przykładowej aplikacji `RunAsCustomService` nosi nazwę zamiast <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> w celu obsługi zdarzenia okresu istnienia w aplikacji. Aby uzyskać więcej informacji, zobacz [obsługi, uruchamianie i zatrzymywanie wydarzeń](#handle-starting-and-stopping-events) sekcji.

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a>Typ wdrożenia

Aby uzyskać informacje i wskazówki dotyczące scenariuszy wdrażania, zobacz [wdrażanie aplikacji .NET Core](/dotnet/core/deploying/).

### <a name="framework-dependent-deployment-fdd"></a>Zależny od struktury wdrożenia (stacje)

Zależny od struktury wdrożenia (stacje) zależy od obecności udostępnione całego systemu w wersji programu .NET Core w systemie docelowym. W przypadku scenariusza z stacje zostanie przyjęte, postępując zgodnie ze wskazówkami w tym artykule, zestaw SDK tworzy plik wykonywalny ( *.exe*), co jest nazywane *zależny od struktury pliku wykonywalnego*.

::: moniker range=">= aspnetcore-3.0"

Dodaj następujące elementy właściwości do pliku projektu:

* `<OutputType>` &ndash; Aplikacja na typ danych wyjściowych (`Exe` dla pliku wykonywalnego).
* `<LangVersion>` &ndash; C# Wersja językowa (`latest` lub `preview`).

A *web.config* pliku, który zazwyczaj jest generowany podczas publikowania aplikacji ASP.NET Core, nie jest konieczne w przypadku aplikacji usługi Windows. Aby wyłączyć tworzenie *web.config* Dodaj `<IsTransformWebConfigDisabled>` właściwością `true`.

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

Windows [identyfikator środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)) zawiera platformę docelową. W poniższym przykładzie ustawiono RID `win7-x64`. `<SelfContained>` Właściwość jest ustawiona na `false`. Te właściwości poinstruować zestawu SDK w celu wygenerowania pliku wykonywalnego ( *.exe*) pliku Windows i aplikacji, która jest zależna od udostępnionej platformy .NET Core.

A *web.config* pliku, który zazwyczaj jest generowany podczas publikowania aplikacji ASP.NET Core, nie jest konieczne w przypadku aplikacji usługi Windows. Aby wyłączyć tworzenie *web.config* Dodaj `<IsTransformWebConfigDisabled>` właściwością `true`.

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

Windows [identyfikator środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier >](/dotnet/core/tools/csproj#runtimeidentifier)) zawiera platformę docelową. W poniższym przykładzie ustawiono RID `win7-x64`. `<SelfContained>` Właściwość jest ustawiona na `false`. Te właściwości poinstruować zestawu SDK w celu wygenerowania pliku wykonywalnego ( *.exe*) pliku Windows i aplikacji, która jest zależna od udostępnionej platformy .NET Core.

`<UseAppHost>` Właściwość jest ustawiona na `true`. Ta właściwość zapewnia usługi ze ścieżką aktywacji (plik wykonywalny *.exe*) dla Dyskietki.

A *web.config* pliku, który zazwyczaj jest generowany podczas publikowania aplikacji ASP.NET Core, nie jest konieczne w przypadku aplikacji usługi Windows. Aby wyłączyć tworzenie *web.config* Dodaj `<IsTransformWebConfigDisabled>` właściwością `true`.

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

### <a name="self-contained-deployment-scd"></a>Niezależne wdrożenia (— SCD)

Niezależne wdrożenia (— SCD) nie jest zależny od obecności udostępnionej platformy w systemie hosta. Środowisko uruchomieniowe i zależności aplikacji są wdrażane przy użyciu aplikacji.

Windows [identyfikator środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog) znajduje się w `<PropertyGroup>` zawierający platforma docelowa:

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

Aby opublikować dla wielu identyfikatorów RID:

* Podaj identyfikatorów RID w liście rozdzielanej średnikami.
* Użyj nazwy właściwości [ \<RuntimeIdentifiers >](/dotnet/core/tools/csproj#runtimeidentifiers) (w liczbie mnogiej).

Aby uzyskać więcej informacji, zobacz [.NET Core RID katalogu](/dotnet/core/rid-catalog).

::: moniker range="< aspnetcore-3.0"

A `<SelfContained>` właściwość jest ustawiona na `true`:

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a>Konto użytkownika usługi

Aby utworzyć konto użytkownika dla usługi, użyj [New LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) polecenia cmdlet z administracyjne powłoki poleceń programu PowerShell 6.

W systemie Windows 10 października 2018 aktualizacja (wersja 1809/build 10.0.17763) lub nowszej:

```PowerShell
New-LocalUser -Name {NAME}
```

W systemie operacyjnym Windows w starszych niż Windows 10 października 2018 aktualizacja (wersja 1809/build 10.0.17763):

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

Podaj [silne hasło](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) po wyświetleniu monitu.

Chyba że `-AccountExpires` parametru do [New LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) polecenia cmdlet z wygaśnięciem <xref:System.DateTime>, konto nie wygasa.

Aby uzyskać więcej informacji, zobacz [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) i [kont użytkowników usług](/windows/desktop/services/service-user-accounts).

Innym sposobem zarządzania użytkownikami, podczas korzystania z usługi Active Directory jest użycie kont usług zarządzanych. Aby uzyskać więcej informacji, zobacz [omówienie kont usług zarządzanych przez grupę](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).

## <a name="log-on-as-a-service-rights"></a>Zaloguj się jako z uprawnieniami usługi

Aby ustanowić *Zaloguj się jako usługa* uprawnienia dla konta użytkownika usługi:

1. Otwórz Edytor zasady zabezpieczeń lokalnych, uruchamiając *secpool.msc*.
1. Rozwiń **zasady lokalne** a następnie wybierz węzeł **Przypisywanie praw użytkownika**.
1. Otwórz **Zaloguj się jako usługa** zasad.
1. Wybierz **Dodaj użytkownika lub grupę**.
1. Podaj nazwę obiektu (konto użytkownika), przy użyciu jednej z następujących metod:
   1. Wpisz konto użytkownika (`{DOMAIN OR COMPUTER NAME\USER}`) w obiekcie nazwy pola i wybierz pozycję **OK** dodać użytkownika do zasad.
   1. Wybierz **zaawansowane**. Wybierz **Znajdź teraz**. Wybierz konto użytkownika z listy. Kliknij przycisk **OK**. Wybierz **OK** ponownie, aby dodać użytkownika do zasad.
1. Wybierz **OK** lub **Zastosuj** aby zatwierdzić zmiany.

## <a name="create-and-manage-the-windows-service"></a>Tworzenie i zarządzanie usługą Windows

### <a name="create-a-service"></a>Tworzenie usługi

Użyj poleceń programu PowerShell w celu zarejestrowania usługi. Z administracyjne powłoki poleceń programu PowerShell 6 wykonaj następujące polecenia:

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* `{EXE PATH}` &ndash; Ścieżka do folderu aplikacji na hoście (na przykład `d:\myservice`). Nie dołączaj ścieżki pliku wykonywalnego aplikacji. Końcowy ukośnik nie jest wymagana.
* `{DOMAIN OR COMPUTER NAME\USER}` &ndash; Konto użytkownika usługi (na przykład `Contoso\ServiceUser`).
* `{NAME}` &ndash; Nazwa usługi (na przykład `MyService`).
* `{EXE FILE PATH}` &ndash; Ścieżka do pliku wykonywalnego aplikacji (na przykład `d:\myservice\myservice.exe`). Zawierać nazwę pliku wykonywalnego z rozszerzeniem.
* `{DESCRIPTION}` &ndash; Opis usługi (na przykład `My sample service`).
* `{DISPLAY NAME}` &ndash; Wyświetlana nazwa usługi (na przykład `My Service`).

### <a name="start-a-service"></a>Uruchom usługę

Uruchom usługę za pomocą następującego polecenia programu PowerShell 6:

```powershell
Start-Service -Name {NAME}
```

Polecenie zajmuje kilka sekund, aby uruchomić usługę.

### <a name="determine-a-services-status"></a>Sprawdź stan usługi

Aby sprawdzić stan usługi, użyj następującego polecenia programu PowerShell 6:

```powershell
Get-Service -Name {NAME}
```

Stan jest zgłaszany jako jeden z następujących wartości:

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a>Zatrzymaj usługę

Zatrzymaj usługę za pomocą następującego polecenia programu Powershell 6:

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a>Usuń usługę

Po krótkiej chwili zatrzymania usługi należy usunąć usługę za pomocą następującego polecenia programu Powershell 6:

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a>Obsługa uruchamianie i zatrzymywanie wydarzeń

Aby obsłużyć <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, i <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> zdarzenia:

1. Utwórz klasę, która pochodzi od klasy <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> z `OnStarting`, `OnStarted`, i `OnStopping` metody:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. Tworzenie metody rozszerzenia dla <xref:Microsoft.AspNetCore.Hosting.IWebHost> , który przekazuje `CustomWebHostService` do <xref:System.ServiceProcess.ServiceBase.Run*>:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. W `Program.Main`, wywołaj `RunAsCustomService` rozszerzenia zamiast metody <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:

   ```csharp
   host.RunAsCustomService();
   ```

   Aby wyświetlić lokalizację <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> w `Program.Main`, można znaleźć w przykładzie kodu pokazano w [typu wdrożenia](#deployment-type) sekcji.

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Serwer proxy i scenariuszy usługi równoważenia obciążenia

Usługi wchodzić w interakcje z żądaniami z Internetu lub sieci firmowej i znajdują się za serwerem proxy lub moduł równoważenia obciążenia, które mogą wymagać dodatkowej konfiguracji. Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>Konfigurowanie protokołu HTTPS

Aby skonfigurować usługę przy użyciu bezpiecznego punktu końcowego:

1. Utwórz certyfikat X.509 dla systemu macierzystego przy użyciu pozyskiwania certyfikatu używanej platformy i mechanizmy wdrażania.

1. Określ [konfiguracji punktu końcowego HTTPS serwera Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) przy użyciu certyfikatu.

Użycie certyfikatu deweloperskiego platformy ASP.NET Core HTTPS, aby zabezpieczyć punkt końcowy usługi nie jest obsługiwane.

## <a name="current-directory-and-content-root"></a>Bieżący katalog i katalog główny zawartości

Bieżący katalog roboczy zwracany przez wywołanie metody <xref:System.IO.Directory.GetCurrentDirectory*> dla usługi Windows jest *C:\\WINDOWS\\system32* folderu. *System32* folder nie jest odpowiednią lokalizację do przechowywania plików usługi (na przykład pliki ustawień). Użyj jednej z następujących metod do utrzymywania i uzyskać dostęp do zasobów i plików ustawień usługi.

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a>Użyj ContentRootPath lub ContentRootFileProvider

Użyj [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) lub <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> lokalizowanie zasobów aplikacji.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

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

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a>Store pliki usługi w odpowiedniej lokalizacji na dysku

Określ ścieżkę bezwzględną z <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> przy użyciu <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> do folderu zawierającego pliki.

## <a name="additional-resources"></a>Dodatkowe zasoby

::: moniker range=">= aspnetcore-3.0"

* [Konfiguracja punktu końcowego kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (w tym konfiguracja protokołu HTTPS i obsługa SNI)
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* [Konfiguracja punktu końcowego kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (w tym konfiguracja protokołu HTTPS i obsługa SNI)
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
