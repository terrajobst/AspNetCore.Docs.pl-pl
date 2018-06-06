---
title: Zwiększ możliwości aplikacji z zewnętrznego zestawu w ASP.NET Core z IHostingStartup
author: guardrex
description: Wykryj jak zwiększyć aplikacji platformy ASP.NET Core z zewnętrznego zestawu przy użyciu implementacji IHostingStartup.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: d913d8a773d312fc4c3191926c6eae2fcb7c6a3e
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734393"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a>Zwiększ możliwości aplikacji z zewnętrznego zestawu w ASP.NET Core z IHostingStartup

Przez [Luke Latham](https://github.com/guardrex)

[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementacji umożliwia dodawania rozszerzenia do aplikacji przy uruchamianiu z zewnętrznego zestawu poza aplikacji `Startup` klasy. Na przykład można użyć biblioteki zewnętrznych narzędzi `IHostingStartup` wdrożenia jest zapewnienie dodatkowej konfiguracji dostawcy usług do aplikacji. `IHostingStartup` *jest dostępna w programie ASP.NET Core 2.0 lub nowszej.*

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="discover-loaded-hosting-startup-assemblies"></a>Odnajdywanie załadowanych zestawów uruchomienia hostingu

Aby dowiedzieć się, że hosting uruchamiania zestawów załadowanych przez aplikację lub bibliotek, Włącz rejestrowanie i sprawdź dzienniki zdarzeń aplikacji. Błędów występujących podczas ładowania zestawów są rejestrowane. Załadowanych zestawów uruchomienia hostingu są zalogowani na poziomie debugowania i są rejestrowane wszystkie błędy.

Odczyty aplikacji przykładowej [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) do `string` tablicy i wyświetlane w aplikacji indeksu strony:

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Wyłącz automatyczne ładowanie hosting zestawy uruchamiania

Istnieją dwa sposoby wyłączyć automatyczne ładowanie hosting zestawy uruchamiania:

* Ustaw [uniemożliwiają uruchamianie Hosting](xref:fundamentals/host/web-host#prevent-hosting-startup) hosta ustawienia konfiguracji.
* Ustaw `ASPNETCORE_PREVENTHOSTINGSTARTUP` zmiennej środowiskowej.

Jeśli ustawienie hosta lub zmienna środowiskowa jest równa `true` lub `1`, hostingu uruchamiania zestawy nie są automatycznie załadowany. Jeśli są ustawione, ustawienie hosta steruje zachowaniem.

Wyłączenie obsługi zestawów uruchamiania przy użyciu zmiennej ustawienie lub środowisko hosta wyłącza je globalnie i wyłączyć pewne cechy aplikacji. Nie można obecnie selektywnego wyłączania hostingu zestawu uruchamiania dodane przez bibliotekę, chyba że biblioteki udostępnia własną opcję konfiguracji. Przyszłych wydaniach oferuje możliwość selektywnego wyłączania hostingu zestawy uruchamiania (zobacz [GitHub wystawiać aspnet/hostingu #1243](https://github.com/aspnet/Hosting/pull/1243)).

## <a name="implement-ihostingstartup"></a>Implementowanie IHostingStartup

### <a name="create-the-assembly"></a>Tworzenie zestawu

`IHostingStartup` Rozszerzenie jest wdrożona jako zestawu oparte na aplikację konsoli bez punktu wejścia. Odwołania do zestawów [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) pakietu:

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atrybut określa klasę jako implementacja `IHostingStartup` ładowania oraz wykonywania podczas kompilowania [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). W poniższym przykładzie, przestrzeń nazw jest `StartupEnhancement`, a klasa `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

Implementuje klasę `IHostingStartup`. Klasa [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) używa metody [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) można dodać rozszerzenia do aplikacji:

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

Podczas kompilowania `IHostingStartup` projektu pliku zależności (*\*. deps.json*) ustawia `runtime` lokalizacji zestawu *bin* folderu:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

Wyświetlane jest tylko część pliku. Nazwa zestawu, w tym przykładzie jest `StartupEnhancement`.

### <a name="update-the-dependencies-file"></a>Zaktualizuj plik zależności

Lokalizacja środowiska uruchomieniowego jest określona w  *\*. deps.json* pliku. Aktywny ulepszeń `runtime` element musi określać lokalizację ulepszenie zestawu środowiska wykonawczego. Prefiks `runtime` lokalizacji z `lib/<TARGET_FRAMEWORK_MONIKER>/`:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

W przykładowej aplikacji, modyfikacja  *\*. deps.json* plików jest wykonywane przez [PowerShell](/powershell/scripting/powershell-scripting) skryptu. Skrypt programu PowerShell jest automatycznie wyzwalane przez element docelowy kompilacji w pliku projektu.

### <a name="enhancement-activation"></a>Rozszerzenie aktywacji

**Umieść plik zestawu**

`IHostingStartup` Implementacji w pliku zestawu musi być *bin*-wdrożone w aplikacji lub umieszczone w [magazynu środowiska uruchomieniowego](/dotnet/core/deploying/runtime-store):

Do użytku dla poszczególnych użytkowników należy umieścić zestaw w magazynie środowiska uruchomieniowego profilu użytkownika:

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

Do użytku globalnego należy umieścić zestaw w magazynie obsługi instalacji platformy .NET Core:

```
<DRIVE>\Program Files\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

Podczas wdrażania zestawu w magazynie środowiska uruchomieniowego, można wdrożyć także pliku symboli, ale nie jest wymagane w celu poprawy do pracy.

**Umieść plik zależności**

Implementacja  *\*. deps.json* plik musi być w dostępnej lokalizacji.

Do użytku dla poszczególnych użytkowników, należy umieścić plik w `additonalDeps` folderu profilu użytkownika `.dotnet` ustawienia:

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

Do użytku globalnego, należy umieścić plik w `additonalDeps` folderu instalacji platformy .NET Core:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

Udostępniony framework w wersji odzwierciedla wersji udostępnionego środowisko uruchomieniowe, które korzysta z aplikacji docelowej. Udostępniony środowiska uruchomieniowego jest wyświetlany w obszarze  *\*. runtimeconfig.json* pliku. W przykładowej aplikacji, udostępnionych środowiska uruchomieniowego jest określona w *HostingStartupSample.runtimeconfig.json* pliku.

**Ustaw zmienne środowiskowe**

Ustaw następujące zmienne środowiskowe w kontekście aplikacji, który używa poprawy.

ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES

Tylko zestawy uruchomienia hostingu są skanowane pod kątem `HostingStartupAttribute`. Nazwa zestawu wdrożenia znajduje się w tej zmiennej środowiskowej. Przykładowa aplikacja ustawia tę wartość na `StartupDiagnostics`.

Wartość można również ustawić za pomocą [Hosting zestawy uruchamiania](xref:fundamentals/host/web-host#hosting-startup-assemblies) hosta ustawienia konfiguracji.

DOTNET\_DODATKOWYCH\_DEPS

Lokalizacja wdrożenia  *\*. deps.json* pliku.

Jeśli plik znajduje się w profilu użytkownika *.dotnet* folderu do użycia na użytkownika:

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

Jeśli plik znajduje się w instalacji platformy .NET Core do użytku globalnego, należy wprowadzić pełną ścieżkę do pliku:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

Przykładowa aplikacja ustawia tę wartość na:

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

Przykłady sposobu ustawiania zmiennych środowiskowych dla różnych systemów operacyjnych, zobacz [używać wiele środowisk](xref:fundamentals/environments).

## <a name="sample-app"></a>Przykładowa aplikacja

[Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)) używa `IHostingStartup` na utworzenie narzędzia diagnostyczne. Dodaje dwa middlewares do aplikacji podczas uruchamiania, aby uzyskać informacje diagnostyczne:

* Zarejestrowane usługi
* Adres: schemat, hosta, podstawa ścieżki, ścieżka, ciąg zapytania
* Połączenie: zdalny adres IP, port zdalny lokalny adres IP, portu lokalnego, certyfikatu klienta
* Nagłówki żądania
* Zmienne środowiskowe

Aby uruchomić przykład:

1. Uruchamianie diagnostyki projekt korzysta z [środowiska PowerShell](/powershell/scripting/powershell-scripting) można zmodyfikować jego *StartupDiagnostics.deps.json* pliku. Środowisko PowerShell jest instalowany domyślnie na systemu operacyjnego Windows, począwszy od systemu Windows 7 z dodatkiem SP1 i Windows Server 2008 R2 z dodatkiem SP1. Aby uzyskać programu PowerShell na innych platformach, zobacz [Instalowanie programu Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).
2. Skompiluj projekt startowy diagnostycznych. Element docelowy kompilacji w pliku projektu:
   * Przenosi zestaw i pliki do profilu użytkownika środowiska uruchomieniowego magazynu symboli.
   * Uruchamia skrypt programu PowerShell w celu zmodyfikowania *StartupDiagnostics.deps.json* pliku.
   * Przenosi *StartupDiagnostics.deps.json* pliku profilu użytkownika `additionalDeps` folderu.
3. Ustaw zmienne środowiskowe:
    * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`
    * `DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`
4. Uruchamianie przykładowej aplikacji.
5. Żądanie `/services` punktu końcowego, aby zobaczyć aplikacji w zarejestrowany usług. Żądanie `/diag` punktu końcowego, aby wyświetlić informacje diagnostyczne.
