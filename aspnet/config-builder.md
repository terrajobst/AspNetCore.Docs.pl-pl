---
uid: config-builder
title: Konstruktorzy konfiguracji dla platformy ASP.NET
author: rick-anderson
description: Dowiedz się, jak uzyskać dane konfiguracyjne ze źródeł innych niż wartości web.config ze źródeł zewnętrznych.
ms.author: riande
ms.date: 10/29/2018
ms.technology: aspnet
msc.type: content
ms.openlocfilehash: 4dcc62573fad13ec8b37b2c59e884eec7ca80b92
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148840"
---
# <a name="configuration-builders-for-aspnet"></a>Konstruktorzy konfiguracji dla platformy ASP.NET

Przez [Molloy Autor: Stephen](https://github.com/StephenMolloy) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Konstruktorzy konfiguracji mechanizmu nowoczesnych i elastyczne aplikacje platformy ASP.NET można pobrać wartości konfiguracji ze źródeł zewnętrznych.

Konstruktorzy konfiguracji:

* Są dostępne w programie .NET Framework 4.7.1 i nowszych wersjach.
* Zapewniają elastyczny mechanizm do odczytywania wartości konfiguracji.
* Adres niektóre podstawowe wymagania aplikacji, zgodnie z ich przenieść w kontenerze i ukierunkowane środowiska w chmurze.
* Może służyć w celu zwiększenia ochrony danych konfiguracji, rysowania ze źródeł wcześniej niedostępne (na przykład usługi Azure Key Vault i zmiennymi środowiskowymi) w systemu konfiguracji platformy .NET.

## <a name="keyvalue-configuration-builders"></a>Konstruktorzy konfiguracji klucz wartość

Typowy scenariusz, który może zostać obsłużony przez producentów konfiguracji jest zapewnienie mechanizmu zastępczy podstawowego klucza i wartości dla sekcji konfiguracji, które wzorca klucz/wartość. .NET Framework koncepcji ConfigurationBuilders nie jest ograniczona do konkretnej konfiguracji sekcje lub wzorce. Jednak wiele z konstruktorów konfiguracji w `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) działają w ramach wzorzec klucz/wartość.

## <a name="keyvalue-configuration-builders-settings"></a>Ustawienia Konstruktorzy konfiguracji klucz wartość

Następujące ustawienia mają zastosowanie do wszystkich Konstruktorzy konfiguracji klucz wartość w `Microsoft.Configuration.ConfigurationBuilders`.

### <a name="mode"></a>Tryb

Konstruktorzy konfiguracji Użyj zewnętrznego źródła danych klucz wartość do wypełnienia wybrany klucz/wartość elementów konfiguracji systemu. W szczególności `<appSettings/>` i `<connectionStrings/>` sekcje otrzymywać specjalnego traktowania Konstruktorzy konfiguracji. Konstruktorzy pracy trzech trybów:

* `Strict` — Domyślny tryb. W tym trybie konstruktora konfiguracji działa tylko w sekcji znanych klucz/wartość przetwarzających konfiguracji. `Strict` Tryb wylicza każdy klucz w sekcji. Jeśli dopasowany klucz zostanie znaleziony w zewnętrznego źródła:

   * Konstruktorzy konfiguracji Zastąp wartość w sekcji Konfiguracja wynikowej wartości z zewnętrznego źródła.
* `Greedy` — W tym trybie jest ściśle powiązane z `Strict` trybu. Zamiast są ograniczone do kluczy, które już istnieją w oryginalnej konfiguracji:

  * Konstruktorzy konfiguracji umożliwia dodanie wszystkich par klucz wartość z zewnętrznego źródła wynikową sekcję konfiguracji.

* `Expand` — Działa w nieprzetworzonym kodzie XML przed jest analizowany na obiekt sekcji konfiguracji. Jego można traktować jako rozszerzenie tokenów w ciągu. Dowolną część nieprzetworzonego ciągu XML, który pasuje do wzorca `${token}` jest kandydatem do rozszerzenia tokenu. Jeśli nie odpowiadająca wartość znajduje się w zewnętrznego źródła, token nie jest zmieniany. Konstruktorzy w tym trybie nie są ograniczone do `<appSettings/>` i `<connectionStrings/>` sekcje.

Następujące znaczniki z *web.config* umożliwia [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) w `Strict` trybu:

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

Poniższy kod odczytuje `<appSettings/>` i `<connectionStrings/>` pokazano w poprzednim *web.config* pliku:

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

Powyższy kod będzie równa wartości właściwości:

* Wartości w *web.config* plik, jeśli klucze nie są ustawione w zmiennych środowiskowych.
* Wartości środowiska zmiennej, jeśli ustawiona.

Na przykład `ServiceID` będzie zawierać:

* "Identyfikator ServiceID wartość z pliku web.config", jeśli zmienna środowiskowa `ServiceID` nie jest ustawiona.
* Wartość `ServiceID` środowiska zmiennej, jeśli ustawiona.

Na poniższej ilustracji przedstawiono `<appSettings/>` kluczy/wartości z poprzednim *web.config* pliku zestawu w edytorze środowiska:

![Edytor środowiska](config-builder/static/env.png)

Uwaga: Może być konieczne zamknięciu i ponownym uruchomieniu programu Visual Studio, aby zobaczyć zmiany w zmiennych środowiskowych.

### <a name="prefix-handling"></a>Obsługa prefiksu

Prefiksy klucza można uprościć klucze ustawienia, ponieważ:

* Konfiguracja środowiska .NET Framework jest złożona i zagnieżdżone.
* Źródła zewnętrznego klucz/wartość często mają charakter podstawowy i płaską ze względu na charakter. Na przykład zmienne środowiskowe nie są zagnieżdżone.

Użyć dowolnej z następujących metod iniekcję zarówno `<appSettings/>` i `<connectionStrings/>` do konfiguracji za pomocą zmiennych środowiskowych:

* Za pomocą `EnvironmentConfigBuilder` w domyślnym `Strict` tryb i odpowiednie nazwy kluczy w pliku konfiguracji. Poprzedni kod i znaczników zajmuje to podejście. W ten sposób możesz **nie** mają identycznie nazwane kluczy w obu `<appSettings/>` i `<connectionStrings/>`.
* Użyj dwóch `EnvironmentConfigBuilder`s w `Greedy` trybu z prefiksami odrębne i `stripPrefix`. Dzięki tej metodzie aplikacja może odczytywać `<appSettings/>` i `<connectionStrings/>` bez konieczności aktualizowania pliku konfiguracji. Następnej sekcji [stripPrefix](#stripprefix), pokazuje, jak to zrobić.
* Użyj dwóch `EnvironmentConfigBuilder`s w `Greedy` trybu z prefiksami distinct. W przypadku tej metody nie może mieć zduplikowanych nazw kluczy zgodnie z nazwami kluczy muszą różnić się od prefiksu.  Na przykład:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

Ze znacznikami poprzedniego tego samego źródła prostego klucz/wartość może służyć do wypełniania konfiguracji dla dwóch różnych sekcji.

Na poniższej ilustracji przedstawiono `<appSettings/>` i `<connectionStrings/>` kluczy/wartości z poprzednim *web.config* pliku zestawu w edytorze środowiska:

![Edytor środowiska](config-builder/static/prefix.png)

Poniższy kod odczytuje `<appSettings/>` i `<connectionStrings/>` kluczy/wartości zawartych w poprzednim *web.config* pliku:

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

Powyższy kod będzie równa wartości właściwości:

* Wartości w *web.config* plik, jeśli klucze nie są ustawione w zmiennych środowiskowych.
* Wartości środowiska zmiennej, jeśli ustawiona.

Na przykład za pomocą poprzedniej *web.config* pliku, klucz/wartość w poprzednim obrazie edytora środowiska i poprzedniego kodu, następujące wartości są ustawione:

|  Key              | Wartość |
| ----------------- | ------------ |
|     AppSetting_ServiceID           | AppSetting_ServiceID ze zmiennych env|
|    AppSetting_default            | Wartość AppSetting_default env |
|       ConnStr_default         | Val ConnStr_default z env|

### <a name="stripprefix"></a>stripPrefix

`stripPrefix`: atrybut typu wartość logiczna, wartość domyślna to `false`. 

Poprzedni znacznik XML oddziela ustawienia aplikacji z parametrów połączenia, ale wymaga wszystkie klucze w *web.config* pliku, aby użyć określonego prefiksu. Na przykład prefiks `AppSetting` muszą zostać dodane do `ServiceID` klucza ("AppSetting_ServiceID"). Za pomocą `stripPrefix`, prefiks nie jest używany w *web.config* pliku. Prefiks jest wymagany w źródle konstruktora konfiguracji (na przykład w środowisku.) Przewidujemy, że większość programistów użyje `stripPrefix`.

Aplikacje zazwyczaj paska wyłączone prefiks. Następujące *web.config* paski prefiks:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

W poprzednim *web.config* pliku `default` klucz znajduje się w obu `<appSettings/>` i `<connectionStrings/>`.

Na poniższej ilustracji przedstawiono `<appSettings/>` i `<connectionStrings/>` kluczy/wartości z poprzednim *web.config* pliku zestawu w edytorze środowiska:

![Edytor środowiska](config-builder/static/prefix.png)

Poniższy kod odczytuje `<appSettings/>` i `<connectionStrings/>` kluczy/wartości zawartych w poprzednim *web.config* pliku:

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

Powyższy kod będzie równa wartości właściwości:

* Wartości w *web.config* plik, jeśli klucze nie są ustawione w zmiennych środowiskowych.
* Wartości środowiska zmiennej, jeśli ustawiona.

Na przykład za pomocą poprzedniej *web.config* pliku, klucz/wartość w poprzednim obrazie edytora środowiska i poprzedniego kodu, następujące wartości są ustawione:

|  Key              | Wartość |
| ----------------- | ------------ |
|     Identyfikator usługi           | AppSetting_ServiceID ze zmiennych env|
|    default            | Wartość AppSetting_default env |
|    default         | Val ConnStr_default z env|

### <a name="tokenpattern"></a>tokenPattern

`tokenPattern`: Ciąg, wartość domyślna to `@"\$\{(\w+)\}"`

`Expand` Zachowanie Konstruktorzy wyszukuje nieprzetworzonym kodzie XML dla tokenów, które mają postać `${token}`. Wyszukiwanie jest przeprowadzane za pomocą wyrażeń regularnych domyślne `@"\$\{(\w+)\}"`. Zestaw znaków, który odpowiada `\w` jest ściślejsze niż XML i wielu źródeł w konfiguracji jest dozwolone. Użyj `tokenPattern` po więcej znaków niż `@"\$\{(\w+)\}"` wymagane są nazwa tokenu.

`tokenPattern`: Ciąg:

* Umożliwia deweloperom Zmienianie wyrażenie regularne, używany do dopasowywania tokenu.
* Aby upewnić się, czy jest poprawnie sformułowanym nieszkodliwe wyrażenie regularne jest wykonywana żadna Weryfikacja.
* Musi zawierać grupę przechwytywania. Całe wyrażenie regularne musi odpowiadać całego tokenu. Pierwszy przechwytywania musi być nazwa tokenu do przeszukania źródło konfiguracji.

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a>Konstruktorzy konfiguracji w Microsoft.Configuration.ConfigurationBuilders

### <a name="environmentconfigbuilder"></a>EnvironmentConfigBuilder

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):

* To najprostsza z konstruktorów konfiguracji.
* Odczytuje wartości ze środowiska.
* Nie ma żadnych dodatkowych opcji konfiguracji.
* `name` Wartość atrybutu jest określana.

**Uwaga:** w środowisku Windows container zmienne ustawione w czasie wykonywania są wprowadzane tylko w środowisku procesu punktu wejścia. Aplikacje działają jako usługę lub proces bez punktu wejścia nie wybierze tych zmiennych, chyba że w przeciwnym razie są one wprowadzane za pośrednictwem mechanizmu w kontenerze. Dla [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)— na podstawie kontenerów, bieżąca wersja [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) obsługuje w *DefaultAppPool* tylko. Inne odmiany kontenera z systemem Windows może być konieczne opracowanie własnych zasad iniekcji dla procesów bez punktu wejścia.

### <a name="usersecretsconfigbuilder"></a>UserSecretsConfigBuilder

> [!WARNING]
> Nigdy nie przechowywanie haseł, parametrów połączenia poufne lub innych danych poufnych w kodzie źródłowym. Wpisy tajne w środowisku produkcyjnym nie powinny być używane do projektowania lub testowania.

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

Ten konstruktor konfiguracji udostępnia funkcję podobne do [platformy ASP.NET Core klucz tajny Menedżera](/aspnet/core/security/app-secrets).

[UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) mogą być używane w projektach .NET Framework, ale musi zostać określony plik wpisów tajnych. Alternatywnie można zdefiniować `UserSecretsId` właściwość w projekcie plików, a następnie utwórz plik raw wpisów tajnych w bieżącej lokalizacji do odczytu. Aby zachować zależności zewnętrznych spoza projektu, plik wpisów tajnych jest formacie XML. Formatowanie, XML jest szczegółowo opisuje implementacja i format nie powinna być używana na. Jeśli zachodzi potrzeba udostępniania *secrets.json* plików w projektach .NET Core, należy rozważyć użycie [SimpleJsonConfigBuilder](#simplejsonconfig). `SimpleJsonConfigBuilder` Formatowania dla platformy .NET Core należy również uwzględnić szczegółowo opisuje implementacja może ulec zmianie.

Konfiguracja atrybuty dla `UserSecretsConfigBuilder`:

* `userSecretsId` — Jest preferowana metoda identyfikowania plik wpisów tajnych XML. Działa podobnie do platformy .NET Core, która używa `UserSecretsId` właściwość do przechowywania tego identyfikatora projektu. Ciąg musi być unikatowy, nie musi być identyfikatorem GUID. Z tym atrybutem `UserSecretsConfigBuilder` przeszukania znaną lokalizacją lokalną (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) dla pliku wpisów tajnych należących do tego identyfikatora.
* `userSecretsFile` -Opcjonalny atrybut, określając pliku zawierającego wpisy tajne. `~` Znak może być użyty na początku, aby odwoływać się do katalogu głównego aplikacji. Albo tego atrybutu lub `userSecretsId` atrybut jest wymagany. Jeśli są określone oba `userSecretsFile` ma pierwszeństwo.
* `optional`: wartość logiczna, wartość domyślna `true` — uniemożliwia wyjątek, jeśli nie można odnaleźć pliku wpisów tajnych. 
* `name` Wartość atrybutu jest określana.

Plik wpisów tajnych ma następujący format:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a>AzureKeyVaultConfigBuilder

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) odczytuje wartości przechowywane w [usługi Azure Key Vault](/azure/key-vault/key-vault-whatis).

`vaultName` jest wymagana (nazwę magazynu) lub identyfikator URI do magazynu. Inne atrybuty umożliwiają kontrolę, o których Magazyn, aby nawiązać połączenie, ale są wymagane tylko, jeśli aplikacja nie jest uruchomiona w środowisku, które współpracuje z `Microsoft.Azure.Services.AppAuthentication`. Biblioteki uwierzytelniania usługi Azure umożliwia automatyczne pobranie, jeśli jest to możliwe informacji o połączeniu ze środowiska wykonawczego. Możesz zastąpić automatycznie wybierze informacji o połączeniu, podając parametry połączenia.

* `vaultName` -Jeśli wymagane `uri` w nie podano. Określa nazwę magazynu, w ramach subskrypcji platformy Azure, z którego można odczytać pary klucz/wartość.
* `connectionString` -Parametry połączenia mogą być używane przez [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)
* `uri` -Łączy się z innymi dostawcami usługi Key Vault przy użyciu określonego `uri` wartość. Jeśli nie zostanie określony, Azure (`vaultName`) jest dostawcą magazynu.
* `version` — Usługa azure Key Vault zapewnia funkcji przechowywania wersji dla wpisów tajnych. Jeśli `version` określono konstruktora pobiera tylko wpisy tajne dopasowania tej wersji.
* `preloadSecretNames` -Domyślnie querys tego konstruktora **wszystkich** klucza nazwy w usłudze key vault, po jego zainicjowaniu. Aby zapobiec, odczytywanie wszystkich wartości kluczy, ustaw ten atrybut na `false`. Ustawienie tej opcji na `false` odczytuje wpisy tajne jednego naraz. Odczytywanie wpisów tajnych, które można pojedynczo przydatne, jeśli magazyn zezwala na dostęp "Pobierz", ale nie "List" dostęp do. **Uwaga:** przy użyciu `Greedy` trybie `preloadSecretNames` musi być `true` (ustawienie domyślne).

### <a name="keyperfileconfigbuilder"></a>KeyPerFileConfigBuilder

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) jest konstruktor podstawowej konfiguracji, który korzysta z plików w katalogu jako źródła wartości. Nazwa pliku jest klucz, a zawartość jest wartość. Tego konstruktora konfiguracji może być przydatne podczas pracy w środowisku kontenera zorganizowane. Systemy, takie jak Docker Swarm i Kubernetes zapewniają `secrets` do ich kontenerów windows zorganizowane w ten sposób klucza każdego pliku.

Szczegóły atrybutu:

* `directoryPath` — Wymagane. Określa ścieżkę do wyszukania wartości. Docker for Windows klucze tajne są przechowywane w *C:\ProgramData\Docker\secrets* katalogu domyślnie.
* `ignorePrefix` -Pliki, rozpoczynające się od tego prefiksu są wyłączone. Wartość domyślna to "ignorowania".
* `keyDelimiter` — Wartość domyślna to `null`. Jeśli zostanie określony, Konstruktor konfiguracji przechodzi przez wiele poziomów katalogu, tworzenia nazw kluczy, z tym ogranicznikiem. Jeśli ta wartość jest `null`, Konstruktor konfiguracji przeszukuje tylko na najwyższym poziomie w katalogu.
* `optional` — Wartość domyślna to `false`. Określa, czy konstruktora konfiguracji powinna powodować błędy, jeśli katalog źródłowy nie istnieje.

### <a name="simplejsonconfigbuilder"></a>SimpleJsonConfigBuilder

> [!WARNING]
> Nigdy nie przechowywanie haseł, parametrów połączenia poufne lub innych danych poufnych w kodzie źródłowym. Wpisy tajne w środowisku produkcyjnym nie powinny być używane do projektowania lub testowania.

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

Projekty .NET core często używane pliki w formacie JSON dla konfiguracji. [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) konstruktora umożliwia plików .NET Core w formacie JSON ma być używany w programie .NET Framework. Ten konstruktor konfiguracji jest zapewnia podstawowe mapowanie ze źródła prostego klucz/wartość w obszarach określonego klucza i wartości konfiguracji .NET Framework. Jest to Konstruktor konfiguracji **nie** zapewnienia konfiguracji hierarchicznej. Tworzenie kopii pliku JSON jest podobny do słownika nie hierarchiczny obiekt złożony. Można użyć wielopoziomowe, hierarchiczne pliku. Ten dostawca `flatten`s głębi, dodając nazwy właściwości na każdym poziomie przy użyciu `:` jako ogranicznik.

Szczegóły atrybutu:

* `jsonFile` — Wymagane. Określa plik JSON do odczytu. `~` Znak może być użyty na początku, aby odwoływać się do katalogu głównego aplikacji.
* `optional` — Wartość logiczna, wartość domyślna to `true`. Zapobiega zgłaszanie wyjątków, jeśli nie można odnaleźć pliku JSON.
* `jsonMode` - `[Flat|Sectional]`. `Flat` jest ustawieniem domyślnym. Gdy `jsonMode` jest `Flat`, plik JSON jest źródłem jednego prostego klucz/wartość. `EnvironmentConfigBuilder` i `AzureKeyVaultConfigBuilder` są również źródeł jednego prostego klucz/wartość. Gdy `SimpleJsonConfigBuilder` jest skonfigurowana w `Sectional` trybu:

  * Plik JSON jest koncepcyjnie podzielony tylko na najwyższym poziomie na wiele słowników.
  * Każdy słowniki są stosowane tylko do sekcji konfiguracji, który pasuje do nazwy właściwości najwyższego poziomu do nich dołączone. Na przykład:

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a>Implementowanie konstruktora niestandardowego klucza i wartości konfiguracji

Jeśli Konstruktorzy konfiguracji nie odpowiada Twoim potrzebom, możesz napisać niestandardowego. `KeyValueConfigBuilder` Klasy bazowej obsługuje tryby podstawienia i większości problemów prefiks. Implementowanie projektu potrzebne tylko:

* Dziedziczenie z klasy bazowej i wdrożenie podstawowego źródła par klucz/wartość za pośrednictwem `GetValue` i `GetAllValues`:
* Dodaj [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) do projektu.

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

`KeyValueConfigBuilder` Klasy bazowej zapewnia znaczną część pracy i spójne zachowanie różnych kluczy/wartości Konstruktorzy konfiguracji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Repozytorium GitHub Konstruktorzy konfiguracji](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [Usługa Usługa uwierzytelniania usługi Azure Key Vault przy użyciu platformy .NET](/azure/key-vault/service-to-service-authentication#connection-string-support)
