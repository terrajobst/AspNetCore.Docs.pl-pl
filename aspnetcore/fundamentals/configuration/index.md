---
title: Konfiguracja w ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować aplikację ASP.NET Core przy użyciu interfejsu API konfiguracji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/12/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 357a3d89648086f0329cd16bc9d72863df9bdcd6
ms.sourcegitcommit: 8a36be1bfee02eba3b07b7a86085ec25c38bae6b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2019
ms.locfileid: "71217789"
---
# <a name="configuration-in-aspnet-core"></a>Konfiguracja w ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

Konfiguracja aplikacji w ASP.NET Core jest oparta na parach klucz-wartość określonych przez *dostawców konfiguracji*. Dostawcy konfiguracji odczytują dane konfiguracji do par klucz-wartość z różnych źródeł konfiguracji:

* W usłudze Azure Key Vault
* Konfiguracja aplikacji platformy Azure
* Argumenty wiersza polecenia
* Dostawcy niestandardowi (instalowani lub utworzony)
* Pliki katalogu
* Zmienne środowiskowe
* Obiekty w pamięci .NET
* Pliki ustawień

::: moniker range=">= aspnetcore-3.0"

Pakiety konfiguracyjne dla typowych scenariuszy dostawcy konfiguracji ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) są dołączone niejawnie przez platformę.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Pakiety konfiguracyjne dla typowych scenariuszy dostawcy konfiguracji ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) są zawarte w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).

::: moniker-end

Przykłady kodu, które obserwują i w przykładowej aplikacji <xref:Microsoft.Extensions.Configuration> używają przestrzeni nazw:

```csharp
using Microsoft.Extensions.Configuration;
```

*Wzorzec opcji* jest rozszerzeniem pojęć konfiguracyjnych opisanych w tym temacie. Opcje używają klas do reprezentowania grup powiązanych ustawień. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="host-versus-app-configuration"></a>Host a konfiguracja aplikacji

Przed skonfigurowaniem i uruchomieniem aplikacji *host* zostanie skonfigurowany i uruchomiony. Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji. Zarówno aplikacja, jak i Host są konfigurowane przy użyciu dostawców konfiguracji opisanych w tym temacie. Klucz konfiguracji hosta — pary wartości są również uwzględnione w konfiguracji aplikacji. Aby uzyskać więcej informacji na temat tego, jak dostawcy konfiguracji są używani podczas kompilowania hosta i jak źródła konfiguracji wpływają na <xref:fundamentals/index#host>konfigurację hosta, zobacz.

## <a name="default-configuration"></a>Konfiguracja domyślna

::: moniker range=">= aspnetcore-3.0"

Aplikacje sieci Web oparte na ASP.NET Coreniu [nowych](/dotnet/core/tools/dotnet-new) szablonów dotnet <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> są wywoływane podczas kompilowania hosta. `CreateDefaultBuilder`zapewnia domyślną konfigurację dla aplikacji w następującej kolejności:

Poniższe zasady dotyczą aplikacji korzystających z [hosta ogólnego](xref:fundamentals/host/generic-host). Aby uzyskać szczegółowe informacje na temat konfiguracji domyślnej podczas korzystania z [hosta sieci Web](xref:fundamentals/host/web-host), zobacz [wersję ASP.NET Core 2,2 tego tematu](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).

* Konfiguracja hosta jest poświadczona z:
  * Zmienne środowiskowe poprzedzone `DOTNET_` znakiem (na `DOTNET_ENVIRONMENT`przykład) przy użyciu [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider). Prefiks (`DOTNET_`) jest usuwany, gdy są ładowane pary klucz-wartość konfiguracji.
  * Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).
* Konfiguracja domyślna hosta sieci Web (`ConfigureWebHostDefaults`):
  * Kestrel jest używany jako serwer sieci Web i konfigurowany przy użyciu dostawców konfiguracji aplikacji.
  * Dodaj oprogramowanie pośredniczące do filtrowania hosta.
  * Dodaj przekierowane nagłówki — oprogramowanie pośredniczące, `ASPNETCORE_FORWARDEDHEADERS_ENABLED` Jeśli zmienna środowiskowa jest ustawiona na. `true`
  * Włącz integrację usług IIS.
* Podano konfigurację aplikacji z:
  * *appSettings. JSON* przy użyciu [dostawcy konfiguracji plików](#file-configuration-provider).
  * *appSettings. {Environment}. JSON* przy użyciu [dostawcy konfiguracji pliku](#file-configuration-provider).
  * [Secret Manager](xref:security/app-secrets) , gdy aplikacja jest uruchamiana `Development` w środowisku przy użyciu zestawu wpisów.
  * Zmienne środowiskowe używające [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).
  * Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Aplikacje sieci Web oparte na ASP.NET Coreniu [nowych](/dotnet/core/tools/dotnet-new) szablonów dotnet <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> są wywoływane podczas kompilowania hosta. `CreateDefaultBuilder`zapewnia domyślną konfigurację dla aplikacji w następującej kolejności:

Poniższe zasady dotyczą aplikacji korzystających z [hosta sieci Web](xref:fundamentals/host/web-host). Aby uzyskać szczegółowe informacje na temat konfiguracji domyślnej w przypadku korzystania z [hosta ogólnego](xref:fundamentals/host/generic-host), zobacz [najnowszą wersję tego tematu](xref:fundamentals/configuration/index).

* Konfiguracja hosta jest poświadczona z:
  * Zmienne środowiskowe poprzedzone `ASPNETCORE_` znakiem (na `ASPNETCORE_ENVIRONMENT`przykład) przy użyciu [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider). Prefiks (`ASPNETCORE_`) jest usuwany, gdy są ładowane pary klucz-wartość konfiguracji.
  * Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).
* Podano konfigurację aplikacji z:
  * *appSettings. JSON* przy użyciu [dostawcy konfiguracji plików](#file-configuration-provider).
  * *appSettings. {Environment}. JSON* przy użyciu [dostawcy konfiguracji pliku](#file-configuration-provider).
  * [Secret Manager](xref:security/app-secrets) , gdy aplikacja jest uruchamiana `Development` w środowisku przy użyciu zestawu wpisów.
  * Zmienne środowiskowe używające [dostawcy konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider).
  * Argumenty wiersza polecenia przy użyciu [dostawcy konfiguracji wiersza polecenia](#command-line-configuration-provider).

::: moniker-end

## <a name="security"></a>Zabezpieczenia

Aby zabezpieczyć poufne dane konfiguracji, należy zastosować następujące rozwiązania:

* Nie należy przechowywać haseł ani innych danych poufnych w kodzie dostawcy konfiguracji ani w plikach konfiguracji zwykłego tekstu.
* Nie używaj tajemnic produkcyjnych w środowiskach deweloperskich i testowych.
* Określ wpisy tajne poza projektem, aby nie mogły zostać przypadkowo przekazane do repozytorium kodu źródłowego.

Więcej informacji znajduje się w następujących tematach:

* <xref:fundamentals/environments>
* <xref:security/app-secrets>&ndash; Zawiera porady dotyczące używania zmiennych środowiskowych do przechowywania poufnych danych. Menedżer wpisów tajnych używa dostawcy konfiguracji plików do przechowywania wpisów tajnych użytkownika w pliku JSON w systemie lokalnym. Dostawca konfiguracji plików został opisany w dalszej części tego tematu.

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

<xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>metody <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> i są dostępne do izolowania sekcji i elementów podrzędnych sekcji w danych konfiguracyjnych. Te metody są opisane w dalszej [części GetSection, GetChildren i EXISTS](#getsection-getchildren-and-exists).

## <a name="conventions"></a>Konwencje

### <a name="sources-and-providers"></a>Źródła i dostawcy

Podczas uruchamiania aplikacji źródła konfiguracji są odczytywane w kolejności, w jakiej są określone przez ich dostawców konfiguracji.

Dostawcy konfiguracji implementujący funkcję wykrywania zmian mają możliwość ponownego załadowania konfiguracji, gdy ustawienie podstawowe zostanie zmienione. Na przykład dostawca konfiguracji plików (opisany w dalszej części tego tematu) i [dostawca konfiguracji Azure Key Vault](xref:security/key-vault-configuration) implementują wykrywanie zmian.

<xref:Microsoft.Extensions.Configuration.IConfiguration>jest dostępny w kontenerze [iniekcji zależności](xref:fundamentals/dependency-injection) aplikacji. <xref:Microsoft.Extensions.Configuration.IConfiguration>można wstrzyknąć do Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> , aby uzyskać konfigurację dla klasy:

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    // The _config local variable is used to obtain configuration 
    // throughout the class.
}
```

Dostawcy konfiguracji nie mogą używać DI, ponieważ są niedostępne, gdy są skonfigurowane przez hosta.

### <a name="keys"></a>Ponownie

Klucze konfiguracji przyjmują następujące konwencje:

* W kluczach nie jest rozróżniana wielkość liter. Na przykład `ConnectionString` i `connectionstring` są traktowane jako równoważne klucze.
* Jeśli wartość tego samego klucza jest ustawiana przez tych samych lub różnych dostawców konfiguracji, Ostatnia wartość ustawiona w tym kluczu jest używana.
* Klucze hierarchiczne
  * W interfejsie API konfiguracji, separator dwukropek`:`() działa na wszystkich platformach.
  * W zmiennych środowiskowych separator dwukropek może nie zadziałał na wszystkich platformach. Podwójne podkreślenie (`__`) jest obsługiwane przez wszystkie platformy i automatycznie konwertowane na dwukropek.
  * W Azure Key Vault klucze hierarchiczne używają `--` (dwóch kresek) jako separatora. Należy podać kod, aby zastąpić łączniki dwukropkiem, gdy wpisy tajne są ładowane do konfiguracji aplikacji.
* <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> Obsługuje tablice powiązań z obiektami przy użyciu indeksów tablicowych w kluczach konfiguracji. Powiązanie tablicowe zostało opisane w sekcji [Powiązywanie tablicy z klasą](#bind-an-array-to-a-class) .

### <a name="values"></a>Wartości

Wartości konfiguracyjne przyjmują następujące konwencje:

* Wartości są ciągami.
* Wartości null nie można przechowywać w konfiguracji ani powiązana z obiektami.

## <a name="providers"></a>udostępnia

W poniższej tabeli przedstawiono dostawców konfiguracji dostępnych do ASP.NET Core aplikacji.

| Dostawca | Zapewnia konfigurację z&hellip; |
| -------- | ----------------------------------- |
| [Dostawca konfiguracji Azure Key Vault](xref:security/key-vault-configuration) (Tematy dotyczące*zabezpieczeń* ) | W usłudze Azure Key Vault |
| [Dostawca konfiguracji aplikacji platformy Azure](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Dokumentacja platformy Azure) | Konfiguracja aplikacji platformy Azure |
| [Dostawca konfiguracji wiersza polecenia](#command-line-configuration-provider) | Parametry wiersza polecenia |
| [Niestandardowy dostawca konfiguracji](#custom-configuration-provider) | Źródło niestandardowe |
| [Dostawca konfiguracji zmiennych środowiskowych](#environment-variables-configuration-provider) | Zmienne środowiskowe |
| [Dostawca konfiguracji plików](#file-configuration-provider) | Pliki (INI, JSON, XML) |
| [Dostawca konfiguracji klucza dla plików](#key-per-file-configuration-provider) | Pliki katalogu |
| [Dostawca konfiguracji pamięci](#memory-configuration-provider) | Kolekcje w pamięci |
| Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) (Tematy dotyczące*zabezpieczeń* ) | Plik w katalogu profilu użytkownika |

Źródła konfiguracji są odczytywane w kolejności, w jakiej dostawcy konfiguracji są określeni podczas uruchamiania. Dostawcy konfiguracji opisane w tym temacie są opisywane w kolejności alfabetycznej, a nie w kolejności, w jakiej kod może je rozmieścić. Zamów dostawcy konfiguracji w kodzie, aby odpowiadały Twoim priorytetom dla źródłowych źródeł konfiguracji.

Typową sekwencją dostawców konfiguracji jest:

1. Pliki (*appSettings. JSON*, *appSettings. { Environment}. JSON*, gdzie `{Environment}` to bieżące środowisko hostingu aplikacji)
1. [Azure Key Vault](xref:security/key-vault-configuration)
1. Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) (Tylko środowisko programistyczne)
1. Zmienne środowiskowe
1. Argumenty wiersza polecenia

Typowym celem jest umieszczenie dostawcy konfiguracji wiersza polecenia jako ostatni w serii dostawców, aby zezwolić na argumenty wiersza polecenia, aby przesłonić konfigurację ustawioną przez innych dostawców.

Poprzednia sekwencja dostawców jest używana po zainicjowaniu nowego konstruktora hostów za `CreateDefaultBuilder`pomocą programu. Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a>Konfigurowanie konstruktora hostów za pomocą ConfigureHostConfiguration

Aby skonfigurować konstruktora hostów, wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> i podaj konfigurację. `ConfigureHostConfiguration`służy do inicjowania <xref:Microsoft.Extensions.Hosting.IHostEnvironment> usługi do użycia w dalszej części procesu kompilacji. `ConfigureHostConfiguration`może być wywoływana wiele razy z wynikami.

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="configure-the-host-builder-with-useconfiguration"></a>Konfigurowanie konstruktora hostów za pomocą UseConfiguration

Aby skonfigurować konstruktora hosta, należy wywołać <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> konstruktora hosta z konfiguracją.

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

::: moniker-end

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić dostawców konfiguracji aplikacji oprócz tych dodanych automatycznie przez `CreateDefaultBuilder`:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a>Zastąp poprzednią konfigurację argumentami wiersza polecenia

Aby podać konfigurację aplikacji, którą można zastąpić za pomocą argumentów wiersza polecenia, wywołaj `AddCommandLine` ostatni:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="consume-configuration-during-app-startup"></a>Użyj konfiguracji podczas uruchamiania aplikacji

Konfiguracja dostarczona do aplikacji w `ConfigureAppConfiguration` programie jest dostępna podczas uruchamiania aplikacji, w tym `Startup.ConfigureServices`. Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja dostępu podczas uruchamiania](#access-configuration-during-startup) .

## <a name="command-line-configuration-provider"></a>Dostawca konfiguracji wiersza polecenia

<xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> Ładowanie konfiguracji z par klucz-wartość argumentu wiersza polecenia w czasie wykonywania.

Aby uaktywnić konfigurację wiersza polecenia, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> Metoda rozszerzenia jest wywoływana w <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>wystąpieniu.

`AddCommandLine`jest wywoływana automatycznie, `CreateDefaultBuilder(string [])` gdy jest wywoływana. Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .

`CreateDefaultBuilder`ładuje również:

* Opcjonalna konfiguracja z pliku *appSettings. JSON* i *appSettings. { Environment}. JSON* — pliki.
* Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.
* Zmienne środowiskowe.

`CreateDefaultBuilder`dodaje dostawcę konfiguracji wiersza polecenia Last. Argumenty wiersza polecenia przekazane w czasie wykonywania zastępują konfigurację ustawioną przez innych dostawców.

`CreateDefaultBuilder`działa, gdy host jest skonstruowany. W związku z tym konfiguracja wiersza polecenia aktywowana przez `CreateDefaultBuilder` program może mieć wpływ na sposób konfigurowania hosta.

W przypadku aplikacji opartych na ASP.NET Core szablonach program `AddCommandLine` został już wywołany przez. `CreateDefaultBuilder` Aby dodać kolejnych dostawców konfiguracji i zachować możliwość przesłonięcia konfiguracji od tych dostawców za pomocą argumentów wiersza polecenia, wywołaj dodatkowych dostawców aplikacji w `ConfigureAppConfiguration` i Wywołaj `AddCommandLine` jako ostatni.

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

**Przykład**

Przykładowa aplikacja korzysta z statycznej wygodnej metody `CreateDefaultBuilder` tworzenia hosta, który obejmuje <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>wywołanie.

1. Otwórz wiersz polecenia w katalogu projektu.
1. Podaj do `dotnet run` polecenia argument wiersza polecenia, `dotnet run CommandLineKey=CommandLineValue`.
1. Po uruchomieniu aplikacji otwórz w przeglądarce aplikację w lokalizacji `http://localhost:5000`.
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

Mapowania przełączników Zezwalaj na logikę zamiany nazwy klucza. W przypadku ręcznego kompilowania konfiguracji za <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>pomocą programu można udostępnić <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> do metody słownik przemieszczeń przełączeń.

Gdy jest używany słownik mapowania przełączników, słownik jest sprawdzany dla klucza, który pasuje do klucza dostarczonego przez argument wiersza polecenia. Jeśli klucz wiersza polecenia zostanie znaleziony w słowniku, wartość słownika (wymiana klucza) zostanie przeniesiona z powrotem, aby ustawić parę klucz-wartość w konfiguracji aplikacji. Mapowanie przełącznika jest wymagane dla każdego klucza wiersza polecenia poprzedzonego pojedynczą kreską (`-`).

Przełącz reguły klucza słownika mapowania:

* Przełączniki muszą zaczynać się kreską`-`() lub podwójną kreską (`--`).
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

W przypadku aplikacji korzystających z mapowań przełączników wywołanie `CreateDefaultBuilder` nie powinno przekazywać argumentów. Wywołanie metody nie obejmuje zamapowanych przełączników i nie ma sposobu przekazywania słownika mapowania przełącznika do `CreateDefaultBuilder`. `CreateDefaultBuilder` `AddCommandLine` Rozwiązanie nie przekazuje argumentów do `CreateDefaultBuilder` , ale zamiast tego `ConfigurationBuilder` zezwala metodzie metody `AddCommandLine` na przetwarzanie zarówno argumentów, jak i słownika mapowania przełącznika.

Po utworzeniu słownika mapowań przełączników zawiera dane przedstawione w poniższej tabeli.

| Key       | Wartość             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

Jeśli klucze mapowane przez przełącznik są używane podczas uruchamiania aplikacji, konfiguracja otrzymuje wartość konfiguracji klucza dostarczonego przez słownik:

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

Po uruchomieniu poprzedniego polecenia Konfiguracja zawiera wartości pokazane w poniższej tabeli.

| Key               | Wartość    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a>Dostawca konfiguracji zmiennych środowiskowych

<xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> Ładowanie konfiguracji ze zmiennej środowiskowej par klucz-wartość w czasie wykonywania.

Aby uaktywnić konfigurację zmiennych środowiskowych, <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> Wywołaj metodę rozszerzenia w <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>wystąpieniu.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

[Azure App Service](https://azure.microsoft.com/services/app-service/) pozwala ustawić zmienne środowiskowe w witrynie Azure Portal, które mogą przesłonić konfigurację aplikacji przy użyciu dostawcy konfiguracji zmiennych środowiskowych. Aby uzyskać więcej informacji, [Zobacz Azure Apps: Przesłoń konfigurację aplikacji przy użyciu witryny](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)Azure Portal.

::: moniker range=">= aspnetcore-3.0"

`AddEnvironmentVariables`służy do ładowania zmiennych środowiskowych, które są `DOTNET_` poprzedzone [konfiguracją hosta](#host-versus-app-configuration) , gdy nowy Konstruktor hosta zostanie zainicjowany z [hostem ogólnym](xref:fundamentals/host/generic-host) i `CreateDefaultBuilder` jest wywoływany. Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .

::: moniker-end

::: moniker range="< aspnetcore-3.0"

`AddEnvironmentVariables`służy do ładowania zmiennych środowiskowych, które są `ASPNETCORE_` poprzedzone [konfiguracją hosta](#host-versus-app-configuration) , gdy nowy Konstruktor hosta zostanie zainicjowany przy użyciu [hosta sieci Web](xref:fundamentals/host/web-host) i `CreateDefaultBuilder` jest wywoływany. Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .

::: moniker-end

`CreateDefaultBuilder`ładuje również:

* Konfiguracja aplikacji z nieoznaczonych zmiennych środowiskowych przez `AddEnvironmentVariables` wywołanie bez prefiksu.
* Opcjonalna konfiguracja z pliku *appSettings. JSON* i *appSettings. { Environment}. JSON* — pliki.
* Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.
* Argumenty wiersza polecenia.

Dostawca konfiguracji zmiennych środowiskowych jest wywoływany po ustanowieniu konfiguracji z poziomu kluczy tajnych użytkownika i plików *AppSettings* . Wywołanie dostawcy w tym miejscu pozwala odczytywać zmienne środowiskowe w czasie wykonywania w celu przesłania konfiguracji ustawionych przez klucze tajne użytkownika i pliki *AppSettings* .

Jeśli musisz podać konfigurację aplikacji na podstawie dodatkowych zmiennych środowiskowych, wywołaj dodatkowych dostawców aplikacji w `ConfigureAppConfiguration` i Wywołaj `AddEnvironmentVariables` z prefiksem.

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call additional providers here as needed.
    // Call AddEnvironmentVariables last if you need to allow
    // environment variables to override values from other 
    // providers.
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
}
```

**Przykład**

Przykładowa aplikacja korzysta z statycznej wygodnej metody `CreateDefaultBuilder` tworzenia hosta, który obejmuje `AddEnvironmentVariables`wywołanie.

1. Uruchom przykładową aplikację. Otwórz w przeglądarce aplikację pod adresem `http://localhost:5000`.
1. Zwróć uwagę, że dane wyjściowe zawierają parę klucz-wartość dla zmiennej `ENVIRONMENT`środowiskowej. Wartość odzwierciedla środowisko, w którym jest uruchomiona aplikacja, zazwyczaj `Development` w przypadku uruchamiania lokalnego.

Aby zachować listę zmiennych środowiskowych renderowanych przez aplikację, aplikacja filtruje zmienne środowiskowe. Zapoznaj się z plikiem przykładowej *strony aplikacji/index. cshtml. cs* .

Jeśli chcesz uwidocznić wszystkie zmienne środowiskowe dostępne dla aplikacji, Zmień wartość `FilteredConfiguration` na *stronie/index. cshtml. cs* w następujący sposób:

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a>Prefiksy

Zmienne środowiskowe ładowane do konfiguracji aplikacji są filtrowane w przypadku podania prefiksu do `AddEnvironmentVariables` metody. Na przykład aby filtrować zmienne środowiskowe na prefiksie `CUSTOM_`, podaj prefiks dla dostawcy konfiguracji:

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

Prefiks jest usuwany podczas tworzenia par klucz-wartość konfiguracji.

Podczas tworzenia konstruktora hostów Konfiguracja hosta jest zapewniana przez zmienne środowiskowe. Aby uzyskać więcej informacji na temat prefiksu używanego dla tych zmiennych środowiskowych, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .

**Prefiksy parametrów połączenia**

Interfejs API konfiguracji ma specjalne reguły przetwarzania dla czterech zmiennych środowiskowych parametrów połączenia związanych z konfigurowaniem parametrów połączenia platformy Azure dla środowiska aplikacji. Zmienne środowiskowe z prefiksami podanymi w tabeli są ładowane do aplikacji, jeśli nie podano `AddEnvironmentVariables`prefiksu.

| Prefiks parametrów połączenia | Dostawca |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | Dostawca niestandardowy |
| `MYSQLCONNSTR_` | [MySQL](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [SQL Server](https://www.microsoft.com/sql-server/) |

Gdy zmienna środowiskowa zostanie odnaleziona i załadowana do konfiguracji z dowolnymi z czterech prefiksów pokazanych w tabeli:

* Klucz konfiguracji jest tworzony przez usunięcie prefiksu zmiennej środowiskowej i dodanie sekcji klucza konfiguracji (`ConnectionStrings`).
* Zostanie utworzona nowa para klucz-wartość konfiguracji, która reprezentuje dostawcę połączenia bazy danych (z wyjątkiem `CUSTOMCONNSTR_`tego, który nie ma określonego dostawcy).

| Klucz zmiennej środowiskowej | Przekonwertowany klucz konfiguracji | Wpis konfiguracji dostawcy                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | Wpis konfiguracji nie został utworzony.                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | Klucz: `ConnectionStrings:<KEY>_ProviderName`:<br>Wartość:`MySql.Data.MySqlClient` |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | Klucz: `ConnectionStrings:<KEY>_ProviderName`:<br>Wartość:`System.Data.SqlClient`  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | Klucz: `ConnectionStrings:<KEY>_ProviderName`:<br>Wartość:`System.Data.SqlClient`  |

## <a name="file-configuration-provider"></a>Dostawca konfiguracji plików

<xref:Microsoft.Extensions.Configuration.FileConfigurationProvider>jest klasą bazową do ładowania konfiguracji z systemu plików. Następujący dostawcy konfiguracji są przydzielone do określonych typów plików:

* [Dostawca konfiguracji pliku INI](#ini-configuration-provider)
* [Dostawca konfiguracji JSON](#json-configuration-provider)
* [Dostawca konfiguracji XML](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a>Dostawca konfiguracji pliku INI

<xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> Ładowanie konfiguracji z par klucz-wartość pliku ini w czasie wykonywania.

Aby uaktywnić konfigurację pliku ini, wywołaj <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> metodę rozszerzenia w <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>wystąpieniu.

Dwukropek może służyć jako ogranicznik sekcji w konfiguracji pliku INI.

Przeciążania Zezwalaj na określanie:

* Czy plik jest opcjonalny.
* Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskiwania dostępu do pliku.

Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.SetBasePath(Directory.GetCurrentDirectory());
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

Ścieżka bazowa jest ustawiana za <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>pomocą.

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

<xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> Ładowanie konfiguracji z par klucz-wartość pliku JSON podczas środowiska uruchomieniowego.

Aby uaktywnić konfigurację pliku JSON, wywołaj <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> metodę rozszerzenia w <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>wystąpieniu.

Przeciążania Zezwalaj na określanie:

* Czy plik jest opcjonalny.
* Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskiwania dostępu do pliku.

`AddJsonFile`jest automatycznie wywoływana dwukrotnie po zainicjowaniu nowego konstruktora hostów za `CreateDefaultBuilder`pomocą programu. Metoda jest wywoływana w celu załadowania konfiguracji z:

* *appSettings. JSON* &ndash; ten plik jest odczytywany jako pierwszy. Wersja środowiska pliku może przesłonić wartości dostarczone przez plik *appSettings. JSON* .
* *appSettings. {Environment}. JSON* &ndash; wersja środowiska pliku jest ładowana na podstawie [IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).

Aby uzyskać więcej informacji, zobacz sekcję [Konfiguracja domyślna](#default-configuration) .

`CreateDefaultBuilder`ładuje również:

* Zmienne środowiskowe.
* Wpisy [tajne użytkownika (Secret Manager)](xref:security/app-secrets) w środowisku deweloperskim.
* Argumenty wiersza polecenia.

Dostawca konfiguracji JSON został ustanowiony jako pierwszy. W związku z tym klucze tajne użytkownika, zmienne środowiskowe i argumenty wiersza polecenia przesłaniają konfigurację ustawioną przez pliki *AppSettings* .

Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji dla plików innych niż *appSettings. JSON* i *appSettings. { Environment}. JSON*:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.SetBasePath(Directory.GetCurrentDirectory());
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

Ścieżka bazowa jest ustawiana za <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>pomocą.

**Przykład**

Przykładowa aplikacja korzysta z statycznej wygodnej metody `CreateDefaultBuilder` tworzenia hosta, który obejmuje dwa wywołania programu. `AddJsonFile` Konfiguracja jest ładowana z pliku *appSettings. JSON* i *appSettings. { Environment}. JSON*.

1. Uruchom przykładową aplikację. Otwórz w przeglądarce aplikację pod adresem `http://localhost:5000`.
1. Zwróć uwagę, że dane wyjściowe zawierają pary klucz-wartość dla konfiguracji pokazanej w tabeli w zależności od środowiska. Klucze konfiguracji rejestrowania używają dwukropka`:`() jako separatora hierarchicznego.

| Key                        | Wartość programistyczna | Wartość produkcyjna |
| -------------------------- | :---------------: | :--------------: |
| Rejestrowanie: LogLevel: system    | Informacje       | Informacje      |
| Rejestrowanie: LogLevel: Microsoft | Informacje       | Informacje      |
| Rejestrowanie: wartość domyślna: LogLevel   | Debugowanie             | Błąd            |
| AllowedHosts               | *                 | *                |

### <a name="xml-configuration-provider"></a>Dostawca konfiguracji XML

<xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> Ładowanie konfiguracji z par klucz-wartość pliku XML w czasie wykonywania.

Aby uaktywnić konfigurację pliku XML, wywołaj <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> metodę rozszerzenia w <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>wystąpieniu.

Przeciążania Zezwalaj na określanie:

* Czy plik jest opcjonalny.
* Czy konfiguracja zostanie ponownie załadowana w przypadku zmiany pliku.
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> Używane do uzyskiwania dostępu do pliku.

Węzeł główny pliku konfiguracji jest ignorowany, gdy są tworzone pary klucz-wartość konfiguracji. Nie określaj definicji typu dokumentu (DTD) ani przestrzeni nazw w pliku.

Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.SetBasePath(Directory.GetCurrentDirectory());
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

Ścieżka bazowa jest ustawiana za <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>pomocą.

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

Powtarzające się elementy, które używają tej samej nazwy elementu `name` , działają, jeśli atrybut jest używany do odróżnienia elementów:

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

<xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> Używa plików katalogu jako par klucz konfiguracji i wartość. Kluczem jest nazwa pliku. Wartość zawiera zawartość pliku. Dostawca konfiguracji klucza dla plików jest używany w scenariuszach hostingu platformy Docker.

Aby uaktywnić konfigurację klucza dla plików, wywołaj <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> metodę rozszerzenia w <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>wystąpieniu. `directoryPath` Do plików musi być ścieżką bezwzględną.

Przeciążania Zezwalaj na określanie:

* `Action<KeyPerFileConfigurationSource>` Delegat, który konfiguruje źródło.
* Określa, czy katalog jest opcjonalny, i ścieżkę do katalogu.

Podwójny znak podkreślenia (`__`) jest używany jako ogranicznik klucza konfiguracji w nazwach plików. Na przykład nazwa `Logging__LogLevel__System` pliku generuje klucz `Logging:LogLevel:System`konfiguracji.

Wywołaj `ConfigureAppConfiguration` podczas kompilowania hosta, aby określić konfigurację aplikacji:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.SetBasePath(Directory.GetCurrentDirectory());
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

Ścieżka bazowa jest ustawiana za <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>pomocą.

## <a name="memory-configuration-provider"></a>Dostawca konfiguracji pamięci

<xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> Używa kolekcji w pamięci jako par klucz konfiguracji-wartość.

Aby uaktywnić konfigurację kolekcji w pamięci, wywołaj <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> metodę rozszerzenia w <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>wystąpieniu.

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

Słownik jest używany z wywołaniem `AddInMemoryCollection` do zapewnienia konfiguracji:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a>GetValue

[ConfigurationBinder. GetValue\<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) wyodrębnia wartość z konfiguracji z określonym kluczem i konwertuje ją na określony typ. Przeciążenie zezwala na podanie wartości domyślnej, jeśli nie znaleziono klucza.

Poniższy przykład:

* Wyodrębnia wartość ciągu z konfiguracji przy użyciu klucza `NumberKey`. Jeśli `NumberKey` nie znaleziono w kluczach konfiguracji, `99` zostanie użyta wartość domyślna.
* Typ wartości `int`.
* Przechowuje wartość we `NumberConfig` właściwości do użycia na stronie.

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

Aby zwrócić element <xref:Microsoft.Extensions.Configuration.IConfigurationSection> zawierający tylko pary klucz-wartość w `section1`, wywołaniu `GetSection` i podać nazwę sekcji:

```csharp
var configSection = _config.GetSection("section1");
```

`configSection` Nie ma wartości, tylko klucza i ścieżki.

Podobnie Aby uzyskać wartości kluczy w `section2:subsection0`, wywołaj `GetSection` i podaj ścieżkę do sekcji:

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

`GetSection`nigdy nie `null`zwraca. Jeśli nie znaleziono pasującej sekcji, zwracany jest `IConfigurationSection` pusty.

Gdy `GetSection` zwraca pasującą sekcję, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> nie jest wypełnione. A i są zwracane, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> gdy istnieje sekcja. <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key>

### <a name="getchildren"></a>GetChildren

Wywołanie [iConfiguration. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) w `section2` programie uzyskuje element `IEnumerable<IConfigurationSection>` obejmujący:

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

Dane przykładowe są `false` spowodowane tym `sectionExists` , że w danych konfiguracji `section2:subsection2` nie ma sekcji.

## <a name="bind-to-a-class"></a>Powiąż z klasą

Konfigurację można powiązać z klasami, które reprezentują grupy powiązanych ustawień przy użyciu *wzorca opcji*. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.

Wartości konfiguracji są zwracane jako ciągi, ale wywołanie <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> umożliwia konstruowanie obiektów [poco](https://wikipedia.org/wiki/Plain_Old_CLR_Object) .

Przykładowa aplikacja zawiera `Starship` model (*modele/Starship. cs*):

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

Sekcja pliku *Starship. JSON* tworzy konfigurację, gdy aplikacja Przykładowa używa dostawcy konfiguracji JSON do załadowania konfiguracji: `starship`

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

Tworzone są następujące pary klucz-wartość konfiguracji:

| Key                   | Wartość                                             |
| --------------------- | ------------------------------------------------- |
| Starship: Nazwa         | USS Enterprise                                    |
| Starship: Rejestr     | NCC-1701                                          |
| Starship: Klasa        | Skład                                      |
| Starship: Długość       | 304,8                                             |
| Starship: prowizja | False                                             |
| handlowych             | Najważniejsze obrazy Corp. https://www.paramount.com |

Przykładowa aplikacja wywołuje `GetSection` `starship` klucz. Pary `starship` klucz-wartość są odizolowane. Metoda jest wywoływana w podsekcji przekazującej w wystąpieniu `Starship` klasy. `Bind` Po powiązaniu wartości wystąpień wystąpienie jest przypisywane do właściwości w celu renderowania:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a>Powiąż z grafem obiektów

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*>jest w stanie powiązać cały Graf obiektów POCO.

Przykład zawiera `TvShow` model, którego obiekt zawiera `Metadata` obiekty i `Actors` klasy (*modele/TvShow. cs*):

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

Przykładowa aplikacja zawiera plik *tvshow. XML* zawierający dane konfiguracji:

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

Konfiguracja jest powiązana z wykresem całego `TvShow` obiektu `Bind` za pomocą metody. Powiązane wystąpienie jest przypisane do właściwości w celu renderowania:

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

[ConfigurationBinder. get\<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) tworzy powiązania i zwraca określony typ. `Get<T>`jest wygodniejszy niż używanie `Bind`. Poniższy kod pokazuje, jak używać `Get<T>` w poprzednim przykładzie, co umożliwia bezpośrednie przypisanie wystąpienia powiązanego do właściwości używanej do renderowania:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a>Powiąż tablicę z klasą

*Przykładowa aplikacja pokazuje Koncepcje opisane w tej sekcji.*

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Obsługuje tablice powiązań z obiektami przy użyciu indeksów tablicowych w kluczach konfiguracji. Każdy format tablicy, który ujawnia segment klucza numerycznego`:0:`( `:1:`, &hellip; , `:{n}:`) jest zdolny do powiązania tablicy z tablicą klas poco.

> [!NOTE]
> Powiązanie jest dostarczane według Konwencji. Niestandardowi dostawcy konfiguracji nie muszą implementować powiązania tablicy.

**Przetwarzanie tablicy w pamięci**

Należy wziąć pod uwagę klucze konfiguracji i wartości podane w poniższej tabeli.

| Key             | Wartość  |
| :-------------: | :----: |
| Tablica: wpisy: 0 | value0 |
| Tablica: wpisy: 1 | sekwencj |
| Tablica: wpisy: 2 | wartość2 |
| Tablica: wpisy: 4 | value4 |
| Tablica: wpisy: 5 | value5 |

Te klucze i wartości są ładowane w przykładowej aplikacji przy użyciu dostawcy konfiguracji pamięci:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,23)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,23)]

::: moniker-end

Tablica pomija wartość dla indeksu &num;3. Segregator konfiguracji nie może powiązać wartości null ani tworzyć wpisów o wartości null w obiektach powiązanych, co oznacza, że w chwili pojawi się wynik powiązania tej tablicy z obiektem.

W przykładowej aplikacji jest dostępna Klasa POCO, która przechowuje powiązane dane konfiguracji:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

Dane konfiguracji są powiązane z obiektem:

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

[ConfigurationBinder. get\<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) można również użyć składni, która daje w wyniku bardziej zwarty kod:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

Obiekt powiązany, wystąpienie elementu `ArrayExample`, otrzymuje dane tablicy z konfiguracji.

| `ArrayExample.Entries`Indeks | `ArrayExample.Entries`Wartościami |
| :--------------------------: | :--------------------------: |
| 0                            | value0                       |
| 1                            | sekwencj                       |
| 2                            | wartość2                       |
| 3                            | value4                       |
| 4                            | value5                       |

Indeks &num;3 w obiekcie powiązanym przechowuje dane konfiguracji `array:4` dla `value4`klucza konfiguracji i jego wartość. Gdy dane konfiguracji zawierające tablicę są powiązane, indeksy tablic w kluczach konfiguracji są używane tylko do iteracji danych konfiguracji podczas tworzenia obiektu. Wartości null nie można zachować w danych konfiguracyjnych, a wpis o wartości null nie jest tworzony w obiekcie powiązanym, gdy tablica w kluczach konfiguracji pomija jeden lub więcej indeksów.

Brakujący element konfiguracji dla indeksu &num;3 można podać przed powiązaniem `ArrayExample` z wystąpieniem przez dowolnego dostawcę konfiguracji, który generuje poprawną parę klucz-wartość w konfiguracji. Jeśli przykład zawiera dodatkowego dostawcę konfiguracji JSON z brakującą parą klucz-wartość, `ArrayExample.Entries` dopasowuje pełną tablicę konfiguracyjną:

*missing_value.json*:

```json
{
  "array:entries:3": "value3"
}
```

W `ConfigureAppConfiguration`programie:

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

Para klucz-wartość pokazana w tabeli jest ładowana do konfiguracji.

| Key             | Wartość  |
| :-------------: | :----: |
| Tablica: wpisy: 3 | Wartość3 |

Jeśli wystąpienie &num; `ArrayExample.Entries` klasy jest powiązane, gdy dostawca konfiguracji JSON zawiera wpis dla indeksu 3, tablica zawiera wartość. `ArrayExample`

| `ArrayExample.Entries`Indeks | `ArrayExample.Entries`Wartościami |
| :--------------------------: | :--------------------------: |
| 0                            | value0                       |
| 1                            | sekwencj                       |
| 2                            | wartość2                       |
| 3                            | Wartość3                       |
| 4                            | value4                       |
| 5                            | value5                       |

**Przetwarzanie tablicy JSON**

Jeśli plik JSON zawiera tablicę, klucze konfiguracji są tworzone dla elementów tablicy z indeksem sekcji o wartości zero. W poniższym pliku `subsection` konfiguracji jest tablicą:

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

Dostawca konfiguracji JSON odczytuje dane konfiguracji do następujących par klucz-wartość:

| Key                     | Wartość  |
| ----------------------- | :----: |
| json_array: klucz          | wartośća |
| json_array:subsection:0 | Wartośćb |
| json_array:subsection:1 | valueC |
| json_array:subsection:2 | Znajdując |

W przykładowej aplikacji jest dostępna następująca Klasa POCO z powiązaniem par klucz-wartość konfiguracji:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

Po powiązaniu `JsonArrayExample.Key` utrzymuje wartość `valueA`. Wartości podsekcji są przechowywane we właściwości `Subsection`tablicy poco.

| `JsonArrayExample.Subsection`Indeks | `JsonArrayExample.Subsection`Wartościami |
| :---------------------------------: | :---------------------------------: |
| 0                                   | Wartośćb                              |
| 1                                   | valueC                              |
| 2                                   | Znajdując                              |

## <a name="custom-configuration-provider"></a>Niestandardowy dostawca konfiguracji

Przykładowa aplikacja pokazuje, jak utworzyć podstawowego dostawcę konfiguracji, który odczytuje pary klucz-wartość konfiguracji z bazy danych przy użyciu [Entity Framework (EF)](/ef/core/).

Dostawca ma następującą charakterystykę:

* Baza danych EF w pamięci jest używana w celach demonstracyjnych. Aby użyć bazy danych, która wymaga parametrów połączenia, zaimplementuj dodatkową `ConfigurationBuilder` wartość w celu dostarczenia parametrów połączenia od innego dostawcy konfiguracji.
* Dostawca odczytuje tabelę bazy danych w konfiguracji podczas uruchamiania. Dostawca nie wykonuje zapytania do bazy danych w oparciu o klucz.
* Ponowne załadowanie nie zostało zaimplementowane, więc aktualizacja bazy danych po uruchomieniu aplikacji nie ma wpływu na konfigurację aplikacji.

`EFConfigurationValue` Zdefiniuj jednostkę do przechowywania wartości konfiguracji w bazie danych.

::: moniker range=">= aspnetcore-3.0"

*Modele/EFConfigurationValue. cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

`EFConfigurationContext` Dodaj do magazynu i uzyskaj dostęp do skonfigurowanych wartości.

*EFConfigurationProvider/EFConfigurationContext. cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

Utwórz klasę implementującą <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.

*EFConfigurationProvider/EFConfigurationSource. cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

Utwórz niestandardowego dostawcę konfiguracji, dziedziczących od <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>. Dostawca konfiguracji inicjuje bazę danych, gdy jest pusta.

*EFConfigurationProvider/EFConfigurationProvider. cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

Metoda rozszerzająca zezwala na Dodawanie źródła konfiguracji `ConfigurationBuilder`do. `AddEFConfiguration`

*Rozszerzenia/EntityFrameworkExtensions. cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

Poniższy kod pokazuje, jak używać niestandardowych `EFConfigurationProvider` w *program.cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=30-31)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

*Modele/EFConfigurationValue. cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

`EFConfigurationContext` Dodaj do magazynu i uzyskaj dostęp do skonfigurowanych wartości.

*EFConfigurationProvider/EFConfigurationContext. cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

Utwórz klasę implementującą <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.

*EFConfigurationProvider/EFConfigurationSource. cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

Utwórz niestandardowego dostawcę konfiguracji, dziedziczących od <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>. Dostawca konfiguracji inicjuje bazę danych, gdy jest pusta.

*EFConfigurationProvider/EFConfigurationProvider. cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

Metoda rozszerzająca zezwala na Dodawanie źródła konfiguracji `ConfigurationBuilder`do. `AddEFConfiguration`

*Rozszerzenia/EntityFrameworkExtensions. cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

Poniższy kod pokazuje, jak używać niestandardowych `EFConfigurationProvider` w *program.cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=30-31)]

::: moniker-end

## <a name="access-configuration-during-startup"></a>Konfiguracja dostępu podczas uruchamiania

Wsuń `IConfiguration` do konstruktora `Startup` , aby uzyskać dostęp do wartości `Startup.ConfigureServices`konfiguracyjnych w. Aby uzyskać dostęp do `Startup.Configure`konfiguracji w programie `IConfiguration` , należy wstrzyknąć bezpośrednio do metody lub użyć wystąpienia z konstruktora:

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

Aby zapoznać się z przykładem uzyskiwania dostępu do konfiguracji przy użyciu [metod uruchamiania, zobacz Uruchamianie aplikacji: Wygodne metody](xref:fundamentals/startup#convenience-methods).

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a>Konfiguracja dostępu na stronie Razor Pages lub widoku MVC

Aby uzyskać dostęp do ustawień konfiguracji na stronie Razor Pages lub widoku MVC, Dodaj [dyrektywę using](xref:mvc/views/razor#using) ([ C# odwołanie: Using](/dotnet/csharp/language-reference/keywords/using-directive)) dla [przestrzeni nazw Microsoft. Extensions. Configuration](xref:Microsoft.Extensions.Configuration) i wsuń <xref:Microsoft.Extensions.Configuration.IConfiguration> ją na stronę lub widokiem.

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

Implementacja umożliwia dodawanie ulepszeń do aplikacji podczas uruchamiania z zewnętrznego zestawu poza `Startup` klasą aplikacji. <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/configuration/options>
