---
title: Wzorzec opcje w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak użyć wzorca opcje do reprezentowania grup powiązanych ustawień w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
uid: fundamentals/configuration/options
ms.openlocfilehash: 6258530beedced9570111478fea630b1556e1a1e
ms.sourcegitcommit: 25150f4398de83132965a89f12d3a030f6cce48d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/25/2018
ms.locfileid: "42927961"
---
# <a name="options-pattern-in-aspnet-core"></a>Wzorzec opcje w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

Wzorzec opcje używa klas do reprezentowania grup powiązane ustawienia. Gdy [ustawienia konfiguracji](xref:fundamentals/configuration/index) są izolowane według scenariusza w osobnych klas, aplikacja działa zgodnie z dwóch zasad ważne inżynierii oprogramowania:

* [Zasady podziału interfejsu (ISP)](http://deviq.com/interface-segregation-principle/): scenariuszy (klasy), które są zależne od ustawień konfiguracji tylko zależą od ustawień konfiguracji, których używają.
* [Separacji](http://deviq.com/separation-of-concerns/): ustawienia dla poszczególnych części aplikacji nie są zależnych lub powiązanych ze sobą.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)) ten artykuł stanowi łatwiejszej do obserwowania z przykładowej aplikacji.

## <a name="prerequisites"></a>Wymagania wstępne

::: moniker range=">= aspnetcore-2.1"

Odwołanie [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) pakietu.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Odwołanie [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) lub Dodaj odwołanie do pakietu [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) pakietu.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Dodaj odwołanie do pakietu [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) pakietu.

::: moniker-end

## <a name="basic-options-configuration"></a>Podstawowe opcje konfiguracji

Opcje podstawowe konfiguracja jest przedstawiona jako przykład &num;1 w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Klasa opcji musi być nieabstrakcyjnej przy użyciu publicznego konstruktora bez parametrów. Następujące klasy `MyOptions`, ma dwie właściwości `Option1` i `Option2`. Ustawianie wartości domyślnych jest opcjonalne, ale konstruktora klasy w poniższym przykładzie ustawiono wartość domyślną `Option1`. `Option2` ma wartość domyślną, ustawiane przez bezpośrednie inicjowanie właściwości (*Models/MyOptions.cs*):

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

`MyOptions` Klasy zostanie dodany do kontenera usługi za pomocą [Konfiguruj&lt;TOptions&gt;(IServiceCollection, wartości IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) i powiązane z konfiguracji:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

Stronie używa modelu [iniekcji zależności konstruktora](xref:mvc/controllers/dependency-injection) z [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) można uzyskać dostęp do ustawień (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

Przykładowe *appsettings.json* plik Określa wartości dla `option1` i `option2`:

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

Gdy aplikacja jest uruchamiana, modelu strony `OnGet` metoda zwraca ciąg przedstawiający wartości klasy opcji:

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> Korzystając z niestandardowego [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) można załadować konfiguracji opcji z pliku ustawień, upewnij się, że ścieżka podstawowa jest prawidłowo:
>
> ```csharp
> var configBuilder = new ConfigurationBuilder()
>    .SetBasePath(Directory.GetCurrentDirectory())
>    .AddJsonFile("appsettings.json", optional: true);
> var config = configBuilder.Build();
>
> services.Configure<MyOptions>(config);
> ```
>
> Jawne ustawianie ścieżka podstawowa nie jest wymagana podczas ładowania konfiguracji opcji z pliku ustawień za pośrednictwem [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

## <a name="configure-simple-options-with-a-delegate"></a>Skonfiguruj opcje prostego za pomocą delegata

Konfigurowanie opcji prostego z delegatem przedstawiono przykład &num;2 [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Użycie delegowania do ustawiania wartości opcji. Ta aplikacja używa przykładowych `MyOptionsWithDelegateConfig` klasy (*Models/MyOptionsWithDelegateConfig.cs*):

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

W poniższym kodzie sekundy `IConfigureOptions<TOptions>` usługi zostanie dodany do kontenera usług. Używa ona delegata do konfigurowania wiązania za pomocą `MyOptionsWithDelegateConfig`:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

Można dodać wielu dostawców konfiguracji. Dostawcy konfiguracji są dostępne w pakietach NuGet. Są one stosowane w celu zapewnienia ich rejestracji.

Każde wywołanie [Konfiguruj&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) dodaje `IConfigureOptions<TOptions>` usługi service container. W powyższym przykładzie wartości `Option1` i `Option2` określono zarówno w *appsettings.json*, ale wartości `Option1` i `Option2` są zastępowane przez delegata skonfigurowany.

Po włączeniu więcej niż jednej konfiguracji usługi ostatni źródło konfiguracji określone *wins* i ustawia wartość konfiguracji. Gdy aplikacja jest uruchamiana, modelu strony `OnGet` metoda zwraca ciąg przedstawiający wartości klasy opcji:

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>Konfiguracja suboptions

Suboptions konfiguracja jest przedstawiona jako przykład &num;3 w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Aplikacje powinny zostać utworzone opcje klasy, które odnoszą się do danego scenariusza grupy (klasy) w aplikacji. Części aplikacji, które wymagają wartości konfiguracji powinien mieć tylko dostęp do wartości konfiguracji, których używają.

Podczas tworzenia powiązania opcje konfiguracji, każdej właściwości w typie opcji jest powiązany z kluczem konfiguracji formularza `property[:sub-property:]`. Na przykład `MyOptions.Option1` właściwość jest powiązana z klucza `Option1`, które są odczytywane z `option1` właściwość *appsettings.json*.

W poniższym kodzie, a trzeci `IConfigureOptions<TOptions>` usługi zostanie dodany do kontenera usług. Powiąże `MySubOptions` do sekcji `subsection` z *appsettings.json* pliku:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

`GetSection` — Metoda rozszerzenia wymaga [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) pakietu NuGet. Jeśli aplikacja korzysta z [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 lub nowszej), pakiet jest automatycznie dołączane.

Przykładowe *appsettings.json* plik definiuje `subsection` członka za pomocą kluczy `suboption1` i `suboption2`:

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

`MySubOptions` Klasy definiuje właściwości, `SubOption1` i `SubOption2`, aby przechowywać wartości opcji (*Models/MySubOptions.cs*):

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

Model strony `OnGet` metoda zwraca ciąg zawierający wartości opcji (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

Gdy aplikacja jest uruchamiana, `OnGet` metoda zwraca ciąg przedstawiający podrzędnych opcję wartości klasy:

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>Opcje dostarczane przez model widoku lub przy użyciu iniekcji bezpośrednie widoku

Opcje dostarczane przez model widoku lub przy użyciu iniekcji bezpośrednie widoku przedstawiono przykład &num;4 w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Opcje mogą być podawane w modelu widoku lub przez iniekcję `IOptions<TOptions>` bezpośrednio w widoku (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

Do bezpośredniej iniekcji należy wstrzyknąć `IOptions<MyOptions>` z `@inject` dyrektywy:

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

Gdy aplikacja jest uruchamiana, wartości opcje są wyświetlane na renderowanej stronie:

![Opcje wartości opcja1: value1_from_json i opcja2: -1 są ładowane z modelu, jak również iniekcję do widoku.](options/_static/view.png)

::: moniker range=">= aspnetcore-1.1"

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>Załaduj ponownie dane konfiguracyjne z IOptionsSnapshot

Ponowne załadowanie danych konfiguracji z `IOptionsSnapshot` przedstawiono w przykładzie &num;5 w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) obsługuje ponownego ładowania opcji z minimalnym przetwarzania.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Opcje są obliczane raz na każde żądanie, gdy dostęp do pamięci podręcznej i okresu istnienia żądania.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

`IOptionsSnapshot` jest migawką [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) i aktualizacje automatyczne zawsze, gdy monitor wyzwolenie zmienia się zależnie od zmiany źródła danych.

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

W poniższym przykładzie pokazano, jak nowy `IOptionsSnapshot` został utworzony po *appsettings.json* zmiany (*Pages/Index.cshtml.cs*). Wiele żądań do serwera zwracają stałe wartości, dostarczone przez *appsettings.json* pliku do momentu zmiany pliku i ponowne załadowanie konfiguracji.

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

Na poniższej ilustracji przedstawiono początkowe `option1` i `option2` wartości załadowane z *appsettings.json* pliku:

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

Zmień wartości w *appsettings.json* plik `value1_from_json UPDATED` i `200`. Zapisz *appsettings.json* pliku. Odśwież przeglądarkę, aby zobaczyć, że zostaną zaktualizowane wartości opcji:

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="named-options-support-with-iconfigurenamedoptions"></a>Opcje pomocy technicznej, za pomocą IConfigureNamedOptions nazwane

Opcje pomocy technicznej, o nazwie [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) przedstawiono przykład &num;6 w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*O nazwie opcje* pomocy technicznej zezwala aplikacji na rozróżnienie między nazwane opcje konfiguracji. W przykładowej aplikacji o nazwie opcje są uznane za pomocą [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(Akcja IServiceCollection, String,&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure)który z kolei wywołuje metodę rozszerzenia [ConfigureNamedOptions&lt;TOptions&gt;. Konfigurowanie](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) metody:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

Przykładowa aplikacja uzyskuje dostęp do opcji o nazwie za pomocą [IOptionsSnapshot&lt;TOptions&gt;. Pobierz](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

Uruchamianie przykładowej aplikacji, zwracane są nazwane opcje:

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

`named_options_1` wartości są dostarczane z konfiguracji, które są ładowane z *appsettings.json* pliku. `named_options_2` wartości są dostarczane przez:

* `named_options_2` Delegowanie w `ConfigureServices` dla `Option1`.
* Wartością domyślną dla `Option2` dostarczone przez `MyOptions` klasy.

Skonfiguruj wszystkie wystąpienia nazwanego opcje z [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) metody. Poniższy kod służy do konfigurowania `Option1` wszystkie nazwane wystąpienia konfiguracji, za pomocą wspólnej wartości. Dodaj następujący kod ręcznie do `Configure` metody:

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

Uruchamianie przykładowej aplikacji po dodaniu kod generuje następujące wyniki:

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> Wszystkie opcje są nazwane wystąpienia. Istniejące `IConfigureOption` wystąpienia są traktowane jako docelowy `Options.DefaultName` wystąpienia, co jest `string.Empty`. `IConfigureNamedOptions` implementuje również `IConfigureOptions`. Domyślna implementacja klasy [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([źródło odwołania](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) zawiera logikę w celu używania każdego odpowiednio. `null` Nazwane opcja jest używana do Docieraj do wszystkich wystąpień nazwanych, zamiast określonego nazwanego wystąpienia ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) i [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) ta Konwencja).

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="options-validation"></a>Opcje weryfikacji

Opcje weryfikacji umożliwia sprawdzenie poprawności opcji, po skonfigurowaniu opcji. Wywołaj `Validate` przy użyciu metody sprawdzania poprawności, która zwraca `true` Jeśli opcje są prawidłowe i `false` Jeśli nie są prawidłowe:

```csharp
// Registration
services.AddOptions<MyOptions>("optionalOptionsName")
    .Configure(o => { }) // Configure the options
    .Validate(o => YourValidationShouldReturnTrueIfValid(o), 
        "custom error");
        
// Consumption
var monitor = services.BuildServiceProvider()
    .GetService<IOptionsMonitor<MyOptions>>();
  
try
{
    var options = monitor.Get("optionalOptionsName");
} 
catch (OptionsValidationException e) 
{
   // e.OptionsName returns "optionalOptionsName"
   // e.OptionsType returns typeof(MyOptions)
   // e.Failures returns a list of errors, which would contain 
   //     "custom error"
}
```

Poprzedni przykład ustawia wystąpienia nazwanego opcji `optionalOptionsName`. Wystąpienie domyślne opcje to `Options.DefaultName`.

Sprawdzanie poprawności jest uruchamiany, gdy tworzone jest wystąpienie opcji. Wystąpienie opcji jest gwarantowane do przekazania razem pierwszego sprawdzania poprawności, gdy jest on dostępny.

> [!IMPORTANT]
> Opcje sprawdzania poprawności nie je przed nieprzewidzianymi uniemożliwiającą modyfikacje opcje po opcji są wstępnie skonfigurowane i zweryfikowane.

`Validate` Metoda przyjmuje `Func<TOptions, bool>`. Aby w pełni dostosować sprawdzanie poprawności, należy zaimplementować `IValidateOptions<TOptions>`, co umożliwia:

* Sprawdzanie poprawności wielu typów opcji: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`
* Sprawdzanie poprawności, który jest zależny od innego typu opcji: `public DependsOnAnotherOptionValidator(IOptions<AnotherOption> options)`

`IValidateOptions` sprawdza poprawność:

* Określonego nazwanego wystąpienia opcje.
* Wszystkie opcje, kiedy `name` jest `null`.

Zwróć `ValidateOptionsResult` z implementacji interfejsu:

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

Weryfikacja eager (po awarii szybkie przy uruchamianiu) i sprawdzanie poprawności danych na podstawie adnotacji są planowane w przyszłej wersji.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

Ustaw postconfiguration z [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1). Postconfiguration jest uruchamiany po wszystkich [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) występuje konfiguracji:

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) pozwala konfigurować po opcji o nazwie:

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

Użyj [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) po Konfiguracja wszystkich nazwanego wystąpienia konfiguracji:

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

::: moniker-end

## <a name="options-factory-monitoring-and-cache"></a>Opcje fabryki, monitorowanie i pamięci podręcznej

[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) jest używany dla powiadomień podczas `TOptions` wystąpienia zmiany. `IOptionsMonitor` obsługuje opcje ładowany ponownie zmienić powiadomienia, i `IPostConfigureOptions`.

::: moniker range=">= aspnetcore-2.0"

[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) odpowiada za tworzenie nowych opcji wystąpień. Ma on jeden [Utwórz](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) metody. Domyślna implementacja pobiera wszystkie zarejestrowane `IConfigureOptions` i `IPostConfigureOptions` i umożliwia uruchamianie wszystkich konfiguruje się najpierw, a następnie konfiguruje używane po tej operacji. Jego rozróżnia `IConfigureNamedOptions` i `IConfigureOptions` i tylko wywołuje odpowiedniego interfejsu.

[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) jest używany przez `IOptionsMonitor` do pamięci podręcznej `TOptions` wystąpień. `IOptionsMonitorCache` Unieważnia opcje wystąpień w monitorze, tak aby wartość jest ponownie przeliczana ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)). Wartości można ręcznie wprowadzić również z [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd). [Wyczyść](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) metoda jest używana, gdy wszystkie wystąpienia nazwanego, należy odtworzyć na żądanie.

::: moniker-end

## <a name="accessing-options-during-startup"></a>Uzyskiwanie dostępu do opcji podczas uruchamiania

`IOptions` mogą być używane w `Startup.Configure`, ponieważ usługi są kompilowane przed `Configure` metoda jest wykonywana.

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.Value.Option1;
}
```

`IOptions` nie powinny być używane w `Startup.ConfigureServices`. Stan opcje niezgodne mogą występować z powodu zamawiania rejestracji usługi.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/configuration/index>
