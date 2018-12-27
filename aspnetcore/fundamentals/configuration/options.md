---
title: Wzorzec opcje w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak użyć wzorca opcje do reprezentowania grup powiązanych ustawień w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2018
uid: fundamentals/configuration/options
ms.openlocfilehash: 0e3784de18be16e3217a015dd94f1b43b6621c1c
ms.sourcegitcommit: 68a3081dd175d6518d1bfa31b4712bd8a2dd3864
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53577893"
---
# <a name="options-pattern-in-aspnet-core"></a>Wzorzec opcje w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

::: moniker range="<= aspnetcore-1.1"

Dla wersji 1.1 w tym temacie, Pobierz [wzorzec opcje w programie ASP.NET Core (w wersji 1.1, plików PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Options_1.1.pdf).

::: moniker-end

Wzorzec opcje używa klas do reprezentowania grup powiązane ustawienia. Gdy [ustawienia konfiguracji](xref:fundamentals/configuration/index) są izolowane według scenariusza w osobnych klas, aplikacja działa zgodnie z dwóch zasad ważne inżynierii oprogramowania:

* [Zasady podziału interfejsu (ISP) lub hermetyzacji](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; scenariuszy (klasy), które są zależne od ustawień konfiguracji tylko zależą od ustawień konfiguracji, których używają.
* [Separacji](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; ustawień dla różnych części aplikacji nie są zależnych lub powiązanych ze sobą.

Opcje również udostępniać mechanizm do sprawdzania poprawności danych konfiguracji. Aby uzyskać więcej informacji, zobacz [opcje weryfikacji](#options-validation) sekcji.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

Odwołanie [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) pakietu.

## <a name="options-interfaces"></a>Opcje interfejsów

<xref:Microsoft.Extensions.Options.IOptionsMonitor`1> Służy do pobierania opcji i zarządzanie opcje powiadomienia dotyczące `TOptions` wystąpień. <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> obsługuje następujące scenariusze:

* Powiadomienia o zmianach
* [Opcje nazwane](#named-options-support-with-iconfigurenamedoptions)
* [Ładowany ponownie konfigurację](#reload-configuration-data-with-ioptionssnapshot)
* Opcje selektywnego unieważniania (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1>)

[Zadania po konfiguracji](#options-post-configuration) scenariuszach pozwala na ustawianie lub zmienianie po wszystkich opcji <xref:Microsoft.Extensions.Options.IConfigureOptions`1> występuje konfiguracji.

<xref:Microsoft.Extensions.Options.IOptionsFactory`1> jest odpowiedzialny za tworzenie nowych wystąpień opcje. Ma on jeden <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> metody. Domyślna implementacja pobiera wszystkie zarejestrowane <xref:Microsoft.Extensions.Options.IConfigureOptions`1> i <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1> i uruchamia wszystkie konfiguracje najpierw następuje po konfiguracji. Jego rozróżnia <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> i <xref:Microsoft.Extensions.Options.IConfigureOptions`1> i tylko wywołuje odpowiedniego interfejsu.

<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> jest używany przez <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> do pamięci podręcznej `TOptions` wystąpień. <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> Unieważnia opcje wystąpień w monitorze, tak aby wartość jest ponownie przeliczana (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>). Można ręcznie wprowadzić wartości z <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>. <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> Metoda jest używana, gdy wszystkie wystąpienia nazwanego, należy odtworzyć na żądanie.

<xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> jest przydatne w scenariuszach, gdzie opcje powinny być obliczane ponownie na każde żądanie. Aby uzyskać więcej informacji, zobacz [ponowne załadowanie danych konfiguracji o IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) sekcji.

<xref:Microsoft.Extensions.Options.IOptions`1> może służyć do obsługi opcji. Jednak <xref:Microsoft.Extensions.Options.IOptions`1> nie obsługuje powyższych scenariuszy z <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>. Możesz nadal używać <xref:Microsoft.Extensions.Options.IOptions`1> z istniejącymi strukturami i bibliotek, które już używają <xref:Microsoft.Extensions.Options.IOptions`1> interfejsu i nie wymagają scenariuszy, dostarczone przez <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>.

## <a name="general-options-configuration"></a>Ogólne opcje konfiguracji

Opcje ogólne konfiguracja jest przedstawiona jako przykład &num;1 w przykładowej aplikacji.

Klasa opcji musi być nieabstrakcyjnej przy użyciu publicznego konstruktora bez parametrów. Następujące klasy `MyOptions`, ma dwie właściwości `Option1` i `Option2`. Ustawianie wartości domyślnych jest opcjonalne, ale konstruktora klasy w poniższym przykładzie ustawiono wartość domyślną `Option1`. `Option2` ma wartość domyślną, ustawiane przez bezpośrednie inicjowanie właściwości (*Models/MyOptions.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

`MyOptions` Klasy zostanie dodany do kontenera usługi za pomocą <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> i powiązane z konfiguracji:

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

Stronie używa modelu [iniekcji zależności konstruktora](xref:mvc/controllers/dependency-injection) z <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> można uzyskać dostęp do ustawień (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

Przykładowe *appsettings.json* plik Określa wartości dla `option1` i `option2`:

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

Gdy aplikacja jest uruchamiana, modelu strony `OnGet` metoda zwraca ciąg przedstawiający wartości klasy opcji:

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> Korzystając z niestandardowego <xref:System.Configuration.ConfigurationBuilder> można załadować konfiguracji opcji z pliku ustawień, upewnij się, że ścieżka podstawowa jest prawidłowo:
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
> Jawne ustawianie ścieżka podstawowa nie jest wymagana podczas ładowania konfiguracji opcji z pliku ustawień za pośrednictwem <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.

## <a name="configure-simple-options-with-a-delegate"></a>Skonfiguruj opcje prostego za pomocą delegata

Konfigurowanie opcji prostego z delegatem przedstawiono przykład &num;2 w przykładowej aplikacji.

Użycie delegowania do ustawiania wartości opcji. Ta aplikacja używa przykładowych `MyOptionsWithDelegateConfig` klasy (*Models/MyOptionsWithDelegateConfig.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

W poniższym kodzie sekundy <xref:Microsoft.Extensions.Options.IConfigureOptions`1> usługi zostanie dodany do kontenera usług. Używa ona delegata do konfigurowania wiązania za pomocą `MyOptionsWithDelegateConfig`:

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

Można dodać wielu dostawców konfiguracji. Dostawcy konfiguracji są dostępne z pakietów NuGet i są stosowane w kolejności, że zostanie zarejestrowany. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.

Każde wywołanie <xref:Microsoft.Extensions.Options.IConfigureOptions`1.Configure*> dodaje <xref:Microsoft.Extensions.Options.IConfigureOptions`1> usługi service container. W powyższym przykładzie wartości `Option1` i `Option2` określono zarówno w *appsettings.json*, ale wartości `Option1` i `Option2` są zastępowane przez delegata skonfigurowany.

Po włączeniu więcej niż jednej konfiguracji usługi ostatni źródło konfiguracji określone *wins* i ustawia wartość konfiguracji. Gdy aplikacja jest uruchamiana, modelu strony `OnGet` metoda zwraca ciąg przedstawiający wartości klasy opcji:

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>Konfiguracja suboptions

Suboptions konfiguracja jest przedstawiona jako przykład &num;3 w przykładowej aplikacji.

Aplikacje powinny zostać utworzone opcje klasy, które odnoszą się do danego scenariusza grupy (klasy) w aplikacji. Części aplikacji, które wymagają wartości konfiguracji powinien mieć tylko dostęp do wartości konfiguracji, których używają.

Podczas tworzenia powiązania opcje konfiguracji, każdej właściwości w typie opcji jest powiązany z kluczem konfiguracji formularza `property[:sub-property:]`. Na przykład `MyOptions.Option1` właściwość jest powiązana z klucza `Option1`, które są odczytywane z `option1` właściwość *appsettings.json*.

W poniższym kodzie, a trzeci <xref:Microsoft.Extensions.Options.IConfigureOptions`1> usługi zostanie dodany do kontenera usług. Powiąże `MySubOptions` do sekcji `subsection` z *appsettings.json* pliku:

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

`GetSection` — Metoda rozszerzenia wymaga [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) pakietu NuGet. Jeśli aplikacja korzysta z [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 lub nowszej), pakiet jest automatycznie dołączane.

Przykładowe *appsettings.json* plik definiuje `subsection` członka za pomocą kluczy `suboption1` i `suboption2`:

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

`MySubOptions` Klasy definiuje właściwości, `SubOption1` i `SubOption2`, aby przechowywać wartości opcji (*Models/MySubOptions.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

Model strony `OnGet` metoda zwraca ciąg zawierający wartości opcji (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

Gdy aplikacja jest uruchamiana, `OnGet` metoda zwraca ciąg przedstawiający suboption wartości klasy:

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>Opcje dostarczane przez model widoku lub przy użyciu iniekcji bezpośrednie widoku

Opcje dostarczane przez model widoku lub przy użyciu iniekcji bezpośrednie widoku przedstawiono przykład &num;4 w przykładowej aplikacji.

Opcje mogą być podawane w modelu widoku lub przez iniekcję <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> bezpośrednio w widoku (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

Przykładowa aplikacja pokazuje, jak wstawić `IOptionsMonitor<MyOptions>` z `@inject` dyrektywy:

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

Gdy aplikacja jest uruchamiana, wartości opcje są wyświetlane na renderowanej stronie:

![Opcje wartości opcja1: value1_from_json i opcja2: -1 są ładowane z modelu, jak również iniekcję do widoku.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>Załaduj ponownie dane konfiguracyjne z IOptionsSnapshot

Ponowne załadowanie danych konfiguracji z <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> przedstawiono w przykładzie &num;5 w przykładowej aplikacji.

<xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> Obsługa ponownego ładowania opcji z minimalnym przetwarzania.

Opcje są obliczane raz na każde żądanie, gdy dostęp do pamięci podręcznej i okresu istnienia żądania.

W poniższym przykładzie pokazano, jak nowy <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> został utworzony po *appsettings.json* zmiany (*Pages/Index.cshtml.cs*). Wiele żądań do serwera zwracają stałe wartości, dostarczone przez *appsettings.json* pliku do momentu zmiany pliku i ponowne załadowanie konfiguracji.

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

Na poniższej ilustracji przedstawiono początkowe `option1` i `option2` wartości załadowane z *appsettings.json* pliku:

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

Zmień wartości w *appsettings.json* plik `value1_from_json UPDATED` i `200`. Zapisz *appsettings.json* pliku. Odśwież przeglądarkę, aby zobaczyć, że zostaną zaktualizowane wartości opcji:

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>Opcje pomocy technicznej, za pomocą IConfigureNamedOptions nazwane

Opcje pomocy technicznej, o nazwie <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> przedstawiono przykład &num;6 w przykładowej aplikacji.

*O nazwie opcje* pomocy technicznej zezwala aplikacji na rozróżnienie między nazwane opcje konfiguracji. W przykładowej aplikacji o nazwie opcje są uznane za pomocą <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*>. `Configure` wywołuje metodę rozszerzenia <xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*> metody:

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

Przykładowa aplikacja uzyskuje dostęp do opcji o nazwie za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

Uruchamianie przykładowej aplikacji, zwracane są nazwane opcje:

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

`named_options_1` wartości są dostarczane z konfiguracji, które są ładowane z *appsettings.json* pliku. `named_options_2` wartości są dostarczane przez:

* `named_options_2` Delegowanie w `ConfigureServices` dla `Option1`.
* Wartością domyślną dla `Option2` dostarczone przez `MyOptions` klasy.

## <a name="configure-all-options-with-the-configureall-method"></a>Skonfiguruj wszystkie opcje przy użyciu metody ConfigureAll

Konfigurowanie wszystkich wystąpień opcje <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> metody. Poniższy kod służy do konfigurowania `Option1` dla wszystkich wystąpień konfiguracji za pomocą wspólnej wartości. Dodaj następujący kod ręcznie do `Startup.ConfigureServices` metody:

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
> Wszystkie opcje są nazwane wystąpienia. Istniejące <xref:Microsoft.Extensions.Options.IConfigureOptions`1> wystąpienia są traktowane jako docelowy `Options.DefaultName` wystąpienia, co jest `string.Empty`. <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> implementuje również <xref:Microsoft.Extensions.Options.IConfigureOptions`1>. Domyślna implementacja klasy <xref:Microsoft.Extensions.Options.IOptionsFactory`1> zawiera logikę w celu używania każdego odpowiednio. `null` Nazwane opcja jest używana do Docieraj do wszystkich wystąpień nazwanych, zamiast określonego nazwanego wystąpienia (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> i <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> ta Konwencja).

## <a name="optionsbuilder-api"></a>OptionsBuilder interfejsu API

<xref:Microsoft.Extensions.Options.OptionsBuilder`1> Służy do konfigurowania `TOptions` wystąpień. `OptionsBuilder` usprawnia tworzenie o nazwie opcji, ponieważ jest tylko jeden parametr do początkowego `AddOptions<TOptions>(string optionsName)` wywoływać zamiast znajdujących się we wszystkich kolejnych wywołań. Opcje sprawdzania poprawności i `ConfigureOptions` przeciążenia, które akceptują zależności usług, są dostępne tylko za pośrednictwem `OptionsBuilder`.

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="configurelttoptions-tdep1--tdep4gt-method"></a>Konfigurowanie&lt;TOptions, TDep1,... TDep4&gt; — metoda

Za pomocą usług z DI, aby skonfigurować opcje implementując `IConfigure[Named]Options` w standardowy sposób jest pełne. Przeciążenia `ConfigureOptions` na `OptionsBuilder<TOptions>` pozwalają na korzystanie z usług do pięciu do konfigurowania opcji:

```csharp
services.AddOptions<MyOptions>("optionalName")
    .Configure<Service1, Service2, Service3, Service4, Service5>(
        (o, s, s2, s3, s4, s5) => 
            o.Property = DoSomethingWith(s, s2, s3, s4, s5));
```

Przeciążenie rejestruje rodzajowy przejściowe <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1>, która ma Konstruktor, który akceptuje typy Usługa ogólna określony. 

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
* Sprawdzanie poprawności, który jest zależny od innego typu opcji: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`

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

Data sprawdzania poprawności opartego na adnotacji jest dostępne z [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) pakietu przez wywołanie metody `ValidateDataAnnotations` metody `OptionsBuilder<TOptions>`. `Microsoft.Extensions.Options.DataAnnotations` znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (platformy ASP.NET Core 2,2 lub nowszej).

```csharp
private class AnnotatedOptions
{
    [Required]
    public string Required { get; set; }

    [StringLength(5, ErrorMessage = "Too long.")]
    public string StringLength { get; set; }

    [Range(-5, 5, ErrorMessage = "Out of range.")]
    public int IntRange { get; set; }
}

[Fact]
public void CanValidateDataAnnotations()
{
    var services = new ServiceCollection();
    services.AddOptions<AnnotatedOptions>()
        .Configure(o =>
        {
            o.StringLength = "111111";
            o.IntRange = 10;
            o.Custom = "nowhere";
        })
        .ValidateDataAnnotations();

    var sp = services.BuildServiceProvider();

    var error = Assert.Throws<OptionsValidationException>(() => 
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().Value);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

Weryfikacja eager (po awarii szybkie przy uruchamianiu) jest pod uwagę w przyszłej wersji.

::: moniker-end

## <a name="options-post-configuration"></a>Opcje zadania po konfiguracji

Ustaw po konfiguracji za pomocą <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1>. Zadania po konfiguracji jest uruchamiana po wszystkich <xref:Microsoft.Extensions.Options.IConfigureOptions`1> występuje konfiguracji:

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> jest dostępna po konfigurowania opcji o nazwie:

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

Użyj <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> po skonfigurować wszystkie wystąpienia konfiguracji:

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a>Uzyskiwanie dostępu do opcji podczas uruchamiania

<xref:Microsoft.Extensions.Options.IOptions`1> i <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> mogą być używane w `Startup.Configure`, ponieważ usługi są kompilowane przed `Configure` metoda jest wykonywana.

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

Nie używaj <xref:Microsoft.Extensions.Options.IOptions`1> lub <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> w `Startup.ConfigureServices`. Stan opcje niezgodne mogą występować z powodu zamawiania rejestracji usługi.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/configuration/index>
