---
title: Wzorzec opcji w ASP.NET Core
author: guardrex
description: Dowiedz się, jak używać wzorca opcji do reprezentowania grup powiązanych ustawień w aplikacjach ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/07/2019
uid: fundamentals/configuration/options
ms.openlocfilehash: 98fe30fbc424dd51ce8f8319b7ce959fd755c480
ms.sourcegitcommit: da2fb2d78ce70accdba903ccbfdcfffdd0112123
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/07/2020
ms.locfileid: "75722742"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="5a83f-103">Wzorzec opcji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5a83f-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="5a83f-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5a83f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5a83f-105">Wzorzec opcji używa klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="5a83f-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="5a83f-106">Gdy [Ustawienia konfiguracji](xref:fundamentals/configuration/index) są izolowane według scenariusza w osobnych klasach, aplikacja będzie zgodna z dwoma ważnymi zasadami inżynierii oprogramowania:</span><span class="sxs-lookup"><span data-stu-id="5a83f-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="5a83f-107">[Zasady podziału interfejsu (ISP) lub &ndash; hermetyzacji](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) (klasy), które są zależne od ustawień konfiguracji, zależą tylko od ustawień konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="5a83f-107">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="5a83f-108">[Separacja problemów](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; ustawień różnych części aplikacji nie zależy od siebie ani nie jest połączona.</span><span class="sxs-lookup"><span data-stu-id="5a83f-108">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="5a83f-109">Opcje umożliwiają również mechanizm weryfikacji danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="5a83f-109">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="5a83f-110">Aby uzyskać więcej informacji, zobacz sekcję [Opcje walidacji](#options-validation) .</span><span class="sxs-lookup"><span data-stu-id="5a83f-110">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="5a83f-111">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5a83f-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="5a83f-112">Package</span><span class="sxs-lookup"><span data-stu-id="5a83f-112">Package</span></span>

<span data-ttu-id="5a83f-113">Pakiet [Microsoft. Extensions. options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) jest niejawnie przywoływany w aplikacjach ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5a83f-113">The [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package is implicitly referenced in ASP.NET Core apps.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="5a83f-114">Interfejsy opcji</span><span class="sxs-lookup"><span data-stu-id="5a83f-114">Options interfaces</span></span>

<span data-ttu-id="5a83f-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> służy do pobierania opcji i zarządzania powiadomieniami o opcjach dla wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="5a83f-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="5a83f-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="5a83f-117">Powiadomienia o zmianie</span><span class="sxs-lookup"><span data-stu-id="5a83f-117">Change notifications</span></span>
* [<span data-ttu-id="5a83f-118">Nazwane opcje</span><span class="sxs-lookup"><span data-stu-id="5a83f-118">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="5a83f-119">Konfiguracja do ponownego ładowania</span><span class="sxs-lookup"><span data-stu-id="5a83f-119">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="5a83f-120">Unieważnianie opcji selektywnych (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="5a83f-120">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="5a83f-121">Scenariusze [po konfiguracji](#options-post-configuration) umożliwiają ustawianie lub zmienianie opcji po wystąpieniu całej konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-121">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="5a83f-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> jest odpowiedzialny za tworzenie nowych wystąpień opcji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="5a83f-123">Ma ona jedną <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> metodę.</span><span class="sxs-lookup"><span data-stu-id="5a83f-123">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="5a83f-124">Domyślna implementacja pobiera wszystkie zarejestrowane <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> i uruchamia najpierw wszystkie konfiguracje, po których następuje po zakończeniu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-124">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="5a83f-125">Odróżnia między <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i wywołuje tylko odpowiedni interfejs.</span><span class="sxs-lookup"><span data-stu-id="5a83f-125">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="5a83f-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> jest używana przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do buforowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="5a83f-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> unieważnia wystąpienia opcji na monitorze, aby wartość była ponownie obliczana (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="5a83f-127">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="5a83f-128">Wartości można wprowadzać ręcznie przy użyciu <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-128">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="5a83f-129">Metoda <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> jest używana, gdy wszystkie nazwane wystąpienia powinny być ponownie tworzone na żądanie.</span><span class="sxs-lookup"><span data-stu-id="5a83f-129">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="5a83f-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> jest przydatne w scenariuszach, w których opcje powinny być ponownie obliczane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="5a83f-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="5a83f-131">Aby uzyskać więcej informacji, zobacz sekcję [Załaduj dane konfiguracji za pomocą IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) .</span><span class="sxs-lookup"><span data-stu-id="5a83f-131">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="5a83f-132"><xref:Microsoft.Extensions.Options.IOptions%601> może służyć do obsługi opcji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-132"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="5a83f-133">Jednak <xref:Microsoft.Extensions.Options.IOptions%601> nie obsługuje powyższych scenariuszy <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-133">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="5a83f-134">Można nadal używać <xref:Microsoft.Extensions.Options.IOptions%601> w istniejących strukturach i bibliotekach, które już używają interfejsu <xref:Microsoft.Extensions.Options.IOptions%601> i nie wymagają scenariuszy udostępnianych przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-134">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="5a83f-135">Konfiguracja opcji ogólnych</span><span class="sxs-lookup"><span data-stu-id="5a83f-135">General options configuration</span></span>

<span data-ttu-id="5a83f-136">Konfiguracja opcji ogólnych jest przedstawiona jako przykład 1 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="5a83f-136">General options configuration is demonstrated as Example 1 in the sample app.</span></span>

<span data-ttu-id="5a83f-137">Klasa Options musi być nieabstrakcyjna z publicznym konstruktorem bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="5a83f-137">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="5a83f-138">Następująca Klasa `MyOptions`ma dwie właściwości, `Option1` i `Option2`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-138">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="5a83f-139">Ustawienie wartości domyślnych jest opcjonalne, ale Konstruktor klasy w poniższym przykładzie ustawia wartość domyślną `Option1`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-139">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="5a83f-140">`Option2` ma ustawioną wartość domyślną, inicjując właściwość bezpośrednio (*modele/opcje. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-140">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="5a83f-141">Klasa `MyOptions` zostanie dodana do kontenera usługi z <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> i powiązana z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="5a83f-141">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="5a83f-142">Poniższy model strony używa [iniekcji zależności konstruktora](xref:mvc/controllers/dependency-injection) z <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, aby uzyskać dostęp do ustawień (*stron/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-142">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="5a83f-143">Plik *appSettings. JSON* przykładu Określa wartości dla `option1` i `option2`:</span><span class="sxs-lookup"><span data-stu-id="5a83f-143">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="5a83f-144">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="5a83f-144">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="5a83f-145">W przypadku używania <xref:System.Configuration.ConfigurationBuilder> niestandardowego do załadowania konfiguracji opcji z pliku ustawień upewnij się, że ścieżka bazowa jest ustawiona poprawnie:</span><span class="sxs-lookup"><span data-stu-id="5a83f-145">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="5a83f-146">Jawne ustawienie ścieżki podstawowej nie jest wymagane podczas ładowania konfiguracji opcji z pliku ustawień za pośrednictwem <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-146">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="5a83f-147">Skonfiguruj proste opcje z delegatem</span><span class="sxs-lookup"><span data-stu-id="5a83f-147">Configure simple options with a delegate</span></span>

<span data-ttu-id="5a83f-148">Konfigurowanie prostych opcji z delegatem jest zademonstrowane jako przykład 2 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-148">Configuring simple options with a delegate is demonstrated as Example 2 in the sample app.</span></span>

<span data-ttu-id="5a83f-149">Użyj delegata, aby ustawić wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-149">Use a delegate to set options values.</span></span> <span data-ttu-id="5a83f-150">Przykładowa aplikacja używa klasy `MyOptionsWithDelegateConfig` (*modele/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-150">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="5a83f-151">W poniższym kodzie do kontenera usługi jest dodawana druga usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-151">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="5a83f-152">Używa delegata do konfigurowania powiązania z `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="5a83f-152">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="5a83f-153">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="5a83f-153">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="5a83f-154">Można dodać wielu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-154">You can add multiple configuration providers.</span></span> <span data-ttu-id="5a83f-155">Dostawcy konfiguracji są dostępni z pakietów NuGet i są stosowane w kolejności, w jakiej zostały zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="5a83f-155">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="5a83f-156">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-156">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="5a83f-157">Każde wywołanie <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> dodaje usługę <xref:Microsoft.Extensions.Options.IConfigureOptions%601> do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="5a83f-157">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="5a83f-158">W poprzednim przykładzie wartości `Option1` i `Option2` są określone w pliku *appSettings. JSON*, ale wartości `Option1` i `Option2` są przesłonięte przez skonfigurowany delegat.</span><span class="sxs-lookup"><span data-stu-id="5a83f-158">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="5a83f-159">Gdy jest włączona więcej niż jedna usługa konfiguracji, ostatnie Źródło konfiguracji określiło *serwer WINS* i ustawi wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-159">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="5a83f-160">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="5a83f-160">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="5a83f-161">Konfiguracja podopcji</span><span class="sxs-lookup"><span data-stu-id="5a83f-161">Suboptions configuration</span></span>

<span data-ttu-id="5a83f-162">Konfiguracja podopcji jest przedstawiana jako przykład 3 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-162">Suboptions configuration is demonstrated as Example 3 in the sample app.</span></span>

<span data-ttu-id="5a83f-163">Aplikacje powinny tworzyć klasy opcji, które odnoszą się do określonych grup scenariuszy (klas) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-163">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="5a83f-164">Części aplikacji, które wymagają wartości konfiguracyjnych, powinny mieć dostęp tylko do wartości konfiguracyjnych, z których korzystają.</span><span class="sxs-lookup"><span data-stu-id="5a83f-164">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="5a83f-165">Po powiązaniu opcji powiązań z konfiguracją Każda właściwość w typie opcji jest powiązana z kluczem konfiguracji formularza `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-165">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="5a83f-166">Na przykład właściwość `MyOptions.Option1` jest powiązana z `Option1`klucza, który jest odczytywany z właściwości `option1` w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="5a83f-166">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="5a83f-167">W poniższym kodzie do kontenera usługi zostanie dodana trzecia usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-167">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="5a83f-168">Wiąże `MySubOptions` z sekcją `subsection` pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="5a83f-168">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="5a83f-169">Metoda `GetSection` wymaga przestrzeni nazw <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-169">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="5a83f-170">Plik *appSettings. JSON* przykładu definiuje `subsection` składową z kluczami dla `suboption1` i `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="5a83f-170">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="5a83f-171">Klasa `MySubOptions` definiuje właściwości, `SubOption1` i `SubOption2`do przechowywania wartości opcji (*modele/MySubOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-171">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="5a83f-172">Metoda `OnGet` modelu strony zwraca ciąg z wartościami opcji (*Pages/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-172">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="5a83f-173">Po uruchomieniu aplikacji Metoda `OnGet` zwraca ciąg pokazujący wartości klasy podopcji:</span><span class="sxs-lookup"><span data-stu-id="5a83f-173">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="5a83f-174">Iniekcja opcji</span><span class="sxs-lookup"><span data-stu-id="5a83f-174">Options injection</span></span>

<span data-ttu-id="5a83f-175">Iniekcja opcji jest przedstawiana jako przykład 4 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-175">Options injection is demonstrated as Example 4 in the sample app.</span></span>

<span data-ttu-id="5a83f-176">Wsuń <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do:</span><span class="sxs-lookup"><span data-stu-id="5a83f-176">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="5a83f-177">Strona Razor lub widok MVC z dyrektywą [`@inject`](xref:mvc/views/razor#inject) Razor.</span><span class="sxs-lookup"><span data-stu-id="5a83f-177">A Razor page or MVC view with the [`@inject`](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="5a83f-178">Model strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="5a83f-178">A page or view model.</span></span>

<span data-ttu-id="5a83f-179">Poniższy przykład z przykładowej aplikacji wprowadza <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do modelu strony (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-179">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="5a83f-180">Przykładowa aplikacja pokazuje, jak wstrzyknąć `IOptionsMonitor<MyOptions>` za pomocą dyrektywy `@inject`:</span><span class="sxs-lookup"><span data-stu-id="5a83f-180">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/3.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="5a83f-181">Po uruchomieniu aplikacji na renderowanej stronie są wyświetlane wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="5a83f-181">When the app is run, the options values are shown in the rendered page:</span></span>

![Wartości opcji opcja1: value1_from_json i Opcja2:-1 są ładowane z modelu i przez iniekcję do widoku.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="5a83f-183">Załaduj ponownie dane konfiguracji za pomocą IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="5a83f-183">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="5a83f-184">Ponowne ładowanie danych konfiguracyjnych za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> przedstawiono w przykładzie 5 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="5a83f-184">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example 5 in the sample app.</span></span>

<span data-ttu-id="5a83f-185">Przy użyciu <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>opcje są obliczane raz dla każdego żądania, gdy jest on używany i buforowany przez okres istnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="5a83f-185">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="5a83f-186">Różnica między `IOptionsMonitor` i `IOptionsSnapshot` to:</span><span class="sxs-lookup"><span data-stu-id="5a83f-186">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="5a83f-187">`IOptionsMonitor` to [Pojedyncza usługa](xref:fundamentals/dependency-injection#singleton) , która pobiera bieżące wartości opcji w dowolnym momencie, która jest szczególnie przydatna w pojedynczych zależnościach.</span><span class="sxs-lookup"><span data-stu-id="5a83f-187">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="5a83f-188">`IOptionsSnapshot` jest [usługą objętą zakresem](xref:fundamentals/dependency-injection#scoped) i zawiera migawkę opcji w czasie konstruowania obiektu `IOptionsSnapshot<T>`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-188">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="5a83f-189">Migawki opcji są przeznaczone do użycia z zależnościami przejściowymi i zakresowymi.</span><span class="sxs-lookup"><span data-stu-id="5a83f-189">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="5a83f-190">Poniższy przykład ilustruje sposób tworzenia nowego <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> po wprowadzeniu zmian w pliku *appSettings. JSON* (*Pages/index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="5a83f-190">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="5a83f-191">Wiele żądań do zwracanych przez serwer wartości stałych dostarczonych przez plik *appSettings. JSON* do momentu zmiany pliku i ponownego załadowania konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-191">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="5a83f-192">Na poniższej ilustracji przedstawiono początkowe `option1` i `option2` wartości załadowane z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="5a83f-192">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="5a83f-193">Zmień wartości w pliku *appSettings. JSON* na `value1_from_json UPDATED` i `200`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-193">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="5a83f-194">Zapisz plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="5a83f-194">Save the *appsettings.json* file.</span></span> <span data-ttu-id="5a83f-195">Odśwież przeglądarkę, aby zobaczyć, że wartości opcji są aktualizowane:</span><span class="sxs-lookup"><span data-stu-id="5a83f-195">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="5a83f-196">Obsługa nazwanych opcji w programie IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="5a83f-196">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="5a83f-197">Obsługa nazwanych opcji w <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> jest przedstawiana jako przykład 6 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="5a83f-197">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example 6 in the sample app.</span></span>

<span data-ttu-id="5a83f-198">Obsługa "nazwanych opcji" pozwala aplikacji rozróżnić między nazwanymi konfiguracjami opcji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-198">Named options support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="5a83f-199">W przykładowej aplikacji, nazwane opcje są zadeklarowane za pomocą [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), która wywołuje [ConfigureNamedOptions\<TOptions >. Skonfiguruj](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) metodę rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="5a83f-199">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method.</span></span> <span data-ttu-id="5a83f-200">W nazwanych opcjach jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="5a83f-200">Named options are case sensitive.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="5a83f-201">Aplikacja Przykładowa uzyskuje dostęp do nazwanych opcji za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-201">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="5a83f-202">Po uruchomieniu aplikacji przykładowej są zwracane nazwane opcje:</span><span class="sxs-lookup"><span data-stu-id="5a83f-202">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="5a83f-203">wartości `named_options_1` są dostarczane z konfiguracji, które są ładowane z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="5a83f-203">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="5a83f-204">wartości `named_options_2` są udostępniane przez:</span><span class="sxs-lookup"><span data-stu-id="5a83f-204">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="5a83f-205">Delegat `named_options_2` w `ConfigureServices` dla `Option1`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-205">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="5a83f-206">Wartość domyślna dla `Option2` dostarczonej przez klasę `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-206">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="5a83f-207">Skonfiguruj wszystkie opcje przy użyciu metody ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="5a83f-207">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="5a83f-208">Skonfiguruj wszystkie wystąpienia opcji za pomocą metody <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-208">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="5a83f-209">Poniższy kod konfiguruje `Option1` dla wszystkich wystąpień konfiguracji z wspólną wartością.</span><span class="sxs-lookup"><span data-stu-id="5a83f-209">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="5a83f-210">Ręcznie Dodaj następujący kod do metody `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5a83f-210">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="5a83f-211">Uruchomienie przykładowej aplikacji po dodaniu kodu daje następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="5a83f-211">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="5a83f-212">Wszystkie opcje są nazwanymi wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="5a83f-212">All options are named instances.</span></span> <span data-ttu-id="5a83f-213">Istniejące wystąpienia <xref:Microsoft.Extensions.Options.IConfigureOptions%601> są traktowane jako ukierunkowane na wystąpienie `Options.DefaultName`, które jest `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-213">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="5a83f-214"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implementuje również <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-214"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="5a83f-215">Domyślna implementacja <xref:Microsoft.Extensions.Options.IOptionsFactory%601> ma logikę do użycia odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="5a83f-215">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="5a83f-216">Opcja `null` nazwana służy do określania wszystkich wystąpień nazwanych zamiast określonego wystąpienia nazwanego (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> i <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> używania tej Konwencji).</span><span class="sxs-lookup"><span data-stu-id="5a83f-216">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="5a83f-217">Interfejs API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="5a83f-217">OptionsBuilder API</span></span>

<span data-ttu-id="5a83f-218"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> służy do konfigurowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-218"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="5a83f-219">`OptionsBuilder` usprawnia Tworzenie nazwanych opcji, ponieważ jest tylko pojedynczym parametrem do początkowego wywołania `AddOptions<TOptions>(string optionsName)`, a nie pojawiają się we wszystkich kolejnych wywołaniach.</span><span class="sxs-lookup"><span data-stu-id="5a83f-219">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="5a83f-220">Sprawdzanie poprawności opcji i przeciążenia `ConfigureOptions`, które akceptują zależności usługi, są dostępne tylko za pośrednictwem `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-220">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="5a83f-221">Korzystanie z usług DI Services w celu konfigurowania opcji</span><span class="sxs-lookup"><span data-stu-id="5a83f-221">Use DI services to configure options</span></span>

<span data-ttu-id="5a83f-222">Można uzyskać dostęp do innych usług z iniekcji zależności podczas konfigurowania opcji na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="5a83f-222">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="5a83f-223">Przekaż delegat konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) w usłudze [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="5a83f-223">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="5a83f-224">`OptionsBuilder<TOptions>` zapewnia przeciążenia [konfiguracji](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) , które zezwalają na użycie maksymalnie pięciu usług w celu skonfigurowania opcji:</span><span class="sxs-lookup"><span data-stu-id="5a83f-224">`OptionsBuilder<TOptions>` provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="5a83f-225">Utwórz własny typ, który implementuje <xref:Microsoft.Extensions.Options.IConfigureOptions%601> lub <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i Zarejestruj typ jako usługę.</span><span class="sxs-lookup"><span data-stu-id="5a83f-225">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="5a83f-226">Zalecamy przekazanie delegata konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), ponieważ tworzenie usługi jest bardziej skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="5a83f-226">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="5a83f-227">Tworzenie własnego typu jest równoznaczne z tym, co jest używane przez platformę podczas [konfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="5a83f-227">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="5a83f-228">Wywołanie metody [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) powoduje zarejestrowanie przejściowej ogólnej <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, która ma Konstruktor akceptujący określone typy usług ogólnych.</span><span class="sxs-lookup"><span data-stu-id="5a83f-228">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="5a83f-229">Sprawdzanie poprawności opcji</span><span class="sxs-lookup"><span data-stu-id="5a83f-229">Options validation</span></span>

<span data-ttu-id="5a83f-230">Sprawdzanie poprawności opcji umożliwia sprawdzenie opcji w przypadku skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-230">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="5a83f-231">Wywołaj `Validate` z metodą walidacji, która zwraca `true`, jeśli opcje są prawidłowe i `false` jeśli są nieprawidłowe:</span><span class="sxs-lookup"><span data-stu-id="5a83f-231">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="5a83f-232">Poprzedni przykład ustawia nazwane wystąpienie opcji na `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-232">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="5a83f-233">Domyślne wystąpienie opcji jest `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-233">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="5a83f-234">Walidacja jest uruchamiana po utworzeniu wystąpienia opcji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-234">Validation runs when the options instance is created.</span></span> <span data-ttu-id="5a83f-235">Twoje wystąpienie opcji gwarantuje przekazanie walidacji przy pierwszej próbie dostępu.</span><span class="sxs-lookup"><span data-stu-id="5a83f-235">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5a83f-236">Sprawdzanie poprawności opcji nie chroni przed modyfikacjami opcji po początkowym skonfigurowaniu opcji i sprawdzeniu poprawności.</span><span class="sxs-lookup"><span data-stu-id="5a83f-236">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="5a83f-237">Metoda `Validate` akceptuje `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-237">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="5a83f-238">Aby w pełni dostosować walidację, zaimplementuj `IValidateOptions<TOptions>`, co umożliwia:</span><span class="sxs-lookup"><span data-stu-id="5a83f-238">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="5a83f-239">Sprawdzanie poprawności wielu typów opcji: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="5a83f-239">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="5a83f-240">Walidacja zależna od innego typu opcji: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="5a83f-240">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="5a83f-241">`IValidateOptions` sprawdza poprawność:</span><span class="sxs-lookup"><span data-stu-id="5a83f-241">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="5a83f-242">Konkretne nazwane wystąpienie opcji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-242">A specific named options instance.</span></span>
* <span data-ttu-id="5a83f-243">Wszystkie opcje, gdy `name` jest `null`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-243">All options when `name` is `null`.</span></span>

<span data-ttu-id="5a83f-244">Zwróć `ValidateOptionsResult` z implementacji interfejsu:</span><span class="sxs-lookup"><span data-stu-id="5a83f-244">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="5a83f-245">Weryfikacja oparta na adnotacji danych jest dostępna w pakiecie [Microsoft. Extensions. options. DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) , wywołując metodę <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> w `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-245">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="5a83f-246">`Microsoft.Extensions.Options.DataAnnotations` jest niejawnie przywoływana w aplikacjach ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5a83f-246">`Microsoft.Extensions.Options.DataAnnotations` is implicitly referenced in ASP.NET Core apps.</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
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
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().CurrentValue);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

<span data-ttu-id="5a83f-247">Eager sprawdzanie poprawności (niepowodzenie szybkie przy uruchamianiu) jest rozważane dla przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-247">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="5a83f-248">Opcje po konfiguracji</span><span class="sxs-lookup"><span data-stu-id="5a83f-248">Options post-configuration</span></span>

<span data-ttu-id="5a83f-249">Ustaw wartość po konfiguracji z <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-249">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="5a83f-250">Po uruchomieniu zostanie uruchomiona po zakończeniu konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="5a83f-250">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="5a83f-251"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> jest dostępny do skonfigurowania nazwanych opcji:</span><span class="sxs-lookup"><span data-stu-id="5a83f-251"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="5a83f-252">Użyj <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>, aby skonfigurować wszystkie wystąpienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="5a83f-252">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="5a83f-253">Dostęp do opcji podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="5a83f-253">Accessing options during startup</span></span>

<span data-ttu-id="5a83f-254"><xref:Microsoft.Extensions.Options.IOptions%601> i <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> mogą być używane w `Startup.Configure`, ponieważ usługi są kompilowane przed wykonaniem metody `Configure`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-254"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, 
    IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="5a83f-255">Nie używaj <xref:Microsoft.Extensions.Options.IOptions%601> ani <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-255">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5a83f-256">Niespójny stan opcji może istnieć ze względu na kolejność rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="5a83f-256">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="5a83f-257">Wzorzec opcji używa klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="5a83f-257">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="5a83f-258">Gdy [Ustawienia konfiguracji](xref:fundamentals/configuration/index) są izolowane według scenariusza w osobnych klasach, aplikacja będzie zgodna z dwoma ważnymi zasadami inżynierii oprogramowania:</span><span class="sxs-lookup"><span data-stu-id="5a83f-258">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="5a83f-259">[Zasady podziału interfejsu (ISP) lub &ndash; hermetyzacji](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) (klasy), które są zależne od ustawień konfiguracji, zależą tylko od ustawień konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="5a83f-259">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="5a83f-260">[Separacja problemów](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; ustawień różnych części aplikacji nie zależy od siebie ani nie jest połączona.</span><span class="sxs-lookup"><span data-stu-id="5a83f-260">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="5a83f-261">Opcje umożliwiają również mechanizm weryfikacji danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="5a83f-261">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="5a83f-262">Aby uzyskać więcej informacji, zobacz sekcję [Opcje walidacji](#options-validation) .</span><span class="sxs-lookup"><span data-stu-id="5a83f-262">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="5a83f-263">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5a83f-263">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5a83f-264">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="5a83f-264">Prerequisites</span></span>

<span data-ttu-id="5a83f-265">Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) .</span><span class="sxs-lookup"><span data-stu-id="5a83f-265">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="5a83f-266">Interfejsy opcji</span><span class="sxs-lookup"><span data-stu-id="5a83f-266">Options interfaces</span></span>

<span data-ttu-id="5a83f-267"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> służy do pobierania opcji i zarządzania powiadomieniami o opcjach dla wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-267"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="5a83f-268"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="5a83f-268"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="5a83f-269">Powiadomienia o zmianie</span><span class="sxs-lookup"><span data-stu-id="5a83f-269">Change notifications</span></span>
* [<span data-ttu-id="5a83f-270">Nazwane opcje</span><span class="sxs-lookup"><span data-stu-id="5a83f-270">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="5a83f-271">Konfiguracja do ponownego ładowania</span><span class="sxs-lookup"><span data-stu-id="5a83f-271">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="5a83f-272">Unieważnianie opcji selektywnych (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="5a83f-272">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="5a83f-273">Scenariusze [po konfiguracji](#options-post-configuration) umożliwiają ustawianie lub zmienianie opcji po wystąpieniu całej konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-273">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="5a83f-274"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> jest odpowiedzialny za tworzenie nowych wystąpień opcji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-274"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="5a83f-275">Ma ona jedną <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> metodę.</span><span class="sxs-lookup"><span data-stu-id="5a83f-275">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="5a83f-276">Domyślna implementacja pobiera wszystkie zarejestrowane <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> i uruchamia najpierw wszystkie konfiguracje, po których następuje po zakończeniu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-276">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="5a83f-277">Odróżnia między <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i wywołuje tylko odpowiedni interfejs.</span><span class="sxs-lookup"><span data-stu-id="5a83f-277">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="5a83f-278"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> jest używana przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do buforowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-278"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="5a83f-279"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> unieważnia wystąpienia opcji na monitorze, aby wartość była ponownie obliczana (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="5a83f-279">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="5a83f-280">Wartości można wprowadzać ręcznie przy użyciu <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-280">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="5a83f-281">Metoda <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> jest używana, gdy wszystkie nazwane wystąpienia powinny być ponownie tworzone na żądanie.</span><span class="sxs-lookup"><span data-stu-id="5a83f-281">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="5a83f-282"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> jest przydatne w scenariuszach, w których opcje powinny być ponownie obliczane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="5a83f-282"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="5a83f-283">Aby uzyskać więcej informacji, zobacz sekcję [Załaduj dane konfiguracji za pomocą IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) .</span><span class="sxs-lookup"><span data-stu-id="5a83f-283">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="5a83f-284"><xref:Microsoft.Extensions.Options.IOptions%601> może służyć do obsługi opcji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-284"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="5a83f-285">Jednak <xref:Microsoft.Extensions.Options.IOptions%601> nie obsługuje powyższych scenariuszy <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-285">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="5a83f-286">Można nadal używać <xref:Microsoft.Extensions.Options.IOptions%601> w istniejących strukturach i bibliotekach, które już używają interfejsu <xref:Microsoft.Extensions.Options.IOptions%601> i nie wymagają scenariuszy udostępnianych przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-286">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="5a83f-287">Konfiguracja opcji ogólnych</span><span class="sxs-lookup"><span data-stu-id="5a83f-287">General options configuration</span></span>

<span data-ttu-id="5a83f-288">Konfiguracja opcji ogólnych jest przedstawiona jako przykład 1 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="5a83f-288">General options configuration is demonstrated as Example 1 in the sample app.</span></span>

<span data-ttu-id="5a83f-289">Klasa Options musi być nieabstrakcyjna z publicznym konstruktorem bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="5a83f-289">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="5a83f-290">Następująca Klasa `MyOptions`ma dwie właściwości, `Option1` i `Option2`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-290">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="5a83f-291">Ustawienie wartości domyślnych jest opcjonalne, ale Konstruktor klasy w poniższym przykładzie ustawia wartość domyślną `Option1`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-291">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="5a83f-292">`Option2` ma ustawioną wartość domyślną, inicjując właściwość bezpośrednio (*modele/opcje. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-292">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="5a83f-293">Klasa `MyOptions` zostanie dodana do kontenera usługi z <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> i powiązana z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="5a83f-293">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="5a83f-294">Poniższy model strony używa [iniekcji zależności konstruktora](xref:mvc/controllers/dependency-injection) z <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, aby uzyskać dostęp do ustawień (*stron/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-294">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="5a83f-295">Plik *appSettings. JSON* przykładu Określa wartości dla `option1` i `option2`:</span><span class="sxs-lookup"><span data-stu-id="5a83f-295">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="5a83f-296">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="5a83f-296">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="5a83f-297">W przypadku używania <xref:System.Configuration.ConfigurationBuilder> niestandardowego do załadowania konfiguracji opcji z pliku ustawień upewnij się, że ścieżka bazowa jest ustawiona poprawnie:</span><span class="sxs-lookup"><span data-stu-id="5a83f-297">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="5a83f-298">Jawne ustawienie ścieżki podstawowej nie jest wymagane podczas ładowania konfiguracji opcji z pliku ustawień za pośrednictwem <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-298">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="5a83f-299">Skonfiguruj proste opcje z delegatem</span><span class="sxs-lookup"><span data-stu-id="5a83f-299">Configure simple options with a delegate</span></span>

<span data-ttu-id="5a83f-300">Konfigurowanie prostych opcji z delegatem jest zademonstrowane jako przykład 2 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-300">Configuring simple options with a delegate is demonstrated as Example 2 in the sample app.</span></span>

<span data-ttu-id="5a83f-301">Użyj delegata, aby ustawić wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-301">Use a delegate to set options values.</span></span> <span data-ttu-id="5a83f-302">Przykładowa aplikacja używa klasy `MyOptionsWithDelegateConfig` (*modele/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-302">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="5a83f-303">W poniższym kodzie do kontenera usługi jest dodawana druga usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-303">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="5a83f-304">Używa delegata do konfigurowania powiązania z `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="5a83f-304">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="5a83f-305">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="5a83f-305">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="5a83f-306">Można dodać wielu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-306">You can add multiple configuration providers.</span></span> <span data-ttu-id="5a83f-307">Dostawcy konfiguracji są dostępni z pakietów NuGet i są stosowane w kolejności, w jakiej zostały zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="5a83f-307">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="5a83f-308">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-308">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="5a83f-309">Każde wywołanie <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> dodaje usługę <xref:Microsoft.Extensions.Options.IConfigureOptions%601> do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="5a83f-309">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="5a83f-310">W poprzednim przykładzie wartości `Option1` i `Option2` są określone w pliku *appSettings. JSON*, ale wartości `Option1` i `Option2` są przesłonięte przez skonfigurowany delegat.</span><span class="sxs-lookup"><span data-stu-id="5a83f-310">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="5a83f-311">Gdy jest włączona więcej niż jedna usługa konfiguracji, ostatnie Źródło konfiguracji określiło *serwer WINS* i ustawi wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-311">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="5a83f-312">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="5a83f-312">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="5a83f-313">Konfiguracja podopcji</span><span class="sxs-lookup"><span data-stu-id="5a83f-313">Suboptions configuration</span></span>

<span data-ttu-id="5a83f-314">Konfiguracja podopcji jest przedstawiana jako przykład 3 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-314">Suboptions configuration is demonstrated as Example 3 in the sample app.</span></span>

<span data-ttu-id="5a83f-315">Aplikacje powinny tworzyć klasy opcji, które odnoszą się do określonych grup scenariuszy (klas) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-315">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="5a83f-316">Części aplikacji, które wymagają wartości konfiguracyjnych, powinny mieć dostęp tylko do wartości konfiguracyjnych, z których korzystają.</span><span class="sxs-lookup"><span data-stu-id="5a83f-316">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="5a83f-317">Po powiązaniu opcji powiązań z konfiguracją Każda właściwość w typie opcji jest powiązana z kluczem konfiguracji formularza `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-317">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="5a83f-318">Na przykład właściwość `MyOptions.Option1` jest powiązana z `Option1`klucza, który jest odczytywany z właściwości `option1` w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="5a83f-318">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="5a83f-319">W poniższym kodzie do kontenera usługi zostanie dodana trzecia usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-319">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="5a83f-320">Wiąże `MySubOptions` z sekcją `subsection` pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="5a83f-320">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="5a83f-321">Metoda `GetSection` wymaga przestrzeni nazw <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-321">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="5a83f-322">Plik *appSettings. JSON* przykładu definiuje `subsection` składową z kluczami dla `suboption1` i `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="5a83f-322">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="5a83f-323">Klasa `MySubOptions` definiuje właściwości, `SubOption1` i `SubOption2`do przechowywania wartości opcji (*modele/MySubOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-323">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="5a83f-324">Metoda `OnGet` modelu strony zwraca ciąg z wartościami opcji (*Pages/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-324">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="5a83f-325">Po uruchomieniu aplikacji Metoda `OnGet` zwraca ciąg pokazujący wartości klasy podopcji:</span><span class="sxs-lookup"><span data-stu-id="5a83f-325">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="5a83f-326">Iniekcja opcji</span><span class="sxs-lookup"><span data-stu-id="5a83f-326">Options injection</span></span>

<span data-ttu-id="5a83f-327">Iniekcja opcji jest przedstawiana jako przykład 4 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-327">Options injection is demonstrated as Example 4 in the sample app.</span></span>

<span data-ttu-id="5a83f-328">Wsuń <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do:</span><span class="sxs-lookup"><span data-stu-id="5a83f-328">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="5a83f-329">Strona Razor lub widok MVC z dyrektywą [`@inject`](xref:mvc/views/razor#inject) Razor.</span><span class="sxs-lookup"><span data-stu-id="5a83f-329">A Razor page or MVC view with the [`@inject`](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="5a83f-330">Model strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="5a83f-330">A page or view model.</span></span>

<span data-ttu-id="5a83f-331">Poniższy przykład z przykładowej aplikacji wprowadza <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do modelu strony (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-331">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="5a83f-332">Przykładowa aplikacja pokazuje, jak wstrzyknąć `IOptionsMonitor<MyOptions>` za pomocą dyrektywy `@inject`:</span><span class="sxs-lookup"><span data-stu-id="5a83f-332">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="5a83f-333">Po uruchomieniu aplikacji na renderowanej stronie są wyświetlane wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="5a83f-333">When the app is run, the options values are shown in the rendered page:</span></span>

![Wartości opcji opcja1: value1_from_json i Opcja2:-1 są ładowane z modelu i przez iniekcję do widoku.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="5a83f-335">Załaduj ponownie dane konfiguracji za pomocą IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="5a83f-335">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="5a83f-336">Ponowne ładowanie danych konfiguracyjnych za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> przedstawiono w przykładzie 5 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="5a83f-336">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example 5 in the sample app.</span></span>

<span data-ttu-id="5a83f-337">Przy użyciu <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>opcje są obliczane raz dla każdego żądania, gdy jest on używany i buforowany przez okres istnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="5a83f-337">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="5a83f-338">Różnica między `IOptionsMonitor` i `IOptionsSnapshot` to:</span><span class="sxs-lookup"><span data-stu-id="5a83f-338">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="5a83f-339">`IOptionsMonitor` to [Pojedyncza usługa](xref:fundamentals/dependency-injection#singleton) , która pobiera bieżące wartości opcji w dowolnym momencie, która jest szczególnie przydatna w pojedynczych zależnościach.</span><span class="sxs-lookup"><span data-stu-id="5a83f-339">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="5a83f-340">`IOptionsSnapshot` jest [usługą objętą zakresem](xref:fundamentals/dependency-injection#scoped) i zawiera migawkę opcji w czasie konstruowania obiektu `IOptionsSnapshot<T>`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-340">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="5a83f-341">Migawki opcji są przeznaczone do użycia z zależnościami przejściowymi i zakresowymi.</span><span class="sxs-lookup"><span data-stu-id="5a83f-341">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="5a83f-342">Poniższy przykład ilustruje sposób tworzenia nowego <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> po wprowadzeniu zmian w pliku *appSettings. JSON* (*Pages/index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="5a83f-342">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="5a83f-343">Wiele żądań do zwracanych przez serwer wartości stałych dostarczonych przez plik *appSettings. JSON* do momentu zmiany pliku i ponownego załadowania konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-343">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="5a83f-344">Na poniższej ilustracji przedstawiono początkowe `option1` i `option2` wartości załadowane z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="5a83f-344">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="5a83f-345">Zmień wartości w pliku *appSettings. JSON* na `value1_from_json UPDATED` i `200`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-345">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="5a83f-346">Zapisz plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="5a83f-346">Save the *appsettings.json* file.</span></span> <span data-ttu-id="5a83f-347">Odśwież przeglądarkę, aby zobaczyć, że wartości opcji są aktualizowane:</span><span class="sxs-lookup"><span data-stu-id="5a83f-347">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="5a83f-348">Obsługa nazwanych opcji w programie IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="5a83f-348">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="5a83f-349">Obsługa nazwanych opcji w <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> jest przedstawiana jako przykład 6 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="5a83f-349">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example 6 in the sample app.</span></span>

<span data-ttu-id="5a83f-350">Obsługa "nazwanych opcji" pozwala aplikacji rozróżnić między nazwanymi konfiguracjami opcji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-350">Named options support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="5a83f-351">W przykładowej aplikacji, nazwane opcje są zadeklarowane za pomocą [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), która wywołuje [ConfigureNamedOptions\<TOptions >. Skonfiguruj](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) metodę rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="5a83f-351">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method.</span></span> <span data-ttu-id="5a83f-352">W nazwanych opcjach jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="5a83f-352">Named options are case sensitive.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="5a83f-353">Aplikacja Przykładowa uzyskuje dostęp do nazwanych opcji za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-353">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="5a83f-354">Po uruchomieniu aplikacji przykładowej są zwracane nazwane opcje:</span><span class="sxs-lookup"><span data-stu-id="5a83f-354">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="5a83f-355">wartości `named_options_1` są dostarczane z konfiguracji, które są ładowane z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="5a83f-355">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="5a83f-356">wartości `named_options_2` są udostępniane przez:</span><span class="sxs-lookup"><span data-stu-id="5a83f-356">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="5a83f-357">Delegat `named_options_2` w `ConfigureServices` dla `Option1`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-357">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="5a83f-358">Wartość domyślna dla `Option2` dostarczonej przez klasę `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-358">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="5a83f-359">Skonfiguruj wszystkie opcje przy użyciu metody ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="5a83f-359">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="5a83f-360">Skonfiguruj wszystkie wystąpienia opcji za pomocą metody <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-360">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="5a83f-361">Poniższy kod konfiguruje `Option1` dla wszystkich wystąpień konfiguracji z wspólną wartością.</span><span class="sxs-lookup"><span data-stu-id="5a83f-361">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="5a83f-362">Ręcznie Dodaj następujący kod do metody `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5a83f-362">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="5a83f-363">Uruchomienie przykładowej aplikacji po dodaniu kodu daje następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="5a83f-363">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="5a83f-364">Wszystkie opcje są nazwanymi wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="5a83f-364">All options are named instances.</span></span> <span data-ttu-id="5a83f-365">Istniejące wystąpienia <xref:Microsoft.Extensions.Options.IConfigureOptions%601> są traktowane jako ukierunkowane na wystąpienie `Options.DefaultName`, które jest `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-365">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="5a83f-366"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implementuje również <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-366"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="5a83f-367">Domyślna implementacja <xref:Microsoft.Extensions.Options.IOptionsFactory%601> ma logikę do użycia odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="5a83f-367">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="5a83f-368">Opcja `null` nazwana służy do określania wszystkich wystąpień nazwanych zamiast określonego wystąpienia nazwanego (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> i <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> używania tej Konwencji).</span><span class="sxs-lookup"><span data-stu-id="5a83f-368">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="5a83f-369">Interfejs API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="5a83f-369">OptionsBuilder API</span></span>

<span data-ttu-id="5a83f-370"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> służy do konfigurowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-370"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="5a83f-371">`OptionsBuilder` usprawnia Tworzenie nazwanych opcji, ponieważ jest tylko pojedynczym parametrem do początkowego wywołania `AddOptions<TOptions>(string optionsName)`, a nie pojawiają się we wszystkich kolejnych wywołaniach.</span><span class="sxs-lookup"><span data-stu-id="5a83f-371">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="5a83f-372">Sprawdzanie poprawności opcji i przeciążenia `ConfigureOptions`, które akceptują zależności usługi, są dostępne tylko za pośrednictwem `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-372">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="5a83f-373">Korzystanie z usług DI Services w celu konfigurowania opcji</span><span class="sxs-lookup"><span data-stu-id="5a83f-373">Use DI services to configure options</span></span>

<span data-ttu-id="5a83f-374">Można uzyskać dostęp do innych usług z iniekcji zależności podczas konfigurowania opcji na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="5a83f-374">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="5a83f-375">Przekaż delegat konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) w usłudze [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="5a83f-375">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="5a83f-376">Program [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) oferuje przeciążenia [konfiguracji](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) , które umożliwiają konfigurowanie opcji przy użyciu maksymalnie pięciu usług:</span><span class="sxs-lookup"><span data-stu-id="5a83f-376">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="5a83f-377">Utwórz własny typ, który implementuje <xref:Microsoft.Extensions.Options.IConfigureOptions%601> lub <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i Zarejestruj typ jako usługę.</span><span class="sxs-lookup"><span data-stu-id="5a83f-377">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="5a83f-378">Zalecamy przekazanie delegata konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), ponieważ tworzenie usługi jest bardziej skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="5a83f-378">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="5a83f-379">Tworzenie własnego typu jest równoznaczne z tym, co jest używane przez platformę podczas [konfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="5a83f-379">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="5a83f-380">Wywołanie metody [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) powoduje zarejestrowanie przejściowej ogólnej <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, która ma Konstruktor akceptujący określone typy usług ogólnych.</span><span class="sxs-lookup"><span data-stu-id="5a83f-380">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="5a83f-381">Sprawdzanie poprawności opcji</span><span class="sxs-lookup"><span data-stu-id="5a83f-381">Options validation</span></span>

<span data-ttu-id="5a83f-382">Sprawdzanie poprawności opcji umożliwia sprawdzenie opcji w przypadku skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-382">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="5a83f-383">Wywołaj `Validate` z metodą walidacji, która zwraca `true`, jeśli opcje są prawidłowe i `false` jeśli są nieprawidłowe:</span><span class="sxs-lookup"><span data-stu-id="5a83f-383">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="5a83f-384">Poprzedni przykład ustawia nazwane wystąpienie opcji na `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-384">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="5a83f-385">Domyślne wystąpienie opcji jest `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-385">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="5a83f-386">Walidacja jest uruchamiana po utworzeniu wystąpienia opcji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-386">Validation runs when the options instance is created.</span></span> <span data-ttu-id="5a83f-387">Twoje wystąpienie opcji gwarantuje przekazanie walidacji przy pierwszej próbie dostępu.</span><span class="sxs-lookup"><span data-stu-id="5a83f-387">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5a83f-388">Sprawdzanie poprawności opcji nie chroni przed modyfikacjami opcji po początkowym skonfigurowaniu opcji i sprawdzeniu poprawności.</span><span class="sxs-lookup"><span data-stu-id="5a83f-388">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="5a83f-389">Metoda `Validate` akceptuje `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-389">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="5a83f-390">Aby w pełni dostosować walidację, zaimplementuj `IValidateOptions<TOptions>`, co umożliwia:</span><span class="sxs-lookup"><span data-stu-id="5a83f-390">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="5a83f-391">Sprawdzanie poprawności wielu typów opcji: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="5a83f-391">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="5a83f-392">Walidacja zależna od innego typu opcji: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="5a83f-392">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="5a83f-393">`IValidateOptions` sprawdza poprawność:</span><span class="sxs-lookup"><span data-stu-id="5a83f-393">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="5a83f-394">Konkretne nazwane wystąpienie opcji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-394">A specific named options instance.</span></span>
* <span data-ttu-id="5a83f-395">Wszystkie opcje, gdy `name` jest `null`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-395">All options when `name` is `null`.</span></span>

<span data-ttu-id="5a83f-396">Zwróć `ValidateOptionsResult` z implementacji interfejsu:</span><span class="sxs-lookup"><span data-stu-id="5a83f-396">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="5a83f-397">Weryfikacja oparta na adnotacji danych jest dostępna w pakiecie [Microsoft. Extensions. options. DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) , wywołując metodę <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> w `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-397">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="5a83f-398">`Microsoft.Extensions.Options.DataAnnotations` jest zawarty w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="5a83f-398">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

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
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().CurrentValue);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

<span data-ttu-id="5a83f-399">Eager sprawdzanie poprawności (niepowodzenie szybkie przy uruchamianiu) jest rozważane dla przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-399">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="5a83f-400">Opcje po konfiguracji</span><span class="sxs-lookup"><span data-stu-id="5a83f-400">Options post-configuration</span></span>

<span data-ttu-id="5a83f-401">Ustaw wartość po konfiguracji z <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-401">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="5a83f-402">Po uruchomieniu zostanie uruchomiona po zakończeniu konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="5a83f-402">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="5a83f-403"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> jest dostępny do skonfigurowania nazwanych opcji:</span><span class="sxs-lookup"><span data-stu-id="5a83f-403"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="5a83f-404">Użyj <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>, aby skonfigurować wszystkie wystąpienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="5a83f-404">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="5a83f-405">Dostęp do opcji podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="5a83f-405">Accessing options during startup</span></span>

<span data-ttu-id="5a83f-406"><xref:Microsoft.Extensions.Options.IOptions%601> i <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> mogą być używane w `Startup.Configure`, ponieważ usługi są kompilowane przed wykonaniem metody `Configure`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-406"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="5a83f-407">Nie używaj <xref:Microsoft.Extensions.Options.IOptions%601> ani <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-407">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5a83f-408">Niespójny stan opcji może istnieć ze względu na kolejność rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="5a83f-408">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="5a83f-409">Wzorzec opcji używa klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="5a83f-409">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="5a83f-410">Gdy [Ustawienia konfiguracji](xref:fundamentals/configuration/index) są izolowane według scenariusza w osobnych klasach, aplikacja będzie zgodna z dwoma ważnymi zasadami inżynierii oprogramowania:</span><span class="sxs-lookup"><span data-stu-id="5a83f-410">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="5a83f-411">[Zasady podziału interfejsu (ISP) lub &ndash; hermetyzacji](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) (klasy), które są zależne od ustawień konfiguracji, zależą tylko od ustawień konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="5a83f-411">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="5a83f-412">[Separacja problemów](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; ustawień różnych części aplikacji nie zależy od siebie ani nie jest połączona.</span><span class="sxs-lookup"><span data-stu-id="5a83f-412">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="5a83f-413">Opcje umożliwiają również mechanizm weryfikacji danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="5a83f-413">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="5a83f-414">Aby uzyskać więcej informacji, zobacz sekcję [Opcje walidacji](#options-validation) .</span><span class="sxs-lookup"><span data-stu-id="5a83f-414">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="5a83f-415">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5a83f-415">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5a83f-416">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="5a83f-416">Prerequisites</span></span>

<span data-ttu-id="5a83f-417">Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) .</span><span class="sxs-lookup"><span data-stu-id="5a83f-417">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="5a83f-418">Interfejsy opcji</span><span class="sxs-lookup"><span data-stu-id="5a83f-418">Options interfaces</span></span>

<span data-ttu-id="5a83f-419"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> służy do pobierania opcji i zarządzania powiadomieniami o opcjach dla wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-419"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="5a83f-420"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="5a83f-420"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="5a83f-421">Powiadomienia o zmianie</span><span class="sxs-lookup"><span data-stu-id="5a83f-421">Change notifications</span></span>
* [<span data-ttu-id="5a83f-422">Nazwane opcje</span><span class="sxs-lookup"><span data-stu-id="5a83f-422">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="5a83f-423">Konfiguracja do ponownego ładowania</span><span class="sxs-lookup"><span data-stu-id="5a83f-423">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="5a83f-424">Unieważnianie opcji selektywnych (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="5a83f-424">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="5a83f-425">Scenariusze [po konfiguracji](#options-post-configuration) umożliwiają ustawianie lub zmienianie opcji po wystąpieniu całej konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-425">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="5a83f-426"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> jest odpowiedzialny za tworzenie nowych wystąpień opcji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-426"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="5a83f-427">Ma ona jedną <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> metodę.</span><span class="sxs-lookup"><span data-stu-id="5a83f-427">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="5a83f-428">Domyślna implementacja pobiera wszystkie zarejestrowane <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> i uruchamia najpierw wszystkie konfiguracje, po których następuje po zakończeniu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-428">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="5a83f-429">Odróżnia między <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i wywołuje tylko odpowiedni interfejs.</span><span class="sxs-lookup"><span data-stu-id="5a83f-429">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="5a83f-430"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> jest używana przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do buforowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-430"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="5a83f-431"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> unieważnia wystąpienia opcji na monitorze, aby wartość była ponownie obliczana (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="5a83f-431">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="5a83f-432">Wartości można wprowadzać ręcznie przy użyciu <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-432">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="5a83f-433">Metoda <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> jest używana, gdy wszystkie nazwane wystąpienia powinny być ponownie tworzone na żądanie.</span><span class="sxs-lookup"><span data-stu-id="5a83f-433">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="5a83f-434"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> jest przydatne w scenariuszach, w których opcje powinny być ponownie obliczane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="5a83f-434"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="5a83f-435">Aby uzyskać więcej informacji, zobacz sekcję [Załaduj dane konfiguracji za pomocą IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) .</span><span class="sxs-lookup"><span data-stu-id="5a83f-435">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="5a83f-436"><xref:Microsoft.Extensions.Options.IOptions%601> może służyć do obsługi opcji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-436"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="5a83f-437">Jednak <xref:Microsoft.Extensions.Options.IOptions%601> nie obsługuje powyższych scenariuszy <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-437">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="5a83f-438">Można nadal używać <xref:Microsoft.Extensions.Options.IOptions%601> w istniejących strukturach i bibliotekach, które już używają interfejsu <xref:Microsoft.Extensions.Options.IOptions%601> i nie wymagają scenariuszy udostępnianych przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-438">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="5a83f-439">Konfiguracja opcji ogólnych</span><span class="sxs-lookup"><span data-stu-id="5a83f-439">General options configuration</span></span>

<span data-ttu-id="5a83f-440">Konfiguracja opcji ogólnych jest przedstawiona jako przykład 1 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="5a83f-440">General options configuration is demonstrated as Example 1 in the sample app.</span></span>

<span data-ttu-id="5a83f-441">Klasa Options musi być nieabstrakcyjna z publicznym konstruktorem bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="5a83f-441">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="5a83f-442">Następująca Klasa `MyOptions`ma dwie właściwości, `Option1` i `Option2`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-442">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="5a83f-443">Ustawienie wartości domyślnych jest opcjonalne, ale Konstruktor klasy w poniższym przykładzie ustawia wartość domyślną `Option1`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-443">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="5a83f-444">`Option2` ma ustawioną wartość domyślną, inicjując właściwość bezpośrednio (*modele/opcje. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-444">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="5a83f-445">Klasa `MyOptions` zostanie dodana do kontenera usługi z <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> i powiązana z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="5a83f-445">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="5a83f-446">Poniższy model strony używa [iniekcji zależności konstruktora](xref:mvc/controllers/dependency-injection) z <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, aby uzyskać dostęp do ustawień (*stron/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-446">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="5a83f-447">Plik *appSettings. JSON* przykładu Określa wartości dla `option1` i `option2`:</span><span class="sxs-lookup"><span data-stu-id="5a83f-447">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="5a83f-448">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="5a83f-448">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="5a83f-449">W przypadku używania <xref:System.Configuration.ConfigurationBuilder> niestandardowego do załadowania konfiguracji opcji z pliku ustawień upewnij się, że ścieżka bazowa jest ustawiona poprawnie:</span><span class="sxs-lookup"><span data-stu-id="5a83f-449">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="5a83f-450">Jawne ustawienie ścieżki podstawowej nie jest wymagane podczas ładowania konfiguracji opcji z pliku ustawień za pośrednictwem <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-450">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="5a83f-451">Skonfiguruj proste opcje z delegatem</span><span class="sxs-lookup"><span data-stu-id="5a83f-451">Configure simple options with a delegate</span></span>

<span data-ttu-id="5a83f-452">Konfigurowanie prostych opcji z delegatem jest zademonstrowane jako przykład 2 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-452">Configuring simple options with a delegate is demonstrated as Example 2 in the sample app.</span></span>

<span data-ttu-id="5a83f-453">Użyj delegata, aby ustawić wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-453">Use a delegate to set options values.</span></span> <span data-ttu-id="5a83f-454">Przykładowa aplikacja używa klasy `MyOptionsWithDelegateConfig` (*modele/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-454">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="5a83f-455">W poniższym kodzie do kontenera usługi jest dodawana druga usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-455">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="5a83f-456">Używa delegata do konfigurowania powiązania z `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="5a83f-456">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="5a83f-457">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="5a83f-457">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="5a83f-458">Można dodać wielu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-458">You can add multiple configuration providers.</span></span> <span data-ttu-id="5a83f-459">Dostawcy konfiguracji są dostępni z pakietów NuGet i są stosowane w kolejności, w jakiej zostały zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="5a83f-459">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="5a83f-460">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-460">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="5a83f-461">Każde wywołanie <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> dodaje usługę <xref:Microsoft.Extensions.Options.IConfigureOptions%601> do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="5a83f-461">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="5a83f-462">W poprzednim przykładzie wartości `Option1` i `Option2` są określone w pliku *appSettings. JSON*, ale wartości `Option1` i `Option2` są przesłonięte przez skonfigurowany delegat.</span><span class="sxs-lookup"><span data-stu-id="5a83f-462">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="5a83f-463">Gdy jest włączona więcej niż jedna usługa konfiguracji, ostatnie Źródło konfiguracji określiło *serwer WINS* i ustawi wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-463">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="5a83f-464">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="5a83f-464">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="5a83f-465">Konfiguracja podopcji</span><span class="sxs-lookup"><span data-stu-id="5a83f-465">Suboptions configuration</span></span>

<span data-ttu-id="5a83f-466">Konfiguracja podopcji jest przedstawiana jako przykład 3 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-466">Suboptions configuration is demonstrated as Example 3 in the sample app.</span></span>

<span data-ttu-id="5a83f-467">Aplikacje powinny tworzyć klasy opcji, które odnoszą się do określonych grup scenariuszy (klas) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-467">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="5a83f-468">Części aplikacji, które wymagają wartości konfiguracyjnych, powinny mieć dostęp tylko do wartości konfiguracyjnych, z których korzystają.</span><span class="sxs-lookup"><span data-stu-id="5a83f-468">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="5a83f-469">Po powiązaniu opcji powiązań z konfiguracją Każda właściwość w typie opcji jest powiązana z kluczem konfiguracji formularza `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-469">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="5a83f-470">Na przykład właściwość `MyOptions.Option1` jest powiązana z `Option1`klucza, który jest odczytywany z właściwości `option1` w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="5a83f-470">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="5a83f-471">W poniższym kodzie do kontenera usługi zostanie dodana trzecia usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-471">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="5a83f-472">Wiąże `MySubOptions` z sekcją `subsection` pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="5a83f-472">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="5a83f-473">Metoda `GetSection` wymaga przestrzeni nazw <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-473">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="5a83f-474">Plik *appSettings. JSON* przykładu definiuje `subsection` składową z kluczami dla `suboption1` i `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="5a83f-474">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="5a83f-475">Klasa `MySubOptions` definiuje właściwości, `SubOption1` i `SubOption2`do przechowywania wartości opcji (*modele/MySubOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-475">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="5a83f-476">Metoda `OnGet` modelu strony zwraca ciąg z wartościami opcji (*Pages/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-476">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="5a83f-477">Po uruchomieniu aplikacji Metoda `OnGet` zwraca ciąg pokazujący wartości klasy podopcji:</span><span class="sxs-lookup"><span data-stu-id="5a83f-477">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="5a83f-478">Opcje udostępniane przez model widoku lub bezpośrednie iniekcja widoku</span><span class="sxs-lookup"><span data-stu-id="5a83f-478">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="5a83f-479">Opcje udostępniane przez model widoku lub bezpośrednie wstrzyknięcie widoku są przedstawiane jako przykład 4 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="5a83f-479">Options provided by a view model or with direct view injection is demonstrated as Example 4 in the sample app.</span></span>

<span data-ttu-id="5a83f-480">Opcje można dostarczyć w modelu widoku lub poprzez wstrzyknięcie <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> bezpośrednio do widoku (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-480">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="5a83f-481">Przykładowa aplikacja pokazuje, jak wstrzyknąć `IOptionsMonitor<MyOptions>` za pomocą dyrektywy `@inject`:</span><span class="sxs-lookup"><span data-stu-id="5a83f-481">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="5a83f-482">Po uruchomieniu aplikacji na renderowanej stronie są wyświetlane wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="5a83f-482">When the app is run, the options values are shown in the rendered page:</span></span>

![Wartości opcji opcja1: value1_from_json i Opcja2:-1 są ładowane z modelu i przez iniekcję do widoku.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="5a83f-484">Załaduj ponownie dane konfiguracji za pomocą IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="5a83f-484">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="5a83f-485">Ponowne ładowanie danych konfiguracyjnych za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> przedstawiono w przykładzie 5 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="5a83f-485">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example 5 in the sample app.</span></span>

<span data-ttu-id="5a83f-486"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> obsługuje opcje ponownego ładowania z minimalnym obciążeniem przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="5a83f-486"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="5a83f-487">Opcje są obliczane raz dla żądania w przypadku uzyskiwania dostępu do pamięci podręcznej i buforowania jej przez okres istnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="5a83f-487">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="5a83f-488">Poniższy przykład ilustruje sposób tworzenia nowego <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> po wprowadzeniu zmian w pliku *appSettings. JSON* (*Pages/index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="5a83f-488">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="5a83f-489">Wiele żądań do zwracanych przez serwer wartości stałych dostarczonych przez plik *appSettings. JSON* do momentu zmiany pliku i ponownego załadowania konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-489">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="5a83f-490">Na poniższej ilustracji przedstawiono początkowe `option1` i `option2` wartości załadowane z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="5a83f-490">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="5a83f-491">Zmień wartości w pliku *appSettings. JSON* na `value1_from_json UPDATED` i `200`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-491">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="5a83f-492">Zapisz plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="5a83f-492">Save the *appsettings.json* file.</span></span> <span data-ttu-id="5a83f-493">Odśwież przeglądarkę, aby zobaczyć, że wartości opcji są aktualizowane:</span><span class="sxs-lookup"><span data-stu-id="5a83f-493">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="5a83f-494">Obsługa nazwanych opcji w programie IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="5a83f-494">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="5a83f-495">Obsługa nazwanych opcji w <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> jest przedstawiana jako przykład 6 w aplikacji przykładowej.</span><span class="sxs-lookup"><span data-stu-id="5a83f-495">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example 6 in the sample app.</span></span>

<span data-ttu-id="5a83f-496">Obsługa "nazwanych opcji" pozwala aplikacji rozróżnić między nazwanymi konfiguracjami opcji.</span><span class="sxs-lookup"><span data-stu-id="5a83f-496">Named options support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="5a83f-497">W przykładowej aplikacji, nazwane opcje są zadeklarowane za pomocą [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), która wywołuje [ConfigureNamedOptions\<TOptions >. Skonfiguruj](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) metodę rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="5a83f-497">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method.</span></span> <span data-ttu-id="5a83f-498">W nazwanych opcjach jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="5a83f-498">Named options are case sensitive.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="5a83f-499">Aplikacja Przykładowa uzyskuje dostęp do nazwanych opcji za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="5a83f-499">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="5a83f-500">Po uruchomieniu aplikacji przykładowej są zwracane nazwane opcje:</span><span class="sxs-lookup"><span data-stu-id="5a83f-500">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="5a83f-501">wartości `named_options_1` są dostarczane z konfiguracji, które są ładowane z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="5a83f-501">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="5a83f-502">wartości `named_options_2` są udostępniane przez:</span><span class="sxs-lookup"><span data-stu-id="5a83f-502">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="5a83f-503">Delegat `named_options_2` w `ConfigureServices` dla `Option1`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-503">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="5a83f-504">Wartość domyślna dla `Option2` dostarczonej przez klasę `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-504">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="5a83f-505">Skonfiguruj wszystkie opcje przy użyciu metody ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="5a83f-505">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="5a83f-506">Skonfiguruj wszystkie wystąpienia opcji za pomocą metody <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-506">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="5a83f-507">Poniższy kod konfiguruje `Option1` dla wszystkich wystąpień konfiguracji z wspólną wartością.</span><span class="sxs-lookup"><span data-stu-id="5a83f-507">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="5a83f-508">Ręcznie Dodaj następujący kod do metody `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5a83f-508">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="5a83f-509">Uruchomienie przykładowej aplikacji po dodaniu kodu daje następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="5a83f-509">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="5a83f-510">Wszystkie opcje są nazwanymi wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="5a83f-510">All options are named instances.</span></span> <span data-ttu-id="5a83f-511">Istniejące wystąpienia <xref:Microsoft.Extensions.Options.IConfigureOptions%601> są traktowane jako ukierunkowane na wystąpienie `Options.DefaultName`, które jest `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-511">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="5a83f-512"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implementuje również <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-512"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="5a83f-513">Domyślna implementacja <xref:Microsoft.Extensions.Options.IOptionsFactory%601> ma logikę do użycia odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="5a83f-513">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="5a83f-514">Opcja `null` nazwana służy do określania wszystkich wystąpień nazwanych zamiast określonego wystąpienia nazwanego (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> i <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> używania tej Konwencji).</span><span class="sxs-lookup"><span data-stu-id="5a83f-514">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="5a83f-515">Interfejs API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="5a83f-515">OptionsBuilder API</span></span>

<span data-ttu-id="5a83f-516"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> służy do konfigurowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-516"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="5a83f-517">`OptionsBuilder` usprawnia Tworzenie nazwanych opcji, ponieważ jest tylko pojedynczym parametrem do początkowego wywołania `AddOptions<TOptions>(string optionsName)`, a nie pojawiają się we wszystkich kolejnych wywołaniach.</span><span class="sxs-lookup"><span data-stu-id="5a83f-517">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="5a83f-518">Sprawdzanie poprawności opcji i przeciążenia `ConfigureOptions`, które akceptują zależności usługi, są dostępne tylko za pośrednictwem `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-518">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="5a83f-519">Korzystanie z usług DI Services w celu konfigurowania opcji</span><span class="sxs-lookup"><span data-stu-id="5a83f-519">Use DI services to configure options</span></span>

<span data-ttu-id="5a83f-520">Można uzyskać dostęp do innych usług z iniekcji zależności podczas konfigurowania opcji na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="5a83f-520">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="5a83f-521">Przekaż delegat konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) w usłudze [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="5a83f-521">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="5a83f-522">Program [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) oferuje przeciążenia [konfiguracji](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) , które umożliwiają konfigurowanie opcji przy użyciu maksymalnie pięciu usług:</span><span class="sxs-lookup"><span data-stu-id="5a83f-522">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="5a83f-523">Utwórz własny typ, który implementuje <xref:Microsoft.Extensions.Options.IConfigureOptions%601> lub <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i Zarejestruj typ jako usługę.</span><span class="sxs-lookup"><span data-stu-id="5a83f-523">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="5a83f-524">Zalecamy przekazanie delegata konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), ponieważ tworzenie usługi jest bardziej skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="5a83f-524">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="5a83f-525">Tworzenie własnego typu jest równoznaczne z tym, co jest używane przez platformę podczas [konfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="5a83f-525">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="5a83f-526">Wywołanie metody [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) powoduje zarejestrowanie przejściowej ogólnej <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, która ma Konstruktor akceptujący określone typy usług ogólnych.</span><span class="sxs-lookup"><span data-stu-id="5a83f-526">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-post-configuration"></a><span data-ttu-id="5a83f-527">Opcje po konfiguracji</span><span class="sxs-lookup"><span data-stu-id="5a83f-527">Options post-configuration</span></span>

<span data-ttu-id="5a83f-528">Ustaw wartość po konfiguracji z <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="5a83f-528">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="5a83f-529">Po uruchomieniu zostanie uruchomiona po zakończeniu konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="5a83f-529">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="5a83f-530"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> jest dostępny do skonfigurowania nazwanych opcji:</span><span class="sxs-lookup"><span data-stu-id="5a83f-530"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="5a83f-531">Użyj <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>, aby skonfigurować wszystkie wystąpienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="5a83f-531">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="5a83f-532">Dostęp do opcji podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="5a83f-532">Accessing options during startup</span></span>

<span data-ttu-id="5a83f-533"><xref:Microsoft.Extensions.Options.IOptions%601> i <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> mogą być używane w `Startup.Configure`, ponieważ usługi są kompilowane przed wykonaniem metody `Configure`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-533"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="5a83f-534">Nie używaj <xref:Microsoft.Extensions.Options.IOptions%601> ani <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5a83f-534">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5a83f-535">Niespójny stan opcji może istnieć ze względu na kolejność rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="5a83f-535">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="5a83f-536">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="5a83f-536">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
