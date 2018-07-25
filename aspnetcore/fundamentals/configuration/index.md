---
title: Konfiguracja w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak skonfigurować aplikację ASP.NET Core za pomocą interfejsu API konfiguracji.
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 59ab0cd0f6975d15bd01ce7e4128521938182c24
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228627"
---
# <a name="configuration-in-aspnet-core"></a>Konfiguracja w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Michaelis znacznik](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), i [Luke Latham](https://github.com/guardrex)

Interfejs API konfiguracji umożliwia konfigurowanie platformy ASP.NET Core aplikacji sieci web, w oparciu o listę par nazwa wartość. Konfiguracja jest do odczytu w czasie wykonywania z wielu źródeł. Pary nazwa wartość, można podzielić na wiele poziomu hierarchii.

Istnieją dostawców konfiguracji:

* Formaty plików (INI, JSON i XML).
* Argumenty wiersza polecenia.
* Zmienne środowiskowe.
* Obiekty .NET w pamięci.
* Niezaszyfrowane [Menedżera klucz tajny](xref:security/app-secrets) magazynu.
* Użytkownik zaszyfrowanych przechowywanych informacji, takich jak [usługi Azure Key Vault](xref:security/key-vault-configuration).
* Dostawcy niestandardowi (zainstalowane lub utworzone).

Każda wartość konfiguracji mapuje klucza typu ciąg. Obsługuje wbudowanych powiązania do deserializacji ustawienia niestandardowego [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) obiektu (.NET klasą prostą przy użyciu właściwości).

Wzorzec opcje używa klas opcji do reprezentowania grup powiązane ustawienia. Aby uzyskać więcej informacji na temat korzystania z wzorca opcji, zobacz [opcje](xref:fundamentals/configuration/options) tematu.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.1"

Przykłady podane w tym temacie opierają się na:

* Ustawianie ścieżki podstawowej aplikacji za pomocą [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath). `SetBasePath` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakietu.
* Rozpoznawanie sekcji plików konfiguracji przy użyciu [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection). `GetSection` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakietu.
* Konfiguracja powiązania z [powiązać](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind). `Bind` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) pakietu.

Te pakiety są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Przykłady podane w tym temacie opierają się na:

* Ustawianie ścieżki podstawowej aplikacji za pomocą [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath). `SetBasePath` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakietu.
* Rozpoznawanie sekcji plików konfiguracji przy użyciu [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection). `GetSection` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakietu.
* Konfiguracja powiązania z [powiązać](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind). `Bind` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) pakietu.

Te pakiety są objęte [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

Przykłady podane w tym temacie opierają się na:

* Ustawianie ścieżki podstawowej aplikacji za pomocą [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath). `SetBasePath` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) pakietu.
* Rozpoznawanie sekcji plików konfiguracji przy użyciu [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection). `GetSection` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) pakietu.
* Konfiguracja powiązania z [powiązać](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind). `Bind` ma zostać udostępnione do aplikacji, odwołując się do [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) pakietu.

::: moniker-end

## <a name="json-configuration"></a>Konfiguracja JSON

Następujące aplikacji konsolowej używa dostawcy konfiguracji JSON:

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

Aplikacja odczytuje i wyświetla następujące ustawienia konfiguracji:

[!code-json[](index/sample/ConfigJson/appsettings.json)]

Konfiguracja obejmuje hierarchiczną listę par nazwa wartość, w których węzłach są oddzielone dwukropkiem (`:`). Aby pobrać wartość, należy pobrać `Configuration` indeksatora kluczem odpowiadający element:

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

Aby pracować z tablic w formacie JSON konfiguracji źródła, należy użyć indeks tablicy jako część ciągu rozdzielone średnikami. Poniższy przykład pobiera nazwę pierwszego elementu w poprzednim `wizards` tablicy:

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

Pary nazwa wartość zapisywane do wbudowanej [konfiguracji](/dotnet/api/microsoft.extensions.configuration) dostawcy są **nie** utrwalone. Jednak można utworzyć niestandardowego dostawcę, który zapisuje wartości. Zobacz [niestandardowego dostawcy konfiguracji](xref:fundamentals/configuration/index#custom-config-providers).

Poprzedni przykład używa konfiguracji indeksatora ma odczytać wartości. Konfiguracja dostępu poza `Startup`, użyj *wzorzec opcje*. Aby uzyskać więcej informacji, zobacz [opcje](xref:fundamentals/configuration/options) tematu.

## <a name="xml-configuration"></a>Plik XML konfiguracji

Aby pracować z tablic w źródła konfiguracji w formacie XML, podaj `name` indeksu do każdego elementu. Aby uzyskać dostęp do wartości, należy użyć indeksu:

```xml
<wizards>
  <wizard name="Gandalf">
    <age>1000</age>
  </wizard>
  <wizard name="Harry">
    <age>17</age>
  </wizard>
</wizards>
```

```csharp
Console.Write($"{Configuration["wizard:Harry:age"]}");
// Output: 17
```

## <a name="configuration-by-environment"></a>Konfiguracja według środowiska

Jest to zazwyczaj mieć różne ustawienia konfiguracji dla różnych środowisk, na przykład, rozwoju, testowania i produkcji. `CreateDefaultBuilder` Metody rozszerzenia w aplikacji ASP.NET Core 2.x (lub za pomocą `AddJsonFile` i `AddEnvironmentVariables` bezpośrednio w aplikacji ASP.NET Core 1.x) dodaje dostawcy konfiguracji do odczytywania źródeł konfiguracji systemu i pliki w formacie JSON:

* *appsettings.json*
* *appsettings.\<EnvironmentName>.json*
* Zmienne środowiskowe

Platforma ASP.NET Core 1.x aplikacji należy wywołać `AddJsonFile` i [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).

Zobacz [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) objaśnienia dotyczące parametrów. `reloadOnChange` jest obsługiwana tylko w programie ASP.NET Core 1.1 i nowszych.

Źródła konfiguracji są do odczytu w kolejności, że jest określona. W poprzednim kodzie zmienne środowiskowe są odczytywane ostatnio. Wszelkie wartości konfiguracji ustawiać za pośrednictwem Zastąp środowiska w dwóch dostawców na poprzednim.

Należy wziąć pod uwagę następujące *appsettings. Staging.JSON* pliku:

[!code-json[](index/sample/appsettings.Staging.json)]

W poniższym kodzie `Configure` odczytuje wartość `MyConfig`:

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]

Środowisko jest zazwyczaj równa `Development`, `Staging`, lub `Production`. Aby uzyskać więcej informacji, zobacz [używanie wielu środowisk](xref:fundamentals/environments).

Zagadnienia dotyczące konfiguracji:

* [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) ponownie załadować dane konfiguracji po jej zmianie.
* Klucze konfiguracji **nie** uwzględniana wielkość liter.
* **Nigdy nie** przechowywanie haseł i innych poufnych danych w konfiguracji dostawcy kodu lub pliki konfiguracyjne w formacie zwykłego tekstu. Nie używać kluczy tajnych w środowisku produkcyjnym w trakcie opracowywania lub środowisk testowych. Określ klucze tajne poza projektem, nie może być przypadkowo wszelkich starań, by repozytorium kodu źródłowego. Dowiedz się więcej o [jak używać wielu środowisk](xref:fundamentals/environments) i zarządzanie nimi [bezpieczne przechowywanie kluczy tajnych aplikacji w trakcie opracowywania](xref:security/app-secrets).
* Wartości konfiguracji hierarchiczne określonych w zmiennych środowiskowych, dwukropek (`:`) może nie działać na wszystkich platformach. Podwójnym podkreśleniem (`__`) jest obsługiwana przez wszystkie platformy.
* Podczas interakcji z konfiguracji interfejsu API, dwukropek (`:`) działa na wszystkich platformach.

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>Dostawca w pamięci i wiązania klasy POCO

Poniższy przykład przedstawia sposób użycia dostawcy w pamięci i powiąż go z klasy:

[!code-csharp[](index/sample/InMemory/Program.cs)]

Wartości konfiguracji są zwracane jako ciągi, ale powiązanie umożliwia konstrukcji obiektów. Powiązanie umożliwia pobieranie obiektów POCO lub wykresy nawet cały obiekt.

### <a name="getvalue"></a>GetValue

W poniższym przykładzie pokazano [GetValue&lt;T&gt; ](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) — metoda rozszerzenia:

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

ConfigurationBinder `GetValue<T>` metody umożliwia określenie wartości domyślnej (80 w przykładzie). `GetValue<T>` Służy do prostych scenariuszy i nie jest powiązana całej sekcji. `GetValue<T>` pobiera wartości skalarnych z `GetSection(key).Value` konwertowane do określonego typu.

## <a name="bind-to-an-object-graph"></a>Powiąż z wykresu obiektu

Każdy obiekt w klasie może być powiązany cyklicznie. Należy wziąć pod uwagę następujące `AppSettings` klasy:

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

Poniższy przykład tworzy powiązanie z `AppSettings` klasy:

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

**Platforma ASP.NET Core 1.1** i nowszej można użyć `Get<T>`, który współdziała z całą sekcję. `Get<T>` może być bardziej wygodne niż przy użyciu `Bind`. Poniższy kod przedstawia sposób użycia `Get<T>` z poprzednim przykładem:

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

Za pomocą następujących *appsettings.json* pliku:

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

Ten program wyświetla `Height 11`.

Poniższy kod może służyć do jednostki konfiguracji testu:

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a>Tworzenie niestandardowego dostawcy programu Entity Framework

W tej sekcji dostawcy konfiguracji podstawowej, która odczytuje pary nazwa wartość z bazy danych przy użyciu programu EF jest tworzony.

Zdefiniuj `ConfigurationValue` jednostki do przechowywania wartości konfiguracji w bazie danych:

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

Dodaj `ConfigurationContext` do przechowywania i uzyskać dostęp do skonfigurowanej wartości:

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

Utwórz klasę, która implementuje [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

Tworzenie niestandardowego dostawcy konfiguracji przez dziedziczenie z [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider). Dostawca konfiguracji inicjuje bazy danych, gdy jest on pusty:

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

Wyróżnione wartości z bazy danych ("value_from_ef_1" i "value_from_ef_2") są wyświetlane po uruchomieniu przykładu.

`EFConfigSource` Metody rozszerzenia umożliwiające dodawanie źródło konfiguracji można użyć:

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

Poniższy kod przedstawia sposób używania niestandardowego `EFConfigProvider`:

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

Przykładowa aplikacja dodaje niestandardową Uwaga `EFConfigProvider` po dostawcy JSON, więc wszystkie ustawienia z bazy danych spowoduje przesłonięcie ustawień z *appsettings.json* pliku.

Za pomocą następujących *appsettings.json* pliku:

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

Są wyświetlane następujące dane wyjściowe:

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>Dostawca konfiguracji wiersza polecenia

[Dostawcę konfiguracji CommandLine](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) odbiera pary klucz wartość argumentu wiersza polecenia dla konfiguracji w czasie wykonywania.

[Wyświetlanie lub pobieranie przykładowych konfiguracji wiersza polecenia](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a>Konfigurowanie i używanie dostawcę konfiguracji wiersza polecenia

# <a name="basic-configurationtabbasicconfiguration"></a>[Konfiguracja podstawowa](#tab/basicconfiguration/)

Aby uaktywnić konfigurację wiersza polecenia, należy wywołać `AddCommandLine` metody rozszerzenia w wystąpieniu [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

Uruchamianie kodu, są wyświetlane następujące dane wyjściowe:

```console
MachineName: MairaPC
Left: 1980
```

Przekazywanie argumentu pary klucz wartość w wierszu polecenia powoduje zmianę wartości `Profile:MachineName` i `App:MainWindow:Left`:

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

W oknie konsoli zostaną wyświetlone:

```console
MachineName: BartPC
Left: 1979
```

Aby zastąpić konfiguracji udostępnianych przez innych dostawców konfiguracji przy użyciu wiersza polecenia konfiguracji, należy wywołać `AddCommandLine` ostatni `ConfigurationBuilder`:

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Typowe aplikacje ASP.NET Core 2.x należy użyć metody statyczne wygody `CreateDefaultBuilder` do tworzenia hosta:

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

`CreateDefaultBuilder` ładuje opcjonalna konfiguracja z *appsettings.json*, *appsettings. { Środowisko} .json*, [wpisami tajnymi użytkowników](xref:security/app-secrets) (w `Development` środowiska), zmienne środowiskowe i argumenty wiersza polecenia. Dostawca konfiguracji wiersza polecenia nazywa się ostatnio. Ostatnie wywoływania dostawcy umożliwia argumenty wiersza polecenia przekazywane w czasie wykonywania, aby zastąpić konfiguracji ustawione przez innych dostawców konfiguracji o nazwie wcześniej.

Aby uzyskać *appsettings* pliki where:

* `reloadOnChange` jest włączone.
* Zawierać tego samego ustawienia w argumentach wiersza polecenia i *appsettings* pliku.
* *Appsettings* plik zawierający pasującego argumentu wiersza polecenia, to ulegnie zmianie po uruchomieniu aplikacji.

Jeśli wszystkie powyższe warunki są spełnione, zostaną zastąpione argumenty wiersza polecenia.

Można użyć aplikacji programu ASP.NET Core 2.x [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) zamiast `CreateDefaultBuilder`. Korzystając z `WebHostBuilder`, ręcznie konfiguracji zestawu z [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder). Zobacz kartę platformy ASP.NET Core 1.x, aby uzyskać więcej informacji.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Tworzenie [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) i wywołać `AddCommandLine` metodę CommandLine dostawcę konfiguracji. Ostatnie wywoływania dostawcy umożliwia argumenty wiersza polecenia przekazywane w czasie wykonywania, aby zastąpić konfiguracji ustawione przez innych dostawców konfiguracji o nazwie wcześniej. Zastosuj konfigurację do [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) z `UseConfiguration` metody:

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>Argumenty

Argumenty wiersza polecenia musi być zgodny z jednym z dwóch formatów pokazano w poniższej tabeli:

| Format argumentu:                                                     | Przykład        |
| ------------------------------------------------------------------- | :------------: |
| Pojedynczy argument: pary klucz wartość oddzielone znak równości (`=`) | `key1=value`   |
| Sekwencja dwa argumenty: pary klucz wartość oddzielone spacją    | `/key1 value1` |

**Jeden argument**

Wartość musi stosować się znak równości (`=`). Może mieć wartości null (na przykład `mykey=`).

Klucz może mieć prefiksu.

| Prefiks klucza               | Przykład         |
| ------------------------ | :-------------: |
| Nie prefiksu                | `key1=value1`   |
| Pojedynczy dash (`-`)&#8224; | `-key2=value2`  |
| Dwa łączniki (`--`)        | `--key3=value3` |
| Kreski ułamkowej (`/`)      | `/key4=value4`  |

&#8224;Klucz z prefiksem jeden (`-`) musi być podana w [Przełącz mapowania](#switch-mappings), które zostały opisane poniżej.

Przykładowe polecenie:

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

Uwaga: Jeśli `-key2` nie jest obecny w [Przełącz mapowania](#switch-mappings) przekazany dostawcy konfiguracji `FormatException` zgłaszany.

**Sekwencja dwóch argumentów**

Wartość nie może mieć wartości null i muszą być zgodne klucza, rozdzielonych spacją.

Klucz musi mieć prefiks.

| Prefiks klucza               | Przykład         |
| ------------------------ | :-------------: |
| Pojedynczy dash (`-`)&#8224; | `-key1 value1`  |
| Dwa łączniki (`--`)        | `--key2 value2` |
| Kreski ułamkowej (`/`)      | `/key3 value3`  |

&#8224;Klucz z prefiksem jeden (`-`) musi być podana w [Przełącz mapowania](#switch-mappings), które zostały opisane poniżej.

Przykładowe polecenie:

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

Uwaga: Jeśli `-key1` nie jest obecny w [Przełącz mapowania](#switch-mappings) przekazany dostawcy konfiguracji `FormatException` zgłaszany.

### <a name="duplicate-keys"></a>Zduplikowane klucze

Jeśli zduplikowane klucze są dostarczane, używana jest ostatnia pary klucz wartość.

### <a name="switch-mappings"></a>Przełącz mapowania

Podczas ręcznego tworzenia konfiguracji za pomocą `ConfigurationBuilder`, przełącznik słownik mapowań mogą być dodawane do `AddCommandLine` metody. Przełącznik jest transformowanie na nazwę klucza wymiany logiki.

W przypadku przełącznika słownik mapowań słownika jest sprawdzane dla klucza, który pasuje do klucza, dostarczone przez argument wiersza polecenia. Jeśli klucz wiersza polecenia zostanie znaleziony w słowniku, wartość słownika (wymiana klucza) jest przekazywana do ustawiania konfiguracji. Mapowanie przełącznika jest wymagane dla dowolnego klucz wiersza polecenia poprzedzone kreską pojedynczego (`-`).

Przełącz mapowania słownika kluczowych reguł:

* Przełączniki musi rozpoczynać się kreską (`-`) lub podwójne kreska (`--`).
* Słownik mapowań przełącznik nie może zawierać zduplikowane klucze.

W poniższym przykładzie `GetSwitchMappings` metoda umożliwia argumenty wiersza polecenia, użyj jednej kreski (`-`) prefiks klucza i uniknąć wiodących prefiksy podklucza.

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

Bez podawania argumenty wiersza polecenia, udostępniane słownik `AddInMemoryCollection` ustawia wartości konfiguracji. Uruchom aplikację za pomocą następującego polecenia:

```console
dotnet run
```

W oknie konsoli zostaną wyświetlone:

```console
MachineName: RickPC
Left: 1980
```

Aby przekazać w ustawieniach konfiguracji, należy użyć następujących:

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

W oknie konsoli zostaną wyświetlone:

```console
MachineName: DahliaPC
Left: 1984
```

Po utworzeniu przełącznika słownik mapowań zawiera dane wyświetlane w poniższej tabeli:

| Key            | Wartość                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

Aby zademonstrować, przełączanie klucza, używając słownika, uruchom następujące polecenie:

```console
dotnet run -MachineName=ChadPC -Left=1988
```

Klucze wiersza polecenia zostały zamienione. W oknie konsoli zostaną wyświetlone wartości konfiguracji dla `Profile:MachineName` i `App:MainWindow:Left`:

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a>plik Web.config

A *web.config* plik jest wymagany w przypadku hostowania aplikacji w usługach IIS lub IIS Express. Ustawienia w *web.config* Włącz [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) do uruchomienia aplikacji i skonfigurować inne ustawienia usług IIS i modułów. Jeśli *web.config* plik nie jest obecny i plik projektu zawiera `<Project Sdk="Microsoft.NET.Sdk.Web">`, publikowania projektu tworzy *web.config* pliku w opublikowanych danych wyjściowych ( *publikowania* folder). Aby uzyskać więcej informacji, zobacz [hosta ASP.NET Core na Windows z programem IIS](xref:host-and-deploy/iis/index#webconfig-file).

## <a name="access-configuration-during-startup"></a>Konfiguracja dostępu podczas uruchamiania

Uzyskać dostępu do konfiguracji w ramach `ConfigureServices` lub `Configure` podczas uruchamiania, zapoznaj się z przykładami w [uruchamiania aplikacji](xref:fundamentals/startup) tematu.

## <a name="adding-configuration-from-an-external-assembly"></a>Dodawanie konfiguracji z zewnętrznego zestawu

[Interfejsu IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementacji umożliwia dodawanie rozszerzeń do aplikacji przy uruchamianiu z zewnętrznego zestawu poza aplikacji `Startup` klasy. Aby uzyskać więcej informacji, zobacz [zwiększanie możliwości aplikacji z zewnętrznego zestawu](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="access-configuration-in-a-razor-page-or-mvc-view"></a>Konfiguracja dostępu w widoku MVC lub strony Razor

Aby uzyskać dostęp do ustawień konfiguracji na stronie stron Razor lub widoku MVC, należy dodać [użycie dyrektywy](xref:mvc/views/razor#using) ([odwołanie w C#: using — dyrektywa](/dotnet/csharp/language-reference/keywords/using-directive)) dla [Microsoft.Extensions.Configuration przestrzeni nazw ](/dotnet/api/microsoft.extensions.configuration) i wstawiać [wartości IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) do strony lub widoku.

Na stronie stron Razor:

```cshtml
@page
@model IndexModel

@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

W widoku MVC:

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

## <a name="additional-notes"></a>Dodatkowe uwagi

* Wstrzykiwanie zależności (DI) nie została ustawiona w górę do momentu po `ConfigureServices` zostanie wywołana.
* System konfiguracji nie jest DI aware.
* `IConfiguration` ma dwie specjalizacje:
  * `IConfigurationRoot` Używany do węzła głównego. Można uruchomić ponownie załadować.
  * `IConfigurationSection` Reprezentuje sekcję konfiguracji wartości. `GetSection` i `GetChildren` metody zwracają `IConfigurationSection`.
  * Użyj [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) podczas ponownego ładowania konfiguracji lub w celu uzyskania dostępu do poszczególnych dostawców. Żadna z tych sytuacji nie występują.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Opcje](xref:fundamentals/configuration/options)
* [Używanie wielu środowisk](xref:fundamentals/environments)
* [Bezpieczne przechowywanie wpisów tajnych aplikacji w czasie projektowania](xref:security/app-secrets)
* [Hosta w programie ASP.NET Core](xref:fundamentals/host/index)
* [Wstrzykiwanie zależności](xref:fundamentals/dependency-injection)
* [Dostawca konfiguracji usługi Azure Key Vault](xref:security/key-vault-configuration)
