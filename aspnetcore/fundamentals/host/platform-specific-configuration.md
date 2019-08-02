---
title: Korzystanie z obsługi zestawów uruchamiania w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak poprawić aplikacji ASP.NET Core z zestawu zewnętrznego za pomocą interfejsu IHostingStartup implementację.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/02/2019
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 3be036d9b4fc6c9898faf14e8a60a8cc7a8683b7
ms.sourcegitcommit: b5e63714afc26e94be49a92619586df5189ed93a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739548"
---
# <a name="use-hosting-startup-assemblies-in-aspnet-core"></a>Korzystanie z obsługi zestawów uruchamiania w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex) i [Pavel Krymets](https://github.com/pakrym)

[Interfejsu IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hostuje uruchamiania) implementacja dodaje rozszerzenia do aplikacji przy uruchamianiu z zestawu zewnętrznego. Na przykład zewnętrznej biblioteki służy hostingu implementacji uruchamiania zapewnienie dodatkowej konfiguracji dostawcy lub usług do aplikacji.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="hostingstartup-attribute"></a>Atrybut HostingStartup

A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atrybut wskazuje obecność hostingu zestaw startowy można aktywować w czasie wykonywania.

Zestaw wpisu lub zestawu zawierającego `Startup` automatycznie klasy jest skanowany pod kątem `HostingStartup` atrybutu. Lista zestawów, aby wyszukać `HostingStartup` atrybutów jest ładowany w czasie wykonywania z konfiguracji w [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey). Lista zestawów do wykluczenia z odnajdywania jest ładowany z [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey). Aby uzyskać więcej informacji, [Zobacz host sieci Web: Hostowanie zestawów](xref:fundamentals/host/web-host#hosting-startup-assemblies) startowych i [hosta sieci Web: Hostowanie wykluczania zestawów](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).

W poniższym przykładzie jest przestrzeń nazw w zestawie hostingu uruchamiania `StartupEnhancement`. Klasa zawierająca kod uruchamiający hostingu jest `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

`HostingStartup` Atrybutu znajduje się w zestawie uruchomienia hostingu `IHostingStartup` pliku z implementacją klasy.

## <a name="discover-loaded-hosting-startup-assemblies"></a>Odkryj załadowanych zestawów uruchomienia hostingu

Aby odnaleźć załadowanych zestawów uruchomienia hostingu, Włącz rejestrowanie i zapoznać się z dziennikami aplikacji. Błędy występujące podczas ładowania zestawów są rejestrowane. Załadowano hostingu zestawy startowe są rejestrowane na poziomie debugowania, a wszystkie błędy są rejestrowane.

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Wyłącz automatyczne ładowanie hostingu zestawy startowe

Aby wyłączyć automatyczne ładowanie hostingu zestawy startowe, użyj jednej z następujących metod:

* Aby zapobiec temu wszystkie zestawy startowe hostingu, ładowania, należy ustawić jedną z następujących czynności, aby `true` lub `1`:
  * [Zapobieganie uruchamianiu hostingu](xref:fundamentals/host/web-host#prevent-hosting-startup) hosta ustawienia konfiguracji.
  * `ASPNETCORE_PREVENTHOSTINGSTARTUP` zmiennej środowiskowej.
* Aby zapobiec określonych zestawów uruchomienia hostingu, ładowanie, ustawić jedną z następujących hostingu uruchamiania zestawów, które mają zostać wykluczone podczas uruchamiania ciągu rozdzielone średnikami:
  * [Hosting uruchamiania wykluczyć zestawy](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) hosta ustawienia konfiguracji.
  * `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` zmiennej środowiskowej.

Jeśli ustawienia konfiguracji hosta i zmienną środowiskową są skonfigurowane, ustawienie hosta steruje zachowaniem.

Wyłączenie hostingu zestawy startowe przy użyciu zmiennej ustawienie lub środowisko hosta globalnie wyłącza zestawu i może wyłączyć pewne cechy aplikacji.

## <a name="project"></a>Projekt

Tworzenie uruchomienia hostingu za pomocą jednej z poniższych typów projektów:

* [Biblioteka klas](#class-library)
* [Aplikacja konsoli bez punktu wejścia](#console-app-without-an-entry-point)

### <a name="class-library"></a>Biblioteka klas

Można podać hostingu ulepszenie uruchamiania w bibliotece klas. Biblioteka zawiera `HostingStartup` atrybutu.

[Przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) obejmuje aplikacją stron Razor *HostingStartupApp*i biblioteki klas, *HostingStartupLibrary*. Biblioteka klas:

* Zawiera klasę uruchomienia hostingu `ServiceKeyInjection`, który implementuje `IHostingStartup`. `ServiceKeyInjection` Dodaje parę ciągów usługi konfiguracji aplikacji przy użyciu dostawcy konfiguracji w pamięci ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).
* Obejmuje `HostingStartup` atrybut, który identyfikuje przestrzeni nazw i klasy uruchomienia hostingu.

`ServiceKeyInjection` Klasy [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metoda używa [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) na dodawanie rozszerzeń do aplikacji.

*HostingStartupLibrary/ServiceKeyInjection.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

Strony indeksu aplikacji odczytuje i renderuje wartości konfiguracji dwa klucze, ustawiane przez bibliotekę klas hostingu uruchamiania zestawu:

*HostingStartupApp/Pages/Index.cshtml.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

[Przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) zawiera także projekt pakietu NuGet, który zawiera osobne uruchomienia hostingu, *HostingStartupPackage*. Pakiet ma takie same charakterystyki biblioteki klas wcześniejszym opisem. Pakiet:

* Zawiera klasę uruchomienia hostingu `ServiceKeyInjection`, który implementuje `IHostingStartup`. `ServiceKeyInjection` dodaje dwa ciągi usługi do konfiguracji aplikacji.
* Obejmuje `HostingStartup` atrybutu.

*HostingStartupPackage/ServiceKeyInjection.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

Strony indeksu aplikacji odczytuje i renderuje wartości konfiguracji dwa klucze, ustawiane przez pakiet hostingu uruchamiania zestawu:

*HostingStartupApp/Pages/Index.cshtml.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a>Aplikacja konsoli bez punktu wejścia

*Ta metoda jest dostępna tylko w przypadku aplikacji .NET Core, nie .NET Framework.*

Dynamiczne hostingu ulepszenie uruchamiania, która nie wymaga odwołania w czasie kompilacji w celu aktywacji można podać w aplikacji konsolowej bez punktu wejścia, który zawiera `HostingStartup` atrybutu. Publikowanie aplikacji konsoli daje hostingu zestaw startowy, który mogą być używane w sklepie środowiska uruchomieniowego.

Aplikacja konsolowa bez punktu wejścia jest używany w ramach tego procesu, ponieważ:

* Do korzystania z hostingu uruchamiania w zestawie hostingu uruchamiania jest wymagana pliku zależności. Plik zależności jest trwały możliwy do uruchomienia aplikacji, który jest wytwarzany przez opublikowanie aplikacji, a nie z biblioteki.
* Nie można dodać bezpośrednio do biblioteki [Magazyn pakietu środowiska uruchomieniowego](/dotnet/core/deploying/runtime-store), co wymaga możliwy do uruchomienia projektu, który jest przeznaczony dla udostępnionego środowiska uruchomieniowego.

W przypadku tworzenia dynamicznego uruchamiania hostingu:

* Zestaw startowy hostingu jest tworzony z aplikacji konsoli, bez punkt wejścia:
  * Zawiera klasę, która zawiera `IHostingStartup` implementacji.
  * Obejmuje [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atrybut do identyfikowania `IHostingStartup` implementacji klasy.
* Aplikacja konsoli jest publikowany uzyskać zależności uruchomienia hostingu. Konsekwencją publikowania aplikacji konsoli jest, że nieużywane zależności są usuwane z pliku zależności.
* Plik zależności jest modyfikowany do ustawiania lokalizacji środowiska uruchomieniowego, zestawu startowego hostingu.
* Zestawie hostingu uruchamiania i jego zależności pliku jest umieszczany w Magazyn pakietu środowiska uruchomieniowego. Aby odnaleźć zestawie hostingu uruchamiania i jego zależności pliku, są wyświetlane w pary zmiennych środowiskowych.

Aplikacja konsoli odwołuje się do [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) pakietu:

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atrybut określa klasę jako implementacja `IHostingStartup` w celu ładowania i wykonania podczas kompilowania [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). W poniższym przykładzie, przestrzeń nazw jest `StartupEnhancement`, a klasa jest `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

Klasa implementuje `IHostingStartup`. Klasa [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metoda używa [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) na dodawanie rozszerzeń do aplikacji. `IHostingStartup.Configure` w hostingu uruchamiania zestawu jest wywoływana przez środowisko wykonawcze przed `Startup.Configure` w kodzie użytkownika, co pozwala kod użytkownika zastąpić żadnej konfiguracji zawartym w zestawie hostingu uruchamiania.

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

Podczas kompilowania `IHostingStartup` projektu plik zależności ( *. deps. JSON*) ustawia `runtime` lokalizację zestawu do folderu *bin* :

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

Jest wyświetlana tylko część pliku. Nazwa zestawu w przykładzie jest `StartupEnhancement`.

## <a name="configuration-provided-by-the-hosting-startup"></a>Konfiguracja dostarczona przez uruchomienie hostingu

Istnieją dwa podejścia do obsługi konfiguracji w zależności od tego, czy konfiguracja uruchamiania hostingu ma mieć pierwszeństwo, czy konfiguracja aplikacji ma mieć pierwszeństwo:

1. Podaj konfigurację aplikacji przy użyciu <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> programu, aby załadować konfigurację po wykonaniu <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegatów aplikacji. Hostowanie konfiguracji uruchamiania ma wyższy priorytet niż konfiguracja aplikacji przy użyciu tego podejścia.
1. Podaj konfigurację aplikacji przy użyciu <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> programu, aby załadować konfigurację przed wykonaniem <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegatów aplikacji. Wartości konfiguracyjne aplikacji mają pierwszeństwo przed tymi, które są udostępniane przez uruchamianie hostingu przy użyciu tego podejścia.

```csharp
public class ConfigurationInjection : IHostingStartup
{
    public void Configure(IWebHostBuilder builder)
    {
        Dictionary<string, string> dict;

        builder.ConfigureAppConfiguration(config =>
        {
            dict = new Dictionary<string, string>
            {
                {"ConfigurationKey1", 
                    "From IHostingStartup: Higher priority than the app's configuration."},
            };

            config.AddInMemoryCollection(dict);
        });

        dict = new Dictionary<string, string>
        {
            {"ConfigurationKey2", 
                "From IHostingStartup: Lower priority than the app's configuration."},
        };

        var builtConfig = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        builder.UseConfiguration(builtConfig);
    }
}
```

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

Po utworzeniu hostingu uruchamiania magazynu środowiska uruchomieniowego jest generowana z użyciem pliku manifestu projektu i [magazynu dotnet](/dotnet/core/tools/dotnet-store) polecenia.

```console
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

W przykładowej aplikacji (*RuntimeStore* projektu) służy następujące polecenie:

``` console
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

Dla środowiska uruchomieniowego odnaleźć magazyn środowiska uruchomieniowego, lokalizacja magazynu środowiska uruchomieniowego jest dodawana do `DOTNET_SHARED_STORE` zmiennej środowiskowej.

**Zmodyfikuj i umieść plik zależności startowe hostingu**

Aby aktywować rozszerzenie bez pakietu odwołania do poprawienia, określ dodatkowe zależności środowiska uruchomieniowego za pomocą `additionalDeps`. `additionalDeps` Umożliwia:

* Rozwiń Graf biblioteki aplikacji, dostarczając zestaw dodatkowych plików *. deps. JSON* do scalenia z własnym plikiem *. deps. JSON* podczas uruchamiania.
* Należy hostingu zestawu startowego obciążana i prostsze do odnalezienia.

Jest zalecane podejście do generowania pliku dodatkowe zależności:

 1. Wykonaj `dotnet publish` w pliku manifestu sklepu środowiska uruchomieniowego, do którego odwołuje się w poprzedniej sekcji.
 1. Usuń odwołanie do manifestu z bibliotek i `runtime` sekcji pliku z wynikiem *. deps. JSON* .

W przykładowym projekcie `store.manifest/1.0.0` właściwości zostanie usunięta z `targets` i `libraries` sekcji:

```json
{
  "runtimeTarget": {
    "name": ".NETCoreApp,Version=v2.1",
    "signature": "4ea77c7b75ad1895ae1ea65e6ba2399010514f99"
  },
  "compilationOptions": {},
  "targets": {
    ".NETCoreApp,Version=v2.1": {
      "store.manifest/1.0.0": {
        "dependencies": {
          "StartupDiagnostics": "1.0.0"
        },
        "runtime": {
          "store.manifest.dll": {}
        }
      },
      "StartupDiagnostics/1.0.0": {
        "runtime": {
          "lib/netcoreapp2.1/StartupDiagnostics.dll": {
            "assemblyVersion": "1.0.0.0",
            "fileVersion": "1.0.0.0"
          }
        }
      }
    }
  },
  "libraries": {
    "store.manifest/1.0.0": {
      "type": "project",
      "serviceable": false,
      "sha512": ""
    },
    "StartupDiagnostics/1.0.0": {
      "type": "package",
      "serviceable": true,
      "sha512": "sha512-oiQr60vBQW7+nBTmgKLSldj06WNLRTdhOZpAdEbCuapoZ+M2DJH2uQbRLvFT8EGAAv4TAKzNtcztpx5YOgBXQQ==",
      "path": "startupdiagnostics/1.0.0",
      "hashPath": "startupdiagnostics.1.0.0.nupkg.sha512"
    }
  }
}
```

Umieść plik *. deps. JSON* w następującej lokalizacji:

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* `{ADDITIONAL DEPENDENCIES PATH}` &ndash; Dodane do lokalizacji `DOTNET_ADDITIONAL_DEPS` zmiennej środowiskowej.
* `{SHARED FRAMEWORK NAME}` &ndash; Udostępnione framework jest wymagany dla tego pliku dodatkowe zależności.
* `{SHARED FRAMEWORK VERSION}` &ndash; Wersja minimalna udostępnionej platformy.
* `{ENHANCEMENT ASSEMBLY NAME}` &ndash; Rozszerzenie nazwy zestawu.

W przykładowej aplikacji (*RuntimeStore* projektu), plik dodatkowe zależności jest umieszczany w następującej lokalizacji:

```
additionalDeps/shared/Microsoft.AspNetCore.App/2.1.0/StartupDiagnostics.deps.json
```

Dla środowiska uruchomieniowego odnaleźć lokalizacji magazynu środowiska uruchomieniowego, lokalizacja pliku dodatkowe zależności jest dodawana do `DOTNET_ADDITIONAL_DEPS` zmiennej środowiskowej.

W przykładowej aplikacji (*RuntimeStore* projektu), tworzenie magazynu środowiska uruchomieniowego i generuje dodatkowe zależności pliku odbywa się przy użyciu [PowerShell](/powershell/scripting/powershell-scripting) skryptu.

Aby zapoznać się z przykładami sposobu ustawiania zmiennych środowiskowych dla różnych systemów operacyjnych, zobacz [używanie wielu środowisk](xref:fundamentals/environments).

**Wdrażanie**

W celu ułatwienia wdrażania hostingu uruchamiania w środowisku multimachine, przykładowa aplikacja tworzy *wdrożenia* folderu w opublikowanych danych wyjściowych, który zawiera:

* Uruchamianie magazyn środowisko uruchomieniowe hostingu.
* Plik hostingu zależności startowe.
* Skrypt programu PowerShell, który tworzy lub modyfikuje `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, i `DOTNET_ADDITIONAL_DEPS` do obsługi aktywacji uruchomienia hostingu. Uruchom skrypt w administracyjnym wierszu polecenia programu PowerShell w systemie wdrożenia.

### <a name="nuget-package"></a>Pakiet NuGet

Można podać hostingu ulepszenie uruchamiania pakietu NuGet. Pakiet ma `HostingStartup` atrybutu. Hostingu typy uruchamiania, dostarczone przez pakiet są dostępne dla aplikacji przy użyciu jednej z następujących metod:

* Plik projektu aplikacji rozszerzone sprawia, że odwołania do pakietu do uruchomienia hostingu w pliku projektu aplikacji (odwołanie w czasie kompilacji). Wraz z odwołaniem w czasie kompilacji, zestaw startowy hostingu i wszystkie jego zależności są zawarte w pliku zależności aplikacji ( *. deps. JSON*). Takie podejście stosuje się do hostingu pakietu zestawu startowego publikowane w [nuget.org](https://www.nuget.org/).
* Plik zależności startowe hostingu ma zostać udostępnione rozszerzone aplikacji zgodnie z opisem w [magazynu środowiska uruchomieniowego](#runtime-store) sekcji (bez odwołania do kompilacji).

Aby uzyskać więcej informacji na temat pakietów NuGet i magazynem środowiska uruchomieniowego zobacz następujące tematy:

* [Jak utworzyć pakiet NuGet za pomocą narzędzi międzyplatformowych](/dotnet/core/deploying/creating-nuget-packages)
* [Publikowanie pakietów](/nuget/create-packages/publish-a-package)
* [Magazyn pakietu środowiska uruchomieniowego](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a>Folder bin projektu

Hostingu ulepszenie uruchamiania mogą być zapewniane przez *bin*-wdrożone zestawu w rozszerzonej aplikacji. Typy uruchamiania hostingu udostępniane przez zestaw są udostępniane aplikacji przy użyciu jednej z następujących metod:

* Plik projektu aplikacji rozszerzone sprawia, że odwołanie do zestawu do uruchomienia hostingu (odwołanie w czasie kompilacji). Wraz z odwołaniem w czasie kompilacji, zestaw startowy hostingu i wszystkie jego zależności są zawarte w pliku zależności aplikacji ( *. deps. JSON*). Takie podejście ma zastosowanie, gdy scenariusz wdrażania wywoła odwołanie do zestawu uruchamiania hostingu (plik *. dll* ) i przenosząc zestaw do jednego z tych metod:
  * Projekt zużywający.
  * Lokalizacja dostępna przez projekt zużywający.
* Plik zależności startowe hostingu ma zostać udostępnione rozszerzone aplikacji zgodnie z opisem w [magazynu środowiska uruchomieniowego](#runtime-store) sekcji (bez odwołania do kompilacji).
* Podczas określania .NET Framework, zestaw jest ładowany w domyślnym kontekście ładowania, który na .NET Framework oznacza, że zestaw znajduje się w jednej z następujących lokalizacji:
  * Ścieżka &ndash; podstawowa aplikacji folder *bin* , w którym znajduje się plik wykonywalny ( *. exe*) aplikacji.
  * Globalna pamięć podręczna zestawów &ndash; (GAC) przechowuje zestawy, które udostępniają kilka .NET Framework aplikacji. Aby uzyskać więcej informacji, zobacz [jak: Zainstaluj zestaw w globalnej pamięci podręcznej](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) zestawów w dokumentacji .NET Framework.

## <a name="sample-code"></a>Przykładowy kod

[Przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample)) pokazuje hostingu scenariuszom dla programów próbnych implementacji:

* Dwa hostingu zestawy startowe (bibliotek klas) Ustaw parę pary klucz wartość w pamięci konfiguracji poszczególnych:
  * Pakiet NuGet (*HostingStartupPackage*)
  * Biblioteka klas (*HostingStartupLibrary*)
* Uruchamianie hostingu została aktywowana w zestawie środowiska uruchomieniowego wdrożonych w magazynie (*StartupDiagnostics*). Zestaw dodaje dwa middlewares do aplikacji podczas uruchamiania, które zawierają informacje diagnostyczne dotyczące:
  * Zarejestrowane usługi
  * Adres (schematu, hosta, podstawa ścieżki, ścieżki, ciąg zapytania)
  * Połączenia (zdalnego adresu IP, port zdalny lokalnego adresu IP, port lokalny, certyfikatu klienta)
  * Nagłówki żądań
  * Zmienne środowiskowe

Do uruchomienia przykładu:

**Aktywacja z pakietu NuGet**

1. Skompilować *HostingStartupPackage* pakietu z [pakietu dotnet](/dotnet/core/tools/dotnet-pack) polecenia.
1. Dodaj nazwę zestawu pakietu *HostingStartupPackage* do `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.
1. Skompiluj i uruchom aplikację. Odwołanie do pakietu jest obecny w aplikacji rozszerzone (odwołanie w czasie kompilacji). A `<PropertyGroup>` w projekcie aplikacji plik Określa danych wyjściowych projektu pakietu ( *... / HostingStartupPackage/bin/Debug*) jako źródło pakietów. Umożliwia to aplikacji na użycie pakietu bez przekazywania pakietu, który ma [nuget.org](https://www.nuget.org/). Aby uzyskać więcej informacji, zobacz uwagi w pliku projektu HostingStartupApp.

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
1. Skompiluj i uruchom aplikację. `<ItemGroup>` w projekcie aplikacji plik odwołuje się do zestawu biblioteki klas ( *.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (odwołanie w czasie kompilacji). Aby uzyskać więcej informacji, zobacz uwagi w pliku projektu HostingStartupApp.

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
1. Wykonaj skrypt *Build. ps1* w folderze *RuntimeStore* . Skrypt:
   * `StartupDiagnostics` Generuje pakiet.
   * Generuje magazyn środowiska uruchomieniowego `StartupDiagnostics` dla programu w folderze *Store* . Polecenie w skrypcie `win7-x64` używa [identyfikatora środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog) do uruchomienia hostingu wdrożonego w systemie Windows. `dotnet store` Podczas zapewniania uruchamiania hostingu dla innego środowiska uruchomieniowego należy zastąpić prawidłowy identyfikator RID w wierszu 37 skryptu.
   * Generuje element `StartupDiagnostics`forw programie *additionalDeps/Shared/Microsoft. AspNetCore. app/{Shared Framework Version}/folder.* `additionalDeps`
   * Umieszcza plik *Deploy. ps1* w folderze *Deployment* .
1. Uruchom skrypt *Deploy. ps1* w folderze *Deployment* . Dołączenie skryptu:
   * `StartupDiagnostics`do zmiennej `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` środowiskowej.
   * Ścieżka zależności uruchamiania hostingu do `DOTNET_ADDITIONAL_DEPS` zmiennej środowiskowej.
   * Ścieżka do magazynu środowiska uruchomieniowego `DOTNET_SHARED_STORE` do zmiennej środowiskowej.
1. Uruchom przykładową aplikację.
1. Żądanie `/services` punktu końcowego, aby wyświetlić aplikację zarejestrowane usługi. Żądanie `/diag` punktu końcowego, aby wyświetlić informacje diagnostyczne.
