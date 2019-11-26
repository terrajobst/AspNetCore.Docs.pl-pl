---
title: Wzorzec opcji w ASP.NET Core
author: guardrex
description: Dowiedz się, jak używać wzorca opcji do reprezentowania grup powiązanych ustawień w aplikacjach ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/18/2019
uid: fundamentals/configuration/options
ms.openlocfilehash: 4192bab8acef7c4f7bdf1ac481c468cd0a835420
ms.sourcegitcommit: 8157e5a351f49aeef3769f7d38b787b4386aad5f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/20/2019
ms.locfileid: "74239796"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="7aa3d-103">Wzorzec opcji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7aa3d-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="7aa3d-104">Autor [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7aa3d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7aa3d-105">Wzorzec opcji używa klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="7aa3d-106">Gdy [Ustawienia konfiguracji](xref:fundamentals/configuration/index) są izolowane według scenariusza w osobnych klasach, aplikacja będzie zgodna z dwoma ważnymi zasadami inżynierii oprogramowania:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="7aa3d-107">[Zasady podziału interfejsu (ISP) lub &ndash; hermetyzacji](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) (klasy), które są zależne od ustawień konfiguracji, zależą tylko od ustawień konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-107">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="7aa3d-108">[Separacja problemów](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; ustawień różnych części aplikacji nie zależy od siebie ani nie jest połączona.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-108">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="7aa3d-109">Opcje umożliwiają również mechanizm weryfikacji danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-109">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="7aa3d-110">Aby uzyskać więcej informacji, zobacz sekcję [Opcje walidacji](#options-validation) .</span><span class="sxs-lookup"><span data-stu-id="7aa3d-110">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="7aa3d-111">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7aa3d-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="7aa3d-112">Package</span><span class="sxs-lookup"><span data-stu-id="7aa3d-112">Package</span></span>

<span data-ttu-id="7aa3d-113">Pakiet [Microsoft. Extensions. options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) jest niejawnie przywoływany w aplikacjach ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-113">The [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package is implicitly referenced in ASP.NET Core apps.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="7aa3d-114">Interfejsy opcji</span><span class="sxs-lookup"><span data-stu-id="7aa3d-114">Options interfaces</span></span>

<span data-ttu-id="7aa3d-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> służy do pobierania opcji i zarządzania powiadomieniami o opcjach dla wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="7aa3d-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="7aa3d-117">Powiadomienia o zmianie</span><span class="sxs-lookup"><span data-stu-id="7aa3d-117">Change notifications</span></span>
* [<span data-ttu-id="7aa3d-118">Nazwane opcje</span><span class="sxs-lookup"><span data-stu-id="7aa3d-118">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="7aa3d-119">Konfiguracja do ponownego ładowania</span><span class="sxs-lookup"><span data-stu-id="7aa3d-119">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="7aa3d-120">Unieważnianie opcji selektywnych (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="7aa3d-120">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="7aa3d-121">Scenariusze [po konfiguracji](#options-post-configuration) umożliwiają ustawianie lub zmienianie opcji po wystąpieniu całej konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-121">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="7aa3d-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> jest odpowiedzialny za tworzenie nowych wystąpień opcji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="7aa3d-123">Ma ona jedną <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> metodę.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-123">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="7aa3d-124">Domyślna implementacja pobiera wszystkie zarejestrowane <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> i uruchamia najpierw wszystkie konfiguracje, po których następuje po zakończeniu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-124">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="7aa3d-125">Odróżnia między <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i wywołuje tylko odpowiedni interfejs.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-125">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="7aa3d-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> jest używana przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do buforowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="7aa3d-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> unieważnia wystąpienia opcji na monitorze, aby wartość była ponownie obliczana (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="7aa3d-127">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="7aa3d-128">Wartości można wprowadzać ręcznie przy użyciu <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-128">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="7aa3d-129">Metoda <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> jest używana, gdy wszystkie nazwane wystąpienia powinny być ponownie tworzone na żądanie.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-129">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="7aa3d-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> jest przydatne w scenariuszach, w których opcje powinny być ponownie obliczane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="7aa3d-131">Aby uzyskać więcej informacji, zobacz sekcję [Załaduj dane konfiguracji za pomocą IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) .</span><span class="sxs-lookup"><span data-stu-id="7aa3d-131">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="7aa3d-132"><xref:Microsoft.Extensions.Options.IOptions%601> może służyć do obsługi opcji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-132"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="7aa3d-133">Jednak <xref:Microsoft.Extensions.Options.IOptions%601> nie obsługuje powyższych scenariuszy <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-133">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="7aa3d-134">Można nadal używać <xref:Microsoft.Extensions.Options.IOptions%601> w istniejących strukturach i bibliotekach, które już używają interfejsu <xref:Microsoft.Extensions.Options.IOptions%601> i nie wymagają scenariuszy udostępnianych przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-134">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="7aa3d-135">Konfiguracja opcji ogólnych</span><span class="sxs-lookup"><span data-stu-id="7aa3d-135">General options configuration</span></span>

<span data-ttu-id="7aa3d-136">Konfiguracja opcji ogólnych jest przedstawiana jako przykład &num;1 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-136">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="7aa3d-137">Klasa Options musi być nieabstrakcyjna z publicznym konstruktorem bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-137">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="7aa3d-138">Następująca Klasa `MyOptions`ma dwie właściwości, `Option1` i `Option2`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-138">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="7aa3d-139">Ustawienie wartości domyślnych jest opcjonalne, ale Konstruktor klasy w poniższym przykładzie ustawia wartość domyślną `Option1`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-139">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="7aa3d-140">`Option2` ma ustawioną wartość domyślną, inicjując właściwość bezpośrednio (*modele/opcje. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-140">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="7aa3d-141">Klasa `MyOptions` zostanie dodana do kontenera usługi z <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> i powiązana z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-141">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="7aa3d-142">Poniższy model strony używa [iniekcji zależności konstruktora](xref:mvc/controllers/dependency-injection) z <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, aby uzyskać dostęp do ustawień (*stron/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-142">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="7aa3d-143">Plik *appSettings. JSON* przykładu Określa wartości dla `option1` i `option2`:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-143">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="7aa3d-144">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-144">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="7aa3d-145">W przypadku używania <xref:System.Configuration.ConfigurationBuilder> niestandardowego do załadowania konfiguracji opcji z pliku ustawień upewnij się, że ścieżka bazowa jest ustawiona poprawnie:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-145">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="7aa3d-146">Jawne ustawienie ścieżki podstawowej nie jest wymagane podczas ładowania konfiguracji opcji z pliku ustawień za pośrednictwem <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-146">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="7aa3d-147">Skonfiguruj proste opcje z delegatem</span><span class="sxs-lookup"><span data-stu-id="7aa3d-147">Configure simple options with a delegate</span></span>

<span data-ttu-id="7aa3d-148">Konfigurowanie prostych opcji za pomocą delegata jest zademonstrowane jako przykład &num;2 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-148">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="7aa3d-149">Użyj delegata, aby ustawić wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-149">Use a delegate to set options values.</span></span> <span data-ttu-id="7aa3d-150">Przykładowa aplikacja używa klasy `MyOptionsWithDelegateConfig` (*modele/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-150">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="7aa3d-151">W poniższym kodzie do kontenera usługi jest dodawana druga usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-151">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="7aa3d-152">Używa delegata do konfigurowania powiązania z `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-152">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="7aa3d-153">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-153">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="7aa3d-154">Można dodać wielu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-154">You can add multiple configuration providers.</span></span> <span data-ttu-id="7aa3d-155">Dostawcy konfiguracji są dostępni z pakietów NuGet i są stosowane w kolejności, w jakiej zostały zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-155">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="7aa3d-156">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-156">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="7aa3d-157">Każde wywołanie <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> dodaje usługę <xref:Microsoft.Extensions.Options.IConfigureOptions%601> do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-157">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="7aa3d-158">W poprzednim przykładzie wartości `Option1` i `Option2` są określone w pliku *appSettings. JSON*, ale wartości `Option1` i `Option2` są przesłonięte przez skonfigurowany delegat.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-158">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="7aa3d-159">Gdy jest włączona więcej niż jedna usługa konfiguracji, ostatnie Źródło konfiguracji określiło *serwer WINS* i ustawi wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-159">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="7aa3d-160">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-160">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="7aa3d-161">Konfiguracja podopcji</span><span class="sxs-lookup"><span data-stu-id="7aa3d-161">Suboptions configuration</span></span>

<span data-ttu-id="7aa3d-162">Konfiguracja podopcji jest przedstawiana jako przykład &num;3 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-162">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="7aa3d-163">Aplikacje powinny tworzyć klasy opcji, które odnoszą się do określonych grup scenariuszy (klas) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-163">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="7aa3d-164">Części aplikacji, które wymagają wartości konfiguracyjnych, powinny mieć dostęp tylko do wartości konfiguracyjnych, z których korzystają.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-164">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="7aa3d-165">Po powiązaniu opcji powiązań z konfiguracją Każda właściwość w typie opcji jest powiązana z kluczem konfiguracji formularza `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-165">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="7aa3d-166">Na przykład właściwość `MyOptions.Option1` jest powiązana z `Option1`klucza, który jest odczytywany z właściwości `option1` w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-166">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="7aa3d-167">W poniższym kodzie do kontenera usługi zostanie dodana trzecia usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-167">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="7aa3d-168">Wiąże `MySubOptions` z sekcją `subsection` pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="7aa3d-168">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="7aa3d-169">Metoda `GetSection` wymaga przestrzeni nazw <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-169">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="7aa3d-170">Plik *appSettings. JSON* przykładu definiuje `subsection` składową z kluczami dla `suboption1` i `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-170">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="7aa3d-171">Klasa `MySubOptions` definiuje właściwości, `SubOption1` i `SubOption2`do przechowywania wartości opcji (*modele/MySubOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-171">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="7aa3d-172">Metoda `OnGet` modelu strony zwraca ciąg z wartościami opcji (*Pages/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-172">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="7aa3d-173">Po uruchomieniu aplikacji Metoda `OnGet` zwraca ciąg pokazujący wartości klasy podopcji:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-173">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="7aa3d-174">Iniekcja opcji</span><span class="sxs-lookup"><span data-stu-id="7aa3d-174">Options injection</span></span>

<span data-ttu-id="7aa3d-175">Iniekcja opcji jest prezentowana jako przykład &num;4 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-175">Options injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="7aa3d-176">Wsuń <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-176">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="7aa3d-177">Strona Razor lub widok MVC z dyrektywą [@inject](xref:mvc/views/razor#inject) Razor.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-177">A Razor page or MVC view with the [@inject](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="7aa3d-178">Model strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-178">A page or view model.</span></span>

<span data-ttu-id="7aa3d-179">Poniższy przykład z przykładowej aplikacji wprowadza <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do modelu strony (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-179">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="7aa3d-180">Przykładowa aplikacja pokazuje, jak wstrzyknąć `IOptionsMonitor<MyOptions>` za pomocą dyrektywy `@inject`:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-180">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/3.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="7aa3d-181">Po uruchomieniu aplikacji na renderowanej stronie są wyświetlane wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-181">When the app is run, the options values are shown in the rendered page:</span></span>

![Wartości opcji opcja1: value1_from_json i Opcja2:-1 są ładowane z modelu i przez iniekcję do widoku.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="7aa3d-183">Załaduj ponownie dane konfiguracji za pomocą IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="7aa3d-183">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="7aa3d-184">Ponowne ładowanie danych konfiguracyjnych za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> przedstawiono w przykładzie &num;5 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-184">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="7aa3d-185">Przy użyciu <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>opcje są obliczane raz dla każdego żądania, gdy jest on używany i buforowany przez okres istnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-185">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="7aa3d-186">Różnica między `IOptionsMonitor` i `IOptionsSnapshot` to:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-186">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="7aa3d-187">`IOptionsMonitor` to [Pojedyncza usługa](xref:fundamentals/dependency-injection#singleton) , która pobiera bieżące wartości opcji w dowolnym momencie, która jest szczególnie przydatna w pojedynczych zależnościach.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-187">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="7aa3d-188">`IOptionsSnapshot` jest [usługą objętą zakresem](xref:fundamentals/dependency-injection#scoped) i zawiera migawkę opcji w czasie konstruowania obiektu `IOptionsSnapshot<T>`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-188">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="7aa3d-189">Migawki opcji są przeznaczone do użycia z zależnościami przejściowymi i zakresowymi.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-189">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="7aa3d-190">Poniższy przykład ilustruje sposób tworzenia nowego <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> po wprowadzeniu zmian w pliku *appSettings. JSON* (*Pages/index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="7aa3d-190">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="7aa3d-191">Wiele żądań do zwracanych przez serwer wartości stałych dostarczonych przez plik *appSettings. JSON* do momentu zmiany pliku i ponownego załadowania konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-191">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="7aa3d-192">Na poniższej ilustracji przedstawiono początkowe `option1` i `option2` wartości załadowane z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="7aa3d-192">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="7aa3d-193">Zmień wartości w pliku *appSettings. JSON* na `value1_from_json UPDATED` i `200`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-193">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="7aa3d-194">Zapisz plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="7aa3d-194">Save the *appsettings.json* file.</span></span> <span data-ttu-id="7aa3d-195">Odśwież przeglądarkę, aby zobaczyć, że wartości opcji są aktualizowane:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-195">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="7aa3d-196">Obsługa nazwanych opcji w programie IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="7aa3d-196">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="7aa3d-197">Obsługa nazwanych opcji w <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> jest przedstawiana jako przykład &num;6 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-197">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="7aa3d-198">Obsługa " *nazwanych opcji* " pozwala aplikacji rozróżnić między nazwanymi konfiguracjami opcji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-198">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="7aa3d-199">W przykładowej aplikacji, nazwane opcje są zadeklarowane za pomocą [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), która wywołuje [ConfigureNamedOptions\<TOptions >. Konfiguruj](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) metodę rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-199">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="7aa3d-200">Aplikacja Przykładowa uzyskuje dostęp do nazwanych opcji za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-200">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="7aa3d-201">Po uruchomieniu aplikacji przykładowej są zwracane nazwane opcje:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-201">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="7aa3d-202">wartości `named_options_1` są dostarczane z konfiguracji, które są ładowane z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="7aa3d-202">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="7aa3d-203">wartości `named_options_2` są udostępniane przez:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-203">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="7aa3d-204">Delegat `named_options_2` w `ConfigureServices` dla `Option1`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-204">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="7aa3d-205">Wartość domyślna dla `Option2` dostarczonej przez klasę `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-205">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="7aa3d-206">Skonfiguruj wszystkie opcje przy użyciu metody ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="7aa3d-206">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="7aa3d-207">Skonfiguruj wszystkie wystąpienia opcji za pomocą metody <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-207">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="7aa3d-208">Poniższy kod konfiguruje `Option1` dla wszystkich wystąpień konfiguracji z wspólną wartością.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-208">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="7aa3d-209">Ręcznie Dodaj następujący kod do metody `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-209">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="7aa3d-210">Uruchomienie przykładowej aplikacji po dodaniu kodu daje następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-210">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="7aa3d-211">Wszystkie opcje są nazwanymi wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-211">All options are named instances.</span></span> <span data-ttu-id="7aa3d-212">Istniejące wystąpienia <xref:Microsoft.Extensions.Options.IConfigureOptions%601> są traktowane jako ukierunkowane na wystąpienie `Options.DefaultName`, które jest `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-212">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="7aa3d-213"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implementuje również <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-213"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="7aa3d-214">Domyślna implementacja <xref:Microsoft.Extensions.Options.IOptionsFactory%601> ma logikę do użycia odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-214">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="7aa3d-215">Opcja `null` nazwana służy do określania wszystkich wystąpień nazwanych zamiast określonego wystąpienia nazwanego (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> i <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> używania tej Konwencji).</span><span class="sxs-lookup"><span data-stu-id="7aa3d-215">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="7aa3d-216">Interfejs API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="7aa3d-216">OptionsBuilder API</span></span>

<span data-ttu-id="7aa3d-217"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> służy do konfigurowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-217"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="7aa3d-218">`OptionsBuilder` usprawnia Tworzenie nazwanych opcji, ponieważ jest tylko pojedynczym parametrem do początkowego wywołania `AddOptions<TOptions>(string optionsName)`, a nie pojawiają się we wszystkich kolejnych wywołaniach.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-218">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="7aa3d-219">Sprawdzanie poprawności opcji i przeciążenia `ConfigureOptions`, które akceptują zależności usługi, są dostępne tylko za pośrednictwem `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-219">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="7aa3d-220">Korzystanie z usług DI Services w celu konfigurowania opcji</span><span class="sxs-lookup"><span data-stu-id="7aa3d-220">Use DI services to configure options</span></span>

<span data-ttu-id="7aa3d-221">Można uzyskać dostęp do innych usług z iniekcji zależności podczas konfigurowania opcji na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-221">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="7aa3d-222">Przekaż delegat konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) w usłudze [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="7aa3d-222">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="7aa3d-223">Program [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) oferuje przeciążenia [konfiguracji](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) , które umożliwiają konfigurowanie opcji przy użyciu maksymalnie pięciu usług:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-223">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="7aa3d-224">Utwórz własny typ, który implementuje <xref:Microsoft.Extensions.Options.IConfigureOptions%601> lub <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i Zarejestruj typ jako usługę.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-224">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="7aa3d-225">Zalecamy przekazanie delegata konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), ponieważ tworzenie usługi jest bardziej skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-225">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="7aa3d-226">Tworzenie własnego typu jest równoznaczne z tym, co jest używane przez platformę podczas [konfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="7aa3d-226">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="7aa3d-227">Wywołanie metody [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) powoduje zarejestrowanie przejściowej ogólnej <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, która ma Konstruktor akceptujący określone typy usług ogólnych.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-227">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="7aa3d-228">Sprawdzanie poprawności opcji</span><span class="sxs-lookup"><span data-stu-id="7aa3d-228">Options validation</span></span>

<span data-ttu-id="7aa3d-229">Sprawdzanie poprawności opcji umożliwia sprawdzenie opcji w przypadku skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-229">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="7aa3d-230">Wywołaj `Validate` z metodą walidacji, która zwraca `true`, jeśli opcje są prawidłowe i `false` jeśli są nieprawidłowe:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-230">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="7aa3d-231">Poprzedni przykład ustawia nazwane wystąpienie opcji na `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-231">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="7aa3d-232">Domyślne wystąpienie opcji jest `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-232">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="7aa3d-233">Walidacja jest uruchamiana po utworzeniu wystąpienia opcji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-233">Validation runs when the options instance is created.</span></span> <span data-ttu-id="7aa3d-234">Twoje wystąpienie opcji gwarantuje przekazanie walidacji przy pierwszej próbie dostępu.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-234">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7aa3d-235">Sprawdzanie poprawności opcji nie chroni przed modyfikacjami opcji po początkowym skonfigurowaniu opcji i sprawdzeniu poprawności.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-235">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="7aa3d-236">Metoda `Validate` akceptuje `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-236">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="7aa3d-237">Aby w pełni dostosować walidację, zaimplementuj `IValidateOptions<TOptions>`, co umożliwia:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-237">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="7aa3d-238">Sprawdzanie poprawności wielu typów opcji: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="7aa3d-238">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="7aa3d-239">Walidacja zależna od innego typu opcji: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="7aa3d-239">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="7aa3d-240">`IValidateOptions` sprawdza poprawność:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-240">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="7aa3d-241">Konkretne nazwane wystąpienie opcji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-241">A specific named options instance.</span></span>
* <span data-ttu-id="7aa3d-242">Wszystkie opcje, gdy `name` jest `null`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-242">All options when `name` is `null`.</span></span>

<span data-ttu-id="7aa3d-243">Zwróć `ValidateOptionsResult` z implementacji interfejsu:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-243">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="7aa3d-244">Weryfikacja oparta na adnotacji danych jest dostępna w pakiecie [Microsoft. Extensions. options. DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) , wywołując metodę <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> w `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-244">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="7aa3d-245">`Microsoft.Extensions.Options.DataAnnotations` jest niejawnie przywoływana w aplikacjach ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-245">`Microsoft.Extensions.Options.DataAnnotations` is implicitly referenced in ASP.NET Core apps.</span></span>

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

<span data-ttu-id="7aa3d-246">Eager sprawdzanie poprawności (niepowodzenie szybkie przy uruchamianiu) jest rozważane dla przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-246">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="7aa3d-247">Opcje po konfiguracji</span><span class="sxs-lookup"><span data-stu-id="7aa3d-247">Options post-configuration</span></span>

<span data-ttu-id="7aa3d-248">Ustaw wartość po konfiguracji z <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-248">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="7aa3d-249">Po uruchomieniu zostanie uruchomiona po zakończeniu konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-249">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="7aa3d-250"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> jest dostępny do skonfigurowania nazwanych opcji:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-250"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="7aa3d-251">Użyj <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>, aby skonfigurować wszystkie wystąpienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-251">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="7aa3d-252">Dostęp do opcji podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="7aa3d-252">Accessing options during startup</span></span>

<span data-ttu-id="7aa3d-253"><xref:Microsoft.Extensions.Options.IOptions%601> i <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> mogą być używane w `Startup.Configure`, ponieważ usługi są kompilowane przed wykonaniem metody `Configure`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-253"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, 
    IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="7aa3d-254">Nie używaj <xref:Microsoft.Extensions.Options.IOptions%601> ani <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-254">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7aa3d-255">Niespójny stan opcji może istnieć ze względu na kolejność rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-255">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="7aa3d-256">Wzorzec opcji używa klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-256">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="7aa3d-257">Gdy [Ustawienia konfiguracji](xref:fundamentals/configuration/index) są izolowane według scenariusza w osobnych klasach, aplikacja będzie zgodna z dwoma ważnymi zasadami inżynierii oprogramowania:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-257">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="7aa3d-258">[Zasady podziału interfejsu (ISP) lub &ndash; hermetyzacji](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) (klasy), które są zależne od ustawień konfiguracji, zależą tylko od ustawień konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-258">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="7aa3d-259">[Separacja problemów](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; ustawień różnych części aplikacji nie zależy od siebie ani nie jest połączona.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-259">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="7aa3d-260">Opcje umożliwiają również mechanizm weryfikacji danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-260">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="7aa3d-261">Aby uzyskać więcej informacji, zobacz sekcję [Opcje walidacji](#options-validation) .</span><span class="sxs-lookup"><span data-stu-id="7aa3d-261">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="7aa3d-262">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7aa3d-262">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7aa3d-263">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7aa3d-263">Prerequisites</span></span>

<span data-ttu-id="7aa3d-264">Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) .</span><span class="sxs-lookup"><span data-stu-id="7aa3d-264">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="7aa3d-265">Interfejsy opcji</span><span class="sxs-lookup"><span data-stu-id="7aa3d-265">Options interfaces</span></span>

<span data-ttu-id="7aa3d-266"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> służy do pobierania opcji i zarządzania powiadomieniami o opcjach dla wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-266"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="7aa3d-267"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-267"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="7aa3d-268">Powiadomienia o zmianie</span><span class="sxs-lookup"><span data-stu-id="7aa3d-268">Change notifications</span></span>
* [<span data-ttu-id="7aa3d-269">Nazwane opcje</span><span class="sxs-lookup"><span data-stu-id="7aa3d-269">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="7aa3d-270">Konfiguracja do ponownego ładowania</span><span class="sxs-lookup"><span data-stu-id="7aa3d-270">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="7aa3d-271">Unieważnianie opcji selektywnych (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="7aa3d-271">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="7aa3d-272">Scenariusze [po konfiguracji](#options-post-configuration) umożliwiają ustawianie lub zmienianie opcji po wystąpieniu całej konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-272">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="7aa3d-273"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> jest odpowiedzialny za tworzenie nowych wystąpień opcji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-273"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="7aa3d-274">Ma ona jedną <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> metodę.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-274">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="7aa3d-275">Domyślna implementacja pobiera wszystkie zarejestrowane <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> i uruchamia najpierw wszystkie konfiguracje, po których następuje po zakończeniu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-275">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="7aa3d-276">Odróżnia między <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i wywołuje tylko odpowiedni interfejs.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-276">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="7aa3d-277"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> jest używana przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do buforowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-277"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="7aa3d-278"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> unieważnia wystąpienia opcji na monitorze, aby wartość była ponownie obliczana (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="7aa3d-278">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="7aa3d-279">Wartości można wprowadzać ręcznie przy użyciu <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-279">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="7aa3d-280">Metoda <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> jest używana, gdy wszystkie nazwane wystąpienia powinny być ponownie tworzone na żądanie.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-280">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="7aa3d-281"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> jest przydatne w scenariuszach, w których opcje powinny być ponownie obliczane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-281"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="7aa3d-282">Aby uzyskać więcej informacji, zobacz sekcję [Załaduj dane konfiguracji za pomocą IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) .</span><span class="sxs-lookup"><span data-stu-id="7aa3d-282">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="7aa3d-283"><xref:Microsoft.Extensions.Options.IOptions%601> może służyć do obsługi opcji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-283"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="7aa3d-284">Jednak <xref:Microsoft.Extensions.Options.IOptions%601> nie obsługuje powyższych scenariuszy <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-284">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="7aa3d-285">Można nadal używać <xref:Microsoft.Extensions.Options.IOptions%601> w istniejących strukturach i bibliotekach, które już używają interfejsu <xref:Microsoft.Extensions.Options.IOptions%601> i nie wymagają scenariuszy udostępnianych przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-285">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="7aa3d-286">Konfiguracja opcji ogólnych</span><span class="sxs-lookup"><span data-stu-id="7aa3d-286">General options configuration</span></span>

<span data-ttu-id="7aa3d-287">Konfiguracja opcji ogólnych jest przedstawiana jako przykład &num;1 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-287">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="7aa3d-288">Klasa Options musi być nieabstrakcyjna z publicznym konstruktorem bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-288">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="7aa3d-289">Następująca Klasa `MyOptions`ma dwie właściwości, `Option1` i `Option2`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-289">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="7aa3d-290">Ustawienie wartości domyślnych jest opcjonalne, ale Konstruktor klasy w poniższym przykładzie ustawia wartość domyślną `Option1`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-290">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="7aa3d-291">`Option2` ma ustawioną wartość domyślną, inicjując właściwość bezpośrednio (*modele/opcje. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-291">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="7aa3d-292">Klasa `MyOptions` zostanie dodana do kontenera usługi z <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> i powiązana z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-292">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="7aa3d-293">Poniższy model strony używa [iniekcji zależności konstruktora](xref:mvc/controllers/dependency-injection) z <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, aby uzyskać dostęp do ustawień (*stron/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-293">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="7aa3d-294">Plik *appSettings. JSON* przykładu Określa wartości dla `option1` i `option2`:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-294">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="7aa3d-295">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-295">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="7aa3d-296">W przypadku używania <xref:System.Configuration.ConfigurationBuilder> niestandardowego do załadowania konfiguracji opcji z pliku ustawień upewnij się, że ścieżka bazowa jest ustawiona poprawnie:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-296">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="7aa3d-297">Jawne ustawienie ścieżki podstawowej nie jest wymagane podczas ładowania konfiguracji opcji z pliku ustawień za pośrednictwem <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-297">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="7aa3d-298">Skonfiguruj proste opcje z delegatem</span><span class="sxs-lookup"><span data-stu-id="7aa3d-298">Configure simple options with a delegate</span></span>

<span data-ttu-id="7aa3d-299">Konfigurowanie prostych opcji za pomocą delegata jest zademonstrowane jako przykład &num;2 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-299">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="7aa3d-300">Użyj delegata, aby ustawić wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-300">Use a delegate to set options values.</span></span> <span data-ttu-id="7aa3d-301">Przykładowa aplikacja używa klasy `MyOptionsWithDelegateConfig` (*modele/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-301">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="7aa3d-302">W poniższym kodzie do kontenera usługi jest dodawana druga usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-302">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="7aa3d-303">Używa delegata do konfigurowania powiązania z `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-303">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="7aa3d-304">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-304">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="7aa3d-305">Można dodać wielu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-305">You can add multiple configuration providers.</span></span> <span data-ttu-id="7aa3d-306">Dostawcy konfiguracji są dostępni z pakietów NuGet i są stosowane w kolejności, w jakiej zostały zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-306">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="7aa3d-307">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-307">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="7aa3d-308">Każde wywołanie <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> dodaje usługę <xref:Microsoft.Extensions.Options.IConfigureOptions%601> do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-308">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="7aa3d-309">W poprzednim przykładzie wartości `Option1` i `Option2` są określone w pliku *appSettings. JSON*, ale wartości `Option1` i `Option2` są przesłonięte przez skonfigurowany delegat.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-309">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="7aa3d-310">Gdy jest włączona więcej niż jedna usługa konfiguracji, ostatnie Źródło konfiguracji określiło *serwer WINS* i ustawi wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-310">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="7aa3d-311">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-311">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="7aa3d-312">Konfiguracja podopcji</span><span class="sxs-lookup"><span data-stu-id="7aa3d-312">Suboptions configuration</span></span>

<span data-ttu-id="7aa3d-313">Konfiguracja podopcji jest przedstawiana jako przykład &num;3 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-313">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="7aa3d-314">Aplikacje powinny tworzyć klasy opcji, które odnoszą się do określonych grup scenariuszy (klas) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-314">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="7aa3d-315">Części aplikacji, które wymagają wartości konfiguracyjnych, powinny mieć dostęp tylko do wartości konfiguracyjnych, z których korzystają.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-315">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="7aa3d-316">Po powiązaniu opcji powiązań z konfiguracją Każda właściwość w typie opcji jest powiązana z kluczem konfiguracji formularza `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-316">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="7aa3d-317">Na przykład właściwość `MyOptions.Option1` jest powiązana z `Option1`klucza, który jest odczytywany z właściwości `option1` w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-317">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="7aa3d-318">W poniższym kodzie do kontenera usługi zostanie dodana trzecia usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-318">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="7aa3d-319">Wiąże `MySubOptions` z sekcją `subsection` pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="7aa3d-319">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="7aa3d-320">Metoda `GetSection` wymaga przestrzeni nazw <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-320">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="7aa3d-321">Plik *appSettings. JSON* przykładu definiuje `subsection` składową z kluczami dla `suboption1` i `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-321">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="7aa3d-322">Klasa `MySubOptions` definiuje właściwości, `SubOption1` i `SubOption2`do przechowywania wartości opcji (*modele/MySubOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-322">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="7aa3d-323">Metoda `OnGet` modelu strony zwraca ciąg z wartościami opcji (*Pages/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-323">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="7aa3d-324">Po uruchomieniu aplikacji Metoda `OnGet` zwraca ciąg pokazujący wartości klasy podopcji:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-324">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="7aa3d-325">Iniekcja opcji</span><span class="sxs-lookup"><span data-stu-id="7aa3d-325">Options injection</span></span>

<span data-ttu-id="7aa3d-326">Iniekcja opcji jest prezentowana jako przykład &num;4 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-326">Options injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="7aa3d-327">Wsuń <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-327">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="7aa3d-328">Strona Razor lub widok MVC z dyrektywą [@inject](xref:mvc/views/razor#inject) Razor.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-328">A Razor page or MVC view with the [@inject](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="7aa3d-329">Model strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-329">A page or view model.</span></span>

<span data-ttu-id="7aa3d-330">Poniższy przykład z przykładowej aplikacji wprowadza <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do modelu strony (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-330">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="7aa3d-331">Przykładowa aplikacja pokazuje, jak wstrzyknąć `IOptionsMonitor<MyOptions>` za pomocą dyrektywy `@inject`:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-331">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="7aa3d-332">Po uruchomieniu aplikacji na renderowanej stronie są wyświetlane wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-332">When the app is run, the options values are shown in the rendered page:</span></span>

![Wartości opcji opcja1: value1_from_json i Opcja2:-1 są ładowane z modelu i przez iniekcję do widoku.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="7aa3d-334">Załaduj ponownie dane konfiguracji za pomocą IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="7aa3d-334">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="7aa3d-335">Ponowne ładowanie danych konfiguracyjnych za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> przedstawiono w przykładzie &num;5 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-335">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="7aa3d-336">Przy użyciu <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>opcje są obliczane raz dla każdego żądania, gdy jest on używany i buforowany przez okres istnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-336">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="7aa3d-337">Różnica między `IOptionsMonitor` i `IOptionsSnapshot` to:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-337">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="7aa3d-338">`IOptionsMonitor` to [Pojedyncza usługa](xref:fundamentals/dependency-injection#singleton) , która pobiera bieżące wartości opcji w dowolnym momencie, która jest szczególnie przydatna w pojedynczych zależnościach.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-338">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="7aa3d-339">`IOptionsSnapshot` jest [usługą objętą zakresem](xref:fundamentals/dependency-injection#scoped) i zawiera migawkę opcji w czasie konstruowania obiektu `IOptionsSnapshot<T>`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-339">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="7aa3d-340">Migawki opcji są przeznaczone do użycia z zależnościami przejściowymi i zakresowymi.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-340">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="7aa3d-341">Poniższy przykład ilustruje sposób tworzenia nowego <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> po wprowadzeniu zmian w pliku *appSettings. JSON* (*Pages/index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="7aa3d-341">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="7aa3d-342">Wiele żądań do zwracanych przez serwer wartości stałych dostarczonych przez plik *appSettings. JSON* do momentu zmiany pliku i ponownego załadowania konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-342">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="7aa3d-343">Na poniższej ilustracji przedstawiono początkowe `option1` i `option2` wartości załadowane z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="7aa3d-343">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="7aa3d-344">Zmień wartości w pliku *appSettings. JSON* na `value1_from_json UPDATED` i `200`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-344">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="7aa3d-345">Zapisz plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="7aa3d-345">Save the *appsettings.json* file.</span></span> <span data-ttu-id="7aa3d-346">Odśwież przeglądarkę, aby zobaczyć, że wartości opcji są aktualizowane:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-346">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="7aa3d-347">Obsługa nazwanych opcji w programie IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="7aa3d-347">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="7aa3d-348">Obsługa nazwanych opcji w <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> jest przedstawiana jako przykład &num;6 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-348">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="7aa3d-349">Obsługa " *nazwanych opcji* " pozwala aplikacji rozróżnić między nazwanymi konfiguracjami opcji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-349">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="7aa3d-350">W przykładowej aplikacji, nazwane opcje są zadeklarowane za pomocą [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), która wywołuje [ConfigureNamedOptions\<TOptions >. Konfiguruj](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) metodę rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-350">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="7aa3d-351">Aplikacja Przykładowa uzyskuje dostęp do nazwanych opcji za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-351">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="7aa3d-352">Po uruchomieniu aplikacji przykładowej są zwracane nazwane opcje:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-352">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="7aa3d-353">wartości `named_options_1` są dostarczane z konfiguracji, które są ładowane z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="7aa3d-353">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="7aa3d-354">wartości `named_options_2` są udostępniane przez:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-354">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="7aa3d-355">Delegat `named_options_2` w `ConfigureServices` dla `Option1`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-355">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="7aa3d-356">Wartość domyślna dla `Option2` dostarczonej przez klasę `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-356">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="7aa3d-357">Skonfiguruj wszystkie opcje przy użyciu metody ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="7aa3d-357">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="7aa3d-358">Skonfiguruj wszystkie wystąpienia opcji za pomocą metody <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-358">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="7aa3d-359">Poniższy kod konfiguruje `Option1` dla wszystkich wystąpień konfiguracji z wspólną wartością.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-359">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="7aa3d-360">Ręcznie Dodaj następujący kod do metody `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-360">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="7aa3d-361">Uruchomienie przykładowej aplikacji po dodaniu kodu daje następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-361">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="7aa3d-362">Wszystkie opcje są nazwanymi wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-362">All options are named instances.</span></span> <span data-ttu-id="7aa3d-363">Istniejące wystąpienia <xref:Microsoft.Extensions.Options.IConfigureOptions%601> są traktowane jako ukierunkowane na wystąpienie `Options.DefaultName`, które jest `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-363">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="7aa3d-364"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implementuje również <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-364"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="7aa3d-365">Domyślna implementacja <xref:Microsoft.Extensions.Options.IOptionsFactory%601> ma logikę do użycia odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-365">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="7aa3d-366">Opcja `null` nazwana służy do określania wszystkich wystąpień nazwanych zamiast określonego wystąpienia nazwanego (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> i <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> używania tej Konwencji).</span><span class="sxs-lookup"><span data-stu-id="7aa3d-366">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="7aa3d-367">Interfejs API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="7aa3d-367">OptionsBuilder API</span></span>

<span data-ttu-id="7aa3d-368"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> służy do konfigurowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-368"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="7aa3d-369">`OptionsBuilder` usprawnia Tworzenie nazwanych opcji, ponieważ jest tylko pojedynczym parametrem do początkowego wywołania `AddOptions<TOptions>(string optionsName)`, a nie pojawiają się we wszystkich kolejnych wywołaniach.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-369">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="7aa3d-370">Sprawdzanie poprawności opcji i przeciążenia `ConfigureOptions`, które akceptują zależności usługi, są dostępne tylko za pośrednictwem `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-370">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="7aa3d-371">Korzystanie z usług DI Services w celu konfigurowania opcji</span><span class="sxs-lookup"><span data-stu-id="7aa3d-371">Use DI services to configure options</span></span>

<span data-ttu-id="7aa3d-372">Można uzyskać dostęp do innych usług z iniekcji zależności podczas konfigurowania opcji na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-372">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="7aa3d-373">Przekaż delegat konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) w usłudze [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="7aa3d-373">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="7aa3d-374">Program [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) oferuje przeciążenia [konfiguracji](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) , które umożliwiają konfigurowanie opcji przy użyciu maksymalnie pięciu usług:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-374">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="7aa3d-375">Utwórz własny typ, który implementuje <xref:Microsoft.Extensions.Options.IConfigureOptions%601> lub <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i Zarejestruj typ jako usługę.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-375">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="7aa3d-376">Zalecamy przekazanie delegata konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), ponieważ tworzenie usługi jest bardziej skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-376">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="7aa3d-377">Tworzenie własnego typu jest równoznaczne z tym, co jest używane przez platformę podczas [konfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="7aa3d-377">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="7aa3d-378">Wywołanie metody [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) powoduje zarejestrowanie przejściowej ogólnej <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, która ma Konstruktor akceptujący określone typy usług ogólnych.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-378">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="7aa3d-379">Sprawdzanie poprawności opcji</span><span class="sxs-lookup"><span data-stu-id="7aa3d-379">Options validation</span></span>

<span data-ttu-id="7aa3d-380">Sprawdzanie poprawności opcji umożliwia sprawdzenie opcji w przypadku skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-380">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="7aa3d-381">Wywołaj `Validate` z metodą walidacji, która zwraca `true`, jeśli opcje są prawidłowe i `false` jeśli są nieprawidłowe:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-381">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="7aa3d-382">Poprzedni przykład ustawia nazwane wystąpienie opcji na `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-382">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="7aa3d-383">Domyślne wystąpienie opcji jest `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-383">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="7aa3d-384">Walidacja jest uruchamiana po utworzeniu wystąpienia opcji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-384">Validation runs when the options instance is created.</span></span> <span data-ttu-id="7aa3d-385">Twoje wystąpienie opcji gwarantuje przekazanie walidacji przy pierwszej próbie dostępu.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-385">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7aa3d-386">Sprawdzanie poprawności opcji nie chroni przed modyfikacjami opcji po początkowym skonfigurowaniu opcji i sprawdzeniu poprawności.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-386">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="7aa3d-387">Metoda `Validate` akceptuje `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-387">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="7aa3d-388">Aby w pełni dostosować walidację, zaimplementuj `IValidateOptions<TOptions>`, co umożliwia:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-388">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="7aa3d-389">Sprawdzanie poprawności wielu typów opcji: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="7aa3d-389">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="7aa3d-390">Walidacja zależna od innego typu opcji: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="7aa3d-390">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="7aa3d-391">`IValidateOptions` sprawdza poprawność:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-391">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="7aa3d-392">Konkretne nazwane wystąpienie opcji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-392">A specific named options instance.</span></span>
* <span data-ttu-id="7aa3d-393">Wszystkie opcje, gdy `name` jest `null`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-393">All options when `name` is `null`.</span></span>

<span data-ttu-id="7aa3d-394">Zwróć `ValidateOptionsResult` z implementacji interfejsu:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-394">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="7aa3d-395">Weryfikacja oparta na adnotacji danych jest dostępna w pakiecie [Microsoft. Extensions. options. DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) , wywołując metodę <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> w `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-395">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="7aa3d-396">`Microsoft.Extensions.Options.DataAnnotations` jest zawarty w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="7aa3d-396">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

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

<span data-ttu-id="7aa3d-397">Eager sprawdzanie poprawności (niepowodzenie szybkie przy uruchamianiu) jest rozważane dla przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-397">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="7aa3d-398">Opcje po konfiguracji</span><span class="sxs-lookup"><span data-stu-id="7aa3d-398">Options post-configuration</span></span>

<span data-ttu-id="7aa3d-399">Ustaw wartość po konfiguracji z <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-399">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="7aa3d-400">Po uruchomieniu zostanie uruchomiona po zakończeniu konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-400">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="7aa3d-401"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> jest dostępny do skonfigurowania nazwanych opcji:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-401"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="7aa3d-402">Użyj <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>, aby skonfigurować wszystkie wystąpienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-402">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="7aa3d-403">Dostęp do opcji podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="7aa3d-403">Accessing options during startup</span></span>

<span data-ttu-id="7aa3d-404"><xref:Microsoft.Extensions.Options.IOptions%601> i <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> mogą być używane w `Startup.Configure`, ponieważ usługi są kompilowane przed wykonaniem metody `Configure`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-404"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="7aa3d-405">Nie używaj <xref:Microsoft.Extensions.Options.IOptions%601> ani <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-405">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7aa3d-406">Niespójny stan opcji może istnieć ze względu na kolejność rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-406">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="7aa3d-407">Wzorzec opcji używa klas do reprezentowania grup powiązanych ustawień.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-407">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="7aa3d-408">Gdy [Ustawienia konfiguracji](xref:fundamentals/configuration/index) są izolowane według scenariusza w osobnych klasach, aplikacja będzie zgodna z dwoma ważnymi zasadami inżynierii oprogramowania:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-408">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="7aa3d-409">[Zasady podziału interfejsu (ISP) lub &ndash; hermetyzacji](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) (klasy), które są zależne od ustawień konfiguracji, zależą tylko od ustawień konfiguracji, których używają.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-409">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="7aa3d-410">[Separacja problemów](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; ustawień różnych części aplikacji nie zależy od siebie ani nie jest połączona.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-410">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="7aa3d-411">Opcje umożliwiają również mechanizm weryfikacji danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-411">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="7aa3d-412">Aby uzyskać więcej informacji, zobacz sekcję [Opcje walidacji](#options-validation) .</span><span class="sxs-lookup"><span data-stu-id="7aa3d-412">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="7aa3d-413">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7aa3d-413">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7aa3d-414">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7aa3d-414">Prerequisites</span></span>

<span data-ttu-id="7aa3d-415">Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. options. ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) .</span><span class="sxs-lookup"><span data-stu-id="7aa3d-415">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="7aa3d-416">Interfejsy opcji</span><span class="sxs-lookup"><span data-stu-id="7aa3d-416">Options interfaces</span></span>

<span data-ttu-id="7aa3d-417"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> służy do pobierania opcji i zarządzania powiadomieniami o opcjach dla wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-417"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="7aa3d-418"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> obsługuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-418"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="7aa3d-419">Powiadomienia o zmianie</span><span class="sxs-lookup"><span data-stu-id="7aa3d-419">Change notifications</span></span>
* [<span data-ttu-id="7aa3d-420">Nazwane opcje</span><span class="sxs-lookup"><span data-stu-id="7aa3d-420">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="7aa3d-421">Konfiguracja do ponownego ładowania</span><span class="sxs-lookup"><span data-stu-id="7aa3d-421">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="7aa3d-422">Unieważnianie opcji selektywnych (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="7aa3d-422">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="7aa3d-423">Scenariusze [po konfiguracji](#options-post-configuration) umożliwiają ustawianie lub zmienianie opcji po wystąpieniu całej konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-423">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="7aa3d-424"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> jest odpowiedzialny za tworzenie nowych wystąpień opcji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-424"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="7aa3d-425">Ma ona jedną <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> metodę.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-425">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="7aa3d-426">Domyślna implementacja pobiera wszystkie zarejestrowane <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> i uruchamia najpierw wszystkie konfiguracje, po których następuje po zakończeniu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-426">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="7aa3d-427">Odróżnia między <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i <xref:Microsoft.Extensions.Options.IConfigureOptions%601> i wywołuje tylko odpowiedni interfejs.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-427">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="7aa3d-428"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> jest używana przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> do buforowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-428"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="7aa3d-429"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> unieważnia wystąpienia opcji na monitorze, aby wartość była ponownie obliczana (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="7aa3d-429">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="7aa3d-430">Wartości można wprowadzać ręcznie przy użyciu <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-430">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="7aa3d-431">Metoda <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> jest używana, gdy wszystkie nazwane wystąpienia powinny być ponownie tworzone na żądanie.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-431">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="7aa3d-432"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> jest przydatne w scenariuszach, w których opcje powinny być ponownie obliczane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-432"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="7aa3d-433">Aby uzyskać więcej informacji, zobacz sekcję [Załaduj dane konfiguracji za pomocą IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) .</span><span class="sxs-lookup"><span data-stu-id="7aa3d-433">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="7aa3d-434"><xref:Microsoft.Extensions.Options.IOptions%601> może służyć do obsługi opcji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-434"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="7aa3d-435">Jednak <xref:Microsoft.Extensions.Options.IOptions%601> nie obsługuje powyższych scenariuszy <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-435">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="7aa3d-436">Można nadal używać <xref:Microsoft.Extensions.Options.IOptions%601> w istniejących strukturach i bibliotekach, które już używają interfejsu <xref:Microsoft.Extensions.Options.IOptions%601> i nie wymagają scenariuszy udostępnianych przez <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-436">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="7aa3d-437">Konfiguracja opcji ogólnych</span><span class="sxs-lookup"><span data-stu-id="7aa3d-437">General options configuration</span></span>

<span data-ttu-id="7aa3d-438">Konfiguracja opcji ogólnych jest przedstawiana jako przykład &num;1 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-438">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="7aa3d-439">Klasa Options musi być nieabstrakcyjna z publicznym konstruktorem bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-439">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="7aa3d-440">Następująca Klasa `MyOptions`ma dwie właściwości, `Option1` i `Option2`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-440">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="7aa3d-441">Ustawienie wartości domyślnych jest opcjonalne, ale Konstruktor klasy w poniższym przykładzie ustawia wartość domyślną `Option1`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-441">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="7aa3d-442">`Option2` ma ustawioną wartość domyślną, inicjując właściwość bezpośrednio (*modele/opcje. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-442">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="7aa3d-443">Klasa `MyOptions` zostanie dodana do kontenera usługi z <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> i powiązana z konfiguracją:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-443">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="7aa3d-444">Poniższy model strony używa [iniekcji zależności konstruktora](xref:mvc/controllers/dependency-injection) z <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>, aby uzyskać dostęp do ustawień (*stron/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-444">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="7aa3d-445">Plik *appSettings. JSON* przykładu Określa wartości dla `option1` i `option2`:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-445">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="7aa3d-446">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-446">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="7aa3d-447">W przypadku używania <xref:System.Configuration.ConfigurationBuilder> niestandardowego do załadowania konfiguracji opcji z pliku ustawień upewnij się, że ścieżka bazowa jest ustawiona poprawnie:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-447">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="7aa3d-448">Jawne ustawienie ścieżki podstawowej nie jest wymagane podczas ładowania konfiguracji opcji z pliku ustawień za pośrednictwem <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-448">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="7aa3d-449">Skonfiguruj proste opcje z delegatem</span><span class="sxs-lookup"><span data-stu-id="7aa3d-449">Configure simple options with a delegate</span></span>

<span data-ttu-id="7aa3d-450">Konfigurowanie prostych opcji za pomocą delegata jest zademonstrowane jako przykład &num;2 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-450">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="7aa3d-451">Użyj delegata, aby ustawić wartości opcji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-451">Use a delegate to set options values.</span></span> <span data-ttu-id="7aa3d-452">Przykładowa aplikacja używa klasy `MyOptionsWithDelegateConfig` (*modele/MyOptionsWithDelegateConfig. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-452">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="7aa3d-453">W poniższym kodzie do kontenera usługi jest dodawana druga usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-453">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="7aa3d-454">Używa delegata do konfigurowania powiązania z `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-454">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="7aa3d-455">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-455">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="7aa3d-456">Można dodać wielu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-456">You can add multiple configuration providers.</span></span> <span data-ttu-id="7aa3d-457">Dostawcy konfiguracji są dostępni z pakietów NuGet i są stosowane w kolejności, w jakiej zostały zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-457">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="7aa3d-458">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-458">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="7aa3d-459">Każde wywołanie <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> dodaje usługę <xref:Microsoft.Extensions.Options.IConfigureOptions%601> do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-459">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="7aa3d-460">W poprzednim przykładzie wartości `Option1` i `Option2` są określone w pliku *appSettings. JSON*, ale wartości `Option1` i `Option2` są przesłonięte przez skonfigurowany delegat.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-460">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="7aa3d-461">Gdy jest włączona więcej niż jedna usługa konfiguracji, ostatnie Źródło konfiguracji określiło *serwer WINS* i ustawi wartość konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-461">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="7aa3d-462">Po uruchomieniu aplikacji Metoda `OnGet` modelu strony zwraca ciąg pokazujący wartości klasy opcji:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-462">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="7aa3d-463">Konfiguracja podopcji</span><span class="sxs-lookup"><span data-stu-id="7aa3d-463">Suboptions configuration</span></span>

<span data-ttu-id="7aa3d-464">Konfiguracja podopcji jest przedstawiana jako przykład &num;3 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-464">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="7aa3d-465">Aplikacje powinny tworzyć klasy opcji, które odnoszą się do określonych grup scenariuszy (klas) w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-465">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="7aa3d-466">Części aplikacji, które wymagają wartości konfiguracyjnych, powinny mieć dostęp tylko do wartości konfiguracyjnych, z których korzystają.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-466">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="7aa3d-467">Po powiązaniu opcji powiązań z konfiguracją Każda właściwość w typie opcji jest powiązana z kluczem konfiguracji formularza `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-467">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="7aa3d-468">Na przykład właściwość `MyOptions.Option1` jest powiązana z `Option1`klucza, który jest odczytywany z właściwości `option1` w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-468">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="7aa3d-469">W poniższym kodzie do kontenera usługi zostanie dodana trzecia usługa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-469">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="7aa3d-470">Wiąże `MySubOptions` z sekcją `subsection` pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="7aa3d-470">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="7aa3d-471">Metoda `GetSection` wymaga przestrzeni nazw <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-471">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="7aa3d-472">Plik *appSettings. JSON* przykładu definiuje `subsection` składową z kluczami dla `suboption1` i `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-472">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="7aa3d-473">Klasa `MySubOptions` definiuje właściwości, `SubOption1` i `SubOption2`do przechowywania wartości opcji (*modele/MySubOptions. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-473">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="7aa3d-474">Metoda `OnGet` modelu strony zwraca ciąg z wartościami opcji (*Pages/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-474">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="7aa3d-475">Po uruchomieniu aplikacji Metoda `OnGet` zwraca ciąg pokazujący wartości klasy podopcji:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-475">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="7aa3d-476">Opcje udostępniane przez model widoku lub bezpośrednie iniekcja widoku</span><span class="sxs-lookup"><span data-stu-id="7aa3d-476">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="7aa3d-477">Opcje udostępniane przez model widoku lub bezpośrednie wstrzyknięcie widoku są prezentowane jako przykład &num;4 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-477">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="7aa3d-478">Opcje można dostarczyć w modelu widoku lub poprzez wstrzyknięcie <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> bezpośrednio do widoku (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-478">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="7aa3d-479">Przykładowa aplikacja pokazuje, jak wstrzyknąć `IOptionsMonitor<MyOptions>` za pomocą dyrektywy `@inject`:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-479">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="7aa3d-480">Po uruchomieniu aplikacji na renderowanej stronie są wyświetlane wartości opcji:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-480">When the app is run, the options values are shown in the rendered page:</span></span>

![Wartości opcji opcja1: value1_from_json i Opcja2:-1 są ładowane z modelu i przez iniekcję do widoku.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="7aa3d-482">Załaduj ponownie dane konfiguracji za pomocą IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="7aa3d-482">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="7aa3d-483">Ponowne ładowanie danych konfiguracyjnych za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> przedstawiono w przykładzie &num;5 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-483">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="7aa3d-484"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> obsługuje opcje ponownego ładowania z minimalnym obciążeniem przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-484"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="7aa3d-485">Opcje są obliczane raz dla żądania w przypadku uzyskiwania dostępu do pamięci podręcznej i buforowania jej przez okres istnienia żądania.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-485">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="7aa3d-486">Poniższy przykład ilustruje sposób tworzenia nowego <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> po wprowadzeniu zmian w pliku *appSettings. JSON* (*Pages/index. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="7aa3d-486">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="7aa3d-487">Wiele żądań do zwracanych przez serwer wartości stałych dostarczonych przez plik *appSettings. JSON* do momentu zmiany pliku i ponownego załadowania konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-487">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="7aa3d-488">Na poniższej ilustracji przedstawiono początkowe `option1` i `option2` wartości załadowane z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="7aa3d-488">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="7aa3d-489">Zmień wartości w pliku *appSettings. JSON* na `value1_from_json UPDATED` i `200`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-489">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="7aa3d-490">Zapisz plik *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="7aa3d-490">Save the *appsettings.json* file.</span></span> <span data-ttu-id="7aa3d-491">Odśwież przeglądarkę, aby zobaczyć, że wartości opcji są aktualizowane:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-491">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="7aa3d-492">Obsługa nazwanych opcji w programie IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="7aa3d-492">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="7aa3d-493">Obsługa nazwanych opcji w <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> jest przedstawiana jako przykład &num;6 w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-493">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="7aa3d-494">Obsługa " *nazwanych opcji* " pozwala aplikacji rozróżnić między nazwanymi konfiguracjami opcji.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-494">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="7aa3d-495">W przykładowej aplikacji, nazwane opcje są zadeklarowane za pomocą [OptionsServiceCollectionExtensions. configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), która wywołuje [ConfigureNamedOptions\<TOptions >. Konfiguruj](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) metodę rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-495">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="7aa3d-496">Aplikacja Przykładowa uzyskuje dostęp do nazwanych opcji za pomocą <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*strony/index. cshtml. cs*):</span><span class="sxs-lookup"><span data-stu-id="7aa3d-496">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="7aa3d-497">Po uruchomieniu aplikacji przykładowej są zwracane nazwane opcje:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-497">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="7aa3d-498">wartości `named_options_1` są dostarczane z konfiguracji, które są ładowane z pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="7aa3d-498">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="7aa3d-499">wartości `named_options_2` są udostępniane przez:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-499">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="7aa3d-500">Delegat `named_options_2` w `ConfigureServices` dla `Option1`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-500">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="7aa3d-501">Wartość domyślna dla `Option2` dostarczonej przez klasę `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-501">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="7aa3d-502">Skonfiguruj wszystkie opcje przy użyciu metody ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="7aa3d-502">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="7aa3d-503">Skonfiguruj wszystkie wystąpienia opcji za pomocą metody <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-503">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="7aa3d-504">Poniższy kod konfiguruje `Option1` dla wszystkich wystąpień konfiguracji z wspólną wartością.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-504">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="7aa3d-505">Ręcznie Dodaj następujący kod do metody `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-505">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="7aa3d-506">Uruchomienie przykładowej aplikacji po dodaniu kodu daje następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-506">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="7aa3d-507">Wszystkie opcje są nazwanymi wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-507">All options are named instances.</span></span> <span data-ttu-id="7aa3d-508">Istniejące wystąpienia <xref:Microsoft.Extensions.Options.IConfigureOptions%601> są traktowane jako ukierunkowane na wystąpienie `Options.DefaultName`, które jest `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-508">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="7aa3d-509"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> implementuje również <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-509"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="7aa3d-510">Domyślna implementacja <xref:Microsoft.Extensions.Options.IOptionsFactory%601> ma logikę do użycia odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-510">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="7aa3d-511">Opcja `null` nazwana służy do określania wszystkich wystąpień nazwanych zamiast określonego wystąpienia nazwanego (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> i <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> używania tej Konwencji).</span><span class="sxs-lookup"><span data-stu-id="7aa3d-511">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="7aa3d-512">Interfejs API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="7aa3d-512">OptionsBuilder API</span></span>

<span data-ttu-id="7aa3d-513"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> służy do konfigurowania wystąpień `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-513"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="7aa3d-514">`OptionsBuilder` usprawnia Tworzenie nazwanych opcji, ponieważ jest tylko pojedynczym parametrem do początkowego wywołania `AddOptions<TOptions>(string optionsName)`, a nie pojawiają się we wszystkich kolejnych wywołaniach.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-514">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="7aa3d-515">Sprawdzanie poprawności opcji i przeciążenia `ConfigureOptions`, które akceptują zależności usługi, są dostępne tylko za pośrednictwem `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-515">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="7aa3d-516">Korzystanie z usług DI Services w celu konfigurowania opcji</span><span class="sxs-lookup"><span data-stu-id="7aa3d-516">Use DI services to configure options</span></span>

<span data-ttu-id="7aa3d-517">Można uzyskać dostęp do innych usług z iniekcji zależności podczas konfigurowania opcji na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-517">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="7aa3d-518">Przekaż delegat konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) w usłudze [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="7aa3d-518">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="7aa3d-519">Program [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) oferuje przeciążenia [konfiguracji](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) , które umożliwiają konfigurowanie opcji przy użyciu maksymalnie pięciu usług:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-519">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="7aa3d-520">Utwórz własny typ, który implementuje <xref:Microsoft.Extensions.Options.IConfigureOptions%601> lub <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> i Zarejestruj typ jako usługę.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-520">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="7aa3d-521">Zalecamy przekazanie delegata konfiguracji w celu [skonfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), ponieważ tworzenie usługi jest bardziej skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-521">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="7aa3d-522">Tworzenie własnego typu jest równoznaczne z tym, co jest używane przez platformę podczas [konfigurowania](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="7aa3d-522">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="7aa3d-523">Wywołanie metody [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) powoduje zarejestrowanie przejściowej ogólnej <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, która ma Konstruktor akceptujący określone typy usług ogólnych.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-523">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-post-configuration"></a><span data-ttu-id="7aa3d-524">Opcje po konfiguracji</span><span class="sxs-lookup"><span data-stu-id="7aa3d-524">Options post-configuration</span></span>

<span data-ttu-id="7aa3d-525">Ustaw wartość po konfiguracji z <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-525">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="7aa3d-526">Po uruchomieniu zostanie uruchomiona po zakończeniu konfiguracji <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-526">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="7aa3d-527"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> jest dostępny do skonfigurowania nazwanych opcji:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-527"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="7aa3d-528">Użyj <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>, aby skonfigurować wszystkie wystąpienia konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="7aa3d-528">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="7aa3d-529">Dostęp do opcji podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="7aa3d-529">Accessing options during startup</span></span>

<span data-ttu-id="7aa3d-530"><xref:Microsoft.Extensions.Options.IOptions%601> i <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> mogą być używane w `Startup.Configure`, ponieważ usługi są kompilowane przed wykonaniem metody `Configure`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-530"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="7aa3d-531">Nie używaj <xref:Microsoft.Extensions.Options.IOptions%601> ani <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-531">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7aa3d-532">Niespójny stan opcji może istnieć ze względu na kolejność rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="7aa3d-532">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="7aa3d-533">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7aa3d-533">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
