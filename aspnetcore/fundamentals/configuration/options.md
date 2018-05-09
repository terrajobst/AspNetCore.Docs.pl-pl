---
title: Wzorzec opcje dla platformy ASP.NET Core
author: guardrex
description: Wykryj sposób użycia wzorca opcje do reprezentowania grupy powiązanych ustawień w aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/options
ms.openlocfilehash: 800ff2039e7cc1fa37315ed55a77711dc9f47504
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="options-pattern-in-aspnet-core"></a>Wzorzec opcje dla platformy ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

Wzorzec opcje używa klas do reprezentowania grupy ustawień. Ustawienia konfiguracji są izolowane przez funkcję w osobnych klas, aplikacja odpowiada dwóch zasad ważne engineering oprogramowania:

* [Zasady podział interfejsu (ISP)](http://deviq.com/interface-segregation-principle/): funkcje (klasy), które są zależne od ustawienia konfiguracji są zależne tylko od ustawienia konfiguracji, które korzystają z.
* [Separacji](http://deviq.com/separation-of-concerns/): ustawienia dla poszczególnych części aplikacji nie są zależne lub sprzężonego ze sobą.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)) w tym artykule jest łatwiej postępuj zgodnie z przykładowej aplikacji.

## <a name="basic-options-configuration"></a>Podstawowe opcje konfiguracji

Podstawowe opcje konfiguracji przedstawiono przykład &num;1 w [Przykładowa aplikacja](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Klasa opcji musi być typem abstrakcyjnym z publicznym konstruktorem bez parametrów. Następujące klasy `MyOptions`, ma dwie właściwości `Option1` i `Option2`. Ustawianie wartości domyślnych jest opcjonalne, ale konstruktora klasy w poniższym przykładzie ustawia domyślną wartość `Option1`. `Option2` ma wartość domyślną, ustawione przez inicjowanie właściwość bezpośrednio (*Models/MyOptions.cs*):

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

`MyOptions` Klasy jest dodawany do kontenera usługi z [Konfiguruj&lt;TOptions&gt;(IServiceCollection, wartości IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) i powiązane z konfiguracji:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

Następujące strony używa modelu [iniekcji zależności konstruktora](xref:fundamentals/dependency-injection#what-is-dependency-injection) z [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) uzyskać dostęp do ustawień (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

Przykładowe *appsettings.json* pliku określa wartości `option1` i `option2`:

[!code-json[](options/sample/appsettings.json)]

Gdy aplikacja jest uruchamiana, modelu strony `OnGet` metoda zwraca ciąg przedstawiający wartości klasy opcji:

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a>Konfiguruj opcje proste delegata

Konfigurowanie opcji prostego z delegata przedstawiono przykład &num;2 w [Przykładowa aplikacja](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Użyj delegata, aby ustawić opcje wartości. Przykładowe zastosowania aplikacji `MyOptionsWithDelegateConfig` klasy (*Models/MyOptionsWithDelegateConfig.cs*):

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

W poniższym kodzie drugiej `IConfigureOptions<TOptions>` usługi jest dodawany do kontenera usług. Używa delegata do konfigurowania wiązania z `MyOptionsWithDelegateConfig`:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

Można dodać wielu dostawców konfiguracji. W pakietach NuGet dostępnych dostawców konfiguracji. Są one stosowane, że są one zarejestrowane.

Każde wywołanie [Konfiguruj&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) dodaje `IConfigureOptions<TOptions>` usługi do kontenera usług. W powyższym przykładzie wartości `Option1` i `Option2` są określone w *appsettings.json*, ale wartości `Option1` i `Option2` zostały zastąpione przez skonfigurowany delegata.

Po włączeniu więcej niż jedna usługa konfiguracji ostatniego źródło konfiguracji określone *wins* i ustawia wartość konfiguracji. Gdy aplikacja jest uruchamiana, modelu strony `OnGet` metoda zwraca ciąg przedstawiający wartości klasy opcji:

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>Konfiguracja suboptions

Konfiguracja suboptions przedstawiono przykład &num;3 w [Przykładowa aplikacja](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Aplikacje, należy utworzyć klasy opcje, które odnoszą się do określonych funkcji grupy (klasy) w aplikacji. Części aplikacji, które wymagają wartości konfiguracji tylko powinien mieć dostęp do wartości konfiguracji, które używają.

Podczas tworzenia wiązania opcje konfiguracji, każdej właściwości w typie opcje jest powiązany z kluczem konfiguracji formularza `property[:sub-property:]`. Na przykład `MyOptions.Option1` właściwość jest powiązana z klucza `Option1`, które są odczytywane z `option1` właściwości w *appsettings.json*.

W poniższym kodzie innej `IConfigureOptions<TOptions>` usługi jest dodawany do kontenera usług. Tworzy ona powiązanie `MySubOptions` do sekcji `subsection` z *appsettings.json* pliku:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

`GetSection` — Metoda rozszerzenia wymaga [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) pakietu NuGet. Jeśli aplikacja korzysta z [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, że pakiet jest automatycznie dołączane.

Przykładowe *appsettings.json* plik definiuje `subsection` elementu członkowskiego z kluczami `suboption1` i `suboption2`:

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

`MySubOptions` Klasa definiuje właściwości `SubOption1` i `SubOption2`, do przechowywania wartości opcji podrzędne (*Models/MySubOptions.cs*):

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

Model strony `OnGet` metoda zwraca ciąg o wartości opcji podrzędne (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

Gdy aplikacja jest uruchamiana, `OnGet` metoda zwraca ciąg przedstawiający opcji podrzędnego klasy wartości:

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>Opcje przekazane przez model widoku lub widok bezpośredniego iniekcji

Opcje przekazane przez model widoku lub widok bezpośredniego iniekcji przedstawiono przykład &num;4 w [Przykładowa aplikacja](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Opcje mogą być dostarczane w modelu widoku lub przez wstrzykiwanie `IOptions<TOptions>` bezpośrednio w widoku (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

Bezpośrednie iniekcji wstrzyknąć `IOptions<MyOptions>` z `@inject` dyrektywy:

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

Po uruchomieniu aplikacji na renderowanej stronie są wyświetlane wartości opcji:

![Opcje wartości opcja 1: value1_from_json i Option2: -1 zostały załadowane z modelu i wstawiania do widoku.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>Załaduj ponownie dane konfiguracyjne z IOptionsSnapshot

Ponowne ładowanie danych konfiguracji z `IOptionsSnapshot` przedstawiono w przykładzie &num;5 w [Przykładowa aplikacja](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*Wymaga platformy ASP.NET Core 1.1 lub nowszej.*

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) obsługuje ponowne ładowanie opcji z minimalnym przetwarzania. W programie ASP.NET Core 1.1 `IOptionsSnapshot` jest migawką [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) i aktualizacje automatyczne zawsze, gdy monitor wyzwala zmiany oparte na Zmiana źródła danych. W programie ASP.NET Core 2.0 lub nowszego oraz opcje są obliczane raz na każde żądanie, gdy dostępne i pamięci podręcznej przez czas ich istnienia żądania.

W poniższym przykładzie pokazano, jak nowy `IOptionsSnapshot` jest tworzony po *appsettings.json* zmiany (*Pages/Index.cshtml.cs*). Wiele żądań do serwera zwracać wartości stałych przez *appsettings.json* pliku do momentu zmiany pliku i ponowne załadowanie konfiguracji.

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

Na poniższej ilustracji przedstawiono początkowej `option1` i `option2` wartości załadowane z *appsettings.json* pliku:

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

Zmień wartości w *appsettings.json* pliku `value1_from_json UPDATED` i `200`. Zapisz *appsettings.json* pliku. Odśwież przeglądarkę, aby zobaczyć, że wartości opcji zostały zaktualizowane:

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>O nazwie Opcje pomocy dotyczącej IConfigureNamedOptions

Opcje obsługi o nazwie [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) przedstawiono przykład &num;6 w [Przykładowa aplikacja](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*Wymaga platformy ASP.NET Core 2.0 lub nowszej.*

*O nazwie opcje* pomocy technicznej umożliwia rozróżnienia nazwanego opcje konfiguracji aplikacji. W przykładowej aplikacji, opcje nazwanego został zadeklarowany z [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Akcja&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure)który z kolei wywołuje metodę rozszerzenie [ConfigureNamedOptions&lt;TOptions&gt;. Skonfiguruj](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) metody:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

Przykładowa aplikacja uzyskuje dostęp do opcji nazwanego za pomocą [IOptionsSnapshot&lt;TOptions&gt;. Pobierz](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

Uruchomiona przykładowej aplikacji, zwracane są nazwane opcje:

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

`named_options_1` wartości są dostarczane z konfiguracji, które są ładowane z *appsettings.json* pliku. `named_options_2` wartości są dostarczane przez:

* `named_options_2` Delegowanie w `ConfigureServices` dla `Option1`.
* Wartość domyślna dla `Option2` podał `MyOptions` klasy.

Skonfiguruj wszystkie wystąpienia nazwanego opcje z [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) metody. Poniższy kod konfiguruje `Option1` wszystkie nazwane wystąpienia konfiguracji z wartością wspólnej. Dodaj następujący kod ręcznie do `Configure` metody:

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

Uruchamianie przykładowej aplikacji po dodaniu kod tworzy następujące wyniki:

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> W programie ASP.NET Core 2.0 lub nowszy wszystkie opcje są nazwane wystąpienia. Istniejące `IConfigureOption` wystąpienia są traktowane jak `Options.DefaultName` wystąpienia, która jest `string.Empty`. `IConfigureNamedOptions` implementuje również `IConfigureOptions`. Domyślna implementacja [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([źródło odwołania](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) ma logiki używania każdego odpowiednio. `null` Nazwanego opcja jest używana do wszystkich wystąpień nazwanych zamiast określonego nazwanego wystąpienia docelowego ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) i [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) Użyj tę Konwencję).

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

*Wymaga platformy ASP.NET Core 2.0 lub nowszej.*

Ustaw postconfiguration z [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1). Postconfiguration jest uruchamiana po wszystkich [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) występuje konfiguracji:

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) pozwala konfigurować po nazwanych opcje:

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

Użyj [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) po skonfigurować wszystkie nazwane wystąpienia konfiguracji:

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a>Opcje fabryki, monitorowania i pamięci podręcznej

[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) jest używany dla powiadomienia po `TOptions` wystąpienia zmiany. `IOptionsMonitor` obsługuje opcje instrument, zmień powiadomienia, a `IPostConfigureOptions`.

[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (platformy ASP.NET Core w wersji 2.0 lub nowszej) jest odpowiedzialny za tworzenie nowych opcji wystąpień. Ma on jeden [Utwórz](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) metody. Domyślna implementacja pobiera wszystkich zarejestrowanych `IConfigureOptions` i `IPostConfigureOptions` i uruchamia wszystkie konfiguruje się najpierw, a następnie po konfiguruje. Go rozróżnia `IConfigureNamedOptions` i `IConfigureOptions` i tylko wywołuje odpowiedniego interfejsu.

[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (platformy ASP.NET Core w wersji 2.0 lub nowszej) jest używany przez `IOptionsMonitor` do pamięci podręcznej `TOptions` wystąpień. `IOptionsMonitorCache` Unieważnia wystąpień opcje w monitorze tak, aby wartość jest przeliczane ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)). Wartości mogą być ręcznie wprowadzone również z [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd). [Wyczyść](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) metoda jest używana, gdy wszystkie wystąpienia nazwanego powinny zostać ponownie utworzone na żądanie.

## <a name="accessing-options-during-startup"></a>Dostęp do opcji podczas uruchamiania

`IOptions` mogą być używane w `Configure`, ponieważ usługi są wbudowane przed `Configure` metoda jest wykonywana. Jeśli usługodawca korzysta z wbudowanej w `ConfigureServices` uzyskać dostęp do opcji, ona nie zawierać opcje konfiguracji po utworzeniu dostawcy usług. W związku z tym stanie niespójne opcje mogą występować z powodu zamawiania rejestracji usługi.

Ponieważ opcje zwykle są ładowane z konfiguracji, konfiguracji mogą być używane w uruchomienia zarówno `Configure` i `ConfigureServices`. Przykłady za pomocą konfiguracji podczas uruchamiania, zobacz [uruchamiania aplikacji](xref:fundamentals/startup) tematu.

## <a name="see-also"></a>Zobacz także

* [Konfiguracja](xref:fundamentals/configuration/index)
