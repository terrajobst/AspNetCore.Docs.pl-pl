---
title: Zwiększanie możliwości aplikacji z zewnętrznego zestawu w programie ASP.NET Core za pomocą interfejsu IHostingStartup
author: guardrex
description: Dowiedz się, jak poprawić aplikacji ASP.NET Core z zestawu zewnętrznego za pomocą interfejsu IHostingStartup implementację.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 2eddfa03b28564fcca7cc098e353b05e23b7c6f6
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336244"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a>Zwiększanie możliwości aplikacji z zewnętrznego zestawu w programie ASP.NET Core za pomocą interfejsu IHostingStartup

Przez [Luke Latham](https://github.com/guardrex)

[Interfejsu IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hostuje uruchamiania) implementacja dodaje rozszerzenia do aplikacji przy uruchamianiu z zestawu zewnętrznego. Na przykład zewnętrznej biblioteki służy hostingu implementacji uruchamiania zapewnienie dodatkowej konfiguracji dostawcy lub usług do aplikacji. `IHostingStartup` *jest dostępna w programie ASP.NET Core 2.0 lub nowszej.*

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="hostingstartup-attribute"></a>Atrybut HostingStartup

A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atrybut wskazuje obecność hostingu zestaw startowy można aktywować w czasie wykonywania.

Zestaw wpisu lub zestawu zawierającego `Startup` automatycznie klasy jest skanowany pod kątem `HostingStartup` atrybutu. Lista zestawów, aby wyszukać `HostingStartup` atrybutów jest ładowany w czasie wykonywania z konfiguracji w [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey). Lista zestawów do wykluczenia z odnajdywania jest ładowany z [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey). Aby uzyskać więcej informacji, zobacz [hosta sieci Web: hostingu zestawy startowe](xref:fundamentals/host/web-host#hosting-startup-assemblies) i [hosta sieci Web: hostingu uruchamiania wykluczyć zestawy](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).

W poniższym przykładzie jest przestrzeń nazw w zestawie hostingu uruchamiania `StartupEnhancement`. Klasa zawierająca kod uruchamiający hostingu jest `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

`HostingStartup` Atrybutu znajduje się w zestawie uruchomienia hostingu `IHostingStartup` pliku z implementacją klasy.

## <a name="discover-loaded-hosting-startup-assemblies"></a>Odkryj załadowanych zestawów uruchomienia hostingu

Aby odnaleźć załadowanych zestawów uruchomienia hostingu, Włącz rejestrowanie i zapoznać się z dziennikami aplikacji. Błędy występujące podczas ładowania zestawów są rejestrowane. Załadowano hostingu zestawy startowe są rejestrowane na poziomie debugowania, a wszystkie błędy są rejestrowane.

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Wyłącz automatyczne ładowanie hostingu zestawy startowe

::: moniker range=">= aspnetcore-2.1"

Aby wyłączyć automatyczne ładowanie hostingu zestawy startowe, użyj jednej z następujących metod:

* Aby zapobiec temu wszystkie zestawy startowe hostingu, ładowania, należy ustawić jedną z następujących czynności, aby `true` lub `1`:
  * [Zapobieganie uruchamianiu hostingu](xref:fundamentals/host/web-host#prevent-hosting-startup) hosta ustawienia konfiguracji.
  * `ASPNETCORE_PREVENTHOSTINGSTARTUP` zmiennej środowiskowej.
* Aby zapobiec określonych zestawów uruchomienia hostingu, ładowanie, ustawić jedną z następujących hostingu uruchamiania zestawów, które mają zostać wykluczone podczas uruchamiania ciągu rozdzielone średnikami:
  * [Hosting uruchamiania wykluczyć zestawy](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) hosta ustawienia konfiguracji.
  * `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` zmiennej środowiskowej.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Aby wyłączyć automatyczne ładowanie hostingu zestawy startowe, ustawić jedną z następujących czynności, aby `true` lub `1`:

* [Zapobieganie uruchamianiu hostingu](xref:fundamentals/host/web-host#prevent-hosting-startup) hosta ustawienia konfiguracji.
* `ASPNETCORE_PREVENTHOSTINGSTARTUP` zmiennej środowiskowej.

::: moniker-end

Jeśli ustawienia konfiguracji hosta i zmienną środowiskową są skonfigurowane, ustawienie hosta steruje zachowaniem.

Wyłączenie hostingu zestawy startowe przy użyciu zmiennej ustawienie lub środowisko hosta globalnie wyłącza zestawu i może wyłączyć pewne cechy aplikacji.

## <a name="project"></a>Projekt

Tworzenie uruchomienia hostingu za pomocą jednej z poniższych typów projektów:

* [Biblioteka klas](#class-library)
* [Aplikacja konsoli bez punktu wejścia](#console-app-without-an-entry-point)

### <a name="class-library"></a>Biblioteka klas

Można podać hostingu ulepszenie uruchamiania w bibliotece klas. Biblioteka zawiera `HostingStartup` atrybutu.

[Przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) obejmuje aplikacją stron Razor *HostingStartupApp*i biblioteki klas, *HostingStartupLibrary*. Biblioteka klas:

* Zawiera klasę uruchomienia hostingu `ServiceKeyInjection`, który implementuje `IHostingStartup`. `ServiceKeyInjection` Dodaje parę ciągów usługi konfiguracji aplikacji przy użyciu dostawcy konfiguracji w pamięci ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).
* Obejmuje `HostingStartup` atrybut, który identyfikuje przestrzeni nazw i klasy uruchomienia hostingu.

`ServiceKeyInjection` Klasy [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metoda używa [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) na dodawanie rozszerzeń do aplikacji. `IHostingStartup.Configure` w hostingu uruchamiania zestawu jest wywoływana przez środowisko wykonawcze przed `Startup.Configure` w kodzie użytkownika, co pozwala kod użytkownika zastąpić żadnej konfiguracji zawartym w zestawie hostingu uruchamiania.

*HostingStartupLibrary/ServiceKeyInjection.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

Strony indeksu aplikacji odczytuje i renderuje wartości konfiguracji dwa klucze, ustawiane przez bibliotekę klas hostingu uruchamiania zestawu:

*HostingStartupApp/Pages/Index.cshtml.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

[Przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) zawiera także projekt pakietu NuGet, który zawiera osobne uruchomienia hostingu, *HostingStartupPackage*. Pakiet ma takie same charakterystyki biblioteki klas wcześniejszym opisem. Pakiet:

* Zawiera klasę uruchomienia hostingu `ServiceKeyInjection`, który implementuje `IHostingStartup`. `ServiceKeyInjection` dodaje dwa ciągi usługi do konfiguracji aplikacji.
* Obejmuje `HostingStartup` atrybutu.

*HostingStartupPackage/ServiceKeyInjection.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

Strony indeksu aplikacji odczytuje i renderuje wartości konfiguracji dwa klucze, ustawiane przez pakiet hostingu uruchamiania zestawu:

*HostingStartupApp/Pages/Index.cshtml.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a>Aplikacja konsoli bez punktu wejścia

*Ta metoda jest dostępna tylko w przypadku aplikacji .NET Core, nie .NET Framework.*

Dynamiczne hostingu ulepszenie uruchamiania, która nie wymaga odwołania w czasie kompilacji w celu aktywacji można podać w aplikacji konsolowej bez punktu wejścia. Ta aplikacja zawiera `HostingStartup` atrybutu. Aby utworzyć dynamiczny uruchomienia hostingu:

1. Implementacja biblioteki jest tworzony z klasy, która zawiera `IHostingStartup` implementacji. Biblioteka implementacja jest traktowany jako normalny pakietu.
1. Aplikacja konsolowa bez punktu wejścia odwołuje się do wdrożenia pakietu biblioteki. Aplikacja konsoli jest używany, ponieważ:
   * Plik zależności jest trwały możliwy do uruchomienia aplikacji, więc biblioteki nie może dostarczyć pliku zależności.
   * Nie można dodać bezpośrednio do biblioteki [Magazyn pakietu środowiska uruchomieniowego](/dotnet/core/deploying/runtime-store), co wymaga możliwy do uruchomienia projektu, który jest przeznaczony dla udostępnionego środowiska uruchomieniowego.
1. Aplikacja konsoli jest publikowany uzyskać zależności uruchomienia hostingu. Konsekwencją publikowania aplikacji konsoli jest, że nieużywane zależności są usuwane z pliku zależności.
1. Aplikacja i jej zależności pliku jest umieszczany w Magazyn pakietu środowiska uruchomieniowego. Do odnajdywania, hostingu zestaw startowy i jego zależności pliku, jest przywoływany w parze zmiennych środowiskowych.

Aplikacja konsoli odwołuje się do [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) pakietu:

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atrybut określa klasę jako implementacja `IHostingStartup` w celu ładowania i wykonania podczas kompilowania [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). W poniższym przykładzie, przestrzeń nazw jest `StartupEnhancement`, a klasa jest `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

Klasa implementuje `IHostingStartup`. Klasa [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metoda używa [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) na dodawanie rozszerzeń do aplikacji. `IHostingStartup.Configure` w hostingu uruchamiania zestawu jest wywoływana przez środowisko wykonawcze przed `Startup.Configure` w kodzie użytkownika, co pozwala kod użytkownika zastąpić żadnej konfiguracji zawartym w zestawie hostingu uruchamiania.

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

Podczas kompilowania `IHostingStartup` projektu pliku zależności (*\*. deps.json*) ustawia `runtime` lokalizacji zestawu do *bin* folderu:

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

Jest wyświetlana tylko część pliku. Nazwa zestawu w przykładzie jest `StartupEnhancement`.

## <a name="specify-the-hosting-startup-assembly"></a>Określ hostingu zestawu startowego

Dla klasy biblioteki — albo konsoli aplikacji dostarczanego hostingu uruchamiania, określić nazwę zestawu startowego hostingu w `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej. Zmienna środowiskowa jest rozdzieloną średnikami listę zestawów.

Tylko hostingu zestawy startowe są skanowane pod kątem `HostingStartup` atrybutu. Dla przykładowej aplikacji *HostingStartupApp*, aby odnaleźć hostingu startupy, które opisano wcześniej, zmienna środowiskowa jest ustawiona na następującą wartość:

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

Zestaw startowy hostingu można również ustawić za pomocą [hostingu zestawy startowe](xref:fundamentals/host/web-host#hosting-startup-assemblies) hosta ustawienia konfiguracji.

Podczas uruchamiania wielu hostingu składa są obecne, ich [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metody są wykonywane w kolejności, czy zestawy są wyświetlane.

## <a name="activation"></a>Aktywacja

Dostępne są następujące opcje do obsługi aktywacji uruchamiania:

* [Środowisko uruchomieniowe magazynu](#runtime-store) &ndash; aktywacji nie wymaga odwołania w czasie kompilacji w celu aktywacji. Przykładowa aplikacja umieszcza hostingu zestawu startowe i zależności pliki do folderu, *wdrożenia*w celu ułatwienia wdrażania hostingu uruchamiania w środowisku multimachine. *Wdrożenia* folder zawiera także skrypt programu PowerShell, który tworzy lub modyfikuje zmienne środowiskowe w systemie wdrożenia, aby włączyć uruchamianie hostingu.
* Dokumentacja kompilacji wymagany do aktywacji
  * [Pakiet NuGet](#nuget-package)
  * [Folder bin projektu](#project-bin-folder)

### <a name="runtime-store"></a>Środowisko uruchomieniowe magazynu

Hostingu implementacji uruchamiania jest umieszczany w [magazynu środowiska uruchomieniowego](/dotnet/core/deploying/runtime-store). Odwołanie do zestawu kompilacji nie jest wymagane przez aplikację rozszerzone.

Po utworzeniu hostingu uruchamiania pliku projektu startowego hostingu służy jako plik manifestu pod kątem [magazynu dotnet](/dotnet/core/tools/dotnet-store) polecenia.

```console
dotnet store --manifest <PROJECT_FILE> --runtime <RUNTIME_IDENTIFIER>
```

To polecenie umieszcza zestawie hostingu uruchamiania i inne zależności, które nie są częścią udostępnionej platformy w magazynie środowiska uruchomieniowego profilu użytkownika na:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

Jeśli wymagasz umieścić zestaw i zależności do użytku globalnego, należy dodać `-o|--output` opcję `dotnet store` polecenia z następującą ścieżką:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

**Zmodyfikuj i umieść plik zależności startowe hostingu**

Lokalizacja środowiska uruchomieniowego jest określona w  *\*. deps.json* pliku. Aby aktywować ulepszenie, `runtime` element musi określać lokalizację zestawu środowiska wykonawczego rozszerzenie. Prefiks `runtime` lokalizacji za pomocą `lib/<TARGET_FRAMEWORK_MONIKER>/`:

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

W przykładowym kodzie (*StartupDiagnostics* projektu), modyfikacji  *\*. deps.json* pliku odbywa się przez [PowerShell](/powershell/scripting/powershell-scripting) skryptu. Skrypt programu PowerShell automatycznie jest wyzwalana przez element docelowy kompilacji, w pliku projektu.

Implementacja  *\*. deps.json* plik musi znajdować się w dostępnej lokalizacji.

Do użytku dla użytkownika, umieść plik w *additonalDeps* folderu profilu użytkownika `.dotnet` ustawienia:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

Do użytku globalnego, umieść plik w *additonalDeps* folderu instalacji platformy .NET Core:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

Udostępnione framework w wersji odzwierciedla wersję udostępnionego środowiska uruchomieniowego, która korzysta z docelową aplikacją. Udostępnione środowisko uruchomieniowe jest wyświetlany w  *\*. runtimeconfig.json* pliku. W przykładowej aplikacji (*HostingStartupApp*), udostępnionego środowiska uruchomieniowego jest określona w *HostingStartupApp.runtimeconfig.json* pliku.

**Listy plików zależności startowe hostingu**

Lokalizacja wdrożenia  *\*. deps.json* plik znajduje się w `DOTNET_ADDITIONAL_DEPS` zmiennej środowiskowej.

Jeśli plik zostanie umieszczony w profilu użytkownika *.dotnet* folder, ustaw wartość zmiennej środowiskowej:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

---

Jeśli plik zostanie umieszczony w instalacji programu .NET Core do użytku globalnego, należy podać pełną ścieżkę do pliku:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

---

Aby uzyskać przykładową aplikację (*HostingStartupApp*) można znaleźć pliku zależności (*HostingStartupApp.runtimeconfig.json*), plik zależności jest umieszczany w profilu użytkownika.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Ustaw `DOTNET_ADDITIONAL_DEPS` zmienną środowiskową na następującą wartość:

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Ustaw `DOTNET_ADDITIONAL_DEPS` zmienną środowiskową na następującą wartość:

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Ustaw `DOTNET_ADDITIONAL_DEPS` zmienną środowiskową na następującą wartość:

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

---

Aby zapoznać się z przykładami sposobu ustawiania zmiennych środowiskowych dla różnych systemów operacyjnych, zobacz [używanie wielu środowisk](xref:fundamentals/environments).

**Wdrażanie**

W celu ułatwienia wdrażania hostingu uruchamiania w środowisku multimachine, przykładowa aplikacja tworzy *wdrożenia* folderu w opublikowanych danych wyjściowych, który zawiera:

* Zestaw startowy hostingu.
* Plik hostingu zależności startowe.
* Skrypt programu PowerShell, który tworzy lub modyfikuje `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` i `DOTNET_ADDITIONAL_DEPS` do obsługi aktywacji uruchomienia hostingu. Uruchom skrypt w administracyjnym wierszu polecenia programu PowerShell w systemie wdrożenia.

### <a name="nuget-package"></a>Pakiet NuGet

Można podać hostingu ulepszenie uruchamiania pakietu NuGet. Pakiet ma `HostingStartup` atrybutu. Hostingu typy uruchamiania, dostarczone przez pakiet są dostępne dla aplikacji przy użyciu jednej z następujących metod:

* Plik projektu aplikacji rozszerzone sprawia, że odwołania do pakietu do uruchomienia hostingu w pliku projektu aplikacji (odwołanie w czasie kompilacji). Za pomocą odwołania w czasie kompilacji w miejscu, zestawie hostingu uruchamiania i wszystkie jego zależności są włączone do pliku zależności aplikacji (*\*. deps.json*). Takie podejście stosuje się do hostingu pakietu zestawu startowego publikowane w [nuget.org](https://www.nuget.org/).
* Plik zależności startowe hostingu ma zostać udostępnione rozszerzone aplikacji zgodnie z opisem w [magazynu środowiska uruchomieniowego](#runtime-store) sekcji (bez odwołania do kompilacji).

Aby uzyskać więcej informacji na temat pakietów NuGet i magazynem środowiska uruchomieniowego zobacz następujące tematy:

* [Jak utworzyć pakiet NuGet za pomocą narzędzi międzyplatformowych](/dotnet/core/deploying/creating-nuget-packages)
* [Publikowanie pakietów](/nuget/create-packages/publish-a-package)
* [Magazyn pakietu środowiska uruchomieniowego](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a>Folder bin projektu

Hostingu ulepszenie uruchamiania mogą być zapewniane przez *bin*-wdrożone zestawu w rozszerzonej aplikacji. Typy uruchamiania hostingu, dostarczone przez zestaw są dostępne dla aplikacji przy użyciu jednej z następujących metod:

* Plik projektu aplikacji rozszerzone sprawia, że odwołanie do zestawu do uruchomienia hostingu (odwołanie w czasie kompilacji). Za pomocą odwołania w czasie kompilacji w miejscu, zestawie hostingu uruchamiania i wszystkie jego zależności są włączone do pliku zależności aplikacji (*\*. deps.json*). Takie podejście ma zastosowanie, gdy scenariusz wdrażania wymaga, aby przenoszenia zestawu skompilowanego hostingu uruchamiania biblioteki (plik DLL) do konsumencki projektu lub do lokalizacji dostępne przez konsumencki projekt i kompilacji odnosi się do hostingu zestaw startowy firmy.
* Plik zależności startowe hostingu ma zostać udostępnione rozszerzone aplikacji zgodnie z opisem w [magazynu środowiska uruchomieniowego](#runtime-store) sekcji (bez odwołania do kompilacji).

## <a name="sample-code"></a>Przykładowy kod

[Przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)) pokazuje hostingu scenariuszom dla programów próbnych implementacji:

* Dwa hostingu zestawy startowe (bibliotek klas) Ustaw parę pary klucz wartość w pamięci konfiguracji poszczególnych:
  * Pakiet NuGet (*HostingStartupPackage*)
  * Biblioteka klas (*HostingStartupLibrary*)
* Uruchamianie hostingu została aktywowana w zestawie środowiska uruchomieniowego wdrożonych w magazynie (*StartupDiagnostics*). Zestaw dodaje dwa middlewares do aplikacji podczas uruchamiania, które zawierają informacje diagnostyczne dotyczące:
  * Zarejestrowane usługi
  * Adres (schematu, hosta, podstawa ścieżki, ścieżki, ciąg zapytania)
  * Połączenia (zdalnego adresu IP, port zdalny lokalnego adresu IP, port lokalny, certyfikatu klienta)
  * Nagłówki żądania
  * Zmienne środowiskowe

Do uruchomienia przykładu:

**Aktywacja z pakietu NuGet**

1. Skompilować *HostingStartupPackage* pakietu z [pakietu dotnet](/dotnet/core/tools/dotnet-pack) polecenia.
1. Dodaj nazwę zestawu pakietu *HostingStartupPackage* do `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.
1. Skompiluj i uruchom aplikację. Odwołanie do pakietu jest obecny w aplikacji rozszerzone (odwołanie w czasie kompilacji). A `<PropertyGroup>` w projekcie aplikacji plik Określa danych wyjściowych projektu pakietu (*... / HostingStartupPackage/bin/Debug*) jako źródło pakietów. Umożliwia to aplikacji na użycie pakietu bez przekazywania pakietu, który ma [nuget.org](https://www.nuget.org/). Aby uzyskać więcej informacji, zobacz uwagi w pliku projektu HostingStartupApp.

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```
1. Sprawdź, czy wartości klucza konfiguracji usługi świadczone przez stronę indeksu odpowiadają wartości ustawione przy użyciu pakietu `ServiceKeyInjection.Configure` metody.

W przypadku wprowadzenia zmian do *HostingStartupPackage* projektu i ponownie ją kompilując, wyczyść pamięciach podręcznych pakietu NuGet, aby upewnić się, że *HostingStartupApp* otrzymuje zaktualizowany pakiet i nie nieodświeżone pakiet z lokalnej pamięci podręcznej. Aby wyczyścić pamięciach podręcznych NuGet, wykonaj następujące czynności, [lokalne nuget dotnet](/dotnet/core/tools/dotnet-nuget-locals) polecenia:

```console
dotnet nuget locals all --clear
```

**Aktywacja z biblioteki klas**

1. Skompilować *HostingStartupLibrary* biblioteki klas z [kompilacji dotnet](/dotnet/core/tools/dotnet-build) polecenia.
1. Dodaj nazwę zestawu biblioteki klas *HostingStartupLibrary* do `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.
1. *pojemnik*— wdrażanie zestawu biblioteki klas w aplikacji, kopiując *HostingStartupLibrary.dll* pliku z biblioteki klas skompilowanych danych wyjściowych w aplikacji *bin/Debug* folderu.
1. Skompiluj i uruchom aplikację. `<ItemGroup>` w projekcie aplikacji plik odwołuje się do zestawu biblioteki klas (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (odwołanie w czasie kompilacji). Aby uzyskać więcej informacji, zobacz uwagi w pliku projektu HostingStartupApp.

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```
1. Sprawdź, czy wartości klucza konfiguracji usługi świadczone przez stronę indeksu odpowiadają wartości ustawione przez bibliotekę klas `ServiceKeyInjection.Configure` metody.

**Aktywacja w zestawie środowiska uruchomieniowego wdrożonych w magazynie**

1. *StartupDiagnostics* projekt używa [PowerShell](/powershell/scripting/powershell-scripting) do modyfikowania jej *StartupDiagnostics.deps.json* pliku. Program PowerShell jest instalowany domyślnie w systemie Windows, począwszy od Windows 7 z dodatkiem SP1 i Windows Server 2008 R2 z dodatkiem SP1. Aby uzyskać programu PowerShell na innych platformach, zobacz [Instalowanie programu Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).
1. Tworzenie *StartupDiagnostics* projektu. Po projekt został utworzony, element docelowy kompilacji, w pliku projektu automatycznie:
   * Uruchamia skrypt programu PowerShell w celu zmodyfikowania *StartupDiagnostics.deps.json* pliku.
   * Przenosi *StartupDiagnostics.deps.json* pliku do profilu użytkownika *additionalDeps* folderu.
1. Wykonaj `dotnet store` polecenia na command prompt w hostingu uruchamiania katalog do przechowywania zestawu i jego zależności w magazynie środowiska uruchomieniowego profilu użytkownika:

   ```console
   dotnet store --manifest StartupDiagnostics.csproj --runtime <RID>
   ```
   Windows, polecenie używa `win7-x64` [identyfikator środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog). Podczas dostarczania hostingu uruchamiania różnych środowiska uruchomieniowego, podstawić poprawne identyfikatorów RID.
1. Ustaw zmienne środowiskowe:
   * Dodaj nazwę zestawu *StartupDiagnostics* do `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.
   * Na Windows, ustaw `DOTNET_ADDITIONAL_DEPS` zmiennej środowiskowej, aby `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`. W systemie macOS/Linux, należy ustawić `DOTNET_ADDITIONAL_DEPS` zmiennej środowiskowej, aby `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, gdzie `<USER>` jest profil użytkownika, który zawiera uruchomienia hostingu.
1. Uruchom przykładową aplikację.
1. Żądanie `/services` punktu końcowego, aby wyświetlić aplikację zarejestrowane usługi. Żądanie `/diag` punktu końcowego, aby wyświetlić informacje diagnostyczne.
