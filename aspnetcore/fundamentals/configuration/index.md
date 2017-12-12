---
title: Konfiguracja platformy ASP.NET Core
author: rick-anderson
description: "Interfejs API konfiguracji umożliwia konfigurowanie aplikacji platformy ASP.NET Core za pomocą wielu metod."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/index
ms.openlocfilehash: 6281d6ba254670b111964715410fc0694ae4d149
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="configure-an-aspnet-core-app"></a>Konfigurowanie aplikacji platformy ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Michaelis znak](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Roth Danielowi](https://github.com/danroth27), i [Luke Latham](https://github.com/guardrex)

Interfejs API konfiguracji umożliwia konfigurowanie platformy ASP.NET Core aplikacji sieci web na podstawie listy par nazwa wartość. Konfiguracja jest do odczytu w czasie wykonywania z wielu źródeł. Te pary nazwa wartość można grupować w wielopoziomowej hierarchii. 

Brak dostawcy konfiguracji:

* Format pliku (INI, JSON i XML)
* Argumenty wiersza polecenia
* Zmienne środowiskowe
* Obiekty .NET w pamięci
* Magazyn zaszyfrowanych użytkownika
* [Usługi Azure Key Vault](xref:security/key-vault-configuration)
* Dostawców niestandardowych (zainstalowane lub utworzone)

Każda wartość konfiguracji jest mapowany na klucz ciągu. Brak obsługi wbudowanych powiązanie zdeserializować ustawień do niestandardowego [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) obiektu (.NET klasą prostą z właściwościami).

Wzorzec opcje używa klas opcji do reprezentowania grupy ustawień. Aby uzyskać więcej informacji o wykorzystaniu wzorca opcji, zobacz [opcje](xref:fundamentals/configuration/options) tematu.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="json-configuration"></a>JSON konfiguracji

Następujące aplikacji konsoli używa dostawcy konfiguracji JSON:

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

Odczytuje i wyświetla następujące ustawienia konfiguracji aplikacji:

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

Konfiguracja obejmuje hierarchiczną listę par nazwa wartość, w których węzły są oddzielone dwukropkiem. Aby pobrać wartość, należy pobrać `Configuration` indeksatora kluczem odpowiedniego elementu:

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

Aby pracować z tablic w formacie JSON konfiguracji źródła, należy użyć indeks tablicy jako część ciągu oddzielone dwukropkiem. Poniższy przykład pobiera nazwę pierwszego elementu w poprzednim `wizards` tablicy:

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

Pary nazwa wartość zapisywane do wbudowanej `Configuration` dostawcy są **nie** utrwalone. Można jednak utworzyć niestandardowego dostawcę, który zapisuje wartości. Zobacz [niestandardowego dostawcy konfiguracji](xref:fundamentals/configuration/index#custom-config-providers).

Powyższego przykładu używa indeksatora konfiguracji można odczytać wartości. Do konfiguracji dostępu poza `Startup`, użyj *wzorzec opcje*. Aby uzyskać więcej informacji, zobacz [opcje](xref:fundamentals/configuration/options) tematu.

Jest to typowe do innej konfiguracji ustawień dla różnych środowisk, na przykład programowania, testowania i produkcji. `CreateDefaultBuilder` — Metoda rozszerzenia w aplikacji platformy ASP.NET Core 2.x (lub przy użyciu `AddJsonFile` i `AddEnvironmentVariables` bezpośrednio w aplikacji platformy ASP.NET Core 1.x) dodaje dostawców konfiguracji do odczytywania źródeł konfiguracji systemu i pliki w formacie JSON:

* *appSettings.JSON*
* *appSettings. \<EnvironmentName > JSON*
* Zmienne środowiskowe

Zobacz [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) dla objaśnienie parametrów. `reloadOnChange`jest obsługiwana tylko w ASP.NET Core 1.1 lub nowszej. 

Konfiguracja źródła są do odczytu w kolejności, że jest określona. W powyższym kodzie zmienne środowiskowe są odczytywane ostatnio. Wartości konfiguracji ustawiana za pośrednictwem Zamień środowiska określone w poprzednich dwóch dostawców.

Środowisko jest zwykle ustawiana na `Development`, `Staging`, lub `Production`. Zobacz [Praca w środowiskach wielu](xref:fundamentals/environments) Aby uzyskać więcej informacji.

Zagadnienia dotyczące konfiguracji:

* `IOptionsSnapshot`można ponownie załadować dane konfiguracji, gdy zmienia. Zobacz [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) Aby uzyskać więcej informacji.
* Klucze konfiguracji są bez uwzględniania wielkości liter.
* Określ zmienne środowiskowe ostatnio tak, aby przesłonić ustawienia w plikach konfiguracji wdrożonej w środowisku lokalnym.
* **Nigdy nie** przechowywania haseł i innych poufnych danych w konfiguracji dostawcy kodu lub pliki konfiguracyjne w formacie zwykłego tekstu. Nie używasz kluczy tajnych produkcji przy projektowaniu lub test środowisk. Zamiast tego należy określić klucze tajne poza projektem, tak aby nie może być przypadkowo przekazane do repozytorium. Dowiedz się więcej o [Praca w środowiskach wielu](xref:fundamentals/environments) i zarządzanie [bezpiecznego magazynu kluczy tajnych aplikacji podczas tworzenia](xref:security/app-secrets).
* Jeśli dwukropkiem (`:`) nie może być używany w zmiennych środowiskowych w systemie, Zastąp dwukropkiem (`:`) o podwójnej precyzji znak podkreślenia (`__`).

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>Dostawca w pamięci i powiązanie z klasą POCO

Poniższy przykład przedstawia sposób korzystają z dostawcy w pamięci i powiąż do klasy:

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

Wartości konfiguracji są zwracane jako ciągi, ale powiązanie umożliwia konstrukcji obiektów. Powiązanie umożliwia pobieranie obiektów POCO lub wykresy nawet całego obiektu.

### <a name="getvalue"></a>GetValue

W poniższym przykładzie pokazano [GetValue&lt;T&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) — metoda rozszerzenia:

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

ConfigurationBinder `GetValue<T>` metoda pozwala na określenie wartości domyślnej (80 w próbce). `GetValue<T>`Służy do scenariuszy proste i nie jest powiązana z całą sekcję. `GetValue<T>`pobiera wartości skalarnych z `GetSection(key).Value` przekonwertować dla określonego typu.

## <a name="bind-to-an-object-graph"></a>Powiązanie do obiektu wykresu.

Można rekursywnie bind do każdego obiektu w klasie. Należy rozważyć `AppSettings` klasy:

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

Poniższy przykład tworzy powiązanie z `AppSettings` klasy:

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

**Platformy ASP.NET Core 1.1** i wyższe służy `Get<T>`, który współpracuje z całą sekcję. `Get<T>`może być wygodniejsze niż przy użyciu `Bind`. Poniższy kod przedstawia sposób użycia `Get<T>` z powyższego przykładu:

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

Użycie następujących *appsettings.json* pliku:

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

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

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

Dodaj `ConfigurationContext` do przechowywania i uzyskać dostęp do skonfigurowanego wartości:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

Tworzenie klasy, która implementuje [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

Tworzenie niestandardowego dostawcy konfiguracji przez dziedziczenie z [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).  Dostawca konfiguracji inicjuje bazy danych, gdy nie jest pusty:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

Wyróżnione wartości z bazy danych ("value_from_ef_1" i "value_from_ef_2") są wyświetlane po uruchomieniu próbki.

Możesz dodać `EFConfigSource` metody rozszerzenia umożliwiające dodawanie źródło konfiguracji:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

Poniższy kod przedstawia sposób użycia niestandardowego `EFConfigProvider`:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

Uwaga próbki dodaje niestandardowego `EFConfigProvider` po dostawcy JSON tak wszystkie ustawienia z bazy danych spowoduje zastąpienie ustawień z *appsettings.json* pliku.

Użycie następujących *appsettings.json* pliku:

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

Wyświetlane są następujące:

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>Dostawca konfiguracji wiersza polecenia

[Dostawcę konfiguracji CommandLine](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) odbiera pary klucz wartość argumentu wiersza polecenia dla konfiguracji w czasie wykonywania.

[Wyświetlanie lub pobieranie przykładowej konfiguracji wiersza polecenia](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a>Skonfigurować i stosować dostawcę konfiguracji wiersza polecenia

# <a name="basic-configurationtabbasicconfiguration"></a>[Konfiguracja podstawowa](#tab/basicconfiguration)

Aby aktywować konfigurację wiersza polecenia, należy wywołać `AddCommandLine` — metoda rozszerzenia w wystąpieniu [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

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

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

Typowe aplikacje 2.x platformy ASP.NET Core należy użyć metody statycznej wygody `CreateDefaultBuilder` tworzenie hosta:

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

`CreateDefaultBuilder`ładuje konfiguracji opcjonalnej z *appsettings.json*, *appsettings. { Środowisko} JSON*, [kluczy tajnych użytkownika](xref:security/app-secrets) (w `Development` środowiska), zmienne środowiskowe i argumenty wiersza polecenia. Dostawca konfiguracji wiersza polecenia nazywa się ostatnio. Ostatnio wywoływania dostawcy umożliwia wcześniej o nazwie argumenty wiersza polecenia przekazywane w czasie wykonywania, aby zastąpić konfiguracji ustawione przez innych dostawców konfiguracji.

Należy pamiętać, że dla *appsettings* pliki `reloadOnChange` jest włączona. Argumenty wiersza polecenia zostały zastąpione, jeśli pasującej wartości konfiguracji w *appsettings* po uruchomieniu aplikacji zostanie zmieniony plik.

> [!NOTE]
> Zamiast przy użyciu `CreateDefaultBuilder` metody tworzenia do hosta przy użyciu [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) i ręcznego tworzenia konfiguracji o [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) jest obsługiwany w ASP.NET Core 2.x. Zobacz kartę 1.x platformy ASP.NET Core, aby uzyskać więcej informacji.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

Utwórz [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) i Wywołaj `AddCommandLine` — metoda korzysta z wiersza polecenia dostawcy konfiguracji. Ostatnio wywoływania dostawcy umożliwia wcześniej o nazwie argumenty wiersza polecenia przekazywane w czasie wykonywania, aby zastąpić konfiguracji ustawione przez innych dostawców konfiguracji. Zastosuj konfigurację do [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) z `UseConfiguration` metody:

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>Argumenty

Argumenty wiersza polecenia musi być zgodna z jednym z dwóch formatów pokazano w poniższej tabeli.

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
| Pojedynczy dash (`-`) &#8224; | `-key2=value2`  |
| Dwa łączniki (`--`)        | `--key3=value3` |
| Ukośnika (`/`)      | `/key4=value4`  |

&#8224; Klucz z prefiksem pojedynczego dash (`-`) musi być wprowadzona w [przełącznika mapowania](#switch-mappings), które zostały opisane poniżej.

Przykładowe polecenie:

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

Uwaga: Jeśli `-key1` nie jest obecny w [przełącznika mapowania](#switch-mappings) przekazany dostawcy konfiguracji `FormatException` jest generowany.

**Sekwencja dwa argumenty**

Wartość nie może być pusta i musi występować po klucz rozdzielonych spacją.

Klucz musi mieć prefiks.

| Prefiks klucza               | Przykład         |
| ------------------------ | :-------------: |
| Pojedynczy dash (`-`) &#8224; | `-key1 value1`  |
| Dwa łączniki (`--`)        | `--key2 value2` |
| Ukośnika (`/`)      | `/key3 value3`  |

&#8224; Klucz z prefiksem pojedynczego dash (`-`) musi być wprowadzona w [przełącznika mapowania](#switch-mappings), które zostały opisane poniżej.

Przykładowe polecenie:

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

Uwaga: Jeśli `-key1` nie jest obecny w [przełącznika mapowania](#switch-mappings) przekazany dostawcy konfiguracji `FormatException` jest generowany.

### <a name="duplicate-keys"></a>Zduplikowane klucze

Jeśli zduplikowane klucze są dostarczane, używana jest ostatnią parę klucz wartość.

### <a name="switch-mappings"></a>Mapowania przełącznika

Podczas ręcznego tworzenia konfiguracji o `ConfigurationBuilder`, opcjonalnie można podać słownik mapowań przełącznika, który `AddCommandLine` metody. Mapowanie przełącznika umożliwić należy podać nazwę klucza wymiany logiki.

W przypadku przełącznika słownik mapowań słownik jest sprawdzany pod kątem klucz pasujący do klucza dostarczonego przez argument wiersza polecenia. Jeśli klucz wiersza polecenia zostanie znaleziony w słowniku, wartość słownika (wymiana klucza) jest przekazywane z powrotem do konfiguracji. Mapowanie przełącznika jest wymagane dla dowolnego klucz wiersza polecenia i jest poprzedzony prefiksem jeden pulpit (`-`).

Przełącz zasady klucza słownika mapowania:

* Przełączniki musi rozpoczynać się kreską (`-`) lub kreska o podwójnej precyzji (`--`).
* Słownik mapowań przełącznik nie może zawierać zduplikowanych kluczy.

W poniższym przykładzie `GetSwitchMappings` metoda umożliwia Twojej argumenty wiersza polecenia użyj pojedynczego dash (`-`) a uniknąć początkowe prefiksy podkluczy dla klucza prefiks.

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

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

Po utworzeniu przełącznika słownik mapowań zawiera dane wyświetlane w poniższej tabeli.

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

## <a name="the-webconfig-file"></a>Plik web.config

A *web.config* hostowania aplikacji w usługach IIS lub usług IIS Express jest wymagany plik. *plik Web.config* włącza AspNetCoreModule w usługach IIS, aby uruchomić aplikację. Ustawienia w *web.config* włączyć AspNetCoreModule w usługach IIS do uruchomienia aplikacji i skonfigurować inne ustawienia usług IIS i modułów. Jeśli używasz programu Visual Studio i usunąć *web.config*, Visual Studio utworzy nowy.

## <a name="additional-notes"></a>Dodatkowe uwagi

* Zależności Iniekcja nie jest ustawiona do po `ConfigureServices` jest wywoływana.
* System konfiguracji nie jest świadome Podpisane.
* `IConfiguration`ma dwa specjalizacje:
  * `IConfigurationRoot`Użyty dla węzła głównego. Może spowodować ponowne załadowanie.
  * `IConfigurationSection`Reprezentuje sekcję konfiguracji wartości. `GetSection` i `GetChildren` metody zwracają `IConfigurationSection`.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Opcje](xref:fundamentals/configuration/options)
* [Praca w środowiskach wielu](xref:fundamentals/environments)
* [Bezpieczne przechowywanie klucze tajne aplikacji w czasie projektowania](xref:security/app-secrets)
* [Hosting w platformy ASP.NET Core](xref:fundamentals/hosting)
* [Iniekcji zależności](xref:fundamentals/dependency-injection)
* [Dostawca konfiguracji usługi Azure Key Vault](xref:security/key-vault-configuration)
