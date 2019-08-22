---
title: Wzorzec opcji w ASP.NET Core
author: guardrex
description: Dowiedz się, jak używać wzorca opcji do reprezentowania grup powiązanych ustawień w aplikacjach ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/19/2019
uid: fundamentals/configuration/options
ms.openlocfilehash: 22dde4b05ea20fedb696c6a4b144755a957e8c0d
ms.sourcegitcommit: 41f2c1a6b316e6e368a4fd27a8b18d157cef91e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886371"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="9c469-103">Wzorzec opcji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9c469-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="9c469-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9c469-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9c469-105">Wzorzec opcji używa klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="9c469-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="9c469-106">Gdy [Ustawienia konfiguracji](xref:fundamentals/configuration/index) są izolowane według scenariusza w osobnych klasach, aplikacja będzie zgodna z dwoma ważnymi zasadami inżynierii oprogramowania:</span><span class="sxs-lookup"><span data-stu-id="9c469-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="9c469-107">[Zasady podziału interfejsu (ISP) lub](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; scenariusze hermetyzacji (klasy), które są zależne od ustawień konfiguracji, zależą tylko od ustawień konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="9c469-107">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="9c469-108">[Rozdzielenie problemów](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Ustawienia dla różnych części aplikacji nie są zależne ani połączone ze sobą.</span><span class="sxs-lookup"><span data-stu-id="9c469-108">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="9c469-109">Opcje umożliwiają również mechanizm weryfikacji danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="9c469-109">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="9c469-110">Aby uzyskać więcej informacji, zobacz sekcję [Opcje walidacji](#options-validation) .</span><span class="sxs-lookup"><span data-stu-id="9c469-110">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="9c469-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9c469-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c469-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="9c469-112">Prerequisites</span></span>

<span data-ttu-id="9c469-113">Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) .</span><span class="sxs-lookup"><span data-stu-id="9c469-113">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="9c469-114">Interfejsy opcji</span><span class="sxs-lookup"><span data-stu-id="9c469-114">Options interfaces</span></span>

<span data-ttu-id="9c469-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601>służy do pobierania opcji i zarządzania powiadomieniami `TOptions` o wystąpieniach.</span><span class="sxs-lookup"><span data-stu-id="9c469-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="9c469-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601>Program obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="9c469-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="9c469-117">Powiadomienia o zmianie</span><span class="sxs-lookup"><span data-stu-id="9c469-117">Change notifications</span></span>
* [<span data-ttu-id="9c469-118">Nazwane opcje</span><span class="sxs-lookup"><span data-stu-id="9c469-118">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="9c469-119">Konfiguracja do ponownego ładowania</span><span class="sxs-lookup"><span data-stu-id="9c469-119">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="9c469-120">Unieważnianie opcji selektywnych (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="9c469-120">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="9c469-121">Scenariusze [po konfiguracji](#options-post-configuration) umożliwiają ustawianie lub zmienianie opcji po wystąpieniu całej <xref:Microsoft.Extensions.Options.IConfigureOptions%601> konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9c469-121">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="9c469-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601>jest odpowiedzialny za tworzenie nowych wystąpień opcji.</span><span class="sxs-lookup"><span data-stu-id="9c469-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="9c469-123">Ma jedną <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> metodę.</span><span class="sxs-lookup"><span data-stu-id="9c469-123">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="9c469-124">Domyślna implementacja przyjmuje wszystkie zarejestrowane <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> uruchamia najpierw wszystkie konfiguracje, po których następuje po zakończeniu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9c469-124">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="9c469-125">Odróżnia między <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i tylko wywołuje odpowiedni interfejs.</span><span class="sxs-lookup"><span data-stu-id="9c469-125">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="9c469-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>jest używany przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> program do `TOptions` buforowania wystąpień.</span><span class="sxs-lookup"><span data-stu-id="9c469-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="9c469-127">Unieważnia wystąpienia opcji w monitorze, aby wartość była ponownie obliczana (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>). <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601></span><span class="sxs-lookup"><span data-stu-id="9c469-127">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="9c469-128">Wartości można wprowadzać ręcznie przy użyciu <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="9c469-128">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="9c469-129">Metoda <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> jest używana, gdy wszystkie nazwane wystąpienia powinny być ponownie tworzone na żądanie.</span><span class="sxs-lookup"><span data-stu-id="9c469-129">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="9c469-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>jest przydatne w scenariuszach, w których opcje powinny być ponownie obliczane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="9c469-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="9c469-131">Aby uzyskać więcej informacji, zobacz sekcję [Załaduj dane konfiguracji za pomocą IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) .</span><span class="sxs-lookup"><span data-stu-id="9c469-131">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="9c469-132"><xref:Microsoft.Extensions.Options.IOptions%601>może służyć do obsługi opcji.</span><span class="sxs-lookup"><span data-stu-id="9c469-132"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="9c469-133">Program <xref:Microsoft.Extensions.Options.IOptions%601> nie obsługuje jednak powyższych <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="9c469-133">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="9c469-134">Można nadal korzystać <xref:Microsoft.Extensions.Options.IOptions%601> z istniejących platform i bibliotek, które już korzystają z <xref:Microsoft.Extensions.Options.IOptions%601> interfejsu i nie wymagają scenariuszy udostępnianych przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>program.</span><span class="sxs-lookup"><span data-stu-id="9c469-134">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="9c469-135">Konfiguracja opcji ogólnych</span><span class="sxs-lookup"><span data-stu-id="9c469-135">General options configuration</span></span>

<span data-ttu-id="9c469-136">Konfiguracja opcji ogólnych jest przedstawiona jako &num;przykład 1 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="9c469-136">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="9c469-137">Klasa Options musi być nieabstrakcyjna z publicznym konstruktorem bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="9c469-137">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="9c469-138">Następująca Klasa `MyOptions`,, ma dwie właściwości, `Option1` i `Option2`.</span><span class="sxs-lookup"><span data-stu-id="9c469-138">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="9c469-139">Ustawienie wartości domyślnych jest opcjonalne, ale Konstruktor klasy w poniższym przykładzie ustawia wartość `Option1`domyślną.</span><span class="sxs-lookup"><span data-stu-id="9c469-139">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="9c469-140">`Option2`ma ustawioną wartość domyślną, inicjując właściwość bezpośrednio (*modele/opcje. cs*):</span><span class="sxs-lookup"><span data-stu-id="9c469-140">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="9c469-141">Klasa jest dodawana do kontenera usługi z <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> i powiązana z konfiguracją: `MyOptions`</span><span class="sxs-lookup"><span data-stu-id="9c469-141">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="9c469-142">Poniższy model strony używa [iniekcji zależności konstruktora](xref:mvc/controllers/dependency-injection) z <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> programem w celu uzyskania dostępu do ustawień (*stron/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="9c469-142">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="9c469-143">Plik *appSettings. JSON* przykładu Określa wartości dla `option1` i: `option2`</span><span class="sxs-lookup"><span data-stu-id="9c469-143">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="9c469-144">Po uruchomieniu aplikacji `OnGet` Metoda modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="9c469-144">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="9c469-145">W przypadku używania niestandardowej <xref:System.Configuration.ConfigurationBuilder> konfiguracji opcji do ładowania z pliku ustawień upewnij się, że ścieżka bazowa jest ustawiona poprawnie:</span><span class="sxs-lookup"><span data-stu-id="9c469-145">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="9c469-146">Jawne ustawienie ścieżki podstawowej nie jest wymagane podczas ładowania konfiguracji opcji z pliku ustawień za pośrednictwem <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="9c469-146">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="9c469-147">Skonfiguruj proste opcje z delegatem</span><span class="sxs-lookup"><span data-stu-id="9c469-147">Configure simple options with a delegate</span></span>

<span data-ttu-id="9c469-148">Konfigurowanie prostych opcji z delegatem jest zademonstrowane jako przykład &num;2 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9c469-148">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="9c469-149">Użyj delegata, aby ustawić wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="9c469-149">Use a delegate to set options values.</span></span> <span data-ttu-id="9c469-150">Przykładowa aplikacja używa `MyOptionsWithDelegateConfig` klasy (*models/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="9c469-150">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="9c469-151">W poniższym kodzie zostanie dodana druga <xref:Microsoft.Extensions.Options.IConfigureOptions%601> usługa do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="9c469-151">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="9c469-152">Używa delegata do konfigurowania powiązania z `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="9c469-152">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="9c469-153">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="9c469-153">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="9c469-154">Można dodać wielu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9c469-154">You can add multiple configuration providers.</span></span> <span data-ttu-id="9c469-155">Dostawcy konfiguracji są dostępni z pakietów NuGet i są stosowane w kolejności, w jakiej zostały zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="9c469-155">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="9c469-156">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="9c469-156">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="9c469-157">Każde wywołanie <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> <xref:Microsoft.Extensions.Options.IConfigureOptions%601> dodaje usługę do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="9c469-157">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="9c469-158">`Option1` W poprzednim przykładzie wartości i `Option2` są określone w pliku `Option1` *appSettings. JSON*, ale wartości i `Option2` są zastępowane przez skonfigurowany delegat.</span><span class="sxs-lookup"><span data-stu-id="9c469-158">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="9c469-159">Gdy jest włączona więcej niż jedna usługa konfiguracji, ostatnie Źródło konfiguracji określiło *serwer WINS* i ustawi wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9c469-159">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="9c469-160">Po uruchomieniu aplikacji `OnGet` Metoda modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="9c469-160">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="9c469-161">Konfiguracja podopcji</span><span class="sxs-lookup"><span data-stu-id="9c469-161">Suboptions configuration</span></span>

<span data-ttu-id="9c469-162">Konfiguracja podopcji jest przedstawiana jako przykład &num;3 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9c469-162">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="9c469-163">Aplikacje powinny tworzyć klasy opcji, które odnoszą się do określonych grup scenariuszy (klas) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9c469-163">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="9c469-164">Części aplikacji, które wymagają wartości konfiguracyjnych, powinny mieć dostęp tylko do wartości konfiguracyjnych, z których korzystają.</span><span class="sxs-lookup"><span data-stu-id="9c469-164">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="9c469-165">Po powiązaniu opcji powiązań z konfiguracją Każda właściwość w typie opcji jest powiązana z kluczem konfiguracji formularza `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="9c469-165">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="9c469-166">Na przykład `MyOptions.Option1` właściwość jest powiązana z kluczem `Option1`, `option1` który jest odczytywany z właściwości w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="9c469-166">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="9c469-167">W poniższym kodzie, trzecia <xref:Microsoft.Extensions.Options.IConfigureOptions%601> usługa jest dodawana do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="9c469-167">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="9c469-168">Wiąże `MySubOptions` się z sekcją `subsection` pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="9c469-168">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="9c469-169">Metoda rozszerzenia wymaga pakietu NuGet [Microsoft. Extensions. options. ConfigurationExtensions.](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) `GetSection`</span><span class="sxs-lookup"><span data-stu-id="9c469-169">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="9c469-170">Jeśli aplikacja używa [pakietu Microsoft. AspNetCore. appbinding](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 lub nowszego), pakiet jest automatycznie dołączany.</span><span class="sxs-lookup"><span data-stu-id="9c469-170">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="9c469-171">Plik *appSettings. JSON* przykładu definiuje `subsection` element członkowski z kluczami dla `suboption1` i `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="9c469-171">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="9c469-172">Klasa definiuje `SubOption1` właściwości i`SubOption2`, aby przechowywać wartości opcji (*modele/MySubOptions. cs*): `MySubOptions`</span><span class="sxs-lookup"><span data-stu-id="9c469-172">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="9c469-173">`OnGet` Metoda modelu strony zwraca ciąg z wartościami opcji (*Pages/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="9c469-173">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="9c469-174">Gdy aplikacja jest uruchomiona, `OnGet` Metoda zwraca ciąg pokazujący wartości klasy podopcji:</span><span class="sxs-lookup"><span data-stu-id="9c469-174">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="9c469-175">Opcje udostępniane przez model widoku lub bezpośrednie iniekcja widoku</span><span class="sxs-lookup"><span data-stu-id="9c469-175">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="9c469-176">Opcje udostępniane przez model widoku lub bezpośrednie wstrzyknięcie widoku są przedstawiane jako przykład &num;4 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="9c469-176">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="9c469-177">Opcje można dostarczyć w modelu widoku lub poprzez wstrzyknięcie <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> bezpośrednio do widoku (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="9c469-177">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="9c469-178">Przykładowa aplikacja pokazuje, `IOptionsMonitor<MyOptions>` `@inject` jak wstrzyknąć przy użyciu dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="9c469-178">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/3.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="9c469-179">Po uruchomieniu aplikacji na renderowanej stronie są wyświetlane wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="9c469-179">When the app is run, the options values are shown in the rendered page:</span></span>

![Wartości opcji opcja1: value1_from_json i Opcja2:-1 są ładowane z modelu i przez iniekcję do widoku.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="9c469-181">Załaduj ponownie dane konfiguracji za pomocą IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="9c469-181">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="9c469-182">Ponowne ładowanie danych konfiguracyjnych przy <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> użyciu programu jest zademonstrowane &num;w przykładzie 5 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9c469-182">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="9c469-183"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>obsługuje opcje ponownego ładowania z minimalnym obciążeniem przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="9c469-183"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="9c469-184">Opcje są obliczane raz dla żądania w przypadku uzyskiwania dostępu do pamięci podręcznej i buforowania jej przez okres istnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="9c469-184">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="9c469-185">W poniższym przykładzie pokazano, jak nowa <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> jest tworzona po wprowadzeniu zmian *appSettings. JSON* (*Pages/index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="9c469-185">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="9c469-186">Wiele żądań do zwracanych przez serwer wartości stałych dostarczonych przez plik *appSettings. JSON* do momentu zmiany pliku i ponownego załadowania konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9c469-186">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="9c469-187">Na poniższej ilustracji przedstawiono początkowe `option1` i `option2` wartości załadowane z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="9c469-187">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="9c469-188">Zmień wartości w pliku *appSettings. JSON* na `value1_from_json UPDATED` i `200`.</span><span class="sxs-lookup"><span data-stu-id="9c469-188">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="9c469-189">Zapisz plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="9c469-189">Save the *appsettings.json* file.</span></span> <span data-ttu-id="9c469-190">Odśwież przeglądarkę, aby zobaczyć, że wartości opcji są aktualizowane:</span><span class="sxs-lookup"><span data-stu-id="9c469-190">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="9c469-191">Obsługa nazwanych opcji w programie IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="9c469-191">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="9c469-192">Pomoc techniczna dotycząca opcji <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> nazwanych w programie jest &num;prezentowana jako przykład 6 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9c469-192">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="9c469-193">Obsługa " *nazwanych opcji* " pozwala aplikacji rozróżnić między nazwanymi konfiguracjami opcji.</span><span class="sxs-lookup"><span data-stu-id="9c469-193">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="9c469-194">W przykładowej aplikacji, nazwane opcje są zadeklarowane za pomocą [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), która [wywołuje\<ConfigureNamedOptions TOptions >. Konfiguruj](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) metodę rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="9c469-194">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="9c469-195">Aplikacja Przykładowa uzyskuje dostęp do nazwanych <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> opcji przy użyciu (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="9c469-195">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="9c469-196">Po uruchomieniu aplikacji przykładowej są zwracane nazwane opcje:</span><span class="sxs-lookup"><span data-stu-id="9c469-196">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="9c469-197">`named_options_1`wartości są dostarczane z konfiguracji, które są ładowane z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="9c469-197">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="9c469-198">`named_options_2`wartości są podawane przez:</span><span class="sxs-lookup"><span data-stu-id="9c469-198">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="9c469-199">Delegat w `ConfigureServices` dla`Option1`. `named_options_2`</span><span class="sxs-lookup"><span data-stu-id="9c469-199">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="9c469-200">Wartość domyślna dla `Option2` dostarczonych `MyOptions` przez klasę.</span><span class="sxs-lookup"><span data-stu-id="9c469-200">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="9c469-201">Skonfiguruj wszystkie opcje przy użyciu metody ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="9c469-201">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="9c469-202">Skonfiguruj wszystkie wystąpienia opcji za pomocą <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> metody.</span><span class="sxs-lookup"><span data-stu-id="9c469-202">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="9c469-203">Poniższy kod konfiguruje `Option1` wszystkie wystąpienia konfiguracji za pomocą wspólnej wartości.</span><span class="sxs-lookup"><span data-stu-id="9c469-203">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="9c469-204">Ręcznie Dodaj następujący kod do `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="9c469-204">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="9c469-205">Uruchomienie przykładowej aplikacji po dodaniu kodu daje następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="9c469-205">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="9c469-206">Wszystkie opcje są nazwanymi wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="9c469-206">All options are named instances.</span></span> <span data-ttu-id="9c469-207">Istniejące <xref:Microsoft.Extensions.Options.IConfigureOptions%601> wystąpienia są traktowane jako docelowe dla `Options.DefaultName` wystąpienia, czyli `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="9c469-207">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="9c469-208"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>implementuje <xref:Microsoft.Extensions.Options.IConfigureOptions%601>także.</span><span class="sxs-lookup"><span data-stu-id="9c469-208"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="9c469-209">Domyślna implementacja <xref:Microsoft.Extensions.Options.IOptionsFactory%601> logiki ma na celu odpowiednie użycie.</span><span class="sxs-lookup"><span data-stu-id="9c469-209">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="9c469-210">Nazwana opcja służy do określania wszystkich wystąpień nazwanych zamiast określonego wystąpienia nazwanego (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> i <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> używania tej Konwencji). `null`</span><span class="sxs-lookup"><span data-stu-id="9c469-210">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="9c469-211">Interfejs API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="9c469-211">OptionsBuilder API</span></span>

<span data-ttu-id="9c469-212"><xref:Microsoft.Extensions.Options.OptionsBuilder%601>służy do konfigurowania `TOptions` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="9c469-212"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="9c469-213">`OptionsBuilder`usprawnia Tworzenie nazwanych opcji, ponieważ jest tylko pojedynczym parametrem do początkowego `AddOptions<TOptions>(string optionsName)` wywołania, a nie pojawia się we wszystkich kolejnych wywołaniach.</span><span class="sxs-lookup"><span data-stu-id="9c469-213">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="9c469-214">Sprawdzanie poprawności opcji i `ConfigureOptions` przeciążenia, które akceptują zależności usługi, są `OptionsBuilder`dostępne tylko za pośrednictwem programu.</span><span class="sxs-lookup"><span data-stu-id="9c469-214">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="9c469-215">Korzystanie z usług DI Services w celu konfigurowania opcji</span><span class="sxs-lookup"><span data-stu-id="9c469-215">Use DI services to configure options</span></span>

<span data-ttu-id="9c469-216">Można uzyskać dostęp do innych usług z iniekcji zależności podczas konfigurowania opcji na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="9c469-216">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="9c469-217">Przekaż delegat konfiguracji w celu [](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) skonfigurowania [na\<OptionsBuilder TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="9c469-217">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="9c469-218">[OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) zapewnia przeciążenia [konfiguracji](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) , które pozwalają na używanie do pięciu usług do konfigurowania opcji:</span><span class="sxs-lookup"><span data-stu-id="9c469-218">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="9c469-219">Utwórz własny typ, który implementuje <xref:Microsoft.Extensions.Options.IConfigureOptions%601> lub <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i rejestruje typ jako usługę.</span><span class="sxs-lookup"><span data-stu-id="9c469-219">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="9c469-220">Zalecamy przekazanie delegata konfiguracji w [](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)celu skonfigurowania, ponieważ tworzenie usługi jest bardziej skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="9c469-220">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="9c469-221">Tworzenie własnego typu jest równoznaczne z tym, co jest używane przez platformę podczas [konfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="9c469-221">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="9c469-222">Wywołanie [konfiguracji](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) rejestruje przejściowe ogólne <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, który ma konstruktora akceptującego określone typy usług ogólnych.</span><span class="sxs-lookup"><span data-stu-id="9c469-222">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="9c469-223">Sprawdzanie poprawności opcji</span><span class="sxs-lookup"><span data-stu-id="9c469-223">Options validation</span></span>

<span data-ttu-id="9c469-224">Sprawdzanie poprawności opcji umożliwia sprawdzenie opcji w przypadku skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="9c469-224">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="9c469-225">Wywołaj `Validate` metodę walidacji zwracającą `true` wartość, jeśli opcje są `false` prawidłowe i jeśli są nieprawidłowe:</span><span class="sxs-lookup"><span data-stu-id="9c469-225">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="9c469-226">Poprzedni przykład ustawia nazwane wystąpienie opcji na `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="9c469-226">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="9c469-227">Domyślne wystąpienie opcji to `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="9c469-227">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="9c469-228">Walidacja jest uruchamiana po utworzeniu wystąpienia opcji.</span><span class="sxs-lookup"><span data-stu-id="9c469-228">Validation runs when the options instance is created.</span></span> <span data-ttu-id="9c469-229">Twoje wystąpienie opcji gwarantuje przekazanie walidacji przy pierwszej próbie dostępu.</span><span class="sxs-lookup"><span data-stu-id="9c469-229">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9c469-230">Sprawdzanie poprawności opcji nie chroni przed modyfikacjami opcji po początkowym skonfigurowaniu opcji i sprawdzeniu poprawności.</span><span class="sxs-lookup"><span data-stu-id="9c469-230">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="9c469-231">`Validate` Metoda`Func<TOptions, bool>`akceptuje.</span><span class="sxs-lookup"><span data-stu-id="9c469-231">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="9c469-232">Aby w pełni dostosować walidację `IValidateOptions<TOptions>`, zaimplementuj, która umożliwia:</span><span class="sxs-lookup"><span data-stu-id="9c469-232">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="9c469-233">Sprawdzanie poprawności wielu typów opcji:`class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="9c469-233">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="9c469-234">Walidacja zależna od innego typu opcji:`public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="9c469-234">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="9c469-235">`IValidateOptions`weryfikuje</span><span class="sxs-lookup"><span data-stu-id="9c469-235">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="9c469-236">Konkretne nazwane wystąpienie opcji.</span><span class="sxs-lookup"><span data-stu-id="9c469-236">A specific named options instance.</span></span>
* <span data-ttu-id="9c469-237">Wszystkie opcje w `name` przypadku `null`.</span><span class="sxs-lookup"><span data-stu-id="9c469-237">All options when `name` is `null`.</span></span>

<span data-ttu-id="9c469-238">`ValidateOptionsResult` Zwróć z implementacji interfejsu:</span><span class="sxs-lookup"><span data-stu-id="9c469-238">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="9c469-239">Sprawdzanie poprawności opartej na adnotacji danych jest dostępne w pakiecie [Microsoft. Extensions. options.](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) DataAnnotations <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> poprzez wywołanie `OptionsBuilder<TOptions>`metody w.</span><span class="sxs-lookup"><span data-stu-id="9c469-239">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="9c469-240">`Microsoft.Extensions.Options.DataAnnotations`jest zawarty w [pakiecie Microsoft. AspNetCore. appbinding](xref:fundamentals/metapackage-app) (ASP.NET Core 2,2 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="9c469-240">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.2 or later).</span></span>

```csharp
using Microsoft.Extensions.DependencyInjection;

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

<span data-ttu-id="9c469-241">Eager sprawdzanie poprawności (niepowodzenie szybkie przy uruchamianiu) jest rozważane dla przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="9c469-241">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="9c469-242">Opcje po konfiguracji</span><span class="sxs-lookup"><span data-stu-id="9c469-242">Options post-configuration</span></span>

<span data-ttu-id="9c469-243">Ustaw wartość po konfiguracji za <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>pomocą.</span><span class="sxs-lookup"><span data-stu-id="9c469-243">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="9c469-244">Po wykonaniu wszystkich <xref:Microsoft.Extensions.Options.IConfigureOptions%601> czynności konfiguracyjnych są wykonywane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="9c469-244">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="9c469-245"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*>jest dostępny do po skonfigurowaniu nazwanych opcji:</span><span class="sxs-lookup"><span data-stu-id="9c469-245"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="9c469-246">Użyj <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> , aby skonfigurować wszystkie wystąpienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="9c469-246">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="9c469-247">Dostęp do opcji podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="9c469-247">Accessing options during startup</span></span>

<span data-ttu-id="9c469-248"><xref:Microsoft.Extensions.Options.IOptions%601>i <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> mogą być używane w `Startup.Configure`, ponieważ `Configure` usługi zostały skompilowane przed wykonaniem metody.</span><span class="sxs-lookup"><span data-stu-id="9c469-248"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="9c469-249">Nie używaj <xref:Microsoft.Extensions.Options.IOptions%601> ani <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9c469-249">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9c469-250">Niespójny stan opcji może istnieć ze względu na kolejność rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="9c469-250">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="9c469-251">Wzorzec opcji używa klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="9c469-251">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="9c469-252">Gdy [Ustawienia konfiguracji](xref:fundamentals/configuration/index) są izolowane według scenariusza w osobnych klasach, aplikacja będzie zgodna z dwoma ważnymi zasadami inżynierii oprogramowania:</span><span class="sxs-lookup"><span data-stu-id="9c469-252">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="9c469-253">[Zasady podziału interfejsu (ISP) lub](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; scenariusze hermetyzacji (klasy), które są zależne od ustawień konfiguracji, zależą tylko od ustawień konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="9c469-253">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="9c469-254">[Rozdzielenie problemów](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Ustawienia dla różnych części aplikacji nie są zależne ani połączone ze sobą.</span><span class="sxs-lookup"><span data-stu-id="9c469-254">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="9c469-255">Opcje umożliwiają również mechanizm weryfikacji danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="9c469-255">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="9c469-256">Aby uzyskać więcej informacji, zobacz sekcję [Opcje walidacji](#options-validation) .</span><span class="sxs-lookup"><span data-stu-id="9c469-256">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="9c469-257">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9c469-257">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c469-258">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="9c469-258">Prerequisites</span></span>

<span data-ttu-id="9c469-259">Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) .</span><span class="sxs-lookup"><span data-stu-id="9c469-259">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="9c469-260">Interfejsy opcji</span><span class="sxs-lookup"><span data-stu-id="9c469-260">Options interfaces</span></span>

<span data-ttu-id="9c469-261"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601>służy do pobierania opcji i zarządzania powiadomieniami `TOptions` o wystąpieniach.</span><span class="sxs-lookup"><span data-stu-id="9c469-261"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="9c469-262"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601>Program obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="9c469-262"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="9c469-263">Powiadomienia o zmianie</span><span class="sxs-lookup"><span data-stu-id="9c469-263">Change notifications</span></span>
* [<span data-ttu-id="9c469-264">Nazwane opcje</span><span class="sxs-lookup"><span data-stu-id="9c469-264">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="9c469-265">Konfiguracja do ponownego ładowania</span><span class="sxs-lookup"><span data-stu-id="9c469-265">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="9c469-266">Unieważnianie opcji selektywnych (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="9c469-266">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="9c469-267">Scenariusze [po konfiguracji](#options-post-configuration) umożliwiają ustawianie lub zmienianie opcji po wystąpieniu całej <xref:Microsoft.Extensions.Options.IConfigureOptions%601> konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9c469-267">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="9c469-268"><xref:Microsoft.Extensions.Options.IOptionsFactory%601>jest odpowiedzialny za tworzenie nowych wystąpień opcji.</span><span class="sxs-lookup"><span data-stu-id="9c469-268"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="9c469-269">Ma jedną <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> metodę.</span><span class="sxs-lookup"><span data-stu-id="9c469-269">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="9c469-270">Domyślna implementacja przyjmuje wszystkie zarejestrowane <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> uruchamia najpierw wszystkie konfiguracje, po których następuje po zakończeniu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9c469-270">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="9c469-271">Odróżnia między <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i tylko wywołuje odpowiedni interfejs.</span><span class="sxs-lookup"><span data-stu-id="9c469-271">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="9c469-272"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>jest używany przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> program do `TOptions` buforowania wystąpień.</span><span class="sxs-lookup"><span data-stu-id="9c469-272"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="9c469-273">Unieważnia wystąpienia opcji w monitorze, aby wartość była ponownie obliczana (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>). <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601></span><span class="sxs-lookup"><span data-stu-id="9c469-273">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="9c469-274">Wartości można wprowadzać ręcznie przy użyciu <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="9c469-274">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="9c469-275">Metoda <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> jest używana, gdy wszystkie nazwane wystąpienia powinny być ponownie tworzone na żądanie.</span><span class="sxs-lookup"><span data-stu-id="9c469-275">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="9c469-276"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>jest przydatne w scenariuszach, w których opcje powinny być ponownie obliczane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="9c469-276"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="9c469-277">Aby uzyskać więcej informacji, zobacz sekcję [Załaduj dane konfiguracji za pomocą IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) .</span><span class="sxs-lookup"><span data-stu-id="9c469-277">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="9c469-278"><xref:Microsoft.Extensions.Options.IOptions%601>może służyć do obsługi opcji.</span><span class="sxs-lookup"><span data-stu-id="9c469-278"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="9c469-279">Program <xref:Microsoft.Extensions.Options.IOptions%601> nie obsługuje jednak powyższych <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="9c469-279">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="9c469-280">Można nadal korzystać <xref:Microsoft.Extensions.Options.IOptions%601> z istniejących platform i bibliotek, które już korzystają z <xref:Microsoft.Extensions.Options.IOptions%601> interfejsu i nie wymagają scenariuszy udostępnianych przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>program.</span><span class="sxs-lookup"><span data-stu-id="9c469-280">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="9c469-281">Konfiguracja opcji ogólnych</span><span class="sxs-lookup"><span data-stu-id="9c469-281">General options configuration</span></span>

<span data-ttu-id="9c469-282">Konfiguracja opcji ogólnych jest przedstawiona jako &num;przykład 1 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="9c469-282">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="9c469-283">Klasa Options musi być nieabstrakcyjna z publicznym konstruktorem bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="9c469-283">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="9c469-284">Następująca Klasa `MyOptions`,, ma dwie właściwości, `Option1` i `Option2`.</span><span class="sxs-lookup"><span data-stu-id="9c469-284">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="9c469-285">Ustawienie wartości domyślnych jest opcjonalne, ale Konstruktor klasy w poniższym przykładzie ustawia wartość `Option1`domyślną.</span><span class="sxs-lookup"><span data-stu-id="9c469-285">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="9c469-286">`Option2`ma ustawioną wartość domyślną, inicjując właściwość bezpośrednio (*modele/opcje. cs*):</span><span class="sxs-lookup"><span data-stu-id="9c469-286">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="9c469-287">Klasa jest dodawana do kontenera usługi z <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> i powiązana z konfiguracją: `MyOptions`</span><span class="sxs-lookup"><span data-stu-id="9c469-287">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="9c469-288">Poniższy model strony używa [iniekcji zależności konstruktora](xref:mvc/controllers/dependency-injection) z <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> programem w celu uzyskania dostępu do ustawień (*stron/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="9c469-288">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="9c469-289">Plik *appSettings. JSON* przykładu Określa wartości dla `option1` i: `option2`</span><span class="sxs-lookup"><span data-stu-id="9c469-289">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="9c469-290">Po uruchomieniu aplikacji `OnGet` Metoda modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="9c469-290">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="9c469-291">W przypadku używania niestandardowej <xref:System.Configuration.ConfigurationBuilder> konfiguracji opcji do ładowania z pliku ustawień upewnij się, że ścieżka bazowa jest ustawiona poprawnie:</span><span class="sxs-lookup"><span data-stu-id="9c469-291">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="9c469-292">Jawne ustawienie ścieżki podstawowej nie jest wymagane podczas ładowania konfiguracji opcji z pliku ustawień za pośrednictwem <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="9c469-292">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="9c469-293">Skonfiguruj proste opcje z delegatem</span><span class="sxs-lookup"><span data-stu-id="9c469-293">Configure simple options with a delegate</span></span>

<span data-ttu-id="9c469-294">Konfigurowanie prostych opcji z delegatem jest zademonstrowane jako przykład &num;2 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9c469-294">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="9c469-295">Użyj delegata, aby ustawić wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="9c469-295">Use a delegate to set options values.</span></span> <span data-ttu-id="9c469-296">Przykładowa aplikacja używa `MyOptionsWithDelegateConfig` klasy (*models/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="9c469-296">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="9c469-297">W poniższym kodzie zostanie dodana druga <xref:Microsoft.Extensions.Options.IConfigureOptions%601> usługa do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="9c469-297">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="9c469-298">Używa delegata do konfigurowania powiązania z `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="9c469-298">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="9c469-299">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="9c469-299">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="9c469-300">Można dodać wielu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9c469-300">You can add multiple configuration providers.</span></span> <span data-ttu-id="9c469-301">Dostawcy konfiguracji są dostępni z pakietów NuGet i są stosowane w kolejności, w jakiej zostały zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="9c469-301">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="9c469-302">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="9c469-302">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="9c469-303">Każde wywołanie <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> <xref:Microsoft.Extensions.Options.IConfigureOptions%601> dodaje usługę do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="9c469-303">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="9c469-304">`Option1` W poprzednim przykładzie wartości i `Option2` są określone w pliku `Option1` *appSettings. JSON*, ale wartości i `Option2` są zastępowane przez skonfigurowany delegat.</span><span class="sxs-lookup"><span data-stu-id="9c469-304">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="9c469-305">Gdy jest włączona więcej niż jedna usługa konfiguracji, ostatnie Źródło konfiguracji określiło *serwer WINS* i ustawi wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9c469-305">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="9c469-306">Po uruchomieniu aplikacji `OnGet` Metoda modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="9c469-306">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="9c469-307">Konfiguracja podopcji</span><span class="sxs-lookup"><span data-stu-id="9c469-307">Suboptions configuration</span></span>

<span data-ttu-id="9c469-308">Konfiguracja podopcji jest przedstawiana jako przykład &num;3 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9c469-308">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="9c469-309">Aplikacje powinny tworzyć klasy opcji, które odnoszą się do określonych grup scenariuszy (klas) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9c469-309">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="9c469-310">Części aplikacji, które wymagają wartości konfiguracyjnych, powinny mieć dostęp tylko do wartości konfiguracyjnych, z których korzystają.</span><span class="sxs-lookup"><span data-stu-id="9c469-310">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="9c469-311">Po powiązaniu opcji powiązań z konfiguracją Każda właściwość w typie opcji jest powiązana z kluczem konfiguracji formularza `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="9c469-311">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="9c469-312">Na przykład `MyOptions.Option1` właściwość jest powiązana z kluczem `Option1`, `option1` który jest odczytywany z właściwości w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="9c469-312">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="9c469-313">W poniższym kodzie, trzecia <xref:Microsoft.Extensions.Options.IConfigureOptions%601> usługa jest dodawana do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="9c469-313">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="9c469-314">Wiąże `MySubOptions` się z sekcją `subsection` pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="9c469-314">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="9c469-315">Metoda rozszerzenia wymaga pakietu NuGet [Microsoft. Extensions. options. ConfigurationExtensions.](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) `GetSection`</span><span class="sxs-lookup"><span data-stu-id="9c469-315">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="9c469-316">Jeśli aplikacja używa [pakietu Microsoft. AspNetCore. appbinding](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 lub nowszego), pakiet jest automatycznie dołączany.</span><span class="sxs-lookup"><span data-stu-id="9c469-316">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="9c469-317">Plik *appSettings. JSON* przykładu definiuje `subsection` element członkowski z kluczami dla `suboption1` i `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="9c469-317">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="9c469-318">Klasa definiuje `SubOption1` właściwości i`SubOption2`, aby przechowywać wartości opcji (*modele/MySubOptions. cs*): `MySubOptions`</span><span class="sxs-lookup"><span data-stu-id="9c469-318">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="9c469-319">`OnGet` Metoda modelu strony zwraca ciąg z wartościami opcji (*Pages/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="9c469-319">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="9c469-320">Gdy aplikacja jest uruchomiona, `OnGet` Metoda zwraca ciąg pokazujący wartości klasy podopcji:</span><span class="sxs-lookup"><span data-stu-id="9c469-320">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="9c469-321">Opcje udostępniane przez model widoku lub bezpośrednie iniekcja widoku</span><span class="sxs-lookup"><span data-stu-id="9c469-321">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="9c469-322">Opcje udostępniane przez model widoku lub bezpośrednie wstrzyknięcie widoku są przedstawiane jako przykład &num;4 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="9c469-322">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="9c469-323">Opcje można dostarczyć w modelu widoku lub poprzez wstrzyknięcie <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> bezpośrednio do widoku (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="9c469-323">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="9c469-324">Przykładowa aplikacja pokazuje, `IOptionsMonitor<MyOptions>` `@inject` jak wstrzyknąć przy użyciu dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="9c469-324">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="9c469-325">Po uruchomieniu aplikacji na renderowanej stronie są wyświetlane wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="9c469-325">When the app is run, the options values are shown in the rendered page:</span></span>

![Wartości opcji opcja1: value1_from_json i Opcja2:-1 są ładowane z modelu i przez iniekcję do widoku.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="9c469-327">Załaduj ponownie dane konfiguracji za pomocą IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="9c469-327">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="9c469-328">Ponowne ładowanie danych konfiguracyjnych przy <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> użyciu programu jest zademonstrowane &num;w przykładzie 5 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9c469-328">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="9c469-329"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>obsługuje opcje ponownego ładowania z minimalnym obciążeniem przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="9c469-329"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="9c469-330">Opcje są obliczane raz dla żądania w przypadku uzyskiwania dostępu do pamięci podręcznej i buforowania jej przez okres istnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="9c469-330">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="9c469-331">W poniższym przykładzie pokazano, jak nowa <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> jest tworzona po wprowadzeniu zmian *appSettings. JSON* (*Pages/index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="9c469-331">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="9c469-332">Wiele żądań do zwracanych przez serwer wartości stałych dostarczonych przez plik *appSettings. JSON* do momentu zmiany pliku i ponownego załadowania konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9c469-332">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="9c469-333">Na poniższej ilustracji przedstawiono początkowe `option1` i `option2` wartości załadowane z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="9c469-333">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="9c469-334">Zmień wartości w pliku *appSettings. JSON* na `value1_from_json UPDATED` i `200`.</span><span class="sxs-lookup"><span data-stu-id="9c469-334">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="9c469-335">Zapisz plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="9c469-335">Save the *appsettings.json* file.</span></span> <span data-ttu-id="9c469-336">Odśwież przeglądarkę, aby zobaczyć, że wartości opcji są aktualizowane:</span><span class="sxs-lookup"><span data-stu-id="9c469-336">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="9c469-337">Obsługa nazwanych opcji w programie IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="9c469-337">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="9c469-338">Pomoc techniczna dotycząca opcji <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> nazwanych w programie jest &num;prezentowana jako przykład 6 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9c469-338">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="9c469-339">Obsługa " *nazwanych opcji* " pozwala aplikacji rozróżnić między nazwanymi konfiguracjami opcji.</span><span class="sxs-lookup"><span data-stu-id="9c469-339">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="9c469-340">W przykładowej aplikacji, nazwane opcje są zadeklarowane za pomocą [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), która [wywołuje\<ConfigureNamedOptions TOptions >. Konfiguruj](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) metodę rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="9c469-340">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="9c469-341">Aplikacja Przykładowa uzyskuje dostęp do nazwanych <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> opcji przy użyciu (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="9c469-341">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="9c469-342">Po uruchomieniu aplikacji przykładowej są zwracane nazwane opcje:</span><span class="sxs-lookup"><span data-stu-id="9c469-342">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="9c469-343">`named_options_1`wartości są dostarczane z konfiguracji, które są ładowane z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="9c469-343">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="9c469-344">`named_options_2`wartości są podawane przez:</span><span class="sxs-lookup"><span data-stu-id="9c469-344">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="9c469-345">Delegat w `ConfigureServices` dla`Option1`. `named_options_2`</span><span class="sxs-lookup"><span data-stu-id="9c469-345">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="9c469-346">Wartość domyślna dla `Option2` dostarczonych `MyOptions` przez klasę.</span><span class="sxs-lookup"><span data-stu-id="9c469-346">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="9c469-347">Skonfiguruj wszystkie opcje przy użyciu metody ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="9c469-347">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="9c469-348">Skonfiguruj wszystkie wystąpienia opcji za pomocą <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> metody.</span><span class="sxs-lookup"><span data-stu-id="9c469-348">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="9c469-349">Poniższy kod konfiguruje `Option1` wszystkie wystąpienia konfiguracji za pomocą wspólnej wartości.</span><span class="sxs-lookup"><span data-stu-id="9c469-349">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="9c469-350">Ręcznie Dodaj następujący kod do `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="9c469-350">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="9c469-351">Uruchomienie przykładowej aplikacji po dodaniu kodu daje następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="9c469-351">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="9c469-352">Wszystkie opcje są nazwanymi wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="9c469-352">All options are named instances.</span></span> <span data-ttu-id="9c469-353">Istniejące <xref:Microsoft.Extensions.Options.IConfigureOptions%601> wystąpienia są traktowane jako docelowe dla `Options.DefaultName` wystąpienia, czyli `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="9c469-353">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="9c469-354"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>implementuje <xref:Microsoft.Extensions.Options.IConfigureOptions%601>także.</span><span class="sxs-lookup"><span data-stu-id="9c469-354"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="9c469-355">Domyślna implementacja <xref:Microsoft.Extensions.Options.IOptionsFactory%601> logiki ma na celu odpowiednie użycie.</span><span class="sxs-lookup"><span data-stu-id="9c469-355">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="9c469-356">Nazwana opcja służy do określania wszystkich wystąpień nazwanych zamiast określonego wystąpienia nazwanego (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> i <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> używania tej Konwencji). `null`</span><span class="sxs-lookup"><span data-stu-id="9c469-356">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="9c469-357">Interfejs API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="9c469-357">OptionsBuilder API</span></span>

<span data-ttu-id="9c469-358"><xref:Microsoft.Extensions.Options.OptionsBuilder%601>służy do konfigurowania `TOptions` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="9c469-358"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="9c469-359">`OptionsBuilder`usprawnia Tworzenie nazwanych opcji, ponieważ jest tylko pojedynczym parametrem do początkowego `AddOptions<TOptions>(string optionsName)` wywołania, a nie pojawia się we wszystkich kolejnych wywołaniach.</span><span class="sxs-lookup"><span data-stu-id="9c469-359">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="9c469-360">Sprawdzanie poprawności opcji i `ConfigureOptions` przeciążenia, które akceptują zależności usługi, są `OptionsBuilder`dostępne tylko za pośrednictwem programu.</span><span class="sxs-lookup"><span data-stu-id="9c469-360">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="9c469-361">Korzystanie z usług DI Services w celu konfigurowania opcji</span><span class="sxs-lookup"><span data-stu-id="9c469-361">Use DI services to configure options</span></span>

<span data-ttu-id="9c469-362">Można uzyskać dostęp do innych usług z iniekcji zależności podczas konfigurowania opcji na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="9c469-362">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="9c469-363">Przekaż delegat konfiguracji w celu [](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) skonfigurowania [na\<OptionsBuilder TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="9c469-363">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="9c469-364">[OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) zapewnia przeciążenia [konfiguracji](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) , które pozwalają na używanie do pięciu usług do konfigurowania opcji:</span><span class="sxs-lookup"><span data-stu-id="9c469-364">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="9c469-365">Utwórz własny typ, który implementuje <xref:Microsoft.Extensions.Options.IConfigureOptions%601> lub <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i rejestruje typ jako usługę.</span><span class="sxs-lookup"><span data-stu-id="9c469-365">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="9c469-366">Zalecamy przekazanie delegata konfiguracji w [](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)celu skonfigurowania, ponieważ tworzenie usługi jest bardziej skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="9c469-366">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="9c469-367">Tworzenie własnego typu jest równoznaczne z tym, co jest używane przez platformę podczas [konfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="9c469-367">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="9c469-368">Wywołanie [konfiguracji](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) rejestruje przejściowe ogólne <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, który ma konstruktora akceptującego określone typy usług ogólnych.</span><span class="sxs-lookup"><span data-stu-id="9c469-368">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="9c469-369">Sprawdzanie poprawności opcji</span><span class="sxs-lookup"><span data-stu-id="9c469-369">Options validation</span></span>

<span data-ttu-id="9c469-370">Sprawdzanie poprawności opcji umożliwia sprawdzenie opcji w przypadku skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="9c469-370">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="9c469-371">Wywołaj `Validate` metodę walidacji zwracającą `true` wartość, jeśli opcje są `false` prawidłowe i jeśli są nieprawidłowe:</span><span class="sxs-lookup"><span data-stu-id="9c469-371">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="9c469-372">Poprzedni przykład ustawia nazwane wystąpienie opcji na `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="9c469-372">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="9c469-373">Domyślne wystąpienie opcji to `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="9c469-373">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="9c469-374">Walidacja jest uruchamiana po utworzeniu wystąpienia opcji.</span><span class="sxs-lookup"><span data-stu-id="9c469-374">Validation runs when the options instance is created.</span></span> <span data-ttu-id="9c469-375">Twoje wystąpienie opcji gwarantuje przekazanie walidacji przy pierwszej próbie dostępu.</span><span class="sxs-lookup"><span data-stu-id="9c469-375">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9c469-376">Sprawdzanie poprawności opcji nie chroni przed modyfikacjami opcji po początkowym skonfigurowaniu opcji i sprawdzeniu poprawności.</span><span class="sxs-lookup"><span data-stu-id="9c469-376">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="9c469-377">`Validate` Metoda`Func<TOptions, bool>`akceptuje.</span><span class="sxs-lookup"><span data-stu-id="9c469-377">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="9c469-378">Aby w pełni dostosować walidację `IValidateOptions<TOptions>`, zaimplementuj, która umożliwia:</span><span class="sxs-lookup"><span data-stu-id="9c469-378">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="9c469-379">Sprawdzanie poprawności wielu typów opcji:`class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="9c469-379">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="9c469-380">Walidacja zależna od innego typu opcji:`public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="9c469-380">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="9c469-381">`IValidateOptions`weryfikuje</span><span class="sxs-lookup"><span data-stu-id="9c469-381">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="9c469-382">Konkretne nazwane wystąpienie opcji.</span><span class="sxs-lookup"><span data-stu-id="9c469-382">A specific named options instance.</span></span>
* <span data-ttu-id="9c469-383">Wszystkie opcje w `name` przypadku `null`.</span><span class="sxs-lookup"><span data-stu-id="9c469-383">All options when `name` is `null`.</span></span>

<span data-ttu-id="9c469-384">`ValidateOptionsResult` Zwróć z implementacji interfejsu:</span><span class="sxs-lookup"><span data-stu-id="9c469-384">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="9c469-385">Sprawdzanie poprawności opartej na adnotacji danych jest dostępne w pakiecie [Microsoft. Extensions. options.](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) DataAnnotations <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> poprzez wywołanie `OptionsBuilder<TOptions>`metody w.</span><span class="sxs-lookup"><span data-stu-id="9c469-385">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="9c469-386">`Microsoft.Extensions.Options.DataAnnotations`jest zawarty w [pakiecie Microsoft. AspNetCore. appbinding](xref:fundamentals/metapackage-app) (ASP.NET Core 2,2 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="9c469-386">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.2 or later).</span></span>

```csharp
using Microsoft.Extensions.DependencyInjection;

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

<span data-ttu-id="9c469-387">Eager sprawdzanie poprawności (niepowodzenie szybkie przy uruchamianiu) jest rozważane dla przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="9c469-387">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="9c469-388">Opcje po konfiguracji</span><span class="sxs-lookup"><span data-stu-id="9c469-388">Options post-configuration</span></span>

<span data-ttu-id="9c469-389">Ustaw wartość po konfiguracji za <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>pomocą.</span><span class="sxs-lookup"><span data-stu-id="9c469-389">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="9c469-390">Po wykonaniu wszystkich <xref:Microsoft.Extensions.Options.IConfigureOptions%601> czynności konfiguracyjnych są wykonywane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="9c469-390">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="9c469-391"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*>jest dostępny do po skonfigurowaniu nazwanych opcji:</span><span class="sxs-lookup"><span data-stu-id="9c469-391"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="9c469-392">Użyj <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> , aby skonfigurować wszystkie wystąpienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="9c469-392">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="9c469-393">Dostęp do opcji podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="9c469-393">Accessing options during startup</span></span>

<span data-ttu-id="9c469-394"><xref:Microsoft.Extensions.Options.IOptions%601>i <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> mogą być używane w `Startup.Configure`, ponieważ `Configure` usługi zostały skompilowane przed wykonaniem metody.</span><span class="sxs-lookup"><span data-stu-id="9c469-394"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="9c469-395">Nie używaj <xref:Microsoft.Extensions.Options.IOptions%601> ani <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9c469-395">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9c469-396">Niespójny stan opcji może istnieć ze względu na kolejność rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="9c469-396">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="9c469-397">Wzorzec opcji używa klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="9c469-397">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="9c469-398">Gdy [Ustawienia konfiguracji](xref:fundamentals/configuration/index) są izolowane według scenariusza w osobnych klasach, aplikacja będzie zgodna z dwoma ważnymi zasadami inżynierii oprogramowania:</span><span class="sxs-lookup"><span data-stu-id="9c469-398">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="9c469-399">[Zasady podziału interfejsu (ISP) lub](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; scenariusze hermetyzacji (klasy), które są zależne od ustawień konfiguracji, zależą tylko od ustawień konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="9c469-399">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="9c469-400">[Rozdzielenie problemów](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Ustawienia dla różnych części aplikacji nie są zależne ani połączone ze sobą.</span><span class="sxs-lookup"><span data-stu-id="9c469-400">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="9c469-401">Opcje umożliwiają również mechanizm weryfikacji danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="9c469-401">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="9c469-402">Aby uzyskać więcej informacji, zobacz sekcję [Opcje walidacji](#options-validation) .</span><span class="sxs-lookup"><span data-stu-id="9c469-402">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="9c469-403">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9c469-403">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c469-404">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="9c469-404">Prerequisites</span></span>

<span data-ttu-id="9c469-405">Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) .</span><span class="sxs-lookup"><span data-stu-id="9c469-405">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="9c469-406">Interfejsy opcji</span><span class="sxs-lookup"><span data-stu-id="9c469-406">Options interfaces</span></span>

<span data-ttu-id="9c469-407"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601>służy do pobierania opcji i zarządzania powiadomieniami `TOptions` o wystąpieniach.</span><span class="sxs-lookup"><span data-stu-id="9c469-407"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="9c469-408"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601>Program obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="9c469-408"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="9c469-409">Powiadomienia o zmianie</span><span class="sxs-lookup"><span data-stu-id="9c469-409">Change notifications</span></span>
* [<span data-ttu-id="9c469-410">Nazwane opcje</span><span class="sxs-lookup"><span data-stu-id="9c469-410">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="9c469-411">Konfiguracja do ponownego ładowania</span><span class="sxs-lookup"><span data-stu-id="9c469-411">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="9c469-412">Unieważnianie opcji selektywnych (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="9c469-412">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="9c469-413">Scenariusze [po konfiguracji](#options-post-configuration) umożliwiają ustawianie lub zmienianie opcji po wystąpieniu całej <xref:Microsoft.Extensions.Options.IConfigureOptions%601> konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9c469-413">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="9c469-414"><xref:Microsoft.Extensions.Options.IOptionsFactory%601>jest odpowiedzialny za tworzenie nowych wystąpień opcji.</span><span class="sxs-lookup"><span data-stu-id="9c469-414"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="9c469-415">Ma jedną <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> metodę.</span><span class="sxs-lookup"><span data-stu-id="9c469-415">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="9c469-416">Domyślna implementacja przyjmuje wszystkie zarejestrowane <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> uruchamia najpierw wszystkie konfiguracje, po których następuje po zakończeniu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9c469-416">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="9c469-417">Odróżnia między <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i tylko wywołuje odpowiedni interfejs.</span><span class="sxs-lookup"><span data-stu-id="9c469-417">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="9c469-418"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>jest używany przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> program do `TOptions` buforowania wystąpień.</span><span class="sxs-lookup"><span data-stu-id="9c469-418"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="9c469-419">Unieważnia wystąpienia opcji w monitorze, aby wartość była ponownie obliczana (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>). <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601></span><span class="sxs-lookup"><span data-stu-id="9c469-419">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="9c469-420">Wartości można wprowadzać ręcznie przy użyciu <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="9c469-420">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="9c469-421">Metoda <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> jest używana, gdy wszystkie nazwane wystąpienia powinny być ponownie tworzone na żądanie.</span><span class="sxs-lookup"><span data-stu-id="9c469-421">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="9c469-422"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>jest przydatne w scenariuszach, w których opcje powinny być ponownie obliczane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="9c469-422"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="9c469-423">Aby uzyskać więcej informacji, zobacz sekcję [Załaduj dane konfiguracji za pomocą IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) .</span><span class="sxs-lookup"><span data-stu-id="9c469-423">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="9c469-424"><xref:Microsoft.Extensions.Options.IOptions%601>może służyć do obsługi opcji.</span><span class="sxs-lookup"><span data-stu-id="9c469-424"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="9c469-425">Program <xref:Microsoft.Extensions.Options.IOptions%601> nie obsługuje jednak powyższych <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="9c469-425">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="9c469-426">Można nadal korzystać <xref:Microsoft.Extensions.Options.IOptions%601> z istniejących platform i bibliotek, które już korzystają z <xref:Microsoft.Extensions.Options.IOptions%601> interfejsu i nie wymagają scenariuszy udostępnianych przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>program.</span><span class="sxs-lookup"><span data-stu-id="9c469-426">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="9c469-427">Konfiguracja opcji ogólnych</span><span class="sxs-lookup"><span data-stu-id="9c469-427">General options configuration</span></span>

<span data-ttu-id="9c469-428">Konfiguracja opcji ogólnych jest przedstawiona jako &num;przykład 1 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="9c469-428">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="9c469-429">Klasa Options musi być nieabstrakcyjna z publicznym konstruktorem bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="9c469-429">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="9c469-430">Następująca Klasa `MyOptions`,, ma dwie właściwości, `Option1` i `Option2`.</span><span class="sxs-lookup"><span data-stu-id="9c469-430">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="9c469-431">Ustawienie wartości domyślnych jest opcjonalne, ale Konstruktor klasy w poniższym przykładzie ustawia wartość `Option1`domyślną.</span><span class="sxs-lookup"><span data-stu-id="9c469-431">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="9c469-432">`Option2`ma ustawioną wartość domyślną, inicjując właściwość bezpośrednio (*modele/opcje. cs*):</span><span class="sxs-lookup"><span data-stu-id="9c469-432">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="9c469-433">Klasa jest dodawana do kontenera usługi z <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> i powiązana z konfiguracją: `MyOptions`</span><span class="sxs-lookup"><span data-stu-id="9c469-433">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="9c469-434">Poniższy model strony używa [iniekcji zależności konstruktora](xref:mvc/controllers/dependency-injection) z <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> programem w celu uzyskania dostępu do ustawień (*stron/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="9c469-434">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="9c469-435">Plik *appSettings. JSON* przykładu Określa wartości dla `option1` i: `option2`</span><span class="sxs-lookup"><span data-stu-id="9c469-435">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="9c469-436">Po uruchomieniu aplikacji `OnGet` Metoda modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="9c469-436">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="9c469-437">W przypadku używania niestandardowej <xref:System.Configuration.ConfigurationBuilder> konfiguracji opcji do ładowania z pliku ustawień upewnij się, że ścieżka bazowa jest ustawiona poprawnie:</span><span class="sxs-lookup"><span data-stu-id="9c469-437">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="9c469-438">Jawne ustawienie ścieżki podstawowej nie jest wymagane podczas ładowania konfiguracji opcji z pliku ustawień za pośrednictwem <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="9c469-438">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="9c469-439">Skonfiguruj proste opcje z delegatem</span><span class="sxs-lookup"><span data-stu-id="9c469-439">Configure simple options with a delegate</span></span>

<span data-ttu-id="9c469-440">Konfigurowanie prostych opcji z delegatem jest zademonstrowane jako przykład &num;2 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9c469-440">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="9c469-441">Użyj delegata, aby ustawić wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="9c469-441">Use a delegate to set options values.</span></span> <span data-ttu-id="9c469-442">Przykładowa aplikacja używa `MyOptionsWithDelegateConfig` klasy (*models/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="9c469-442">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="9c469-443">W poniższym kodzie zostanie dodana druga <xref:Microsoft.Extensions.Options.IConfigureOptions%601> usługa do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="9c469-443">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="9c469-444">Używa delegata do konfigurowania powiązania z `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="9c469-444">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="9c469-445">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="9c469-445">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="9c469-446">Można dodać wielu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9c469-446">You can add multiple configuration providers.</span></span> <span data-ttu-id="9c469-447">Dostawcy konfiguracji są dostępni z pakietów NuGet i są stosowane w kolejności, w jakiej zostały zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="9c469-447">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="9c469-448">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="9c469-448">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="9c469-449">Każde wywołanie <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> <xref:Microsoft.Extensions.Options.IConfigureOptions%601> dodaje usługę do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="9c469-449">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="9c469-450">`Option1` W poprzednim przykładzie wartości i `Option2` są określone w pliku `Option1` *appSettings. JSON*, ale wartości i `Option2` są zastępowane przez skonfigurowany delegat.</span><span class="sxs-lookup"><span data-stu-id="9c469-450">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="9c469-451">Gdy jest włączona więcej niż jedna usługa konfiguracji, ostatnie Źródło konfiguracji określiło *serwer WINS* i ustawi wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9c469-451">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="9c469-452">Po uruchomieniu aplikacji `OnGet` Metoda modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="9c469-452">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="9c469-453">Konfiguracja podopcji</span><span class="sxs-lookup"><span data-stu-id="9c469-453">Suboptions configuration</span></span>

<span data-ttu-id="9c469-454">Konfiguracja podopcji jest przedstawiana jako przykład &num;3 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9c469-454">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="9c469-455">Aplikacje powinny tworzyć klasy opcji, które odnoszą się do określonych grup scenariuszy (klas) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9c469-455">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="9c469-456">Części aplikacji, które wymagają wartości konfiguracyjnych, powinny mieć dostęp tylko do wartości konfiguracyjnych, z których korzystają.</span><span class="sxs-lookup"><span data-stu-id="9c469-456">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="9c469-457">Po powiązaniu opcji powiązań z konfiguracją Każda właściwość w typie opcji jest powiązana z kluczem konfiguracji formularza `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="9c469-457">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="9c469-458">Na przykład `MyOptions.Option1` właściwość jest powiązana z kluczem `Option1`, `option1` który jest odczytywany z właściwości w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="9c469-458">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="9c469-459">W poniższym kodzie, trzecia <xref:Microsoft.Extensions.Options.IConfigureOptions%601> usługa jest dodawana do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="9c469-459">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="9c469-460">Wiąże `MySubOptions` się z sekcją `subsection` pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="9c469-460">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="9c469-461">Metoda rozszerzenia wymaga pakietu NuGet [Microsoft. Extensions. options. ConfigurationExtensions.](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) `GetSection`</span><span class="sxs-lookup"><span data-stu-id="9c469-461">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="9c469-462">Jeśli aplikacja używa [pakietu Microsoft. AspNetCore. appbinding](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 lub nowszego), pakiet jest automatycznie dołączany.</span><span class="sxs-lookup"><span data-stu-id="9c469-462">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="9c469-463">Plik *appSettings. JSON* przykładu definiuje `subsection` element członkowski z kluczami dla `suboption1` i `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="9c469-463">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="9c469-464">Klasa definiuje `SubOption1` właściwości i`SubOption2`, aby przechowywać wartości opcji (*modele/MySubOptions. cs*): `MySubOptions`</span><span class="sxs-lookup"><span data-stu-id="9c469-464">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="9c469-465">`OnGet` Metoda modelu strony zwraca ciąg z wartościami opcji (*Pages/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="9c469-465">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="9c469-466">Gdy aplikacja jest uruchomiona, `OnGet` Metoda zwraca ciąg pokazujący wartości klasy podopcji:</span><span class="sxs-lookup"><span data-stu-id="9c469-466">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="9c469-467">Opcje udostępniane przez model widoku lub bezpośrednie iniekcja widoku</span><span class="sxs-lookup"><span data-stu-id="9c469-467">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="9c469-468">Opcje udostępniane przez model widoku lub bezpośrednie wstrzyknięcie widoku są przedstawiane jako przykład &num;4 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="9c469-468">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="9c469-469">Opcje można dostarczyć w modelu widoku lub poprzez wstrzyknięcie <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> bezpośrednio do widoku (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="9c469-469">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="9c469-470">Przykładowa aplikacja pokazuje, `IOptionsMonitor<MyOptions>` `@inject` jak wstrzyknąć przy użyciu dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="9c469-470">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="9c469-471">Po uruchomieniu aplikacji na renderowanej stronie są wyświetlane wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="9c469-471">When the app is run, the options values are shown in the rendered page:</span></span>

![Wartości opcji opcja1: value1_from_json i Opcja2:-1 są ładowane z modelu i przez iniekcję do widoku.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="9c469-473">Załaduj ponownie dane konfiguracji za pomocą IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="9c469-473">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="9c469-474">Ponowne ładowanie danych konfiguracyjnych przy <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> użyciu programu jest zademonstrowane &num;w przykładzie 5 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9c469-474">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="9c469-475"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>obsługuje opcje ponownego ładowania z minimalnym obciążeniem przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="9c469-475"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="9c469-476">Opcje są obliczane raz dla żądania w przypadku uzyskiwania dostępu do pamięci podręcznej i buforowania jej przez okres istnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="9c469-476">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="9c469-477">W poniższym przykładzie pokazano, jak nowa <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> jest tworzona po wprowadzeniu zmian *appSettings. JSON* (*Pages/index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="9c469-477">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="9c469-478">Wiele żądań do zwracanych przez serwer wartości stałych dostarczonych przez plik *appSettings. JSON* do momentu zmiany pliku i ponownego załadowania konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9c469-478">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="9c469-479">Na poniższej ilustracji przedstawiono początkowe `option1` i `option2` wartości załadowane z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="9c469-479">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="9c469-480">Zmień wartości w pliku *appSettings. JSON* na `value1_from_json UPDATED` i `200`.</span><span class="sxs-lookup"><span data-stu-id="9c469-480">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="9c469-481">Zapisz plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="9c469-481">Save the *appsettings.json* file.</span></span> <span data-ttu-id="9c469-482">Odśwież przeglądarkę, aby zobaczyć, że wartości opcji są aktualizowane:</span><span class="sxs-lookup"><span data-stu-id="9c469-482">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="9c469-483">Obsługa nazwanych opcji w programie IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="9c469-483">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="9c469-484">Pomoc techniczna dotycząca opcji <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> nazwanych w programie jest &num;prezentowana jako przykład 6 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9c469-484">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="9c469-485">Obsługa " *nazwanych opcji* " pozwala aplikacji rozróżnić między nazwanymi konfiguracjami opcji.</span><span class="sxs-lookup"><span data-stu-id="9c469-485">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="9c469-486">W przykładowej aplikacji, nazwane opcje są zadeklarowane za pomocą [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), która [wywołuje\<ConfigureNamedOptions TOptions >. Konfiguruj](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) metodę rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="9c469-486">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="9c469-487">Aplikacja Przykładowa uzyskuje dostęp do nazwanych <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> opcji przy użyciu (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="9c469-487">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="9c469-488">Po uruchomieniu aplikacji przykładowej są zwracane nazwane opcje:</span><span class="sxs-lookup"><span data-stu-id="9c469-488">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="9c469-489">`named_options_1`wartości są dostarczane z konfiguracji, które są ładowane z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="9c469-489">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="9c469-490">`named_options_2`wartości są podawane przez:</span><span class="sxs-lookup"><span data-stu-id="9c469-490">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="9c469-491">Delegat w `ConfigureServices` dla`Option1`. `named_options_2`</span><span class="sxs-lookup"><span data-stu-id="9c469-491">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="9c469-492">Wartość domyślna dla `Option2` dostarczonych `MyOptions` przez klasę.</span><span class="sxs-lookup"><span data-stu-id="9c469-492">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="9c469-493">Skonfiguruj wszystkie opcje przy użyciu metody ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="9c469-493">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="9c469-494">Skonfiguruj wszystkie wystąpienia opcji za pomocą <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> metody.</span><span class="sxs-lookup"><span data-stu-id="9c469-494">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="9c469-495">Poniższy kod konfiguruje `Option1` wszystkie wystąpienia konfiguracji za pomocą wspólnej wartości.</span><span class="sxs-lookup"><span data-stu-id="9c469-495">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="9c469-496">Ręcznie Dodaj następujący kod do `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="9c469-496">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="9c469-497">Uruchomienie przykładowej aplikacji po dodaniu kodu daje następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="9c469-497">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="9c469-498">Wszystkie opcje są nazwanymi wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="9c469-498">All options are named instances.</span></span> <span data-ttu-id="9c469-499">Istniejące <xref:Microsoft.Extensions.Options.IConfigureOptions%601> wystąpienia są traktowane jako docelowe dla `Options.DefaultName` wystąpienia, czyli `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="9c469-499">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="9c469-500"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>implementuje <xref:Microsoft.Extensions.Options.IConfigureOptions%601>także.</span><span class="sxs-lookup"><span data-stu-id="9c469-500"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="9c469-501">Domyślna implementacja <xref:Microsoft.Extensions.Options.IOptionsFactory%601> logiki ma na celu odpowiednie użycie.</span><span class="sxs-lookup"><span data-stu-id="9c469-501">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="9c469-502">Nazwana opcja służy do określania wszystkich wystąpień nazwanych zamiast określonego wystąpienia nazwanego (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> i <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> używania tej Konwencji). `null`</span><span class="sxs-lookup"><span data-stu-id="9c469-502">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="9c469-503">Interfejs API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="9c469-503">OptionsBuilder API</span></span>

<span data-ttu-id="9c469-504"><xref:Microsoft.Extensions.Options.OptionsBuilder%601>służy do konfigurowania `TOptions` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="9c469-504"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="9c469-505">`OptionsBuilder`usprawnia Tworzenie nazwanych opcji, ponieważ jest tylko pojedynczym parametrem do początkowego `AddOptions<TOptions>(string optionsName)` wywołania, a nie pojawia się we wszystkich kolejnych wywołaniach.</span><span class="sxs-lookup"><span data-stu-id="9c469-505">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="9c469-506">Sprawdzanie poprawności opcji i `ConfigureOptions` przeciążenia, które akceptują zależności usługi, są `OptionsBuilder`dostępne tylko za pośrednictwem programu.</span><span class="sxs-lookup"><span data-stu-id="9c469-506">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="9c469-507">Korzystanie z usług DI Services w celu konfigurowania opcji</span><span class="sxs-lookup"><span data-stu-id="9c469-507">Use DI services to configure options</span></span>

<span data-ttu-id="9c469-508">Można uzyskać dostęp do innych usług z iniekcji zależności podczas konfigurowania opcji na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="9c469-508">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="9c469-509">Przekaż delegat konfiguracji w celu [](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) skonfigurowania [na\<OptionsBuilder TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="9c469-509">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="9c469-510">[OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) zapewnia przeciążenia [konfiguracji](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) , które pozwalają na używanie do pięciu usług do konfigurowania opcji:</span><span class="sxs-lookup"><span data-stu-id="9c469-510">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="9c469-511">Utwórz własny typ, który implementuje <xref:Microsoft.Extensions.Options.IConfigureOptions%601> lub <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i rejestruje typ jako usługę.</span><span class="sxs-lookup"><span data-stu-id="9c469-511">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="9c469-512">Zalecamy przekazanie delegata konfiguracji w [](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*)celu skonfigurowania, ponieważ tworzenie usługi jest bardziej skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="9c469-512">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="9c469-513">Tworzenie własnego typu jest równoznaczne z tym, co jest używane przez platformę podczas [konfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="9c469-513">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="9c469-514">Wywołanie [konfiguracji](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) rejestruje przejściowe ogólne <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, który ma konstruktora akceptującego określone typy usług ogólnych.</span><span class="sxs-lookup"><span data-stu-id="9c469-514">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-post-configuration"></a><span data-ttu-id="9c469-515">Opcje po konfiguracji</span><span class="sxs-lookup"><span data-stu-id="9c469-515">Options post-configuration</span></span>

<span data-ttu-id="9c469-516">Ustaw wartość po konfiguracji za <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>pomocą.</span><span class="sxs-lookup"><span data-stu-id="9c469-516">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="9c469-517">Po wykonaniu wszystkich <xref:Microsoft.Extensions.Options.IConfigureOptions%601> czynności konfiguracyjnych są wykonywane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="9c469-517">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="9c469-518"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*>jest dostępny do po skonfigurowaniu nazwanych opcji:</span><span class="sxs-lookup"><span data-stu-id="9c469-518"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="9c469-519">Użyj <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> , aby skonfigurować wszystkie wystąpienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="9c469-519">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="9c469-520">Dostęp do opcji podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="9c469-520">Accessing options during startup</span></span>

<span data-ttu-id="9c469-521"><xref:Microsoft.Extensions.Options.IOptions%601>i <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> mogą być używane w `Startup.Configure`, ponieważ `Configure` usługi zostały skompilowane przed wykonaniem metody.</span><span class="sxs-lookup"><span data-stu-id="9c469-521"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="9c469-522">Nie używaj <xref:Microsoft.Extensions.Options.IOptions%601> ani <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9c469-522">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9c469-523">Niespójny stan opcji może istnieć ze względu na kolejność rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="9c469-523">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="9c469-524">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9c469-524">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
