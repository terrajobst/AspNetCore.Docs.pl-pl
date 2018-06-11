---
title: Konfiguracja platformy ASP.NET Core
author: rick-anderson
description: Interfejs API konfiguracji umożliwia konfigurowanie aplikacji platformy ASP.NET Core za pomocą wielu metod.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/index
ms.openlocfilehash: 1048554c78e3810206b1261371ae7c41485c436a
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252129"
---
# <a name="configuration-in-aspnet-core"></a>Konfiguracja platformy ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Michaelis znak](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Roth Danielowi](https://github.com/danroth27), i [Luke Latham](https://github.com/guardrex)

Interfejs API konfiguracji umożliwia konfigurowanie platformy ASP.NET Core aplikacji sieci web na podstawie listy par nazwa wartość. Konfiguracja jest do odczytu w czasie wykonywania z wielu źródeł. Pary nazwa wartość można grupować w wielopoziomowej hierarchii.

Brak dostawcy konfiguracji:

* Format pliku (INI, JSON i XML).
* Argumenty wiersza polecenia.
* Zmienne środowiskowe.
* Obiekty .NET w pamięci.
* Niezaszyfrowane [Manager klucz tajny](xref:security/app-secrets) magazynu.
* Przechowywać zaszyfrowane użytkownika, takich jak [usługi Azure Key Vault](xref:security/key-vault-configuration).
* Dostawców niestandardowych (zainstalowane lub utworzone).

Każda wartość konfiguracji jest mapowany na klucz ciągu. Brak obsługi wbudowanych powiązanie zdeserializować ustawień do niestandardowego [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) obiektu (.NET klasą prostą z właściwościami).

Wzorzec opcje używa klas opcji do reprezentowania grupy ustawień. Aby uzyskać więcej informacji o wykorzystaniu wzorca opcji, zobacz [opcje](xref:fundamentals/configuration/options) tematu.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="json-configuration"></a>JSON konfiguracji

Następujące aplikacji konsoli używa dostawcy konfiguracji JSON:

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

Odczytuje i wyświetla następujące ustawienia konfiguracji aplikacji:

[!code-json[](index/sample/ConfigJson/appsettings.json)]

Konfiguracja obejmuje hierarchiczną listę par nazwa wartość, w których węzły są oddzielone dwukropkiem (`:`). Aby pobrać wartość, należy pobrać `Configuration` indeksatora kluczem odpowiedniego elementu:

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

Aby pracować z tablic w formacie JSON konfiguracji źródła, należy użyć indeks tablicy jako część ciągu oddzielone dwukropkiem. Poniższy przykład pobiera nazwę pierwszego elementu w poprzednim `wizards` tablicy:

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

Pary nazwa wartość zapisywane do wbudowanej [konfiguracji](/dotnet/api/microsoft.extensions.configuration) dostawcy są **nie** utrwalone. Jednak można utworzyć niestandardowego dostawcę, który zapisuje wartości. Zobacz [niestandardowego dostawcy konfiguracji](xref:fundamentals/configuration/index#custom-config-providers).

Powyższego przykładu używa indeksatora konfiguracji można odczytać wartości. Do konfiguracji dostępu poza `Startup`, użyj *wzorzec opcje*. Aby uzyskać więcej informacji, zobacz [opcje](xref:fundamentals/configuration/options) tematu.

## <a name="xml-configuration"></a>Konfiguracja XML

Aby pracować z tablic w formacie XML konfiguracji źródła, należy podać `name` indeksu do każdego elementu. Użyj indeksu, aby uzyskać dostęp do wartości:

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

## <a name="configuration-by-environment"></a>Konfiguracja przez środowisko

Jest to typowe do innej konfiguracji ustawień dla różnych środowisk, na przykład programowania, testowania i produkcji. `CreateDefaultBuilder` — Metoda rozszerzenia w aplikacji platformy ASP.NET Core 2.x (lub przy użyciu `AddJsonFile` i `AddEnvironmentVariables` bezpośrednio w aplikacji platformy ASP.NET Core 1.x) dodaje dostawców konfiguracji do odczytywania źródeł konfiguracji systemu i pliki w formacie JSON:

* *appsettings.json*
* *appsettings.\<EnvironmentName>.json*
* Zmienne środowiskowe

Aplikacje 1.x platformy ASP.NET Core musi wywołać `AddJsonFile` i [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).

Zobacz [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) dla objaśnienie parametrów. `reloadOnChange` jest obsługiwana tylko w ASP.NET Core 1.1 lub nowszej.

Konfiguracja źródła są do odczytu w kolejności, że jest określona. W powyższym kodzie zmienne środowiskowe są odczytywane ostatnio. Wartości konfiguracji ustawiana za pośrednictwem Zamień środowiska określone w poprzednich dwóch dostawców.

Należy rozważyć *appsettings. Staging.JSON* pliku:

[!code-json[](index/sample/appsettings.Staging.json)]

Jeśli środowisko jest równa `Staging`, następujące `Configure` metoda odczytuje wartość `MyConfig`:

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]

Środowisko jest zwykle ustawiana na `Development`, `Staging`, lub `Production`. Aby uzyskać więcej informacji, zobacz [używać wiele środowisk](xref:fundamentals/environments).

Zagadnienia dotyczące konfiguracji:

* [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) można ponownie załadować dane konfiguracji, gdy zmienia.
* Klucze konfiguracji **nie** z uwzględnieniem wielkości liter.
* **Nigdy nie** przechowywania haseł i innych poufnych danych w konfiguracji dostawcy kodu lub pliki konfiguracyjne w formacie zwykłego tekstu. Nie używasz produkcji kluczy tajnych w rozwoju lub testowania środowisk. Określ klucze tajne poza projektem, aby nie może być przypadkowo przekazane do repozytorium kodu źródłowego. Dowiedz się więcej o [jak używać wiele środowisk](xref:fundamentals/environments) i zarządzanie [bezpiecznego magazynu kluczy tajnych aplikacji do rozwoju](xref:security/app-secrets).
* Dla wartości hierarchiczna konfiguracji określonych w zmiennych środowiskowych, dwukropek (`:`) może nie działać na wszystkich platformach. Podwójne podkreślenia (`__`) jest obsługiwana przez wszystkie platformy.
* Gdy korzysta z konfiguracji interfejsu API, dwukropek (`:`) działa na wszystkich platformach.

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>Dostawca w pamięci i powiązanie z klasą POCO

Poniższy przykład przedstawia sposób korzystają z dostawcy w pamięci i powiąż do klasy:

[!code-csharp[](index/sample/InMemory/Program.cs)]

Wartości konfiguracji są zwracane jako ciągi, ale powiązanie umożliwia konstrukcji obiektów. Powiązanie umożliwia pobieranie obiektów POCO lub wykresy nawet całego obiektu.

### <a name="getvalue"></a>GetValue

W poniższym przykładzie pokazano [GetValue&lt;T&gt; ](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) — metoda rozszerzenia:

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

ConfigurationBinder `GetValue<T>` metody umożliwia określenie wartości domyślnej (80 w próbce). `GetValue<T>` Służy do scenariuszy proste i nie należy powiązać całą sekcję. `GetValue<T>` pobiera wartości skalarnych z `GetSection(key).Value` przekonwertować dla określonego typu.

## <a name="bind-to-an-object-graph"></a>Powiązanie do obiektu wykresu.

Każdy obiekt w klasie można rekursywnie powiązany. Należy rozważyć `AppSettings` klasy:

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

Poniższy przykład tworzy powiązanie z `AppSettings` klasy:

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

**Platformy ASP.NET Core 1.1** i wyższe służy `Get<T>`, który współpracuje z całą sekcję. `Get<T>` może być wygodniejsze niż przy użyciu `Bind`. Poniższy kod przedstawia sposób użycia `Get<T>` z powyższego przykładu:

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

Użycie następujących *appsettings.json* pliku:

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

Wyświetla program `Height 11`.

Poniższy kod może służyć do jednostki jest przetestowanie konfiguracji:

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

W tej sekcji tworzony jest odczytujący pary nazwa wartość z bazy danych przy użyciu EF dostawca konfiguracji podstawowej.

Zdefiniuj `ConfigurationValue` jednostki do przechowywania wartości konfiguracji w bazie danych:

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

Dodaj `ConfigurationContext` do przechowywania i uzyskać dostęp do skonfigurowanego wartości:

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

Utwórz klasę, która implementuje [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

Tworzenie niestandardowego dostawcy konfiguracji przez dziedziczenie z [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider). Dostawca konfiguracji inicjuje bazy danych, gdy nie jest pusty:

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

Wyróżnione wartości z bazy danych ("value_from_ef_1" i "value_from_ef_2") są wyświetlane po uruchomieniu próbki.

`EFConfigSource` Metody rozszerzenia umożliwiające dodawanie źródło konfiguracji można użyć:

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

Poniższy kod przedstawia sposób użycia niestandardowego `EFConfigProvider`:

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

Uwaga próbki dodaje niestandardowego `EFConfigProvider` po dostawcy JSON tak wszystkie ustawienia z bazy danych spowoduje zastąpienie ustawień z *appsettings.json* pliku.

Użycie następujących *appsettings.json* pliku:

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

Wyświetlane są następujące dane wyjściowe:

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>Dostawca konfiguracji wiersza polecenia

[Dostawcę konfiguracji CommandLine](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) odbiera pary klucz wartość argumentu wiersza polecenia dla konfiguracji w czasie wykonywania.

[Wyświetlanie lub pobieranie przykładowej konfiguracji wiersza polecenia](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a>Skonfigurować i stosować dostawcę konfiguracji wiersza polecenia

# <a name="basic-configurationtabbasicconfiguration"></a>[Konfiguracja podstawowa](#tab/basicconfiguration/)

Aby aktywować konfigurację wiersza polecenia, należy wywołać `AddCommandLine` — metoda rozszerzenia w wystąpieniu [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

Wykonywanie kodu, wyświetlane są następujące dane wyjściowe:

```console
MachineName: MairaPC
Left: 1980
```

Przekazywanie argumentów pary klucz wartość w wierszu polecenia zmienia wartości `Profile:MachineName` i `App:MainWindow:Left`:

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

Wyświetla okno konsoli:

```console
MachineName: BartPC
Left: 1979
```

Aby zastąpić konfiguracji dostarczanych przez innych dostawców konfiguracji z wiersza polecenia konfiguracji, należy wywołać `AddCommandLine` ostatniego na `ConfigurationBuilder`:

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Typowe aplikacje 2.x platformy ASP.NET Core należy użyć metody statycznej wygody `CreateDefaultBuilder` tworzenie hosta:

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

`CreateDefaultBuilder` ładuje konfiguracji opcjonalnej z *appsettings.json*, *appsettings. { Środowisko} JSON*, [kluczy tajnych użytkownika](xref:security/app-secrets) (w `Development` środowiska), zmienne środowiskowe i argumenty wiersza polecenia. Dostawca konfiguracji wiersza polecenia nazywa się ostatnio. Ostatnio wywoływania dostawcy umożliwia wcześniej o nazwie argumenty wiersza polecenia przekazywane w czasie wykonywania, aby zastąpić konfiguracji ustawione przez innych dostawców konfiguracji.

Aby uzyskać *appsettings* pliki gdzie:

* `reloadOnChange` jest włączone.
* Zawiera tego samego ustawienia w argumentach wiersza polecenia i *appsettings* pliku.
* *Appsettings* plik zawierający pasujący argument wiersza polecenia zostaną zmienione po uruchomieniu aplikacji.

Jeśli wszystkie powyższe warunki są spełnione, argumenty wiersza polecenia są zastępowane.

Można użyć aplikacji 2.x platformy ASP.NET Core [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) zamiast `CreateDefaultBuilder`. Korzystając z `WebHostBuilder`, ręcznie Konfiguracja zestawu z [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder). Zobacz kartę 1.x platformy ASP.NET Core, aby uzyskać więcej informacji.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Utwórz [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) i Wywołaj `AddCommandLine` — metoda korzysta z wiersza polecenia dostawcy konfiguracji. Ostatnio wywoływania dostawcy umożliwia wcześniej o nazwie argumenty wiersza polecenia przekazywane w czasie wykonywania, aby zastąpić konfiguracji ustawione przez innych dostawców konfiguracji. Zastosuj konfigurację do [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) z `UseConfiguration` metody:

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>Argumenty

Argumenty wiersza polecenia musi być zgodna z jednym z dwóch formatów pokazano w poniższej tabeli:

| Format argumentu                                                     | Przykład        |
| ------------------------------------------------------------------- | :------------: |
| Pojedynczy argument: pary klucz wartość oddzielone znakiem równości (`=`) | `key1=value`   |
| Sekwencja dwa argumenty: pary klucz wartość oddzielone spacją    | `/key1 value1` |

**Jeden argument**

Wartość musi występować po znaku równości (`=`). Wartość może mieć wartości null (na przykład `mykey=`).

Klucz może mieć prefiksu.

| Prefiks klucza               | Przykład         |
| ------------------------ | :-------------: |
| Bez prefiksu                | `key1=value1`   |
| Pojedynczy dash (`-`)&#8224; | `-key2=value2`  |
| Dwa łączniki (`--`)        | `--key3=value3` |
| Ukośnika (`/`)      | `/key4=value4`  |

&#8224;Klucz z prefiksem pojedynczego dash (`-`) musi być wprowadzona w [przełącznika mapowania](#switch-mappings), które zostały opisane poniżej.

Przykładowe polecenie:

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

Uwaga: Jeśli `-key2` nie jest obecny w [przełącznika mapowania](#switch-mappings) przekazany dostawcy konfiguracji `FormatException` jest generowany.

**Sekwencja dwa argumenty**

Wartość nie może być pusta i musi występować po klucz rozdzielonych spacją.

Klucz musi mieć prefiks.

| Prefiks klucza               | Przykład         |
| ------------------------ | :-------------: |
| Pojedynczy dash (`-`)&#8224; | `-key1 value1`  |
| Dwa łączniki (`--`)        | `--key2 value2` |
| Ukośnika (`/`)      | `/key3 value3`  |

&#8224;Klucz z prefiksem pojedynczego dash (`-`) musi być wprowadzona w [przełącznika mapowania](#switch-mappings), które zostały opisane poniżej.

Przykładowe polecenie:

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

Uwaga: Jeśli `-key1` nie jest obecny w [przełącznika mapowania](#switch-mappings) przekazany dostawcy konfiguracji `FormatException` jest generowany.

### <a name="duplicate-keys"></a>Zduplikowane klucze

Jeśli zduplikowane klucze są dostarczane, używana jest ostatnią parę klucz wartość.

### <a name="switch-mappings"></a>Mapowania przełącznika

Podczas ręcznego tworzenia konfiguracji o `ConfigurationBuilder`, słownik mapowań przełącznika mogą być dodawane do `AddCommandLine` metody. Mapowanie przełącznika umożliwić nazwę klucza wymiany logiki.

W przypadku przełącznika słownik mapowań słownik jest sprawdzany pod kątem klucz pasujący do klucza dostarczonego przez argument wiersza polecenia. Jeśli klucz wiersza polecenia zostanie znaleziony w słowniku, wartość słownika (wymiana klucza) jest przekazywane z powrotem do konfiguracji. Mapowanie przełącznika jest wymagane dla dowolnego klucz wiersza polecenia i jest poprzedzony prefiksem jeden pulpit (`-`).

Przełącz zasady klucza słownika mapowania:

* Przełączniki musi rozpoczynać się kreską (`-`) lub kreska o podwójnej precyzji (`--`).
* Słownik mapowań przełącznik nie może zawierać zduplikowanych kluczy.

W poniższym przykładzie `GetSwitchMappings` metoda pozwala argumenty wiersza polecenia użyć jednego dash (`-`) a uniknąć początkowe prefiksy podkluczy dla klucza prefiks.

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

Bez konieczności podawania argumenty wiersza polecenia, podany słownik `AddInMemoryCollection` ustawia wartości konfiguracji. Uruchom aplikację za pomocą następującego polecenia:

```console
dotnet run
```

Wyświetla okno konsoli:

```console
MachineName: RickPC
Left: 1980
```

Aby przekazać w ustawieniach konfiguracji, skorzystaj z następujących:

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

Wyświetla okno konsoli:

```console
MachineName: DahliaPC
Left: 1984
```

Po utworzeniu przełącznika słownik mapowań zawiera dane wyświetlane w poniższej tabeli:

| Key            | Wartość                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

Aby zademonstrować przełączanie klucza za pomocą słownika, uruchom następujące polecenie:

```console
dotnet run -MachineName=ChadPC -Left=1988
```

Klucze wiersza polecenia są zamienione. W oknie konsoli wyświetla wartości konfiguracji `Profile:MachineName` i `App:MainWindow:Left`:

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a>plik Web.config

A *web.config* hosting aplikacji w usługach IIS lub usług IIS Express jest wymagany plik. Ustawienia w *web.config* włączyć [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) do uruchomienia aplikacji i skonfigurować inne ustawienia usług IIS i modułów. Jeśli *web.config* plik nie jest obecny i plik projektu zawiera `<Project Sdk="Microsoft.NET.Sdk.Web">`, publikowania projektu tworzy *web.config* plików publikowanych danych wyjściowych ( *publikowania* folderu). Aby uzyskać więcej informacji, zobacz [hosta platformy ASP.NET Core w systemie Windows z programem IIS](xref:host-and-deploy/iis/index#webconfig-file).

## <a name="access-configuration-during-startup"></a>Konfiguracja dostępu podczas uruchamiania

Do konfiguracji dostępu w ramach `ConfigureServices` lub `Configure` podczas uruchamiania, zobacz przykłady w [uruchamiania aplikacji](xref:fundamentals/startup) tematu.

## <a name="adding-configuration-from-an-external-assembly"></a>Dodawanie konfiguracji z zewnętrznego zestawu

[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementacji umożliwia dodawania rozszerzenia do aplikacji przy uruchamianiu z zewnętrznego zestawu poza aplikacji `Startup` klasy. Aby uzyskać więcej informacji, zobacz [ulepszyć aplikację z zewnętrznego zestawu](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="access-configuration-in-a-razor-page-or-mvc-view"></a>Konfiguracja dostępu w widoku Razor strony lub MVC

Aby uzyskać dostęp do ustawień konfiguracji na stronie aparatu Razor strony lub widok MVC, Dodaj [dyrektywa using](xref:mvc/views/razor#using) ([odwołanie w C#: dyrektywa using](/dotnet/csharp/language-reference/keywords/using-directive)) dla [Microsoft.Extensions.Configuration przestrzeni nazw ](/dotnet/api/microsoft.extensions.configuration) i wstrzyknąć [wartości IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) do strony lub widoku.

Na stronie aparatu Razor strony:

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

* Zależności Iniekcja nie został skonfigurowany do po `ConfigureServices` jest wywoływana.
* System konfiguracji nie jest świadome Podpisane.
* `IConfiguration` ma dwa specjalizacje:
  * `IConfigurationRoot` Użyty dla węzła głównego. Może spowodować ponowne załadowanie.
  * `IConfigurationSection` Reprezentuje sekcję konfiguracji wartości. `GetSection` i `GetChildren` metody zwracają `IConfigurationSection`.
  * Użyj [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) podczas ponownego ładowania konfiguracji lub dostęp do każdego dostawcy. Żadna z tych sytuacji są często używane.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Opcje](xref:fundamentals/configuration/options)
* [Używanie wielu środowisk](xref:fundamentals/environments)
* [Bezpieczne przechowywanie wpisów tajnych aplikacji w czasie projektowania](xref:security/app-secrets)
* [Host platformy ASP.NET Core](xref:fundamentals/host/index)
* [Wstrzykiwanie zależności](xref:fundamentals/dependency-injection)
* [Dostawca konfiguracji usługi Azure Key Vault](xref:security/key-vault-configuration)
