---
title: Konfiguracja w ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować aplikację ASP.NET Core przy użyciu interfejsu API konfiguracji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/10/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: 3dcabae3f76d81e641057c346dbb9097c2da42c7
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656332"
---
# <a name="configuration-in-aspnet-core"></a>Konfiguracja w ASP.NET Core

Autor [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

Konfiguracja aplikacji w ASP.NET Core jest oparta na parach klucz-wartość określonych przez *dostawców konfiguracji*. Dostawcy konfiguracji odczytują dane konfiguracji do par klucz-wartość z różnych źródeł konfiguracji:

* W usłudze Azure Key Vault
* Azure App Configuration
* Argumenty wiersza polecenia
* Dostawcy niestandardowi (instalowani lub utworzony)
* Pliki katalogu
* Zmienne środowiskowe
* Obiekty w pamięci .NET
* Pliki ustawień

Pakiety konfiguracyjne dla typowych scenariuszy dostawcy konfiguracji ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) są dołączone niejawnie przez platformę.

Przykłady kodu, które obserwują i w przykładowej aplikacji używają przestrzeni nazw <xref:Microsoft.Extensions.Configuration>:

```csharp
using Microsoft.Extensions.Configuration;
```

*Wzorzec opcji* jest rozszerzeniem pojęć konfiguracyjnych opisanych w tym temacie. Opcje używają klas do reprezentowania grup powiązanych ustawień. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="host-versus-app-configuration"></a>Host a konfiguracja aplikacji

Przed skonfigurowaniem i uruchomieniem aplikacji *host* zostanie skonfigurowany i uruchomiony. Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji. Zarówno aplikacja, jak i Host są konfigurowane przy użyciu dostawców konfiguracji opisanych w tym temacie. Klucz konfiguracji hosta — pary wartości są również uwzględnione w konfiguracji aplikacji. Aby uzyskać więcej informacji na temat tego, jak dostawcy konfiguracji są używani podczas kompilowania hosta i jak źródła konfiguracji wpływają na konfigurację hosta, zobacz <xref:fundamentals/index#host>.

## <a name="other-configuration"></a>Inna konfiguracja

Ten temat dotyczy tylko *konfiguracji aplikacji*. Inne aspekty uruchamiania i hostowania aplikacji ASP.NET Core są konfigurowane przy użyciu plików konfiguracji nieuwzględnionych w tym temacie:

* plik *Launch. json*/*profilu launchsettings. JSON* to pliki konfiguracji narzędzi dla środowiska programistycznego, opisane w temacie:
  * W <xref:fundamentals/environments#development>.
  * W zestawie dokumentacji, w której pliki są używane do konfigurowania ASP.NET Core aplikacji na potrzeby scenariuszy programistycznych.
* *Web. config* to plik konfiguracji serwera opisany w następujących tematach:
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

Aby uzyskać więcej informacji na temat migrowania konfiguracji aplikacji z wcześniejszych wersji programu ASP.NET, zobacz <xref:migration/proper-to-2x/index#store-configurations>.

## <a name="default-configuration"></a>Konfiguracja domyślna

Aplikacje sieci Web oparte na ASP.NET Core [nowym szablonów dotnet](/dotnet/core/tools/dotnet-new) <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> podczas kompilowania hosta. `CreateDefaultBuilder` zapewnia domyślną konfigurację dla aplikacji w następującej kolejności:

Poniższe zasady dotyczą aplikacji korzystających z [hosta ogólnego](xref:fundamentals/host/generic-host). Aby uzyskać szczegółowe informacje na temat konfiguracji domyślnej podczas korzystania z [hosta sieci Web](xref:fundamentals/host/web-host), zobacz [wersję ASP.NET Core 2,2 tego tematu](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).

* Konfiguracja hosta jest poświadczona z:
  * Zmienne środowiskowe poprzedzone prefiksem `DOTNET_` (na przykład `DOTNET_ENVIRONMENT`) przy użyciu [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider). Prefiks (`DOTNET_`) jest usuwany podczas ładowania par klucz-wartość konfiguracji.
  * Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).
* Konfiguracja domyślna hosta sieci Web (`ConfigureWebHostDefaults`):
  * Kestrel jest używany jako serwer sieci Web i konfigurowany przy użyciu dostawców konfiguracji aplikacji.
  * Dodaj oprogramowanie pośredniczące do filtrowania hosta.
  * Dodaj przekierowane nagłówki — oprogramowanie pośredniczące, jeśli zmienna środowiskowa `ASPNETCORE_FORWARDEDHEADERS_ENABLED` jest ustawiona na `true`.
  * Włącz integrację usług IIS.
* Podano konfigurację aplikacji z:
  * *appSettings. JSON* przy użyciu [dostawcy konfiguracji plików](#file-configuration-provider).
  * *appSettings. {Environment}. JSON* przy użyciu [dostawcy konfiguracji pliku](#file-configuration-provider).
  * [Secret Manager](xref:security/app-secrets) , gdy aplikacja jest uruchamiana w środowisku `Development` przy użyciu zestawu wpisów.
  * Zmienne środowiskowe używające [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).
  * Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).

## <a name="security"></a>Bezpieczeństwo

Aby zabezpieczyć poufne dane konfiguracji, należy zastosować następujące rozwiązania:

* Nie należy przechowywać haseł ani innych danych poufnych w kodzie dostawcy konfiguracji ani w plikach konfiguracji zwykłego tekstu.
* Nie używaj tajemnic produkcyjnych w środowiskach deweloperskich i testowych.
* Określ wpisy tajne poza projektem, aby nie mogły zostać przypadkowo przekazane do repozytorium kodu źródłowego.

Aby uzyskać więcej informacji, zobacz następujące tematy:

* <xref:fundamentals/environments>
* <xref:security/app-secrets> &ndash; zawiera porady dotyczące używania zmiennych środowiskowych do przechowywania poufnych danych. Menedżer wpisów tajnych używa dostawcy konfiguracji plików do przechowywania wpisów tajnych użytkownika w pliku JSON w systemie lokalnym. Dostawca konfiguracji plików został opisany w dalszej części tego tematu.

[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) bezpieczne przechowywanie wpisów tajnych aplikacji dla ASP.NET Core aplikacji. Aby uzyskać więcej informacji, zobacz <xref:security/key-vault-configuration>.

## <a name="hierarchical-configuration-data"></a>Hierarchiczne dane konfiguracji

Interfejs API konfiguracji jest w stanie utrzymywać hierarchiczne dane konfiguracji przez spłaszczonie danych hierarchicznych przy użyciu ogranicznika w kluczach konfiguracji.

W poniższym pliku JSON cztery klucze istnieją w hierarchii strukturalnej dwóch sekcji:

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

Gdy plik jest odczytywany do konfiguracji, są tworzone unikatowe klucze, aby zachować oryginalną hierarchiczną strukturę danych źródła konfiguracji. Sekcje i klucze są spłaszczone przy użyciu dwukropka (`:`), aby zachować oryginalną strukturę:

* section0:key0
* section0: Klucz1
* section1:key0
* Section1: Klucz1

Metody <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> i <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> są dostępne do izolowania sekcji i elementów podrzędnych sekcji w danych konfiguracyjnych. Te metody są opisane w dalszej [części GetSection, GetChildren i EXISTS](#getsection-getchildren-and-exists).

## <a name="conventions"></a>Konwencja

### <a name="sources-and-providers"></a>Źródła i dostawcy

Podczas uruchamiania aplikacji źródła konfiguracji są odczytywane w kolejności, w jakiej są określone przez ich dostawców konfiguracji.

Dostawcy konfiguracji implementujący funkcję wykrywania zmian mają możliwość ponownego załadowania konfiguracji, gdy ustawienie podstawowe zostanie zmienione. Na przykład dostawca konfiguracji plików (opisany w dalszej części tego tematu) i [dostawca konfiguracji Azure Key Vault](xref:security/key-vault-configuration) implementują wykrywanie zmian.

<xref:Microsoft.Extensions.Configuration.IConfiguration> jest dostępny w kontenerze [iniekcji zależności](xref:fundamentals/dependency-injection) aplikacji. <xref:Microsoft.Extensions.Configuration.IConfiguration> można wstrzyknąć do Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> lub MVC <xref:Microsoft.AspNetCore.Mvc.Controller>, aby uzyskać konfigurację dla klasy.

W poniższych przykładach pole `_config` służy do uzyskiwania dostępu do wartości konfiguracyjnych:

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }
}
```

```csharp
public class HomeController : Controller
{
    private readonly IConfiguration _config;

    public HomeController(IConfiguration config)
    {
        _config = config;
    }
}
```

Dostawcy konfiguracji nie mogą używać DI, ponieważ są niedostępne, gdy są skonfigurowane przez hosta.

### <a name="keys"></a>Klucze

Klucze konfiguracji przyjmują następujące konwencje:

* W kluczach nie jest rozróżniana wielkość liter. Na przykład `ConnectionString` i `connectionstring` są traktowane jako klucze równoważne.
* Jeśli wartość tego samego klucza jest ustawiana przez tych samych lub różnych dostawców konfiguracji, Ostatnia wartość ustawiona w tym kluczu jest używana.
* Klucze hierarchiczne
  * W interfejsie API konfiguracji, separator dwukropek (`:`) działa na wszystkich platformach.
  * W zmiennych środowiskowych separator dwukropek może nie zadziałał na wszystkich platformach. Podwójne podkreślenie (`__`) jest obsługiwane przez wszystkie platformy i automatycznie konwertowane na dwukropek.
  * W Azure Key Vault klucze hierarchiczne używają `--` (dwóch kresek) jako separatora. Napisz kod, aby zastąpić łączniki dwukropkiem, gdy wpisy tajne są ładowane do konfiguracji aplikacji.
* <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> obsługuje powiązania tablic z obiektami przy użyciu indeksów tablicowych w kluczach konfiguracji. Powiązanie tablicowe zostało opisane w sekcji [Powiązywanie tablicy z klasą](#bind-an-array-to-a-class) .

### <a name="values"></a>Wartości

Wartości konfiguracyjne przyjmują następujące konwencje:

* Wartości są ciągami.
* Wartości null nie można przechowywać w konfiguracji ani powiązana z obiektami.

## <a name="providers"></a>Dostawcy

W poniższej tabeli przedstawiono dostawców konfiguracji dostępnych do ASP.NET Core aplikacji.

| Dostawca | Zapewnia konfigurację z&hellip; |
| -------- | ----------------------------------- |
| [Dostawca konfiguracji Azure Key Vault](xref:security/key-vault-configuration) (tematy dotyczące*zabezpieczeń* ) | W usłudze Azure Key Vault |
| [Dostawca konfiguracji aplikacji platformy Azure](/azure/azure-app-configuration/quickstart-aspnet-core-app) (dokumentacja platformy Azure) | Azure App Configuration |
| [Dostawca konfiguracji wiersza polecenia](#command-line-configuration-provider) | Parametry wiersza polecenia |
| [Niestandardowy dostawca konfiguracji](#custom-configuration-provider) | Źródło niestandardowe |
| [Dostawca konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider) | Zmienne środowiskowe |
| [Dostawca konfiguracji plików](#file-configuration-provider) | Pliki (INI, JSON, XML) |
| [Dostawca konfiguracji klucza dla plików](#key-per-file-configuration-provider) | Pliki katalogu |
| [Dostawca konfiguracji pamięci](#memory-configuration-provider) | Kolekcje w pamięci |
| Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) (tematy dotyczące*zabezpieczeń* ) | Plik w katalogu profilu użytkownika |

Źródła konfiguracji są odczytywane w kolejności, w jakiej dostawcy konfiguracji są określeni podczas uruchamiania. Dostawcy konfiguracji opisane w tym temacie są opisane w kolejności alfabetycznej, a nie w kolejności, w jakiej kod ich rozmieszcza. Zamów dostawców konfiguracji w kodzie, aby odpowiadały priorytetom źródłowych źródeł konfiguracji wymaganych przez aplikację.

Typową sekwencją dostawców konfiguracji jest:

1. Pliki (*appSettings. JSON*, *appSettings. { Environment}. JSON*, gdzie `{Environment}` jest bieżącym środowiskiem hostingu aplikacji)
1. [Usługa Azure Key Vault](xref:security/key-vault-configuration)
1. Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) (tylko środowisko programistyczne)
1. Zmienne środowiskowe
1. Argumenty wiersza polecenia

Typowym celem jest umieszczenie dostawcy konfiguracji wiersza polecenia jako ostatni w serii dostawców, aby zezwolić na argumenty wiersza polecenia, aby przesłonić konfigurację ustawioną przez innych dostawców.

Poprzednia sekwencja dostawców jest używana, gdy nowy Konstruktor hosta zostanie zainicjowany przy użyciu `CreateDefaultBuilder`. Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a>Konfigurowanie konstruktora hostów za pomocą ConfigureHostConfiguration

Aby skonfigurować konstruktora hostów, wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> i podaj konfigurację. `ConfigureHostConfiguration` służy do inicjowania <xref:Microsoft.Extensions.Hosting.IHostEnvironment> do późniejszego użycia w procesie kompilacji. `ConfigureHostConfiguration` może być wywoływana wiele razy z wynikami.

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureHostConfiguration(config =>
        {
            var dict = new Dictionary<string, string>
            {
                {"MemoryCollectionKey1", "value1"},
                {"MemoryCollectionKey2", "value2"}
            };

            config.AddInMemoryCollection(dict);
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić dostawców konfiguracji aplikacji oprócz tych dodanych automatycznie przez `CreateDefaultBuilder`:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a>Zastąp poprzednią konfigurację argumentami wiersza polecenia

Aby podać konfigurację aplikacji, którą można zastąpić za pomocą argumentów wiersza polecenia, wywołaj `AddCommandLine` Last:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a>Usuń dostawców dodanych przez CreateDefaultBuilder

Aby usunąć dostawców dodanych przez `CreateDefaultBuilder`, wywołaj najpierw polecenie [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) w [IConfigurationBuilder. sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) :

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a>Użyj konfiguracji podczas uruchamiania aplikacji

Konfiguracja dostarczona do aplikacji w `ConfigureAppConfiguration` jest dostępna podczas uruchamiania aplikacji, w tym `Startup.ConfigureServices`. Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja dostępu podczas uruchamiania](#access-configuration-during-startup) .

## <a name="command-line-configuration-provider"></a>Dostawca konfiguracji wiersza polecenia

<xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> ładuje konfigurację z par klucz-wartość argumentu wiersza polecenia w czasie wykonywania.

Aby uaktywnić konfigurację wiersza polecenia, Metoda rozszerzenia <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> jest wywoływana w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

`AddCommandLine` jest wywoływana automatycznie, gdy zostanie wywołana `CreateDefaultBuilder(string [])`. Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .

`CreateDefaultBuilder` również ładowania:

* Opcjonalna konfiguracja z pliku *appSettings. JSON* i *appSettings. { Environment}. JSON* — pliki.
* Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.
* Zmienne środowiskowe.

`CreateDefaultBuilder` dodaje dostawcę konfiguracji wiersza polecenia Last. Argumenty wiersza polecenia przekazane w czasie wykonywania zastępują konfigurację ustawioną przez innych dostawców.

`CreateDefaultBuilder` działa, gdy host jest skonstruowany. W związku z tym konfiguracja wiersza polecenia aktywowana przez `CreateDefaultBuilder` może mieć wpływ na sposób konfigurowania hosta.

W przypadku aplikacji opartych na ASP.NET Core szablonach `AddCommandLine` została już wywołana przez `CreateDefaultBuilder`. Aby dodać kolejnych dostawców konfiguracji i zachować możliwość przesłonięcia konfiguracji od tych dostawców za pomocą argumentów wiersza polecenia, wywołaj dodatkowych dostawców aplikacji w `ConfigureAppConfiguration` i Wywołaj `AddCommandLine` Last.

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

**Przykład**

Przykładowa aplikacja korzysta z statycznej metody wygodnej `CreateDefaultBuilder` do kompilowania hosta, który obejmuje wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.

1. Otwórz wiersz polecenia w katalogu projektu.
1. Podaj argument wiersza polecenia do polecenia `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.
1. Po uruchomieniu aplikacji otwórz w przeglądarce aplikację w `http://localhost:5000`.
1. Zwróć uwagę, że dane wyjściowe zawierają parę klucz-wartość dla argumentu wiersza polecenia konfiguracji dostarczonego do `dotnet run`.

### <a name="arguments"></a>Argumenty

Wartość musi następować po znaku równości (`=`) lub klucz musi mieć prefiks (`--` lub `/`), gdy wartość znajduje się w miejscu. Wartość nie jest wymagana, jeśli jest używany znak równości (na przykład `CommandLineKey=`).

| Prefiks klucza               | Przykład                                                |
| ------------------------ | ------------------------------------------------------ |
| Brak prefiksu                | `CommandLineKey1=value1`                               |
| Dwie kreski (`--`)        | `--CommandLineKey2=value2`, `--CommandLineKey2 value2` |
| Ukośnik (`/`)      | `/CommandLineKey3=value3`, `/CommandLineKey3 value3`   |

W tym samym poleceniu nie należy mieszać par klucz-wartość argumentu wiersza polecenia, które używają znaku równości z parami klucz-wartość, które używają spacji.

Przykładowe polecenia:

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a>Mapowanie przełączników

Mapowania przełączników Zezwalaj na logikę zamiany nazwy klucza. Podczas ręcznego kompilowania konfiguracji przy użyciu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>należy udostępnić słownik przemieszczeń Switch do metody <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.

Gdy jest używany słownik mapowania przełączników, słownik jest sprawdzany dla klucza, który pasuje do klucza dostarczonego przez argument wiersza polecenia. Jeśli klucz wiersza polecenia zostanie znaleziony w słowniku, wartość słownika (wymiana klucza) zostanie przeniesiona z powrotem, aby ustawić parę klucz-wartość w konfiguracji aplikacji. Mapowanie przełącznika jest wymagane dla każdego klucza wiersza polecenia poprzedzonego jedną kreską (`-`).

Przełącz reguły klucza słownika mapowania:

* Przełączniki muszą zaczynać się kreską (`-`) lub podwójną kreską (`--`).
* Słownik mapowania przełącznika nie może zawierać zduplikowanych kluczy.

Utwórz słownik mapowań mapowania. W poniższym przykładzie są tworzone dwa mapowania przełączników:

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

Po skompilowaniu hosta Wywołaj `AddCommandLine` przy użyciu słownika mapowania przełączników:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

W przypadku aplikacji korzystających z mapowań przełączników wywołanie `CreateDefaultBuilder` nie powinno przekazywać argumentów. Wywołanie `AddCommandLine` metody `CreateDefaultBuilder` nie obejmuje zamapowanych przełączników i nie ma sposobu przekazywania słownika mapowania przełącznika do `CreateDefaultBuilder`. Rozwiązanie nie przekazuje argumentów do `CreateDefaultBuilder`, ale zamiast zezwalać `AddCommandLine` metody `ConfigurationBuilder` do przetwarzania zarówno argumentów, jak i słownika mapowania przełącznika.

Po utworzeniu słownika mapowań przełączników zawiera dane przedstawione w poniższej tabeli.

| Klucz       | Wartość             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

Jeśli klucze mapowane przez przełącznik są używane podczas uruchamiania aplikacji, konfiguracja otrzymuje wartość konfiguracji klucza dostarczonego przez słownik:

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

Po uruchomieniu poprzedniego polecenia Konfiguracja zawiera wartości pokazane w poniższej tabeli.

| Klucz               | Wartość    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a>Dostawca konfiguracji zmiennych środowiskowych

<xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> ładuje konfigurację ze par klucz-wartość zmiennej środowiskowej w czasie wykonywania.

Aby uaktywnić konfigurację zmiennych środowiskowych, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

[Azure App Service](https://azure.microsoft.com/services/app-service/) umożliwia ustawianie zmiennych środowiskowych w witrynie Azure Portal, które mogą przesłonić konfigurację aplikacji przy użyciu dostawcy konfiguracji zmiennych środowiskowych. Aby uzyskać więcej informacji, zobacz artykuł [Azure Apps: zastępowanie konfiguracji aplikacji przy użyciu witryny Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).

`AddEnvironmentVariables` jest używany do ładowania zmiennych środowiskowych, które są poprzedzone `DOTNET_` na potrzeby [konfiguracji hosta](#host-versus-app-configuration) , gdy nowy Konstruktor hosta zostanie zainicjowany z [hostem ogólnym](xref:fundamentals/host/generic-host) i zostanie wywołane `CreateDefaultBuilder`. Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .

`CreateDefaultBuilder` również ładowania:

* Konfiguracja aplikacji z nieoznaczonych zmiennych środowiskowych przez wywołanie `AddEnvironmentVariables` bez prefiksu.
* Opcjonalna konfiguracja z pliku *appSettings. JSON* i *appSettings. { Environment}. JSON* — pliki.
* Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.
* Argumenty wiersza polecenia.

Dostawca konfiguracji zmiennych środowiskowych jest wywoływany po ustanowieniu konfiguracji z poziomu kluczy tajnych użytkownika i plików *AppSettings* . Wywołanie dostawcy w tym miejscu pozwala odczytywać zmienne środowiskowe w czasie wykonywania w celu przesłania konfiguracji ustawionych przez klucze tajne użytkownika i pliki *AppSettings* .

Aby zapewnić konfigurację aplikacji na podstawie dodatkowych zmiennych środowiskowych, wywołaj dodatkowych dostawców aplikacji w `ConfigureAppConfiguration` i Wywołaj `AddEnvironmentVariables` z prefiksem:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

Wywołaj `AddEnvironmentVariables` ostatnią, aby zezwolić na zmienne środowiskowe z danym prefiksem w celu przesłonięcia wartości z innych dostawców.

**Przykład**

Przykładowa aplikacja korzysta z statycznej metody wygodnej `CreateDefaultBuilder` do kompilowania hosta, który obejmuje wywołanie `AddEnvironmentVariables`.

1. Uruchom przykładową aplikację. Otwórz w przeglądarce aplikację w `http://localhost:5000`.
1. Zwróć uwagę, że dane wyjściowe zawierają parę klucz-wartość dla zmiennej środowiskowej `ENVIRONMENT`. Wartość odzwierciedla środowisko, w którym jest uruchomiona aplikacja, zwykle `Development` podczas uruchamiania lokalnego.

Aby zachować listę zmiennych środowiskowych renderowanych przez aplikację, aplikacja filtruje zmienne środowiskowe. Zapoznaj się z plikiem przykładowej *strony aplikacji/index. cshtml. cs* .

Aby udostępnić wszystkie zmienne środowiskowe dostępne dla aplikacji, należy zmienić `FilteredConfiguration` na *stronie/index. cshtml. cs* w następujący sposób:

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a>Prefiksy

Zmienne środowiskowe ładowane do konfiguracji aplikacji są filtrowane podczas dostarczania prefiksu do metody `AddEnvironmentVariables`. Na przykład aby filtrować zmienne środowiskowe na prefiksie `CUSTOM_`, podaj prefiks dla dostawcy konfiguracji:

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

Prefiks jest usuwany podczas tworzenia par klucz-wartość konfiguracji.

Podczas tworzenia konstruktora hostów Konfiguracja hosta jest zapewniana przez zmienne środowiskowe. Aby uzyskać więcej informacji na temat prefiksu używanego dla tych zmiennych środowiskowych, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .

**Prefiksy parametrów połączenia**

Interfejs API konfiguracji ma specjalne reguły przetwarzania dla czterech zmiennych środowiskowych parametrów połączenia związanych z konfigurowaniem parametrów połączenia platformy Azure dla środowiska aplikacji. Zmienne środowiskowe z prefiksami podanymi w tabeli są ładowane do aplikacji, jeśli nie podano prefiksu do `AddEnvironmentVariables`.

| Prefiks parametrów połączenia | Dostawca |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | Dostawca niestandardowy |
| `MYSQLCONNSTR_` | [MySQL](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [SQL Server](https://www.microsoft.com/sql-server/) |

Gdy zmienna środowiskowa zostanie odnaleziona i załadowana do konfiguracji z dowolnymi z czterech prefiksów pokazanych w tabeli:

* Klucz konfiguracji jest tworzony przez usunięcie prefiksu zmiennej środowiskowej i dodanie sekcji klucza konfiguracji (`ConnectionStrings`).
* Zostanie utworzona nowa para klucz-wartość konfiguracji, która reprezentuje dostawcę połączenia bazy danych (z wyjątkiem `CUSTOMCONNSTR_`, które nie ma określonego dostawcy).

| Klucz zmiennej środowiskowej | Przekonwertowany klucz konfiguracji | Wpis konfiguracji dostawcy                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | Wpis konfiguracji nie został utworzony.                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | Klucz: `ConnectionStrings:{KEY}_ProviderName`:<br>Wartość:`MySql.Data.MySqlClient` |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | Klucz: `ConnectionStrings:{KEY}_ProviderName`:<br>Wartość:`System.Data.SqlClient`  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | Klucz: `ConnectionStrings:{KEY}_ProviderName`:<br>Wartość:`System.Data.SqlClient`  |

**Przykład**

Na serwerze zostanie utworzona niestandardowa zmienna środowiskowa parametrów połączenia:

* Nazwa &ndash; `CUSTOMCONNSTR_ReleaseDB`
* `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True` &ndash; wartości

Jeśli `IConfiguration` zostanie dodany i przypisany do pola o nazwie `_config`, przeczytaj wartość:

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a>Dostawca konfiguracji plików

<xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> jest klasą bazową do ładowania konfiguracji z systemu plików. Następujący dostawcy konfiguracji są przydzielone do określonych typów plików:

* [Dostawca konfiguracji pliku INI](#ini-configuration-provider)
* [Dostawca konfiguracji JSON](#json-configuration-provider)
* [Dostawca konfiguracji XML](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a>Dostawca konfiguracji pliku INI

<xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku INI w czasie wykonywania.

Aby uaktywnić konfigurację pliku INI, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Dwukropek może służyć jako ogranicznik sekcji w konfiguracji pliku INI.

Przeciążania Zezwalaj na określanie:

* Czy plik jest opcjonalny.
* Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> używany do uzyskiwania dostępu do pliku.

Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

Ogólny przykład pliku konfiguracji INI:

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

Poprzedni plik konfiguracji ładuje następujące klucze z `value`:

* section0:key0
* section0: Klucz1
* Section1: podsekcja: Key
* section2: subsection0: klucz
* section2: subsection1: klucz

### <a name="json-configuration-provider"></a>Dostawca konfiguracji JSON

<xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku JSON podczas środowiska uruchomieniowego.

Aby uaktywnić konfigurację pliku JSON, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Przeciążania Zezwalaj na określanie:

* Czy plik jest opcjonalny.
* Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> używany do uzyskiwania dostępu do pliku.

`AddJsonFile` jest automatycznie wywoływana dwukrotnie, gdy nowy Konstruktor hosta zostanie zainicjowany z `CreateDefaultBuilder`. Metoda jest wywoływana w celu załadowania konfiguracji z:

* *appSettings. json* &ndash; ten plik jest najpierw odczytywany. Wersja środowiska pliku może przesłonić wartości dostarczone przez plik *appSettings. JSON* .
* *appSettings. {Environment}. JSON* &ndash; wersja środowiska pliku jest ładowana na podstawie [IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).

Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .

`CreateDefaultBuilder` również ładowania:

* Zmienne środowiskowe.
* Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.
* Argumenty wiersza polecenia.

Dostawca konfiguracji JSON został ustanowiony jako pierwszy. W związku z tym klucze tajne użytkownika, zmienne środowiskowe i argumenty wiersza polecenia przesłaniają konfigurację ustawioną przez pliki *AppSettings* .

Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji dla plików innych niż *appSettings. JSON* i *appSettings. { Environment}. JSON*:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

**Przykład**

Przykładowa aplikacja korzysta z statycznej metody wygodnej `CreateDefaultBuilder` do kompilowania hosta, który obejmuje dwa wywołania `AddJsonFile`:

* Pierwsze wywołanie `AddJsonFile` ładuje konfigurację z pliku *appSettings. JSON*:

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.json)]

* Drugie wywołanie `AddJsonFile` ładuje konfigurację z *appSettings. { Environment}. JSON*. Dla *appSettings. Plik Development. JSON* w przykładowej aplikacji jest ładowany:

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.Development.json)]

1. Uruchom przykładową aplikację. Otwórz w przeglądarce aplikację w `http://localhost:5000`.
1. Dane wyjściowe zawierają pary klucz-wartość dla konfiguracji w oparciu o środowisko aplikacji. Poziom dziennika dla klucza `Logging:LogLevel:Default` jest `Debug` podczas uruchamiania aplikacji w środowisku deweloperskim.
1. Ponownie uruchom przykładową aplikację w środowisku produkcyjnym:
   1. Otwórz plik *Properties/profilu launchsettings. JSON* .
   1. W profilu `ConfigurationSample` Zmień wartość zmiennej środowiskowej `ASPNETCORE_ENVIRONMENT` na `Production`.
   1. Zapisz plik i uruchom aplikację za pomocą `dotnet run` w powłoce poleceń.
1. Ustawienia w *appSettings. Plik Development. JSON* nie zastępuje już ustawień w pliku *appSettings. JSON*. Poziom dziennika `Logging:LogLevel:Default` jest `Information`.

### <a name="xml-configuration-provider"></a>Dostawca konfiguracji XML

<xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku XML w czasie wykonywania.

Aby uaktywnić konfigurację pliku XML, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Przeciążania Zezwalaj na określanie:

* Czy plik jest opcjonalny.
* Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> używany do uzyskiwania dostępu do pliku.

Węzeł główny pliku konfiguracji jest ignorowany, gdy są tworzone pary klucz-wartość konfiguracji. Nie określaj definicji typu dokumentu (DTD) ani przestrzeni nazw w pliku.

Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

Pliki konfiguracji XML mogą używać odrębnych nazw elementów dla powtarzających się sekcji:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

Poprzedni plik konfiguracji ładuje następujące klucze z `value`:

* section0:key0
* section0: Klucz1
* section1:key0
* Section1: Klucz1

Powtarzalne elementy, które używają tej samej nazwy elementu, działają, jeśli atrybut `name` jest używany do odróżnienia elementów:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

Poprzedni plik konfiguracji ładuje następujące klucze z `value`:

* sekcja: section0: Key: Key0
* sekcja: section0: Key: Klucz1
* sekcja: Section1: Key: Key0
* sekcja: Section1: Key: Klucz1

Atrybuty mogą służyć do dostarczania wartości:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

Poprzedni plik konfiguracji ładuje następujące klucze z `value`:

* Key: Attribute
* sekcja: Key: Attribute

## <a name="key-per-file-configuration-provider"></a>Dostawca konfiguracji klucza dla plików

<xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> używa plików katalogu jako par klucz konfiguracji i wartość. Kluczem jest nazwa pliku. Wartość zawiera zawartość pliku. Dostawca konfiguracji klucza dla plików jest używany w scenariuszach hostingu platformy Docker.

Aby uaktywnić konfigurację klucza dla plików, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>. `directoryPath` plików musi być ścieżką bezwzględną.

Przeciążania Zezwalaj na określanie:

* Delegat `Action<KeyPerFileConfigurationSource>`, który konfiguruje źródło.
* Określa, czy katalog jest opcjonalny, i ścieżkę do katalogu.

Podwójny znak podkreślenia (`__`) jest używany jako ogranicznik klucza konfiguracji w nazwach plików. Na przykład nazwa pliku `Logging__LogLevel__System` generuje klucz konfiguracji `Logging:LogLevel:System`.

Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a>Dostawca konfiguracji pamięci

<xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> używa kolekcji w pamięci jako par klucz-wartość konfiguracji.

Aby uaktywnić konfigurację kolekcji w pamięci, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Dostawcę konfiguracji można zainicjować przy użyciu `IEnumerable<KeyValuePair<String,String>>`.

Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji.

W poniższym przykładzie tworzony jest słownik konfiguracji:

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

Słownik jest używany z wywołaniem `AddInMemoryCollection`, aby zapewnić konfigurację:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a>GetValue

[ConfigurationBinder. GetValue\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) wyodrębnia jedną wartość z konfiguracji z określonym kluczem i konwertuje ją na określony typ niekolekcje. Przeciążenie akceptuje wartość domyślną.

Poniższy przykład:

* Wyodrębnia wartość ciągu z konfiguracji z kluczem `NumberKey`. Jeśli nie można odnaleźć `NumberKey` w kluczach konfiguracji, zostanie użyta wartość domyślna `99`.
* Typ wartości jako `int`.
* Przechowuje wartość we właściwości `NumberConfig` do użycia na stronie.

```csharp
public class IndexModel : PageModel
{
    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    public int NumberConfig { get; private set; }

    public void OnGet()
    {
        NumberConfig = _config.GetValue<int>("NumberKey", 99);
    }
}
```

## <a name="getsection-getchildren-and-exists"></a>GetSection, GetChildren i EXISTS

W poniższych przykładach należy wziąć pod uwagę następujący plik JSON. Cztery klucze są dostępne w dwóch sekcjach, z których jedna zawiera parę podsekcji:

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

Gdy plik jest odczytywany do konfiguracji, następujące unikatowe klucze hierarchiczne są tworzone w celu przechowywania wartości konfiguracyjnych:

* section0:key0
* section0: Klucz1
* section1:key0
* Section1: Klucz1
* section2:subsection0:key0
* section2: subsection0: Klucz1
* section2:subsection1:key0
* section2: subsection1: Klucz1

### <a name="getsection"></a>GetSection

[IConfiguration. GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) wyodrębnia podsekcję konfiguracji z określonym kluczem podsekcji.

Aby zwrócić <xref:Microsoft.Extensions.Configuration.IConfigurationSection> zawierający tylko pary klucz-wartość w `section1`, wywołaj `GetSection` i podaj nazwę sekcji:

```csharp
var configSection = _config.GetSection("section1");
```

`configSection` nie ma wartości, tylko klucza i ścieżki.

Podobnie, aby uzyskać wartości kluczy w `section2:subsection0`, wywołaj `GetSection` i podaj ścieżkę sekcji:

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

`GetSection` nigdy nie zwraca `null`. Jeśli nie znaleziono pasującej sekcji, zwracany jest pusty `IConfigurationSection`.

Gdy `GetSection` zwraca pasującą sekcję, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> nie jest wypełnione. <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> i <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> są zwracane, gdy istnieje sekcja.

### <a name="getchildren"></a>GetChildren

Wywołanie [iConfiguration. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) w `section2` uzyskuje `IEnumerable<IConfigurationSection>` obejmujący:

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a>Exists

Użyj [ConfigurationExtensions. istnieje](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) , aby określić, czy istnieje sekcja konfiguracji:

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

Dane przykładowe `sectionExists` jest `false`, ponieważ w danych konfiguracyjnych nie ma `section2:subsection2` sekcji.

## <a name="bind-to-a-class"></a>Powiąż z klasą

Konfigurację można powiązać z klasami, które reprezentują grupy powiązanych ustawień przy użyciu *wzorca opcji*. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.

Wartości konfiguracji są zwracane jako ciągi, ale wywołanie <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> umożliwia konstruowanie obiektów [poco](https://wikipedia.org/wiki/Plain_Old_CLR_Object) . Obiekt wiążący wiąże wartości ze wszystkimi publicznymi właściwościami odczytu/zapisu dostarczonego typu. Pola **nie** są powiązane.

Przykładowa aplikacja zawiera model `Starship` (*modele/Starship. cs*):

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

Sekcja `starship` pliku *Starship. JSON* tworzy konfigurację, gdy aplikacja Przykładowa używa dostawcy konfiguracji JSON do załadowania konfiguracji:

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

Tworzone są następujące pary klucz-wartość konfiguracji:

| Klucz                   | Wartość                                             |
| --------------------- | ------------------------------------------------- |
| Starship: Nazwa         | USS Enterprise                                    |
| Starship: Rejestr     | NCC-1701                                          |
| Starship: Klasa        | Skład                                      |
| Starship: Długość       | 304,8                                             |
| Starship: prowizja | False                                             |
| handlowych             | Najważniejsze obrazy Corp. https://www.paramount.com |

Przykładowa aplikacja wywołuje `GetSection` z kluczem `starship`. Pary klucz-wartość `starship` są odizolowane. Metoda `Bind` jest wywoływana w podsekcji przekazującej w wystąpieniu klasy `Starship`. Po powiązaniu wartości wystąpień wystąpienie jest przypisywane do właściwości w celu renderowania:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

## <a name="bind-to-an-object-graph"></a>Powiąż z grafem obiektów

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> jest w stanie powiązać cały Graf obiektów POCO. Podobnie jak w przypadku powiązania prostego obiektu, powiązane są tylko publiczne właściwości odczytu i zapisu.

Przykład zawiera model `TvShow`, którego wykres obiektu zawiera klasy `Metadata` i `Actors` (*modele/TvShow. cs*):

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

Przykładowa aplikacja zawiera plik *tvshow. XML* zawierający dane konfiguracji:

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

Konfiguracja jest powiązana z całym grafem obiektu `TvShow` za pomocą metody `Bind`. Powiązane wystąpienie jest przypisane do właściwości w celu renderowania:

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

[ConfigurationBinder. Get\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) tworzy powiązania i zwraca określony typ. `Get<T>` jest wygodniejszy niż korzystanie z `Bind`. Poniższy kod pokazuje, jak używać `Get<T>` w poprzednim przykładzie, co umożliwia bezpośrednie przypisanie wystąpienia powiązanego do właściwości używanej do renderowania:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a>Powiąż tablicę z klasą

*Przykładowa aplikacja pokazuje Koncepcje opisane w tej sekcji.*

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> obsługuje powiązania tablic z obiektami przy użyciu indeksów tablicowych w kluczach konfiguracji. Każdy format tablicy, który ujawnia segment klucza numerycznego (`:0:`, `:1:`, &hellip; `:{n}:`), jest w stanie powiązać powiązanie tablicową z tablicą klas POCO.

> [!NOTE]
> Powiązanie jest dostarczane według Konwencji. Niestandardowi dostawcy konfiguracji nie muszą implementować powiązania tablicy.

**Przetwarzanie tablicy w pamięci**

Należy wziąć pod uwagę klucze konfiguracji i wartości podane w poniższej tabeli.

| Klucz             | Wartość  |
| :-------------: | :----: |
| Tablica: wpisy: 0 | value0 |
| Tablica: wpisy: 1 | Sekwencj |
| Tablica: wpisy: 2 | Wartość2 |
| Tablica: wpisy: 4 | value4 |
| Tablica: wpisy: 5 | value5 |

Te klucze i wartości są ładowane w przykładowej aplikacji przy użyciu dostawcy konfiguracji pamięci:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

Tablica pomija wartość indeksu &num;3. Segregator konfiguracji nie może powiązać wartości null ani tworzyć wpisów o wartości null w obiektach powiązanych, co oznacza, że w chwili pojawi się wynik powiązania tej tablicy z obiektem.

W przykładowej aplikacji jest dostępna Klasa POCO, która przechowuje powiązane dane konfiguracji:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

Dane konfiguracji są powiązane z obiektem:

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

[ConfigurationBinder. Get\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) można również użyć składni, co spowoduje zwiększenie kodu kompaktowego:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

Obiekt powiązany, wystąpienie `ArrayExample`, otrzymuje dane tablicy z konfiguracji.

| Indeks `ArrayExample.Entries` | Wartość `ArrayExample.Entries` |
| :--------------------------: | :--------------------------: |
| 0                            | value0                       |
| 1                            | Sekwencj                       |
| 2                            | Wartość2                       |
| 3                            | value4                       |
| 4                            | value5                       |

Indeks &num;3 w obiekcie powiązanym zawiera dane konfiguracyjne `array:4` klucza konfiguracji i jego wartość `value4`. Gdy dane konfiguracji zawierające tablicę są powiązane, indeksy tablic w kluczach konfiguracji są używane tylko do iteracji danych konfiguracji podczas tworzenia obiektu. Wartości null nie można zachować w danych konfiguracyjnych, a wpis o wartości null nie jest tworzony w obiekcie powiązanym, gdy tablica w kluczach konfiguracji pomija jeden lub więcej indeksów.

Brakujący element konfiguracji dla indeksu &num;3 można dostarczyć przed powiązaniem do wystąpienia `ArrayExample` przez dowolnego dostawcę konfiguracji, który wygeneruje poprawną parę klucz-wartość w konfiguracji. Jeśli przykład zawiera dodatkowego dostawcę konfiguracji JSON z brakującą parą klucz-wartość, `ArrayExample.Entries` pasuje do kompletnej tablicy konfiguracji:

plik *missing_value. JSON*:

```json
{
  "array:entries:3": "value3"
}
```

W pliku `ConfigureAppConfiguration`:

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

Para klucz-wartość pokazana w tabeli jest ładowana do konfiguracji.

| Klucz             | Wartość  |
| :-------------: | :----: |
| Tablica: wpisy: 3 | Wartość3 |

Jeśli wystąpienie klasy `ArrayExample` jest powiązane, gdy dostawca konfiguracji JSON zawiera wpis dla indeksu &num;3, tablica `ArrayExample.Entries` zawiera wartość.

| Indeks `ArrayExample.Entries` | Wartość `ArrayExample.Entries` |
| :--------------------------: | :--------------------------: |
| 0                            | value0                       |
| 1                            | Sekwencj                       |
| 2                            | Wartość2                       |
| 3                            | Wartość3                       |
| 4                            | value4                       |
| 5                            | value5                       |

**Przetwarzanie tablicy JSON**

Jeśli plik JSON zawiera tablicę, klucze konfiguracji są tworzone dla elementów tablicy z indeksem sekcji o wartości zero. W poniższym pliku konfiguracyjnym `subsection` jest tablicą:

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

Dostawca konfiguracji JSON odczytuje dane konfiguracji do następujących par klucz-wartość:

| Klucz                     | Wartość  |
| ----------------------- | :----: |
| json_array: klucz          | wartośća |
| json_array:subsection:0 | Wartośćb |
| json_array:subsection:1 | valueC |
| json_array:subsection:2 | Znajdując |

W przykładowej aplikacji jest dostępna następująca Klasa POCO z powiązaniem par klucz-wartość konfiguracji:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

Po powiązaniu `JsonArrayExample.Key` utrzymuje `valueA`wartości. Wartości podsekcji są przechowywane we właściwości tablicy POCO `Subsection`.

| Indeks `JsonArrayExample.Subsection` | Wartość `JsonArrayExample.Subsection` |
| :---------------------------------: | :---------------------------------: |
| 0                                   | Wartośćb                              |
| 1                                   | valueC                              |
| 2                                   | Znajdując                              |

## <a name="custom-configuration-provider"></a>Niestandardowy dostawca konfiguracji

Przykładowa aplikacja pokazuje, jak utworzyć podstawowego dostawcę konfiguracji, który odczytuje pary klucz-wartość konfiguracji z bazy danych przy użyciu [Entity Framework (EF)](/ef/core/).

Dostawca ma następującą charakterystykę:

* Baza danych EF w pamięci jest używana w celach demonstracyjnych. Aby użyć bazy danych, która wymaga parametrów połączenia, zaimplementuj pomocniczą `ConfigurationBuilder`, aby podać parametry połączenia od innego dostawcy konfiguracji.
* Dostawca odczytuje tabelę bazy danych w konfiguracji podczas uruchamiania. Dostawca nie wykonuje zapytania do bazy danych w oparciu o klucz.
* Ponowne załadowanie nie zostało zaimplementowane, więc aktualizacja bazy danych po uruchomieniu aplikacji nie ma wpływu na konfigurację aplikacji.

Zdefiniuj jednostkę `EFConfigurationValue` do przechowywania wartości konfiguracji w bazie danych.

*Modele/EFConfigurationValue. cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

Dodaj `EFConfigurationContext` do przechowywania skonfigurowanych wartości i uzyskiwania do nich dostępu.

*EFConfigurationProvider/EFConfigurationContext. cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

Utwórz klasę, która implementuje <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.

*EFConfigurationProvider/EFConfigurationSource. cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

Tworzenie niestandardowego dostawcy konfiguracji przez dziedziczenie z <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>. Dostawca konfiguracji inicjuje bazę danych, gdy jest pusta. Ponieważ w [kluczach konfiguracji jest rozróżniana wielkość liter](#keys), słownik używany do inicjowania bazy danych jest tworzony przy użyciu funkcji porównującej bez uwzględniania wielkości liter ([StringComparer. OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).

*EFConfigurationProvider/EFConfigurationProvider. cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

Metoda rozszerzenia `AddEFConfiguration` zezwala na Dodawanie źródła konfiguracji do `ConfigurationBuilder`.

*Rozszerzenia/EntityFrameworkExtensions. cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

Poniższy kod pokazuje, jak używać niestandardowych `EFConfigurationProvider` w *program.cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a>Konfiguracja dostępu podczas uruchamiania

Wsuń `IConfiguration` do konstruktora `Startup`, aby uzyskać dostęp do wartości konfiguracyjnych w `Startup.ConfigureServices`. Aby uzyskać dostęp do konfiguracji w `Startup.Configure`, należy wstrzyknąć `IConfiguration` bezpośrednio do metody lub użyć wystąpienia z konstruktora:

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

Aby zapoznać się z przykładem uzyskiwania dostępu do konfiguracji przy użyciu metod uruchamiania, zobacz [Uruchamianie aplikacji: wygodne metody](xref:fundamentals/startup#convenience-methods).

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a>Konfiguracja dostępu na stronie Razor Pages lub widoku MVC

Aby uzyskać dostęp do ustawień konfiguracji na stronie Razor Pages lub widoku MVC, Dodaj [dyrektywę using](xref:mvc/views/razor#using) ([ C# odwołanie: Using](/dotnet/csharp/language-reference/keywords/using-directive)) dla [przestrzeni nazw Microsoft. Extensions. Configuration](xref:Microsoft.Extensions.Configuration) i wsuń <xref:Microsoft.Extensions.Configuration.IConfiguration> do strony lub widoku.

Na stronie Razor Pages:

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
    <p>Configuration value for 'key': @Configuration["key"]</p>
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
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a>Dodawanie konfiguracji z zestawu zewnętrznego

Implementacja <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> umożliwia dodawanie ulepszeń do aplikacji podczas uruchamiania z zestawu zewnętrznego poza klasą `Startup` aplikacji. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/configuration/options>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Konfiguracja aplikacji w ASP.NET Core jest oparta na parach klucz-wartość określonych przez *dostawców konfiguracji*. Dostawcy konfiguracji odczytują dane konfiguracji do par klucz-wartość z różnych źródeł konfiguracji:

* W usłudze Azure Key Vault
* Azure App Configuration
* Argumenty wiersza polecenia
* Dostawcy niestandardowi (instalowani lub utworzony)
* Pliki katalogu
* Zmienne środowiskowe
* Obiekty w pamięci .NET
* Pliki ustawień

Pakiety konfiguracyjne dla typowych scenariuszy dostawcy konfiguracji ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) są zawarte w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).

Przykłady kodu, które obserwują i w przykładowej aplikacji używają przestrzeni nazw <xref:Microsoft.Extensions.Configuration>:

```csharp
using Microsoft.Extensions.Configuration;
```

*Wzorzec opcji* jest rozszerzeniem pojęć konfiguracyjnych opisanych w tym temacie. Opcje używają klas do reprezentowania grup powiązanych ustawień. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="host-versus-app-configuration"></a>Host a konfiguracja aplikacji

Przed skonfigurowaniem i uruchomieniem aplikacji *host* zostanie skonfigurowany i uruchomiony. Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji. Zarówno aplikacja, jak i Host są konfigurowane przy użyciu dostawców konfiguracji opisanych w tym temacie. Klucz konfiguracji hosta — pary wartości są również uwzględnione w konfiguracji aplikacji. Aby uzyskać więcej informacji na temat tego, jak dostawcy konfiguracji są używani podczas kompilowania hosta i jak źródła konfiguracji wpływają na konfigurację hosta, zobacz <xref:fundamentals/index#host>.

## <a name="other-configuration"></a>Inna konfiguracja

Ten temat dotyczy tylko *konfiguracji aplikacji*. Inne aspekty uruchamiania i hostowania aplikacji ASP.NET Core są konfigurowane przy użyciu plików konfiguracji nieuwzględnionych w tym temacie:

* plik *Launch. json*/*profilu launchsettings. JSON* to pliki konfiguracji narzędzi dla środowiska programistycznego, opisane w temacie:
  * W <xref:fundamentals/environments#development>.
  * W zestawie dokumentacji, w której pliki są używane do konfigurowania ASP.NET Core aplikacji na potrzeby scenariuszy programistycznych.
* *Web. config* to plik konfiguracji serwera opisany w następujących tematach:
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

Aby uzyskać więcej informacji na temat migrowania konfiguracji aplikacji z wcześniejszych wersji programu ASP.NET, zobacz <xref:migration/proper-to-2x/index#store-configurations>.

## <a name="default-configuration"></a>Konfiguracja domyślna

Aplikacje sieci Web oparte na ASP.NET Core [nowym szablonów dotnet](/dotnet/core/tools/dotnet-new) <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> podczas kompilowania hosta. `CreateDefaultBuilder` zapewnia domyślną konfigurację dla aplikacji w następującej kolejności:

Poniższe zasady dotyczą aplikacji korzystających z [hosta sieci Web](xref:fundamentals/host/web-host). Aby uzyskać szczegółowe informacje na temat konfiguracji domyślnej w przypadku korzystania z [hosta ogólnego](xref:fundamentals/host/generic-host), zobacz [najnowszą wersję tego tematu](xref:fundamentals/configuration/index).

* Konfiguracja hosta jest poświadczona z:
  * Zmienne środowiskowe poprzedzone prefiksem `ASPNETCORE_` (na przykład `ASPNETCORE_ENVIRONMENT`) przy użyciu [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider). Prefiks (`ASPNETCORE_`) jest usuwany podczas ładowania par klucz-wartość konfiguracji.
  * Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).
* Podano konfigurację aplikacji z:
  * *appSettings. JSON* przy użyciu [dostawcy konfiguracji plików](#file-configuration-provider).
  * *appSettings. {Environment}. JSON* przy użyciu [dostawcy konfiguracji pliku](#file-configuration-provider).
  * [Secret Manager](xref:security/app-secrets) , gdy aplikacja jest uruchamiana w środowisku `Development` przy użyciu zestawu wpisów.
  * Zmienne środowiskowe używające [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).
  * Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).

## <a name="security"></a>Bezpieczeństwo

Aby zabezpieczyć poufne dane konfiguracji, należy zastosować następujące rozwiązania:

* Nie należy przechowywać haseł ani innych danych poufnych w kodzie dostawcy konfiguracji ani w plikach konfiguracji zwykłego tekstu.
* Nie używaj tajemnic produkcyjnych w środowiskach deweloperskich i testowych.
* Określ wpisy tajne poza projektem, aby nie mogły zostać przypadkowo przekazane do repozytorium kodu źródłowego.

Aby uzyskać więcej informacji, zobacz następujące tematy:

* <xref:fundamentals/environments>
* <xref:security/app-secrets> &ndash; zawiera porady dotyczące używania zmiennych środowiskowych do przechowywania poufnych danych. Menedżer wpisów tajnych używa dostawcy konfiguracji plików do przechowywania wpisów tajnych użytkownika w pliku JSON w systemie lokalnym. Dostawca konfiguracji plików został opisany w dalszej części tego tematu.

[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) bezpieczne przechowywanie wpisów tajnych aplikacji dla ASP.NET Core aplikacji. Aby uzyskać więcej informacji, zobacz <xref:security/key-vault-configuration>.

## <a name="hierarchical-configuration-data"></a>Hierarchiczne dane konfiguracji

Interfejs API konfiguracji jest w stanie utrzymywać hierarchiczne dane konfiguracji przez spłaszczonie danych hierarchicznych przy użyciu ogranicznika w kluczach konfiguracji.

W poniższym pliku JSON cztery klucze istnieją w hierarchii strukturalnej dwóch sekcji:

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

Gdy plik jest odczytywany do konfiguracji, są tworzone unikatowe klucze, aby zachować oryginalną hierarchiczną strukturę danych źródła konfiguracji. Sekcje i klucze są spłaszczone przy użyciu dwukropka (`:`), aby zachować oryginalną strukturę:

* section0:key0
* section0: Klucz1
* section1:key0
* Section1: Klucz1

Metody <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> i <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> są dostępne do izolowania sekcji i elementów podrzędnych sekcji w danych konfiguracyjnych. Te metody są opisane w dalszej [części GetSection, GetChildren i EXISTS](#getsection-getchildren-and-exists).

## <a name="conventions"></a>Konwencja

### <a name="sources-and-providers"></a>Źródła i dostawcy

Podczas uruchamiania aplikacji źródła konfiguracji są odczytywane w kolejności, w jakiej są określone przez ich dostawców konfiguracji.

Dostawcy konfiguracji implementujący funkcję wykrywania zmian mają możliwość ponownego załadowania konfiguracji, gdy ustawienie podstawowe zostanie zmienione. Na przykład dostawca konfiguracji plików (opisany w dalszej części tego tematu) i [dostawca konfiguracji Azure Key Vault](xref:security/key-vault-configuration) implementują wykrywanie zmian.

<xref:Microsoft.Extensions.Configuration.IConfiguration> jest dostępny w kontenerze [iniekcji zależności](xref:fundamentals/dependency-injection) aplikacji. <xref:Microsoft.Extensions.Configuration.IConfiguration> można wstrzyknąć do Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> lub MVC <xref:Microsoft.AspNetCore.Mvc.Controller>, aby uzyskać konfigurację dla klasy.

W poniższych przykładach pole `_config` służy do uzyskiwania dostępu do wartości konfiguracyjnych:

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }
}
```

```csharp
public class HomeController : Controller
{
    private readonly IConfiguration _config;

    public HomeController(IConfiguration config)
    {
        _config = config;
    }
}
```

Dostawcy konfiguracji nie mogą używać DI, ponieważ są niedostępne, gdy są skonfigurowane przez hosta.

### <a name="keys"></a>Klucze

Klucze konfiguracji przyjmują następujące konwencje:

* W kluczach nie jest rozróżniana wielkość liter. Na przykład `ConnectionString` i `connectionstring` są traktowane jako klucze równoważne.
* Jeśli wartość tego samego klucza jest ustawiana przez tych samych lub różnych dostawców konfiguracji, Ostatnia wartość ustawiona w tym kluczu jest używana.
* Klucze hierarchiczne
  * W interfejsie API konfiguracji, separator dwukropek (`:`) działa na wszystkich platformach.
  * W zmiennych środowiskowych separator dwukropek może nie zadziałał na wszystkich platformach. Podwójne podkreślenie (`__`) jest obsługiwane przez wszystkie platformy i automatycznie konwertowane na dwukropek.
  * W Azure Key Vault klucze hierarchiczne używają `--` (dwóch kresek) jako separatora. Napisz kod, aby zastąpić łączniki dwukropkiem, gdy wpisy tajne są ładowane do konfiguracji aplikacji.
* <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> obsługuje powiązania tablic z obiektami przy użyciu indeksów tablicowych w kluczach konfiguracji. Powiązanie tablicowe zostało opisane w sekcji [Powiązywanie tablicy z klasą](#bind-an-array-to-a-class) .

### <a name="values"></a>Wartości

Wartości konfiguracyjne przyjmują następujące konwencje:

* Wartości są ciągami.
* Wartości null nie można przechowywać w konfiguracji ani powiązana z obiektami.

## <a name="providers"></a>Dostawcy

W poniższej tabeli przedstawiono dostawców konfiguracji dostępnych do ASP.NET Core aplikacji.

| Dostawca | Zapewnia konfigurację z&hellip; |
| -------- | ----------------------------------- |
| [Dostawca konfiguracji Azure Key Vault](xref:security/key-vault-configuration) (tematy dotyczące*zabezpieczeń* ) | W usłudze Azure Key Vault |
| [Dostawca konfiguracji aplikacji platformy Azure](/azure/azure-app-configuration/quickstart-aspnet-core-app) (dokumentacja platformy Azure) | Azure App Configuration |
| [Dostawca konfiguracji wiersza polecenia](#command-line-configuration-provider) | Parametry wiersza polecenia |
| [Niestandardowy dostawca konfiguracji](#custom-configuration-provider) | Źródło niestandardowe |
| [Dostawca konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider) | Zmienne środowiskowe |
| [Dostawca konfiguracji plików](#file-configuration-provider) | Pliki (INI, JSON, XML) |
| [Dostawca konfiguracji klucza dla plików](#key-per-file-configuration-provider) | Pliki katalogu |
| [Dostawca konfiguracji pamięci](#memory-configuration-provider) | Kolekcje w pamięci |
| Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) (tematy dotyczące*zabezpieczeń* ) | Plik w katalogu profilu użytkownika |

Źródła konfiguracji są odczytywane w kolejności, w jakiej dostawcy konfiguracji są określeni podczas uruchamiania. Dostawcy konfiguracji opisane w tym temacie są opisane w kolejności alfabetycznej, a nie w kolejności, w jakiej kod ich rozmieszcza. Zamów dostawców konfiguracji w kodzie, aby odpowiadały priorytetom źródłowych źródeł konfiguracji wymaganych przez aplikację.

Typową sekwencją dostawców konfiguracji jest:

1. Pliki (*appSettings. JSON*, *appSettings. { Environment}. JSON*, gdzie `{Environment}` jest bieżącym środowiskiem hostingu aplikacji)
1. [Usługa Azure Key Vault](xref:security/key-vault-configuration)
1. Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) (tylko środowisko programistyczne)
1. Zmienne środowiskowe
1. Argumenty wiersza polecenia

Typowym celem jest umieszczenie dostawcy konfiguracji wiersza polecenia jako ostatni w serii dostawców, aby zezwolić na argumenty wiersza polecenia, aby przesłonić konfigurację ustawioną przez innych dostawców.

Poprzednia sekwencja dostawców jest używana, gdy nowy Konstruktor hosta zostanie zainicjowany przy użyciu `CreateDefaultBuilder`. Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .

## <a name="configure-the-host-builder-with-useconfiguration"></a>Konfigurowanie konstruktora hostów za pomocą UseConfiguration

Aby skonfigurować konstruktora hostów, wywołaj <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> w konstruktorze hosta z konfiguracją.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

    var config = new ConfigurationBuilder()
        .AddInMemoryCollection(dict)
        .Build();

    return WebHost.CreateDefaultBuilder(args)
        .UseConfiguration(config)
        .UseStartup<Startup>();
}
```

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić dostawców konfiguracji aplikacji oprócz tych dodanych automatycznie przez `CreateDefaultBuilder`:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a>Zastąp poprzednią konfigurację argumentami wiersza polecenia

Aby podać konfigurację aplikacji, którą można zastąpić za pomocą argumentów wiersza polecenia, wywołaj `AddCommandLine` Last:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a>Usuń dostawców dodanych przez CreateDefaultBuilder

Aby usunąć dostawców dodanych przez `CreateDefaultBuilder`, wywołaj najpierw polecenie [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) w [IConfigurationBuilder. sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) :

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a>Użyj konfiguracji podczas uruchamiania aplikacji

Konfiguracja dostarczona do aplikacji w `ConfigureAppConfiguration` jest dostępna podczas uruchamiania aplikacji, w tym `Startup.ConfigureServices`. Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja dostępu podczas uruchamiania](#access-configuration-during-startup) .

## <a name="command-line-configuration-provider"></a>Dostawca konfiguracji wiersza polecenia

<xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> ładuje konfigurację z par klucz-wartość argumentu wiersza polecenia w czasie wykonywania.

Aby uaktywnić konfigurację wiersza polecenia, Metoda rozszerzenia <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> jest wywoływana w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

`AddCommandLine` jest wywoływana automatycznie, gdy zostanie wywołana `CreateDefaultBuilder(string [])`. Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .

`CreateDefaultBuilder` również ładowania:

* Opcjonalna konfiguracja z pliku *appSettings. JSON* i *appSettings. { Environment}. JSON* — pliki.
* Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.
* Zmienne środowiskowe.

`CreateDefaultBuilder` dodaje dostawcę konfiguracji wiersza polecenia Last. Argumenty wiersza polecenia przekazane w czasie wykonywania zastępują konfigurację ustawioną przez innych dostawców.

`CreateDefaultBuilder` działa, gdy host jest skonstruowany. W związku z tym konfiguracja wiersza polecenia aktywowana przez `CreateDefaultBuilder` może mieć wpływ na sposób konfigurowania hosta.

W przypadku aplikacji opartych na ASP.NET Core szablonach `AddCommandLine` została już wywołana przez `CreateDefaultBuilder`. Aby dodać kolejnych dostawców konfiguracji i zachować możliwość przesłonięcia konfiguracji od tych dostawców za pomocą argumentów wiersza polecenia, wywołaj dodatkowych dostawców aplikacji w `ConfigureAppConfiguration` i Wywołaj `AddCommandLine` Last.

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

**Przykład**

Przykładowa aplikacja korzysta z statycznej metody wygodnej `CreateDefaultBuilder` do kompilowania hosta, który obejmuje wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.

1. Otwórz wiersz polecenia w katalogu projektu.
1. Podaj argument wiersza polecenia do polecenia `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.
1. Po uruchomieniu aplikacji otwórz w przeglądarce aplikację w `http://localhost:5000`.
1. Zwróć uwagę, że dane wyjściowe zawierają parę klucz-wartość dla argumentu wiersza polecenia konfiguracji dostarczonego do `dotnet run`.

### <a name="arguments"></a>Argumenty

Wartość musi następować po znaku równości (`=`) lub klucz musi mieć prefiks (`--` lub `/`), gdy wartość znajduje się w miejscu. Wartość nie jest wymagana, jeśli jest używany znak równości (na przykład `CommandLineKey=`).

| Prefiks klucza               | Przykład                                                |
| ------------------------ | ------------------------------------------------------ |
| Brak prefiksu                | `CommandLineKey1=value1`                               |
| Dwie kreski (`--`)        | `--CommandLineKey2=value2`, `--CommandLineKey2 value2` |
| Ukośnik (`/`)      | `/CommandLineKey3=value3`, `/CommandLineKey3 value3`   |

W tym samym poleceniu nie należy mieszać par klucz-wartość argumentu wiersza polecenia, które używają znaku równości z parami klucz-wartość, które używają spacji.

Przykładowe polecenia:

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a>Mapowanie przełączników

Mapowania przełączników Zezwalaj na logikę zamiany nazwy klucza. Podczas ręcznego kompilowania konfiguracji przy użyciu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>należy udostępnić słownik przemieszczeń Switch do metody <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.

Gdy jest używany słownik mapowania przełączników, słownik jest sprawdzany dla klucza, który pasuje do klucza dostarczonego przez argument wiersza polecenia. Jeśli klucz wiersza polecenia zostanie znaleziony w słowniku, wartość słownika (wymiana klucza) zostanie przeniesiona z powrotem, aby ustawić parę klucz-wartość w konfiguracji aplikacji. Mapowanie przełącznika jest wymagane dla każdego klucza wiersza polecenia poprzedzonego jedną kreską (`-`).

Przełącz reguły klucza słownika mapowania:

* Przełączniki muszą zaczynać się kreską (`-`) lub podwójną kreską (`--`).
* Słownik mapowania przełącznika nie może zawierać zduplikowanych kluczy.

Utwórz słownik mapowań mapowania. W poniższym przykładzie są tworzone dwa mapowania przełączników:

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

Po skompilowaniu hosta Wywołaj `AddCommandLine` przy użyciu słownika mapowania przełączników:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

W przypadku aplikacji korzystających z mapowań przełączników wywołanie `CreateDefaultBuilder` nie powinno przekazywać argumentów. Wywołanie `AddCommandLine` metody `CreateDefaultBuilder` nie obejmuje zamapowanych przełączników i nie ma sposobu przekazywania słownika mapowania przełącznika do `CreateDefaultBuilder`. Rozwiązanie nie przekazuje argumentów do `CreateDefaultBuilder`, ale zamiast zezwalać `AddCommandLine` metody `ConfigurationBuilder` do przetwarzania zarówno argumentów, jak i słownika mapowania przełącznika.

Po utworzeniu słownika mapowań przełączników zawiera dane przedstawione w poniższej tabeli.

| Klucz       | Wartość             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

Jeśli klucze mapowane przez przełącznik są używane podczas uruchamiania aplikacji, konfiguracja otrzymuje wartość konfiguracji klucza dostarczonego przez słownik:

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

Po uruchomieniu poprzedniego polecenia Konfiguracja zawiera wartości pokazane w poniższej tabeli.

| Klucz               | Wartość    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a>Dostawca konfiguracji zmiennych środowiskowych

<xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> ładuje konfigurację ze par klucz-wartość zmiennej środowiskowej w czasie wykonywania.

Aby uaktywnić konfigurację zmiennych środowiskowych, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

[Azure App Service](https://azure.microsoft.com/services/app-service/) umożliwia ustawianie zmiennych środowiskowych w witrynie Azure Portal, które mogą przesłonić konfigurację aplikacji przy użyciu dostawcy konfiguracji zmiennych środowiskowych. Aby uzyskać więcej informacji, zobacz artykuł [Azure Apps: zastępowanie konfiguracji aplikacji przy użyciu witryny Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).

`AddEnvironmentVariables` jest używany do ładowania zmiennych środowiskowych, które są poprzedzone `ASPNETCORE_` na potrzeby [konfiguracji hosta](#host-versus-app-configuration) , gdy nowy Konstruktor hosta zostanie zainicjowany przy użyciu [hosta sieci Web](xref:fundamentals/host/web-host) i zostanie wywołane `CreateDefaultBuilder`. Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .

`CreateDefaultBuilder` również ładowania:

* Konfiguracja aplikacji z nieoznaczonych zmiennych środowiskowych przez wywołanie `AddEnvironmentVariables` bez prefiksu.
* Opcjonalna konfiguracja z pliku *appSettings. JSON* i *appSettings. { Environment}. JSON* — pliki.
* Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.
* Argumenty wiersza polecenia.

Dostawca konfiguracji zmiennych środowiskowych jest wywoływany po ustanowieniu konfiguracji z poziomu kluczy tajnych użytkownika i plików *AppSettings* . Wywołanie dostawcy w tym miejscu pozwala odczytywać zmienne środowiskowe w czasie wykonywania w celu przesłania konfiguracji ustawionych przez klucze tajne użytkownika i pliki *AppSettings* .

Aby zapewnić konfigurację aplikacji na podstawie dodatkowych zmiennych środowiskowych, wywołaj dodatkowych dostawców aplikacji w `ConfigureAppConfiguration` i Wywołaj `AddEnvironmentVariables` z prefiksem:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

Wywołaj `AddEnvironmentVariables` ostatnią, aby zezwolić na zmienne środowiskowe z danym prefiksem w celu przesłonięcia wartości z innych dostawców.

**Przykład**

Przykładowa aplikacja korzysta z statycznej metody wygodnej `CreateDefaultBuilder` do kompilowania hosta, który obejmuje wywołanie `AddEnvironmentVariables`.

1. Uruchom przykładową aplikację. Otwórz w przeglądarce aplikację w `http://localhost:5000`.
1. Zwróć uwagę, że dane wyjściowe zawierają parę klucz-wartość dla zmiennej środowiskowej `ENVIRONMENT`. Wartość odzwierciedla środowisko, w którym jest uruchomiona aplikacja, zwykle `Development` podczas uruchamiania lokalnego.

Aby zachować listę zmiennych środowiskowych renderowanych przez aplikację, aplikacja filtruje zmienne środowiskowe. Zapoznaj się z plikiem przykładowej *strony aplikacji/index. cshtml. cs* .

Aby udostępnić wszystkie zmienne środowiskowe dostępne dla aplikacji, należy zmienić `FilteredConfiguration` na *stronie/index. cshtml. cs* w następujący sposób:

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a>Prefiksy

Zmienne środowiskowe ładowane do konfiguracji aplikacji są filtrowane podczas dostarczania prefiksu do metody `AddEnvironmentVariables`. Na przykład aby filtrować zmienne środowiskowe na prefiksie `CUSTOM_`, podaj prefiks dla dostawcy konfiguracji:

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

Prefiks jest usuwany podczas tworzenia par klucz-wartość konfiguracji.

Podczas tworzenia konstruktora hostów Konfiguracja hosta jest zapewniana przez zmienne środowiskowe. Aby uzyskać więcej informacji na temat prefiksu używanego dla tych zmiennych środowiskowych, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .

**Prefiksy parametrów połączenia**

Interfejs API konfiguracji ma specjalne reguły przetwarzania dla czterech zmiennych środowiskowych parametrów połączenia związanych z konfigurowaniem parametrów połączenia platformy Azure dla środowiska aplikacji. Zmienne środowiskowe z prefiksami podanymi w tabeli są ładowane do aplikacji, jeśli nie podano prefiksu do `AddEnvironmentVariables`.

| Prefiks parametrów połączenia | Dostawca |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | Dostawca niestandardowy |
| `MYSQLCONNSTR_` | [MySQL](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [SQL Server](https://www.microsoft.com/sql-server/) |

Gdy zmienna środowiskowa zostanie odnaleziona i załadowana do konfiguracji z dowolnymi z czterech prefiksów pokazanych w tabeli:

* Klucz konfiguracji jest tworzony przez usunięcie prefiksu zmiennej środowiskowej i dodanie sekcji klucza konfiguracji (`ConnectionStrings`).
* Zostanie utworzona nowa para klucz-wartość konfiguracji, która reprezentuje dostawcę połączenia bazy danych (z wyjątkiem `CUSTOMCONNSTR_`, które nie ma określonego dostawcy).

| Klucz zmiennej środowiskowej | Przekonwertowany klucz konfiguracji | Wpis konfiguracji dostawcy                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | Wpis konfiguracji nie został utworzony.                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | Klucz: `ConnectionStrings:{KEY}_ProviderName`:<br>Wartość:`MySql.Data.MySqlClient` |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | Klucz: `ConnectionStrings:{KEY}_ProviderName`:<br>Wartość:`System.Data.SqlClient`  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | Klucz: `ConnectionStrings:{KEY}_ProviderName`:<br>Wartość:`System.Data.SqlClient`  |

**Przykład**

Na serwerze zostanie utworzona niestandardowa zmienna środowiskowa parametrów połączenia:

* Nazwa &ndash; `CUSTOMCONNSTR_ReleaseDB`
* `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True` &ndash; wartości

Jeśli `IConfiguration` zostanie dodany i przypisany do pola o nazwie `_config`, przeczytaj wartość:

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a>Dostawca konfiguracji plików

<xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> jest klasą bazową do ładowania konfiguracji z systemu plików. Następujący dostawcy konfiguracji są przydzielone do określonych typów plików:

* [Dostawca konfiguracji pliku INI](#ini-configuration-provider)
* [Dostawca konfiguracji JSON](#json-configuration-provider)
* [Dostawca konfiguracji XML](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a>Dostawca konfiguracji pliku INI

<xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku INI w czasie wykonywania.

Aby uaktywnić konfigurację pliku INI, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Dwukropek może służyć jako ogranicznik sekcji w konfiguracji pliku INI.

Przeciążania Zezwalaj na określanie:

* Czy plik jest opcjonalny.
* Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> używany do uzyskiwania dostępu do pliku.

Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

Ogólny przykład pliku konfiguracji INI:

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

Poprzedni plik konfiguracji ładuje następujące klucze z `value`:

* section0:key0
* section0: Klucz1
* Section1: podsekcja: Key
* section2: subsection0: klucz
* section2: subsection1: klucz

### <a name="json-configuration-provider"></a>Dostawca konfiguracji JSON

<xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku JSON podczas środowiska uruchomieniowego.

Aby uaktywnić konfigurację pliku JSON, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Przeciążania Zezwalaj na określanie:

* Czy plik jest opcjonalny.
* Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> używany do uzyskiwania dostępu do pliku.

`AddJsonFile` jest automatycznie wywoływana dwukrotnie, gdy nowy Konstruktor hosta zostanie zainicjowany z `CreateDefaultBuilder`. Metoda jest wywoływana w celu załadowania konfiguracji z:

* *appSettings. json* &ndash; ten plik jest najpierw odczytywany. Wersja środowiska pliku może przesłonić wartości dostarczone przez plik *appSettings. JSON* .
* *appSettings. {Environment}. JSON* &ndash; wersja środowiska pliku jest ładowana na podstawie [IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).

Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .

`CreateDefaultBuilder` również ładowania:

* Zmienne środowiskowe.
* Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.
* Argumenty wiersza polecenia.

Dostawca konfiguracji JSON został ustanowiony jako pierwszy. W związku z tym klucze tajne użytkownika, zmienne środowiskowe i argumenty wiersza polecenia przesłaniają konfigurację ustawioną przez pliki *AppSettings* .

Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji dla plików innych niż *appSettings. JSON* i *appSettings. { Environment}. JSON*:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

**Przykład**

Przykładowa aplikacja korzysta z statycznej metody wygodnej `CreateDefaultBuilder` do kompilowania hosta, który obejmuje dwa wywołania `AddJsonFile`:

* Pierwsze wywołanie `AddJsonFile` ładuje konfigurację z pliku *appSettings. JSON*:

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* Drugie wywołanie `AddJsonFile` ładuje konfigurację z *appSettings. { Environment}. JSON*. Dla *appSettings. Plik Development. JSON* w przykładowej aplikacji jest ładowany:

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. Uruchom przykładową aplikację. Otwórz w przeglądarce aplikację w `http://localhost:5000`.
1. Dane wyjściowe zawierają pary klucz-wartość dla konfiguracji w oparciu o środowisko aplikacji. Poziom dziennika dla klucza `Logging:LogLevel:Default` jest `Debug` podczas uruchamiania aplikacji w środowisku deweloperskim.
1. Ponownie uruchom przykładową aplikację w środowisku produkcyjnym:
   1. Otwórz plik *Properties/profilu launchsettings. JSON* .
   1. W profilu `ConfigurationSample` Zmień wartość zmiennej środowiskowej `ASPNETCORE_ENVIRONMENT` na `Production`.
   1. Zapisz plik i uruchom aplikację za pomocą `dotnet run` w powłoce poleceń.
1. Ustawienia w *appSettings. Plik Development. JSON* nie zastępuje już ustawień w pliku *appSettings. JSON*. Poziom dziennika `Logging:LogLevel:Default` jest `Warning`.

### <a name="xml-configuration-provider"></a>Dostawca konfiguracji XML

<xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> ładuje konfigurację z par klucz-wartość pliku XML w czasie wykonywania.

Aby uaktywnić konfigurację pliku XML, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Przeciążania Zezwalaj na określanie:

* Czy plik jest opcjonalny.
* Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> używany do uzyskiwania dostępu do pliku.

Węzeł główny pliku konfiguracji jest ignorowany, gdy są tworzone pary klucz-wartość konfiguracji. Nie określaj definicji typu dokumentu (DTD) ani przestrzeni nazw w pliku.

Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

Pliki konfiguracji XML mogą używać odrębnych nazw elementów dla powtarzających się sekcji:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

Poprzedni plik konfiguracji ładuje następujące klucze z `value`:

* section0:key0
* section0: Klucz1
* section1:key0
* Section1: Klucz1

Powtarzalne elementy, które używają tej samej nazwy elementu, działają, jeśli atrybut `name` jest używany do odróżnienia elementów:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

Poprzedni plik konfiguracji ładuje następujące klucze z `value`:

* sekcja: section0: Key: Key0
* sekcja: section0: Key: Klucz1
* sekcja: Section1: Key: Key0
* sekcja: Section1: Key: Klucz1

Atrybuty mogą służyć do dostarczania wartości:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

Poprzedni plik konfiguracji ładuje następujące klucze z `value`:

* Key: Attribute
* sekcja: Key: Attribute

## <a name="key-per-file-configuration-provider"></a>Dostawca konfiguracji klucza dla plików

<xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> używa plików katalogu jako par klucz konfiguracji i wartość. Kluczem jest nazwa pliku. Wartość zawiera zawartość pliku. Dostawca konfiguracji klucza dla plików jest używany w scenariuszach hostingu platformy Docker.

Aby uaktywnić konfigurację klucza dla plików, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>. `directoryPath` plików musi być ścieżką bezwzględną.

Przeciążania Zezwalaj na określanie:

* Delegat `Action<KeyPerFileConfigurationSource>`, który konfiguruje źródło.
* Określa, czy katalog jest opcjonalny, i ścieżkę do katalogu.

Podwójny znak podkreślenia (`__`) jest używany jako ogranicznik klucza konfiguracji w nazwach plików. Na przykład nazwa pliku `Logging__LogLevel__System` generuje klucz konfiguracji `Logging:LogLevel:System`.

Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a>Dostawca konfiguracji pamięci

<xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> używa kolekcji w pamięci jako par klucz-wartość konfiguracji.

Aby uaktywnić konfigurację kolekcji w pamięci, wywołaj metodę rozszerzenia <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> w wystąpieniu <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Dostawcę konfiguracji można zainicjować przy użyciu `IEnumerable<KeyValuePair<String,String>>`.

Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji.

W poniższym przykładzie tworzony jest słownik konfiguracji:

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

Słownik jest używany z wywołaniem `AddInMemoryCollection`, aby zapewnić konfigurację:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a>GetValue

[ConfigurationBinder. GetValue\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) wyodrębnia jedną wartość z konfiguracji z określonym kluczem i konwertuje ją na określony typ niekolekcje. Przeciążenie akceptuje wartość domyślną.

Poniższy przykład:

* Wyodrębnia wartość ciągu z konfiguracji z kluczem `NumberKey`. Jeśli nie można odnaleźć `NumberKey` w kluczach konfiguracji, zostanie użyta wartość domyślna `99`.
* Typ wartości jako `int`.
* Przechowuje wartość we właściwości `NumberConfig` do użycia na stronie.

```csharp
public class IndexModel : PageModel
{
    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    public int NumberConfig { get; private set; }

    public void OnGet()
    {
        NumberConfig = _config.GetValue<int>("NumberKey", 99);
    }
}
```

## <a name="getsection-getchildren-and-exists"></a>GetSection, GetChildren i EXISTS

W poniższych przykładach należy wziąć pod uwagę następujący plik JSON. Cztery klucze są dostępne w dwóch sekcjach, z których jedna zawiera parę podsekcji:

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

Gdy plik jest odczytywany do konfiguracji, następujące unikatowe klucze hierarchiczne są tworzone w celu przechowywania wartości konfiguracyjnych:

* section0:key0
* section0: Klucz1
* section1:key0
* Section1: Klucz1
* section2:subsection0:key0
* section2: subsection0: Klucz1
* section2:subsection1:key0
* section2: subsection1: Klucz1

### <a name="getsection"></a>GetSection

[IConfiguration. GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) wyodrębnia podsekcję konfiguracji z określonym kluczem podsekcji.

Aby zwrócić <xref:Microsoft.Extensions.Configuration.IConfigurationSection> zawierający tylko pary klucz-wartość w `section1`, wywołaj `GetSection` i podaj nazwę sekcji:

```csharp
var configSection = _config.GetSection("section1");
```

`configSection` nie ma wartości, tylko klucza i ścieżki.

Podobnie, aby uzyskać wartości kluczy w `section2:subsection0`, wywołaj `GetSection` i podaj ścieżkę sekcji:

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

`GetSection` nigdy nie zwraca `null`. Jeśli nie znaleziono pasującej sekcji, zwracany jest pusty `IConfigurationSection`.

Gdy `GetSection` zwraca pasującą sekcję, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> nie jest wypełnione. <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> i <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> są zwracane, gdy istnieje sekcja.

### <a name="getchildren"></a>GetChildren

Wywołanie [iConfiguration. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) w `section2` uzyskuje `IEnumerable<IConfigurationSection>` obejmujący:

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a>Exists

Użyj [ConfigurationExtensions. istnieje](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) , aby określić, czy istnieje sekcja konfiguracji:

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

Dane przykładowe `sectionExists` jest `false`, ponieważ w danych konfiguracyjnych nie ma `section2:subsection2` sekcji.

## <a name="bind-to-a-class"></a>Powiąż z klasą

Konfigurację można powiązać z klasami, które reprezentują grupy powiązanych ustawień przy użyciu *wzorca opcji*. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.

Wartości konfiguracji są zwracane jako ciągi, ale wywołanie <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> umożliwia konstruowanie obiektów [poco](https://wikipedia.org/wiki/Plain_Old_CLR_Object) . Obiekt wiążący wiąże wartości ze wszystkimi publicznymi właściwościami odczytu/zapisu dostarczonego typu. Pola **nie** są powiązane.

Przykładowa aplikacja zawiera model `Starship` (*modele/Starship. cs*):

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

Sekcja `starship` pliku *Starship. JSON* tworzy konfigurację, gdy aplikacja Przykładowa używa dostawcy konfiguracji JSON do załadowania konfiguracji:

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

Tworzone są następujące pary klucz-wartość konfiguracji:

| Klucz                   | Wartość                                             |
| --------------------- | ------------------------------------------------- |
| Starship: Nazwa         | USS Enterprise                                    |
| Starship: Rejestr     | NCC-1701                                          |
| Starship: Klasa        | Skład                                      |
| Starship: Długość       | 304,8                                             |
| Starship: prowizja | False                                             |
| handlowych             | Najważniejsze obrazy Corp. https://www.paramount.com |

Przykładowa aplikacja wywołuje `GetSection` z kluczem `starship`. Pary klucz-wartość `starship` są odizolowane. Metoda `Bind` jest wywoływana w podsekcji przekazującej w wystąpieniu klasy `Starship`. Po powiązaniu wartości wystąpień wystąpienie jest przypisywane do właściwości w celu renderowania:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

## <a name="bind-to-an-object-graph"></a>Powiąż z grafem obiektów

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> jest w stanie powiązać cały Graf obiektów POCO. Podobnie jak w przypadku powiązania prostego obiektu, powiązane są tylko publiczne właściwości odczytu i zapisu.

Przykład zawiera model `TvShow`, którego wykres obiektu zawiera klasy `Metadata` i `Actors` (*modele/TvShow. cs*):

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

Przykładowa aplikacja zawiera plik *tvshow. XML* zawierający dane konfiguracji:

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

Konfiguracja jest powiązana z całym grafem obiektu `TvShow` za pomocą metody `Bind`. Powiązane wystąpienie jest przypisane do właściwości w celu renderowania:

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

[ConfigurationBinder. Get\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) tworzy powiązania i zwraca określony typ. `Get<T>` jest wygodniejszy niż korzystanie z `Bind`. Poniższy kod pokazuje, jak używać `Get<T>` w poprzednim przykładzie, co umożliwia bezpośrednie przypisanie wystąpienia powiązanego do właściwości używanej do renderowania:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a>Powiąż tablicę z klasą

*Przykładowa aplikacja pokazuje Koncepcje opisane w tej sekcji.*

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> obsługuje powiązania tablic z obiektami przy użyciu indeksów tablicowych w kluczach konfiguracji. Każdy format tablicy, który ujawnia segment klucza numerycznego (`:0:`, `:1:`, &hellip; `:{n}:`), jest w stanie powiązać powiązanie tablicową z tablicą klas POCO.

> [!NOTE]
> Powiązanie jest dostarczane według Konwencji. Niestandardowi dostawcy konfiguracji nie muszą implementować powiązania tablicy.

**Przetwarzanie tablicy w pamięci**

Należy wziąć pod uwagę klucze konfiguracji i wartości podane w poniższej tabeli.

| Klucz             | Wartość  |
| :-------------: | :----: |
| Tablica: wpisy: 0 | value0 |
| Tablica: wpisy: 1 | Sekwencj |
| Tablica: wpisy: 2 | Wartość2 |
| Tablica: wpisy: 4 | value4 |
| Tablica: wpisy: 5 | value5 |

Te klucze i wartości są ładowane w przykładowej aplikacji przy użyciu dostawcy konfiguracji pamięci:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

Tablica pomija wartość indeksu &num;3. Segregator konfiguracji nie może powiązać wartości null ani tworzyć wpisów o wartości null w obiektach powiązanych, co oznacza, że w chwili pojawi się wynik powiązania tej tablicy z obiektem.

W przykładowej aplikacji jest dostępna Klasa POCO, która przechowuje powiązane dane konfiguracji:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

Dane konfiguracji są powiązane z obiektem:

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

[ConfigurationBinder. Get\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) można również użyć składni, co spowoduje zwiększenie kodu kompaktowego:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

Obiekt powiązany, wystąpienie `ArrayExample`, otrzymuje dane tablicy z konfiguracji.

| Indeks `ArrayExample.Entries` | Wartość `ArrayExample.Entries` |
| :--------------------------: | :--------------------------: |
| 0                            | value0                       |
| 1                            | Sekwencj                       |
| 2                            | Wartość2                       |
| 3                            | value4                       |
| 4                            | value5                       |

Indeks &num;3 w obiekcie powiązanym zawiera dane konfiguracyjne `array:4` klucza konfiguracji i jego wartość `value4`. Gdy dane konfiguracji zawierające tablicę są powiązane, indeksy tablic w kluczach konfiguracji są używane tylko do iteracji danych konfiguracji podczas tworzenia obiektu. Wartości null nie można zachować w danych konfiguracyjnych, a wpis o wartości null nie jest tworzony w obiekcie powiązanym, gdy tablica w kluczach konfiguracji pomija jeden lub więcej indeksów.

Brakujący element konfiguracji dla indeksu &num;3 można dostarczyć przed powiązaniem do wystąpienia `ArrayExample` przez dowolnego dostawcę konfiguracji, który wygeneruje poprawną parę klucz-wartość w konfiguracji. Jeśli przykład zawiera dodatkowego dostawcę konfiguracji JSON z brakującą parą klucz-wartość, `ArrayExample.Entries` pasuje do kompletnej tablicy konfiguracji:

plik *missing_value. JSON*:

```json
{
  "array:entries:3": "value3"
}
```

W pliku `ConfigureAppConfiguration`:

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

Para klucz-wartość pokazana w tabeli jest ładowana do konfiguracji.

| Klucz             | Wartość  |
| :-------------: | :----: |
| Tablica: wpisy: 3 | Wartość3 |

Jeśli wystąpienie klasy `ArrayExample` jest powiązane, gdy dostawca konfiguracji JSON zawiera wpis dla indeksu &num;3, tablica `ArrayExample.Entries` zawiera wartość.

| Indeks `ArrayExample.Entries` | Wartość `ArrayExample.Entries` |
| :--------------------------: | :--------------------------: |
| 0                            | value0                       |
| 1                            | Sekwencj                       |
| 2                            | Wartość2                       |
| 3                            | Wartość3                       |
| 4                            | value4                       |
| 5                            | value5                       |

**Przetwarzanie tablicy JSON**

Jeśli plik JSON zawiera tablicę, klucze konfiguracji są tworzone dla elementów tablicy z indeksem sekcji o wartości zero. W poniższym pliku konfiguracyjnym `subsection` jest tablicą:

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

Dostawca konfiguracji JSON odczytuje dane konfiguracji do następujących par klucz-wartość:

| Klucz                     | Wartość  |
| ----------------------- | :----: |
| json_array: klucz          | wartośća |
| json_array:subsection:0 | Wartośćb |
| json_array:subsection:1 | valueC |
| json_array:subsection:2 | Znajdując |

W przykładowej aplikacji jest dostępna następująca Klasa POCO z powiązaniem par klucz-wartość konfiguracji:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

Po powiązaniu `JsonArrayExample.Key` utrzymuje `valueA`wartości. Wartości podsekcji są przechowywane we właściwości tablicy POCO `Subsection`.

| Indeks `JsonArrayExample.Subsection` | Wartość `JsonArrayExample.Subsection` |
| :---------------------------------: | :---------------------------------: |
| 0                                   | Wartośćb                              |
| 1                                   | valueC                              |
| 2                                   | Znajdując                              |

## <a name="custom-configuration-provider"></a>Niestandardowy dostawca konfiguracji

Przykładowa aplikacja pokazuje, jak utworzyć podstawowego dostawcę konfiguracji, który odczytuje pary klucz-wartość konfiguracji z bazy danych przy użyciu [Entity Framework (EF)](/ef/core/).

Dostawca ma następującą charakterystykę:

* Baza danych EF w pamięci jest używana w celach demonstracyjnych. Aby użyć bazy danych, która wymaga parametrów połączenia, zaimplementuj pomocniczą `ConfigurationBuilder`, aby podać parametry połączenia od innego dostawcy konfiguracji.
* Dostawca odczytuje tabelę bazy danych w konfiguracji podczas uruchamiania. Dostawca nie wykonuje zapytania do bazy danych w oparciu o klucz.
* Ponowne załadowanie nie zostało zaimplementowane, więc aktualizacja bazy danych po uruchomieniu aplikacji nie ma wpływu na konfigurację aplikacji.

Zdefiniuj jednostkę `EFConfigurationValue` do przechowywania wartości konfiguracji w bazie danych.

*Modele/EFConfigurationValue. cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

Dodaj `EFConfigurationContext` do przechowywania skonfigurowanych wartości i uzyskiwania do nich dostępu.

*EFConfigurationProvider/EFConfigurationContext. cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

Utwórz klasę, która implementuje <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.

*EFConfigurationProvider/EFConfigurationSource. cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

Tworzenie niestandardowego dostawcy konfiguracji przez dziedziczenie z <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>. Dostawca konfiguracji inicjuje bazę danych, gdy jest pusta.

*EFConfigurationProvider/EFConfigurationProvider. cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

Metoda rozszerzenia `AddEFConfiguration` zezwala na Dodawanie źródła konfiguracji do `ConfigurationBuilder`.

*Rozszerzenia/EntityFrameworkExtensions. cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

Poniższy kod pokazuje, jak używać niestandardowych `EFConfigurationProvider` w *program.cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a>Konfiguracja dostępu podczas uruchamiania

Wsuń `IConfiguration` do konstruktora `Startup`, aby uzyskać dostęp do wartości konfiguracyjnych w `Startup.ConfigureServices`. Aby uzyskać dostęp do konfiguracji w `Startup.Configure`, należy wstrzyknąć `IConfiguration` bezpośrednio do metody lub użyć wystąpienia z konstruktora:

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

Aby zapoznać się z przykładem uzyskiwania dostępu do konfiguracji przy użyciu metod uruchamiania, zobacz [Uruchamianie aplikacji: wygodne metody](xref:fundamentals/startup#convenience-methods).

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a>Konfiguracja dostępu na stronie Razor Pages lub widoku MVC

Aby uzyskać dostęp do ustawień konfiguracji na stronie Razor Pages lub widoku MVC, Dodaj [dyrektywę using](xref:mvc/views/razor#using) ([ C# odwołanie: Using](/dotnet/csharp/language-reference/keywords/using-directive)) dla [przestrzeni nazw Microsoft. Extensions. Configuration](xref:Microsoft.Extensions.Configuration) i wsuń <xref:Microsoft.Extensions.Configuration.IConfiguration> do strony lub widoku.

Na stronie Razor Pages:

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
    <p>Configuration value for 'key': @Configuration["key"]</p>
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
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a>Dodawanie konfiguracji z zestawu zewnętrznego

Implementacja <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> umożliwia dodawanie ulepszeń do aplikacji podczas uruchamiania z zestawu zewnętrznego poza klasą `Startup` aplikacji. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/configuration/options>

::: moniker-end
